`Tone.start()` について、と

```js
Tone.getContext().on("statechange", (state) => {  
	console.log(state); // "running" など  
});
```

というのが、あるらしい。あと、`onContextRunning` ってなんだ？



```js
  /* Starting Audio */
  document.addEventListener(
    'pointerup',
    async () => {
      await Tone.start();
      transport.start();
    },
    {
      once: true,
    },
  );
```

と、アクション検知で、`Tone.start();` していて、その流れで`transport.start();` をしている。

が、`transport.start();` は、`running` であれば、そのまま`transport.start();` 自体