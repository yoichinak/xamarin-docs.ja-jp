---
title: CCDrawNode によるジオメトリの描画
description: このドキュメントでは、線、円、三角形などの描画プリミティブ オブジェクトのメソッドを提供する、CCDrawNode について説明します。
ms.prod: xamarin
ms.assetid: 46A3C3CE-74CC-4A3A-AB05-B694AE182ADB
author: conceptdev
ms.author: crdun
ms.date: 03/24/2017
ms.openlocfilehash: b910e136366c429de8bd2ba1ac959882b4d7201d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61377594"
---
# <a name="drawing-geometry-with-ccdrawnode"></a>CCDrawNode によるジオメトリの描画

_`CCDrawNode` 線、円、三角形などの描画プリミティブ オブジェクトのメソッドを提供します。_

`CCDrawNode` CocosSharp クラスは、一般的な幾何学的図形を描画するための複数のメソッドを提供します。 継承、`CCNode`クラスとに追加されます通常`CCLayer`インスタンス。 このガイドは、使用する方法を説明します。`CCDrawNode`カスタム レンダリングを実行するインスタンス。 また、スクリーン ショットとコード例で使用可能な描画関数の包括的な一覧を提供します。


## <a name="creating-a-ccdrawnode"></a>CCDrawNode を作成します。

`CCDrawNode`クラスは、円、四角形、線などの幾何オブジェクトを描画するために使用できます。 たとえば、次のコード サンプルが作成する方法を示します、`CCDrawNode`インスタンスで、円を描画する、`CCLayer`クラスを実装します。


```csharp
public class GameLayer : CCLayer
{
    public GameLayer ()
    {
        var drawNode = new CCDrawNode ();
        this.AddChild (drawNode);
        // Origin is bottom-left of the screen. This moves
        // the drawNode 100 pixels to the right and 100 pixels up
        drawNode.PositionX = 100;
        drawNode.PositionY = 100;

        drawNode.DrawCircle (
            center: new CCPoint (0, 0),
            radius: 20,
            color: CCColor4B.White);

    }
} 
```

このコードは、実行時に次の円を生成します。

![](ccdrawnode-images/image1.png "このコードは、実行時にこの円を生成します。")


## <a name="draw-method-details"></a>描画メソッドの詳細

描画に関連するいくつかの詳細を見ていきましょう、 `CCDrawNode`:


### <a name="draw-methods-positions-are-relative-to-the-ccdrawnode"></a>描画位置は、CCDrawNode メソッド

すべての描画方法には、描画のために少なくとも 1 つの位置の値が必要です。 この位置の値が関連して、`CCDrawNode`インスタンス。 つまり、`CCDrawNode`自体に含まれて、位置、およびすべての描画に対する呼び出し、`CCDrawNode`も 1 つまたは複数の位置の値を取得します。 これらの値を結合する方法については、いくつかの例を見てみましょう。

最初に見て、`DrawCircle`上記の例。


```csharp
...
drawNode.PositionX = 100;
drawNode.PositionY = 100;

drawNode.DrawCircle (center: new CCPoint (0, 0),
...
```

ここで、 `CCDrawNode` (100, 100) に配置されます (0, 0) を基準とする、円を描画し、 `CCDrawNode`、最大、およびゲーム画面の左下隅の右側には、100 ピクセルの中央揃えをされている円の中で結果として得られる。

`CCDrawNode`原点 (画面の下部にある左) に配置することもできますオフセットの円に依存します。


```csharp
...
drawNode.PositionX = 0;
drawNode.PositionY = 0;

drawNode.DrawCircle (center: new CCPoint (50, 60),
...
```

50 個のユニットにある円の中心に上記のコード (`drawNode.PositionX` + `CCPoint.X`) 60、画面の左側の右側に (`drawNode.PositionY` + `CCPoint.Y`)、画面の下の単位。

しない限り、描画オブジェクトは変更できません draw メソッドが呼び出された後、`CCDrawNode.Clear`メソッドが呼び出されるで実行する必要があるため、再配置、`CCDrawNode`自体。

