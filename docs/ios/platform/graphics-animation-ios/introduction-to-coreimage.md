---
title: CoreImage
description: "CoreImage は、iOS 5 をイメージ処理を提供し、ライブ ビデオの拡張機能で導入された新しいフレームワークです。 この記事では、Xamarin.iOS サンプルを使ってこれらの機能を紹介します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 91E0780B-FF8A-E70D-9CD4-419119612B2D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 2d9c2b27de7addc0ed1faeed038db81e2470087f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="coreimage"></a>CoreImage

_CoreImage は、iOS 5 をイメージ処理を提供し、ライブ ビデオの拡張機能で導入された新しいフレームワークです。この記事では、Xamarin.iOS サンプルを使ってこれらの機能を紹介します。_

CoreImage は、さまざまな組み込みのフィルターと画像とビデオ、顔の検出などに適用する効果を提供する iOS 5 で導入された新しいフレームワークです。

このドキュメントには、簡単な例が含まれています。

-  検出に直面してください。
-  イメージにフィルターを適用します。
-  使用できるフィルターを一覧表示します。


これらの例は、Xamarin.iOS アプリケーションに CoreImage 機能を組み込むことを開始するのに役立ちます。

## <a name="requirements"></a>必要条件

Xcode の最新バージョンを使用する必要があります。

## <a name="face-detection"></a>顔の検出

CoreImage 顔の検出機能の特長だけ何書かれている – 写真の顔を特定しようとして認識されているすべての面の座標を返します。 この情報は、イメージ内のユーザーの数をカウント、(イメージ上のインジケーターの描画に使用できます。 'タグの' 人写真で)、またはその他の考えることができます。

CoreImage\SampleCode.cs から次のコードでは、作成し、埋め込み画像の顔の検出を使用する方法を示します。

```csharp
var image = new UIImage("photoFace.JPG");
var context = CIContext.FromOptions(null);
var detector = CIDetector.CreateFaceDetector (context, true);
var ciImage = CIImage.FromCGImage(image.CGImage);
CIFeature[] features = detector.FeaturesInImage(ciImage);
```

機能の配列が設定されます`CIFaceFeature`オブジェクト (すべての面が検出されなかった) 場合。 `CIFaceFeature`各面です。 `CIFaceFeature` 次のプロパティがあります。

-  HasMouthPosition – この面が、口が検出されたかどうか。
-  HasLeftEyePosition – この面が左側の目が検出されたかどうか。
-  HasRightEyePosition – この面が右の目が検出されたかどうか。 
-  MouthPosition – この面の口の座標。
-  LeftEyePosition – この面の左、目の座標。
-  RightEyePosition – この面の右側の目の座標。


これらすべてのプロパティの座標では、下部左に – UIKit 原点として左上を使用するとは異なり、origin があります。 座標を使用するとき`CIFaceFeature`'反転' にしてください。 CoreImage\CoreImageViewController.cs でこのカスタム イメージが非常に基本的なビューが、イメージに 'フェイス インジケーター' 三角形を描画する方法を示します (注、`FlipForBottomOrigin`メソッド)。

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

SampleCode.cs ファイルで、イメージと機能が割り当てられます、イメージが再描画される前に。

```csharp
faceView.Image = image;
faceView.Features = features;
faceView.SetNeedsDisplay();
```

スクリーン ショットは、出力の例を示しています。 検出されたの顔の特徴の場所は、UITextView で表示され、CoreGraphics を使用して、ソース イメージ上に描画します。

方法により、顔認識が機能するかは場合によっては検出 (これら toy 猿!) のようなヒューマン面ほかに、処理します。

## <a name="filters"></a>フィルター

50 以上の異なる組み込みフィルターがあるし、フレームワークは、新しいフィルターを実装できるように拡張します。

## <a name="using-filters"></a>フィルターを使用します。

次の 4 つの個別のステップがイメージにフィルターを適用する: イメージを読み込み、フィルターを作成して、フィルターを適用して保存 (またはを表示する) 結果。

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

3 番目に、アクセス、`OutputImage`プロパティと呼び出し、`CreateCGImage`最終的な結果を表示するメソッド。

```csharp
CIImage output = sepia.OutputImage;
var context = CIContext.FromOptions(null);
var cgimage = context.CreateCGImage (output, output.Extent);
```

最後に、結果を表示するイメージを割り当てます。 実際のアプリケーション、filesystem、フォト アルバム、ツイートまたは電子メールに、結果のイメージを保存する可能性があります。

```csharp
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

これらのスクリーン ショットの結果を表示する、`CISepia`と`CIHueAdjust`CoreImage.zip で紹介されているフィルターのサンプル コードです。

参照してください、[コントラクトの調整およびイメージ レシピの明るさ](https://developer.xamarin.com/recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image)の例については、`CIColorControls`フィルター。

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

CoreImage\SampleCode.cs から次のコードは、組み込みのフィルターの完全な一覧とそのパラメーターを出力します。

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

[CIFilter クラス参照](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.html)50 の組み込みのフィルターとそのプロパティについて説明します。 上記のコードを使用すると、パラメーターの既定値と最大値と最小許容値 (フィルターを適用する前に入力の検証に使用できます) を含む、フィルター クラスを照会できます。

リストのカテゴリの出力は、シミュレーターでは次のよう – すべてのフィルターとそのパラメーターを表示するリストをスクロールすることができます。

 [ ![](introduction-to-coreimage-images/coreimage05.png "リストのカテゴリの出力は、シミュレーターでは次のよう")](introduction-to-coreimage-images/coreimage05.png)

表示されている各フィルターは、アセンブリ ブラウザーまたは Mac 用の Visual Studio または Visual Studio のいずれかでオートコンプリートを使用して、Xamarin.iOS.CoreImage API を表示することも、Xamarin.iOS、内のクラスとして公開されています。 

## <a name="summary"></a>まとめ

この記事では、いくつかの顔の検出とイメージにフィルターを適用するように、新しい iOS 5 CoreImage framework 機能を使用する方法を説明しました。 多数の異なるイメージ フィルターを使用するためのフレームワークで使用できます。

## <a name="related-links"></a>関連リンク

- [Core イメージ (サンプル)](https://developer.xamarin.com/samples/CoreImage/)
- [コントラクトおよびイメージ レシピの明るさを調整します。](https://developer.xamarin.com/recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image)
- [CoreImage フィルターを使用します。](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Conceptual/CoreImaging/ci_tasks/ci_tasks.html)
- [CIFilter クラスのリファレンス](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.htm)
