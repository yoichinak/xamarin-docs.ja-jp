---
title: Xamarin.Essentials デバイス情報
description: DeviceInfo クラス実行しているデバイス、アプリケーションに関する情報を提供します。
ms.assetid: A1AC5373-926A-4FB6-8D7D-4B87EB8EB522
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3a9fc352336f67375b732247560f8c1d4dd45f70
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-device-information"></a>Xamarin.Essentials デバイス情報

![プレリリース NuGet](~/media/shared/pre-release.png)

**DeviceInfo**クラスで、アプリケーションが実行されているデバイスに関する情報を提供します。

## <a name="using-deviceinfo"></a>DeviceInfo を使用します。

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

次の情報は、API を介して公開されています。

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

`DeviceInfo.Platform` オペレーティング システムにマップされる文字列定数を相互に関連付けます。 値をチェックすることができます、`Platforms`クラス。

- **DeviceInfo.Platforms.iOS** – iOS
- **DeviceInfo.Platforms.Android** – Android
- **DeviceInfo.Platforms.UWP** – UWP
- **DeviceInfo.Platforms.Unsupported** – サポートされていません。

## <a name="idiomsxrefxamarinessentialsdeviceinfoidioms"></a>[表現方法](xref:Xamarin.Essentials.DeviceInfo.Idioms)

`DeviceInfo.Idiom` 関係するという、アプリケーションをデバイスの種類にマップされる文字列定数がで実行されています。 値をチェックすることができます、`Idioms`クラス。

- **DeviceInfo.Idioms.Phone** – 電話
- **DeviceInfo.Idioms.Tablet** – タブレット
- **DeviceInfo.Idioms.Desktop** – デスクトップ
- **DeviceInfo.Idioms.TV** – テレビ
- **DeviceInfo.Idioms.Unsupported** – サポートされていません。

## <a name="device-type"></a>デバイスの種類

`DeviceInfo.DeviceType` かどうか、アプリケーションは実行している、物理または仮想デバイスを決定する列挙体を関連付けます。 仮想デバイスは、シミュレーターまたはエミュレーターです。

## <a name="api"></a>API

- [DeviceInfo ソース コード](https://github.com/xamarin/Essentials/tree/master/Essentials/DeviceInfo)
- [DeviceInfo API ドキュメント](xref:Xamarin.Essentials.DeviceInfo)
