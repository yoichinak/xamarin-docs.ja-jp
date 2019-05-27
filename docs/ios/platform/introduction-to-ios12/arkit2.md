---
title: ARKit 2 in Xamarin.iOS
description: このドキュメントでは、iOS 12 で ARKit への更新について説明します。 重点的に検出するための参照オブジェクトとイメージを使用して、環境のテクスチャでは、コードを含むおよび ARKit プログラミングで一般的な問題について説明します。
ms.prod: xamarin
ms.assetid: af758092-1523-4ab7-aa53-c37a81fb156a
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/22/2018
ms.openlocfilehash: 81d9ab12a4b8e8184e0a61dc9b6d53d72004d25c
ms.sourcegitcommit: b986460787677cf8c2fc7cc8c03f4bc60c592120
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/24/2019
ms.locfileid: "66213325"
---
# <a name="arkit-2-in-xamarinios"></a>ARKit 2 in Xamarin.iOS

ARKit が昨年 iOS 11 で導入されて以来大きく高まっています。 何よりもまず、今すぐを検出できます垂直および水平方向の平面が大幅に屋内拡張現実エクスペリエンスの実用性が向上します。 さらに、これには新しい機能があります。

* 現実の世界とデジタル画像の間の結合として参照イメージとオブジェクトを認識します。
* 実際の照明をシミュレートする新しい照明モード
* 共有し、AR 環境を保持する機能
* 新しいファイル形式が AR コンテンツを格納するための推奨

## <a name="recognizing-reference-objects"></a>参照オブジェクトを認識します。

