---
title: Xamarin.Essentials 接続
description: 接続クラスには、デバイスのネットワークの状態での変更の監視には、現在のネットワーク アクセス、および現在の接続方法を確認することができます。
ms.assetid: E1B1F152-B1D5-4227-965E-C0AEBF528F49
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: fd757bec32d2854d2c9693812dece05ef11e2f80
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-connectivity"></a>Xamarin.Essentials 接続

![プレリリース NuGet](~/media/shared/pre-release.png)

**接続**クラスにより、デバイスのネットワークの状態の変更を監視する現在のネットワーク アクセス、および現在の接続方法を確認します。

## <a name="getting-started"></a>作業の開始

アクセスする、**接続**次のプラットフォーム固有のセットアップの機能が必要です。

# <a name="androidtabandroid"></a>[Android](#tab/android)

`AccessNetworkState`権限が必要と Android プロジェクトで構成する必要があります。 これは、次の方法で追加できます。

開く、 **AssemblyInfo.cs**下にあるファイル、**プロパティ**フォルダーを追加。

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessNetworkState)]
```

または、Android のマニフェストを更新します。

開く、 **AndroidManifest.xml**下にあるファイル、**プロパティ**フォルダー内の次の追加と、**マニフェスト**ノード。

```xml
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

または、Anroid プロジェクトを右クリックし、プロジェクトのプロパティを開きます。 **Android マニフェスト**検索、**権限が必要:** 領域とチェック、**ネットワークのアクセス状態**権限です。 これは自動的に更新、 **AndroidManifest.xml**ファイル。

# <a name="iostabios"></a>[iOS](#tab/ios)

追加の設定が必要です。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

追加の設定が必要です。

-----

## <a name="using-connectivity"></a>接続の使用

クラスの Xamarin.Essentials への参照を追加します。

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

[ネットワーク アクセス](xref:Xamarin.Essentials.NetworkAccess)次のカテゴリに分類されます。

* **インターネット**– ローカルとインターネットのアクセス。
* **ConstrainedInternet** – インターネットへのアクセスを制限します。 Web ポータルへのローカル アクセスを提供するは、インターネットへのアクセスは、特定の資格情報がポータルを介して提供されることが必要ですが専用のポータル接続を示します。
* **ローカル**– ローカル ネットワークのみにアクセスします。
* **None** – 使用できる接続がありません。
* **不明な**– をインターネット接続を確認できません。

種類を確認する[接続プロファイル](xref:Xamarin.Essentials.ConnectionProfile)デバイスがアクティブに使用します。

```csharp
var profiles = Connectivity.Profiles;
if (profiles.Contains(ConnectionProfile.WiFi))
{
    // Active Wi-Fi connection.
}
```

接続プロファイルまたはネットワーク アクセスの変更されるたびにトリガーされたときにイベントが表示されることができます。

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

考えられる点に注意することが重要です`Internet`によって報告された`NetworkAccess`が web へのフル アクセスでは使用できません。 各プラットフォームでの接続の動作のために、接続を使用できる、のみ保証できます。 インスタンス、デバイスは、Wi-fi ネットワークに接続することがありますが、ルーターが、インターネットから切断されています。 このインスタンスでは、インターネットを報告する可能性がありますが、アクティブな接続は使用できません。

## <a name="api"></a>API

* [接続のソース コード](https://github.com/xamarin/Essentials/tree/master/Essentials/Connectivity)
* [接続 API ドキュメント](xref:Xamarin.Essentials.Connectivity)
