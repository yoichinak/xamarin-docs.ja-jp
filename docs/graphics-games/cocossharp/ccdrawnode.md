---
title: CCDrawNode ジオメトリの描画
description: CCDrawNode は、線、円、および三角形などの描画プリミティブ オブジェクトに対してメソッドを提供します。
ms.prod: xamarin
ms.assetid: 46A3C3CE-74CC-4A3A-AB05-B694AE182ADB
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: 2a4278f97a69ef9eb705c6fedf1a5c7ec98875b8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="drawing-geometry-with-ccdrawnode"></a>CCDrawNode ジオメトリの描画

_`CCDrawNode` 線、円、および三角形などの描画プリミティブ オブジェクトのメソッドを提供します。_

`CCDrawNode` CocosSharp 内のクラスは、一般的な図形を描画するための複数のメソッドを提供します。 継承して、`CCNode`クラスとに追加されます通常`CCLayer`インスタンス。 このガイドを使用する方法を説明する`CCDrawNode`カスタム描画を実行するインスタンス。 また、スクリーン ショットとコード例を含む関数の使用可能な描画の包括的な一覧を提供します。


## <a name="creating-a-ccdrawnode"></a>CCDrawNode を作成します。

`CCDrawNode`クラスは、円、四角形、線などの幾何オブジェクトの描画に使用できます。 たとえば、次のコード サンプルが作成する方法を示します、`CCDrawNode`で円を描画するインスタンス、`CCLayer`クラスを実装します。


```csharp
public class GameLayer : CCLayer
{
    public GameLayer ()
    {
        var drawNode = new CCDrawNode ();
        this.AddChild (drawNode);
        // Origin is bottom-left of the screen. This moves
        // the drawNode 100 pixels to the right and 100 pixels up
        drawNode.PositionX = 100;
        drawNode.PositionY = 100;

        drawNode.DrawCircle (
            center: new CCPoint (0, 0),
            radius: 20,
            color: CCColor4B.White);

    }
} 
```

このコードは、実行時に次の円を生成します。

![](ccdrawnode-images/image1.png "このコードは、実行時にこの円を生成します")


## <a name="draw-method-details"></a>メソッドの詳細を描画します。

描画に関連するいくつかの詳細を見てみましょう、 `CCDrawNode`:


### <a name="draw-methods-positions-are-relative-to-the-ccdrawnode"></a>描画位置は、CCDrawNode メソッド

すべての描画方法では、描画のためには、少なくとも 1 つの位置の値が必要です。 この位置の値が基準には、`CCDrawNode`インスタンス。 つまり、`CCDrawNode`自体、位置があり、すべての描画呼び出しに加えられた、`CCDrawNode`も 1 つまたは複数の位置の値を取得します。 これらの値を結合する方法の理解に役立つ、いくつかの例を見てみましょう。

最初に見ていきます、`DrawCircle`上記の例。


```csharp
...
drawNode.PositionX = 100;
drawNode.PositionY = 100;

drawNode.DrawCircle (center: new CCPoint (0, 0),
...
```

ここで、 `CCDrawNode` (100, 100) に配置されます描画丸が (0, 0) を基準には、 `CCDrawNode`、最大、およびゲームの画面の左下隅の右側に 100 ピクセルの中央をされている円で囲んだ結果として得られる。

`CCDrawNode`原点 (画面の下部左) に配置されていることができますも証明書利用者のオフセットの円周上。


```csharp
...
drawNode.PositionX = 0;
drawNode.PositionY = 0;

drawNode.DrawCircle (center: new CCPoint (50, 60),
...
```

50 個のユニットにある円の中心の結果上に、コード (`drawNode.PositionX` + `CCPoint.X`)、画面から 60 の左側の右側に (`drawNode.PositionY` +、 `CCPoint.Y`)、画面の最下部より単位です。

Draw メソッドが呼び出されて、描画されたオブジェクトは変更できませんしない限り、`CCDrawNode.Clear`メソッドが呼び出されると、実行する必要があるため、再配置、`CCDrawNode`自体です。

オブジェクトによって描画された`CCNodes`によって影響を受けるも、`CCNode`インスタンスの`Rotation`と`Scale`プロパティです。


