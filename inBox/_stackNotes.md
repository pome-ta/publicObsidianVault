もう面倒だから、全部書き落としていくか

# 📝 2026/03/07

simd まできた、、、やはり面倒かもしれん

## simd の前に


```python
from ctypes import Structure, c_float

class simd_float3(Structure):
    _fields_ = [
        ("x", c_float),
        ("y", c_float),
        ("z", c_float),
        ("_pad", c_float),   # ← padding
    ]
```


`simd_float3` のpadding テストをやる


# 📝 2026/03/01

protocol と、Swift とPython(rubicon) でのinit の違いにぐぬぬぬ


# 📝 2026/02/25

## 記事でも書こうかと

yapf のことを書こうかと思ったが

いろいろ面倒に思ってきた。


## Metal

モジュール内の単品実行どうしようかな

単体テストするか？


# 📝 2026/02/24

## Pythonista のフォーマッター

[GitHub - google/yapf: A formatter for Python files](https://github.com/google/yapf) なのだが、

バージョンが`0.32.0` と古い、最新は今現在`0.43.0` 。

### `match` `case` がフォーマットできない

多分バージョンが古く、Python バージョンの文法と合ってない気がする。

本来は、Switch 文なんて、あまり書かない方がいいのだろうが、
対応していない。というのもずいぶん気持ち悪い。

### 無理やり対応させるか。。。

Pythonista にはpip がない。なるべくプレーンにyapf を入れていきたい

[pythonista_reformat.py at main · pome-ta/draftPythonistaScripts · GitHub](https://github.com/pome-ta/draftPythonistaScripts/blob/main/Pythonista3App/modulesMaster/app/pythonista_reformat.py)

これが走るので、


[site-packages/yapf at main · pome-ta/draftPythonistaScripts · GitHub](https://github.com/pome-ta/draftPythonistaScripts/tree/main/Pythonista3App/modulesMaster/site-packages/yapf)

これと見比べならが、やっていく。


### 着地

`/site-packages` 直下に

```
./site-packages
    ├ yapf/
    └ yapf_third_party/
```

と、移動。


`yapf_third_party` は、`third_party` 内のを持ってくる


# 📝 2026/02/21

## モジュール間のpath 連携

05 については、初手の時点では、view 実行のみを想定したpath にする

## メモリ

この沼忘れてた

```
// 16
print(MemoryLayout<float3>.stride)

// float2 = 8
// float3 = 16
// float4 = 16

dump(MemoryLayout<float2>.size) //  8
dump(MemoryLayout<float3>.size) //  16
dump(MemoryLayout<float4>.size) //  16

// 32
print(MemoryLayout<Vertex>.stride)
// .size => 32




print(vertices.count)   // 4
print(MemoryLayout<Vertex>.stride)  //32
print(vertices.count * MemoryLayout<Vertex>.stride)  // 128
```



# 📝 2026/02/20

a-shell Metal 問題は、（もしかしたら）shader のpath 指定が間違えていただけかも、、、
（Pythonista3 に慣れまくっていて、path 扱いが雑だった。。。）

1. path 指定
    - Pythonista3
        - `Path('./Shader.metal')` 
    - a-shell とPythonista3
        - `Path(Path(__file__).parent, 'Shader.metal')`
2. ~`metalView.delegate` 定義位置~
    - `metalView.delegate = self`
    - `drawInMTKView_` ループいか以外での設定を終えた後で定義する位置にする


違う、path 指定の問題ですた。。。エラーが出ないから、気がつけなかったのか、、、


## とはいえ、調査

ChatGPT とGemini に色々と（嘘の）アドバイス貰っているので、この際に確認するでもいいかも

- lifeCycle
- onMainThread
- `dispatch_semaphore`
- `autoreleasepool`
    - なんかちゃんと動いてるっぽい

整理して、理解が深まればいいかも


# 📝 2026/02/16

## どっちが原因として考えればええやろか？

- `lifeCycle.py` としてる`asyncio` 周りが果たして正しいのかのか
- Metal の処理として誤ったことをしているのではないか？


そもそもとして、いまの設計になったのはいつ頃からどんな経緯でできたのか？




# 📝 2026/02/15


## a-shell のコマンドで、View が出るやつの確認

[`pickFolder` | a-Shell/SceneDelegate.swift](https://github.com/holzschu/a-shell/blob/9013425d35cd406da535cae40c80159caab4667b/a-Shell/SceneDelegate.swift#L2014)
[`@_cdecl("pickFolder")` | a-Shell/ExtraCommands.swift](https://github.com/holzschu/a-shell/blob/9013425d35cd406da535cae40c80159caab4667b/a-Shell/ExtraCommands.swift#L249)

### コード

```python
UIApplication = ObjCClass('UIApplication')

sharedApplication = UIApplication.sharedApplication


for scene in sharedApplication.connectedScenes.allObjects():
  pdbr.state(scene)
```

### a-shell


```
["ObjCClass('NSObject')",
 "ObjCClass('UIResponder')",
 "ObjCClass('UIScene')",
 "ObjCClass('UIWindowScene')",
 "ObjCClass('NSKVONotifying_UIWindowScene')"]
<ObjCInstance: NSKVONotifying_UIWindowScene at 0x1270b7d90: <UIWindowScene: 0x106764000; role: UIWindowSceneSessionRoleApplication; activationState: UISceneActivationStateForegroundActive> {
    session = <UISceneSession: 0x10659dfe0; persistentIdentifier: 327CBD3C-9EDE-4C0B-B363-C7B4FF5A2754> {
        configuration = <UISceneConfiguration: 0x142110a00; name: "Default Configuration">;
    };
    delegate = <a_Shell_mini.SceneDelegate: 0x101d9f060>;
    effectiveGeometry = <UIWindowSceneGeometry: 0x106799130; coordinateSpace.bounds: {{0, 0}, {393, 852}}; interfaceOrientation: portrait (1); isInteractivelyResizing: NO>;
    screen = <UIScreen: 0x1064d0780; bounds: {{0, 0}, {393, 852}}; mode: <UIScreenMode: 0x1244a5e40; size = 1179.000000 x 2556.000000>>;
    windows = {
        <UIWindow: 0x101da03e0; frame = (0 0; 393 852); autoresize = W+H; gestureRecognizers = <NSArray: 0x124884240>; layer = <UIWindowLayer: 0x106425440>>;
        <UITextEffectsWindow: 0x119604000; frame = (0 0; 393 852); opaque = NO; autoresize = W+H; gestureRecognizers = <NSArray: 0x14217aaf0>; layer = <UIWindowLayer: 0x1196016c0>>;
    }
}>

```


### Pythonista3

```
["ObjCClass('NSObject')",
 "ObjCClass('UIResponder')",
 "ObjCClass('UIScene')",
 "ObjCClass('UIWindowScene')"]
<ObjCInstance: UIWindowScene at 0x122997fd0: <UIWindowScene: 0x101629500; role: UIWindowSceneSessionRoleApplication; activationState: UISceneActivationStateForegroundActive> {
    session = <UISceneSession: 0x106194550; persistentIdentifier: E386F5EB-87BA-433E-8B31-3C33870E36F9> {
        configuration = <UISceneConfiguration: 0x1222f7570; name: 0x0>;
    };
    delegate = (nil);
    effectiveGeometry = <UIWindowSceneGeometry: 0x106194960; coordinateSpace.bounds: {{0, 0}, {393, 852}}; interfaceOrientation: portrait (1); isInteractivelyResizing: NO>;
    screen = <UIScreen: 0x1060e0780; bounds: {{0, 0}, {393, 852}}; mode: <UIScreenMode: 0x1204256a0; size = 1179.000000 x 2556.000000>>;
    windows = {
        <PA3PythonistaWindow: 0x101632e30; baseClass = UIWindow; frame = (0 0; 393 852); autoresize = W+H; gestureRecognizers = <NSArray: 0x1221bd320>; layer = <UIWindowLayer: 0x1061f9f00>>;
        <UITextEffectsWindow: 0x109bd7700; frame = (0 0; 393 852); opaque = NO; autoresize = W+H; gestureRecognizers = <NSArray: 0x1174c8ee0>; layer = <UIWindowLayer: 0x120703580>>;
    }
}>

```


うーん、調査的にはあまり意味なさそう。。。

## a-shell でMetal が動かない

- 2回目が起動View すら出ず止まる
- `commandBuffer.commit()` のように、レンダリングすると、初回からだめ
- 事前に裏で走っていて連動していないか？
    - 一回のみのレンダリングでもダメっぽい


# 📝 2026/02/13

## a-shell でMetal だめか？


```lifeCycle.py
import asyncio
from pyrubicon.objc.eventloop import RubiconEventLoop, EventLoopPolicy

__all__ = [
  'loop',
]

try:
  print('try s: loop')
  loop = asyncio.get_running_loop()
  print('try e: loop')
except RuntimeError as e:
  print(f'RuntimeError: {e}')
  print('except s: loop')
  loop = RubiconEventLoop()
  asyncio.set_event_loop(loop)
  print('except e: loop')
```

### Pythonista3

#### Metal 1 回目

```
try s: loop
RuntimeError: no running event loop
except s: loop
except e: loop
--- --- start
init
s: set_rootViewController
e: set_rootViewController
main
present
app s: run
RootNavigationController: loadView
    - MainViewController: loadView
    - MainViewController: viewDidLoad
v
    - MainViewController: viewWillDisappear_
RootNavigationController: viewWillDisappear_
    - MainViewController: viewDidDisappear_
RootNavigationController: viewDidDisappear_
- RootNavigationController: dealloc
stop
app e: run
app s: close
app e: close
main.present


```

#### Metal 2回目

```
--- --- start
init
s: set_rootViewController
e: set_rootViewController
main
present
app s: run
RootNavigationController: loadView
    - MainViewController_2: loadView
    - MainViewController_2: viewDidLoad
v
    - MainViewController_2: viewWillDisappear_
RootNavigationController: viewWillDisappear_
    - MainViewController_2: viewDidDisappear_
RootNavigationController: viewDidDisappear_
- RootNavigationController: dealloc
stop
app e: run
app s: close
app e: close
main.present
	 - MainViewController: dealloc


```

#### UIView 1回目

```
try s: loop
RuntimeError: no running event loop
except s: loop
except e: loop
init
s: set_rootViewController
e: set_rootViewController
present
app s: run
RootNavigationController: loadView
    - MainViewController: loadView
    - MainViewController: viewDidLoad
RootNavigationController: viewWillDisappear_
    - MainViewController: viewDidDisappear_
RootNavigationController: viewDidDisappear_
- RootNavigationController: dealloc
stop
app e: run
app s: close
app e: close


```

#### UIView 2回目

```
init
s: set_rootViewController
e: set_rootViewController
present
app s: run
RootNavigationController: loadView
    - MainViewController_2: loadView
    - MainViewController_2: viewDidLoad
RootNavigationController: viewWillDisappear_
    - MainViewController_2: viewDidDisappear_
RootNavigationController: viewDidDisappear_
- RootNavigationController: dealloc
stop
app e: run
app s: close
app e: close
	 - MainViewController: dealloc


```


### a-shell

うまくコピーできんかったけど、

`RuntimeError: no running event loop` まいかい走ってる

```
try s: loop
RuntimeError: no running event loop
except s: loop
except e: loop
--- --- start
init
s: set_rootViewController
e: set_rootViewController
main
present
app s: run
RootNavigationController: loadView
    - MainViewController: loadView
    - MainViewController: viewDidLoad
v
    - MainViewController: viewWillDisappear_
RootNavigationController: viewWillDisappear_
    - MainViewController: viewDidDisappear_
RootNavigationController: viewDidDisappear_

```


あと`s: set_rootViewController` ここで止まるかも


# 📝 2026/02/10

`:%s/hoge.*huga/置換後文字列/g`

特殊文字エスケープは、`\`

# 📝 2026/02/09

## `objc_property`

結構かなり不要みたい。

今度、やるコトない時に`objc_property` 処理を消して回るかぁ


# 📝 2026/02/08


## BeginningMetal/02_GettingStarted

久々の写経

[pystaRubiconObjcSandBox/playground/Metal/BeginningMetal/memo.md at main · pome-ta/pystaRubiconObjcSandBox · GitHub](https://github.com/pome-ta/pystaRubiconObjcSandBox/blob/main/playground/Metal/BeginningMetal/memo.md)

### MTLClearColor やMTLClearColorMake

make に関しては、Metal Framework からfunction みつからん。

view 上で型を作ってなげても、元が`objc.types._Anonymous` の型になっているので、エラー。

とりあえず、簡単にtuple 投げるようにしてる。

[toga/iOS/src/toga_iOS/libs/mapkit.py at afac874cf430f13679b9d05e9ba370e18258d15f · beeware/toga · GitHub](https://github.com/beeware/toga/blob/afac874cf430f13679b9d05e9ba370e18258d15f/iOS/src/toga_iOS/libs/mapkit.py#L37) ここでどうにかなるんか？知らんけど

`@with_preferred_encoding` と`@with_encoding` ？多分違う気がする

↑ 正解でした
[pystaRubiconObjcSandBox/modules/objc_frameworks/Metal/MTLRenderPass.py at 02fce42c97087acad6ebdac37e40495c22b90099 · pome-ta/pystaRubiconObjcSandBox](https://github.com/pome-ta/pystaRubiconObjcSandBox/blob/02fce42c97087acad6ebdac37e40495c22b90099/modules/objc_frameworks/Metal/MTLRenderPass.py#L6)


### MTKViewDelegate

`drawInMTKView_` だけではだめ、`mtkView_drawableSizeWillChange_` も定義（中は空）してあげんと落ちるで。


### formatter 用のコメントなんだっけ？

配列をガガーっと書きそうなので、強制的に整理しないコメント

`# yapf: disable`

過去に、使ってたけど、忘れっちゃたので、ここに




# 📝 2026/02/05

[Displaying a point cloud using scene depth | Apple Developer Documentation](https://developer.apple.com/documentation/arkit/displaying-a-point-cloud-using-scene-depth?language=objc)

これ、ビルドしたら、なんか変？（iPhone 15 Pro）


## なんとなくやっていき

### `UIViewController` がファットになりそう

継承を想定していくか？

ん？View って継承したとして、必須なメソッドってある？



# 📝 2026/02/03

再挑戦

[Displaying a point cloud using scene depth | Apple Developer Documentation](https://developer.apple.com/documentation/arkit/displaying-a-point-cloud-using-scene-depth?language=objc)

うーんSwiftUI だから、読み解きづらい、、、





# 📝 2026/01/30

## `CGAffineTransform`

[CGAffineTransform | Apple Developer Documentation](https://developer.apple.com/documentation/coregraphics/cgaffinetransform?language=objc)

`CGAffineTransform(scaleX: 1.0/captureSize.width, y: 1.0/captureSize.height)`

イニシャライザで、`scaleX` とあるから、

[CGAffineTransformMakeScale | Apple Developer Documentation](https://developer.apple.com/documentation/coregraphics/cgaffinetransformmakescale(_:_:)?language=objc)


# 📝 2026/01/29


[【Swift】画面の向き（縦横）が変わったことを検知する方法 | Swift Note](https://naoya-ono.com/swift/swift-orientation/)



# 📝 2026/01/27

## depth

[ExampleOfiOSLiDAR/ExampleOfiOSLiDAR/Samples/PointCloud/PointCloudViewController.swift at main · TokyoYoshida/ExampleOfiOSLiDAR · GitHub](https://github.com/TokyoYoshida/ExampleOfiOSLiDAR/blob/main/ExampleOfiOSLiDAR/Samples/PointCloud/PointCloudViewController.swift)

こっちにするか、SwiftUI 読み解きは少し辛い

[ARKit 4のLiDAR Depth API #Swift - Qiita](https://qiita.com/shu223/items/722077b4bbe868f58ddc)
後にこれか？



[ここから始まるお手軽地形計測 iPhoneへLiDARスキャナ搭載【ARKit】 - aptpod Tech Blog](https://tech.aptpod.co.jp/entry/2020/12/22/100000)





# 📝 2026/01/18

## ドキュメントを読む

[Displaying a point cloud using scene depth | Apple Developer Documentation](https://developer.apple.com/documentation/arkit/displaying-a-point-cloud-using-scene-depth?language=objc)


`PointCloudDepthSample.swift` とりま、これがentry point 。ファイル内は、超簡潔。


view のstack 処理は、色々な見せ方の場合分け？


# 📝 2026/01/14

## Depth にする

mesh の`node.setSimdTransform_(anchor.transform)` がダメなため。

`transform` を呼べない。

[ARKitのLiDAR Depth API](https://zenn.dev/shu223/articles/arkit-lidar-depth)



```
 "ObjCClass('ARDepthData')": [
    "adCameraCalibration",
    "cameraIntrinsics",
    "confidenceBuffer",
    "confidenceMap",
    "dealloc",
    "depthMap",
    "depthSensorsState",
    "deviceTransform",
    "encodeWithCoder_",
    "extrinsicsToAppNode",
    "initWithCoder_",
    "initWithDepthMap_confidenceMap_",
    "isValid",
    "normalsBuffer",
    "setAdCameraCalibration_",
    "setCameraIntrinsics_",
    "setConfidenceBuffer_",
    "setConfidenceMap_",
    "setDepthMap_",
    "setDepthSensorsState_",
    "setDeviceTransform_",
    "setExtrinsicsToAppNode_",
    "setIsValid_",
    "setNormalsBuffer_",
    "setTimestamp_",
    "timestamp",
    "tracingEntry"
  ]
```

```
["ObjCClass('NSObject')", "ObjCClass('ARDepthData')"]
<ObjCInstance: ARDepthData at 0x13d8f01c0: <ARDepthData: 0x12b255e00>>

```

## いや、ワンチャンあるか？

[SceneKitLiDARMesh.swift · GitHub](https://gist.github.com/eospi/b54e412afde3f07942240e0e306a32f2)


### あった

コードがちょっとアレだったが（実装意図もわからず）できたやも？

[draftPythonistaScripts/objcs/ARKit/rubiconARKit/rendererSceneKitLiDARMesh.py at main · pome-ta/draftPythonistaScripts · GitHub](https://github.com/pome-ta/draftPythonistaScripts/blob/main/objcs/ARKit/rubiconARKit/rendererSceneKitLiDARMesh.py)


### mesh のいろつけ

[swift - SceneKitのSCNGeometryで線を描いた時に色をつけることができない。 - スタック・オーバーフロー](https://ja.stackoverflow.com/questions/53124/scenekit%E3%81%AEscngeometry%E3%81%A7%E7%B7%9A%E3%82%92%E6%8F%8F%E3%81%84%E3%81%9F%E6%99%82%E3%81%AB%E8%89%B2%E3%82%92%E3%81%A4%E3%81%91%E3%82%8B%E3%81%93%E3%81%A8%E3%81%8C%E3%81%A7%E3%81%8D%E3%81%AA%E3%81%84)



# 📝 2026/01/13

## まーた`SIMD` さんですか？

[simdTransform | Apple Developer Documentation](https://developer.apple.com/documentation/scenekit/scnnode/simdtransform?language=objc)

[transform | Apple Developer Documentation](https://developer.apple.com/documentation/arkit/aranchor/transform?language=objc)

[simd_float4x4 | Apple Developer Documentation](https://developer.apple.com/documentation/simd/simd_float4x4?language=objc)


```
["ObjCClass('NSObject')", "ObjCClass('ARAnchor')", "ObjCClass('ARMeshAnchor')"]
<ObjCInstance: ARMeshAnchor at 0x12eacfd00: <ARMeshAnchor: 0x1592c3e80 identifier="2AEC1C54-6543-45F5-9692-540932D73690" geometry="<ARMeshGeometry: 0x15aaf4e70 vertices="<ARGeometrySource: 0x15b2e8a40 count="587">" normals="<ARGeometrySource: 0x15b2e9d40 count="587">" faces="<ARGeometryElement: 0x15aaf5230 count="823">">" transform=<translation=(0.132272 -0.596969 -0.096379) rotation=(-0.00° -93.02° 0.00°)>>>
```

```
["ObjCClass('NSObject')", "ObjCClass('ARAnchor')", "ObjCClass('ARMeshAnchor')"]
<ObjCInstance: ARMeshAnchor at 0x12385ce20: <ARMeshAnchor: 0x1592c1040 identifier="E32410D4-B2EC-45E5-8158-ADB95DE4F5AF" geometry="<ARMeshGeometry: 0x15aaf4330 vertices="<ARGeometrySource: 0x15b2e8b80 count="643">" normals="<ARGeometrySource: 0x15b2e9380 count="643">" faces="<ARGeometryElement: 0x15aaf6670 count="1128">">" transform=<translation=(0.252113 0.003031 -0.090184) rotation=(-0.00° -93.02° 0.00°)>>>
```

```
["ObjCClass('NSObject')", "ObjCClass('ARAnchor')", "ObjCClass('ARMeshAnchor')"]
<ObjCInstance: ARMeshAnchor at 0x1279e1000: <ARMeshAnchor: 0x14e0e8a00 identifier="49D419B2-942B-43D6-ACD9-5D562E746FD6" geometry="<ARMeshGeometry: 0x14cd6ab80 vertices="<ARGeometrySource: 0x14f7930c0 count="85">" normals="<ARGeometrySource: 0x14f793100 count="85">" faces="<ARGeometryElement: 0x14cec50b0 count="70">">" transform=<translation=(-0.303127 0.007237 -0.354261) rotation=(-0.00° -91.01° 0.00°)>>>
```

# 📝 2026/01/10

## ARKit とLiDAR

`ARView` が使えない（RealityKit  がダメ）ので、`ARSCNView` で進めることに。

しかし、RealityKit 前提のコードが多い & サンプルがどのような挙動になるかわからん。。。


何をしたいのか明確にステップ決めて行った方がいいな


日本語だと、なんかcss うまく当たってない？

- [Metalを利用したAR体験の表示 - 日本語ドキュメント - Apple Developer](https://developer.apple.com/jp/documentation/arkit/displaying_an_ar_experience_with_metal/)
- [Displaying an AR Experience with Metal | Apple Developer Documentation](https://developer.apple.com/documentation/arkit/displaying-an-ar-experience-with-metal?language=objc)


これになるのかな？

- [Displaying a point cloud using scene depth | Apple Developer Documentation](https://developer.apple.com/documentation/arkit/displaying-a-point-cloud-using-scene-depth?language=objc)


## 参照するコード

とりあえずで、以下の実装ができるように。。。

- [ARKit と LiDAR で 3 次元空間認識して SceneKit でリアルタイム描画 - OPTiM TECH BLOG](https://tech-blog.optim.co.jp/entry/2021/05/06/100000)

SwiftのConvenience Initialize で、`SCNGeometry`, `SCNGeometrySource`, `SCNGeometryElement` まるっと生成している。。。

- [SCNGeometry | Apple Developer Documentation](https://developer.apple.com/documentation/scenekit/scngeometry?language=objc)
- [SCNGeometrySource | Apple Developer Documentation](https://developer.apple.com/documentation/scenekit/scngeometrysource?language=objc)
    - [init(buffer:vertexFormat:semantic:vertexCount:dataOffset:dataStride:) | Apple Developer Documentation](https://developer.apple.com/documentation/scenekit/scngeometrysource/init(buffer:vertexformat:semantic:vertexcount:dataoffset:datastride:)?language=objc)
        - [SCNGeometrySourceSemanticVertex | Apple Developer Documentation](https://developer.apple.com/documentation/scenekit/scngeometrysource/semantic-swift.struct/vertex?language=objc)
- [SCNGeometryElement | Apple Developer Documentation](https://developer.apple.com/documentation/scenekit/scngeometryelement?language=objc)
    - [init(data:primitiveType:primitiveCount:bytesPerIndex:) | Apple Developer Documentation](https://developer.apple.com/documentation/scenekit/scngeometryelement/init(data:primitivetype:primitivecount:bytesperindex:)?language=objc)
        - [initWithBytesNoCopy:length:deallocator: | Apple Developer Documentation](https://developer.apple.com/documentation/foundation/nsdata/init(bytesnocopy:length:deallocator:)?language=objc)
        - [initWithBytesNoCopy:length: | Apple Developer Documentation](https://developer.apple.com/documentation/foundation/nsdata/init(bytesnocopy:length:)?language=objc)
            - [init(bytesNoCopy:count:deallocator:) | Apple Developer Documentation](https://developer.apple.com/documentation/foundation/data/init(bytesnocopy:count:deallocator:))

rubicon でのclass のオーバーライドで考えると、ちょっと事故りそうなんよな。。。
素直に書くか。。。

### SCNGeometryElement

[geometryElementWithData:primitiveType:primitiveCount:bytesPerIndex: | Apple Developer Documentation](https://developer.apple.com/documentation/scenekit/scngeometryelement/init(data:primitivetype:primitivecount:bytesperindex:)?language=objc) の`data` が怪しいな。。。


```swift
        // Indices element
        let faces = meshGeometry.faces
        let facesElement = SCNGeometryElement(
            data: Data(
                bytesNoCopy: faces.buffer.contents(),
                count: faces.buffer.length,
                deallocator: .none
            ),
            primitiveType: .triangles,
            primitiveCount: faces.count,
            bytesPerIndex: faces.bytesPerIndex
        )

```

`contents` が、`c_void_p` になるから、これをうまい感じにどうにかしなきゃ。。。

### ARMeshGeometry を深追い

[ARMeshGeometry | Apple Developer Documentation](https://developer.apple.com/documentation/arkit/armeshgeometry?language=objc)

#### `.faces`

[faces | Apple Developer Documentation](https://developer.apple.com/documentation/arkit/armeshgeometry/faces?language=objc)

```swift
extension ARMeshGeometry {
    func vertexIndicesOf(faceWithIndex index: Int) -> [Int] {
        let indicesPerFace = faces.indexCountPerPrimitive
        let facesPointer = faces.buffer.contents()
        var vertexIndices = [Int]()
        for offset in 0..<indicesPerFace {
            let vertexIndexAddress = facesPointer.advanced(by: (index * indicesPerFace + offset) * MemoryLayout<UInt32>.size)
            vertexIndices.append(Int(vertexIndexAddress.assumingMemoryBound(to: UInt32.self).pointee))
        }
        return vertexIndices
    }
}
```

```swift
let facesPointer = faces.buffer.contents()
```

[contents() | Apple Developer Documentation](https://developer.apple.com/documentation/metal/mtlbuffer/contents()?language=objc)



```
<AGXG16PFamilyBuffer: 0x12ba53900>
    label = <none> 
    length = 4096 
    cpuCacheMode = MTLCPUCacheModeDefaultCache 
    storageMode = MTLStorageModeShared 
    hazardTrackingMode = MTLHazardTrackingModeTracked 
    resourceOptions = MTLResourceCPUCacheModeDefaultCache MTLResourceStorageModeShared MTLResourceHazardTrackingModeTracked  
    purgeableState = MTLPurgeableStateNonVolatile
```

これ、metal か何かでやったっけか？


```
<AGXG16PFamilyBuffer: 0x158006700>
    label = <none> 
    length = 256 
    cpuCacheMode = MTLCPUCacheModeDefaultCache 
    storageMode = MTLStorageModeShared 
    hazardTrackingMode = MTLHazardTrackingModeTracked 
    resourceOptions = MTLResourceCPUCacheModeDefaultCache MTLResourceStorageModeShared MTLResourceHazardTrackingModeTracked  
    purgeableState = MTLPurgeableStateNonVolatile


```

[pystaMetalStudy/src/everythingAboutTheMetalAPI/chapter05/__main__.py at 476f24b20c2710ca2661b5f23eab14ee7291e0a4 · pome-ta/pystaMetalStudy · GitHub](https://github.com/pome-ta/pystaMetalStudy/blob/476f24b20c2710ca2661b5f23eab14ee7291e0a4/src/everythingAboutTheMetalAPI/chapter05/__main__.py#L133)




# 📝 2026/01/09

[ARKit入門 / 特徴点の検出｜npaka](https://note.com/npaka/n/nbaf752f684f8)


[（Swift）iPhoneのLiDARセンサーから無加工のデータを取得する](https://zenn.dev/moutend/articles/552aa982f5cefb)

## AVCaptureDevice

[AVCaptureDevice | Apple Developer Documentation](https://developer.apple.com/documentation/avfoundation/avcapturedevice?language=objc)

これもあり？

## ARSessionDelegate

[ARSessionDelegate | Apple Developer Documentation](https://developer.apple.com/documentation/arkit/arsessiondelegate?language=objc)

のちのち、継承して見通しよくできるといいな


# 📝 2026/01/05


## `enumerations.py`

書き換え

## `globalVariables.py`

ディレクトリで分けるか


# 📝 2026/01/05


## rubicon での、`Global Variable`

- [ARSCNDebugOptionShowFeaturePoints | Apple Developer Documentation](https://developer.apple.com/documentation/arkit/arscndebugoptionshowfeaturepoints?language=objc)
- [ARSCNDebugOptionShowWorldOrigin | Apple Developer Documentation](https://developer.apple.com/documentation/arkit/arscndebugoptionshowworldorigin?language=objc)


### `Global Variable` の整理

[Global variables and constants (e.g. `NSFoundationVersionNumber`) | Calling plain C functions from Python - Rubicon ObjC](https://rubicon-objc.beeware.org/en/stable/topics/c-functions/#global-variables-and-constants-eg-nsfoundationversionnumber)



## RealityKit はやはり無理か？

- [Does this mean that I can use Rust to build an immersive application of CompositorService on visionOS through this project? · Issue #668 · madsmtm/objc2 · GitHub](https://github.com/madsmtm/objc2/issues/668)
- [Consider adding support for Swift interop? · Issue #524 · madsmtm/objc2 · GitHub](https://github.com/madsmtm/objc2/issues/524)


# 📝 2026/01/03

## SceneKit だわ、、、

[SceneKit | Apple Developer Documentation](https://developer.apple.com/documentation/scenekit?language=objc)

## レイアウト

文字だと右左あるけど、位置だときにしなくていいか

```python
    NSLayoutConstraint.activateConstraints_([
      showingHidingStackView.leadingAnchor.constraintEqualToAnchor_(
        layoutMarginsGuide.leadingAnchor),
      showingHidingStackView.trailingAnchor.constraintEqualToAnchor_(
        layoutMarginsGuide.trailingAnchor),
      showingHidingStackView.topAnchor.constraintEqualToAnchor_constant_(
        safeAreaLayoutGuide.topAnchor, 8.0),
    ])
```

```python
    NSLayoutConstraint.activateConstraints_([
      scnView.centerXAnchor.constraintEqualToAnchor_(
        safeAreaLayoutGuide.centerXAnchor),
      scnView.centerYAnchor.constraintEqualToAnchor_(
        safeAreaLayoutGuide.centerYAnchor),
      scnView.widthAnchor.constraintEqualToAnchor_multiplier_(
        safeAreaLayoutGuide.widthAnchor, 1.0),
      scnView.heightAnchor.constraintEqualToAnchor_multiplier_(
        safeAreaLayoutGuide.heightAnchor, 1.0),
    ])

```



## メモ

- [業務でSceneKitが必要になりそうな気配がするのでメモ](https://zenn.dev/yukichi_tech/scraps/b91fa600175e2b)
- [ARViewとARSCNViewとRealityKitとSceneKitの違い](https://zenn.dev/adaniya/books/0e7251467ddfea/viewer/2c8e6a)
- [SCNTechniqueが[SceneKit] Error: can not render without programs, using defaultと言われて適用できないとき。 #Swift - Qiita](https://qiita.com/noppefoxwolf/items/73d74876bae4cc49f204)


## 勉強し直し

Deprecated でレガシーなものを勉強し直すのも癪だが、仕方なし。

[Pythonista3 で3DCG やろうぜ！SceneKit のプリミティブなもので遊ぶ #Mobile - Qiita](https://qiita.com/pome-ta/items/551bf5fb2448ddcacae0)





# 📝 2026/01/02

1. LiDAR で何かやりたい
2. ARKit を使う
3. rubicon でそこらへんのViewController やってないかも
4. SceneKit からおさらいか？
5. `SceneKit` が`Deprecated` !?
  - [SceneKit | Apple Developer Documentation](SceneKit)
6. RealityKit でいける？

# 📝 2026/01/01

## LiDAR やってみる？

iPhone 15 Pro にしたので、11 にはハードウェア的になかったLiDAR をrubicon-objc 経由でいじってみるか

- [iPhoneのLiDARを使って撮影したデータはどのようにして3Dデータに変換されるのかっていう雑記 #QiitaEngineerFesta2022 - Qiita](https://qiita.com/nokonoko_1203/items/6a836fb0e1d2d3f6e3a6)
- [LiDARスキャナを使って現実世界をボクセルにしてみた #iOS - Qiita](https://qiita.com/TokyoYoshida/items/1c62c01093e6a9be442e)
- [（Swift）iPhoneのLiDARセンサーから無加工のデータを取得する](https://zenn.dev/moutend/articles/552aa982f5cefb)
- [ARKit と LiDAR で 3 次元空間認識して SceneKit でリアルタイム描画 - OPTiM TECH BLOG](https://tech-blog.optim.co.jp/entry/2021/05/06/100000)

## とりま、ARKit か？

完全に忘れてるからいいチャンスかも
一度ARKit やり直しもありかも？

### 過去の差分

- [pystaUIKitCatalogChallenge/rbedge/rootNavigationController.py at main · pome-ta/pystaUIKitCatalogChallenge · GitHub](https://github.com/pome-ta/pystaUIKitCatalogChallenge/blob/main/rbedge/rootNavigationController.py)
- [draftPythonistaScripts/objcs/ARKit/rubiconARKit/rbedge/rootNavigationController.py at main · pome-ta/draftPythonistaScripts · GitHub](https://github.com/pome-ta/draftPythonistaScripts/blob/main/objcs/ARKit/rubiconARKit/rbedge/rootNavigationController.py)



`rootNavigationController` の挙動どうしようかな。一応WKWebView 用の調整だったけども。。。


# 📝 2025/12/31

iPhone 15 Pro に変更


# 📝 2025/12/14

a-shell

iPhone だと、なぜセッションが残るのか？

とか、Terminal コマンドがわからんから整理してみたい。


```
a-Shell is a terminal emulator for iOS, with many Unix commands: ls, pwd, tar, mkdir, grep....

a-Shell can do most of the things you can do in a terminal, locally on your iPhone or iPad. You can redirect command output to a file with ">" (append with ">>") and you can pipe commands with "|".

- customize appearance with config
- pickFolder: open, bookmark and access a directory anywhere (another app, iCloud, WorkingCopy, file providers...)
- newWindow: open a new window
- exit: close the current window

- All your files, including configuration files (.bashrc, .profile, .ssh...) are in ~/Documents/
- Files created by Shortcuts are in ~shortcuts/
- a-Shell executes the ~/Documents/.profile and ~/Documents/.bashrc files for each new window

- Single-finger swipes move the cursor or scroll, two-finger swipes send keyboard input (up, down, escape, tab). "man gestures" for more.

- Edit files with vim and pico.
- Transfer files with curl, tar, scp and sftp.
- Clone repositories and do version control with lg2 (similar to git)
- Install more commands with "pkg"
- Process files with python3, lua, jsc, clang, pdflatex, lualatex.
- Open files in other apps with open, play sound and video with play, preview with view.
- For network queries: nslookup, ping, host, whois, ifconfig...

- bookmark the current directory with "bookmark <name>" and access it later with "cd ~name" or "jump <name>".
- showmarks: show current list of bookmarks
- renamemark, deletemark: change list of bookmarks

User guide: https://bianshen00009.gitbook.io/a-guide-to-a-shell/
Support: e-mail (another_shell@icloud.com), Bluesky (@a-shell-ios.bsky.social‬), github (https://github.com/holzschu/a-shell/issues) and Discord (https://discord.gg/cvYnZm69Gy).

For a full list of commands, type help -l

```


a-ShellはiOS用のターミナルエミュレータで、多くのUnixコマンドを備えています：`ls`、`pwd`、`tar`、`mkdir`、`grep` ...
a-Shellは、iPhoneやiPad上でローカルに、ターミナルで実行できるほとんどの操作を実行できます。コマンドの出力をファイルにリダイレクトするには「`>`」を（追加するには「`>>`」を）、コマンドをパイプでつなぐには「`|`」を使用できます。
- 設定で外観をカスタマイズ
- `pickFolder`: ディレクトリをどこでも開く、ブックマークする、アクセスする（他のアプリ、iCloud、WorkingCopy、ファイルプロバイダなど）
- `newWindow`: 新しいウィンドウを開く
- `exit`: 現在のウィンドウを閉じる


- 設定ファイル（`.bashrc`、`.profile`、`.ssh` など）を含むすべてのファイルは、`~/Documents/` にあります。
- ショートカットによって作成されたファイルは、`~shortcuts/` にあります。
- a-Shellは、新しいウィンドウごとに`~/Documents/.profile` と`~/Documents/.bashrc` ファイルを実行します。

- 1 本指でスワイプするとカーソルを移動したりスクロールしたりでき、2 本指でスワイプするとキーボード入力 (上、下、Esc、Tab) を送信できます。詳細については、「`man gestures`」を参照してください。

```
alias
apropos
  - 「man」コマンドで表示されるやつ（マニュアル）に関する情報を表示するときに使うコマンド
  - 指定キーワードでwhatisデータベースの中身を「部分一致で」検索するときに使うコマンド
awk
base64
basename
  - パス名からファイル名を取得する
bc
  - 電卓
blink
  - x86-64Linux プログラム実行の仮想マシン？
  - Blink Shell のやつ？
bookmark
brave
calc
call
cat
cd
chflags
chmod
cksum
clear
compare
composite
compress
config
conjure
convert
cowsay
cp
credits
ctags.wasm3
curl
cygdb
cython
cythonize
dash
date
dc
deactivate
deletemark
diff
dig
dirname
downloadFile
downloadFolder
du
ebong
echo
ed
egrep
env
exit
export
expr
ffmpeg
ffprobe
fgrep
file.wasm3
find
firefox
googlechrome
grep
gunzip
gzip
head
help
hideKeyboard
hideToolbar
history
host
id
identify
idle3
idle3.13
ifconfig
import
internalbrowser
ipython
ipython3
isForeground
jq
jsc
jsc_core
jsi
jump
keepDirectoryAfterShortcut
less
lessecho
lex
lg2
link
ln
ls
lua
luac
magick
magick-script
man
mandoc
mandocdb
md5
mkdir
mktemp
mogrify
montage
more
multimarkdown
mv
nc
newWindow
nslookup
open
openrsync
openurl
opera
pbcopy
pbpaste
pickFolder
pico
ping
ping6
pip
pip3
pip3.13
pkg
play
printenv
pwd
pydoc3
pydoc3.13
pygmentize
pyproject-build
python
python3
python3-config
python3.13
python3.13-config
readlink
readtags.wasm3
realpath
rehash
renamemark
repeatCommand
rlogin
rm
rmdir
rsync
safari
say
scp
sed
setenv
setuptools-scm
sftp
sh
sha1
sha256
showToolbar
showmarks
sleep
sort
source
ssh
ssh-add
ssh-agent
ssh-copy-id
ssh-keygen
stat
stream
stty
sum
tail
tar
task
tee
telnet
text
touch
tr
tree.wasm3
unalias
uname
uncompress
uniq
unlink
unrar
unsetenv
uptime
view
vim
wasm
wasm3
wc
whatis
wheel
whoami
whois
wol
xargs
xxd
yandexbrowser
z

```

# 📝 2025/12/13

chatGPT がゴリっと内容を忘れてしまってるから
いい感じに切り分けが必要？

あらためて整理が必要やな。

## 何を目指して、何をしようとしていたのか？

プロンプトとして、再度使えるように整理していきたい。

大枠や、疑問点。現在の状況と見通し。


## codemirror のLSP

[GitHub - codemirror/dev: Development repository for the CodeMirror editor project](https://github.com/codemirror/dev)

[GitHub - codemirror/lsp-client: Language server protocol client for CodeMirror](https://github.com/codemirror/lsp-client)


[@codemirror/lsp-client - npm](https://www.npmjs.com/package/@codemirror/lsp-client)

で、LSP を実現。

### 実装環境（事情）

ブラウザで完結。iOS Safari 。

node を使わない。node を使わない（使えない）ので:

- `package.json` ではなく、`importmap`
- ビルドなし
- サーバーを建てない
- WebSocket ではなくWebWorker






# 📝 2025/12/04

```
.
├── core
│   ├── error-codes.js
│   ├── lsp-core.js
│   └── vfs-core.js
├── main.js
├── test
│   ├── test-runner.js
│   └── v0.0.2
│       ├── notUsed
│       │   ├── diag-chain-import.test.js
│       │   ├── diag-chain-raw.test.js
│       │   ├── diag-chain-related.test.js
│       │   └── diag-utils.unit.test.js
│       ├── test-utils.js
│       ├── worker-diagnostics-flatten.test.js
│       ├── worker-didChange-basic.test.js
│       ├── worker-didChange-incremental-min.test.js
│       ├── worker-lsp-diagnostics.test.js
│       ├── worker-lsp-initialize.test.js
│       ├── worker-lsp-multi-file.test.js
│       ├── worker-ready.test.js
│       ├── worker-vfs-cached-init.test.js
│       ├── worker-vfs-file.test.js
│       └── worker-vfs-init.test.js
├── util
│   ├── async-utils.js
│   └── logger.js
└── worker.js


```

# 📝 2025/11/30

## [GitHub - pome-ta/challengeTestWebWorkerLSP at ea6b63f81bb43a7abf1869b1072132c5bf100ca0](https://github.com/pome-ta/challengeTestWebWorkerLSP/tree/ea6b63f81bb43a7abf1869b1072132c5bf100ca0)

ごちゃごちゃと喧嘩しだしてる（私が一方的に）ので整理。

### ゴール

`lsp/_getRawDiagnostics` を不要とした時にどこまで戻すか？
また、どのような状態となるか。


### 現状


#### ChatGPT の回答

[ChatGPT - LSP/VFS プロジェクト支援](https://chatgpt.com/s/t_692bf117e6c88191abf4528c7b6ac588)

> 今回の `diag-utils.unit.test.js`（あなたが貼ったテスト）は、あなた自身のこれまでの方針と矛盾している。

> この `diag-utils.unit.test.js` は破棄または縮小すべき

> ## 7. 正しい最終的な形
> ### `flattenDiagnosticMessage()`
> → 純正 TS の `ts.flattenDiagnosticMessageText` を使うだけ
> → 独自仕様は一切なし
> → maxDepth なし
> → cycle detection なし
> → relatedInformation を message に埋め込まない
> ### `mapTsDiagnosticToLsp()`
> → LSP 標準準拠
> → relatedInformation は “別フィールド” に入れる（統合しない）
> ### テスト
> → TS の標準 flatten の動作をそのまま確認する最小限のテストにする
> → 独自仕様の再現テストは削除する


### 実装状態

- 確認（学習）用と、本実装のコードが混ざっている
- `diag-utils.unit.test.js` は不要ぽい
    - となると、`core/diag-utils.js` も不要
- 私自身が、なにがどのように連携しているか理解が乏しい
    - 変更時の、提案の代用案をどこに差し込むか不明


## 方針

- 実際にコードみながら、現在の実装を把握
- `chai` どうしようかな
    - [https://esm.sh/chai@5.1.1](https://esm.sh/chai@5.1.1)
    - 

### 流れ

1. `./test-runner.html`
2. `./js/test/test-runner.js`
3. `./js/test/v0.0.2/worker-lsp-multi-file.test.js` （今回は）
    1. `./js/test/v0.0.2/test-utils.js`
4. `./js/worker.js`
    1. `./js/core/error-codes.js`
    2. `./js/util/logger.js`
5. `./js/core/vfs-core.js`
    1. `import * as vfs from 'https://esm.sh/@typescript/vfs'`
    2. `import ts from 'https://esm.sh/typescript'`
    3. `./js/util/logger.js`
    4. `./js/util/async-utils.js`
6. `./js/core/lsp-core.js`
    1. `import ts from 'https://esm.sh/typescript'`
    2. `./js/core/vfs-core.js`


### とりまメモ

コード内`todo: ` として、追記記載もあり。

- `util/logger.js` で、`isDebugEnabled` しちゃっていいのか？
    - worker のログ送信なんて、テスト環境以外不要では？
    - でも`./js/worker.js` で`postLog` する場面あるのか
    - 

- log の表記
    - `[17:28:38.572 | WorkerLog] [Worker]`
        - と、`Worker` 表記多いな。。。
        - できれば、なんの処理してるかわかるといいのだけど
            - 第2引数に、文字列投げ込むか？
            - `[Worker]` この表記を、文字列に反映させるとか？
            - ああでも、ログの並び上、文字列の影響でデコポコするのは読み辛いかな。。。？


## VSCode のコード散策のメモ

- `F12` (`cmd + クリック`)
    - 定義ジャンプ



# 📝 2025/11/29

ロードマップとして、以下の導入はどの辺りとなるか確認したい。
提示したものを即実装したい。という希望ではない。

全体の流れとして、どこに位置するのか確認したいだけである。

- `vscode-jsonrpc`

  - npm
    - [vscode-jsonrpc - npm](https://www.npmjs.com/package/vscode-jsonrpc)
  - GitHub
    - [vscode-languageserver-node/jsonrpc at main · microsoft/vscode-languageserver-node · GitHub](https://github.com/microsoft/vscode-languageserver-node/tree/main/jsonrpc)
  - 「あなたの」過去の発言メモ
    - JSON-RPC 通信は vscode-jsonrpc/browser に委譲する設計
    - `MessageReader` / `MessageWriter` をブラウザ API で再実装

- `@typescript/ata`

  - npm
    - [@typescript/ata - npm](https://www.npmjs.com/package/@typescript/ata)
  - GitHub
    - [TypeScript-Website/packages/ata at v2 · microsoft/TypeScript-Website · GitHub](https://github.com/microsoft/TypeScript-Website/tree/v2/packages/ata)
  - 「あなたの」過去の発言メモ
    - 自動型取得(Automatic Type Acquisition) `import "p5"` のような外部型定義を自動 DL。通信環境が必要
    - ネットワーク経由で動的に取得して、仮想環境に取り込む機構
    - ATA は npm レジストリ(または jsDelivr/unpkg/CDN)にアクセスして型定義を探す

- `vscode-languageserver-protocol`
  - npm
    - [vscode-languageserver-protocol - npm](https://www.npmjs.com/package/vscode-languageserver-protocol)
  - GitHub
    - [vscode-languageserver-node/protocol at main · microsoft/vscode-languageserver-node · GitHub](https://github.com/microsoft/vscode-languageserver-node/tree/main/protocol)
  - 「あなたの」過去の発言メモ
    - LSP メッセージ定義は vscode-languageserver-protocol に委譲

# 📝 2025/11/25

`worker-lsp-multi-file.test` が v0.0.1.3 でも、v0.0.2.6 でも沼ったのでメモ

# worker-lsp-multi-file.test

## 技術報告書（詳細解析版）

本書は、`worker-lsp-multi-file.test` における  
**多ファイル対応 / import 解決 / VFS 再構築 / LSP initialize**  
の一連の動作が、今回のテストでどのように実行され、どのような挙動を示し、なぜ成功したのかを技術的に総括した報告書である。

対象範囲は以下：

- WebWorker 内 LSP サーバ（`LspServer`）
- VFS コア（`VfsCore`）
- TypeScript Program の生成・再生成
- 多ファイル（file1.ts / file2.ts）取り扱い
- import 解決
- LSP textDocument/didOpen
- sanitizeCompilerOptions の影響
- 結果のログの精読解析

---

## 1. テスト目的

本テストは以下を検証するために設計されている：

1. **複数の TS ファイルを didOpen で順に開いた際、VFS が正しく全ファイルを保持すること**
2. **createEnvironment が複数ファイルを roots として正確に Program を生成できること**
3. **import 解決が行われ、sourceFile が null にならないこと**
4. **Semantic/Syntactic diagnostics が正常取得されること**
5. **全体が race condition を起こさず安定動作すること**
6. **sanitizeCompilerOptions によって、TS エラー TS5096/TS5098 のような node 用オプションが適切に処理されること**

結論として、本テストは成功し、目的はすべて達成された。

---

## 2. 実行ログの詳細解析

以下は実際のログの重要部分を再掲し、各行をどのように解釈すべきか技術的に説明する。

### 2.1 Worker 起動 ～ VFS 初期化

```
Worker loaded and ready.
Received: vfs/ensureReady
VFS init attempt 1/3
defaultMap size: 79
VFS ensureReady complete
```

- Web Worker 自体が正常にロードされた
- defaultMap（TS の libs 群）が 79 ファイル読み込まれた
- VFS は完全に ready 状態  
  → この時点で LSP initialize が安全に実行可能

### 2.2 LSP initialize

```
Received: lsp/initialize
sanitizeCompilerOptions:
  - enabling noEmit (because allowImportingTsExtensions)
  - moduleResolution -> Bundler (because resolvePackageJsonExports/Imports)
createEnvironment: roots: []
VFS environment created (initial)
```

主なポイント：

- ユーザー指定の LSP/TS コンパイラオプションをブラウザ実行に合わせて sanitize
- node 用の moduleResolution を使用せず、`Bundler` に自動変換
- TS に noEmit を強制
- env は空 roots で生成（ファイル未オープン）

これは TypeScript をブラウザで扱う際に必須の措置。

### 2.3 file1.ts の didOpen

```
didOpen /file1.ts (version:1)
createEnvironment: injected initial file: /file1.ts
createEnvironment: about to create env; roots: [/file1.ts]
VFS environment created
recreateEnv done; roots: [/file1.ts]
Publishing 0 diagnostics for /file1.ts
```

技術的要点：

- file1.ts のコンテンツが VFS に登録される
- createEnvironment が再実行され、rootFiles が `/file1.ts` 1 つで Program 構築
- diagnostics 0 → Syntax/semantic 問題なし
- import 不要のため、ここでは依存解決の評価は発生しない

### 2.4 file2.ts の didOpen（最も重要）

```
didOpen /file2.ts (version:1)
injected initial file: /file1.ts
injected initial file: /file2.ts
createEnvironment: roots: [/file1.ts, /file2.ts]
VFS environment created
recreateEnv done; roots: [/file1.ts, /file2.ts]
Publishing 0 diagnostics for /file2.ts
```

重要ポイント：

- VFS は file1.ts + file2.ts の両方を保持
- createEnvironment が **2 ファイル構成の Program を完全に再構築**
- 依存関係（import）解決が可能な状態になった
- diagnostics 0 → import 解決も正常に通っていることを意味する  
  （import に問題があればここで必ず diagnostic が発生）

ここが本テストの成功判定となる部分。

---

## 3. sanitizeCompilerOptions の技術的重要性

元々、以下の TS エラーが発生していた：

- TS5096: allowImportingTsExtensions を使うなら noEmit 必須
- TS5098: resolvePackageJson\* を使うなら moduleResolution = node16/nodenext/bundler 必須

これは node 環境前提のオプション。

ブラウザ内 TS サービスでは意味を持たないため、sanitize 処理が以下のように自動修正した。

### 変更点概要

| オプション                 | ユーザー指定 | sanitize 後                |
| -------------------------- | ------------ | -------------------------- |
| allowImportingTsExtensions | true         | noEmit = true を付与       |
| resolvePackageJsonExports  | true         | moduleResolution = bundler |
| resolvePackageJsonImports  | true         | moduleResolution = bundler |

結果、TS プログラム生成時の fatal error が消失し、正常に Program が生成できるようになった。

---

## 4. VFS / Program 再構築戦略の評価

再構築方式は以下：

- didOpen / didChange / didClose など各操作ごとに **全ファイルを再注入**
- createEnvironment を再実行して **完全に新しい Program を構築**

この設計の利点：

- race condition が一切ない
- Program の内部キャッシュが TS サービス内部と矛盾しない
- バグ発生確率が最小化される
- ブラウザ・モバイル（iPhone Safari）での動作が強固になる

これは「最速ではないが、最も安定」のアプローチとして LSP 実装では正解。

---

## 5. テスト成功の理由（技術的総括）

**本テストが成功した理由は以下の通り。**

1. `sanitizeCompilerOptions` が TS エラーを事前に正しく修正した
2. VFS が file1.ts と file2.ts の両方を整合性ある状態で保持した
3. createEnvironment が 2 ファイルを root として Program を正しく構築した
4. TypeScript の import 解決が成功し、sourceFile が null にならなかった
5. semantic/syntactic diagnostics が正しく取得され、0 件であった
6. Worker のログに race condition や二重初期化の兆候なし

以上より、**多ファイル + import 解決 + diagnostics の全フローが正常動作**したことが確認された。

---

## 6. 今後検討すべき改善点

安定性には問題ないが、将来的な最適化余地として以下を推奨：

- recreateEnv のログ重複を整理する
- createEnvironment 呼び出し回数を減らす（段階的最適化）
- 高負荷時の Program 再生成のパフォーマンス検証
- diagnostics flatten のエッジケース追加テスト

現状のコードは「安定性重視 LSP」として実運用に耐えうる品質。

---

## 7. 結論

本テストは完全に成功し、  
**multi-file LSP + import resolution をブラウザ Worker 内で安定して実現できた**  
ことが技術的に確認された。

今回の結果は、今後 CodeMirror 連携、補完、jump-to-definition、rename 等の  
より高度な LSP 機能を実装するための確固たる基礎となる。

# 📝 2025/11/22

gpt なんか、会話の記憶が継続しないというか、変なところで覚えてて、抑えておいて欲しいところが抜ける

# 📝 2025/11/19

## gpt が 5 からマイナアプデ？

なんか馬鹿になった気がする

```
# CONTEXT TRANSFER PROMPT — Paste this at the top of the NEW THREAD (exact, do not modify)

NOTE:
- このブロックはそのまま **先頭** に貼って、新しいスレッドを開始するための機械的指示セット。
- 人が読む/理解する必要はない。Assistant がこの会話の状態を正確に復元するためのコンテキストです。
- ブロックの内容を変更すると正常に継続できない可能性がある。必ずそのまま貼ること。

==== BEGIN TRANSFER ====

[METADATA]
project: LSP × CodeMirror × TypeScript VFS (browser worker)
primary_env: browser (iPhone Safari), Web Worker (ESM module), importmap/ESM, no Node/npm required
current_version_snapshot: v0.0.1.7 -> next target v0.0.2.0
test_framework: browser-based tests; chai via esm.sh; manual test-runner HTML
logging: worker -> main via postMessage structured messages (type/log fields)
repo_notes: existing tests under test/, worker at js/worker.js, test-utils.createTestWorker available

[HIGH-LEVEL POLICIES]
- Use DOM-based JavaScript examples (no Node-specific code) unless explicitly asked otherwise.
- Use const for variables that do not change. Use let only when reassignment is required.
- Always use template literals ${} for string composition. Do not use + for string concatenation.
- Comments in code examples should annotate only added/changed lines; avoid verbose commentary.
- Tone: factual, concise, no evaluative praise. Explain omissions (e.g., why self. omitted).
- Use self. explicitly in worker context where semantically appropriate.
- When presenting code to the user later, preserve file/version headers exactly.

[VERSIONING RULES]
- Version format: vMAJOR.MINOR.PATCH.BUILD (four segments), e.g., v0.0.1.7.
- When saving changes, update file header comment to new version exactly.
- Tests are versioned similarly; include version header in test files.

[TEST / TDD RULES]
- Follow TDD approach. Tests reside under test/ (or js/test/ per repo).
- test-runner.html loads test/test-runner.js which imports individual test modules.
- Tests must output to console AND append results to DOM ordered list with id "testOrdered".
- Use createTestWorker(path) from test-utils.js to create workers; it relays worker logs to console.
- Each test handles its own timeout; include expected behavior and expected output comments when describing tests.

[WORKER <-> MAIN COMMUNICATION]
- Use structured postMessage objects in worker -> main and main -> worker:
  - Worker log: { type: 'log', message: '…' }
  - Ready: { type: 'ready' }
  - Response: { type: 'response', message: <payload> }
  - Error: { type: 'error', message: <string> }
- Tests and harness should listen for messages by checking event.data.type and handle accordingly.
- createTestWorker attaches a handler that prints type:'log' messages with a timestamp.

[VFS / @typescript/vfs USAGE]
- Use esm.sh packages in worker context:
  - import * as vfs from 'https://esm.sh/@typescript/vfs';
  - import ts from 'https://esm.sh/typescript';
- Provide robust VFS initialization helper:
  - safeCreateDefaultMap(retryCount = 3, perAttemptTimeoutMs = 5000)
  - Distinguish network errors (give up) vs timeouts (retry).
- After defaultMap creation:
  - const system = vfs.createSystem(defaultMap);
  - const env = vfs.createVirtualTypeScriptEnvironment(system, [], ts, compilerOptions);
- Typical compilerOptions used in tests:
  - target: ts.ScriptTarget.ES2022
  - moduleResolution: ts.ModuleResolutionKind.Bundler
  - allowArbitraryExtensions: true
  - allowJs: true
  - checkJs: true
  - strict: true
  - noUnusedLocals: true
  - noUnusedParameters: true
- Use env.createFile(path, text), env.updateFile(path, text), env.deleteFile(path), env.languageService.getSemanticDiagnostics(path) for tests.

[FILE LAYOUT / SPLIT STRATEGY (v0.0.2.0 plan)]
Short-term (v0.0.2.0) sketch:
src/
  worker.js                // entry: message dispatch only
  core/
    vfs-core.js            // VFS init + file ops (create/update/delete, diag helpers)
    lsp-core.js            // LSP handlers (initialize, didOpen, publishDiagnostics, ping, shutdown)
  transport/
    worker-transport.js    // Worker <-> main transport adapter (string in/out for JSON-RPC)
  client/
    worker-client.js       // WorkerClient wrapper used by main (send/notify helpers)
test/
  worker-*.test.js         // tests interacting with worker via createTestWorker

- worker.js should be thin: import cores and dispatch incoming messages to appropriate handlers.
- Keep core modules as plain functions/objects first; refactor to classes only when necessary.

[LSP MINIMAL SET FOR v0.0.2.0]
Minimum features to add:
- initialize (await safe VFS boot)
- initialized (notification)
- textDocument/didOpen (create file in env and schedule diagnostics)
- textDocument/publishDiagnostics (send diagnostics via transport)
Mapping: TypeScript diagnostics -> LSP diagnostics (severity map) before publishing.

[JSON-RPC / MESSAGE FORMAT]
- WorkerClientImpl uses JSON-RPC style for requests/responses:
  - Request: { jsonrpc: '2.0', id: <num>, method: 'initialize', params: {...} } (sent as JSON string)
  - Response: { jsonrpc: '2.0', id: <num>, result: ... } or error variant
  - Notification: { jsonrpc: '2.0', method: 'initialized', params: {...} }
- Worker transport accepts both string and object; CodeMirror LSPClient expects stringified JSON messages.

[LOGGING]
- Worker debug logs only when DEBUG flag true.
- Use structured log messages posted to main: { type: 'log', message: '...' } so test harness can capture and timestamp.
- Avoid sending very large objects via postMessage; prefer small structured summaries.

[TEST NAMING & PATTERNS]
- Descriptive tests:
  - worker-vfs-file.test.js
  - worker-vfs-multi-file.test.js
  - worker-vfs-delete.test.js
  - worker-vfs-missing-import.test.js
  - worker-vfs-circular-import.test.js
  - worker-vfs-update-recheck.test.js
- Test pattern:
  1. const worker = createTestWorker('./js/worker.js');
  2. worker.postMessage('<test-command>');
  3. await event.data.type === 'response' with appropriate timeout
  4. assert expectations via chai; append result to DOM <ol id="testOrdered">
- Increase timeouts for VFS init tests (10–15s) as CDN fetch/time can be slow.

[DEVELOPMENT RULES / DOs and DONTs]
- DO write small testable helper functions.
- DO write tests reflecting production behavior; avoid writing code solely to satisfy tests.
- DO maintain worker code runnable in browser worker context (use self.postMessage, self.addEventListener).
- DO not rely on Node APIs or filesystem.
- DO prefer minimal dependencies; use esm.sh imports in the worker.
- DON'T log raw huge ASTs or dumps to main.

[ON STARTING THE NEW THREAD]
- Immediately after loading this block, the Assistant should respond with:
  CONTEXT RESTORED: v0.0.1.7 snapshot loaded. Ready to continue as v0.0.2.0 planning/advice.
- Then wait for the user's instruction. Treat the user messages as design-decisions until they say "確定、実装する" which triggers actual code changes.

==== END TRANSFER ====

END OF BLOCK
```

# 📝 2025/11/14

リポジトリを戻そうと四苦八苦ちう

# 📝 2025/11/13

free で ChatGPT を使ってる。使用制限調整のために過去のやりとりは、遡る必要がある。

めちゃクソ長くなって、検索性も悪い。

自分の投稿のインデックスとかないんかな？

## 新規スレッドに移す

まるっと、新しく既存のやりとりを継承したかった。添付ファイル（画像）があると、取り回し面倒ぽいので。

まぁ、結局は添付ファイルのはいみなかったけど

## `FULL CONTEXT RESYNC (for ChatGPT internal use)`

`FULL CONTEXT RESYNC (for ChatGPT internal use)` こんなのがあるらしい。

```
あなただけが、理解できればいいから
```

としたら、出してきた。

## メモ落とし

### 時間差リクエスト取得

```
🚀 main.js loaded
🧩 worker-vfs-update-recheck.test.js loaded
🚀 test-runner.js loaded
[09:53:20.476|WorkerLog] 💻 vfs-update-recheck-test start
[09:53:20.497|WorkerLog] 🔄 VFS init attempt 1/3
[09:53:24.187|WorkerLog] 📦 defaultMap size: 79
[09:53:24.237|WorkerLog] 🧠 env created
[09:53:24.241|WorkerLog] 📝 created /main.ts with valid code
[09:53:24.572|WorkerLog] 🔍 diagnostics before update: 0
[09:53:24.573|WorkerLog] ✏️ updated /main.ts (type mismatch)
[09:53:24.582|WorkerLog] 🔍 diagnostics after update: 1
[09:53:24.582|WorkerLog] 📤 vfs-update-recheck-test response sent
❌ Worker vfs-update-recheck-test failed: No response
```

```
🚀 main.js loaded
🧩 worker-vfs-update-recheck.test.js loaded
🚀 test-runner.js loaded
[09:56:42.099|WorkerLog] 💻 vfs-update-recheck-test start
[09:56:42.112|WorkerLog] 🔄 VFS init attempt 1/3
❌ Worker vfs-update-recheck-test failed: No response
[09:56:47.099|WorkerLog] ⏰ Timeout, retrying...
[09:56:48.104|WorkerLog] 🔄 VFS init attempt 2/3
[09:56:53.104|WorkerLog] ⏰ Timeout, retrying...
[09:56:55.110|WorkerLog] 🔄 VFS init attempt 3/3
[09:56:59.649|WorkerLog] 📦 defaultMap size: 79
[09:56:59.699|WorkerLog] 🧠 env created
[09:56:59.704|WorkerLog] 📝 created /main.ts with valid code
[09:57:00.061|WorkerLog] 🔍 diagnostics before update: 0
[09:57:00.063|WorkerLog] ✏️ updated /main.ts (type mismatch)
[09:57:00.089|WorkerLog] 🔍 diagnostics after update: 1
[09:57:00.090|WorkerLog] 📤 vfs-update-recheck-test response sent
```

意図的に出せて、テストできるといいな。

# 📝 2025/11/12

## v0.0.2.0 へ向けて

方針を固めた

# v0.0.2.0 実装方針書

## 概要

v0.0.1 系で行ってきた Virtual File System（VFS）中心のテストフェーズを一区切りとし、  
v0.0.2.0 では **LSP 構造導入の準備** として「VFS と LSP の責務分離」を行う。  
この段階ではまだ LSP 通信は最小限（initialize / shutdown / ping）に留め、  
将来的な拡張に耐えられるモジュール構成へ移行する。

---

## 構成スケッチ

### 🥚 v0.0.2.x（現段階）

**目的：VFS と LSP の責務を分離し、worker.js を単純化する。**

構成案：

- src/
  - worker.js … メッセージループ / JSON-RPC エントリポイント
  - core/
    - vfs-core.js … Virtual FileSystem の操作 API 群
    - lsp-core.js … LSP の initialize / shutdown / ping など Lifecycle 系
  - util/
    - logger.js … postLog / formatTimestamp など共通ロガー
  - tests/
    - worker-vfs-file.test.js
    - worker-vfs-delete.test.js
    - worker-vfs-circular-import.test.js
    - ...

ポイント：

- `core/` に VFS と LSP の実装を分離
- `worker.js` は dispatcher 的な役割のみにする
- すべての通信は `type` または `method` に基づいてルーティング
- VFS テストは継続利用可能

---

### 🐣 v0.0.3.x（LSP 構造導入期）

**目的：LSP 基本通信を整備し、LSPWorker 化を進める。**

構成案：

- src/
  - worker.js … LSPWorker の bootstrap のみ
  - worker-core/
    - lsp-server-core.js … LspServerCore クラス：LSP メソッド実装
    - rpc-handler.js … JSON-RPC のループ処理 (request / notify)
    - vfs-adapter.js … VFSCore を TextDocument 管理層として包む
  - core/
    - vfs-core.js
    - lsp-core.js
  - util/
    - logger.js
    - rpc-utils.js
  - tests/
    - worker-lsp-init.test.js
    - worker-lsp-didopen.test.js
    - ...

---

### 🐥 v0.0.4.x（外部連携期）

**目的：CodeMirror LSP クライアント・@typescript/ata 統合の準備。**

構成案：

- src/
  - main.js … CodeMirror 側エントリ
  - worker/
    - index.js … LSP Worker entry
    - lsp-server-core.js
    - vfs-adapter.js
    - rpc-handler.js
  - client/
    - worker-client.js … createWorkerRpc() など
    - lsp-adapter.js … codemirror/lsp-client との橋渡し
    - ata-loader.js … @typescript/ata 統合
  - core/
    - vfs-core.js
    - lsp-core.js
    - rpc-utils.js
  - util/
    - logger.js
    - env.js
  - tests/
    - ...

---

### 🐉 v1.0.0（最終構成）

**目的：完全な LSP 構造と CodeMirror 統合を確立。**

構成案：

- src/
  - main.js … UI / CodeMirror entry
  - client/
    - worker-client.js
    - lsp-adapter.js
    - ata-loader.js
    - config.js
  - worker/
    - index.js
    - lsp-server-core.js
    - lsp-handlers/
      - lifecycle.js
      - textdocument.js
      - diagnostics.js
    - rpc/
      - rpc-handler.js
      - rpc-utils.js
    - vfs/
      - vfs-core.js
      - vfs-utils.js
  - util/
    - logger.js
    - env.js
  - tests/
    - vfs/
    - lsp/
    - integration/

---

## まとめ

- v0.0.2.x では **VFS / LSP / Worker の 3 層を整理**
- v0.0.3.x 以降で LSPWorker を中心に据え、JSON-RPC と TextDocument 管理を導入
- v0.0.4.x 以降で `@codemirror/lsp-client` および `@typescript/ata` を連携
- 将来的に v1.0.0 構成で完全な LSP サーバワーカーモジュールへ到達する

---

# 📝 2025/11/08

## 区切りなので

現在の状況をまとめさせてみた。

# Web Worker × LSP テスト運用まとめ（v0.0.0.6）

## 概要

本ドキュメントでは、**ブラウザ上で TypeScript LSP を動かすための Worker テスト基盤**について、  
`v0.0.0.6` までの開発内容を整理する。

---

## 1. テスト運用の方向性

目的は「Worker 内で LSP を動かし、その初期化や通信を安全にテストする」こと。

### 方針

- 各機能を独立した test ファイルで管理  
  (`worker-init.test.js`, `worker-ping.test.js`, `worker-shutdown.test.js`, `worker-vfs-init.test.js`)
- ブラウザ上で即時実行＋結果を `<ol>` に表示
- タイムアウト判定を採用し、Safari 特有の GC 問題も回避

---

## 2. 現時点の構成

```
/js/
  └─ worker.js              # LSP Worker 本体
/test/
  ├─ worker-init.test.js
  ├─ worker-ping.test.js
  ├─ worker-shutdown.test.js
  ├─ worker-vfs-init.test.js
  └─ test-utils.js
```

| ファイル        | 役割                                          |
| --------------- | --------------------------------------------- |
| `worker.js`     | Worker ロジック（VFS 初期化、ping、shutdown） |
| `test-utils.js` | Worker 作成・ログ整形                         |
| `*.test.js`     | 各機能の独立テスト                            |

---

## 3. 各コードと工夫点

### `worker.js`

```js
// worker.js
// v0.0.0.6

import * as vfs from 'https://esm.sh/@typescript/vfs';
import ts from 'https://esm.sh/typescript';

const DEBUG = true;

const postLog = (message) => {
  DEBUG && self.postMessage({ type: 'log', message });
};
const sleep = (ms) => new Promise((r) => setTimeout(r, ms));

postLog('👷 worker.js loaded');

async function safeCreateDefaultMap(retryCount = 3) {
  const timeoutMs = 5000;
  let lastError = null;

  for (let i = 0; i < retryCount; i++) {
    postLog(`🔄 VFS init attempt ${i + 1}/${retryCount}`);

    try {
      const timeout = new Promise((_, reject) =>
        setTimeout(() => reject(new Error('timeout')), timeoutMs)
      );

      const defaultMap = await Promise.race([
        vfs.createDefaultMapFromCDN(
          {
            target: ts.ScriptTarget.ES2022,
            module: ts.ModuleKind.ESNext,
          },
          ts.version,
          false,
          ts
        ),
        timeout,
      ]);

      postLog(`📦 defaultMap size: ${defaultMap.size}`);
      return defaultMap;
    } catch (error) {
      lastError = error;
      if (
        error.message.includes('fetch') ||
        error.message.includes('NetworkError')
      ) {
        postLog(`🚫 Network error: ${error.message}`);
        throw error;
      } else if (error.message.includes('timeout')) {
        postLog(`⏰ Timeout, retrying...`);
        await sleep(1000 * (i + 1));
        continue;
      } else {
        postLog(`❌ Unknown error: ${error.message}`);
        throw error;
      }
    }
  }

  throw lastError || new Error('VFS init failed after retries');
}

self.addEventListener('message', async (event) => {
  const { data } = event;

  if (data === 'vfs-init') {
    postLog('💻 vfs-init start');

    try {
      const defaultMap = await safeCreateDefaultMap(3);
      setTimeout(() => {
        try {
          self.postMessage({ type: 'response', message: 'return' });
          postLog('📤 vfs-init response sent (delayed)');
        } catch (e) {
          postLog(`⚠️ vfs-init postMessage failed: ${e.message}`);
        }
      }, 50);
    } catch (error) {
      postLog(`❌ vfs-init error: ${error.message}`);
      self.postMessage({ type: 'error', message: error.message });
    }
  }

  if (data === 'ping') {
    postLog('📡 Received: ping');
    self.postMessage({ type: 'response', message: 'pong' });
  }

  if (data === 'shutdown') {
    postLog('👋 Worker shutting down...');
    self.postMessage({ type: 'response', message: 'shutdown-complete' });
    setTimeout(() => self.close(), 100);
  }
});

self.postMessage({ type: 'ready' });
```

#### 主な工夫点

- **`safeCreateDefaultMap()` による堅牢化**
  - 5 秒タイムアウト＋最大 3 回リトライ。
  - ネットワークエラーと処理遅延を明確に分岐。
- **Safari 向け安全対策**
  - `setTimeout(50)` で postMessage の競合を回避。
- **ログ構造の一元化**
  - すべての重要処理が `[WorkerLog]` に統一されて出力。

---

### `worker-vfs-init.test.js`

```js
// test/worker-vfs-init.test.js
// v0.0.0.6

import { expect } from 'chai';
import { createTestWorker } from './test-utils.js';

console.log('🧩 worker-vfs-init.test.js loaded');

const orderedList = document.getElementById('testOrdered');
const liItem = document.createElement('li');

let textContent;

(async () => {
  try {
    const worker = createTestWorker('./js/worker.js');

    worker.postMessage('vfs-init');

    const message = await new Promise((resolve, reject) => {
      const timer = setTimeout(
        () => reject(new Error('No vfs-init response')),
        13000
      );
      worker.addEventListener('message', (event) => {
        const { type, message } = event.data;
        if (type === 'response') {
          clearTimeout(timer);
          resolve(message);
        }
      });
    });

    expect(message).to.equal('return');
    textContent = '✅ Worker vfs-init test passed';
    console.log(textContent);
  } catch (error) {
    textContent = `❌ Worker vfs-init test failed: ${error.message}`;
    console.error(`❌ Worker vfs-init test failed: ${error}`);
  }

  liItem.textContent = textContent;
  orderedList.appendChild(liItem);
})();
```

- タイムアウトを 13 秒に設定し、遅延 CDN や再試行にも耐える。
- 成功／失敗をブラウザ DOM に即時反映。

---

### `test-utils.js`

```js
// test/test-utils.js

export const createTestWorker = (path) => {
  const worker = new Worker(path, { type: 'module' });

  worker.addEventListener('message', (event) => {
    const { data } = event;
    data?.type === 'log' &&
      console.log(
        `[${new Date().toLocaleTimeString('ja-JP', {
          hour12: false,
          hour: '2-digit',
          minute: '2-digit',
          second: '2-digit',
          fractionalSecondDigits: 3,
        })}|WorkerLog] ${data.message}`
      );
  });

  return worker;
};
```

- `WorkerLog` に時刻付きログを出力。
- 実行時の遅延・再試行間隔を可視化可能。

---

## 4. 今後の展望と課題

| フェーズ               | 目的                         | 想定課題                   | 対策                                             |
| ---------------------- | ---------------------------- | -------------------------- | ------------------------------------------------ |
| **LSP lifecycle 実装** | initialize / shutdown / exit | Worker 同期                | `__ready` による同期管理                         |
| **VFS → ATA 切替**     | 型取得のリアルタイム化       | 通信遅延・ネットワーク負荷 | retry ＋ timeout 制御の流用                      |
| **CodeMirror 連携**    | 実際のエディタ統合           | 高頻度通信                 | RPC 分離（`worker-client.js` / `worker-rpc.js`） |
| **堅牢化**             | iOS Safari 特有問題          | postMessage タイミング     | setTimeout(50) 遅延維持                          |

---

## 5. まとめ

v0.0.0.6 では以下を達成した：

1. **VFS 初期化の安定化（retry + timeout）**
2. **Safari 安全対策の導入**
3. **テスト環境の整備と即時可視化**

今後はこの安定基盤をもとに、  
CodeMirror との LSP 連携、`@typescript/ata` 導入、RPC 分離へと発展させる。

# 📝 2025/11/01

```
CodeMirror
  ⇅ (codemirror/lsp-client)
WorkerClient (自前実装)
  ⇅ (vscode-languageserver-protocol + vscode-jsonrpc)
LspServerCore (自前実装)
  ⇅ (typescript + @typescript/vfs + @typescript/ata)
TypeScript LanguageService
```

## GPT と Gemini Code Assist の連携

気軽にコピペする運用。gpt との取り決めを VSCode 上でも齟齬なく行うために。

### 最初の指示用のテンプレ

```markdown
# Project Design Guide

この文書は、Gemini Code Assist および他の開発支援 AI に対し、
本プロジェクトの設計方針・コーディングルール・構成方針を伝えるためのメモである。

---

## 1. 基本方針

- コードはできる限り **既存標準（LSP 仕様、CodeMirror 仕様など）に準拠**する
- **自前実装を最小限にし、メンテナンス性を優先**する
- コードの動作説明コメントは「変更・追加した部分のみ」に記述する
- ロジック上の同期・初期化は race condition を避け、`__ready` などのシグナルで同期を取る

---

## 2. コーディング規約

- 変数は変更されない限り `const` を使う
- ファイル内での順序は「クラス定義 → エクスポート → 補助関数」
- 関数名・メソッド名は動詞始まり（例: `createWorkerRpc`, `handleRequest`）
- コメントは箇条書き形式で簡潔に記述する（文末句点不要）
- LSP 通信（JSON-RPC）は仕様に忠実に。独自プロトコルは避ける

---

## 3. ファイル構成方針

|           ファイル | 役割                                                                                    |
| -----------------: | --------------------------------------------------------------------------------------- |
| `worker-client.js` | メインスレッド側。Worker との JSON-RPC 通信を行う                                       |
|        `worker.js` | LSP サーバロジックを実装。`LspServerCore` と `LSPWorker` を含む                         |
|          `main.js` | Editor 初期化および LSP の起動シーケンス（`initialize → initialized → shutdown`）を管理 |

将来的に、`worker-client.js` は `worker-client.js` と `worker-rpc.js` に分割予定

---

## 4. 実装指針

- `WorkerClient` は LSP リクエスト（`send`）と通知（`notify`）を区別して扱う
- `LSPWorker` は JSON-RPC のループを担当し、未定義メソッドにはエラーを返す
- `LspServerCore` は `initialize`, `initialized`, `ping`, `shutdown` を実装済みであること
- 追加機能はまず LSP 標準メソッドに適合する形で検討する

---

## 5. CodeMirror 連携方針

- `@codemirror/language` と `@codemirror/lsp-client` を基本利用
- LSP 側での診断結果や補完は、CodeMirror の組み込み処理に委ねる
- エディタイベントやドキュメント同期のカスタム処理は最小限にする

---

## 6. 命名・コメントルール（補足）

- ファイル内コメントは **変更箇所のみに限定**
- コメントの書式（例）
  - `// 変更: initialize() の呼び出し順を修正`
- 定数・クラス名は PascalCase、メソッド・関数名は camelCase

---

## 7. 補助メモ（AI 補完向け）

AI 補完（Gemini / ChatGPT など）でこの方針を踏まえるための指示（プロジェクト内に置く場合）:

このプロジェクトでは上記 design.md に記載された方針を前提として補完・生成を行うこと。  
構成や命名、通信仕様はこれに反しないようにすること。

---

## 8. 今後の拡張メモ（任意）

- `exit` 通知の追加（`shutdown` 後の後処理）
- ファイル監視による VFS 更新
- `textDocument/didChange` の最小差分同期
- `CodeMirror` の診断表示と LSP 結果同期

---
```

### 修正や編集テンプレとして提案されたもの

```markdown
# Edit Instruction Guide

このファイルは、ChatGPT（設計アシスタント）に対して  
`design.md` の内容を **正確に追加・修正・削除** するための指示テンプレート。

---

## ✅ 基本ルール

1. 対象範囲を明示する

   - どの章・セクション・箇条書きを触るかを明記
   - 例: 「第 3 章『ファイル構成方針』」「第 2 章の 2 つ目の箇条書き」

2. 操作種別を明記する

   - 「追加」「修正」「削除」「置換」「追記」など

3. 最終的に入れたい形で書く

   - ChatGPT に自然文を解釈させず、最終形の Markdown をそのまま示す

4. 出力形式を指定する
   - 「変更箇所のみ Markdown で出力」または「design.md 全体を再出力」など

---

## 🧩 指示テンプレート（標準形式）

# 編集指示

対象: 第 ◯ 章「章タイトル」  
操作: （追加／修正／削除／置換／追記）  
位置: （任意。例: 章の末尾、2 行目の箇条書きなど）  
内容:  
（ここに最終的に入れたい Markdown 形式の文を記載）

出力形式: （例: 修正箇所のみ Markdown で出力）

---

## 📘 指示例

### 例 1: 追記

対象: 第 2 章「コーディング規約」  
操作: 追記  
内容:

- import 文の順序は、外部ライブラリ → ローカルモジュール の順に統一する  
  出力形式: 修正箇所のみ Markdown で出力

---

### 例 2: 修正

対象: 第 1 章「基本方針」  
操作: 修正  
内容:  
「自前実装を最小限にし、メンテナンス性を優先する」  
→ 「自前実装は必要最低限にとどめ、再利用性を優先する」  
出力形式: 修正箇所のみ Markdown で出力

---

### 例 3: 削除

対象: 第 8 章「今後の拡張メモ」  
操作: 削除  
内容:

- `exit` 通知の追加（`shutdown` 後の後処理）  
  出力形式: 修正箇所のみ Markdown で出力

---

### 例 4: 全体再構成

対象: design.md 全体  
操作: 章構成を再編成  
内容:

- 「命名・コメントルール」と「コーディング規約」を統合し、第 2 章にまとめる  
  出力形式: design.md 全体を再出力

---

## ⚡ 簡易ショート形式（慣れたら）

「2 章に import 順序ルール追加して」  
「1 章の方針を“再利用性を優先”に書き換えて」  
「8 章の exit 通知削除して」

この短縮形でも処理可能。  
ただし、複数の変更を同時に行う場合は正式テンプレートを使う。

---

## 🧠 複数変更を一度に指示する場合

# 編集指示

---変更 1---  
対象: 第 2 章「コーディング規約」  
操作: 追記  
内容:

- import 文の順序は、外部ライブラリ → ローカルモジュール の順に統一する

---変更 2---  
対象: 第 5 章「CodeMirror 連携方針」  
操作: 修正  
内容:

- 「CodeMirror の組み込み処理に委ねる」を  
  「CodeMirror の LSP クライアント機能を利用して処理する」に変更

---

## ✅ 出力依頼時のおすすめ文例

- 「上記の指示に従って修正を反映して」
- 「修正後の Markdown を出力して」
- 「修正箇所だけ出力して」
- 「design.md 全体を再出力して」

---

## 🧾 運用メモ

- この `edit-instruction.md` は ChatGPT への指示テンプレートとして維持する
- `design.md` と同じディレクトリに置くと便利
- `design.md` の変更履歴を管理したい場合は Git でコミット前に差分を確認する

---
```

# 📝 2025/10/29

## 改めて LSP を理解し直す

```
CodeMirror
⇅
codemirror/lsp-client
⇅ （JSON-RPC 通信）
LSPWorker（実装中の worker.js 内）
⇅ （関数呼び出し）
ts.LanguageService
```

```
┌──────────────────────────────────┐
│ LSP プロトコル層（JSON-RPC で通信）    │  ←  initialize, completion などの受付
├──────────────────────────────────┤
│ 言語サービス層（TypeScript API など）  │  ←  実際の補完・解析・診断処理
├──────────────────────────────────┤
│ ファイルシステム層（VFSなど）           │  ←  ファイルの内容・依存解決
└──────────────────────────────────┘
```

```
───────────────────────────────────────────────
🧩 CodeMirror + codemirror/lsp-client  (共通層)
───────────────────────────────────────────────
          ↓ JSON-RPC (LSP プロトコル)
───────────────────────────────────────────────
   通信層 (Transport)
───────────────────────────────────────────────
  WebSocket版                      WebWorker版
───────────────────────────────────────────────
  WebSocket(JSON-RPC)              postMessage(JSON-RPC)
  ↳ WebSocketServer                ↳ WorkerClient
  ↳ vscode-ws-jsonrpc              ↳ MessagePort Transport
───────────────────────────────────────────────
   LSPサーバー層 (Server Core)
───────────────────────────────────────────────
  vscode-languageserver/node       LspServerCore（自作）
  ↳ LSPの各メソッド実装            ↳ initialize / didOpen / didChange …
  ↳ 診断・補完・hover など         ↳ 同様のLSPメソッドを再現
───────────────────────────────────────────────
   TypeScript LanguageService
───────────────────────────────────────────────
  typescript（Node.js）             typescript（ブラウザ）
  ↳ ファイルIOあり（fs）            ↳ @typescript/vfs 経由で仮想FS利用
───────────────────────────────────────────────
   ファイル管理層 (FS)
───────────────────────────────────────────────
  Node.js FS or ts.sys             @typescript/vfs.createSystem()
  ↳ 実際のファイル操作              ↳ メモリ上のファイルマップ
───────────────────────────────────────────────
   実行環境
───────────────────────────────────────────────
  Node.js                         Browser (Worker)
───────────────────────────────────────────────
```

[目的：Web Worker でも極力「パッケージだけ」で LSP を動かす構成 | ChatGPT - @codemirror/lsp-client](https://chatgpt.com/s/t_6901a2b7da5c81919eaaed894133f8c8)

# Web Test Runner vs Mocha（ブラウザ向け）比較

以下は **Web Test Runner**（modern-web.dev の実装）と **Mocha（ブラウザ実行）** を、実務で比較して選べるように主要な観点で対比した表。各セルは短く要点を列挙している。選定方針に合わせて最後に推奨ケースも示す。

| 比較項目                            |                                                                                          Web Test Runner | Mocha（ブラウザ実行）                                                                                                        |
| ----------------------------------- | -------------------------------------------------------------------------------------------------------: | ---------------------------------------------------------------------------------------------------------------------------- |
| 主目的 / 設計思想                   |                              ブラウザ（ES Module）でのモダンなテスト実行。E2E に近いブラウザ実行を意図。 | 汎用的なテストフレームワーク。Node／ブラウザ両対応だが、ブラウザで使うときはランタイムやバンドラ等の補助が必要。             |
| ESModule サポート                   |                                       ネイティブ ESM を前提に最適化。import ベースでそのまま読み込める。 | 元来は CommonJS 志向。ブラウザで ESM を直接使うには設定やバンドラ（または esm CDN）が必要。                                  |
| Worker（Web Worker）テスト          |                           そのまま Worker を spawn してテスト可能。Worker／window 間の統合テストが容易。 | Worker テストは可能だが、手動で Worker を生成してメッセージを待つ形。環境整備や helper が必要になる場合が多い。              |
| ブラウザ API（DOM, postMessage 等） |                                        ブラウザ上で動く実テストランナー。ブラウザ API をそのまま使える。 | ブラウザで実行すればブラウザ API を使える。ただしセットアップで HTML ラッパーが必要。                                        |
| セットアップ難易度                  |                                           比較的簡単（設定は柔軟）。ESM + テスト用プラグイン中心で済む。 | 基本は低いがブラウザ実行にすると `index.html` と読み込み順、スクリプトタグ管理が必要。ESM での運用は追加設定が発生しやすい。 |
| 拡張性 / プラグイン                 |           プラグイン設計（プラグインで Babel、TypeScript、ファイル監視、ブラウザマトリクス等追加可能）。 | 多数の周辺ライブラリ（chai, sinon 等）とは互換あり。プラグイン体系は Web Test Runner より古典的。                            |
| TypeScript 対応                     |                 公式に TypeScript サポートが良い（プラグインでトランスパイル）。ソースマップ連携が良好。 | TypeScript は `ts-node` やトランスパイル手順が必要。ブラウザで直接テストするには事前ビルドか CDN トランスパイルが必要。      |
| 並列・分散・マルチブラウザ          |           ブラウザプール／並列ラン実行に対応するプラグインがあり、マルチブラウザ・クラウド連携しやすい。 | 並列実行は自前やラッパー（karma 等）に依存することが多い。                                                                   |
| CI 適性（ヘッドレス）               |                       ヘッドレスブラウザを使った CI 実行に対応（Playwright や Puppeteer と組み合わせ）。 | ヘッドレス実行は可能だが、セットアップ（ヘッドレスブラウザの起動）がやや手動。Karma 等と組むと良い。                         |
| デバッグ体験                        |                               ブラウザの DevTools でそのままデバッグ。ソースマップでステップ実行が自然。 | 同様に DevTools でデバッグ可能だが、ブラウザ向けに整えるための手間（HTML/スクリプトラップ等）がある場合がある。              |
| レポーター/出力                     |                                               多数のレポーターやカスタム出力機能をプラグインで追加可能。 | 大量の既存レポーター（spec, nyan 等）あり。CI 向け junit 等も豊富。                                                          |
| エコシステム安定度                  |                比較的新しく、モダンなワークフローに最適化。活発だが Mocha に比べ成熟度はやや劣る点あり。 | 非常に成熟・広く使われている。プラグイン・ドキュメント・事例が豊富。                                                         |
| 依存関係 / バンドリング             |           ESM ネイティブ運用前提で依存解決がシンプル。CDN 読み込みやローカルファイル直接読み込みが自然。 | 依存ライブラリ次第でバンドラ（Rollup, Webpack）や CDN 経由が必要になることが多い。                                           |
| 学習コスト                          |                                             テスト概念は既知なら低い。ESM やプラグイン利用の理解が必要。 | テスト API 自体は簡単。ブラウザ実行の細かな設定で時間を取ることがある。                                                      |
| 長所まとめ                          | モダンなブラウザネイティブテスト、Worker/DOM の統合テストが容易、TypeScript やソースマップサポート良好。 | 汎用性・互換性が高く成熟。大量の既存資産（例：chai, sinon, レポーター）が使える。                                            |
| 短所まとめ                          |                                                     比較的新しいため既存プラグインに乏しいケースがある。 | ブラウザ向けに最適化されていない部分があり、ESM/Worker 中心に使うと追加設定が必要。                                          |

---

## 追加の実用メモ（選定の補助）

- **ES Modules と WebWorker を多用するブラウザネイティブ実装**（あなたのケース） → **Web Test Runner が優位**。Worker をそのまま spawn でき、import ベースのソースを直接テストできるため手間が少ない。
- **既に Mocha ベースの大量テスト資産がある／Node とブラウザ両方で同じランナーを使いたい** → **Mocha**。ただしブラウザ実行は `index.html` テンプレートやバンドル準備が必要になる場合がある。
- **CI での安定・広範なレポートが必要** → Mocha の既存エコシステム（ジャッジ用レポーター等）が便利。ただし Web Test Runner も Playwright 等と組めば十分対応可能。

---

## 推奨（あなたの状況を踏まえて）

- あなたは「ブラウザ」「Web Worker」「ESM（CDN 経由も含む）」を積極的に使っているため、**まずは Web Test Runner を試作環境に導入**することを勧める。  
  理由：Worker のテストが自然に書け、TypeScript（トランスパイル）やソースマップ連携も楽で、テストのデバッグがブラウザ DevTools で直感的に行えるため、開発速度とデバッグ効率が上がる。
- ただし既に Mocha のテスト資産が多数ある場合は、**Mocha を選んで移行コストを減らす**のが合理的。

---

もっと具体的な手順が欲しい場合（例：Web Test Runner を使って `worker.js` の ping/pong テストを書く、あるいは Mocha で同等のテストを作る）は、どちらを先に試したいか指定してくれれば、すぐに実際のテストコード（index.html / テストファイル / 実行手順）を用意する。

| 観点                   | `<script src="https://cdn.jsdelivr.net/...">` 形式                  | `import eruda from 'eruda'` + importmap 形式            |
| ---------------------- | ------------------------------------------------------------------- | ------------------------------------------------------- |
| **読み込みタイミング** | HTML 解析時に即ロードされ、グローバル (`window.eruda`) に登録される | モジュールスクリプト内で `import` 時にロードされる      |
| **スコープ**           | グローバルスコープ（どのスクリプトからもアクセス可能）              | モジュールスコープ（`import` したモジュール内のみ）     |
| **依存管理**           | CDN URL を直書きするため、依存関係を importmap で管理できない       | importmap で一元管理でき、依存バージョンを固定しやすい  |
| **ブラウザ対応**       | すべてのブラウザ（古い Safari 含む）で動作                          | ES Modules 対応ブラウザ限定（iOS 12 以降）              |
| **初期化タイミング**   | `eruda.init()` を `<body>` 末尾で呼べば即可                         | `import` 完了後に明示的に `eruda.init()` を呼ぶ必要あり |
| **キャッシュ**         | CDN が最適化済みで高速キャッシュあり                                | esm.sh が動的にビルドするため初回ロードはやや遅い       |
| **利便性**             | 最短で動かせる（スニペット 1 行で OK）                              | よりモジュラーで、他の import と統一的に扱える          |
| **使用例**             | デバッグ専用・すぐ試したいとき                                      | プロジェクト内で統一したモジュール構成を保ちたいとき    |
| **推奨用途**           | 手動検証・実験・PoC                                                 | 本番に近い構成（importmap ベースの TDD 環境）           |

# 📝 2025/10/27

## `@types/p5` を入れてみる

```js
// 例: worker.js / LspServerCore 内
async #bootVfs() {
  if (this.#env) return;

  const fsMap = await createDefaultMapFromCDN({
    target: ts.ScriptTarget.ES2022,
    lib: ['es2022', 'dom'],
  }, ts);

  // 独自の型定義を追加
  const customDts = `
    declare global {
      interface Window {
        myCustomValue: string;
      }
      function myGlobalFunction(msg: string): void;
    }
  `;
  fsMap.set('/custom-globals.d.ts', customDts);

  const system = createSystem(fsMap);
  const host = createVirtualLanguageServiceHost(system, fsMap, ts, '/index.ts');
  const service = ts.createLanguageService(host);

  this.#env = { fsMap, system, host, languageService: service };
}
```

```js
// worker.js 内
import { createDefaultMapFromCDN } from '@typescript/vfs';

async function loadExtraDTS(vfs) {
  const url = 'https://esm.sh/@types/p5/index.d.ts';
  const res = await fetch(url);
  if (!res.ok) throw new Error(`Failed to load ${url}`);
  const code = await res.text();

  // 仮想ファイルとして追加
  vfs.set('/node_modules/@types/p5/index.d.ts', code);
}
```

```js
//実際の仕様
import { createDefaultMapFromCDN } from '@typescript/vfs';

// この関数は TypeScript チームが管理している CDN から
// lib.es2022.d.ts や lib.dom.d.ts などを自動取得する
const defaultMap = await createDefaultMapFromCDN({
  target: ts.ScriptTarget.ES2022,
  module: ts.ModuleKind.ESNext,
  lib: ['es2022', 'dom'],
});
```

# 📝 2025/10/20

## ChatGPT やり取りログ

やり取りが多くて、過去の確認したい所に移動できないのでメモ。

[GitHub - pome-ta/challengeTestWebWorkerLSP](https://github.com/pome-ta/challengeTestWebWorkerLSP)

この実装。

### `LSP Coverage Map`

> `LSP Coverage Map`

スコープが大きい、実装状況。

- `$/cancelRequest` これは、不要なのかしら？
- `Capability Negotiation (ServerCapabilities)` これはなんだなんだ？
-

# 📝2025/10/19

## codemirror と LSP

#codemirror #lsp

AI に聞きながら、ゆっくりと実験中。

[GitHub - pome-ta/challengeTestWebWorkerLSP](https://github.com/pome-ta/challengeTestWebWorkerLSP)

（あとで AI に頼るとして）

- サーバーを立てずに LSP を使う
- Worker で使うこともできる
- `@typescript/vfs` と、`typescript` でいい感じにできる
- これをサーバーとして、通信する
- [GitHub - codemirror/lsp-client: Language server protocol client for CodeMirror](https://github.com/codemirror/lsp-client)
- クライアントは、これを窓口にするのを必須とする
- 裏側をコネコネと実装していく

# 📝2025/10/17

## AI との付き合い

#ai #google

### モバイル

chatGPT の free でなんとかやってる

### PC

VSCode で Gemini Code Assist 使ってみてる

いまは、プロンプトに質問投げるかたちの、いつもの使い方をしている。
なんだかんだ、chatGPT （free）より、質問できる回数は多いかも。

実質無制限の使い方を調べていきたい感じ。

# 📝2025/09/26

## codemirror と lsp

- [Building a better online editor for TypeScript | Val Town Blog](https://blog.val.town/vtlsp?utm_source=chatgpt.com)
- [Bringing the TypeScript Language Server to Observable | Observable](https://observablehq.com/blog/bringing-the-typescript-language-server-to-observable?utm_source=chatgpt.com)
- [Codemirror 6 and Typescript LSP - v6 - discuss.CodeMirror](https://discuss.codemirror.net/t/codemirror-6-and-typescript-lsp/3398)

まず、server 建てて試した方がいいかもしれないけども、、、

# 📝 2025/09/16

## [p5SketchBook/dumpDirectlyTree.py at main · pome-ta/p5SketchBook · GitHub](https://github.com/pome-ta/p5SketchBook/blob/main/dumpDirectlyTree.py)

[`dialog` 要素に「ダイアログの外側クリックで閉じる」処理の追加](https://zenn.dev/de_teiu_tkg/articles/96a46374655e56)

# 📝 2025/09/13

## rubion-objc

[GitHub - pome-ta/pystaUIKitCatalogChallenge: Implemented Apple's official sample UIKitCatalog with Pythonista3 and rubicon-objc.](https://github.com/pome-ta/pystaUIKitCatalogChallenge)

`0.5.2` に update してみた

多分だけど、変更なく実行できてると思う

### mac での Pythonista3(rubicon) 実行

iPad でも問題なさそうなのに、mac だとクラッシュしてくれる。
クラッシュレポートちゃんと読んでないけど、何かしら実行環境が違うみたい。

Pythonista3 でも a-shell でも同様

`UIColor` をインスタンスとして呼び出す時に、`ObjCInstance` が呼べないとかなる

# 📝 2025/09/11

p5 の色々な環境

- [GitHub - pome-ta/p5js4codemirror6](https://github.com/pome-ta/p5js4codemirror6)
  - もりもりとコードを書いていくところ
  - エディタ機能もつけて編集ができる
  - 新しく何かを書くところ
- [GitHub - pome-ta/p5SketchBook](https://github.com/pome-ta/p5SketchBook)
  - Fix したコード置き場
  - ファイル一覧を`.json` で管理
  - `_` が先頭にあるファイル名が（多分読めない？）
    - ローカル実行だと大丈夫そう？
    - GitHub pages だとだめかも？
      - 要。原因検討

似通ったリポジトリになってしまった。が、入力保存機能のキャッチが、面倒。同一機能として統合はしないかも

## `importmap` がおもろい

[`<script type="importmap">` - HTML | MDN](https://developer.mozilla.org/ja/docs/Web/HTML/Reference/Elements/script/type/importmap)

`../.../` と設置場所依存な呼び出し方をしてたが、スッキリ読み込みの書き方ができそう

# 📝 2025/09/03

## [p5SketchBook/dumpDirectlyTree.py at main · pome-ta/p5SketchBook · GitHub](https://github.com/pome-ta/p5SketchBook/blob/main/dumpDirectlyTree.py)

`.json` の dump をつくる

- 作成日
- 更新日
- type
- 拡張子？

# 📝 2025/09/02

#api #github

## GitHub REST API (Contents API)

多分今回は使わないけど

```js
const owner = 'pome-ta';
const repo = 'p5SketchBook';

const endPoint = 'https://api.github.com/repos';
const kind = 'contents';

async function getRepoContents() {
  const res = await fetch([endPoint, owner, repo, kind].join('/'));
  const data = await res.json();
  console.log(data);
}

getRepoContents();
```

オブジェクトで、4 つ（リポジトリ直下のもの）を持って来てる

```js
(info): [
  {
    "name": "LICENSE",
    "path": "LICENSE",
    "sha": "a72ad48a1d26d5d69d4fa705a91eb3f32e888189",
    "size": 1064,
    "url": "https://api.github.com/repos/pome-ta/p5SketchBook/contents/LICENSE?ref=main",
    "html_url": "https://github.com/pome-ta/p5SketchBook/blob/main/LICENSE",
    "git_url": "https://api.github.com/repos/pome-ta/p5SketchBook/git/blobs/a72ad48a1d26d5d69d4fa705a91eb3f32e888189",
    "download_url": "https://raw.githubusercontent.com/pome-ta/p5SketchBook/main/LICENSE",
    "type": "file",
    "_links": {
      "self": "https://api.github.com/repos/pome-ta/p5SketchBook/contents/LICENSE?ref=main",
      "git": "https://api.github.com/repos/pome-ta/p5SketchBook/git/blobs/a72ad48a1d26d5d69d4fa705a91eb3f32e888189",
      "html": "https://github.com/pome-ta/p5SketchBook/blob/main/LICENSE"
    }
  },
  {
    "name": "README.md",
    "path": "README.md",
    "sha": "b39dfa34417ea243c03142560593600b490afb61",
    "size": 14,
    "url": "https://api.github.com/repos/pome-ta/p5SketchBook/contents/README.md?ref=main",
    "html_url": "https://github.com/pome-ta/p5SketchBook/blob/main/README.md",
    "git_url": "https://api.github.com/repos/pome-ta/p5SketchBook/git/blobs/b39dfa34417ea243c03142560593600b490afb61",
    "download_url": "https://raw.githubusercontent.com/pome-ta/p5SketchBook/main/README.md",
    "type": "file",
    "_links": {
      "self": "https://api.github.com/repos/pome-ta/p5SketchBook/contents/README.md?ref=main",
      "git": "https://api.github.com/repos/pome-ta/p5SketchBook/git/blobs/b39dfa34417ea243c03142560593600b490afb61",
      "html": "https://github.com/pome-ta/p5SketchBook/blob/main/README.md"
    }
  },
  {
    "name": "modules",
    "path": "modules",
    "sha": "52cb3d3bbf2538ae4dd5d00a93d2676541522631",
    "size": 0,
    "url": "https://api.github.com/repos/pome-ta/p5SketchBook/contents/modules?ref=main",
    "html_url": "https://github.com/pome-ta/p5SketchBook/tree/main/modules",
    "git_url": "https://api.github.com/repos/pome-ta/p5SketchBook/git/trees/52cb3d3bbf2538ae4dd5d00a93d2676541522631",
    "download_url": null,
    "type": "dir",
    "_links": {
      "self": "https://api.github.com/repos/pome-ta/p5SketchBook/contents/modules?ref=main",
      "git": "https://api.github.com/repos/pome-ta/p5SketchBook/git/trees/52cb3d3bbf2538ae4dd5d00a93d2676541522631",
      "html": "https://github.com/pome-ta/p5SketchBook/tree/main/modules"
    }
  },
  {
    "name": "sounds",
    "path": "sounds",
    "sha": "02e73a230f836ee824920dab794fbb5972747707",
    "size": 0,
    "url": "https://api.github.com/repos/pome-ta/p5SketchBook/contents/sounds?ref=main",
    "html_url": "https://github.com/pome-ta/p5SketchBook/tree/main/sounds",
    "git_url": "https://api.github.com/repos/pome-ta/p5SketchBook/git/trees/02e73a230f836ee824920dab794fbb5972747707",
    "download_url": null,
    "type": "dir",
    "_links": {
      "self": "https://api.github.com/repos/pome-ta/p5SketchBook/contents/sounds?ref=main",
      "git": "https://api.github.com/repos/pome-ta/p5SketchBook/git/trees/02e73a230f836ee824920dab794fbb5972747707",
      "html": "https://github.com/pome-ta/p5SketchBook/tree/main/sounds"
    }
  }
]

```

# 📝 2025/09/01

過去自分が書いた module みたいなのの整理

```js
export default class DomFactory {
  #element;
  #buildEvent;

  constructor(domTag) {
    this.#element =
      typeof domTag === 'string' ? document.createElement(domTag) : domTag;

    this.#buildEvent = new CustomEvent('build', { detail: this.#element });
  }

  get element() {
    return this.#element;
  }

  get buildEvent() {
    return this.#buildEvent;
  }

  static create(tag, options) {
    const instance = new this(tag);
    options
      ? Object.entries(options).forEach(([key, value]) => instance[key](value))
      : null;
    instance.element.dispatchEvent(instance.buildEvent);

    return instance.element;
  }

  setAttr(name, val) {
    this.#element.setAttribute(name, val);

    return this;
  }

  setAttrs(attrs) {
    Object.entries(attrs).forEach(([key, value]) => this.setAttr(key, value));

    return this;
  }

  setStyle(prop, val) {
    this.#element.style.setProperty(prop, val);

    return this;
  }

  setStyles(styles) {
    Object.entries(styles).forEach(([key, value]) => this.setStyle(key, value));

    return this;
  }

  addClassList(nameList) {
    this.#element.classList.add(...nameList);

    return this;
  }

  textContent(value) {
    this.#element.textContent = value;

    return this;
  }

  addEventListener({ type, listener, options }) {
    this.#element.addEventListener(type, listener, options);

    return this;
  }

  addEventListeners(args) {
    [...args].forEach((arg) => this.addEventListener(arg));

    return this;
  }

  targetAddEventListener({ target, type, listener, options }) {
    target.addEventListener(type, listener, options);

    return this;
  }

  targetAddEventListeners(args) {
    [...args].forEach((arg) => this.targetAddEventListener(arg));

    return this;
  }

  appendChildren(children) {
    [...children].forEach((child) => this.#element.appendChild(child));

    return this;
  }

  appendParent(parent) {
    parent.appendChild(this.#element);

    return this;
  }
}
```

```js
/**
 * メソッドチェーン（fluent interface）を用いてDOM要素の生成と操作を効率化するファクトリークラス。
 * @example
 * const button = DomFactory.create('button', { textContent: 'Click Me' });
 * document.body.appendChild(button);
 */
export default class DomFactory {
  /**
   * 内部で管理しているDOM要素。
   * @private
   * @type {HTMLElement}
   */
  #element;

  /**
   * 要素が生成されたことを示すカスタムイベント。
   * @private
   * @type {CustomEvent}
   */
  #buildEvent;

  /**
   * @param {string | HTMLElement} domTag - 作成する要素のタグ名（'div'など）、または既存のDOM要素。
   */
  constructor(domTag) {
    this.#element =
      typeof domTag === 'string' ? document.createElement(domTag) : domTag;
    this.#buildEvent = new CustomEvent('build', { detail: this.#element });
  }

  /**
   * 現在のDOM要素を返します。
   * @returns {HTMLElement}
   */
  get element() {
    return this.#element;
  }

  /**
   * 'build'カスタムイベントを返します。
   * @returns {CustomEvent}
   */
  get buildEvent() {
    return this.#buildEvent;
  }

  /**
   * 新しいDOM要素を生成し、オプションを適用します。
   * @param {string} tag - 生成する要素のHTMLタグ名。
   * @param {object} [options] - 要素に適用するメソッドと値のキーペア。例: { textContent: 'Hello' }
   * @returns {HTMLElement} - 生成・設定済みのDOM要素。
   */
  static create(tag, options) {
    const instance = new this(tag);
    options
      ? Object.entries(options).forEach(([key, value]) => instance[key](value))
      : null;
    instance.element.dispatchEvent(instance.buildEvent);

    return instance.element;
  }

  /**
   * 要素に属性を1つ設定します。
   * @param {string} name - 属性名。
   * @param {string} val - 属性値。
   * @returns {this} メソッドチェーンのための自身のインスタンス。
   */
  setAttr(name, val) {
    this.#element.setAttribute(name, val);
    return this;
  }

  /**
   * 要素に複数の属性を一度に設定します。
   * @param {Record<string, string>} attrs - 属性のキーと値のペアを持つオブジェクト。
   * @returns {this} メソッドチェーンのための自身のインスタンス。
   */
  setAttrs(attrs) {
    Object.entries(attrs).forEach(([key, value]) => this.setAttr(key, value));
    return this;
  }

  /**
   * 要素にCSSスタイルを1つ設定します。
   * @param {string} prop - CSSプロパティ名。
   * @param {string} val - CSSプロパティの値。
   * @returns {this} メソッドチェーンのための自身のインスタンス。
   */
  setStyle(prop, val) {
    this.#element.style.setProperty(prop, val);
    return this;
  }

  /**
   * 要素に複数のCSSスタイルを一度に設定します。
   * @param {Record<string, string>} styles - CSSプロパティのキーと値のペアを持つオブジェクト。
   * @returns {this} メソッドチェーンのための自身のインスタンス。
   */
  setStyles(styles) {
    Object.entries(styles).forEach(([key, value]) => this.setStyle(key, value));
    return this;
  }

  /**
   * 要素のクラスリストに複数のクラスを追加します。
   * @param {string[]} nameList - 追加するクラス名の配列。
   * @returns {this} メソッドチェーンのための自身のインスタンス。
   */
  addClassList(nameList) {
    this.#element.classList.add(...nameList);
    return this;
  }

  /**
   * 要素のテキストコンテンツを設定します。
   * @param {string} value - 設定するテキスト。
   * @returns {this} メソッドチェーンのための自身のインスタンス。
   */
  textContent(value) {
    this.#element.textContent = value;
    return this;
  }

  /**
   * 要素にイベントリスナーを1つ追加します。
   * @param {object} eventArgs - イベントリスナーの引数オブジェクト。
   * @param {string} eventArgs.type - イベントタイプ。
   * @param {EventListenerOrEventListenerObject} eventArgs.listener - イベントリスナー関数。
   * @param {boolean | AddEventListenerOptions} [eventArgs.options] - イベントリスナーのオプション。
   * @returns {this} メソッドチェーンのための自身のインスタンス。
   */
  addEventListener({ type, listener, options }) {
    this.#element.addEventListener(type, listener, options);
    return this;
  }

  /**
   * 要素に複数のイベントリスナーを一度に追加します。
   * @param {Array<object>} args - `addEventListener`に渡す引数オブジェクトの配列。
   * @returns {this} メソッドチェーンのための自身のインスタンス。
   */
  addEventListeners(args) {
    [...args].forEach((arg) => this.addEventListener(arg));
    return this;
  }

  /**
   * 任意のターゲットにイベントリスナーを1つ追加します。
   * @param {object} eventArgs - イベントリスナーの引数オブジェクト。
   * @param {EventTarget} eventArgs.target - イベントリスナーを追加するターゲット。
   * @param {string} eventArgs.type - イベントタイプ。
   * @param {EventListenerOrEventListenerObject} eventArgs.listener - イベントリスナー関数。
   * @param {boolean | AddEventListenerOptions} [eventArgs.options] - イベントリスナーのオプション。
   * @returns {this} メソッドチェーンのための自身のインスタンス。
   */
  targetAddEventListener({ target, type, listener, options }) {
    target.addEventListener(type, listener, options);
    return this;
  }

  /**
   * 任意のターゲットに複数のイベントリスナーを一度に追加します。
   * @param {Array<object>} args - `targetAddEventListener`に渡す引数オブジェクトの配列。
   * @returns {this} メソッドチェーンのための自身のインスタンス。
   */
  targetAddEventListeners(args) {
    [...args].forEach((arg) => this.targetAddEventListener(arg));
    return this;
  }

  /**
   * 要素に複数の子要素を追加します。
   * @param {HTMLElement[]} children - 追加する子要素の配列。
   * @returns {this} メソッドチェーンのための自身のインスタンス。
   */
  appendChildren(children) {
    [...children].forEach((child) => this.#element.appendChild(child));
    return this;
  }

  /**
   * 指定された親要素に自身を追加します。
   * @param {HTMLElement} parent - 親となるDOM要素。
   * @returns {this} メソッドチェーンのための自身のインスタンス。
   */
  appendParent(parent) {
    parent.appendChild(this.#element);
    return this;
  }
}
```

# 📝 2025/08/31

p5 の sketch を選択して変更させたい

ローカルではなく、リポジトリ上のを読み込む

# 📝 2025/08/25

codemirror は、依存関係が面倒だからやーめた

p5.sound(v1) の方で、`loadSound` が外部から呼べるようだったので、v1 で色々な example を実行することにする。

私の実装だと、sound 関係を一旦リセット（コード読み直し時）したい。が、`preload` 時に読み込みをしてしまうと
`setup` でリセットされてしまう。

であれば、addon として、sketch code に書かずとも、勝手に実行してもらうことにしたい

## p5.js の addon

[Creating an Addon Library](https://p5js.org/contribute/creating_libraries/)

`beta` もあるので、気をつけたし

[Creating an Addon Library](https://beta.p5js.org/contribute/creating_libraries/)

[GitHub - processing/p5.js-addon-template](https://github.com/processing/p5.js-addon-template)

この tmp のリポジトリは、v2 の方っぽい。

beta の

```js
function loadCSVAddon(p5, fn, lifecycles) {}
```

と、なっているため。

v1 は、`.prototype` をゴリゴリ使う：

```js
p5.Element.prototype.shout = function () {
  this.elt.innerHTML += '<span>!</span>';
};
```

ChatGPT に無理やり翻訳させたけど、ちょっと返答内容が気持ち悪い（step3 までしか出さないとか）ので

のんびりと、翻訳かけながら確認をするか。。。

### 実装整理

```js
function soundReStart() {
  // wip: クリップノイズ対策
  p.disposeSound();

  const soundArray = p.soundOut.soundArray;
  for (let soundIdx = soundArray.length - 1; soundIdx >= 0; soundIdx--) {
    const sound = soundArray[soundIdx];
    // todo: 過剰処理?
    sound?.stop && sound.stop();
    sound?.dispose && sound.dispose();
    sound?.disconnect && sound.disconnect();

    soundArray.splice(soundIdx, 1);
  }

  const parts = p.soundOut.parts;
  for (let partIdx = parts.length - 1; partIdx >= 0; partIdx--) {
    const phrases = parts[partIdx].phrases;
    for (let phraseIdx = phrases.length - 1; phraseIdx >= 0; phraseIdx--) {
      phrases.splice(phraseIdx, 1);
    }
    parts.splice(partIdx, 1);
  }

  p.soundOut.soundArray = [];
  p.soundOut.parts = [];
  p.soundOut.extensions = []; // todo: 対応必要?

  p.userStartAudio();
}
```

この処理を、`preload` の一番最初か、呼び出し直前に実行する。
addon とするので、`index.html` で読み込みをする。
sketch のコードには、記載しない（読み込みで勝手に実行する）。

#### 懸念

- 実行タイミングに寄って、読み込みが前後して壊れないように
- instance mode でも global mode でも実行できる
- `p5.sound` が呼ばれてないなら、そもそも実行しない

### 今後

他にも、個人的に addon として設定しているのもあるので、マナーを揃えて書いていきたい

# 📝 2025/08/24

## codemirror の esm.sh

さて、どれを呼んでいくか、、、

- [CodeMirror6 の概要と基本的な使い方](https://toach.biz/blog/codemirror6-basics/#2-2)
- [ESM-compatible CodeMirror build (directly importable in browser) - v6 - discuss.CodeMirror](https://discuss.codemirror.net/t/esm-compatible-codemirror-build-directly-importable-in-browser/5933)

キャレット出ないやつ

- [text selection grabbers no longer show on iOS (18.3.1) since 6.35.1 · Issue #1538 · codemirror/dev](https://github.com/codemirror/dev/issues/1538)

## `web-console-log` テスト

[GitHub - tkihira/web-console-log](https://github.com/tkihira/web-console-log)

ローカルにつっこんだら、素直に出てきた？

`eruda` と共存できないだけか？

```html
<script type="module">
  import eruda from 'https://esm.sh/eruda';

  eruda.init();
</script>
```

# 📝 2025/08/21

## todo ?

- スクリプトを選択できるように？
- `v0.1` の音データの外部読み込み
- ems.sh で codemirror 組んでみる
  - キャレットがで出てくるまで戻す
  - それで相互互換性が保てるか
- [GitHub - tkihira/web-console-log](https://github.com/tkihira/web-console-log) 検証

# 📝 2025/08/20

- [GitHub - processing/p5.sound](https://github.com/processing/p5.sound.js)
- [GitHub - Tonejs/Tone.js: A Web Audio framework for making interactive music in the browser.](https://github.com/Tonejs/Tone.js)

`v0.2` は、まだ先かなぁ、、、

音止めたりするのが、全く違うかも。

# 📝 2025/08/16

gas の非同期実行のやつ、まとめておきたい。しかし、実際に走るコード見ながらでないと、難しいな。。。

## 参照あさり

- [Make google.script.run looks a promise · GitHub](https://gist.github.com/torufurukawa/64339baf16efd3598e71dd763d1db0cf)
  - これでいいんだっけ？
- [Asynchronous execution for Google App Scripts (gas) · GitHub](https://gist.github.com/sdesalas/2972f8647897d5481fd8e01f03122805)
  - こっちの方が多く引っかかる？
- [【GAS の起動時間の制限を回避せよ】分割実行や非同期処理を使って高速実行を実現してみた！ - ポンコツエンジニアのごじゃっぺ開発日記。](https://www.pnkts.net/2019/12/25/gas-split-execution-and-_asynchronous-processing)
- [GAS の関数を Promise でラップする (魔法のオブジェクト Proxy) #JavaScript - Qiita](https://qiita.com/Shankou/items/7b73686c3aa9364c20ce)
  - `Proxy` ?
  - [Proxy - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Proxy)

## もってきたやつ

### menu 系

```js
const activeSpreadsheet = SpreadsheetApp.getActiveSpreadsheet();

function onOpen() {
  const ui = SpreadsheetApp.getUi();
  ui.createMenu('📂 ScriptMenus').addItem('一括処理', 'customUpdate').addToUi();
}

/**
 * カスタムメニューがクリックされた時に走る処理
 */
function customUpdate() {
  // xxx: ボタン誘導のコンテクスト
  // const selector = Browser.msgBox("じっこうするー", "実行することの意味を理解していますか？", Browser.Buttons.OK_CANCEL);
  // if (selector === 'cancel') {
  // return
  // }
  const html =
    HtmlService.createTemplateFromFile('customUpdate.html').evaluate();
  SpreadsheetApp.getUi().showModalDialog(
    html,
    '処理を止めたい場合は[x] 押す →'
  );
}

function getDeploySheetDataList() {
  const dataSheet = activeSpreadsheet.getSheetByName('社員リスト');
  const lastRow = dataSheet.getLastRow();
  const deployData = dataSheet.getRange(`A2:C${lastRow}`).getValues();
  return deployData;
}

/**
 * 実行ログの吐き出し
 */
function setStatusLogs(times, logs) {
  const ws = activeSpreadsheet.insertSheet(`log:${times[0]}`);
  console.log('------------- set');
  SpreadsheetApp.flush();
  ws.getRange('A1:A2').setValues([[times[0]], [times[1]]]);
  const length = logs.length;
  ws.getRange(`B1:E${length}`).setValues(logs);
}
```

### 実行のコア

```html
<!DOCTYPE html>
<html>
  <head>
    <base target="_top" />
    <script>
      // --- 非同期処理準備
      /* gasの関数で、Promise が返せるように */
      // https://gist.github.com/torufurukawa/64339baf16efd3598e71dd763d1db0cf
      function scriptRunPromise() {
        const gs = {};
        const keys = Object.keys(google.script.run);
        for (let i = 0; i < keys.length; i++) {
          gs[keys[i]] = (function (key) {
            return function (...args) {
              return new Promise(function (resolve, reject) {
                google.script.run
                  .withSuccessHandler(resolve)
                  .withFailureHandler(reject)
                  [key].apply(google.script.run, args);
              });
            };
          })(keys[i]);
        }
        return gs;
      }

      /**
       * 与えられたイテラブルから得られる関数を順に、
       * 指定された数まで並列に実行する。
       *
       * @param iterable {Iterable<() => Promise<void>}
       *  実行したい関数を要素に持つイテラブル。
       *  各関数は引数を持たず、Promise を返す。
       * @param concurrency {number} この数まで並列に実行する。
       * @return {Promise<void>}
       *  全ての関数を実行し終えると resolve される Promise。
       */
      async function runConcurrentlyAsync(iterable, concurrency) {
        const iterator = iterable[Symbol.iterator]();
        let index = 0; // ログ用
        const promises = Array.from({ length: concurrency }, (_, id) => {
          return new Promise(async (resolve) => {
            for (
              let result = iterator.next();
              !result.done;
              result = iterator.next()
            ) {
              const i = index++;
              console.log(`${id}: ${i}...`);
              await result.value();
              console.log(`        ...${id}: ${i}`);
              prgrsbr.value = i; // プログレスバー用
            }
            resolve();
          });
        });
        await Promise.all(promises);
      }
      // --- 非同期処理準備終わり/

      let prgrsbr, logDiv; // プログレスバー用
      let startTimeLog, finishTimeLog;
      let statusLogs = [];

      window.addEventListener('load', () => {
        console.log('load: start');
        // sv-SEロケールはYYYY-MM-DD形式の日付文字列を戻す
        const _ymd = new Date().toLocaleDateString('sv-SE');
        const _time = new Date().toLocaleTimeString('ja-JP', { hour12: false });
        startTimeLog = `${_ymd}_${_time}`;

        setUpUI();
        // todo: 初回に一括でデータを取ってくるところ
        scriptRunPromise()
          .getDeploySheetDataList()
          .then((resolve) => asyncTouchSheets(resolve));
      });
      // エリアグループ名と、URL の配列が入っている
      function asyncTouchSheets(arrayNameURLs) {
        const INIT = 0;
        const MAX = arrayNameURLs.length;
        const CONCURRENCY = 2; // 同時実行できる数を定義
        prgrsbr.max = MAX; // プログレスバー用

        const generator = (function* createGenerator() {
          for (let i = INIT; i < MAX; i++) {
            const [staffNum, staffName, mailAddress] = arrayNameURLs[i];
            const idx = i + 1;

            let stateSignal;

            // todo: 最終的に実行させる関数
            yield async () => {
              try {
                await scriptRunPromise().allCallFunc(
                  staffNum,
                  staffName,
                  mailAddress
                );
                stateSignal = ['🟢', ''];
              } catch (error) {
                stateSignal = ['🔴', `${error}`];
              } finally {
                addLogText(
                  `${stateSignal[0]}\t${idx}:\t${staffNum} \t${stateSignal[1]}`
                );
                statusLogs = [
                  ...statusLogs,
                  [stateSignal[0], idx, staffNum, stateSignal[1]],
                ];
              }
            };
          }
        })();

        runConcurrentlyAsync(generator, CONCURRENCY).then((resolve) => {
          const _ymd = new Date().toLocaleDateString('sv-SE');
          const _time = new Date().toLocaleTimeString('ja-JP', {
            hour12: false,
          });
          finishTimeLog = `${_ymd}_${_time}`;

          google.script.run.setStatusLogs(
            [startTimeLog, finishTimeLog],
            statusLogs
          );
          google.script.host.close();
        });
      }

      // 進捗プログレスバー作成用
      function addLogText(logWord) {
        const logText = document.createElement('p');
        logText.style.margin = 0;
        logText.style.padding = 0;
        logText.textContent = `${logWord}`;
        // logDiv.appendChild(logText);
        logDiv.prepend(logText);
      }

      function setUpUI() {
        const wrap = document.createElement('main');
        prgrsbr = document.createElement('progress');
        prgrsbr.style.width = '100%';
        prgrsbr.min = 0;
        prgrsbr.value = 0;
        prgrsbr.style.position = 'sticky';
        prgrsbr.style.top = 0;
        const memo =
          document.createTextNode('😤 処理が終わったら自動で消えます');
        logDiv = document.createElement('div');
        logDiv.style.fontSize = '0.8rem';
        document.body.appendChild(wrap);
        wrap.appendChild(prgrsbr);
        wrap.appendChild(memo);
        wrap.appendChild(logDiv);
      }
    </script>
  </head>

  <body></body>
</html>
```

### 逐次処理

```js
function allCallFunc(num, name, mail) {
  console.log(num);
  const s = SpreadsheetApp.openByUrl(mail);
  s.getSheetByName('a').getRange('A1').getValue();
}
```

## うーん

aside とか clap とか入れてやってみる？

# 📝 2025/08/12

## p5.sound の`reset`

> `p5.soundOut` is the p5.sound final output bus. It sends output to the destination of this window's web audio context. It contains Web Audio API nodes including a dyanmicsCompressor (<code>.limiter</code>), and Gain Nodes for <code>.input</code> and <code>.output</code>.
> `p5.soundOut` は p5.sound の最終出力バスです。このウィンドウの Web Audio コンテキストの宛先に出力を送信します。このバスには、Web Audio API ノード（dynamicsCompressor (<code>.limiter</code>) を含む）と、<code>.input</code> および <code>.output</code> 用の Gain ノードが含まれています。

# 📝 2025/08/11

[GitHub - tkihira/web-console-log](https://github.com/tkihira/web-console-log)

# 📝 2025/08/09

## `p5.Envelope`

[p5.Envelope](https://p5js.org/reference/p5.sound/p5.Envelope/)

play とか trigger とか

ADSR の指定と、`constructor` のデフォルトとか

# 📝 2025/08/02

## p5 sound

- [getOctaveBands](https://p5js.org/reference/p5.FFT/getOctaveBands/)
- [logAverages](https://p5js.org/reference/p5.FFT/logAverages/)

ここら辺、良い感じにつかえるのかしら？

## module 化？

そもそも、`import` 読み込み設定せんとあかんか

spectrum analyzer とか、大きくなってしまったので、分けて使うか？

# 📝 2025/07/31

ai にコード聞くようになってから、ここで書かなくなってるや

スペクトラムアナライザーとして log スケールとかもごもごやってる

# 📝 2025/07/16

## [GitHub - pome-ta/p5js4codemirror6](https://github.com/pome-ta/p5js4codemirror6) todo

- module 化
  - 依存減らす
    - 具体的な変数で参照させない？
    - `querySelector` も極力使わないとか
  - 自由度高く
- プレビューの画面
  - 全画面
    - header を出したり、隠したり
    - canvas の指定サイズには忠実でありたい
- autocomplete
  - p5 用に
  - lsp でなくてもいい
- エディタ設定
  - テーマ関係が雑
- `.css`
  - module ごとの考え方とか
  - 全体のは、ほぼ使わないとか？
- スケッチファイル
  - 切り替えできると良き？
  - ストレージとか？
- console 出せるようにする？
- 音のノイズ
  - 終了時、更新時のノイズ
  - クリップノイズをなるべく排除したい
