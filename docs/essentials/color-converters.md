---
title: Xamarin.Essentials Color Converters
description: Xamarin.Essentials の ColorConverters クラスには、System.Drawing.Color と併用できるヘルパー メソッドと拡張メソッドがいくつかあります。
ms.assetid: B10428D6-89E2-4714-A39F-7E6E626391B2
author: jamesmontemagno
ms.author: jamont
ms.date: 03/13/2019
ms.openlocfilehash: 5f26edf9515be79660574de0ae621daab3d1ea7d
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70756910"
---
# <a name="xamarinessentials-color-converters"></a>Xamarin.Essentials:Color Converters

Xamarin.Essentials の **ColorConverters** クラスには、System.Drawing.Color 用のヘルパー メソッドがいくつかあります。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-color-converters"></a>Color Converters の使用

自分のクラスに Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

`System.Drawing.Color` の使用時、Xamarin.Forms の組み込みコンバーターを使用し、Hsl、Hex、UInt から色を作成できます。

```csharp
var blueHex = ColorConverters.FromHex("#3498db");
var blueHsl = ColorConverters.FromHsl(204, 70, 53);
var blueUInt = ColorConverers.FromUInt(3447003);
```

## <a name="using-color-extensions"></a>Color Extensions の使用

`System.Drawing.Color` の拡張メソッドを利用すると、さまざまなプロパティを適用できます。

```csharp
var blue = ColorConverters.FromHex("#3498db");

// Multiplies the current alpha by 50%
var blueWithAlpha = blue.MultiplyAlpha(.5f);
```

拡張メソッドには他にも次のようなものがあります。

- ToUInt
- MultiplyAlpha
- WithHue
- WithAlpha
- WithSaturation
- WithLuminosity

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
