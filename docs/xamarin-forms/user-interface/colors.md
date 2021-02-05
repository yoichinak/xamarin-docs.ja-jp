---
title: 色 Xamarin.Forms
description: Xamarin.Forms 柔軟なクロスプラットフォームカラークラスを提供します。 この記事では、Color クラスによって提供される機能とその使用方法について説明します。
ms.prod: xamarin
ms.assetid: 22288ABF-57BE-47A9-ACC3-AC604D787C46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/02/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6018946f280afa3f02d8f81bfc64338e561950fe
ms.sourcegitcommit: 10c7dd16fe78226053d1d036492b6c9102fc421b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/05/2021
ms.locfileid: "93375292"
---
# <a name="colors-in-xamarinforms"></a>色 Xamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/workingwithcolors)

_Xamarin.Forms 柔軟なクロスプラットフォームカラークラスを提供します。_

この記事では、でクラスを使用するさまざまな方法について説明し [`Color`](xref:Xamarin.Forms.Color) Xamarin.Forms ます。

クラスには、 [`Color`](xref:Xamarin.Forms.Color) インスタンスを構築するためのメソッドがいくつか用意されてい `Color` ます。

- **名前** 付きの色-、、など、一般的な名前付きの色のコレクション `Red` `Green` `Blue` 。
- `FromHex` -HTML で使用される構文 ("00FF00" など) に類似した文字列値。 必要に応じて、最初の文字ペア ("CC00FF00") として Alpha を指定できます。
- `FromHsla` -色合い、鮮やかさ、および明るさの `double` 値 (オプションのアルファ値 (0.0 ~ 1.0))。
- `FromHsv` -色相、鮮やかさ、値 `int` または値 `double` 。
- `FromHsva` -色相、鮮やかさ、値 `int` または値 `double` 。
- `FromRgb` -赤、緑、および青の `int` 値 (0-255)。
- `FromRgba` -赤、緑、青、およびアルファ  `int` 値 (0-255)。
- `FromUint` - `double` **argb** を表す単一の値を設定します。

次に、 `BackgroundColor` 許可されている構文のさまざまなバリエーションを使用して、一部のラベルのに割り当てられている色の例を示します。

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

これらの色は、以下のプラットフォームごとに表示されます。 最終的な色は、iOS と Android の青色の色であることに注意して `Accent` ください。この値はによって定義され Xamarin.Forms ます。

 [![色のデモ](colors-images/colors-sml.png "色のデモ")](colors-images/colors.png#lightbox "色のデモ")

## <a name="colordefault"></a>色。既定値

を使用して、 `Default` 色の値をプラットフォームの既定値に戻します (または再設定します)。これは、各プロパティのプラットフォームごとに、基になる色が異なることを理解しています。

開発者はこの値を使用し `Color` てプロパティを設定できますが、コンポーネントの RGB 値に対してこのインスタンスのクエリを実行することはできません (すべて-1 に設定 **されてい** ます)。

## <a name="colortransparent"></a>色透明

色をクリアに設定します。

## <a name="coloraccent"></a>色。アクセント

IOS および Android では、このインスタンスは、既定の背景に表示されるが、既定のテキストの色と同じではないコントラストの色に設定されます。

## <a name="additional-methods"></a>他の方法

[`Color`](xref:Xamarin.Forms.Color) インスタンスには、次の追加メソッドが含まれます。

- `AddLuminosity` -指定された `Color` デルタによって輝度を変更することによって、を返します。
- `MultiplyAlpha` -指定された `Color` アルファ値で乗算されたアルファを変更することによって、を返します。
- `ToHex` -の16進数 `string` 表現を返し `Color` ます。
- `WithHue` -を返し `Color` ます。色合いを指定された値に置き換えます。
- `WithLuminosity` -を返します。これは、指定された `Color` 値で明るさを置き換えます。
- `WithSaturation` -を返し `Color` ます。鮮やかさを指定された値に置き換えます。

## <a name="implicit-conversions"></a>暗黙的な変換

型と型の間の暗黙的な変換は、次のように `Xamarin.Forms.Color` `System.Drawing.Color` 実行できます。

```csharp
Xamarin.Forms.Color xfColor = Xamarin.Forms.Color.FromRgb(0, 72, 255);
System.Drawing.Color sdColor = System.Drawing.Color.FromArgb(38, 127, 0);

// Implicity convert from a Xamarin.Forms.Color to a System.Drawing.Color
System.Drawing.Color sdColor2 = xfColor;

// Implicitly convert from a System.Drawing.Color to a Xamarin.Forms.Color
Xamarin.Forms.Color xfColor2 = sdColor;
```

## <a name="deviceruntimeplatform"></a>デバイス. RuntimePlatform

このコードスニペットでは、プロパティを使用して `Device.RuntimePlatform` 、の色を選択的に設定し `ActivityIndicator` ます。

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

- [ColorsSample](/samples/xamarin/xamarin-forms-samples/workingwithcolors)
- [バインド可能なピッカー (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-bindablepicker)