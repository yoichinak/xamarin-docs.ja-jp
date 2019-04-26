---
title: Xamarin.iOS で core イメージ
description: Core のイメージは、iOS 5 イメージ処理を提供し、ライブ ビデオの拡張機能で導入された新しいフレームワークです。 この記事では、Xamarin.iOS サンプルを使ってこれらの機能について説明します。
ms.prod: xamarin
ms.assetid: 91E0780B-FF8A-E70D-9CD4-419119612B2D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: d71f14c26865b71eca991910df4a68f2540f9715
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61085575"
---
# <a name="core-image-in-xamarinios"></a>Xamarin.iOS で core イメージ

_Core のイメージは、iOS 5 イメージ処理を提供し、ライブ ビデオの拡張機能で導入された新しいフレームワークです。この記事では、Xamarin.iOS サンプルを使ってこれらの機能について説明します。_

Core のイメージは、さまざまな組み込みのフィルターと画像および顔検出を含むビデオに適用する効果を提供する iOS 5 で導入された新しいフレームワークです。

このドキュメントには、簡単な例が含まれています。

-  顔検出します。
-  イメージにフィルターを適用
-  使用可能なフィルターを一覧表示します。


これらの例は、Xamarin.iOS アプリケーションにコア イメージ機能を組み込む作業を開始するヘルプする必要があります。

## <a name="requirements"></a>必要条件

Xcode の最新バージョンを使用する必要があります。

## <a name="face-detection"></a>顔検出

Core のイメージの顔検出機能は名前だけ – 写真で顔を特定しようとし、認識しているすべての面の座標を返します。 この情報は、イメージ内のユーザーの数をカウント、インジケーターを (例: イメージの描画に使用できます。 'タグの' people 写真で)、またはその他考えることができます。

CoreImage\SampleCode.cs から次のコードでは、作成し、埋め込み画像の顔検出を使用する方法を示しています。

```csharp
var image = new UIImage("photoFace.JPG");
var context = CIContext.FromOptions(null);
var detector = CIDetector.CreateFaceDetector (context, true);
var ciImage = CIImage.FromCGImage(image.CGImage);
CIFeature[] features = detector.FeaturesInImage(ciImage);
```

機能の配列を伴って`CIFaceFeature`オブジェクト (すべての顔が検出されなかった) 場合。 `CIFaceFeature`の顔ごと。 `CIFaceFeature` 次のプロパティがあります。

-  HasMouthPosition – この顔を口が検出されたかどうか。
-  HasLeftEyePosition – 左目がこの顔の検出されたかどうか。
-  HasRightEyePosition – この面の適切な目が検出されたかどうか。 
-  MouthPosition – この顔口の座標。
-  LeftEyePosition – この面の左目の座標。
-  RightEyePosition – この面の適切な目の座標。


これらすべてのプロパティの座標では、左下 – UIKit の原点として左上を使用するとは異なりにその出所があります。 座標を使用して`CIFaceFeature`'反転' にしてください。 CoreImage\CoreImageViewController.cs でこのカスタム イメージを非常に基本的なビューがイメージに 'face インジケーター' 三角形を描画する方法を示します (注、`FlipForBottomOrigin`メソッド)。

```csharp
public class FaceDetectImageView : UIView
{
    public Xamarin.iOS.CoreImage.CIFeature[] Features;
    public UIImage Image;
    public FaceDetectImageView (RectangleF rect) : base(rect) {}
    CGPath path;
    public override void Draw (RectangleF rect) {
        base.Draw (rect);
        if (Image != null)
            Image.Draw(rect);

        using (var context = UIGraphics.GetCurrentContext()) {
            context.SetLineWidth(4);
            UIColor.Red.SetStroke ();
            UIColor.Clear.SetFill ();
            if (Features != null) {
                foreach (var feature in Features) { // for each face
                    var facefeature = (CIFaceFeature)feature;
                    path = new CGPath ();
                    path.AddLines(new PointF[]{ // assumes all 3 features found
                        FlipForBottomOrigin(facefeature.LeftEyePosition, 200),
                        FlipForBottomOrigin(facefeature.RightEyePosition, 200),
                        FlipForBottomOrigin(facefeature.MouthPosition, 200)
                    });
                    path.CloseSubpath();
                    context.AddPath(path);
                    context.DrawPath(CGPathDrawingMode.FillStroke);
                }
            }
        }
    }
    /// <summary>
    /// Face recognition coordinates have their origin in the bottom-left
    /// but we are drawing with the origin in the top-left, so "flip" the point
    /// </summary>
    PointF FlipForBottomOrigin (PointF point, int height)
    {
        return new PointF(point.X, height - point.Y);
    }
}
```

SampleCode.cs ファイルでイメージと機能が割り当てられているイメージが再描画される前に。

```csharp
faceView.Image = image;
faceView.Features = features;
faceView.SetNeedsDisplay();
```

スクリーン ショットのサンプル出力: 検出された顔の特徴の場所は、UITextView で表示され、CoreGraphics を使用してソース イメージの上に描画します。

