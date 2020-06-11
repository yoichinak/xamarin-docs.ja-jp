---
title: "SkiaSharp Lines and Paths" description: "この記事では、SkiaSharp を使用してアプリケーションで線とグラフィックスパスを描画する方法について説明 Xamarin.Forms し、サンプルコードを使用してこれを示します。"
ms. 製品: xamarin ms. assetid: 316A15FE-383D-4D06-8641-BAC7EE7474CA: xamarin-skiasharp author: davidbritch ms. author: dabritch ms. date: 03/10/2017 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="skiasharp-lines-and-paths"></a>SkiaSharp の線とパス

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp を使用して線とグラフィックスパスを描画する_

[前のセクション](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md)では、SkiaSharp クラスに、 `SKCanvas` 円、楕円、四角形、角丸四角形、テキスト、ビットマップを描画するためのメソッドがいくつか含まれていることを示しています。 このセクションでは、*グラフィックスパス*の作成とレンダリングに関連するさまざまなクラスについて説明します。

グラフィックスパスは、SkiaSharp で直線と曲線を描画するための最も一般的なアプローチです。 ここでは、オブジェクトを使用して [`SKPath`](xref:SkiaSharp.SKPath) 直線を描画する方法と、小さな直線のコレクション (*ポリライン*と呼ばれます) を使用して、アルゴリズムを定義できる曲線を描画する方法について説明します。 [**SkiaSharp の曲線とパス**](../curves/index.md)に関する後のセクションでは、でサポートされるさまざまな種類の曲線について説明し `SKPath` ます。

このセクションのすべてのサンプルプログラムは、 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)プログラムのホームページの見出し**とパス**、およびそのソリューションの[**paths**](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths)フォルダーの下に表示されます。

## <a name="lines-and-stroke-caps"></a>[線とストローク キャップ](lines.md)

SkiaSharp を使用して、異なるストロークキャップの線を描画する方法について説明します。

## <a name="path-basics"></a>[パスの基礎](paths.md)

`SKPath`直線と曲線を結合するための SkiaSharp オブジェクトを調べます。

## <a name="the-path-fill-types"></a>[パスの塗りつぶしの種類](fill-types.md)

SkiaSharp path fill の種類で使用できるさまざまな効果について説明します。

## <a name="polylines-and-parametric-equations"></a>[多角形やパラメーターの式](polylines.md)

SkiaSharp を使用して、パラメーター式を使用して定義できる任意の線を表示します。

## <a name="dots-and-dashes"></a>[ドットとダッシュ](dots.md)

SkiaSharp に点線と破線を描画した複雑な部分をマスターします。

## <a name="finger-painting"></a>[指による描画](finger-paint.md)

指を使用して、キャンバス上に描画します。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