オブジェクトで描画される`CCNodes`によって影響を受けることも、`CCNode`インスタンスの`Rotation`と`Scale`プロパティ。


### <a name="draw-methods-do-not-need-to-be-called-every-frame"></a>描画メソッドは、すべてのフレームで呼び出される必要はありません。

描画メソッドは、永続的なビジュアルを作成する 1 回だけ呼び出される必要があります。 呼び出しを上記の例で`DrawCircle`のコンス トラクター、 `GameLayer` –`DrawCircle`再画面が更新されると、円を描画するためにはフレームごとに呼び出される必要はありません。

これを通常はレンダリングを 1 つのフレームを画面に何かと、すべてのフレームを呼び出す必要があります MonoGame で描画メソッドとは異なります。

描画メソッドはすべてのフレームと呼ばれるかどうかは、オブジェクトは、呼び出し元の内部で最終的に蓄積されます`CCDrawNode`インスタンス、複数のオブジェクトが描画されたとおりになフレーム レートが低下なります。


### <a name="each-ccdrawnode-supports-multiple-draw-calls"></a>各 CCDrawNode は、複数の描画呼び出しをサポートしています。

`CCDrawNode` インスタンスは、複数の図形を描画するために使用できます。 これにより、複雑なのビジュアル オブジェクトを 1 つのオブジェクトが収納されています。 次のコードを使用して、1 つで複数の円を表示するために、 `CCDrawNode`:


```csharp
for (int i = 0; i < 8; i++)
{
    drawNode.DrawCircle (
        center: new CCPoint (i*15, 0),
        radius: 20,
        color: CCColor4B.White);
} 
```

これは、次の図が得られます。

![](ccdrawnode-images/image2.png "これは、結果、次の図")


## <a name="draw-call-examples"></a>描画呼び出しの例

次の描画呼び出しが表示されます`CCDrawNode`:。

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

`DrawCardinalSpline` 曲線ポイント数が変数を作成します。 

`config`パラメーターは、スプラインをパススルーするポイントを定義します。 次の例では、4 つのポイントを通過するスプラインを示します。

`tension`シャープなまたはラウンド スプライン上の点が表示する方法のパラメーターを制御します。 A`tension`スプライン曲線の値を 0 になります、`tension`直線とエッジによって描画されるスプライン値 1 が発生します。

スプライン曲線は CocosSharp は直線でスプラインを描画します。 `segments`パラメーターは、スプラインを描画するために使用するセグメントの数を制御します。 パフォーマンスを犠牲に曲線が滑らかにスプライン多数になります。 

コード例:


```csharp
var splinePoints = new List<CCPoint> ();
splinePoints.Add (new CCPoint (0, 0));
splinePoints.Add (new CCPoint (50, 70));
splinePoints.Add (new CCPoint (0, 140));
splinePoints.Add (new CCPoint (100, 210));

drawNode.DrawCardinalSpline (
    config: splinePoints,
    tension: 0,
    segments: 64,
    color:CCColor4B.Red); 
```

![](ccdrawnode-images/image3.png "セグメント パラメーターは、スプラインを描画するために使用するセグメントの数を制御します。")


### <a name="drawcatmullrom"></a>DrawCatmullRom

`DrawCatmullRom` 変数のようなポイント数で曲線を作成します。`DrawCardinalLine`します。 このメソッドでは、テンション パラメーターは含まれません。

コード例:

```csharp
var splinePoints = new List<CCPoint> ();
splinePoints.Add (new CCPoint (0, 0));
splinePoints.Add (new CCPoint (80, 90));
splinePoints.Add (new CCPoint (100, 0));
splinePoints.Add (new CCPoint (0, 130)); 

drawNode.DrawCatmullRom (
    points: splinePoints,
    segments: 64); 
```

![](ccdrawnode-images/image4.png "DrawCatmullRom を通じて、変数の数のポイント、DrawCardinalLine のような曲線を作成します")


### <a name="drawcircle"></a>DrawCircle

`DrawCircle` 円の境界を作成し、指定された`radius`します。