### <a name="draw-methods-do-not-need-to-be-called-every-frame"></a>Draw メソッドは、すべてのフレームを呼び出す必要はありません。

描画メソッドは、永続的なビジュアルの作成に 1 回だけ呼び出す必要があります。 上記の呼び出しの例で`DrawCircle`のコンス トラクターで、 `GameLayer` –`DrawCircle`すべてフレームが更新されると、画面に、円を再描画するに呼び出せる必要はありません。

これが通常表示する 1 つのフレームを画面に何かと、すべてのフレームを呼び出す必要があります MonoGame の描画方法によって異なります。

描画メソッドには、すべてのフレームが呼び出されるかどうかは、呼び出し元の内部オブジェクトを溜めてしまう`CCDrawNode`インスタンス、複数のオブジェクトが描画されたとおりのフレーム レートのドロップの結果として得られます。


### <a name="each-ccdrawnode-supports-multiple-draw-calls"></a>各 CCDrawNode が複数の描画呼び出しをサポートしています

`CCDrawNode` インスタンスは、複数の図形の描画に使用できます。 これにより、単一のオブジェクトに入れする複雑な visual オブジェクトできます。 次のコードを使用して、いずれかで複数の円を表示するために、 `CCDrawNode`:


```csharp
for (int i = 0; i < 8; i++)
{
    drawNode.DrawCircle (
        center: new CCPoint (i*15, 0),
        radius: 20,
        color: CCColor4B.White);
} 
```

これは、結果は、次の図になります。

![](ccdrawnode-images/image2.png "これは、結果、次の図")


## <a name="draw-call-examples"></a>描画呼び出しの例

次の描画呼び出しで使用できる`CCDrawNode`:

- [`DrawCatmullRom`](#drawcatmullrom)
- [`DrawCircle`](#drawcircle)
- [`DrawCubicBezier`](#drawcubicbezier)
- [`DrawEllipse`](#drawellipse)
- [`DrawLineList`](#drawlinelist)
- [`DrawPolygon`](#drawpolygon)
- [`DrawQuadBezier`](#drawquadbezier)
- [`DrawRect`](#drawrect)
- [`DrawSegment`](#drawsegment)
- [`DrawSolidArc`](#drawsolidarc)
- [`DrawSolidCircle`](#drawsolidcircle)
- [`DrawTriangleList`](#drawtrianglelist)


### <a name="drawcardinalspline"></a>DrawCardinalSpline

`DrawCardinalSpline` 曲線ポイント数が変数を介してを作成します。 

`config`スプラインがパススルーするポイントを定義します。 次の例では、4 つの点を通過するスプラインを示します。

`tension`パラメーター コントロールは、どのようにシャープなまたは、round スプライン上の点が表示されます。 A`tension`値を 0 になります、曲線スプラインと`tension`直線とエッジによって描画されたスプライン 1 の値になります。

スプライン曲線が、CocosSharp は直線でスプラインを描画します。 `segments`パラメーターはスプラインを描画に使用するセグメントの数を制御します。 大きい数値は、パフォーマンスが低下曲線を滑らかにスプラインになります。 

コードの例:


```csharp
var splinePoints = new List<CCPoint> ();
splinePoints.Add (new CCPoint (0, 0));
splinePoints.Add (new CCPoint (50, 70));
splinePoints.Add (new CCPoint (0, 140));
splinePoints.Add (new CCPoint (100, 210));

drawNode.DrawCardinalSpline (
    config: splinePoints,
    tension: 0,
    segments: 64,
    color:CCColor4B.Red); 
```

![](ccdrawnode-images/image3.png "セグメント パラメーターは、スプラインを描画に使用するセグメントの数を制御します。")


### <a name="drawcatmullrom"></a>DrawCatmullRom

`DrawCatmullRom` 曲線ポイントのようなさまざまな変数を作成`DrawCardinalLine`です。 このメソッドでは、テンション パラメーターは含まれません。

コードの例:

```csharp
var splinePoints = new List<CCPoint> ();
splinePoints.Add (new CCPoint (0, 0));
splinePoints.Add (new CCPoint (80, 90));
splinePoints.Add (new CCPoint (100, 0));
splinePoints.Add (new CCPoint (0, 130)); 

drawNode.DrawCatmullRom (
    points: splinePoints,
    segments: 64); 
```

![](ccdrawnode-images/image4.png "DrawCatmullRom 曲線ポイント、DrawCardinalLine のようなさまざまな変数を作成します。")


### <a name="drawcircle"></a>DrawCircle

`DrawCircle` 円の境界を作成、特定`radius`です。

コードの例:

```csharp
drawNode.DrawCircle (
    center:new CCPoint (0, 0),
    radius:20,
    color:CCColor4B.Yellow); 
```

![](ccdrawnode-images/image5.png "DrawCircle、特定の半径の円の境界を作成します。")


### <a name="drawcubicbezier"></a>DrawCubicBezier

`DrawCubicBezier` コントロール ポイントを使用して、2 つのポイント間のパスを設定する、2 つのポイント間の曲線を描画します。

コードの例:

```csharp
drawNode.DrawCubicBezier (
    origin: new CCPoint (0, 0),
    control1: new CCPoint (50, 150),
    control2: new CCPoint (250, 150),
    destination: new CCPoint (170, 0),
    segments: 64,
    lineWidth: 1,
    color: CCColor4B.Green); 
```

 ![](ccdrawnode-images/image6.png "DrawCubicBezier 2 点間の曲線を描画します。")


### <a name="drawellipse"></a>DrawEllipse

`DrawEllipse` 輪郭を作成、*楕円*、これは多くの場合と呼びます楕円 (ただし、2 つが幾何学的と同じではありません)。 によって、楕円の形状を定義できる、`CCRect`インスタンス。

コードの例:


```csharp
drawNode.DrawEllipse (
    rect: new CCRect (0, 0, 130, 90),
    lineWidth: 2,
    color: CCColor4B.Gray); 
```

![](ccdrawnode-images/image8.png "DrawEllipse は多くの場合と呼ばれる楕円楕円の輪郭を作成します。")


### <a name="drawline"></a>DrawLine

`DrawLine` 指定された幅の行でポイントに接続します。 このメソッドはのような`DrawSegment`、round エンドポイントではなくフラットなエンドポイントを作成します。

コードの例:


```csharp
drawNode.DrawLine (
    from: new CCPoint (0, 0),
    to: new CCPoint (150, 30),
    lineWidth: 5,
    color:CCColor4B.Orange); 
```

![](ccdrawnode-images/image9.png "指定された幅の行でポイントに接続する DrawLine")


### <a name="drawlinelist"></a>DrawLineList

`DrawLineList` 指定された点の各ペアを接続することで複数の行を作成、`CCV3F_C4B`配列。 `CCV3F_C4B`構造体には位置と色の値が含まれています。 `verts`各行が 2 つの点で定義されている、パラメーターは、ポイント数が偶数を含める常にする必要があります。

コードの例:


```csharp
CCV3F_C4B[] verts = new CCV3F_C4B[] {
    // First line:
    new CCV3F_C4B( new CCPoint(0,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(30,60), CCColor4B.White),
    // second line, will blend from white to red:
    new CCV3F_C4B( new CCPoint(60,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(120,120), CCColor4B.Red)
};

drawNode.DrawLineList (verts); 
```

 ![](ccdrawnode-images/image10.png "Verts パラメーター常には、偶数の数のポイント、各行が 2 つの点で定義されています。")




### <a name="drawpolygon"></a>DrawPolygon

`DrawPolygon` 可変幅と色のアウトラインを塗りつぶされた多角形を作成します。

コードの例:


```csharp
CCPoint[] verts = new CCPoint[] {
    new CCPoint(0,0),
    new CCPoint(0, 100),
    new CCPoint(50, 150),
    new CCPoint(100, 100),
    new CCPoint(100, 0)
};

drawNode.DrawPolygon (verts,
    count: verts.Length,
    fillColor: CCColor4B.White,
    borderWidth: 5,
    borderColor: CCColor4B.Red,
    closePolygon: true); 
```

![](ccdrawnode-images/image11.png "DrawPolygon 可変幅と色のアウトライン塗りつぶされた多角形を作成します。")


### <a name="drawquadbezier"></a>DrawQuadBezier

`DrawQuadBezier` 行に 2 つのポイントを接続できます。 同じように動作する`DrawCubicBezier`がのみ 1 つのコントロール ポイントをサポートします。

コードの例:


```csharp
drawNode.DrawQuadBezier (
    origin:new CCPoint (0, 0),
    control:new CCPoint (200, 0),
    destination:new CCPoint (0, 300),
    segments:64,
    lineWidth:1,
    color:CCColor4B.White);
```

![](ccdrawnode-images/image12.png "DrawQuadBezier、線の 2 つのポイントを接続します。")


### <a name="drawrect"></a>DrawRect

`DrawRect` 可変幅と色のアウトラインを塗りつぶされた四角形を作成します。

コードの例: 


```csharp
var shape = new CCRect (
    0, 0, 100, 200);
drawNode.DrawRect(shape,
    fillColor:CCColor4B.Blue,
    borderWidth: 4,
    borderColor:CCColor4B.White); 
```

![](ccdrawnode-images/image13.png "DrawRect 可変幅と色のアウトライン塗りつぶされた四角形を作成します。")


### <a name="drawsegment"></a>DrawSegment

`DrawSegment` 可変幅と色の行に 2 つのポイントを接続できます。 似ています`DrawLine`、フラットなエンドポイントではなく、丸いエンドポイントが作成されます。

コードの例:


```csharp
drawNode.DrawSegment (from: new CCPoint (0, 0),
    to: new CCPoint (100, 200),
    radius: 5,
    color:new CCColor4F(1,1,1,1)); 
```

![](ccdrawnode-images/image14.png "DrawSegment を可変幅と色の行の 2 つのポイントを接続します。")


### <a name="drawsolidarc"></a>DrawSolidArc

`DrawSolidArc` 指定された色と半径の入力でくさび形を作成します。

コードの例:


```csharp
drawNode.DrawSolidArc(
    pos:new CCPoint(100, 100),
    radius:200,
    startAngle:0,
    sweepAngle:CCMathHelper.Pi/2, // this is in radians, clockwise
    color:CCColor4B.White); 
```

![](ccdrawnode-images/image15.png "DrawSolidArc の特定の色と radius 塗りつぶされたくさび形を作成します。")


### <a name="drawsolidcircle"></a>DrawSolidCircle

`DrawCircle` 指定した radius の塗りつぶされた円を作成します。

コードの例:


```csharp
drawNode.DrawSolidCircle(
    pos: new CCPoint (100, 100),
    radius: 50,
    color: CCColor4B.Yellow); 
```

![](ccdrawnode-images/image16.png "DrawCircle の特定の半径の塗りつぶされた円を作成します。")


### <a name="drawtrianglelist"></a>DrawTriangleList

`DrawTriangleList` 三角形のリストを作成します。 個々 の三角形が 3 で定義されている`CCV3F_C4B`配列内のインスタンス。 渡される配列内の頂点の数、`verts`パラメーターが 3 の倍数にする必要があります。 含まれている情報だけを`CCV3F_C4B`、verts と – の色の位置、`DrawTriangleList`メソッドは、テクスチャの描画の三角形をサポートしていません。

コードの例:


```csharp
CCV3F_C4B[] verts = new CCV3F_C4B[] {
    // First triangle:
    new CCV3F_C4B( new CCPoint(0,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(30,60), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(60,0), CCColor4B.White),
    // second triangle, each point has different colors:
    new CCV3F_C4B( new CCPoint(90,0), CCColor4B.Yellow),
    new CCV3F_C4B( new CCPoint(120,60), CCColor4B.Red),
    new CCV3F_C4B( new CCPoint(150,0), CCColor4B.Blue)
};

drawNode.DrawTriangleList (verts); 
```

![](ccdrawnode-images/image17.png "DrawTriangleList の三角形のリストを作成します。")


## <a name="summary"></a>まとめ

このガイドを作成する方法について説明、`CCDrawNode`プリミティブ ベースのレンダリングを実行します。 各描画呼び出しの例を示します。

## <a name="related-links"></a>関連リンク

- [CCDrawNode API](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode/)
- [完全なサンプル](https://developer.xamarin.com/samples/mobile/CCDrawNode/)
