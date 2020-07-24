---
title: Xamarin のコアグラフィック
description: この記事では、グラフィックスの主要な iOS フレームワークについて説明します。 ここでは、コアグラフィックスを使用して、ジオメトリ、画像、Pdf を描画する方法を示します。
ms.prod: xamarin
ms.assetid: 4A30F480-0723-4B8A-9049-7CEB6211304A
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 84e21378a8ac7b61bc1a389352eb53b75881592a
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86929650"
---
# <a name="core-graphics-in-xamarinios"></a>Xamarin のコアグラフィック

_この記事では、グラフィックスの主要な iOS フレームワークについて説明します。ここでは、コアグラフィックスを使用して、ジオメトリ、画像、Pdf を描画する方法を示します。_

iOS には、低レベルの描画をサポートするための[*コアグラフィックス*](https://developer.apple.com/library/prerelease/ios/documentation/CoreGraphics/Reference/CoreGraphics_Framework/index.html)フレームワークが含まれています。 これらのフレームワークによって、UIKit 内の豊富なグラフィカル機能が有効になります。

コアグラフィックスは、デバイスに依存しないグラフィックスを描画できる低レベルの2D グラフィックスフレームワークです。 UIKit のすべての2D 描画は、内部的にコアグラフィックスを使用します。

コアグラフィックスは、次のようなさまざまなシナリオでの描画をサポートしています。

- [を `UIView` 使用して画面に描画する](#Drawing_in_a_UIView_Subclass).
- [メモリまたは画面にイメージを描画](#Drawing_Images_and_Text)します。
- PDF の作成と描画。
- 既存の PDF の読み取りと描画。

## <a name="geometric-space"></a>幾何学的空間

このシナリオに関係なく、コアグラフィックスで実行されるすべての描画はジオメトリック空間で実行されます。これは、ピクセルではなく抽象ポイントで動作することを意味します。 ジオメトリと描画の状態 (色、線のスタイルなど) で描画するものを記述します。また、コアグラフィックスはすべてをピクセルに変換します。 このような状態はグラフィックスコンテキストに追加されます。これは、塗装のキャンバスのように考えることができます。

このアプローチにはいくつかの利点があります。

- 描画コードは動的になり、後で実行時にグラフィックスを変更できます。
- アプリケーションバンドルの静的イメージの必要性を減らすと、アプリケーションのサイズが小さくなる可能性があります。
- グラフィックスは、デバイス間の解像度の変更に対して、より高い回復力を持ちます。

<a name="Drawing_in_a_UIView_Subclass"></a>

## <a name="drawing-in-a-uiview-subclass"></a>UIView サブクラスでの描画

すべてのには、 `UIView` `Draw` 描画する必要があるときにシステムによって呼び出されるメソッドがあります。 描画コードをビューに追加するには、次のようにサブクラス `UIView` を作成してオーバーライドし `Draw` ます。

```csharp
public class TriangleView : UIView
{
    public override void Draw (CGRect rect)
    {
        base.Draw (rect);
    }
}
```

描画を直接呼び出すことはできません。 これは、実行ループ処理中にシステムによって呼び出されます。 ビューがビュー階層に追加された後に run ループを初めて実行すると、その `Draw` メソッドが呼び出されます。 後続のの呼び出しは、ビュー `Draw` にまたはのいずれかを呼び出すことによって描画する必要があるとしてマークされている場合に発生し `SetNeedsDisplay` `SetNeedsDisplayInRect` ます。

### <a name="pattern-for-graphics-code"></a>グラフィックスコードのパターン

実装内のコードでは、 `Draw` 描画する内容を記述する必要があります。 描画コードは、いくつかの描画状態を設定し、メソッドを呼び出して描画を要求するパターンに従います。 このパターンは、次のように一般化できます。

1. グラフィックスコンテキストを取得します。

2. 描画属性を設定します。

3. 描画プリミティブからいくつかのジオメトリを作成します。

4. 描画またはストロークメソッドを呼び出します。

### <a name="basic-drawing-example"></a>基本的な描画の例

たとえば、次のコード スニペットを考えてみます。

```csharp
//get graphics context
using (CGContext g = UIGraphics.GetCurrentContext ()) {

    //set up drawing attributes
    g.SetLineWidth (10);
    UIColor.Blue.SetFill ();
    UIColor.Red.SetStroke ();

    //create geometry
    var path = new CGPath ();

    path.AddLines (new CGPoint[]{
    new CGPoint (100, 200),
    new CGPoint (160, 100),
    new CGPoint (220, 200)});

    path.CloseSubpath ();

    //add geometry to graphics context and draw it
    g.AddPath (path);
    g.DrawPath (CGPathDrawingMode.FillStroke);
}
```

このコードを中断してみましょう。

```csharp
using (CGContext g = UIGraphics.GetCurrentContext ()) {
...
}
```

この行では、最初に描画に使用する現在のグラフィックスコンテキストを取得します。 描画を行うキャンバスとしてグラフィックスコンテキストを考えることができます。これには、ストロークや塗りつぶしの色、描画するジオメトリなど、描画に関するすべての状態が含まれます。

```csharp
g.SetLineWidth (10);
UIColor.Blue.SetFill ();
UIColor.Red.SetStroke ();
```

グラフィックスコンテキストを取得した後は、上に示したように、コードで描画時に使用するいくつかの属性を設定します。 この場合、線の幅、ストローク、塗りつぶしの色が設定されます。 それ以降の描画では、これらの属性がグラフィックスコンテキストの状態で維持されるため、これらの属性が使用されます。

Geometry を作成するには、コードでを使用し `CGPath` ます。これにより、直線と曲線からグラフィックスパスを記述できるようになります。 この場合、パスによって、三角形を構成する点の配列を結ぶ線が追加されます。 次に示すように、コアグラフィックスでは、原点が左上にあり、正の x-右に正の y 方向が下になる、ビューの描画に座標系が使用されます。

```csharp
var path = new CGPath ();

path.AddLines (new CGPoint[]{
new CGPoint (100, 200),
new CGPoint (160, 100),
new CGPoint (220, 200)});

path.CloseSubpath ();
```

パスが作成されると、とを呼び出すことができるように、そのパスがグラフィックスコンテキストに追加され `AddPath` `DrawPath` ます。

結果のビューは次のようになります。

 ![サンプルの出力三角形](core-graphics-images/00-bluetriangle.png)

## <a name="creating-gradient-fills"></a>グラデーションの塗りつぶしの作成

より豊富な形式の描画も利用できます。 たとえば、コアグラフィックスでは、グラデーションの塗りつぶしを作成したり、クリッピングパスを適用したりできます。 前の例のパスの内側にグラデーションの塗りつぶしを描画するには、まずパスをクリッピングパスとして設定する必要があります。

```csharp
// add the path back to the graphics context so that it is the current path
g.AddPath (path);
// set the current path to be the clipping path
g.Clip ();
```

現在のパスをクリッピングパスとして設定すると、パスのジオメトリ内の後続のすべての描画が制限されます。たとえば、次のコードは線形グラデーションを描画します。

```csharp
// the color space determines how Core Graphics interprets color information
    using (CGColorSpace rgb = CGColorSpace.CreateDeviceRGB()) {
        CGGradient gradient = new CGGradient (rgb, new CGColor[] {
        UIColor.Blue.CGColor,
        UIColor.Yellow.CGColor
    });

// draw a linear gradient
    g.DrawLinearGradient (
        gradient,
        new CGPoint (path.BoundingBox.Left, path.BoundingBox.Top),
        new CGPoint (path.BoundingBox.Right, path.BoundingBox.Bottom),
        CGGradientDrawingOptions.DrawsBeforeStartLocation);
    }
```

これらの変更により、次のようにグラデーションの塗りつぶしが生成されます。

 ![グラデーションの塗りつぶしを含む例](core-graphics-images/01-gradient-fill.png)

## <a name="modifying-line-patterns"></a>線のパターンの変更

線の描画属性は、コアグラフィックスを使用して変更することもできます。 これには、次のコードに示すように、線の幅とストロークの色、および線パターン自体の変更が含まれます。

```csharp
//use a dashed line
g.SetLineDash (0, new nfloat[] { 10, 4 * (nfloat)Math.PI });
```

描画操作の前にこのコードを追加すると、次に示すように、ダッシュの間に4単位の間隔が付き、破線の長さが10単位になります。

 ![描画操作の前にこのコードを追加すると、破線になります](core-graphics-images/02-dashed-stroke.png)

Xamarin の Unified API を使用する場合、配列の型はである必要があります。 `nfloat` また、数学的に明示的にキャストする必要があります。

<a name="Drawing_Images_and_Text"></a>

## <a name="drawing-images-and-text"></a>描画 (イメージとテキストを)

コアグラフィックスでは、ビューのグラフィックスコンテキストでパスを描画するだけでなく、イメージとテキストの描画もサポートしています。 イメージを描画するには、を作成 `CGImage` して呼び出しに渡すだけです `DrawImage` 。

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);

    using(CGContext g = UIGraphics.GetCurrentContext ()){
        g.DrawImage (rect, UIImage.FromFile ("MyImage.png").CGImage);
    }
}
```

ただし、次のように、反転されたイメージが生成されます。

 ![反転された画像](core-graphics-images/03-upside-down-monkey.png)

この理由は、画像描画の中心となるグラフィックスの原点は左下にあり、ビューの原点は左上にあります。 そのため、イメージを正しく表示するには、origin を変更する必要があります。これは、*現在の変換行列* *(CTM)* を変更することで実現できます。 CTM は、ポイントが存在する場所 (*ユーザー空間*とも呼ばれます) を定義します。 Y 方向の CTM を反転し、負の y 方向の境界の高さでシフトすることで、イメージを反転させることができます。

グラフィックスコンテキストには、CTM を変換するためのヘルパーメソッドがあります。 この場合、 `ScaleCTM` 次に示すように、描画を "反転" し、 `TranslateCTM` 左上に移動します。

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);

    using (CGContext g = UIGraphics.GetCurrentContext ()) {

        // scale and translate the CTM so the image appears upright
        g.ScaleCTM (1, -1);
        g.TranslateCTM (0, -Bounds.Height);
        g.DrawImage (rect, UIImage.FromFile ("MyImage.png").CGImage);
}
```

次に、結果として得られたイメージが上下に表示されます。

 ![上に表示されているサンプルイメージ](core-graphics-images/04-upright-monkey.png)

> [!IMPORTANT]
> グラフィックスコンテキストに加えた変更は、後続のすべての描画操作に適用されます。 そのため、CTM が変換されると、追加の描画に影響します。 たとえば、CTM 変換の後に三角形を描画した場合、その三角形は反転して表示されます。

### <a name="adding-text-to-the-image"></a>画像へのテキストの追加

パスと画像と同様に、コアグラフィックスを使用してテキストを描画する場合も、グラフィックスの状態を設定して描画するメソッドを呼び出すのと同じ基本的なパターンが必要になります。 テキストの場合、テキストを表示するメソッドは `ShowText` です。 イメージ描画の例に追加すると、次のコードは、コアグラフィックスを使用してテキストを描画します。

```csharp
public override void Draw (RectangleF rect)
{
    base.Draw (rect);

    // image drawing code omitted for brevity ...

    // translate the CTM by the font size so it displays on screen
    float fontSize = 35f;
    g.TranslateCTM (0, fontSize);

    // set general-purpose graphics state
    g.SetLineWidth (1.0f);
    g.SetStrokeColor (UIColor.Yellow.CGColor);
    g.SetFillColor (UIColor.Red.CGColor);
    g.SetShadow (new CGSize (5, 5), 0, UIColor.Blue.CGColor);

    // set text specific graphics state
    g.SetTextDrawingMode (CGTextDrawingMode.FillStroke);
    g.SelectFont ("Helvetica", fontSize, CGTextEncoding.MacRoman);

    // show the text
    g.ShowText ("Hello Core Graphics");
}
```

ご覧のように、テキスト描画のグラフィックスの状態を設定することは、描画ジオメトリに似ています。 ただし、テキスト描画の場合は、テキスト描画モードとフォントも適用されます。 この場合、影も適用されますが、影の適用はパス描画と同じです。

次に示すように、結果として得られるテキストがイメージと共に表示されます。

 ![結果のテキストが画像と共に表示されます。](core-graphics-images/05-text-on-image.png)

## <a name="memory-backed-images"></a>メモリによってサポートされるイメージ

コアグラフィックスは、ビューのグラフィックスコンテキストに描画するだけでなく、イメージの描画とも呼ばれるイメージの描画をサポートします。 これを行うには、以下が必要です。

- インメモリビットマップによってサポートされるグラフィックスコンテキストを作成する
- 描画状態の設定と描画コマンドの発行
- コンテキストからイメージを取得する
- コンテキストの削除

`Draw`ビューによってコンテキストが提供されるメソッドとは異なり、この場合は、次の2つの方法のいずれかでコンテキストを作成します。

1. `UIGraphics.BeginImageContext`(または) を呼び出すことによって `BeginImageContextWithOptions`

2. 新しいを作成する`CGBitmapContextInstance`

 `CGBitmapContextInstance`は、イメージビットを直接操作する場合 (カスタムイメージ操作アルゴリズムを使用している場合など) に便利です。 それ以外の場合は、またはを使用する必要があり `BeginImageContext` `BeginImageContextWithOptions` ます。

イメージコンテキストを作成したら、描画コードを追加することは、サブクラスの場合と同じです `UIView` 。 たとえば、前の例で示したように、三角形を描画するために使用したコード例は、次のようにではなく、メモリ内のイメージに描画するために使用でき `UIView` ます。

```csharp
UIImage DrawTriangle ()
{
    UIImage triangleImage;

    //push a memory backed bitmap context on the context stack
    UIGraphics.BeginImageContext (new CGSize (200.0f, 200.0f));

    //get graphics context
    using(CGContext g = UIGraphics.GetCurrentContext ()){

        //set up drawing attributes
        g.SetLineWidth(4);
        UIColor.Purple.SetFill ();
        UIColor.Black.SetStroke ();

        //create geometry
        path = new CGPath ();

        path.AddLines(new CGPoint[]{
            new CGPoint(100,200),
            new CGPoint(160,100),
            new CGPoint(220,200)});

        path.CloseSubpath();

        //add geometry to graphics context and draw it
        g.AddPath(path);
        g.DrawPath(CGPathDrawingMode.FillStroke);

        //get a UIImage from the context
        triangleImage = UIGraphics.GetImageFromCurrentImageContext ();
    }

    return triangleImage;
}
```

メモリによってサポートされるビットマップへの描画は、通常、任意のからイメージをキャプチャするために使用し `UIView` ます。 たとえば、次のコードでは、ビューのレイヤーをビットマップコンテキストにレンダリングし、 `UIImage` そこからを作成します。

```csharp
UIGraphics.BeginImageContext (cellView.Frame.Size);

//render the view's layer in the current context
anyView.Layer.RenderInContext (UIGraphics.GetCurrentContext ());

//get a UIImage from the context
UIImage anyViewImage = UIGraphics.GetImageFromCurrentImageContext ();
UIGraphics.EndImageContext ();
```

## <a name="drawing-pdfs"></a>Pdf の描画

イメージに加えて、コアグラフィックスは PDF 描画をサポートしています。 画像と同様に、PDF をメモリ内にレンダリングするだけでなく、PDF を読み取り、でレンダリングすることもでき `UIView` ます。

### <a name="pdf-in-a-uiview"></a>UIView での PDF

コアグラフィックスでは、ファイルから PDF を読み取り、クラスを使用してビューに表示することもでき `CGPDFDocument` ます。 クラスは、 `CGPDFDocument` コード内の PDF を表し、ページの読み取りおよび描画に使用できます。

たとえば、サブクラスの次のコードは、 `UIView` ファイルから PDF をに読み取り `CGPDFDocument` ます。

```csharp
public class PDFView : UIView
{
    CGPDFDocument pdfDoc;

    public PDFView ()
    {
        //create a CGPDFDocument from file.pdf included in the main bundle
        pdfDoc = CGPDFDocument.FromFile ("file.pdf");
    }

     public override void Draw (Rectangle rect)
    {
        ...
    }
}
```

次に示すように、メソッドはを使用してページを読み取り、を `Draw` `CGPDFDocument` `CGPDFPage` 呼び出すことによって表示でき `DrawPDFPage` ます。

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);

    //flip the CTM so the PDF will be drawn upright
    using (CGContext g = UIGraphics.GetCurrentContext ()) {
        g.TranslateCTM (0, Bounds.Height);
        g.ScaleCTM (1, -1);

        // render the first page of the PDF
        using (CGPDFPage pdfPage = pdfDoc.GetPage (1)) {

        //get the affine transform that defines where the PDF is drawn
        CGAffineTransform t = pdfPage.GetDrawingTransform (CGPDFBox.Crop, rect, 0, true);

        //concatenate the pdf transform with the CTM for display in the view
        g.ConcatCTM (t);

        //draw the pdf page
        g.DrawPDFPage (pdfPage);
        }
    }
}
```

### <a name="memory-backed-pdf"></a>メモリがサポートされている PDF

インメモリ PDF の場合は、を呼び出して PDF コンテキストを作成する必要があり `BeginPDFContext` ます。 PDF への描画はページごとに細分化されています。 各ページはを呼び出して開始され `BeginPDFPage` 、を呼び出すことによって完了し `EndPDFContent` ます。 また、イメージ描画と同様に、メモリがサポートされている PDF の描画では左下の原点が使用されます。これは、イメージと同様に CTM を変更することによって考慮できます。

次のコードは、PDF にテキストを描画する方法を示しています。

```csharp
//data buffer to hold the PDF
NSMutableData data = new NSMutableData ();

//create a PDF with empty rectangle, which will configure it for 8.5x11 inches
UIGraphics.BeginPDFContext (data, CGRect.Empty, null);

//start a PDF page
UIGraphics.BeginPDFPage ();

using (CGContext g = UIGraphics.GetCurrentContext ()) {
    g.ScaleCTM (1, -1);
    g.TranslateCTM (0, -25);
    g.SelectFont ("Helvetica", 25, CGTextEncoding.MacRoman);
    g.ShowText ("Hello Core Graphics");
    }

//complete a PDF page
UIGraphics.EndPDFContent ();
```

生成されたテキストは PDF に描画され、 `NSData` 保存、アップロード、電子メールなどのに含まれます。

## <a name="summary"></a>まとめ

この記事では、*コアグラフィックス*フレームワークによって提供されるグラフィックス機能について説明しました。 コアグラフィックスを使用して、のコンテキスト内、 `UIView,` およびメモリによってサポートされるグラフィックスコンテキスト内で、ジオメトリ、画像、pdf を描画する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [コアグラフィックスのサンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/graphicsandanimation)
- [グラフィックスとアニメーションのチュートリアル](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [コア アニメーション](~/ios/platform/graphics-animation-ios/core-animation.md)
- [コアアニメーションのレシピ](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)
