---
title: Xamarin.Forms での色
description: Xamarin.Forms は、柔軟なクロスプラット フォーム対応の色クラスを提供します。 この記事では、色のクラス、およびその使用方法によって提供される機能について説明します。
ms.prod: xamarin
ms.assetid: 22288ABF-57BE-47A9-ACC3-AC604D787C46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2017
ms.openlocfilehash: a7acb300dbbd6daa02eace955066d3227834cf67
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103076"
---
# <a name="colors-in-xamarinforms"></a>Xamarin.Forms での色

_Xamarin.Forms は、柔軟なクロスプラット フォーム対応の色クラスを提供します。_

この記事でさまざまな方法について説明、`Color`クラスは、Xamarin.Forms で使用できます。

`Color`クラスは、さまざまな色のインスタンスを作成するメソッドを提供します。

-  **名前付きの色**-一般的な名前付きの色などのコレクション`Red`、 `Green`、および`Blue`します。
-  **FromHex** -文字列値を HTML では、次のような"00FF00"で使用される構文に似ています。 アルファは、最初の文字 ("CC00FF00") のペアとして必要に応じて指定できます。
-  **FromHsla** -色相、彩度と輝度`double`アルファ値は省略可能な (0.0 ~ 1.0) の値。
-  **FromRgb** -赤、緑、および青`int`値 (0 ~ 255)。
-  **FromRgba** -赤、緑、青、およびアルファ`int`値 (0 ~ 255)。
-  **FromUint** -1 つの設定`double`値を表す**argb**します。

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

## <a name="additional-methods"></a>追加のメソッド

`Color` インスタンスには、新しい色の作成に使用できる追加のメソッドが含まれます。

-  **AddLuminosity** -指定のデルタによって、明るさを変更することで、新しい色を返します。
-  **WithHue** -色合いを指定した値に置き換えて、新しい色を返します。
-  **WithLuminosity**の明るさを指定した値に置き換えて、新しい色を返します。
-  **WithSaturation** -鮮やかさを指定した値に置き換えて、新しい色を返します。
-  **MultiplyAlpha** -、アルファは、指定されたアルファ値を乗算することを変更することで、新しい色を返します。

## <a name="implicit-conversions"></a>暗黙の型変換

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

定義済みの色の名前または 16 進表現を次に示しますを使用して XAML での色は簡単に参照こともできます。

```xaml
<Label Text="Sea color" BackgroundColor="Aqua" />
<Label Text="RGB" BackgroundColor="#00FF00" />
<Label Text="Alpha plus RGB" BackgroundColor="#CC00FF00" />
<Label Text="Tiny RGB" BackgroundColor="#0F0" />
<Label Text="Tiny Alpha plus RGB" BackgroundColor="#C0F0" />
```

> [!NOTE]
> XAML のコンパイルを使用して、色の名前が大文字と小文字を区別しないため、小文字で記述することができます。 XAML のコンパイルの詳細については、次を参照してください。 [XAML コンパイル](~/xamarin-forms/xaml/xamlc.md)します。

## <a name="summary"></a>まとめ

Xamarin.Forms`Color`プラットフォームに対応した色の参照を作成するクラスを使用します。 これは、共有コードと XAML で使用できます。


## <a name="related-links"></a>関連リンク

- [ColorsSample](https://developer.xamarin.com/samples/WorkingWithColors)
- [バインド可能なピッカー (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindablePicker/)
