---
title: "Xamarin.Essentials 色のコンバーター" の説明: "Xamarin.Essentials の ColorConverters クラスには、System.Drawing.Color と連携するヘルパー メソッドと拡張メソッドがいくつか用意されています。"
ms.assetid: B10428D6-89E2-4714-A39F-7E6E626391B2 author: jamesmontemagno ms.author: jamont ms.date: 01/06/2020 ms.custom: video no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinessentials-color-converters"></a>Xamarin.Essentials:色のコンバーター

Xamarin.Essentials の **ColorConverters** クラスには、System.Drawing.Color 用のヘルパー メソッドがいくつかあります。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-color-converters"></a>Color Converters の使用

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

`System.Drawing.Color` の使用時には、Xamarin.Forms の組み込みコンバーターを使用し、Hsl、Hex、UInt から色を作成できます。

```csharp
var blueHex = ColorConverters.FromHex("#3498db");
var blueHsl = ColorConverters.FromHsl(204, 70, 53);
var blueUInt = ColorConverters.FromUInt(3447003);
```

## <a name="using-color-extensions"></a>Color Extensions の使用

`System.Drawing.Color` の拡張メソッドを利用すると、さまざまなプロパティを適用できます。

```csharp
var blue = ColorConverters.FromHex("#3498db");

// Multiplies the current alpha by 50%
var blueWithAlpha = blue.MultiplyAlpha(.5f);
```

拡張メソッドには他にも次のようなものがあります。

- GetComplementary
- MultiplyAlpha
- ToUInt
- WithAlpha
- WithHue
- WithLuminosity
- WithSaturation

## <a name="using-platform-extensions"></a>プラットフォーム拡張の使用

また、System.Drawing.Color をプラットフォーム固有の色構造に変換できます。 これらのメソッドは iOS、Android、UWP プロジェクトからのみ呼び出すことができます。

```csharp
var system = System.Drawing.Color.FromArgb(255, 52, 152, 219);

// Extension to convert to Android.Graphics.Color, UIKit.UIColor, or Windows.UI.Color
var platform = system.ToPlatformColor();
```

```csharp
var platform = new Android.Graphics.Color(52, 152, 219, 255);

// Back to System.Drawing.Color
var system = platform.ToSystemColor();
```

`ToSystemColor` メソッドは Android.Graphics.Color、UIKit.UIColor、Windows.UI.Color に適用されます。

## <a name="api"></a>API

- [Color Converters のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Types/ColorConverters.shared.cs)
- [Color Converters の API ドキュメント](xref:Xamarin.Essentials.ColorConverters)
- [Color Extensions のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Types/ColorConverters.shared.cs)
- [Color Extensions の API ドキュメント](xref:Xamarin.Essentials.ColorExtensions)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Color-Converters-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
