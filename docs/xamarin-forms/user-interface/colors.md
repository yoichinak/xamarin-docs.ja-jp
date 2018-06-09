---
title: Xamarin.Forms の色
description: Xamarin.Forms では、柔軟なクロスプラット フォームの色クラスを提供します。 この記事では、色クラスとその使用方法によって提供される機能について説明します。
ms.prod: xamarin
ms.assetid: 22288ABF-57BE-47A9-ACC3-AC604D787C46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2017
ms.openlocfilehash: 45adcb8a0fe25e729211e8b166be51ce2c4d93bd
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243019"
---
# <a name="colors-in-xamarinforms"></a>Xamarin.Forms の色

_Xamarin.Forms では、柔軟なクロスプラット フォームの色クラスを提供します。_

この記事には、さまざまな方法が導入されています、 `Color` Xamarin.Forms でクラスを使用できます。

`Color`クラスには、多数の色のインスタンスを構築するメソッドが用意されています

-  **名前付きの色**-一般的な名前付きの色などのコレクション`Red`、 `Green`、および`Blue`です。
-  **FromHex** -文字列値、HTML、たとえば"00FF00"で使用される構文に似ています。 アルファは、必要に応じて、最初の文字 ("CC00FF00") のペアとして指定できます。
-  **FromHsla** -色相、彩度、光度`double`アルファ値は省略可能な (0.0 ~ 1.0) の値。
-  **FromRgb** -赤、緑、および青`int`値 (0 ~ 255) です。
-  **FromRgba** -赤、緑、青、およびアルファ`int`値 (0 ~ 255) です。
-  **FromUint** -1 つを設定`double`を表す値**argb**です。

ここでは、いくつかの例の色に割り当てられている、`BackgroundColor`許可されている構文のさまざまなバリエーションを使用していくつかのラベルの。

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

これらの色は、各プラットフォームの下に表示されます。 最終的な色に注意してください`Accent`-iOS および Android; blue-ish 色は、Xamarin.Forms でこの値が定義されています。

 [![色のデモ](colors-images/colors-sml.png "色デモ")](colors-images/colors.png#lightbox "色デモ")

## <a name="colordefault"></a>Color.Default

使用して、`Default`プラットフォームの既定値 (プロパティごとに各プラットフォームで別の基になる色を表すこの understanding) に色の値を設定 (または再設定されます)。

開発者は、この値を使用して設定する、`Color`プロパティ必要があります**いない**のコンポーネント RGB 値 (がすべて揃って-1) のこのインスタンスのクエリを実行します。

## <a name="colortransparent"></a>Color.Transparent

色をオフに設定します。

## <a name="coloraccent"></a>Color.Accent

IOS および Android では、このインスタンスは対照的な色が、既定の背景に表示が既定のテキストの色と同じではないに設定されます。

## <a name="additional-methods"></a>追加のメソッド

`Color` インスタンスには、新しい色の作成に使用できる追加のメソッドが含まれます。

-  **AddLuminosity** -指定されたデルタによって、明るさを変更することで新しい色を返します。
-  **WithHue**の色合いを指定した値に置き換えて、新しい色を返します。
-  **WithLuminosity**の明るさを指定した値に置き換えて、新しい色を返します。
-  **WithSaturation**の鮮やかさを指定した値に置き換えて、新しい色を返します。
-  **MultiplyAlpha** -英字、指定されたアルファ値で乗算を変更することで新しい色を返します。

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

## <a name="using-from-xaml"></a>XAML の使用

色参照することも簡単に定義されている色の名前または次に示すの 16 進形式を使用して XAML で。

```xaml
<Label Text="Sea color" BackgroundColor="Aqua" />
<Label Text="RGB" BackgroundColor="#00FF00" />
<Label Text="Alpha plus RGB" BackgroundColor="#CC00FF00" />
<Label Text="Tiny RGB" BackgroundColor="#0F0" />
<Label Text="Tiny Alpha plus RGB" BackgroundColor="#C0F0" />
```

## <a name="summary"></a>まとめ

Xamarin.Forms`Color`プラットフォームに対応する色の参照を作成するクラスを使用します。 これは、共有コードと XAML で使用できます。


## <a name="related-links"></a>関連リンク

- [ColorsSample](https://developer.xamarin.com/samples/WorkingWithColors)
- [バインド可能なピッカー (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindablePicker/)
