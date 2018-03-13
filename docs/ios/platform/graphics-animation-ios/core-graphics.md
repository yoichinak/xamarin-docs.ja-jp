---
title: "コア グラフィック"
description: "この記事では、コア グラフィックスの iOS フレームワークについて説明します。 コア グラフィックスを使用して、geometry、イメージ、および Pdf を描画する方法を示します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4A30F480-0723-4B8A-9049-7CEB6211304A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: d53494e61d702b83a28534c644f33fb5327b5958
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="core-graphics"></a>コア グラフィック

_この記事では、コア グラフィックスの iOS フレームワークについて説明します。コア グラフィックスを使用して、geometry、イメージ、および Pdf を描画する方法を示します。_

iOS を含む、 [*コア グラフィックス*](https://developer.apple.com/library/prerelease/ios/documentation/CoreGraphics/Reference/CoreGraphics_Framework/index.html)低レベルの描画サポートを提供するフレームワークです。 これらのフレームワークは、UIKit 内での豊富なグラフィカルな機能を有効にする新機能です。 

コア グラフィックスは、デバイスの独立したグラフィックスの描画をできるようにする低レベルの 2 次元グラフィックス フレームワークです。 すべての 2D UIKit で図面は、内部コア グラフィックスを使用します。

コア グラフィックスは、多くのシナリオなどで描画をサポートします。

-  [使用して、画面に描画する`UIView`](#Drawing_in_a_UIView_Subclass)です。
-  [メモリまたは画面にイメージを描画](#Drawing_Images_and_Text)です。
-  作成して、PDF を描画します。
-  読み取りと既存の PDF を描画します。


## <a name="geometric-space"></a>幾何学的空間

シナリオに関係なくすべての描画をコア グラフィックスの完了 (ピクセル) ではなく、抽象ポイント内で機能を意味幾何学模様の領域で行われます。 対象 geometry と状態の色、線のスタイルなどの描画の観点から描画を説明し、コア グラフィックスがピクセルに変換することで、すべてを処理します。 画家のキャンバスのように考えることができます、グラフィックスのコンテキストには、このような状態を追加します。

この方法にいくつかの利点があります。

-  描画コードを動的になり実行時にグラフィックスを後で変更できます。
-  アプリケーション バンドルの静的なイメージが減少すると、アプリケーションのサイズを小さきます。
-  グラフィックスは、デバイス間で解像度の変更を回復可能になります。

<a name="Drawing_in_a_UIView_Subclass"/>

## <a name="drawing-in-a-uiview-subclass"></a>描画 (UIView サブクラスで)

各`UIView`が、`Draw`それが描画されなければならないときに、システムによって呼び出されるメソッド。 ビューに描画コードを追加するサブクラス`UIView`オーバーライドと`Draw`:

```csharp
public class TriangleView : UIView
{
    public override void Draw (CGRect rect)
    {
        base.Draw (rect);
    }
}
```

描画を直接呼び出すことはありません必要があります。 実行ループ処理中に、システムによって呼び出されます。 最初に、ループの実行、ビューが階層の表示に追加された後、`Draw`メソッドが呼び出されます。 後続の呼び出し`Draw`を呼び出して、描画する必要があると、ビューが付いているときに発生する`SetNeedsDisplay`または`SetNeedsDisplayInRect`ビュー。

### <a name="pattern-for-graphics-code"></a>グラフィックスのコード パターン

内のコード、`Draw`実装が必要なもの描画を記述する必要があります。 描画用のコードは、パターンをいくつかの描画状態を設定して描画することを要求するメソッドを呼び出すに従います。 このパターンは、次のように一般化することができます。

1. グラフィックス コンテキストを取得します。

2. 描画属性を設定します。

3. 描画プリミティブの geometry を作成します。

4. 描画またはストロークにメソッドを呼び出します。

### <a name="basic-drawing-example"></a>基本描画の例

たとえば、次のコード スニペットがあるとします。

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

このコードを分割してみましょう。

```csharp
using (CGContext g = UIGraphics.GetCurrentContext ()) {
...
}
```
次の行は、最初に描画に使用する現在のグラフィックス コンテキストを取得します。 考えることができますグラフィックス コンテキストのキャンバス図面を実施するなど、線、塗りつぶしの色だけでなくを描画するジオメトリの描画に関するすべての状態を格納します。

```csharp
g.SetLineWidth (10);
UIColor.Blue.SetFill ();
UIColor.Red.SetStroke ();
``` 

コードは、グラフィックス コンテキストを取得した後、描画、上記のようにするときに使用するいくつかの属性を設定します。 ここでは線の幅、線、および塗りつぶしの色が設定されます。 すべての後続の描画のグラフィックス コンテキストの状態で維持されるためこれらの属性が使用されます。

使用するには、コードのジオメトリを作成、`CGPath`直線と曲線から説明するグラフィックス パスを許可します。 この場合、パスは、三角形を構成する点の配列を結ぶ線を追加します。 次に示すコア グラフィックス使用してビューの描画の座標系、場所原点は正の x - ダイレクト権限、および正の値と y 方向下に、左上には。

```csharp
var path = new CGPath ();

path.AddLines (new CGPoint[]{
new CGPoint (100, 200),
new CGPoint (160, 100), 
new CGPoint (220, 200)});

path.CloseSubpath ();
``` 

パスが作成した後に追加されますグラフィックス コンテキストを呼び出すように`AddPath`と`DrawPath`それぞれ描画できます。

結果のビューは、次に示します。

 ![](core-graphics-images/00-bluetriangle.png "サンプル出力の三角形")

## <a name="creating-gradient-fills"></a>塗りつぶしのグラデーションを作成します。

描画の高度なフォームも使用できます。 たとえば、コア グラフィックスは、塗りつぶしのグラデーションを作成し、クリッピング パスを適用することができます。 前の例に、パス内のグラデーションの塗りつぶしを描くには、最初のパスする必要がありますクリッピング パスとして設定します。

```csharp
// add the path back to the graphics context so that it is the current path
g.AddPath (path);
// set the current path to be the clipping path
g.Clip ();
```

線形グラデーションを描画するクリッピング パスは、次のコードなど、パスの geometry 内のすべての後続描画を制約としては、現在のパスを設定します。

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

これらの変更は、次に示すように、グラデーション塗りつぶしを生成します。

 ![](core-graphics-images/01-gradient-fill.png "塗りつぶしのグラデーションの例では、")

## <a name="modifying-line-patterns"></a>線の種類を変更します。

線の描画属性は、コア グラフィックに変更できます。 線の幅と線の色と線パターン自体には、次のコードに示すよう。

```csharp
//use a dashed line
g.SetLineDash (0, new nfloat[] { 10, 4 * (nfloat)Math.PI });
```

次に示すように、ダッシュの間隔の 4 つの単位に長い、破線ストローク 10 単位で描画操作結果の前にこのコードを追加しています。

 ![](core-graphics-images/02-dashed-stroke.png "破線のストロークで描画操作結果の前にこのコードを追加します。")
 
Xamarin.iOS で Unified API を使用している場合、配列の型する必要があります、`nfloat`も Math.PI を明示的にキャストする必要があります。

<a name="Drawing_Images_and_Text"/>

## <a name="drawing-images-and-text"></a>描画画像とテキスト

コア グラフィックスだけでなく、ビューのグラフィックス コンテキストでパスの描画、イメージとテキストの描画がもサポートします。 イメージを描画するだけで作成、`CGImage`に渡すと、`DrawImage`呼び出し。

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);

    using(CGContext g = UIGraphics.GetCurrentContext ()){
        g.DrawImage (rect, UIImage.FromFile ("MyImage.png").CGImage);
    }
}
```

ただし、上下逆に、次のように描画されるイメージを生成します。

 ![](core-graphics-images/03-upside-down-monkey.png "上下逆に描画されるイメージ")

その理由は、コア グラフィックスの元の画像の描画の左、下の間は、ビューが、左上の原点です。 そのため、イメージを正しく表示する、原点変更する必要は、変更することで実行する、*現在の変換行列* *(CTM)*です。 CTM 定義ポイント住まいとも呼ばれる*ユーザー領域*です。 Y 方向の CTM を反転し、負の値の y 方向の境界の高さでシフト、イメージを反転します。

グラフィックス コンテキストでは、CTM を変換するヘルパー メソッドがあります。 ここでは、`ScaleCTM`図面の「反転」と`TranslateCTM`次のように、左にシフトします。

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

結果のイメージが表示されます、垂直。

 ![](core-graphics-images/04-upright-monkey.png "サンプル表示されるイメージ直立")

> [!IMPORTANT]
>  **注:**グラフィックス コンテキストへの変更が後続のすべての描画操作に適用します。 したがって、CTM を変換すると、その他の描画に影響します。 たとえば、CTM 変換後の三角形を描画した場合は、表示される上下逆にします。

### <a name="adding-text-to-the-image"></a>テキスト、イメージを追加します。

パスと、イメージとして、コア グラフィックスとテキストを描画は、グラフィックスの状態の設定およびを描画するメソッドを呼び出すと、同じ基本的なパターンです。 テキストを表示する方法は、文字列は、`ShowText`です。 例の描画、イメージに追加すると、次のコードはコア グラフィックスを使用するいくつかのテキストを描画します。

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

ご覧のようは、ジオメトリの描画に似ていますがテキストの描画のグラフィックスの状態を設定します。 ただしテキストを描画するは、テキスト モードとフォントを描画はも適用されます。 この例では、影も適用、影を適用する動作しますが、パスの描画の同じされます。

次のように、イメージを生成されるテキストが表示されます。

 ![](core-graphics-images/05-text-on-image.png "イメージが表示され、生成されるテキスト")

## <a name="memory-backed-images"></a>メモリに格納された画像

ビューのグラフィックス コンテキストに描画、に加えては、メモリの描画を主要なグラフィック サポートとも呼ばれる画面の描画、イメージをバックアップします。 これが必要です。

-  メモリにバックアップするようにグラフィックス コンテキストを作成するビットマップ
-  描画の状態に設定し、描画コマンドを発行します。
-  コンテキストからイメージを取得します。
-  コンテキストを削除します。


異なり、`Draw`メソッド、コンテキストは、ビューによって提供される、ここでコンテキストを作成する、2 つの方法のいずれかで。

1. 呼び出して`UIGraphics.BeginImageContext`(または`BeginImageContextWithOptions`)

2. で新たに作成します。 `CGBitmapContextInstance`

 `CGBitmapContextInstance` カスタム イメージ操作のアルゴリズムを使用している場合のようにイメージのビットを直接操作している場合に役立ちます。 それ以外の場合は、使用することは`BeginImageContext`または`BeginImageContextWithOptions`です。

イメージ コンテキストを作成したら、追加は描画コード内にあるのと同じように、`UIView`サブクラスです。 使用して三角形を描画するコード例を使用しての代わりに、メモリ内のイメージを描画するなど、`UIView`次のように、します。

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

メモリに格納されたビットマップを描画の一般的な用途はいずれかからイメージをキャプチャする`UIView`です。 たとえば、次のコードはビットマップ コンテキストにビューのレイヤーを表示し、作成、`UIImage`から。

```csharp
UIGraphics.BeginImageContext (cellView.Frame.Size);

//render the view's layer in the current context
anyView.Layer.RenderInContext (UIGraphics.GetCurrentContext ());

//get a UIImage from the context
UIImage anyViewImage = UIGraphics.GetImageFromCurrentImageContext ();
UIGraphics.EndImageContext ();
```

## <a name="drawing-pdfs"></a>Pdf の描画

イメージ、他は、コア グラフィックスは、PDF の描画をサポートします。 画像、するメモリ内の PDF を表示したりできるよう、PDF で表示するための読み取り、`UIView`です。

### <a name="pdf-in-a-uiview"></a>PDF、UIView

コア グラフィックス ファイルから、PDF の読み取りとサポートを使用してビューで表示、`CGPDFDocument`クラスです。 `CGPDFDocument`クラスがコードでは、PDF を表し、読み取りし、ページの描画に使用できます。

内の次のコード例については、`UIView`サブクラス PDF ファイルから読み取りますに、 `CGPDFDocument`:

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

`Draw`メソッドを使用し、`CGPDFDocument`にページを読み取る`CGPDFPage`し、呼び出すことによってレンダリング`DrawPDFPage`、次のように。

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

### <a name="memory-backed-pdf"></a>メモリに格納された PDF

インメモリ PDF では、を呼び出して PDF コンテキストを作成する必要があります。`BeginPDFContext`です。 PDF へ描画は、粒度のページにです。 各ページは呼び出すことによって開始`BeginPDFPage`呼び出しで完了と`EndPDFContent`には、コードの間にします。 また、画像の描画にメモリ バックアップ済みとして描画は PDF、origin だけ、CTM を変更することで把握できる左下に使用するようなイメージです。

次のコードは、PDF へテキストを描画する方法を示しています。

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

結果として得られるテキストを描画し、含まれている PDF に、`NSData`を保存する、アップロードされた、電子メールで送信されたなどです。


## <a name="summary"></a>まとめ

この記事の内容について説明しましたを介して提供されるグラフィックス機能、*コア グラフィックス*フレームワークです。 コンテキスト内でジオメトリ、イメージ、および Pdf を描画する主要なグラフィックスを使用する方法を説明しました、`UIView,`だけメモリ サポートのグラフィックス コンテキスト。

## <a name="related-links"></a>関連リンク

- [コア グラフィックスのサンプル](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)
- [グラフィックスおよびアニメーションのチュートリアル](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [コア アニメーション](~/ios/platform/graphics-animation-ios/core-animation.md)
- [アニメーションのレシピをコアします。](https://developer.xamarin.com/recipes/ios/animation/coreanimation)