顔認識では動作に起因は (これらおもちゃ猿!) などの人間の顔のほかに、処理の検出が場合があります。

## <a name="filters"></a>フィルター

50 以上のさまざまな組み込みフィルターがあるし、フレームワークは、新しいフィルターを実装できるように拡張可能です。

## <a name="using-filters"></a>フィルターの使用

4 つの手順には、イメージにフィルターを適用: イメージの読み込み、フィルターを作成、フィルターを適用して保存 (または表示する) の結果。

最初にイメージを読み込む、`CIImage`オブジェクト。

```csharp
var uiimage = UIImage.FromFile ("photo.JPG");
var ciimage = new CIImage (uiimage);
```

次に、フィルター クラスを作成し、そのプロパティを設定します。

```csharp
var sepia = new CISepiaTone();
sepia.Image = ciimage;
sepia.Intensity = 0.8f;
```

3 番目に、アクセス、`OutputImage`プロパティと呼び出し、`CreateCGImage`最終的な結果をレンダリングするメソッド。

```csharp
CIImage output = sepia.OutputImage;
var context = CIContext.FromOptions(null);
var cgimage = context.CreateCGImage (output, output.Extent);
```

最後に、結果を表示するビューに、イメージを割り当てます。 実際のアプリケーション、ファイル システム、フォト アルバム、ツイートまたは電子メールに、結果のイメージを保存する可能性があります。

```csharp
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

これらのスクリーン ショットの結果を表示する、`CISepia`と`CIHueAdjust`CoreImage.zip で紹介されているフィルターのサンプル コード。

参照してください、[コントラクトを調整して、イメージのレシピの明るさ](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image)の例については、`CIColorControls`フィルター。

```csharp
var uiimage = UIImage.FromFile("photo.JPG");
var ciimage = new CIImage(uiimage);
var hueAdjust = new CIHueAdjust();   // first filter
hueAdjust.Image = ciimage;
hueAdjust.Angle = 2.094f;
var sepia = new CISepiaTone();       // second filter
sepia.Image = hueAdjust.OutputImage; // output from last filter, input to this one
sepia.Intensity = 0.3f;
CIFilter color = new CIColorControls() { // third filter
    Saturation = 2,
    Brightness = 1,
    Contrast = 3,
    Image = sepia.OutputImage    // output from last filter, input to this one
};
var output = color.OutputImage;
var context = CIContext.FromOptions(null);
// ONLY when CreateCGImage is called do all the effects get rendered
var cgimage = context.CreateCGImage (output, output.Extent);
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

```csharp
var context = CIContext.FromOptions (null);
```

```csharp
var context = CIContext.FromOptions(new CIContextOptions() {
    UseSoftwareRenderer = true  // CPU
});
var cgimage = context.CreateCGImage (output, output.Extent);
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

### <a name="listing-filters-and-their-properties"></a>フィルターとそのプロパティを一覧表示します。

CoreImage\SampleCode.cs から次のコードでは、組み込みのフィルターの完全な一覧とそのパラメーターを出力します。

```csharp
var filters = CIFilter.FilterNamesInCategories(new string[0]);
foreach (var filter in filters){
   display.Text += filter +"\n";
   var f = CIFilter.FromName (filter);
   foreach (var key in f.InputKeys){
     var attributes = (NSDictionary)f.Attributes[new NSString(key)];
     var attributeClass = attributes[new NSString("CIAttributeClass")];
     display.Text += "   " + key;
     display.Text += "   " + attributeClass + "\n";
   }
}
```

[CIFilter クラス参照](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.html)50 の組み込みのフィルターとそのプロパティについて説明します。 上記のコードを使用してパラメーターの既定値、最大と最小許容値 (フィルターを適用する前に入力の検証に使用できます) など、フィルター クラスを照会できます。

リスト カテゴリ出力次のようにシミュレーター – すべてのフィルターとそのパラメーターを表示するリストをスクロールすることができます。

 [![](introduction-to-coreimage-images/coreimage05.png "リスト カテゴリ出力次のようにシミュレーター")](introduction-to-coreimage-images/coreimage05.png#lightbox)

表示されている各フィルターは、アセンブリ ブラウザーまたは Visual Studio for Mac または Visual Studio のいずれかでオートコンプリートを使用して、Xamarin.iOS.CoreImage API を利用できるように、Xamarin.iOS、内のクラスとして公開されています。 

## <a name="summary"></a>まとめ

この記事では、顔の検出とイメージにフィルターを適用するように、新しい iOS 5 Core イメージ framework 機能の一部を使用する方法を説明しました。 使用するためのフレームワークで使用できる多数のさまざまなイメージ フィルターされます。

## <a name="related-links"></a>関連リンク

- [Core のイメージ (サンプル)](https://developer.xamarin.com/samples/CoreImage/)
- [コントラクトと、イメージのレシピの明るさを調整します。](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image)
- [コア イメージ フィルターを使用します。](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Conceptual/CoreImaging/ci_tasks/ci_tasks.html)
- [CIFilter クラスのリファレンス](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.htm)
