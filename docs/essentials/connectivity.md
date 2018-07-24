---
title: 'Xamarin.Essentials: 接続'
description: Xamarin.Essentials で接続クラスでは、デバイスのネットワークの状態の変更を監視、現在のネットワーク アクセスと現在の接続方法を確認できます。
ms.assetid: E1B1F152-B1D5-4227-965E-C0AEBF528F49
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 54c165e15e725caaecb1573b74cfe295170db141
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38848610"
---
# <a name="xamarinessentials-connectivity"></a>Xamarin.Essentials: 接続

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

**接続**クラスでは、デバイスのネットワークの状態の変化を監視できますが、現在のネットワーク アクセスと現在の接続方法を確認します。

## <a name="getting-started"></a>作業の開始

アクセスする、**接続**次のプラットフォーム固有設定の機能が必要です。

# <a name="androidtabandroid"></a>[Android](#tab/android)

`AccessNetworkState`アクセス許可は必須であり、Android プロジェクトで構成する必要があります。 これは、次の方法で追加できます。

開く、 **AssemblyInfo.cs**ファイル、**プロパティ**フォルダーを追加。

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessNetworkState)]
```

または、Android マニフェストを更新します。

開く、 **AndroidManifest.xml**ファイル、**プロパティ**フォルダー内の次の追加と、**マニフェスト**ノード。

```xml
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

または、Anroid プロジェクトを右クリックし、プロジェクトのプロパティを開きます。 **Android マニフェスト**検索、**ために必要なアクセス許可:** 領域とチェック、**ネットワークのアクセス状態**権限。 これは自動的に更新、 **AndroidManifest.xml**ファイル。

# <a name="iostabios"></a>[iOS](#tab/ios)

追加の設定が必要です。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

追加の設定が必要です。

-----

## <a name="using-connectivity"></a>接続の使用

クラスで Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

現在のネットワーク アクセスを確認します。

```csharp
var current = Connectivity.NetworkAccess;

if (current == NetworkAccess.Internet)
{
    // Connection to internet is available
}
```

[ネットワーク アクセス](xref:Xamarin.Essentials.NetworkAccess)は、次のカテゴリに分類されます。

* **インターネット**– ローカルとインターネットのアクセス。
* **ConstrainedInternet** – インターネットへのアクセスを制限します。 Captive portal の接続、web ポータルへのローカル アクセスが提供されているが、インターネットへのアクセスは、特定の資格情報は、ポータル経由で提供されることが必要ですことを示します。
* **ローカル**– ローカル ネットワークのみにアクセスします。
* **None** – 使用できる接続がありません。
* **不明な**: インターネット接続を確認できません。

種類をチェックする[接続プロファイル](xref:Xamarin.Essentials.ConnectionProfile)デバイスが実際に使用します。

```csharp
var profiles = Connectivity.Profiles;
if (profiles.Contains(ConnectionProfile.WiFi))
{
    // Active Wi-Fi connection.
}
```

接続プロファイルまたはネットワーク アクセスの変更されるたびにトリガーされたときにイベントを受信できます。

```csharp
public class ConnectivityTest
{
    public ConnectivityTest()
    {
        // Register for connectivity changes, be sure to unsubscribe when finished
        Connectivity.ConnectivityChanged += Connectivity_ConnectivityChanged;
    }

    void Connectivity_ConnectivityChanged(ConnectivityChangedEventArgs  e)
    {
        var access = e.NetworkAccess;
        var profiles = e.Profiles;
    }
}
```

## <a name="limitations"></a>制限事項

考えられる点に注意することが重要する`Internet`によって報告される`NetworkAccess`が web へのフル アクセスでは使用できません。 接続が各プラットフォームでどのように動作するしくみにより、接続を使用できるを保証ことできますだけです。 たとえば、デバイスは、Wi-fi ネットワークに接続する可能性がありますが、ルーターが、インターネットから切断されています。 このインスタンスで、インターネットが報告されるが、アクティブな接続は使用できません。

## <a name="api"></a>API

* [接続のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Connectivity)
* [接続 API ドキュメント](xref:Xamarin.Essentials.Connectivity)
