## Array from wholes join optimization

```diff
  macro BufferJoin(implicit context: Context)(buffer: Buffer,
                      sep: String): String {
    dcheck(IsValidPositiveSmi(buffer.totalStringLength));
    if (buffer.totalStringLength == 0) return kEmptyString;

    // Fast path when there's only one chunk and one buffer element.
    if (buffer.index == 2 && buffer.head == buffer.chunk) {
      const chunk: FixedArray = buffer.head;
      typeswitch (chunk.objects[1]) {
        case (str: String): {
          return str;
        }
        case (nofSeparators: Smi): {
          dcheck(nofSeparators > 0);
          //     \/ \/ \/ \/
          return StringRepeat(context, sep, nofSeparators); // <------
          //     /\ /\ /\ /\
        }
        case (Object): {
          unreachable;
        }
      }
    }

    // ...
  }
```

[https://github.com/v8/v8/blob/b5db50f99bc02b1713a28c1718c4bd666264c9e7/src/builtins/array-join.tq#L342](https://github.com/v8/v8/blob/b5db50f99bc02b1713a28c1718c4bd666264c9e7/src/builtins/array-join.tq#L342)

```js
LEN = 1_000_000;

function FooPlus() {
  this.str = '';
  for (let i = 0; i < LEN; i++) {
    this.str += 'x';
  }
}
fooPlus = new FooPlus();

function FooJoinSomething() {
  this.str = new Array(LEN).fill('x').join('');
}
fooJoinSomething = new FooJoinSomething();

function FooJoinWholes() {
  this.str = new Array(LEN + 1).join('x');
}
fooJoinWholes = new FooJoinWholes();
```

![](not_balanced_ConsString_vs_SeqString_vs_balanced_ConsString_construct.png)

```
1. plus (+=)  —  ConsString. Unbalanced binary tree  —  20_000.0 kB
─────────────────────────────────────────────────────────────────────

Each += creates a ConsString node (~20 bytes).
1_000_000 nodes × 20 bytes ≈ 20_000 kB

                ConsString (len=1000000)
                ╱            ╲
         ConsString(999999)  "x"
         ╱            ╲
   ConsString(999998) "x"
   ╱                ╲
 ConsString(999997) "x"     Every node stores: first_, second_, length_, hash_
  ...                       All right children point to separate "x" SeqStrings
 ConsString(13)
 ╱              ╲
"xxxxxxxxxxxx"  "x"            ← first ConsString node (len=13 ≥ kMinLength)
(SeqString=12)  (SeqString=1)    below 13: += copies into flat SeqString


2. join something  —  SeqString. Flat string  —  1_000.0 kB
─────────────────────────────────────────────────────────────

new Array(LEN).fill('x').join('') allocates one flat SeqString.
1_000_000 bytes = 1_000 kB

┌──────────────────────────────────────────────────┐
│ x x x x x x x x x x x x x x ... x x x x x x x    │  SeqOneByteString
│                 1_000_000 bytes                  │  (one contiguous allocation)
└──────────────────────────────────────────────────┘


3. join wholes  —  ConsString. Balanced binary tree with references to a flat string  —  0.5 kB
────────────────────────────────────────────────────────────────────────────────────────────────

new Array(LEN + 1).join('x') uses StringRepeat → balanced ConsString tree.
Splits by halving recursively. Both halves reference the same child → 0.5 kB total.
Leaves are flat SeqStrings, both halves reference the same leaf.

                      ConsString (len=1000000)
                      ╱                      ╲
             ConsString(500000)           ConsString(500000)
               ╱            ╲            ╱            ╲
    ConsString(250000)       ...       ...    ConsString(250000)
            /                  \                    \
          ...                  ...                  ...
         ╱   ╲                ╱   ╲                ╱   ╲
 "xxxxxxx"  "xxxxxxxx" "xxxxxxx" "xxxxxxxx" "xxxxxxx" "xxxxxxxx"
 (SeqStr 7) (SeqStr 8)   ↑same     ↑same      ↑same     ↑same
```

![](not_balanced_ConsString_vs_SeqString_vs_balanced_ConsString_after_flat.png)

