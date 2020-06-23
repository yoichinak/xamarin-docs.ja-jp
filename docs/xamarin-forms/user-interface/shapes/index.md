---
title: Xamarin.Forms図形
description: Xamarin.Forms図形は、画面に図形を描画できるようにするビューの種類です。
ms.prod: xamarin
ms.assetid: 4E749FE8-852C-46DA-BB1E-652936106357
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/22/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 13c79d7597325a3bf8dbabfa2983d55a92309c4b
ms.sourcegitcommit: dc49ba58510eeb52048a866e5d3daf5f1f68fbd2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/22/2020
ms.locfileid: "85130883"
---
# <a name="xamarinforms-shapes"></a>Xamarin.Forms図形

![](~/media/shared/preview.png "This API is currently pre-release")

は、 `Shape` [`View`](xref:Xamarin.Forms.View) 画面に図形を描画できるようにするの型です。 `Shape`オブジェクトは、クラスがクラスから派生するため、レイアウトクラスやほとんどのコントロール内で使用でき `Shape` `View` ます。

Xamarin.Forms図形は、 `Xamarin.Forms.Shapes` iOS、Android、macOS、ユニバーサル Windows プラットフォーム (UWP)、および Windows Presentation Foundation (WPF) の名前空間で使用できます。

> [!IMPORTANT]
> Xamarin.Forms図形は現在試験段階であり、フラグを設定することによってのみ使用でき `Shapes_Experimental` ます。 詳細については、「試験的な[フラグ](~/xamarin-forms/internals/experimental-flags.md)」を参照してください。

`Shape` は次の特性を定義します。

- `Aspect`型のは、 `Stretch` 割り当てられた領域を形状がどのように塗りつぶすかを記述します。 このプロパティの既定値は `Stretch.None` です。
- `Fill`型のは、 [`Color`](xref:Xamarin.Forms.Color) 図形の内部を描画するために使用する色を示します。
- `Stroke`型のは、 [`Color`](xref:Xamarin.Forms.Color) 図形の輪郭の描画に使用する色を示します。
- `StrokeDashArray`型の。 `DoubleCollection` これ `double` は、形状のアウトラインに使用される破線のパターンを示す値のコレクションを表します。
- `StrokeDashOffset`型のは、ダッシュ `double` を開始する破線パターン内の距離を指定します。 このプロパティの既定値は0.0 です。
- `StrokeLineCap`型のは、 `PenLineCap` 直線またはセグメントの先頭および末尾にある図形を記述します。 このプロパティの既定値は `PenLineCap.Flat` です。
- `StrokeLineJoin`型のは、 `PenLineJoin` 図形の頂点で使用される結合の種類を指定します。 このプロパティの既定値は `PenLineJoin.Miter` です。
- `StrokeThickness`型のは、 `double` 図形のアウトラインの幅を示します。 このプロパティの既定値は1.0 です。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

Xamarin.Formsクラスから派生するオブジェクトの数を定義 `Shape` します。 これらは、、、、、 `Ellipse` `Line` `Path` `Polygon` `Polyline` 、および `Rectangle` です。

## <a name="paint-shapes"></a>描画図形

[`Color`](xref:Xamarin.Forms.Color)オブジェクトは、図形のおよびを描画するために使用され `Stroke` `Fill` ます。

```xaml
<Ellipse Fill="DarkBlue"
         Stroke="Red"
         StrokeThickness="4"
         WidthRequest="150"
         HeightRequest="50"
         HorizontalOptions="Start" />
```

この例では、のストロークと塗りつぶし `Ellipse` が指定されています。

![描画図形](images/ellipse.png "描画図形")

> [!IMPORTANT]
> に値を指定しなかった場合、 [`Color`](xref:Xamarin.Forms.Color) `Stroke` またはを0に設定した場合、 `StrokeThickness` 図形の周りの境界線は描画されません。

有効な値の詳細について [`Color`](xref:Xamarin.Forms.Color) は、「[の Xamarin.Forms 色](~/xamarin-forms/user-interface/colors.md)」を参照してください。

## <a name="stretch-shapes"></a>拡張図形

`Shape`オブジェクトには `Aspect` 、型のプロパティがあり `Stretch` ます。 このプロパティは、オブジェクトの `Shape` レイアウト空間を埋めるために、オブジェクトの内容をどのように拡大するかを決定し `Shape` ます。 `Shape`オブジェクトのレイアウト空間は、が `Shape` レイアウトシステムによって割り当てられる領域の量です Xamarin.Forms 。これは、明示的な `WidthRequest` 設定と設定、 `HeightRequest` またはとの設定が原因 `HorizontalOptions` `VerticalOptions` であるためです。

`Stretch` 列挙体を使って、次のメンバーを定義できます。

- `None`。コンテンツの元のサイズが保持されることを示します。 これは、`Shape.Aspect` プロパティの既定値です。
- `Fill`。コピー先のディメンションを埋めるようにコンテンツのサイズが変更されることを示します。 縦横比は維持されません。
- `Uniform`。縦横比を維持したまま、コピー先のディメンションに合わせてコンテンツのサイズが変更されることを示します。
- `UniformToFill`は、縦横比を維持したまま、コピー先のディメンションを埋めるようにコンテンツのサイズを変更することを示します。 ソース コンテンツの縦横比が対象の四角形の縦横比と異なる場合は、ソース コンテンツが対象の四角形に収まるように切り取られます。

次の XAML は、プロパティを設定する方法を示してい `Aspect` ます。

```xaml
<Path Aspect="Uniform"
      Stroke="Yellow"
      StrokeThickness="1"
      Fill="Red"
      BackgroundColor="LightGray"
      HorizontalOptions="Start"
      HeightRequest="100"
      WidthRequest="100">
    <Path.Data>
        <!-- Path data goes here -->
    </Path.Data>  
</Path>      
```

この例では、 `Path` オブジェクトはハートを描画します。 `Path`オブジェクトの `WidthRequest` プロパティとプロパティは、 `HeightRequest` デバイスに依存しない100に設定され、その `Aspect` プロパティはに設定され `Uniform` ます。 その結果、オブジェクトの内容は、縦横比を維持したまま、変換先のディメンションに合わせてサイズが変更されます。

![拡張図形](images/aspect.png "拡張図形")

## <a name="related-links"></a>関連リンク

- [図形のデモ (サンプル)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)
- [色Xamarin.Forms](~/xamarin-forms/user-interface/colors.md)
