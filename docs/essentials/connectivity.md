---
title: Xamarin.Essentials:接続
description: Xamarin.Essentials の Connectivity クラスを使用すると、デバイスのネットワーク状態の変更を監視したり、現在のネットワーク アクセスとその現在の接続方法を確認したりできます。
ms.assetid: E1B1F152-B1D5-4227-965E-C0AEBF528F49
author: jamesmontemagno
ms.author: jamont
ms.date: 01/08/2019
ms.custom: video
ms.openlocfilehash: c70510f7b47f93c6119532b6a1c06f6c2e9e56ea
ms.sourcegitcommit: afe9d93373d66eb45d82cabefca83b5733969634
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2019
ms.locfileid: "67855759"
---
# <a name="xamarinessentials-connectivity"></a>Xamarin.Essentials:接続

**Connectivity** クラスを使用すると、デバイスのネットワーク状態の変更を監視したり、現在のネットワーク アクセスとその現在の接続方法を確認したりできます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

**接続**の機能にアクセスするには、次のプラットフォーム固有の設定が必要です。

# <a name="androidtabandroid"></a>[Android](#tab/android)

`AccessNetworkState` アクセス許可が必要です。Android プロジェクト内で構成する必要があります。 これは次の方法で追加できます。

**[プロパティ]** フォルダーにある **AssemblyInfo.cs** ファイルを開き、以下を追加します。

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessNetworkState)]
```

または、Android マニフェストを追加します。

**[プロパティ]** フォルダーにある **AndroidManifest.xml** ファイルを開き、**manifest** ノードの内部に以下を追加します。

```xml
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

または、Android プロジェクトを右クリックし、プロジェクトのプロパティを開きます。 **[Android マニフェスト]** の下で **[必要なアクセス許可:]** 領域を探し、 **[Access Network State]\(ネットワーク アクセスの状態\)** アクセス許可を確認します。 これにより、**AndroidManifest.xml** ファイルが自動的に更新されます。

# <a name="iostabios"></a>[iOS](#tab/ios)

追加の設定は必要ありません。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

追加の設定は必要ありません。

-----

## <a name="using-connectivity"></a>接続の使用

自分のクラスに Xamarin.Essentials への参照を追加します。

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

[ネットワーク アクセス](xref:Xamarin.Essentials.NetworkAccess)は次のカテゴリに分けられます。

* **Internet** – ローカルとインターネットのアクセス。
* **ConstrainedInternet** – 制限付きインターネット アクセスです。 Web ポータルへのローカル接続が提供されている、キャプティブ ポータルの接続を示しています。ただし、インターネットにアクセスするにはポータル経由で特定の資格情報を提供する必要があります。
* **local** – ローカル ネットワークのみにアクセスします。
* **None** – 使用できる接続がありません。
* **Unknown** – インターネット接続を確認できません。

デバイスが実際に使用している[接続プロファイル](xref:Xamarin.Essentials.ConnectionProfile)の種類を確認できます。

```csharp
var profiles = Connectivity.ConnectionProfiles;
if (profiles.Contains(ConnectionProfile.WiFi))
{
    // Active Wi-Fi connection.
}
```

接続プロファイルまたはネットワーク アクセスが変更されるたびに、トリガーされたイベントを受信できます。

```csharp
public class ConnectivityTest
{
    public ConnectivityTest()
    {
        // Register for connectivity changes, be sure to unsubscribe when finished
        Connectivity.ConnectivityChanged += Connectivity_ConnectivityChanged;
    }

    void Connectivity_ConnectivityChanged(object sender, ConnectivityChangedEventArgs e)
    {
        var access = e.NetworkAccess;
        var profiles = e.ConnectionProfiles;
    }
}
```

## <a name="limitations"></a>制限事項

`NetworkAccess` によって `Internet` が報告されるが、Web へのフル アクセスは利用できないという場合があることに注意する必要があります。 各プラットフォーム上での接続の動作方法によってのみ、接続が使用可能であることを保証できます。 たとえば、デバイスは Wi-Fi ネットワークに接続されているが、ルーターがインターネットから切断されている可能性があります。 この例ではインターネットが報告される場合がありますが、実際には接続を使用できません。

## <a name="api"></a>API

* [Connectivity のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Connectivity)
* [Connectivity API ドキュメント](xref:Xamarin.Essentials.Connectivity)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Connectivity-Essential-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
