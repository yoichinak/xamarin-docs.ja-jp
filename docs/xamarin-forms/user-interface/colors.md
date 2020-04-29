---
title: Xamarin 形式の色
description: Xamarin Forms は、柔軟なクロスプラットフォームカラークラスを提供します。 この記事では、Color クラスによって提供される機能とその使用方法について説明します。
ms.prod: xamarin
ms.assetid: 22288ABF-57BE-47A9-ACC3-AC604D787C46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/02/2020
ms.openlocfilehash: 42b532b8565d2d8e0289b8fd446e1dd7762a09ac
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517034"
---
# <a name="colors-in-xamarinforms"></a>Xamarin 形式の色

[![](~/media/shared/download.png)サンプルをダウンロードするサンプルをダウンロードする](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithcolors)

_Xamarin Forms は、柔軟なクロスプラットフォームカラークラスを提供します。_

この記事では、Xamarin で[`Color`](xref:Xamarin.Forms.Color)クラスを使用するさまざまな方法を紹介します。

クラス[`Color`](xref:Xamarin.Forms.Color)には、インスタンスを`Color`構築するためのメソッドがいくつか用意されています。

- **名前**付きの色- `Red`、 `Green`、など、一般的な名前付きの色`Blue`のコレクション。
- `FromHex`-HTML で使用される構文 ("00FF00" など) に類似した文字列値。 必要に応じて、最初の文字ペア ("CC00FF00") として Alpha を指定できます。
- `FromHsla`-色合い、鮮やかさ、および`double`明るさの値 (オプションのアルファ値 (0.0 ~ 1.0))。
- `FromHsv`-色相、鮮やかさ、値`int`または`double`値。
- `FromHsva`-色相、鮮やかさ、値`int`または`double`値。
- `FromRgb`-赤、緑、および青`int`の値 (0-255)。
- `FromRgba`-赤、緑、青、およびアルファ`int`値 (0-255)。
- `FromUint`-argb を表す`double`単一の**argb**値を設定します。

次に、許可されている構文`BackgroundColor`のさまざまなバリエーションを使用して、一部のラベルのに割り当てられている色の例を示します。

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

これらの色は、以下のプラットフォームごとに表示されます。 最終的な色`Accent`に注意してください。 IOS と Android では、青の色が使用されます。この値は、Xamarin. Forms によって定義されます。

 [![色のデモ](colors-images/colors-sml.png "色のデモ")](colors-images/colors.png#lightbox "色のデモ")

## <a name="colordefault"></a>色。既定値

を使用`Default`して、色の値をプラットフォームの既定値に戻します (または再設定します)。これは、各プロパティのプラットフォームごとに、基になる色が異なることを理解しています。

開発者はこの値を使用し`Color`てプロパティを設定できますが、コンポーネントの RGB 値に対してこのインスタンスのクエリを実行することはできません (すべて-1 に設定**されてい**ます)。

## <a name="colortransparent"></a>色透明

色をクリアに設定します。

## <a name="coloraccent"></a>色。アクセント

IOS および Android では、このインスタンスは、既定の背景に表示されるが、既定のテキストの色と同じではないコントラストの色に設定されます。

## <a name="additional-methods"></a>その他のメソッド

[`Color`](xref:Xamarin.Forms.Color)インスタンスには、次の追加メソッドが含まれます。

- `AddLuminosity`-指定さ`Color`れたデルタによって輝度を変更することによって、を返します。
- `MultiplyAlpha`-指定さ`Color`れたアルファ値で乗算されたアルファを変更することによって、を返します。
- `ToHex`-の16進数`string`表現を返し`Color`ます。
- `WithHue`-を返し`Color`ます。色合いを指定された値に置き換えます。
- `WithLuminosity`-を返し`Color`ます。これは、指定された値で明るさを置き換えます。
- `WithSaturation`-を返し`Color`ます。鮮やかさを指定された値に置き換えます。

## <a name="implicit-conversions"></a>暗黙の変換

型と`System.Drawing.Color`型の`Xamarin.Forms.Color`間の暗黙的な変換は、次のように実行できます。

```csharp
Xamarin.Forms.Color xfColor = Xamarin.Forms.Color.FromRgb(0, 72, 255);
System.Drawing.Color sdColor = System.Drawing.Color.FromArgb(38, 127, 0);

// Implicity convert from a Xamarin.Forms.Color to a System.Drawing.Color
System.Drawing.Color sdColor2 = xfColor;

// Implicitly convert from a System.Drawing.Color to a Xamarin.Forms.Color
Xamarin.Forms.Color xfColor2 = sdColor;
```

## <a name="deviceruntimeplatform"></a>デバイス. RuntimePlatform

このコードスニペットでは`Device.RuntimePlatform` 、プロパティを使用して、 `ActivityIndicator`の色を選択的に設定します。

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator
{
    Color = Device.RuntimePlatform == Device.iOS ? Color.Black : Color.Default,
    IsRunning = true
};
```

## <a name="use-from-xaml"></a>XAML からの使用

次に示す色の名前または16進表現を使用して、XAML で色を参照することもできます。

```xaml
<Label Text="Sea color" BackgroundColor="Aqua" />
<Label Text="RGB" BackgroundColor="#00FF00" />
<Label Text="Alpha plus RGB" BackgroundColor="#CC00FF00" />
<Label Text="Tiny RGB" BackgroundColor="#0F0" />
<Label Text="Tiny Alpha plus RGB" BackgroundColor="#C0F0" />
```

> [!NOTE]
> XAML コンパイルを使用する場合、色名は大文字と小文字が区別されないため、小文字で記述できます。 XAML のコンパイルの詳細については、[XAML のコンパイル](~/xamarin-forms/xaml/xamlc.md)に関するページを参照してください。

## <a name="related-links"></a>関連リンク

- [ColorsSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithcolors)
- [バインド可能なピッカー (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablepicker)
