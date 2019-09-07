---
title: Xamarin のコアイメージ
description: コアイメージは、iOS 5 で導入された新しいフレームワークであり、イメージ処理機能とライブビデオ拡張機能を提供します。 この記事では、Xamarin のサンプルでこれらの機能について説明します。
ms.prod: xamarin
ms.assetid: 91E0780B-FF8A-E70D-9CD4-419119612B2D
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/19/2017
ms.openlocfilehash: ffaa6553830a64589818c991e8f729ff7232e367
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70752841"
---
# <a name="core-image-in-xamarinios"></a>Xamarin のコアイメージ

_コアイメージは、iOS 5 で導入された新しいフレームワークであり、イメージ処理機能とライブビデオ拡張機能を提供します。この記事では、Xamarin のサンプルでこれらの機能について説明します。_

コアイメージは、iOS 5 で導入された新しいフレームワークであり、顔検出など、イメージやビデオに適用するさまざまな組み込みのフィルターおよび効果を提供します。

このドキュメントには、次の簡単な例が含まれています。

- 顔検出。
- 画像へのフィルターの適用
- 使用可能なフィルターを一覧表示します。

これらの例では、Xamarin の iOS アプリケーションにコアイメージ機能を組み込む方法について説明します。

## <a name="requirements"></a>必要条件

Xcode の最新バージョンを使用する必要があります。

## <a name="face-detection"></a>顔検出

主要な画像の顔検出機能では、写真内の顔を識別し、認識している顔の座標を返すことを試みます。 この情報を使用して、イメージ内の人数をカウントしたり、画像にインジケーターを描画したりすることができます (例として、 写真に "タグを付けている"、または他の人が考えてみてください。

この CoreImage\SampleCode.cs のコードは、埋め込み画像で顔検出を作成して使用する方法を示しています。

```csharp
var image = new UIImage("photoFace.JPG");
var context = CIContext.FromOptions(null);
var detector = CIDetector.CreateFaceDetector (context, true);
var ciImage = CIImage.FromCGImage(image.CGImage);
CIFeature[] features = detector.FeaturesInImage(ciImage);
```

特徴配列には、 `CIFaceFeature`オブジェクト (任意の顔が検出された場合) が設定されます。 各面`CIFaceFeature`にはがあります。 `CIFaceFeature`には、次のプロパティがあります。

- HasMouthPosition –この顔で口が検出されたかどうかを指定します。
- HasLeftEyePosition –この顔に対して左目が検出されたかどうかを指定します。
- HasRightEyePosition –この顔に対して、右目が検出されたかどうかを指定します。 
- 笑顔の位置–この顔の口の座標。
- LeftEyePosition –この顔の左側の視点の座標です。
- RightEyePosition –この顔の右目の座標です。

これらのすべてのプロパティの座標の原点は、左上にあります。これは、左上を原点として使用する UIKit とは異なります。 座標を使用する場合`CIFaceFeature`は、必ず ' 反転 ' してください。 CoreImage\CoreImageViewController.cs のこの非常に基本的なカスタムイメージビューでは、イメージに ' face indicator ' 三角形を描画する`FlipForBottomOrigin`方法を示しています (メソッドに注意してください)。

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

次に、SampleCode.cs ファイルで、イメージが再描画される前にイメージと機能が割り当てられます。

```csharp
faceView.Image = image;
faceView.Features = features;
faceView.SetNeedsDisplay();
```

スクリーンショットは、サンプル出力を示しています。検出された顔機能の場所は、UITextView に表示され、CoreGraphics を使用してソースイメージに描画されます。

顔認識のしくみによって、人の顔以外のものも検出されることがあります (このようなおもちゃの猿!)。

## <a name="filters"></a>フィルター

50の異なる組み込みフィルターがあり、新しいフィルターを実装できるように、フレームワークは拡張可能です。

## <a name="using-filters"></a>フィルターの使用

画像にフィルターを適用するには、イメージの読み込み、フィルターの作成、フィルターの適用、結果の保存 (または表示) という4つの手順を実行します。

まず、イメージを`CIImage`オブジェクトに読み込みます。

```csharp
var uiimage = UIImage.FromFile ("photo.JPG");
var ciimage = new CIImage (uiimage);
```

次に、フィルタークラスを作成し、そのプロパティを設定します。

```csharp
var sepia = new CISepiaTone();
sepia.Image = ciimage;
sepia.Intensity = 0.8f;
```

3番目に`OutputImage` 、プロパティにアクセス`CreateCGImage`し、メソッドを呼び出して最終的な結果を表示します。

```csharp
CIImage output = sepia.OutputImage;
var context = CIContext.FromOptions(null);
var cgimage = context.CreateCGImage (output, output.Extent);
```

最後に、画像をビューに割り当てて、結果を表示します。 実際のアプリケーションでは、生成されたイメージは、ファイルシステム、フォトアルバム、ツイート、または電子メールに保存される可能性があります。

```csharp
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

これらのスクリーンショットは、coreimage `CIHueAdjust` . .zip サンプルコードに示されているフィルター `CISepia`とフィルターの結果を示しています。

`CIColorControls`フィルターの例については、「[イメージレシピのコントラクトと明るさの調整](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image)」を参照してください。

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

### <a name="listing-filters-and-their-properties"></a>フィルターとそのプロパティの一覧表示

CoreImage\SampleCode.cs からのこのコードは、組み込みフィルターとそのパラメーターの完全な一覧を出力します。

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

[Cifilter クラスのリファレンス](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.html)では、50の組み込みフィルターとそのプロパティについて説明します。 上記のコードを使用すると、パラメーターの既定値や許容される最大値 (フィルターを適用する前に入力を検証するために使用される可能性がある) を含むフィルタークラスに対してクエリを実行できます。

リストのカテゴリの出力はシミュレーターでは次のようになります。リストをスクロールして、すべてのフィルターとそのパラメーターを表示できます。

 [![](introduction-to-coreimage-images/coreimage05.png "リストのカテゴリの出力は、シミュレーターでは次のようになります。")](introduction-to-coreimage-images/coreimage05.png#lightbox)

一覧表示されている各フィルターは、Xamarin. iOS のクラスとして公開されています。そのため、アセンブリブラウザーで Xamarin. iOS. CoreImage API を探索したり、Visual Studio for Mac または Visual Studio でオートコンプリートを使用したりすることもできます。 

## <a name="summary"></a>Summary

この記事では、顔検出や画像へのフィルターの適用など、新しい iOS 5 コアイメージフレームワークの機能の一部を使用する方法について説明しました。 フレームワークで使用できるイメージフィルターは多数あります。

## <a name="related-links"></a>関連リンク

- [コアイメージ (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/coreimage)
- [イメージレシピのコントラクトと明るさを調整する](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image)
- [コアイメージフィルターの使用](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Conceptual/CoreImaging/ci_tasks/ci_tasks.html)
- [CIFilter クラス参照](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.htm)
