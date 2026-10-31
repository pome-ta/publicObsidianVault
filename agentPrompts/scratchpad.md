「同じcontext か？」について。ほぼコードの初動で、


```js
  // --- Tone.js
  const p5Ctx = p.getAudioContext();
  Tone.setContext(p5Ctx, true);
  const toneCtx = Tone.getContext();
  /* Starting Audio */
  document.addEventListener(
    'pointerup',
    async () => await Tone.start(),
    {
      once: true,
    },
  );
```

としてるのだが、

`Tone.BitCrusher` とかと、おなじcontext なのか？を確認する方法を知りたい。