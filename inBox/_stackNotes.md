もう面倒だから、全部書き落としていくか


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



