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
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305757"
---
# <a name="colors-in-xamarinforms"></a>Xamarin.Forms での色

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithcolors)

_Xamarin Forms は、柔軟なクロスプラットフォームカラークラスを提供します。_

この記事では、 [`Color`](xref:Xamarin.Forms.Color)クラスを Xamarin. Forms で使用するさまざまな方法について説明します。

[`Color`](xref:Xamarin.Forms.Color)クラスには、color インスタンスを構築するためのメソッドがいくつか用意されています。

- **名前**付きの色-`Red`、`Green`、`Blue`を含む、共通の名前付きの色のコレクション。
- **Fromhex** -HTML で使用される構文に似た文字列値 ("00FF00" など)。 必要に応じて、最初の文字ペア ("CC00FF00") として Alpha を指定できます。
- **Fromhsla** -色合い、鮮やかさ、および明度は、オプションのアルファ値 (0.0-1.0) を使用して `double` ます。
- **Fromrgb** -赤、緑、および青 `int` 値 (0-255)。
- **Fromrgba** -赤、緑、青、およびアルファ `int` 値 (0-255)。
- **Fromuint** - **argb**を表す単一の `double` 値を設定します。

次に、許可されている構文のさまざまなバリエーションを使用して、いくつかのラベルの `BackgroundColor` に割り当てられた色の例を示します。

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

これらの色は、各プラットフォームの下に表示されます。 最終的な色の `Accent`-iOS と Android では青の色が使用されていることに注意してください。この値は、Xamarin. Forms によって定義されます。

 [![色のデモ](colors-images/colors-sml.png "色のデモ")](colors-images/colors.png#lightbox "色のデモ")

## <a name="colordefault"></a>Color.Default

`Default` を使用して、カラー値をプラットフォームの既定値に戻し (または再設定) します (これは、各プロパティの各プラットフォームで基になる色が異なることを理解しています)。

開発者はこの値を使用して `Color` プロパティを設定できますが、コンポーネントの RGB 値に対してこのインスタンスを照会することはできません (すべて-1 に設定**されてい**ます)。

## <a name="colortransparent"></a>Color.Transparent

クリアする色を設定します。

## <a name="coloraccent"></a>Color.Accent

IOS と Android では、このインスタンスが既定の背景に表示されますが、既定のテキスト色と同じコントラスト色に設定されます。

## <a name="additional-methods"></a>その他のメソッド

[`Color`](xref:Xamarin.Forms.Color)インスタンスには、次の追加のメソッドがあります。

- **Addluminosity** -指定されたデルタによって輝度を変更することによって、`Color` を返します。
- **乗数 yalpha** -指定されたアルファ値で乗算して、アルファを変更することによって `Color` を返します。
- **Tohex** -`Color`の16進数の `string` 表現を返します。
- **Withhue** -`Color`を返します。色合いを指定された値に置き換えます。
- **Withluminosity** -`Color`を返します。明るさを指定された値に置き換えます。
- **Withsaturation** -`Color`を返します。鮮やかさを指定された値に置き換えます。

## <a name="implicit-conversions"></a>暗黙的な変換

`Xamarin.Forms.Color` 型と `System.Drawing.Color` 型の間の暗黙的な変換は、次のように実行できます。

```csharp
Xamarin.Forms.Color xfColor = Xamarin.Forms.Color.FromRgb(0, 72, 255);
System.Drawing.Color sdColor = System.Drawing.Color.FromArgb(38, 127, 0);

// Implicity convert from a Xamarin.Forms.Color to a System.Drawing.Color
System.Drawing.Color sdColor2 = xfColor;

// Implicitly convert from a System.Drawing.Color to a Xamarin.Forms.Color
Xamarin.Forms.Color xfColor2 = sdColor;
```

## <a name="deviceruntimeplatform"></a>Device.RuntimePlatform

このコードスニペットでは、`Device.RuntimePlatform` プロパティを使用して、`ActivityIndicator`の色を選択的に設定します。

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
> XAML のコンパイルを使用して、色の名前が大文字と小文字を区別しないため、小文字で記述することができます。 XAML のコンパイルの詳細については、[XAML のコンパイル](~/xamarin-forms/xaml/xamlc.md)に関するページを参照してください。

## <a name="related-links"></a>関連リンク

- [ColorsSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithcolors)
- [バインド可能なピッカー (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablepicker)
