---
title: Xamarin. iOS の ARKit 2
description: このドキュメントでは、iOS 12 における ARKit の更新について説明します。 ここでは、参照オブジェクトと検出のためのイメージの使用に焦点を当て、環境テクスチャのコードを示し、ARKit プログラミングにおける一般的な問題について説明します。
ms.prod: xamarin
ms.assetid: af758092-1523-4ab7-aa53-c37a81fb156a
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 08/22/2018
ms.openlocfilehash: 17995f61d92856a88769e2cd7ac8ed7445cf9782
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70281136"
---
# <a name="arkit-2-in-xamarinios"></a>Xamarin. iOS の ARKit 2

ARKit は、iOS 11 の昨年の導入以来、大幅に成熟しています。 まず、縦方向のプレーンを検出できるようになりました。これにより、実用性の室内エクスペリエンスが大幅に向上します。 さらに、次の新機能が追加されています。

- 現実世界とデジタル画像の間の接合としての参照イメージとオブジェクトの認識
- 実際の照明をシミュレートする新しい照明モード
- AR 環境を共有して永続化する機能
- AR コンテンツを格納するために推奨される新しいファイル形式

## <a name="recognizing-reference-objects"></a>参照オブジェクトの認識

ARKit 2 の1つのショーケース機能は、参照イメージとオブジェクトを認識する機能です。 参照イメージは通常のイメージファイルから読み込むことができますが ([後で説明](#more-tracking-configurations)します)、参照オブジェクトは、 [`ARObjectScanningConfiguration`](xref:ARKit.ARObjectScanningConfiguration)開発者が重視したを使用してスキャンする必要があります。

### <a name="sample-app-scanning-and-detecting-3d-objects"></a>サンプルアプリ:3D オブジェクトのスキャンと検出

[3D オブジェクトのスキャンと検出](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-scanninganddetecting3dobjects)のサンプルは、 [Apple プロジェクト](https://developer.apple.com/documentation/arkit/scanning_and_detecting_3d_objects?language=objc)のポートであり、次のことを示しています。

- オブジェクトを使用し[`NSNotification`](xref:Foundation.NSNotification)たアプリケーション状態管理
- カスタムビジュアル
- 複雑なジェスチャ
- オブジェクトのスキャン
- 格納 ([`ARReferenceObject`](xref:ARKit.ARReferenceObject)

参照オブジェクトのスキャンはバッテリおよびプロセッサ集中型であり、古いデバイスでは、安定した追跡を実現する際に問題が発生することがよくあります。

### <a name="state-management-using-nsnotification-objects"></a>NSNotification オブジェクトを使用した状態管理

このアプリケーションは、次の状態の間を遷移するステートマシンを使用します。

- `AppState.StartARSession`
- `AppState.NotReady`
- `AppState.Scanning`
- `AppState.Testing`

さらに、次の`AppState.Scanning`場合には、状態と遷移の埋め込みセットも使用します。

- `Scan.ScanState.Ready`
- `Scan.ScanState.DefineBoundingBox`
- `Scan.ScanState.Scanning`
- `Scan.ScanState.AdjustingOrigin`

このアプリでは、状態遷移通知をに[`NSNotificationCenter`](xref:Foundation.NSNotificationCenter)送信し、これらの通知をサブスクライブするリアクティブアーキテクチャを使用します。 セットアップは次のスニペットの`ViewController.cs`ようになります。

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

一般的な通知ハンドラーは UI を更新し、場合によっては、オブジェクトのスキャン時に更新されるこのハンドラーなどのアプリケーションの状態を変更します。

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

最後に`Enter{State}` 、メソッドは、新しい状態に応じてモデルと UX を変更します。

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

### <a name="custom-visualization"></a>カスタムビジュアル

アプリには、検出された水平平面に投影された境界ボックス内に含まれるオブジェクトの低レベルの "ポイントクラウド" が表示されます。

このポイントクラウドは、 [`ARFrame.RawFeaturePoints`](xref:ARKit.ARFrame.RawFeaturePoints)開発者がプロパティで使用できます。 ポイントクラウドを効率的に視覚化することは、厄介な問題になる可能性があります。 ポイントを繰り返し処理した後、ポイントごとに新しい SceneKit ノードを作成して配置すると、フレームレートが強制終了します。 または、非同期に完了した場合は、遅延が発生します。 このサンプルでは、3つの部分から構成される方法でパフォーマンスを維持します。

- アンセーフコードを使用してデータを適切に固定し、データをバイトの生バッファーとして解釈します。
- 生のバッファー [`SCNGeometrySource`](xref:SceneKit.SCNGeometrySource)をに変換し、"template" [`SCNGeometryElement`](xref:SceneKit.SCNGeometryElement)オブジェクトを作成します。
- を使用して生データとテンプレートを迅速に "合成" します。[`SCNGeometry.Create(SCNGeometrySource[], SCNGeometryElement[])`](xref:SceneKit.SCNGeometry.Create(SceneKit.SCNGeometrySource[],SceneKit.SCNGeometryElement[]))

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

結果は次のようになります。

![point_cloud](images/arkit_point_cloud.jpeg)

### <a name="complex-gestures"></a>複雑なジェスチャ

ユーザーは、ターゲットオブジェクトを囲む境界ボックスを拡大縮小、回転、ドラッグすることができます。 関連するジェスチャレコグナイザーには、興味深い点が2つあります。

まず、すべてのジェスチャレコグナイザーは、しきい値が渡された後にのみアクティブになります。たとえば、指が多くのピクセルをドラッグしたり、回転が一定の角度を超えたりしたとします。 この手法では、しきい値を超えたときに移動を累積し、段階的に適用します。

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

ジェスチャに関連して実行される2番目の興味深い点は、検出された実際の平面に対して境界ボックスが移動される方法です。 この側面については、 [Xamarin のブログ記事](https://blog.xamarin.com/exploring-new-ios-12-arkit-capabilities-with-xamarin/)で説明します。

## <a name="other-new-features-in-arkit-2"></a>ARKit 2 のその他の新機能

### <a name="more-tracking-configurations"></a>その他の追跡構成

これで、mixed reality エクスペリエンスの基礎として、次のいずれかを使用できるようになりました。

- デバイス加速度計のみ ([`AROrientationTrackingConfiguration`](xref:ARKit.AROrientationTrackingConfiguration)、iOS 11)
- 顔 ([`ARFaceTrackingConfiguration`](xref:ARKit.ARFaceTrackingConfiguration)、iOS 11)
- 参照イメージ ([`ARImageTrackingConfiguration`](xref:ARKit.ARImageTrackingConfiguration)、iOS 12)
- 3d オブジェクトのスキャン[`ARObjectScanningConfiguration`](xref:ARKit.ARObjectScanningConfiguration)(、iOS 12)
- ビジュアル慣性 odometry ([`ARWorldTrackingConfiguration`](xref:ARKit.ARWorldTrackingConfiguration)、iOS 12 で改良)

`AROrientationTrackingConfiguration`[このブログの投稿とF#サンプル](https://github.com/lobrien/FSharp_Face_AR)で説明されているのは、最も限定的であり、あまり複雑ではありません。これは、デバイスと画面を実際の世界に結び付けずに、デジタルオブジェクトをデバイスの動きに関連付けて配置するだけであるためです。

で`ARImageTrackingConfiguration`は、現実世界の2d イメージ (描い、ロゴなど) を認識し、それらを使用してデジタル画像を固定することができます。

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

この構成には、次の2つの興味深い側面があります。

- 効率的であり、多くの参照イメージで使用できます。
- デジタル画像は画像に固定されています。実際の世界でその画像が動く場合でも (たとえば、本の表紙が認識された場合、棚から引き出したり、下に並べられているときには、本を追跡します)。

「 `ARObjectScanningConfiguration` 」では、[前述](#recognizing-reference-objects)の「3d オブジェクトをスキャンするための開発者向けの構成」を説明しました。 プロセッサとバッテリが大量に消費されるため、エンドユーザーアプリケーションでは使用しないでください。 「 [3D オブジェクトのスキャンと検出](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-scanninganddetecting3dobjects)」のサンプルでは、この構成の使用方法を示しています。

最終的な追跡構成 ( `ARWorldTrackingConfiguration` ) は、ほとんどの混合環境での主力製品です。 この構成では、"ビジュアル慣性 odometry" を使用して、実際の "特徴ポイント" をデジタル画像に関連付けます。 デジタルジオメトリまたはスプライトは、現実世界の水平方向と垂直方向の面、また`ARReferenceObject`は検出されたインスタンスに対して相対的に固定されます。 この構成では、世界の原点は、Z 軸が重力に調整された領域内のカメラの元の位置であり、デジタルオブジェクトは実際の世界のオブジェクトを基準としています。

### <a name="environmental-texturing"></a>環境テクスチャ

ARKit 2 では、キャプチャした画像を使用して光源を推定し、光沢のあるオブジェクトに反射の光源を適用する "環境テクスチャ" をサポートしています。 環境キューブマップは動的に構築され、カメラがすべての方向に見られると、印象的の現実的なエクスペリエンスを生み出すことができます。

![環境テクスチャのデモイメージ](images/arkit_env_texturing.png)

環境テクスチャを使用するには:

- オブジェクトはを使用[`SCNLightingModel.PhysicallyBased`](xref:SceneKit.SCNLightingModel.PhysicallyBased)して[`Metalness.Contents`](xref:SceneKit.SCNMaterial.Metalness) 、0 ~ 1 の範囲の値を and [`Roughness.Contents`](xref:SceneKit.SCNMaterialProperty.Contents)で代入する必要があります。 [`SCNMaterial`](xref:SceneKit.SCNMaterial)
- 追跡構成では、 [`EnvironmentTexturing`](xref:ARKit.ARWorldTrackingConfiguration.EnvironmentTexturing)次の設定 =  [`AREnvironmentTexturing.Automatic`](xref:ARKit.AREnvironmentTexturing.Automatic)が必要です。

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

前のコードスニペットに示されている完璧な反射のテクスチャは、サンプルでは楽しいものですが、"uncanny バレー" の応答をトリガーする環境テクスチャを使用することをお勧めします (テクスチャはカメラの内容に基づいた推定のみです)。記録済み)。


### <a name="shared-and-persistent-ar-experiences"></a>共有と永続的な AR エクスペリエンス

Arkit 2 に追加された[`ARWorldMap`](xref:ARKit.ARWorldMap)もう1つの主な機能は、クラスです。このクラスを使用すると、世界の追跡データを共有したり、保存したりできます。 現在のワールドマップを取得する[`ARSession.GetCurrentWorldMapAsync`](xref:ARKit.ARSession.GetCurrentWorldMapAsync)に[`GetCurrentWorldMap(Action<ARWorldMap,NSError>)`](xref:ARKit.ARSession.GetCurrentWorldMap(System.Action{ARKit.ARWorldMap,Foundation.NSError}))は、またはを使用します。

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

ワールドマップを共有または復元するには、次のようにします。

1. ファイルからデータを読み込みます。
2. `ARWorldMap`オブジェクトへのアーカイブを解除します。
3. これを[`ARWorldTrackingConfiguration.InitialWorldMap`](xref:ARKit.ARWorldTrackingConfiguration.InitialWorldMap)プロパティの値として使用します。

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

に`ARWorldMap`は、非表示の世界追跡データ[`ARAnchor`](xref:ARKit.ARAnchor)とオブジェクトのみが含まれており、デジタル資産は含まれて_いません_。 ジオメトリや画像を共有するには、ユースケースに適した独自の戦略を作成する必要があります (たとえば、ジオメトリの位置と向きのみを格納/転送し、 `SCNGeometry`それを静的に適用したり、格納/転送によって適用したりするなど)。シリアル化されたオブジェクト)。 の`ARWorldMap`利点は、共有`ARAnchor`に対して相対的に配置された資産が、デバイス間またはセッション間で一貫して表示されることです。

### <a name="universal-scene-description-file-format"></a>汎用シーンの説明ファイルの形式

ARKit 2 の最後のヘッドライン機能は、Apple が Pixar の[汎用シーン記述](https://graphics.pixar.com/usd/docs/Introduction-to-USD.html)ファイル形式を採用していることを示しています。 この形式は、ARKit アセットを共有および格納するための適切な形式として、使用しているメディアの DAE 形式を置き換えます。 資産の視覚化のサポートは、iOS 12 と Mojave に組み込まれています。 USDZ ファイル拡張機能は、米国ドルのファイルを含む、圧縮されていない暗号化されていない zip アーカイブです。 Pixar は、[米ドルのファイルを操作するためのツールを提供](https://graphics.pixar.com/usd/docs/USD-Toolset.html#USDToolset-usdedit)していますが、まだサードパーティのサポートはあまりありません。

## <a name="arkit-programming-tips"></a>ARKit のプログラミングに関するヒント

### <a name="manual-resource-management"></a>手動リソース管理

ARKit では、リソースを手動で管理することが重要です。 これによってフレームレートが高くなるだけでなく、実際には混乱を避ける_必要_があります。 ARKit フレームワークは、新しいカメラフレーム ([`ARSession.CurrentFrame`](xref:ARKit.ARSession.CurrentFrame). 現在[`ARFrame`](xref:ARKit.ARFrame) の`Dispose()`が呼び出されるまで、arkit は新しいフレームを提供しません。 これにより、アプリの残りの部分が応答する場合でもビデオが "フリーズ" されます。 解決策としては`ARSession.CurrentFrame` 、常`using`にブロックを使用`Dispose()`してアクセスするか、手動で呼び出す必要があります。

から`NSObject`派生したすべて`IDisposable`の`NSObject`オブジェクトはであり、 [Dispose パターン](https://docs.microsoft.com/dotnet/standard/design-guidelines/dispose-pattern)を実装します。そのため、通常、このパターンに従って、[派生クラスにを実装`Dispose` ](https://docs.microsoft.com/dotnet/standard/garbage-collection/implementing-dispose)する必要があります。

### <a name="manipulating-transform-matrices"></a>変換行列の操作

どの3D アプリケーションでも、4×4の変換行列を扱うことになります。コンパクトは、3D 空間を使用してオブジェクトを移動、回転、傾斜させる方法を説明します。 SceneKit では、これら[`SCNMatrix4`](xref:SceneKit.SCNMatrix4)はオブジェクトです。  

プロパティ[`SCNNode.Transform`](xref:SceneKit.SCNNode.Transform)は、 `SCNMatrix4`行メジャー _型によってサポート_されるの変換行列を返します。 `simdfloat4x4` [`SCNNode`](xref:SceneKit.SCNNode) だから例えば：

```csharp
var node = new SCNNode { Position = new SCNVector3(2, 3, 4) };  
var xform = node.Transform;
Console.WriteLine(xform);
// Output is: "(1, 0, 0, 0)\n(0, 1, 0, 0)\n(0, 0, 1, 0)\n(2, 3, 4, 1)"
```  

ご覧のように、位置は一番下の行の最初の3つの要素にエンコードされています。

Xamarin では、変換行列を操作するための`NVector4`共通の型はであり、慣例によって、列の主要な方法で解釈されます。 つまり、M14、M24、M34、M41、M42、M43 ではなく平行移動/位置コンポーネントが想定されているとします。

![row-major と縦棒-major](images/arkit_row_vs_column.png)

適切な動作を行うには、マトリックスの解釈の選択と一貫性があることが重要です。 3D 変換行列は4×4であるため、一貫性の間違いによってコンパイル時や実行時の例外が生成されることはありません。操作が予期せず動作するだけです。 SceneKit/ARKit オブジェクトがスタックしている、飛んでいる、またはジッターであるように見える場合は、正しくない変換行列が適している可能性があります。 ソリューションは単純です。 [`NMatrix4.Transpose`](xref:OpenTK.NMatrix4.Transpose*)では、要素のインプレース転調が実行されます。

## <a name="related-links"></a>関連リンク

- [サンプルアプリ-3D オブジェクトのスキャンと検出](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-scanninganddetecting3dobjects)
- [ARKit 2 の新機能 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/602/)
- [ARKit の追跡と検出について (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/610/)
- [Xamarin の ARKit の概要](https://docs.microsoft.com/xamarin/ios/platform/introduction-to-ios11/arkit/)
