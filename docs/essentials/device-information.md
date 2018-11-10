---
title: 'Xamarin.Essentials: デバイス情報'
description: このドキュメントでは、アプリケーションが実行されているデバイスに関する情報を提供する Xamarin.Essentials の DeviceInfo クラスについて説明します。
ms.assetid: A1AC5373-926A-4FB6-8D7D-4B87EB8EB522
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 530b04446703d78452357b2c9f9089e59ebf6e6c
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2018
ms.locfileid: "50674814"
---
# <a name="xamarinessentials-device-information"></a>Xamarin.Essentials: デバイス情報

![プレリリースの NuGet](~/media/shared/pre-release.png)

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

`DeviceInfo.Platform` は、オペレーティング システムにマップされる定数文字列に対応します。 値は `Platforms` クラスで確認できます。

- **DeviceInfo.Platforms.iOS** – iOS
- **DeviceInfo.Platforms.Android** – Android
- **DeviceInfo.Platforms.UWP** – UWP
- **DeviceInfo.Platforms.Unsupported** – 非サポート

## <a name="idiomsxrefxamarinessentialsdeviceinfoidioms"></a>[表示形式](xref:Xamarin.Essentials.DeviceInfo.Idioms)

`DeviceInfo.Idiom` は、アプリケーションが実行されているデバイスの種類にマップされる文字列定数に対応します。 値は `Idioms` クラスで確認できます。

- **DeviceInfo.Idioms.Phone** – 電話
- **DeviceInfo.Idioms.Tablet** – タブレット
- **DeviceInfo.Idioms.Desktop** – デスクトップ
- **DeviceInfo.Idioms.TV** – テレビ
- **DeviceInfo.Idioms.Unsupported** – 非サポート

## <a name="device-type"></a>デバイスの種類

`DeviceInfo.DeviceType` は、アプリケーションが物理デバイスまたは仮想デバイスのどちらで実行されているかを示す列挙型に対応します。 仮想デバイスは、シミュレーターやエミュレーターです。

## <a name="platform-implementation-specifics"></a>プラットフォームの実装の詳細

# <a name="iostabios"></a>[iOS](#tab/ios)

iOS では、特定の iOS デバイスの名前を取得するための API は開発者に対して公開されていません。 代わりに、iPhone X を示す _iPhone10,6_ のようなハードウェア識別子が返されます。これらの識別子のマッピングは Apple では提供されていませんが、[iPhone Wiki](https://www.theiphonewiki.com/wiki/Models) (非公式ソース) で確認できます。

--------------

## <a name="api"></a>API

- [DeviceInfo のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceInfo)
- [DeviceInfo API のドキュメント](xref:Xamarin.Essentials.DeviceInfo)
