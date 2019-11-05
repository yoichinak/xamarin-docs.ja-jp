---
title: Xamarin.Forms マップの位置と距離
description: Xamarin.Forms.Maps 名前空間には、マップとそのピンを配置するときに通常使用される位置構造体と、マップを配置するときに必要に応じて使用できる距離構造体が含まれます。
ms.prod: xamarin
ms.assetid: 2F4EA3D2-1351-40AD-A71D-CF7F1F18F1E8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/30/2019
ms.openlocfilehash: a68eab7bb3e6da764f5a35af4461d6af2be50ebe
ms.sourcegitcommit: 3ea19e3a51515b30349d03c70a5b3acd7eca7fe7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/01/2019
ms.locfileid: "73426283"
---
# <a name="xamarinforms-map-position-and-distance"></a>Xamarin.Forms マップの位置と距離

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

[`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps)名前空間には、マップとそのピンを配置するときに通常使用される[`Position`](xref:Xamarin.Forms.Maps.Position)構造体と、マップを配置するときに必要に応じて使用できる[`Distance`](xref:Xamarin.Forms.Maps.Distance)構造体が含まれています。

## <a name="position"></a>位置

[`Position`](xref:Xamarin.Forms.Maps.Position)構造体は、緯度と経度の値として格納されている位置をカプセル化します。 この構造体は、次の2つの読み取り専用プロパティを定義します。

- [`Latitude`](xref:Xamarin.Forms.Maps.Position.Latitude)は、`double`型で、小数点以下の位置の緯度を表します。
- [`Longitude`](xref:Xamarin.Forms.Maps.Position.Longitude)は、`double`型で、小数点以下の位置の経度を表します。

[`Position`](xref:Xamarin.Forms.Maps.Position)オブジェクトは、`Position` コンストラクターを使用して作成されます。これには `double` 値として指定された緯度と経度の引数が必要です。

```csharp
Position position = new Position(36.9628066, -122.0194722);
```

> [!NOTE]
> `Position` オブジェクトを作成すると、緯度の値は-90.0 と90.0 の間でクランプされ、経度の値は-180.0 と180.0 の間でクランプされます。

## <a name="distance"></a>単位

[`Distance`](xref:Xamarin.Forms.Maps.Distance)構造体は、`double` 値として格納されている距離をカプセル化します。これは、メートル単位の距離を表します。 この構造体は、次の3つの読み取り専用プロパティを定義します。

- [`Kilometers`](xref:Xamarin.Forms.Maps.Distance.Kilometers)は、`double`型で、`Distance`によってスパンされる距離 (キロメートル単位) を表します。
- [`Meters`](xref:Xamarin.Forms.Maps.Distance.Meters)は、`double`型で、`Distance`によってスパンされるメートル単位の距離を表します。
- [`Miles`](xref:Xamarin.Forms.Maps.Distance.Miles)は、`double`型で、`Distance`によってスパンされる距離 (マイル単位) を表します。

[`Distance`](xref:Xamarin.Forms.Maps.Distance)オブジェクトは、`Distance` コンストラクターを使用して作成できます。これには、`double`として指定されたメーター引数が必要です。

```csharp
Distance distance = new Distance(1450.5);
```

また、 [`FromKilometers`](xref:Xamarin.Forms.Maps.Distance.FromKilometers*)、 [`FromMeters`](xref:Xamarin.Forms.Maps.Distance.FromMeters*)、および[`FromMiles`](xref:Xamarin.Forms.Maps.Distance.FromMiles*)ファクトリメソッドを使用して[`Distance`](xref:Xamarin.Forms.Maps.Distance)オブジェクトを作成することもできます。

```csharp
Distance distance1 = Distance.FromKilometers(1.45); // argument represents the number of kilometers
Distance distance2 = Distance.FromMeters(1450.5);   // argument represents the number of meters
Distance distance3 = Distance.FromMiles(0.969);     // argument represents the number of miles
```

## <a name="related-links"></a>関連リンク

- [Maps サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
