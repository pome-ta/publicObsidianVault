もう面倒だから、全部書き落としていくか

# 📝 2026/07/25


## p5 のplayground

sandbox 生成時には、`.html` は差し込みが終わり完了していて欲しいかも。。。
もうp5 の再描画はせずに、ゴリっと書き直しているから。

先にsandbox を作り、js 操作で`script` を差し込んでも、`setup` の`async` のタイミングが悪い気がする。


とはいえ、`.html` ファイル要素を文字列で操作するのも少し気持ち悪い。

### sound のexample

[002-Amplitude-VisualizingLoudness](https://github.com/processing/p5.sound.js/blob/28ab3d7f6ac979f2f62086189bb6bda575fa62b0/examples/002-Amplitude-VisualizingLoudness/sketch.js#L2)

```js
async function setup() {
  sound = loadSound('https://tonejs.github.io/audio/berklee/gong_1.mp3');
  createCanvas(400, 400);
  textAlign(CENTER);
  fill(255);

  amp = new p5.Amplitude();
  sound.connect(amp);

  describe('The color of the background changes based on the amplitude of the sound.');
}
 
function mousePressed() {
  sound.play();
}
 
function draw() {
  let level = amp.getLevel();
  level = map(level, 0, 0.2, 0, 255);
  background(level, 0, 0);
  text('click to play', width/2, height/2);
}
```

`loadSound` に`await` 必要な気がしている。



# 📝 2026/07/24

以前のは、[inBox/02_stackNotes](inBox/02_stackNotes) へ移動
