## Speedtest `+=` vs `join`

### `+=` code

```js
str = '';
for (let i = 0; i < 1_000; i++) {
  str += i;
}
```

### `join` code

```js
str = '';
arr = [];
for (let i = 0; i < 1_000; i++) {
	arr.push(i);
}
str = arr.join('');
```

### JSBench.Me

#### Generating stroke

![](JSBench_Me.png)

https://jsbench.me/wymmwc5lqz/1

#### Using stroke

![](JSBench_Me_with_using.png)

https://jsbench.me/vvmmwc6bts/1