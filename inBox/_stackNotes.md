もう面倒だから、全部書き落としていくか


# 📝 2026/07/30

## p5.sound とTone.js

wrapper ってなんやろか（哲学）基本的にTone.js 使用でええんか？

### LSP を加味すると。。。


```html
<script src="http://unpkg.com/tone"></script>
```

これでglobal 展開できるらしいけど（未検証）、LSP の時ATA 考慮すると、`import` 明示させた方がええか？


とはいえ、プレイグラウンドだと、別に書くから点在しちゃうか？




# 📝 2026/07/27

## [p5js4codemirror6](https://github.com/pome-ta/p5js4codemirror6/tree/18c4e92efbb0827eb968b8f0a5432b7b4cc1556f) の書き換え

↑のリンクは、このメモ時のコミット


- sandbox へは、事前に文字列HTML を作り読み込み
    - sandbox を使い回す必要がなくなったため
- いい具合に`fetch` して、取得する
    - `await` をトップレベルで使うと、`document.addEventListener('DOMContentLoaded', () => {});` が面倒になるため、要調整
- sandbox のエントリポイントが、`main.js` をroot となる？

### 非同期source 取得

```js
/* --- load Source */
async function insertFetchDoc(filePath) {
  const fetchFilePath = async (path) => {
    const res = await fetch(path);
    return await res.text();
  };
  return await fetchFilePath(filePath);
}
```

無駄ありすぎるから、そろそろなんとかしたい



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
