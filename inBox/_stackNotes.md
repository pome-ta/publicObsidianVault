もう面倒だから、全部書き落としていくか

# 📝 2026/04/04

## 差分 : `11 - Ambient Lighting/Final/`

### Instance.swift

- File Diff: `10 - Instances/Challenge/Instance.swift` vs `11 - Ambient Lighting/Final/Instance.swift`

```diff Instance.swift:swift
--- 10 - Instances/Challenge/Instance.swift
+++ 11 - Ambient Lighting/Final/Instance.swift
@@ -67,0 +68,2 @@
+  
+
@@ -74 +75,0 @@
-    
@@ -85 +85,0 @@
-    
@@ -88 +87,0 @@
-    
```


### LightingScene.swift

- new file


### Model.swift

- File Diff: `10 - Instances/Challenge/Model.swift` vs `11 - Ambient Lighting/Final/Model.swift`

```diff Model.swift:swift
--- 10 - Instances/Challenge/Model.swift
+++ 11 - Ambient Lighting/Final/Model.swift
@@ -69 +69 @@
-      fragmentFunctionName = "textured_fragment"
+      fragmentFunctionName = "lit_textured_fragment"
@@ -71 +70,0 @@
-    
@@ -109 +107,0 @@
-    
```


### Scene.swift

- File Diff: `10 - Instances/Challenge/Scene.swift` vs `11 - Ambient Lighting/Final/Scene.swift`

```diff Scene.swift:swift
--- 10 - Instances/Challenge/Scene.swift
+++ 11 - Ambient Lighting/Final/Scene.swift
@@ -29,0 +30 @@
+  var light = Light()
@@ -44,0 +46,4 @@
+    commandEncoder.setFragmentBytes(&light,
+                                    length: MemoryLayout<Light>.stride,
+                                    at: 3)
+    
```


### Types.swift

- File Diff: `10 - Instances/Challenge/Types.swift` vs `11 - Ambient Lighting/Final/Types.swift`

```diff Types.swift:swift
--- 10 - Instances/Challenge/Types.swift
+++ 11 - Ambient Lighting/Final/Types.swift
@@ -38,0 +39,7 @@
+
+struct Light {
+  var color = float3(1)
+  var ambientIntensity: Float = 1.0
+}
+
+
```


### ViewController.swift

- File Diff: `10 - Instances/Challenge/ViewController.swift` vs `11 - Ambient Lighting/Final/ViewController.swift`

```diff ViewController.swift:swift
--- 10 - Instances/Challenge/ViewController.swift
+++ 11 - Ambient Lighting/Final/ViewController.swift
@@ -51 +51 @@
-    metalView.clearColor =  Colors.skyBlue
+    metalView.clearColor =  Colors.wenderlichGreen
@@ -53 +53 @@
-    renderer?.scene = LandscapeScene(device: device, size: view.bounds.size)
+    renderer?.scene = LightingScene(device: device, size: view.bounds.size)
```





# 📝 2026/04/03

## 差分 : `10 - Instances/Challenge/`

今回はpdf 確認しながらステップバイステップでいきたいかもな


### Instance.swift

- File Diff: `Final/Instance.swift` vs `Challenge/Instance.swift`

```diff Instance.swift:swift
--- Final/Instance.swift
+++ Challenge/Instance.swift
@@ -68,2 +67,0 @@
-  
-
@@ -75,0 +74 @@
+    
@@ -85,0 +85 @@
+    
@@ -87,0 +88 @@
+    
```


### LandscapeScene.swift

- File Diff: `Final/LandscapeScene.swift` vs `Challenge/LandscapeScene.swift`

```diff LandscapeScene.swift:swift
--- Final/LandscapeScene.swift
+++ Challenge/LandscapeScene.swift
@@ -27,0 +28,3 @@
+  let ground: Plane
+  let grass: Instance
+  let mushroom: Model
@@ -29,0 +33,4 @@
+    ground = Plane(device: device)
+    grass = Instance(device: device, modelName: "grass",
+                     instances: 10000)
+    mushroom = Model(device: device, modelName: "mushroom")
@@ -33,0 +41,48 @@
+    setupScene()
+  }
+  
+  func setupScene() {
+    ground.materialColor = float4(0.4, 0.3, 0.1, 1) // brown
+    add(childNode: ground)
+    add(childNode: grass)
+    add(childNode: mushroom)
+    
+    ground.scale = float3(20)
+    ground.rotation.x = radians(fromDegrees: 90)
+    
+    camera.rotation.x = radians(fromDegrees: -10)
+    camera.position.z = -20
+    camera.position.y = -2
+    
+    let greens = [
+      float4(0.34, 0.51, 0.01, 1),
+      float4(0.5, 0.5, 0, 1),
+      float4(0.29, 0.36, 0.14, 1)
+    ]
+    
+    for row in 0..<100 {
+      for column in 0..<100 {
+        var position = float3(0)
+        position.x = Float(row) / 4
+        position.z = Float(column) / 4
+        
+        let blade = grass.nodes[row * 100 + column]
+        blade.scale = float3(0.5)
+        blade.position = position
+        
+        blade.materialColor = greens[Int(arc4random_uniform(3))]
+        blade.rotation.y = radians(fromDegrees: Float(arc4random_uniform(360)))
+      }
+    }
+    grass.position.x = -12
+    grass.position.z = -12
+    
+    mushroom.position.x = -6
+    mushroom.position.z = -8
+    mushroom.scale = float3(2)
+    
+    sun.position.y = 7
+    sun.position.x = 6
+    sun.scale = float3(2)
+    
+    camera.fovDegrees = 25
```