コード例:

```csharp
drawNode.DrawCircle (
    center:new CCPoint (0, 0),
    radius:20,
    color:CCColor4B.Yellow); 
```

![](ccdrawnode-images/image5.png "DrawCircle は、指定した半径の円の境界を作成します。")


### <a name="drawcubicbezier"></a>DrawCubicBezier

`DrawCubicBezier` 2 点間のパスを設定する管理ポイントを使用して、2 つのポイント間の曲線を描画します。

コード例:

```csharp
drawNode.DrawCubicBezier (
    origin: new CCPoint (0, 0),
    control1: new CCPoint (50, 150),
    control2: new CCPoint (250, 150),
    destination: new CCPoint (170, 0),
    segments: 64,
    lineWidth: 1,
    color: CCColor4B.Green); 
```

 ![](ccdrawnode-images/image6.png "DrawCubicBezier 2 点を結ぶ曲線を描画します")


### <a name="drawellipse"></a>DrawEllipse

`DrawEllipse` アウトラインを作成、*楕円*、これは多くの場合、呼ばれます楕円 (ただし、2 つは、幾何学的に同一ではない)。 楕円のシェイプによって定義できる、`CCRect`インスタンス。

コード例:


```csharp
drawNode.DrawEllipse (
    rect: new CCRect (0, 0, 130, 90),
    lineWidth: 2,
    color: CCColor4B.Gray); 
```

![](ccdrawnode-images/image8.png "オペレーションは、多くの場合、楕円として呼ばれる、楕円のアウトラインを作成します。")


### <a name="drawline"></a>DrawLine

`DrawLine` 行の幅を指定したポイントに接続します。 このメソッドは`DrawSegment`を除き、round エンドポイントではなくフラットなエンドポイントを作成します。

コード例:


```csharp
drawNode.DrawLine (
    from: new CCPoint (0, 0),
    to: new CCPoint (150, 30),
    lineWidth: 5,
    color:CCColor4B.Orange); 
```

![](ccdrawnode-images/image9.png "DrawLine が行の幅を指定したポイントに接続します。")


### <a name="drawlinelist"></a>DrawLineList

`DrawLineList` 指定されたポイントの各ペアを接続することで複数の行を作成し、`CCV3F_C4B`配列。 `CCV3F_C4B`構造体には位置と色の値が含まれています。 `verts`各行が 2 つのポイントで定義されている、パラメーターは、ポイントの偶数を含める常にする必要があります。

コード例:


```csharp
CCV3F_C4B[] verts = new CCV3F_C4B[] {
    // First line:
    new CCV3F_C4B( new CCPoint(0,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(30,60), CCColor4B.White),
    // second line, will blend from white to red:
    new CCV3F_C4B( new CCPoint(60,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(120,120), CCColor4B.Red)
};

drawNode.DrawLineList (verts); 
```

 ![](ccdrawnode-images/image10.png "各行が 2 つのポイントで定義されている、verts パラメーターは、ポイントの偶数を含める常にする必要があります。")




### <a name="drawpolygon"></a>DrawPolygon

`DrawPolygon` 可変幅と色のアウトラインで塗りつぶされた多角形を作成します。

コード例:


```csharp
CCPoint[] verts = new CCPoint[] {
    new CCPoint(0,0),
    new CCPoint(0, 100),
    new CCPoint(50, 150),
    new CCPoint(100, 100),
    new CCPoint(100, 0)
};

drawNode.DrawPolygon (verts,
    count: verts.Length,
    fillColor: CCColor4B.White,
    borderWidth: 5,
    borderColor: CCColor4B.Red,
    closePolygon: true); 
```

![](ccdrawnode-images/image11.png "DrawPolygon の可変幅と色を外枠付きで塗りつぶされたポリゴンを作成します。")


### <a name="drawquadbezier"></a>DrawQuadBezier

`DrawQuadBezier` 行と 2 つのポイントに接続します。 同様に動作`DrawCubicBezier`しますが、のみ 1 つのコントロール ポイントをサポートしています。

コード例:


