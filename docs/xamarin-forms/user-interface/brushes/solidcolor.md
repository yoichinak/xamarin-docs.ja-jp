---
title: 'Xamarin.Forms ブラシ: 純色'
description: Xamarin.FormsSystem.windows.media.solidcolorbrush> クラスは、純色で領域を塗りつぶします。
ms.prod: xamarin
ms.assetid: 4225D40A-16C1-40E1-ACBE-23E321E7FDE4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/27/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3c3caf064ca550086f8e7924786ac8bcaf1badfc
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91556283"
---
# <a name="no-locxamarinforms-brushes-solid-colors"></a>Xamarin.Forms ブラシ: 純色

![プレビュー API](~/media/shared/preview.png "この API は現在プレリリースです")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-brushdemos/)

`SolidColorBrush`クラスはクラスから派生 `Brush` し、純色で領域を塗りつぶすために使用されます。 の色を指定するには、さまざまな方法があり `SolidColorBrush` ます。 たとえば、値を使用して色を指定することも、 [`Color`](xref:Xamarin.Forms.Color) クラスによって提供される定義済みオブジェクトのいずれかを使用することもでき `SolidColorBrush` `Brush` ます。

クラスは、 `SolidColorBrush` `Color` [`Color`](xref:Xamarin.Forms.Color) ブラシの色を表す型のプロパティを定義します。 このプロパティは、オブジェクトによって支えられています。これは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) データバインディングのターゲットとスタイルを設定できることを意味します。

`SolidColorBrush`クラスには、ブラシに色が `IsEmpty` `bool` 割り当てられているかどうかを表すを返すメソッドもあります。

## <a name="create-a-solidcolorbrush"></a>System.windows.media.solidcolorbrush> を作成する

を作成するには、主に3つの方法があり `SolidColorBrush` ます。 からを作成したり `SolidColorBrush` [`Color`](xref:Xamarin.Forms.Color) 、定義済みのブラシを使用したり、 `SolidColorBrush` 16 進表記を使用してを作成したりできます。

### <a name="use-a-predefined-color"></a>定義済みの色を使用する

Xamarin.Forms には、値からを作成する型コンバーターが含まれてい `SolidColorBrush` [`Color`](xref:Xamarin.Forms.Color) ます。 XAML では、これにより、 `SolidColorBrush` 定義済みの値からを作成でき `Color` ます。

```xaml
<Frame Background="DarkBlue"
       BorderColor="LightGray"
       HasShadow="True"
       CornerRadius="12"
       HeightRequest="120"
       WidthRequest="120" />
```

この例では、の背景 [`Frame`](xref:Xamarin.Forms.Frame) が濃い青で塗りつぶされてい `SolidColorBrush` ます。

![定義済みの色で塗りつぶされたフレーム](solidcolor-images/predefined-color.png)

または、 [`Color`](xref:Xamarin.Forms.Color) プロパティタグ構文を使用して値を指定することもできます。

```xaml
<Frame BorderColor="LightGray"
       HasShadow="True"
       CornerRadius="12"
       HeightRequest="120"
       WidthRequest="120">
       <Frame.Background>
           <SolidColorBrush Color="DarkBlue" />
       </Frame.Background>
</Frame>
```

この例では、 [`Frame`](xref:Xamarin.Forms.Frame) `SolidColorBrush` プロパティを設定することによって色が指定されたを使用して、の背景が描画され `SolidColorBrush.Color` ます。

### <a name="use-a-predefined-brush"></a>定義済みのブラシを使用する

クラスは、 `Brush` 一般的に使用されるオブジェクトのセットを定義し `SolidColorBrush` ます。 次の例では、これらの定義済みオブジェクトのいずれかを使用し `SolidColorBrush` ます。

```xaml
<Frame Background="{x:Static Brush.Indigo}"
       BorderColor="LightGray"
       HasShadow="True"
       CornerRadius="12"
       HeightRequest="120"
       WidthRequest="120" />       
```

これに相当する C# コードを次に示します。

```csharp
Frame frame = new Frame
{
    Background = Brush.Indigo,
    BorderColor = Color.LightGray,
    // ...
};
```

この例では、の背景 [`Frame`](xref:Xamarin.Forms.Frame) が indigo で描画され `SolidColorBrush` ます。

![定義済みの System.windows.media.solidcolorbrush> で塗りつぶされたフレーム](solidcolor-images/predefined-brush.png)

`SolidColorBrush`クラスによって提供される定義済みオブジェクトの一覧につい `Brush` ては、「[純色ブラシ](#solid-color-brushes)」を参照してください。

### <a name="use-hexadecimal-notation"></a>16進表記を使用する

`SolidColorBrush` オブジェクトは、16進数表記を使用して作成することもできます。 この方法では、色が赤、緑、および青の値で1色に結合されます。 16進表記を使用して色を指定するための主な形式は `#rrggbb` 、次のとおりです。

- `rr` は、相対的な赤の値を指定する2桁の16進数です。
- `gg` は、緑の相対値を指定する2桁の16進数値です。
- `bb` は、青の相対的な金額を指定する2桁の16進数です。

また、色を指定することもでき `#aarrggbb` `aa` ます。ここでは、色のアルファ値 (透明度) を指定します。 この方法により、部分的に透明な色を作成することができます。

次の例では、 `SolidColorBrush` 16 進表記を使用しての色の値を設定します。

```xaml
<Frame Background="#FF9988"
       BorderColor="LightGray"
       HasShadow="True"
       CornerRadius="12"
       HeightRequest="120"
       WidthRequest="120" />
```

この例では、の背景 [`Frame`](xref:Xamarin.Forms.Frame) がピンク色で塗りつぶされてい `SolidColorBrush` ます。

![16進表記を使用して作成された System.windows.media.solidcolorbrush> で塗りつぶされたフレーム](solidcolor-images/hex.png)

色を記述するその他の方法については、「 [」の Xamarin.Forms 「色](~/xamarin-forms/user-interface/colors.md)」を参照してください。

## <a name="solid-color-brushes"></a>単色ブラシ

便宜上、クラスは、 `Brush` やなどの一般的に使用されるオブジェクトのセットを提供し `SolidColorBrush` `AliceBlue` `YellowGreen` ます。 次の図は、定義済みの各ブラシの色、名前、および16進値を示しています。

[![色の見本、色の名前、16進数の値を含むカラーテーブル](solidcolor-images/solidcolorbrushes.png)](solidcolor-images/solidcolorbrushes-large.png#lightbox)

## <a name="related-links"></a>関連リンク

- [BrushesDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-brushdemos/)
- [色 Xamarin.Forms](~/xamarin-forms/user-interface/colors.md)