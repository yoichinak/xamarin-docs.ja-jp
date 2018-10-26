---
title: Xamarin.iOS でコア グラフィックス
description: この記事では、Core Graphics の iOS フレームワークについて説明します。 コア グラフィックスを使用して、geometry、画像、Pdf を描画する方法を示します。
ms.prod: xamarin
ms.assetid: 4A30F480-0723-4B8A-9049-7CEB6211304A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: f3fe22e56a2c45524923a316ef28e54e5a3cc3f8
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50102972"
---
# <a name="core-graphics-in-xamarinios"></a>Xamarin.iOS でコア グラフィックス

_この記事では、Core Graphics の iOS フレームワークについて説明します。コア グラフィックスを使用して、geometry、画像、Pdf を描画する方法を示します。_

iOS を含む、 [ *Core Graphics* ](https://developer.apple.com/library/prerelease/ios/documentation/CoreGraphics/Reference/CoreGraphics_Framework/index.html)低レベルの描画のサポートを提供するフレームワークです。 これらのフレームワークは、UIKit 内で豊富なグラフィカル機能できるようになります。 

コア グラフィックスは、デバイス非依存グラフィックスを描画できるようにする低レベルの 2D グラフィックス フレームワークです。 すべての 2D 描画 UIKit では、Core Graphics を内部的に使用します。

コア グラフィックスでは、多くのシナリオなどでの描画をサポートします。

-  [使用して、画面に描画を`UIView`](#Drawing_in_a_UIView_Subclass)します。
-  [メモリ内、または画面上のイメージの描画](#Drawing_Images_and_Text)します。
-  作成して、PDF に描画します。
-  読み取りと既存の PDF を描画します。


## <a name="geometric-space"></a>ジオメトリの領域

シナリオに関係なくコア グラフィックスですべての描画ピクセルではなく、抽象ポイントでうまくつまり幾何学領域で実行されます。 対象のジオメトリと状態の色、線のスタイルなどの描画の観点から描画を説明し、Core Graphics がピクセルに変換することで、すべてを処理します。 このような状態は、コピー/貼り付けのキャンバスのように考えることができます、グラフィックスのコンテキストに追加されます。

これにはこの方法にいくつかの利点があります。

-  描画コードを動的になり、実行時にグラフィックスを後で変更できます。
-  アプリケーション バンドルの静的なイメージの必要性を減らすと、アプリケーションのサイズを縮小できます。
-  グラフィックスは、デバイス間での解像度の変更に回復力のあるようになります。

<a name="Drawing_in_a_UIView_Subclass"/>

## <a name="drawing-in-a-uiview-subclass"></a>UIView サブクラスで描画

すべて`UIView`が、`Draw`を描画する必要があるときに、システムによって呼び出されるメソッド。 描画コードをビューに追加するサブクラス`UIView`オーバーライドと`Draw`:

```csharp
public class TriangleView : UIView
{
    public override void Draw (CGRect rect)
    {
        base.Draw (rect);
    }
}
```

描画を直接呼び出すことはありません必要があります。 実行ループの処理中に、システムによって呼び出されます。 ビューがビューの階層構造に追加された後に実行ループを最初にその`Draw`メソッドが呼び出されます。 後続の呼び出し`Draw`ビューがいずれかを呼び出すことで描画する必要があるとマークされている場合に発生する`SetNeedsDisplay`または`SetNeedsDisplayInRect`ビュー。

### <a name="pattern-for-graphics-code"></a>グラフィックス コードのパターン

内のコード、`Draw`実装が必要なもの描画を記述する必要があります。 描画のコードは、いくつかの描画状態を設定し、描画を要求するメソッドを呼び出すをパターンに従います。 このパターンは次のように汎用化できます。

1. グラフィックス コンテキストを取得します。

2. 描画属性を設定します。

3. プリミティブを描画からの geometry を作成します。

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

は、このコードを分割してみましょう。

```csharp
using (CGContext g = UIGraphics.GetCurrentContext ()) {
...
}
```
この行では、まず描画に使用する現在のグラフィックス コンテキストを取得します。 考えることができますグラフィックス コンテキスト キャンバスとしてこと描画では、ストロークや塗りつぶしの色を描画するために、ジオメトリなど、描画の概要のすべての状態を格納します。

```csharp
g.SetLineWidth (10);
UIColor.Blue.SetFill ();
UIColor.Red.SetStroke ();
``` 

コードは、グラフィックス コンテキストを取得した後、描画、前述のようにするときに使用するいくつかの属性を設定します。 この場合、線の幅、ストロークおよび塗りつぶしの色が設定されます。 すべての後続の描画ではこれらの属性を使用してグラフィックス コンテキストの状態で維持されるため。

使用するには、コードを作成するには、geometry、 `CGPath`、グラフィック パス直線と曲線の説明を取得することができます。 この場合、パスは、三角形を構成するには点の配列を結ぶ線を追加します。 コア グラフィックスは、ビューの描画の座標系を表示、配信元が、右、下の正の y 方向に正の x ダイレクトでは、左上。

```csharp
var path = new CGPath ();

path.AddLines (new CGPoint[]{
new CGPoint (100, 200),
new CGPoint (160, 100), 
new CGPoint (220, 200)});

path.CloseSubpath ();
``` 

呼び出すように、パスが作成されると、そのはグラフィックス コンテキストに追加`AddPath`と`DrawPath`それぞれ描画できます。

結果のビューは、以下に示します。

 ![](core-graphics-images/00-bluetriangle.png "サンプル出力の三角形")

## <a name="creating-gradient-fills"></a>グラデーション塗りつぶしを作成します。

描画の豊富なフォームも使用できます。 たとえば、Core Graphics は、グラデーション塗りつぶしを作成して、クリッピング パスの適用を使用できます。 前の例からのパス内のグラデーションの塗りつぶしを描画するために最初のパスする必要がクリッピング パスとして設定します。 ある

```csharp
// add the path back to the graphics context so that it is the current path
g.AddPath (path);
// set the current path to be the clipping path
g.Clip ();
```

線形グラデーションを描画するクリッピング パスは、次のコードなど、パスのジオメトリ内すべての後続の描画を制約として、現在のパスを設定します。

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

これらの変更は、次に示すようにグラデーション塗りつぶしを生成します。

 ![](core-graphics-images/01-gradient-fill.png "塗りつぶしのグラデーションの例では、")

## <a name="modifying-line-patterns"></a>線の種類を変更します。

コア グラフィックス、線の描画属性を変更こともできます。 これが含まれています行パターン自体と線の幅とストロークの色を変更する次のコードに示すよう。

```csharp
//use a dashed line
g.SetLineDash (0, new nfloat[] { 10, 4 * (nfloat)Math.PI });
```

次に示すようにダッシュの間隔の 4 つの単位に長破線ストローク 10 単位で描画操作結果の前にこのコードを追加します。

 ![](core-graphics-images/02-dashed-stroke.png "破線のストロークで描画操作結果の前にこのコードを追加します。")
 
Unified API Xamarin.iOS でを使用する場合に、配列の型が必要、`nfloat`も Math.PI を明示的にキャストする必要があります。

<a name="Drawing_Images_and_Text"/>

## <a name="drawing-images-and-text"></a>描画イメージとテキスト

コア グラフィックスだけでなく、ビューのグラフィックス コンテキストでパスの描画、イメージとテキストの描画がもサポートします。 イメージを描画するために作成、`CGImage`に渡すと、`DrawImage`呼び出し。

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);

    using(CGContext g = UIGraphics.GetCurrentContext ()){
        g.DrawImage (rect, UIImage.FromFile ("MyImage.png").CGImage);
    }
}
```

ただし、上下逆に、次に示すように描画されるイメージが生成されます。

 ![](core-graphics-images/03-upside-down-monkey.png "上下に描画されるイメージ")

この理由はイメージの描画のためのコア グラフィックの原点左、下にあるビューが左上の原点です。 そのため、イメージを正しく表示する、配信元の変更が必要、変更することで実現できますが、*変換行列を現在* *(CTM)* します。 CTM 定義ポイントどことも呼ばれます*ユーザー空間*します。 Y 方向の CTM を反転し、シフトが負の値の y 方向の境界の高さは、イメージを反転できます。

グラフィックス コンテキストでは、変換、CTM するヘルパー メソッドがあります。 ここでは、`ScaleCTM`図面の「反転」と`TranslateCTM`次に示すように、左にシフトします。

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

 ![](core-graphics-images/04-upright-monkey.png "サンプルに表示されるイメージの直立")

> [!IMPORTANT]
> グラフィックス コンテキストへの変更は、すべての後続の描画操作に適用されます。 そのため、CTM が変換されるときに、追加の描画に影響されます。 たとえば、CTM 変換後の三角形を描画した場合は、表示される上下。

### <a name="adding-text-to-the-image"></a>テキスト、イメージを追加します。

パスとイメージ、として、コア グラフィックスと描画のテキストは、によってグラフィックスの状態を設定し、描画するメソッドを呼び出すのと同じ基本的なパターンです。 テキストを表示する方法は、文字列は、`ShowText`します。 イメージの描画を例に追加すると、次のコードはコア グラフィックスを使用していくつかのテキストを描画します。

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

、ご覧のとおり、テキストの描画のグラフィックスの状態を設定するはジオメトリの描画に似ています。 ただしテキストを描画、テキストの描画モードとフォントはも適用されます。 この場合、影も適用されます、同じパスの描画の影を適用することは動作します。

次に示すよう、イメージと、結果のテキストが表示されます。

 ![](core-graphics-images/05-text-on-image.png "イメージと、結果のテキストが表示されます。")

## <a name="memory-backed-images"></a>メモリ ベースのイメージ

ビューのグラフィックス コンテキストに描画、だけでなくメモリの描画をコア グラフィックス サポート イメージ、画面外に描画とも呼ばれます支えられています。 これが必要です。

-  メモリ内でサポートするグラフィックス コンテキストを作成するビットマップ
-  描画の状態を設定し、描画コマンドの発行
-  コンテキストからのイメージの取得
-  コンテキストを削除します。


異なり、`Draw`メソッド、コンテキストは、ビューによって提供される、このケースでコンテキストを作成する、2 つの方法のいずれかで。

1. 呼び出して`UIGraphics.BeginImageContext`(または`BeginImageContextWithOptions`)

2. で新しいを作成します。 `CGBitmapContextInstance`

 `CGBitmapContextInstance` カスタム イメージの操作のアルゴリズムを使用している場合のように、イメージ ビットを直接操作している場合に役立ちます。 その他のすべてのケースで使用する必要があります`BeginImageContext`または`BeginImageContextWithOptions`します。

内にあるのと同じように、描画コードを追加する、イメージのコンテキストを作成したら、`UIView`サブクラスです。 三角形を描画するために以前に使用するコード例を使用して、代わりにメモリ内のイメージを描画するなど、`UIView`以下に示すように。

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

メモリ ベースのビットマップの描画の一般的な用途は、いずれかからイメージをキャプチャする`UIView`します。 次のコードがビットマップのコンテキストにビューのレイヤーを表示し、作成など、`UIImage`から。

```csharp
UIGraphics.BeginImageContext (cellView.Frame.Size);

//render the view's layer in the current context
anyView.Layer.RenderInContext (UIGraphics.GetCurrentContext ());

//get a UIImage from the context
UIImage anyViewImage = UIGraphics.GetImageFromCurrentImageContext ();
UIGraphics.EndImageContext ();
```

## <a name="drawing-pdfs"></a>Pdf の描画

に加えて、イメージは、Core Graphics は、PDF 描画をサポートします。 イメージのようにすることができますメモリ内の PDF をレンダリングするだけでなくで表示するための PDF の読み取り、`UIView`します。

### <a name="pdf-in-a-uiview"></a>UIView PDF

コア グラフィックス ファイルから PDF の読み取りとサポートを使用してビューにレンダリングされる、`CGPDFDocument`クラス。 `CGPDFDocument`クラスがコードでは、PDF を表し、読み取りし、ページの描画に使用できます。

次のコードなど、`UIView`サブクラスですが、ファイルから PDF を読み取ります、 `CGPDFDocument`:

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

`Draw`メソッドを使用できますし、`CGPDFDocument`にページを読み取る`CGPDFPage`呼び出すことでレンダリングする`DrawPDFPage`以下に示すように。

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

### <a name="memory-backed-pdf"></a>メモリ ベースの PDF

メモリ内 PDF では、PDF コンテキストを呼び出すことによって作成する必要があります。`BeginPDFContext`します。 PDF への描画がページに細分化されました。 各ページが呼び出しによって開始された`BeginPDFPage`を呼び出して完了と`EndPDFContent`、間に、グラフィックス コードします。 また、イメージの描画、使用メモリ バックアップ描画は PDF、CTM を単に変更することで考慮できる左下に、配信元は、イメージなど。

次のコードでは、PDF にテキストを描画する方法を示します。

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

含まれているし、PDF に、生成されるテキストを描画、`NSData`を保存できる、アップロードされた、電子メールで送信したなど。


## <a name="summary"></a>まとめ

この記事で経由で提供されるグラフィックス機能でしました、 *Core Graphics*フレームワーク。 コンテキスト内のジオメトリ、画像、Pdf の描画にコア グラフィックスを使用する方法を説明しました、`UIView,`メモリ サポートのグラフィックス コンテキストにもします。

## <a name="related-links"></a>関連リンク

- [コア グラフィックスのサンプル](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)
- [グラフィックスとアニメーションのチュートリアル](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [コア アニメーション](~/ios/platform/graphics-animation-ios/core-animation.md)
- [コア アニメーションのレシピ](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)