### Primitive.swift

- File Diff: `Final/Primitive.swift` vs `Challenge/Primitive.swift`

```diff Primitive.swift:swift
--- Final/Primitive.swift
+++ Challenge/Primitive.swift
@@ -27,0 +28 @@
+  
@@ -37 +38 @@
-  var fragmentFunctionName: String = "fragment_shader"
+  var fragmentFunctionName: String = "fragment_color"
@@ -116,0 +118 @@
+    modelConstants.materialColor = materialColor
```


### Renderer.swift

- File Diff: `Final/Renderer.swift` vs `Challenge/Renderer.swift`

```diff Renderer.swift:swift
--- Final/Renderer.swift
+++ Challenge/Renderer.swift
@@ -59 +59,3 @@
-  func mtkView(_ view: MTKView, drawableSizeWillChange size: CGSize) { }
+  func mtkView(_ view: MTKView, drawableSizeWillChange size: CGSize) {
+    scene?.sceneSizeWillChange(to: size)
+  }
```


### Scene.swift

- File Diff: `Final/Scene.swift` vs `Challenge/Scene.swift`

```diff Scene.swift:swift
--- Final/Scene.swift
+++ Challenge/Scene.swift
@@ -35 +34,0 @@
-    camera.aspect = Float(size.width / size.height)
@@ -53,0 +53,4 @@
+  
+  func sceneSizeWillChange(to size: CGSize) {
+    camera.aspect = Float(size.width / size.height)
+  }
```


### ViewController.swift

- File Diff: `Final/ViewController.swift` vs `Challenge/ViewController.swift`

```diff ViewController.swift:swift
--- Final/ViewController.swift
+++ Challenge/ViewController.swift
@@ -53 +53 @@
-    renderer?.scene = InstanceScene(device: device, size: view.bounds.size)
+    renderer?.scene = LandscapeScene(device: device, size: view.bounds.size)
```






# 📝 2026/04/01

## difflib で差分確認

参照コードの変化が目視確認では辛いので、ゴリっと出すようにした

## diff

なんとなく、改行での差分が多めか？

### AppDelegate.swift

- File Diff: `9 - Model IO/Challenge/AppDelegate.swift` vs `10 - Instances/Final/AppDelegate.swift`

```diff AppDelegate.swift:swift
--- 9 - Model IO/Challenge/AppDelegate.swift
+++ 10 - Instances/Final/AppDelegate.swift
@@ -30 +29,0 @@
-
```


### CrowdScene.swift

- new file


### GameScene.swift

- File Diff: `9 - Model IO/Challenge/GameScene.swift` vs `10 - Instances/Final/GameScene.swift`

```diff GameScene.swift:swift
--- 9 - Model IO/Challenge/GameScene.swift
+++ 10 - Instances/Final/GameScene.swift
@@ -41 +40,0 @@
-  
```


### Instance.swift

- new file


### InstanceScene.swift

- new file


### Model.swift

- File Diff: `9 - Model IO/Challenge/Model.swift` vs `10 - Instances/Final/Model.swift`

```diff Model.swift:swift
--- 9 - Model IO/Challenge/Model.swift
+++ 10 - Instances/Final/Model.swift
@@ -108,0 +109 @@
+    
@@ -116 +116,0 @@
-    
```


### Primitive.swift

- File Diff: `9 - Model IO/Challenge/Primitive.swift` vs `10 - Instances/Final/Primitive.swift`

```diff Primitive.swift:swift
--- 9 - Model IO/Challenge/Primitive.swift
+++ 10 - Instances/Final/Primitive.swift
@@ -28 +27,0 @@
-  
```


### Renderable.swift

- File Diff: `9 - Model IO/Challenge/Renderable.swift` vs `10 - Instances/Final/Renderable.swift`

```diff Renderable.swift:swift
--- 9 - Model IO/Challenge/Renderable.swift
+++ 10 - Instances/Final/Renderable.swift
@@ -57 +56,0 @@
-
```


### ViewController.swift

- File Diff: `9 - Model IO/Challenge/ViewController.swift` vs `10 - Instances/Final/ViewController.swift`

```diff ViewController.swift:swift
--- 9 - Model IO/Challenge/ViewController.swift
+++ 10 - Instances/Final/ViewController.swift
@@ -53 +53 @@
-    renderer?.scene = LandscapeScene(device: device, size: view.bounds.size)
+    renderer?.scene = InstanceScene(device: device, size: view.bounds.size)
```



