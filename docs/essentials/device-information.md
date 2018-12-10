---
title: 'Xamarin.Essentials: デバイス情報'
description: このドキュメントでは、アプリケーションが実行されているデバイスに関する情報を提供する Xamarin.Essentials の DeviceInfo クラスについて説明します。
ms.assetid: A1AC5373-926A-4FB6-8D7D-4B87EB8EB522
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: b78c04d30871552f9b1e18a42c871e24464c4802
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2018
ms.locfileid: "52898954"
---
# <a name="xamarinessentials-device-information"></a>Xamarin.Essentials: デバイス情報

**DeviceInfo** クラスでは、アプリケーションが実行されているデバイスに関する情報が提供されます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-deviceinfo"></a>DeviceInfo の使用

自分のクラスの Xamarin.Essentials に参照を追加します。

```csharp
using Xamarin.Essentials;
```

API では次の情報が公開されます。

```csharp
// Device Model (SMG-950U, iPhone10,6)
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

`DeviceInfo.Platform` は、オペレーティング システムにマップされる定数文字列に対応します。 値は次の `DevicePlatform` 構造体で確認できます。

- **DevicePlatform.iOS** – iOS
- **DevicePlatform.Android** – Android
- **DevicePlatform.UWP** – UWP
- **DevicePlatform.Unknown** – 不明

## <a name="idiomsxrefxamarinessentialsdeviceinfoidioms"></a>[表示形式](xref:Xamarin.Essentials.DeviceInfo.Idioms)

`DeviceInfo.Idiom` は、アプリケーションが実行されているデバイスの種類にマップされる文字列定数に対応します。 値は次の `DeviceIdiom` 構造体で確認できます。

- **DeviceIdiom.Phone** – 電話
- **DeviceIdiom.Tablet** – タブレット
- **DeviceIdiom.Desktop** – デスクトップ
- **DeviceIdiom.TV** – テレビ
- **DeviceIdiom.Watch** – 腕時計
- **DeviceIdiom.Unknown** – 不明

## <a name="device-type"></a>デバイスの種類

`DeviceInfo.DeviceType` は、アプリケーションが物理デバイスまたは仮想デバイスのどちらで実行されているかを示す列挙型に対応します。 仮想デバイスは、シミュレーターやエミュレーターです。

## <a name="platform-implementation-specifics"></a>プラットフォームの実装の詳細

# <a name="iostabios"></a>[iOS](#tab/ios)

iOS では、特定の iOS デバイスの名前を取得するための API は開発者に対して公開されていません。 代わりに、iPhone X を示す _iPhone10,6_ のようなハードウェア識別子が返されます。これらの識別子のマッピングは Apple では提供されていませんが、[iPhone Wiki](https://www.theiphonewiki.com/wiki/Models) (非公式ソース) で確認できます。

--------------

## <a name="api"></a>API

- [DeviceInfo のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceInfo)
- [DeviceInfo API のドキュメント](xref:Xamarin.Essentials.DeviceInfo)
