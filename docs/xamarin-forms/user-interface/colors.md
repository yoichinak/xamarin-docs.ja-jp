---
title: Xamarin.Forms での色
description: Xamarin.Forms は、柔軟なクロスプラット フォーム対応の色クラスを提供します。 この記事では、色のクラス、およびその使用方法によって提供される機能について説明します。
ms.prod: xamarin
ms.assetid: 22288ABF-57BE-47A9-ACC3-AC604D787C46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2019
ms.openlocfilehash: 2a17b037803d1ca6e54000ea7ba3f05c8ce6034f
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/21/2019
ms.locfileid: "69888472"
---
# <a name="colors-in-xamarinforms"></a>Xamarin.Forms での色

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithcolors)

_Xamarin.Forms は、柔軟なクロスプラット フォーム対応の色クラスを提供します。_

この記事では、Xamarin で[`Color`](xref:Xamarin.Forms.Color)クラスを使用するさまざまな方法を紹介します。

クラス[`Color`](xref:Xamarin.Forms.Color)には、color インスタンスを構築するためのさまざまなメソッドが用意されています。

- **名前付きの色**-一般的な名前付きの色などのコレクション`Red`、 `Green`、および`Blue`します。
- **Fromhex** -HTML で使用される構文に似た文字列値 ("00FF00" など)。 必要に応じて、最初の文字ペア ("CC00FF00") として Alpha を指定できます。
- **FromHsla** -色相、彩度と輝度`double`アルファ値は省略可能な (0.0 ~ 1.0) の値。
- **FromRgb** -赤、緑、および青`int`値 (0 ~ 255)。
- **FromRgba** -赤、緑、青、およびアルファ`int`値 (0 ~ 255)。
- **FromUint** -1 つの設定`double`値を表す**argb**します。

ここに割り当てられている、いくつかの例の色、`BackgroundColor`の許可されている構文のさまざまなバリエーションを使用して一部のラベル。

```csharp
var red    = new Label { Text = "Red",   BackgroundColor = Color.Red };
var orange = new Label { Text = "Orange",BackgroundColor = Color.FromHex("FF6A00") };
var yellow = new Label { Text = "Yellow",BackgroundColor = Color.FromHsla(0.167, 1.0, 0.5, 1.0) };
var green  = new Label { Text = "Green", BackgroundColor = Color.FromRgb (38, 127, 0) };
var blue   = new Label { Text = "Blue",  BackgroundColor = Color.FromRgba(0, 38, 255, 255) };
var indigo = new Label { Text = "Indigo",BackgroundColor = Color.FromRgb (0, 72, 255) };
var violet = new Label { Text = "Violet",BackgroundColor = Color.FromHsla(0.82, 1, 0.25, 1) };

var transparent = new Label { Text = "Transparent",BackgroundColor = Color.Transparent };
var @default = new Label    { Text = "Default",    BackgroundColor = Color.Default };
var accent = new Label      { Text = "Accent",     BackgroundColor = Color.Accent };
```

これらの色は、各プラットフォームの下に表示されます。 -最終的な色に注意してください`Accent`-iOS および Android 用 blue-ish 色は、この値は、Xamarin.Forms で定義されます。

 [![色のデモ](colors-images/colors-sml.png "色デモ")](colors-images/colors.png#lightbox "色のデモ")

## <a name="colordefault"></a>Color.Default

使用して、 `Default` (理解の各プロパティの各プラットフォームで別の基になる色を表す) プラットフォームの既定値に色の値を設定する (または再設定)。

開発者は、この値を使用して設定する、`Color`プロパティ必要があります**いない**(がすべて揃って-1) のコンポーネントの RGB 値のこのインスタンスのクエリを実行します。

## <a name="colortransparent"></a>Color.Transparent

クリアする色を設定します。

## <a name="coloraccent"></a>Color.Accent

IOS と Android では、このインスタンスが既定の背景に表示されますが、既定のテキスト色と同じコントラスト色に設定されます。

## <a name="additional-methods"></a>その他のメソッド

[`Color`](xref:Xamarin.Forms.Color)インスタンスには、次の追加メソッドが含まれます。

- **Addluminosity** -指定さ`Color`れたデルタによって輝度を変更することによって、を返します。
- **乗数 yalpha** -指定さ`Color`れたアルファ値を乗算して、アルファを変更することによってを返します。
- **Tohex** -の`Color`16 進数`string`表現を返します。
- **Withhue** -を`Color`返します。色合いを指定された値に置き換えます。
- **Withluminosity** -明るさを`Color`指定された値に置き換えて、を返します。
- **Withsaturation** -を`Color`返します。鮮やかさを指定された値に置き換えます。

## <a name="implicit-conversions"></a>暗黙の変換

間の暗黙的な変換、`Xamarin.Forms.Color`と`System.Drawing.Color`型を実行することができます。

```csharp
Xamarin.Forms.Color xfColor = Xamarin.Forms.Color.FromRgb(0, 72, 255);
System.Drawing.Color sdColor = System.Drawing.Color.FromArgb(38, 127, 0);

// Implicity convert from a Xamarin.Forms.Color to a System.Drawing.Color
System.Drawing.Color sdColor2 = xfColor;

// Implicitly convert from a System.Drawing.Color to a Xamarin.Forms.Color
Xamarin.Forms.Color xfColor2 = sdColor;
```

## <a name="deviceruntimeplatform"></a>Device.RuntimePlatform

このコード スニペットを使用して、`Device.RuntimePlatform`の色を選択的に設定するプロパティ、 `ActivityIndicator`:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator
{
    Color = Device.RuntimePlatform == Device.iOS ? Color.Black : Color.Default,
    IsRunning = true
};
```

## <a name="using-from-xaml"></a>XAML から使用します。

次に示す色の名前または16進表現を使用して、XAML で色を参照することもできます。

```xaml
<Label Text="Sea color" BackgroundColor="Aqua" />
<Label Text="RGB" BackgroundColor="#00FF00" />
<Label Text="Alpha plus RGB" BackgroundColor="#CC00FF00" />
<Label Text="Tiny RGB" BackgroundColor="#0F0" />
<Label Text="Tiny Alpha plus RGB" BackgroundColor="#C0F0" />
```

> [!NOTE]
> XAML のコンパイルを使用して、色の名前が大文字と小文字を区別しないため、小文字で記述することができます。 XAML のコンパイルの詳細については、[XAML のコンパイル](~/xamarin-forms/xaml/xamlc.md) を参照してください。

## <a name="related-links"></a>関連リンク

- [ColorsSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithcolors)
- [バインド可能なピッカー (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablepicker)
