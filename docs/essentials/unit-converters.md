---
title: Xamarin.Essentials Unit Converters
description: Xamarin.Essentials の UnitConverters クラスは単位変換機能をいくつか備えており、Xamarin.Essentials の使用時、開発者を支援します。
ms.assetid: 35DE2704-E730-4337-9476-66CD53376943
author: jamesmontemagno
ms.author: jamont
ms.date: 01/06/2020
ms.openlocfilehash: c07e0c7d9645c22f0d70c75fd7d8dffdec8cde04
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "77545037"
---
# <a name="xamarinessentials-unit-converters"></a>Xamarin.Essentials:Unit Converters

**UnitConverters** クラスは単位変換機能をいくつか備えており、Xamarin.Essentials の使用時、開発者を支援します。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-unit-converters"></a>Unit Converters の使用

自分のクラスに Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

単位変換機能はすべて、Xamarin.Essentials の静的 `UnitConverters` クラスの使用時に利用できます。 たとえば、華氏を摂氏に簡単に変換できます。

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
