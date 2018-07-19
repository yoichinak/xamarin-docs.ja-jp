---
title: 'Xamarin.Essentials: デバイス情報'
description: このドキュメントでアプリケーションが実行されているデバイスに関する情報を提供する、Xamarin.Essentials で DeviceInfo クラスについて説明します。
ms.assetid: A1AC5373-926A-4FB6-8D7D-4B87EB8EB522
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: b7246afca19607ef2f70288d4643696f4ac35d52
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831488"
---
# <a name="xamarinessentials-device-information"></a>Xamarin.Essentials: デバイス情報

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

**DeviceInfo**クラスで、アプリケーションが実行されているデバイスに関する情報を提供します。

## <a name="using-deviceinfo"></a>DeviceInfo を使用します。

クラスで Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

次の情報は、API を介して公開されます。

```csharp
// Device Model (SMG-950U)
var device = DeviceInfo.Model;

// Manufacturer (Samsung)
var manufacturer = DeviceInfo.Manufacturer;

// Device Name (Motz's iPhone)
var deviceName = DeviceInfo.Name;

// Operating System Version Number (7.0)
var version = DeviceInfo.VersionString;

// Platform (Android)
var platform = DeviceInfo.Platform;

// Idiom (Phone)
var idiom = DeviceInfo.Idiom;

// Device Type (Physical)
var deviceType = DeviceInfo.DeviceType;
```

## <a name="platformsxrefxamarinessentialsdeviceinfoplatforms"></a>[プラットフォーム](xref:Xamarin.Essentials.DeviceInfo.Platforms)

`DeviceInfo.Platform` オペレーティング システムにマップされる定数文字列を相互に関連付けます。 値を確認できます、`Platforms`クラス。

- **DeviceInfo.Platforms.iOS** – iOS
- **DeviceInfo.Platforms.Android** – Android
- **DeviceInfo.Platforms.UWP** – UWP
- **DeviceInfo.Platforms.Unsupported** – サポートされていません。

## <a name="idiomsxrefxamarinessentialsdeviceinfoidioms"></a>[表現方法](xref:Xamarin.Essentials.DeviceInfo.Idioms)

`DeviceInfo.Idiom` correlates アプリケーションをデバイスの種類にマップされる文字列定数がで実行されています。 値を確認できます、`Idioms`クラス。

- **DeviceInfo.Idioms.Phone** – 電話
- **DeviceInfo.Idioms.Tablet** – タブレット
- **DeviceInfo.Idioms.Desktop** – デスクトップ
- **DeviceInfo.Idioms.TV** – テレビ
- **DeviceInfo.Idioms.Unsupported** – サポートされていません。

## <a name="device-type"></a>デバイスの種類

`DeviceInfo.DeviceType` かどうか、アプリケーションは実行する物理または仮想デバイスを決定する列挙体を関連付けます。 仮想デバイスは、シミュレーターやエミュレーターです。

## <a name="api"></a>API

- [DeviceInfo ソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceInfo)
- [DeviceInfo API ドキュメント](xref:Xamarin.Essentials.DeviceInfo)
