---
title: Xamarin.Forms図形
description: Xamarin.Forms図形は、画面に図形を描画できるようにするビューの種類です。
ms.prod: xamarin
ms.assetid: 4E749FE8-852C-46DA-BB1E-652936106357
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6f8cb91a3699ce7e18e7f4d2e891fafa1ff552c2
ms.sourcegitcommit: 34fa3086c55b1e01838419c930f839c20662c362
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990761"
---
# <a name="xamarinforms-shapes"></a>Xamarin.Forms図形

![](~/media/shared/preview.png "This API is currently pre-release")

は、 `Shape` [`View`](xref:Xamarin.Forms.View) 画面に図形を描画できるようにするの型です。 `Shape`オブジェクトは、クラスがクラスから派生するため、レイアウトクラスやほとんどのコントロール内で使用でき `Shape` `View` ます。

Xamarin.Forms図形は、 `Xamarin.Forms.Shapes` iOS、Android、macOS、ユニバーサル Windows プラットフォーム (UWP)、および Windows Presentation Foundation (WPF) の名前空間で使用できます。

> [!IMPORTANT]
> Xamarin.Forms図形は現在試験段階であり、フラグを設定することによってのみ使用でき `Shapes_Experimental` ます。 詳細については、「試験的な[フラグ](~/xamarin-forms/internals/experimental-flags.md)」を参照してください。

`Shape` は次の特性を定義します。

- `Aspect`型のは、 `Stretch` 割り当てられた領域を形状がどのように塗りつぶすかを記述します。 このプロパティの既定値は `Stretch.None` です。
- `Fill`型のは、 `Color` 図形の内部を描画するために使用する色を示します。
- `Stroke`型のは、 `Color` 図形の輪郭の描画に使用する色を示します。
- `StrokeDashArray`型の。 `DoubleCollection` これ `double` は、形状のアウトラインに使用される破線のパターンを示す値のコレクションを表します。
- `StrokeDashOffset`型のは、ダッシュ `double` を開始する破線パターン内の距離を指定します。 このプロパティの既定値は0.0 です。
- `StrokeLineCap`型のは、 `PenLineCap` 直線またはセグメントの先頭および末尾にある図形を記述します。 このプロパティの既定値は `PenLineCap.Flat` です。
- `StrokeLineJoin`型のは、 `PenLineJoin` 図形の頂点で使用される結合の種類を指定します。 このプロパティの既定値は `PenLineJoin.Miter` です。
- `StrokeThickness`型のは、 `double` 図形のアウトラインの幅を示します。 このプロパティの既定値は1.0 です。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

Xamarin.Formsクラスから派生するオブジェクトの数を定義 `Shape` します。 これらには、`Ellipse`、`Line`、`Path`、`Polygon`、`Polyline`、`Rectangle` が含まれます。

## <a name="related-links"></a>関連リンク

- [図形のデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
