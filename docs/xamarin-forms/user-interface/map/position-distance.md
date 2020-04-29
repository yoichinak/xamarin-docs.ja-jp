---
title: Xamarin. フォームマップの位置と距離
description: Xamarin. map 名前空間には、マップとそのピンを配置するときに通常使用される位置構造体と、マップを配置するときに必要に応じて使用できる距離構造体が含まれます。
ms.prod: xamarin
ms.assetid: 2F4EA3D2-1351-40AD-A71D-CF7F1F18F1E8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2020
ms.openlocfilehash: 567e1b5620378f0c1b64d17c56c8fb451f18abc3
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517447"
---
# <a name="xamarinforms-map-position-and-distance"></a>Xamarin. フォームマップの位置と距離

[![](~/media/shared/download.png)サンプルをダウンロードするサンプルをダウンロードする](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

名前[`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps)空間には[`Position`](xref:Xamarin.Forms.Maps.Position) 、マップとそのピンを配置するときに通常使用される構造[`Distance`](xref:Xamarin.Forms.Maps.Distance)体と、マップを配置するときに必要に応じて使用できる構造体が含まれています。

## <a name="position"></a>[位置]

構造[`Position`](xref:Xamarin.Forms.Maps.Position)体は、緯度と経度の値として格納されている位置をカプセル化します。 この構造体は、次の2つの読み取り専用プロパティを定義します。

- [`Latitude`](xref:Xamarin.Forms.Maps.Position.Latitude)型`double`の。これは、小数点以下の位置の緯度を表します。
- [`Longitude`](xref:Xamarin.Forms.Maps.Position.Longitude)型`double`の。これは、小数点以下の位置の経度を表します。

[`Position`](xref:Xamarin.Forms.Maps.Position)オブジェクトは`Position`コンストラクターを使用して作成されます。このコンストラクターに`double`は、値として指定された緯度と経度の引数が必要です。

```csharp
Position position = new Position(36.9628066, -122.0194722);
```

オブジェクトを`Position`作成するとき、緯度の値は-90.0 と90.0 の間でクランプされ、経度の値は-180.0 と180.0 の間で固定されます。

> [!NOTE]
> クラス`GeographyUtils`には、 `ToRadians`値を`double`度数からラジアンに変換する拡張メソッドと、 `ToDegrees` `double`値をラジアンから度に変換する拡張メソッドがあります。

## <a name="distance"></a>Distance

構造[`Distance`](xref:Xamarin.Forms.Maps.Distance)体は、 `double`値として格納されている距離をカプセル化します。これは、メートル単位の距離を表します。 この構造体は、次の3つの読み取り専用プロパティを定義します。

- [`Kilometers`](xref:Xamarin.Forms.Maps.Distance.Kilometers)によって`double`スパンされる距離 (キロメートル単位) を表す、 `Distance`型の。
- [`Meters`](xref:Xamarin.Forms.Maps.Distance.Meters)によって`double`スパンされるメートル単位の距離を表す、型の`Distance`。
- [`Miles`](xref:Xamarin.Forms.Maps.Distance.Miles)によって`double`分散されている距離 (マイル単位) を表す`Distance`、型の。

[`Distance`](xref:Xamarin.Forms.Maps.Distance)オブジェクトは、次の`Distance` `double`ように指定されたメーター引数を必要とするコンストラクターを使用して作成できます。

```csharp
Distance distance = new Distance(1450.5);
```

また、 [`Distance`](xref:Xamarin.Forms.Maps.Distance) 、、 [`FromMiles`](xref:Xamarin.Forms.Maps.Distance.FromMiles*)、および`BetweenPositions`の各[`FromKilometers`](xref:Xamarin.Forms.Maps.Distance.FromKilometers*)ファクトリメソッドを使用して[`FromMeters`](xref:Xamarin.Forms.Maps.Distance.FromMeters*)オブジェクトを作成することもできます。

```csharp
Distance distance1 = Distance.FromKilometers(1.45); // argument represents the number of kilometers
Distance distance2 = Distance.FromMeters(1450.5);   // argument represents the number of meters
Distance distance3 = Distance.FromMiles(0.969);     // argument represents the number of miles
Distance distance4 = Distance.BetweenPositions(position1, position2);
```

## <a name="related-links"></a>関連リンク

- [Maps サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
