---
title: 'Xamarin.Essentials: デバイス情報'
description: このドキュメントでアプリケーションが実行されているデバイスに関する情報を提供する、Xamarin.Essentials で DeviceInfo クラスについて説明します。
ms.assetid: A1AC5373-926A-4FB6-8D7D-4B87EB8EB522
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 18fe081372cc190e5ead2045f36d63652f8702c3
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353803"
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

`DeviceInfo.DeviceType` アプリケーションが、物理または仮想デバイスで実行されているかどうかを決定する列挙体を関連付けます。 仮想デバイスは、シミュレーターやエミュレーターです。

## <a name="platform-implementation-specifics"></a>プラットフォームの実装の詳細

# <a name="iostabios"></a>[iOS](#tab/ios)

iOS では、開発者が特定の iOS デバイスの名前を取得するための API を公開しません。 などのハードウェア識別子が返される代わりに_iPhone10、6_ iPhone X を指します。これらの識別子のマッピングが Apple によって指定されていないがご覧[iPhone Wiki](https://www.theiphonewiki.com/wiki/Models) (非公式ソース ソース)。

--------------

## <a name="api"></a>API

- [DeviceInfo ソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceInfo)
- [DeviceInfo API ドキュメント](xref:Xamarin.Essentials.DeviceInfo)
