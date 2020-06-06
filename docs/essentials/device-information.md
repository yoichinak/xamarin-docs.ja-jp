---
タイトル: "Xamarin.Essentials: デバイス情報" の説明: "このドキュメントでは、アプリケーションが実行されているデバイスに関する情報を提供する Xamarin.Essentials の DeviceInfo クラスについて説明します。"
ms.assetid:A1AC5373-926A-4FB6-8D7D-4B87EB8EB522 author: jamesmontemagno ms.custom: video ms.author: jamont ms.date:11/04/2018 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinessentials-device-information"></a>Xamarin.Essentials:デバイス情報

**DeviceInfo** クラスでは、アプリケーションが実行されているデバイスに関する情報が提供されます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-deviceinfo"></a>DeviceInfo の使用

クラスの Xamarin.Essentials への参照を追加します。

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

## <a name="platforms"></a>プラットフォーム

[`DeviceInfo.Platform`](xref:Xamarin.Essentials.DeviceInfo.Platform) は、オペレーティング システムにマップされる定数文字列に関連付けられます。 値は次の `DevicePlatform` 構造体で確認できます。

- **DevicePlatform.iOS** – iOS
- **DevicePlatform.Android** – Android
- **DevicePlatform.UWP** – UWP
- **DevicePlatform.Unknown** – 不明

## <a name="idioms"></a>表示形式

[`DeviceInfo.Idiom`](xref:Xamarin.Essentials.DeviceInfo.Idiom) は、アプリケーションが実行されるデバイスの種類にマップされる文字列定数に関連付けられます。 値は次の `DeviceIdiom` 構造体で確認できます。

- **DeviceIdiom.Phone** – 電話
- **DeviceIdiom.Tablet** – タブレット
- **DeviceIdiom.Desktop** – デスクトップ
- **DeviceIdiom.TV** – テレビ
- **DeviceIdiom.Watch** – 腕時計
- **DeviceIdiom.Unknown** – 不明

## <a name="device-type"></a>デバイスの種類

`DeviceInfo.DeviceType` は、アプリケーションが物理デバイスまたは仮想デバイスのどちらで実行されているかを示す列挙型に対応します。 仮想デバイスは、シミュレーターやエミュレーターです。

## <a name="platform-implementation-specifics"></a>プラットフォームの実装の詳細

# <a name="ios"></a>[iOS](#tab/ios)

iOS では、特定の iOS デバイスのモデルを取得するための API は開発者に対して公開されていません。 代わりに、iPhone X を示す _iPhone10,6_ のようなハードウェア識別子が返されます。これらの識別子のマッピングは Apple では提供されていませんが、これらの (非公式ソースの) [iPhone Wiki](https://www.theiphonewiki.com/wiki/Models) および [iOS モデルの取得](https://github.com/dannycabrera/Get-iOS-Model)で確認できます。

--------------

## <a name="api"></a>API

- [DeviceInfo のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceInfo)
- [DeviceInfo API のドキュメント](xref:Xamarin.Essentials.DeviceInfo)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Device-Information-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