```csharp
drawNode.DrawQuadBezier (
    origin:new CCPoint (0, 0),
    control:new CCPoint (200, 0),
    destination:new CCPoint (0, 300),
    segments:64,
    lineWidth:1,
    color:CCColor4B.White);
```

![](ccdrawnode-images/image12.png "DrawQuadBezier 線で 2 つのポイントを接続します。")


### <a name="drawrect"></a>DrawRect

`DrawRect` 可変幅と色のアウトラインで塗りつぶされた四角形を作成します。

コード例: 


```csharp
var shape = new CCRect (
    0, 0, 100, 200);
drawNode.DrawRect(shape,
    fillColor:CCColor4B.Blue,
    borderWidth: 4,
    borderColor:CCColor4B.White); 
```

![](ccdrawnode-images/image13.png "DrawRect の可変幅と色を外枠付きで塗りつぶされた四角形を作成します。")


### <a name="drawsegment"></a>DrawSegment

`DrawSegment` 可変幅と色の線で 2 つのポイントに接続します。 似ています`DrawLine`。 ただし、フラットなエンドポイントではなくラウンド エンドポイントが作成されます。

コード例:


```csharp
drawNode.DrawSegment (from: new CCPoint (0, 0),
    to: new CCPoint (100, 200),
    radius: 5,
    color:new CCColor4F(1,1,1,1)); 
```

![](ccdrawnode-images/image14.png "DrawSegment 可変幅と色の線で 2 つのポイントを接続します。")


### <a name="drawsolidarc"></a>DrawSolidArc

`DrawSolidArc` 指定した色と半径の塗りつぶされたくさび形を作成します。

コード例:


```csharp
drawNode.DrawSolidArc(
    pos:new CCPoint(100, 100),
    radius:200,
    startAngle:0,
    sweepAngle:CCMathHelper.Pi/2, // this is in radians, clockwise
    color:CCColor4B.White); 
```

![](ccdrawnode-images/image15.png "DrawSolidArc は、特定の色と半径の塗りつぶされたくさび形を作成します。")


### <a name="drawsolidcircle"></a>DrawSolidCircle

`DrawCircle` 指定した半径の塗りつぶされた円を作成します。

コード例:


```csharp
drawNode.DrawSolidCircle(
    pos: new CCPoint (100, 100),
    radius: 50,
    color: CCColor4B.Yellow); 
```

![](ccdrawnode-images/image16.png "DrawCircle は、指定した半径の塗りつぶされた円を作成します。")


### <a name="drawtrianglelist"></a>DrawTriangleList

`DrawTriangleList` 三角形の一覧を作成します。 各三角形が 3 で定義されている`CCV3F_C4B`配列内のインスタンス。 渡される配列内の頂点の数、`verts`パラメーターは 3 つの倍数である必要があります。 唯一の情報に含まれるので注意`CCV3F_C4B`、verts と – の色の位置、`DrawTriangleList`メソッドでは、テクスチャの三角形を描画することはできません。

コード例:


```csharp
CCV3F_C4B[] verts = new CCV3F_C4B[] {
    // First triangle:
    new CCV3F_C4B( new CCPoint(0,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(30,60), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(60,0), CCColor4B.White),
    // second triangle, each point has different colors:
    new CCV3F_C4B( new CCPoint(90,0), CCColor4B.Yellow),
    new CCV3F_C4B( new CCPoint(120,60), CCColor4B.Red),
    new CCV3F_C4B( new CCPoint(150,0), CCColor4B.Blue)
};

drawNode.DrawTriangleList (verts); 
```

![](ccdrawnode-images/image17.png "DrawTriangleList 三角形の一覧を作成します")


## <a name="summary"></a>まとめ

このガイドを作成する方法について説明、`CCDrawNode`プリミティブ ベースのレンダリングを実行します。 各描画呼び出しの例を提供します。

## <a name="related-links"></a>関連リンク

- [CCDrawNode API](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode/)
- [完全なサンプル](https://developer.xamarin.com/samples/mobile/CCDrawNode/)
