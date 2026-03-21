[`src/objects/string.h`](https://github.com/fosemberg/v8/commit/a483d91a7adcd43a72d75d1eb924eaba361b5131#diff-09027ad9b0e8ef86b64321b2d54987fbd207dac4092998643df0768b7dc51afcR1053)
```diff
    // Minimum length for a cons string
-   static const uint32_t kMinLength = 13;
+   static const uint32_t kMinLength = 23;

    // ...

    // Minimum length for a sliced string.
-   static const uint32_t kMinLength = 13;
+   static const uint32_t kMinLength = 23;
```

`./out/kMinLength_23/d8 test_js/ConsString.js`

```
[Phase 1: StringAdd] left_len: 0
[Phase 1: StringAdd] right_len: 1
[Phase 1: StringAdd] left_len: 1
[Phase 1: StringAdd] right_len: 1
[Phase 1: StringAdd] Full string concatenate
[Phase 1: StringAdd] left_len: 2
[Phase 1: StringAdd] right_len: 1
[Phase 1: StringAdd] Full string concatenate
[Phase 1: StringAdd] left_len: 3
[Phase 1: StringAdd] right_len: 1
[Phase 1: StringAdd] Full string concatenate
[Phase 1: StringAdd] left_len: 4
[Phase 1: StringAdd] right_len: 1
[Phase 1: StringAdd] Full string concatenate
[Phase 1: StringAdd] left_len: 5
[Phase 1: StringAdd] right_len: 1
[Phase 1: StringAdd] Full string concatenate
[Phase 1: StringAdd] left_len: 6
[Phase 1: StringAdd] right_len: 1
[Phase 1: StringAdd] Full string concatenate
[Phase 1: StringAdd] left_len: 7
[Phase 1: StringAdd] right_len: 1
[Phase 1: StringAdd] Full string concatenate
[Phase 1: StringAdd] left_len: 8
[Phase 1: StringAdd] right_len: 1
[Phase 1: StringAdd] Full string concatenate
[Phase 1: StringAdd] left_len: 9
[Phase 1: StringAdd] right_len: 2
[Phase 1: StringAdd] Full string concatenate
[Phase 1: StringAdd] left_len: 11
[Phase 1: StringAdd] right_len: 2
[Phase 1: StringAdd] Full string concatenate
[Phase 1: StringAdd] left_len: 13
[Phase 1: StringAdd] right_len: 2
[Phase 1: StringAdd] Full string concatenate
[Phase 1: StringAdd] left_len: 15
[Phase 1: StringAdd] right_len: 2
[Phase 1: StringAdd] Full string concatenate
[Phase 1: StringAdd] left_len: 17
[Phase 1: StringAdd] right_len: 2
[Phase 1: StringAdd] Full string concatenate
[Phase 1: StringAdd] left_len: 19
[Phase 1: StringAdd] right_len: 2
[Phase 1: StringAdd] Full string concatenate
[Phase 1: StringAdd] left_len: 21
[Phase 1: StringAdd] right_len: 2
[Phase 2: AllocateConsString] left_len: 21
[Phase 2: AllocateConsString] right_len: 2
[Phase 1: StringAdd] left_len: 23
[Phase 1: StringAdd] right_len: 2
[Phase 2: AllocateConsString] left_len: 23
[Phase 2: AllocateConsString] right_len: 2
[Phase 1: StringAdd] left_len: 25
[Phase 1: StringAdd] right_len: 2
[Phase 2: AllocateConsString] left_len: 25
[Phase 2: AllocateConsString] right_len: 2
[Phase 1: StringAdd] left_len: 27
[Phase 1: StringAdd] right_len: 2
[Phase 2: AllocateConsString] left_len: 27
[Phase 2: AllocateConsString] right_len: 2
[Phase 1: StringAdd] left_len: 29
[Phase 1: StringAdd] right_len: 2
[Phase 2: AllocateConsString] left_len: 29
[Phase 2: AllocateConsString] right_len: 2

[Phase 3: Flatten] ConsString total_length=31
[Phase 3: Flatten] allocating OneByte SeqString, length=31
```