ARKit 2 ショーケース機能の 1 つは、参照イメージとオブジェクトを認識する機能です。 参照イメージは、通常のイメージ ファイルから読み込むことができます ([後述](#more-tracking-configurations))、開発者向けを使用してオブジェクトをスキャンする必要がありますの参照が[ `ARObjectScanningConfiguration`](xref:ARKit.ARObjectScanningConfiguration)します。

### <a name="sample-app-scanning-and-detecting-3d-objects"></a>サンプル アプリ:スキャンおよび 3D オブジェクトを検出します。

[スキャン、3 D オブジェクトを検出して](https://developer.xamarin.com/samples/monotouch/ios12/ScanningAndDetecting3DObjects/)サンプルは、ポートが、 [Apple プロジェクト](https://developer.apple.com/documentation/arkit/scanning_and_detecting_3d_objects?language=objc)を示しています。

* アプリケーションの状態管理を使用して[ `NSNotification` ](xref:Foundation.NSNotification)オブジェクト
* カスタム ビジュアル
* 複雑なジェスチャ
* オブジェクトのスキャン
* 格納する、 [`ARReferenceObject`](xref:ARKit.ARReferenceObject)

バッテリとプロセッサを集中的には参照オブジェクトをスキャンし、古いデバイスの安定性の追跡を実現するために問題がよくあります。

### <a name="state-management-using-nsnotification-objects"></a>NSNotification オブジェクトを使用して状態の管理

このアプリケーションは、次の状態が遷移するステート マシンを使用します。

* `AppState.StartARSession`
* `AppState.NotReady`
* `AppState.Scanning`
* `AppState.Testing`

さらに、埋め込まれた状態のセットを使用し、遷移の場合に`AppState.Scanning`:

* `Scan.ScanState.Ready`
* `Scan.ScanState.DefineBoundingBox`
* `Scan.ScanState.Scanning`
* `Scan.ScanState.AdjustingOrigin`

アプリへの通知の状態遷移を投稿する事後対応型のアーキテクチャを使用して[ `NSNotificationCenter` ](xref:Foundation.NSNotificationCenter)これらの通知をサブスクライブしているとします。 このスニペットのようにこの設定は`ViewController.cs`:

```csharp
// Configure notifications for application state changes
var notificationCenter = NSNotificationCenter.DefaultCenter;

notificationCenter.AddObserver(Scan.ScanningStateChangedNotificationName, State.ScanningStateChanged);
notificationCenter.AddObserver(ScannedObject.GhostBoundingBoxCreatedNotificationName, State.GhostBoundingBoxWasCreated);
notificationCenter.AddObserver(ScannedObject.GhostBoundingBoxRemovedNotificationName, State.GhostBoundingBoxWasRemoved);
notificationCenter.AddObserver(ScannedObject.BoundingBoxCreatedNotificationName, State.BoundingBoxWasCreated);
notificationCenter.AddObserver(BoundingBox.ScanPercentageChangedNotificationName, ScanPercentageChanged);
notificationCenter.AddObserver(BoundingBox.ExtentChangedNotificationName, BoundingBoxExtentChanged);
notificationCenter.AddObserver(BoundingBox.PositionChangedNotificationName, BoundingBoxPositionChanged);
notificationCenter.AddObserver(ObjectOrigin.PositionChangedNotificationName, ObjectOriginPositionChanged);
notificationCenter.AddObserver(NSProcessInfo.PowerStateDidChangeNotification, DisplayWarningIfInLowPowerMode);

```

一般的な通知のハンドラーは、UI を更新し、このハンドラー オブジェクトがスキャンされるように更新するなど、アプリケーションの状態を変更します。

```csharp
private void ScanPercentageChanged(NSNotification notification)
{
    var pctNum = TryGet<NSNumber>(notification.UserInfo, BoundingBox.ScanPercentageUserKey);
    if (pctNum == null)
    {
        return;
    }
    double percentage = pctNum.DoubleValue;
    // Switch to the next state if scan is complete
    if (percentage >= 100.0)
    {
        State.SwitchToNextState();
    }
    else
    {
        DispatchQueue.MainQueue.DispatchAsync(() => navigationBarController.SetNavigationBarTitle($"Scan ({percentage})"));
    }
}

```

最後に、`Enter{State}`メソッドは新しい状態に適したモデルと UX を変更します。

```csharp
internal void EnterStateTesting()
{
    navigationBarController.SetNavigationBarTitle("Testing");
    navigationBarController.ShowBackButton(false);
    loadModelButton.Hidden = true;
    flashlightButton.Hidden = false;
    nextButton.Enabled = true;
    nextButton.SetTitle("Share", UIControlState.Normal);

    testRun = new TestRun(sessionInfo, sceneView);
    TestObjectDetection();
    CancelMaxScanTimeTimer();
}
```

### <a name="custom-visualization"></a>カスタム ビジュアル

検出された水平面に投影、低レベル「ポイント クラウド」オブジェクトの境界ボックス内に含まれるアプリに表示されます。

このポイントのクラウドがで開発者が利用できる、 [ `ARFrame.RawFeaturePoints` ](xref:ARKit.ARFrame.RawFeaturePoints)プロパティ。 ポイント クラウドを効率的に視覚化する厄介な問題になります。 ポイントを反復しを作成して、ポイントごとに新しい SceneKit ノードを配置することは強制終了フレーム レート。 または、非同期的に完了すると場合、タイム ラグがあります。 サンプルは、3 つの部分を使用したパフォーマンスを維持します。

* Pin 内のデータを配置し、バイトの生バッファーとしてのデータを解釈するアンセーフ コードを使用します。
* その生のバッファーを変換する、 [ `SCNGeometrySource` ](xref:SceneKit.SCNGeometrySource) 「テンプレート」を作成および[ `SCNGeometryElement` ](xref:SceneKit.SCNGeometryElement)オブジェクト。
* 迅速に「を合成すること」生のデータとテンプレートを使用して、 [`SCNGeometry.Create(SCNGeometrySource[], SCNGeometryElement[])`](xref:SceneKit.SCNGeometry.Create(SceneKit.SCNGeometrySource[],SceneKit.SCNGeometryElement[]))

```csharp
internal static SCNGeometry CreateVisualization(NVector3[] points, UIColor color, float size)
{
  if (points.Length == 0)
  {
    return null;
  }

  unsafe
  {
    var stride = sizeof(float) * 3;

    // Pin the data down so that it doesn't move
    fixed (NVector3* pPoints = &amp;points[0])
    {
      // Important: Don't unpin until after `SCNGeometry.Create`, because geometry creation is lazy

      // Grab a pointer to the data and treat it as a byte buffer of the appropriate length
      var intPtr = new IntPtr(pPoints);
      var pointData = NSData.FromBytes(intPtr, (System.nuint) (stride * points.Length));

      // Create a geometry source (factory) configured properly for the data (3 vertices)
      var source = SCNGeometrySource.FromData(
        pointData,
        SCNGeometrySourceSemantics.Vertex,
        points.Length,
        true,
        3,
        sizeof(float),
        0,
        stride
      );

      // Create geometry element
      // The null and bytesPerElement = 0 look odd, but this is just a template object
      var template = SCNGeometryElement.FromData(null, SCNGeometryPrimitiveType.Point, points.Length, 0);
      template.PointSize = 0.001F;
      template.MinimumPointScreenSpaceRadius = size;
      template.MaximumPointScreenSpaceRadius = size;

      // Stitch the data (source) together with the template to create the new object
      var pointsGeometry = SCNGeometry.Create(new[] { source }, new[] { template });
      pointsGeometry.Materials = new[] { Utilities.Material(color) };
      return pointsGeometry;
    }
  }
}
```

結果のようになります。

![point_cloud](images/arkit_point_cloud.jpeg)

### <a name="complex-gestures"></a>複雑なジェスチャ

ユーザーは拡大縮小、回転、およびターゲット オブジェクトを囲む境界ボックスをドラッグします。 関連付けられているジェスチャ レコグナイザーで 2 つの興味深い点があります。

しきい値が渡されました。 後でのみをアクティブ化すべてジェスチャ レコグナイザーの最初に、たとえば、指が非常に多くのピクセルをドラッグまたは回転の角度を超えています。 この技法では、しきい値を超過した段階的に適用するまで、移動を蓄積します。

```csharp
// A custom rotation gesture recognizer that fires only when a threshold is passed
internal partial class ThresholdRotationGestureRecognizer : UIRotationGestureRecognizer
{
    // The threshold after which this gesture is detected.
    const double threshold = Math.PI / 15; // (12°)

    // Indicates whether the currently active gesture has exceeded the threshold
    private bool thresholdExceeded = false;

    private double previousRotation = 0;
    internal double RotationDelta { get; private set; }

    internal ThresholdRotationGestureRecognizer(IntPtr handle) : base(handle)
    {
    }

    // Observe when the gesture's state changes to reset the threshold
    public override UIGestureRecognizerState State
    {
        get => base.State;
        set
        {
            base.State = value;

            switch(value)
            {
                case UIGestureRecognizerState.Began :
                case UIGestureRecognizerState.Changed :
                    break;
                default :
                    // Reset threshold check
                    thresholdExceeded = false;
                    previousRotation = 0;
                    RotationDelta = 0;
                    break;
            }
        }
    }

    public override void TouchesMoved(NSSet touches, UIEvent evt)
    {
        base.TouchesMoved(touches, evt);

        if (thresholdExceeded)
        {
            RotationDelta = Rotation - previousRotation;
            previousRotation = Rotation;
        }

        if (! thresholdExceeded && Math.Abs(Rotation) > threshold)
        {
            thresholdExceeded = true;
            previousRotation = Rotation;
        }
    }
}
```

ジェスチャを基準として実行されている 2 つ目の興味深い点は、現実世界の平面が検出されたに対する境界ボックスが移動される方法です。 この側面は、後ほど[この Xamarin ブログの投稿](https://blog.xamarin.com/exploring-new-ios-12-arkit-capabilities-with-xamarin/)します。

## <a name="other-new-features-in-arkit-2"></a>ARKit 2 で他の新機能

### <a name="more-tracking-configurations"></a>複数の追跡の構成

ここで、複合現実エクスペリエンスの基盤として、次のいずれかを使用することができます。

* デバイスの加速のみ ([`AROrientationTrackingConfiguration`](xref:ARKit.AROrientationTrackingConfiguration)、iOS 11)
* 面 ([`ARFaceTrackingConfiguration`](xref:ARKit.ARFaceTrackingConfiguration)、iOS 11)
* イメージを参照 ([`ARImageTrackingConfiguration`](xref:ARKit.ARImageTrackingConfiguration)、iOS 12)
* 3D オブジェクトをスキャン ([`ARObjectScanningConfiguration`](xref:ARKit.ARObjectScanningConfiguration)、iOS 12)
* ビジュアルの慣性 odometry ([`ARWorldTrackingConfiguration`](xref:ARKit.ARWorldTrackingConfiguration)iOS 12 でが改善され、)

`AROrientationTrackingConfiguration`、で説明した[このブログの投稿とF#サンプル](https://github.com/lobrien/FSharp_Face_AR)は最も限定的な、複合現実エクスペリエンスの低下は、デバイスを関連付けるし、画面にしようとしないで、デバイスの動き、関連オブジェクトをデジタルのみ配置現実の世界です。

`ARImageTrackingConfiguration`現実 2D イメージ (絵画、ロゴなど) を認識し、デジタル画像のアンカーを使用することができます。

```csharp
var imagesAndWidths = new[] {
    ("cover1.jpg", 0.185F),
    ("cover2.jpg", 0.185F),
     //...etc...
    ("cover100.jpg", 0.185F),
};

var referenceImages = new NSSet<ARReferenceImage>(
    imagesAndWidths.Select( imageAndWidth =>
    {
      // Tuples cannot be destructured in lambda arguments
        var (image, width) = imageAndWidth;
        // Read the image
        var img = UIImage.FromFile(image).CGImage;
        return new ARReferenceImage(img, ImageIO.CGImagePropertyOrientation.Up, width);
    }).ToArray());

configuration.TrackingImages = referenceImages;
```

この構成に 2 つの興味深い側面があります。

* 可能性のある多数の参照イメージで使用できる、効率的です。
* そのイメージが現実の世界を移動する場合でも、イメージにデジタル画像が固定されています (たとえば、本の表紙が認識された場合に、追跡書籍ダウンなどのレイアウト、棚から取り出され、。)。

`ARObjectScanningConfiguration`説明した[以前](#recognizing-reference-objects)および 3D オブジェクトをスキャンするための開発者向けの構成。 強くプロセッサとバッテリの処理を要するエンド ユーザー アプリケーションで使用する必要があります。 サンプル[スキャン、3 D オブジェクトを検出して](https://developer.xamarin.com/samples/monotouch/ios12/ScanningAndDetecting3DObjects/)この構成の使用方法を示します。

最終的な追跡の構成、 `ARWorldTrackingConfiguration` 、ほとんどの複合現実エクスペリエンスの主力製品ですが。 この構成は、「機能ポイント」の現実世界をデジタル画像に関連付けるには、"visual 慣性 odometry"を使用します。 デジタル geometry やスプライトが現実世界の水平および垂直の平面の基準とした固定またはに関連する検出`ARReferenceObject`インスタンス。 この構成で世界配信元が重力に合わせた z 軸で空間でカメラの元の位置にあり、デジタル オブジェクト「ままインプレース」現実の世界でのオブジェクトに対して相対的です。

### <a name="environmental-texturing"></a>環境のテクスチャ

ARKit 2 では、キャプチャしたイメージを使用して、照明の推定をも輝く物体に反射の光源を適用「環境テクスチャ」をサポートします。 環境キューブ マップ動的に構築され、カメラがすべての方向に検索した後は、持ち運び現実的な経験を生成できます。

![環境のテクスチャ デモ イメージ](images/arkit_env_texturing.png)

環境のテクスチャを使用するには

* [ `SCNMaterial` ](xref:SceneKit.SCNMaterial)オブジェクトを使用する必要があります[ `SCNLightingModel.PhysicallyBased` ](xref:SceneKit.SCNLightingModel.PhysicallyBased)の 0 ~ 1 の範囲内の値を割り当てると[ `Metalness.Contents` ](xref:SceneKit.SCNMaterial.Metalness)と[ `Roughness.Contents`](xref:SceneKit.SCNMaterialProperty.Contents)と
* 追跡構成を設定する必要があります[ `EnvironmentTexturing` ](xref:ARKit.ARWorldTrackingConfiguration.EnvironmentTexturing)  =  [ `AREnvironmentTexturing.Automatic` ](xref:ARKit.AREnvironmentTexturing.Automatic) :

```csharp
var sphere = SCNSphere.Create(0.33F);
sphere.FirstMaterial.LightingModelName = SCNLightingModel.PhysicallyBased;
// Shiny metallic sphere
sphere.FirstMaterial.Metalness.Contents = new NSNumber(1.0F);
sphere.FirstMaterial.Roughness.Contents = new NSNumber(0.0F);

// Session configuration:
var configuration = new ARWorldTrackingConfiguration
{
    PlaneDetection = ARPlaneDetection.Horizontal | ARPlaneDetection.Vertical,
    LightEstimationEnabled = true,
    EnvironmentTexturing = AREnvironmentTexturing.Automatic
};
```

上記のコード スニペットに示すように完全に反射テクスチャ サンプルの楽しいですが、環境のテクスチャをお勧め可能性があります (テクスチャがどのようなカメラに基づいた概算のみ、「突き詰めてバレー」の応答をトリガーにならないように普通の使用記録)。


### <a name="shared-and-persistent-ar-experiences"></a>共有され、AR の永続的なエクスペリエンス

ARKit 2 にもう 1 つの主要な機能として、 [ `ARWorldMap` ](xref:ARKit.ARWorldMap)クラスは、共有したり、世界中の追跡データを格納することができます。 現在の世界地図を取得する[ `ARSession.GetCurrentWorldMapAsync` ](xref:ARKit.ARSession.GetCurrentWorldMapAsync)または[ `GetCurrentWorldMap(Action<ARWorldMap,NSError>)` ](xref:ARKit.ARSession.GetCurrentWorldMap(System.Action{ARKit.ARWorldMap,Foundation.NSError})) :

```csharp
// Local storage
var PersistentWorldPath => Environment.GetFolderPath(Environment.SpecialFolder.Personal) + "/arworldmap";

// Later, after scanning the environment thoroughly...
var worldMap = await Session.GetCurrentWorldMapAsync();
if (worldMap != null)
{
    var data = NSKeyedArchiver.ArchivedDataWithRootObject(worldMap, true, out var err);
    if (err != null)
    {
        Console.WriteLine(err);
    }
    File.WriteAllBytes(PersistentWorldPath, data.ToArray());
}
```

共有または世界地図を復元します。

1. ファイルからデータを読み込む
2. 展開に、`ARWorldMap`オブジェクト
3. 値として使用する、 [ `ARWorldTrackingConfiguration.InitialWorldMap` ](xref:ARKit.ARWorldTrackingConfiguration.InitialWorldMap)プロパティ。

```csharp
var data = NSData.FromArray(File.ReadAllBytes(PersistentWorldController.PersistenWorldPath));
var worldMap = (ARWorldMap)NSKeyedUnarchiver.GetUnarchivedObject(typeof(ARWorldMap), data, out var err);

var configuration = new ARWorldTrackingConfiguration
{
    PlaneDetection = ARPlaneDetection.Horizontal | ARPlaneDetection.Vertical,
    LightEstimationEnabled = true,
    EnvironmentTexturing = AREnvironmentTexturing.Automatic,
    InitialWorldMap = worldMap
};
```

`ARWorldMap`のみ非表示の世界の追跡データが含まれる、 [ `ARAnchor` ](xref:ARKit.ARAnchor)オブジェクトの場合、これは_いない_デジタル資産が含まれて。 ジオメトリまたは画像を共有するには、ユース ケースに適切な独自の戦略を開発する必要があります (おそらく、場所と、ジオメトリの向きのみを格納/送信、静的に適用することによって`SCNGeometry`または格納/送信します。シリアル化されたオブジェクト)。 利点、`ARWorldMap`は、資産が 1 回、共有を基準に配置される`ARAnchor`、デバイスまたはセッションの間で一貫して表示されます。

### <a name="universal-scene-description-file-format"></a>ユニバーサルのシーンの記述ファイルの形式

ARKit 2 の見出し行の最終的な機能は、Apple の導入の Pixar の[シーンの Universal Description](https://graphics.pixar.com/usd/docs/Introduction-to-USD.html)ファイル形式。 この形式は、共有および ARKit 資産を格納するための推奨される形式としての Collada DAE 形式を置き換えます。 資産を視覚化するためのサポートは、iOS 12 と Mojave に組み込まれています。 USDZ ファイル拡張機能は、(米ドル) ファイルを含む、圧縮と暗号化されていない zip アーカイブです。 Pixar [(米ドル) ファイルを操作するためのツールを提供します。](https://graphics.pixar.com/usd/docs/USD-Toolset.html#USDToolset-usdedit)がまだかなりのサード パーティのサポートはありません。

## <a name="arkit-programming-tips"></a>ARKit プログラミングのヒント

### <a name="manual-resource-management"></a>リソースの手動管理

ARKit でリソースを手動で管理するために重要です。 こうすれば、高のフレーム レート、実際にはだけでなく_必要_混乱を招く「画面のフリーズ」を回避するために ARKit フレームワークが新しいカメラ フレームの指定に関する遅延 ([`ARSession.CurrentFrame`](xref:ARKit.ARSession.CurrentFrame)します。 現在まで[ `ARFrame` ](xref:ARKit.ARFrame)しました`Dispose()`で呼び出されると、ARKit が供給されない新しいフレーム。 これにより、「固定」場合でも、アプリの残りの部分が応答するビデオです。 解決するには常にアクセスする`ARSession.CurrentFrame`で、`using`ブロックまたは手動で呼び出す`Dispose()`にします。

派生したすべてのオブジェクト`NSObject`は`IDisposable`と`NSObject`実装、 [Dispose パターン](https://docs.microsoft.com/dotnet/standard/design-guidelines/dispose-pattern)で一般的に実行する必要があるため、[を実装するためには、このパターン`Dispose`の派生クラス](https://docs.microsoft.com/dotnet/standard/garbage-collection/implementing-dispose)します。

### <a name="manipulating-transform-matrices"></a>変換行列を操作します。

3D アプリケーションで、コンパクトに移動、回転、および 3 D 空間でオブジェクトを傾斜させる方法について説明する 4 x 4 変換行列を扱うしようとしています。 これらはである SceneKit を[ `SCNMatrix4` ](xref:SceneKit.SCNMatrix4)オブジェクト。  

[ `SCNNode.Transform` ](xref:SceneKit.SCNNode.Transform)プロパティが返す、`SCNMatrix4`の変換行列、 [ `SCNNode` ](xref:SceneKit.SCNNode) _支えと_行優先`simdfloat4x4`型。 だから例えば：

```csharp
var node = new SCNNode { Position = new SCNVector3(2, 3, 4) };  
var xform = node.Transform;
Console.WriteLine(xform);
// Output is: "(1, 0, 0, 0)\n(0, 1, 0, 0)\n(0, 0, 1, 0)\n(2, 3, 4, 1)"
```  

ご覧のように、一番下の行の最初の 3 つの要素の位置がエンコードされます。

Xamarin では、変換行列を操作するための一般的な型は`NVector4`規則では、列優先の方法で解釈されます。 つまり、M14、M24、M34、M41、M42 ではなく M43 に翻訳/位置コンポーネントが必要です。

![行優先と列優先](images/arkit_row_vs_column.png)

マトリックスの解釈の選択と一貫しているは、適切な動作に不可欠です。 一貫性のミスで任意の種類のコンパイル時または実行時も例外は生成されません 3D 変換行列は、4 x 4 であるためは操作が予期せずに機能することだけです。 場合、SceneKit/ARKit オブジェクトのスタックや飛行、ジッターと思われる、不適切な変換マトリックスが適切な可能性があります。 ソリューションは簡単: [ `NMatrix4.Transpose` ](xref:OpenTK.NMatrix4.Transpose*)要素のインプレース入れ換えを実行します。

## <a name="related-links"></a>関連リンク

- [サンプル アプリ – スキャンおよび 3D オブジェクトを検出します。](https://developer.xamarin.com/samples/monotouch/iOS12/ScanningAndDetecting3DObjects/)
- [新機能については ARKit 2 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/602/)
- [Understanding ARKit を追跡および検出 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/610/)
- [Xamarin.iOS で ARKit の概要](https://docs.microsoft.com/xamarin/ios/platform/introduction-to-ios11/arkit/)
