---
title: "Xamarin.Essentialsユニット コンバーター" の説明: "Xamarin.Essentials の UnitConverters クラスは単位変換機能をいくつか備えており、Xamarin.Essentials の使用時に開発者を支援します。"
ms.assetid:35DE2704-E730-4337-9476-66CD53376943 author: jamesmontemagno ms.custom: video ms.author: jamont ms.date:01/06/2020 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinessentials-unit-converters"></a>Xamarin.Essentials:単位コンバーター

**UnitConverters** クラスは単位変換機能をいくつか備えており、Xamarin.Essentials の使用時に開発者を支援します。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-unit-converters"></a>Unit Converters の使用

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

単位コンバーターはすべて、Xamarin.Essentials の静的 `UnitConverters` クラスの使用時に利用できます。 たとえば、華氏を摂氏に簡単に変換できます。

```csharp
var celsius = UnitConverters.FahrenheitToCelsius(32.0);
```

利用できる変換の一覧:

- FahrenheitToCelsius
- CelsiusToFahrenheit
- CelsiusToKelvin
- KelvinToCelsius
- MilesToMeters
- MilesToKilometers
- KilometersToMiles
- MetersToInternationalFeet
- InternationalFeetToMeters
- DegreesToRadians
- RadiansToDegrees
- DegreesPerSecondToRadiansPerSecond
- RadiansPerSecondToDegreesPerSecond
- DegreesPerSecondToHertz
- RadiansPerSecondToHertz
- HertzToDegreesPerSecond
- HertzToRadiansPerSecond
- KilopascalsToHectopascals
- HectopascalsToKilopascals
- KilopascalsToPascals
- HectopascalsToPascals
- AtmospheresToPascals
- PascalsToAtmospheres
- CoordinatesToMiles
- CoordinatesToKilometers
- KilogramsToPounds
- PoundsToKilograms
- StonesToPounds
- PoundsToStones

## <a name="api"></a>API

- [Unit Converters のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Types/UnitConverters.shared.cs)
- [Unit Converters の API ドキュメント](xref:Xamarin.Essentials.UnitConverters)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Unit-Conversion-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
