```js
  const snareSeq = new Tone.Sequence({
    callback: (time, velocity) => {
    console.log('sequence callback', time);
      snareGen.triggerAttack(time, ftRand(velocity));
    },
    events: [[null, 1, null, 1]],
    subdivision: '1n',
  });
```

ちょっと表記は変わるけど、提示したものと同じです。
このログで、

```
sequence callback 0.7
sequence callback 1.9000000000000001
sequence callback 3.1
sequence callback 4.300000000000001  // ここが出てない
sequence callback 5.5
sequence callback 6.699999999999999
sequence callback 7.8999999999999995
sequence callback 9.1
```

これ以降は、出るんだよな