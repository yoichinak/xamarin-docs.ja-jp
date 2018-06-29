---
title: 'Xamarin.Essentials: セキュリティで保護されたストレージ'
description: このドキュメントでは、単純なキー/値ペアを安全に保管にも役立ちます Xamarin.Essentials で SecureStorage クラスについて説明します。 これには、クラス、プラットフォームの実装の詳細、および制限事項を使用する方法について説明します。
ms.assetid: 78856C0D-76BB-406E-A880-D5A3987B7D64
author: redth
ms.author: jodick
ms.date: 05/04/2018
ms.openlocfilehash: df2aa1fd23976e8db34d7c466317a8630408af7a
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080353"
---
# <a name="xamarinessentials-secure-storage"></a>Xamarin.Essentials: セキュリティで保護されたストレージ

![プレリリース NuGet](~/media/shared/pre-release.png)

**SecureStorage**クラスは、単純なキー/値ペアを安全に保管します。

## <a name="getting-started"></a>作業の開始

アクセスする、 **SecureStorage**機能、次のプラットフォーム固有のセットアップが必要です。

# <a name="androidtabandroid"></a>[Android](#tab/android)

追加の設定が必要です。

# <a name="iostabios"></a>[iOS](#tab/ios)

IOS シミュレーターで開発する場合に有効にするには**キーチェーン**権利キーチェーン アクセス グループを追加するアプリケーションのバンドル id とします。

開く、 **Entitlements.plist** iOS プロジェクトと検索、**キーチェーン**権利し有効にします。 グループとして、アプリケーションの識別子が自動的に追加されます。

プロジェクトのプロパティで  **iOS バンドル署名 *** 設定、**カスタム権利**に**Entitlements.plist**です。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

追加の設定が必要です。

-----

## <a name="using-secure-storage"></a>セキュリティで保護されたストレージを使用します。

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

値を保存する、指定された_キー_セキュリティで保護された記憶域に。

```csharp
await SecureStorage.SetAsync("oauth_token", "secret-oauth-token-value");
```

セキュリティで保護されたストレージから値を取得します。

```csharp
var oauthToken = await SecureStorage.GetAsync("oauth_token");
```

特定のキーを削除するには、次のように呼び出します。

```csharp
SecureStorage.Remove("oauth_token");
```

すべてのキーを削除するには、次のように呼び出します。

```csharp
SecureStorage.RemoveAll();
```


## <a name="platform-implementation-specifics"></a>プラットフォームの実装の詳細

# <a name="androidtabandroid"></a>[Android](#tab/android)

[Android キーストア](https://developer.android.com/training/articles/keystore.html)に保存されるまで、値の暗号化に使用される暗号キーを格納するため、[共有設定](https://developer.android.com/training/data-storage/shared-preferences.html)のファイル名を持つ **[、-アプリ-パッケージの ID] .xamarinessentials**.  共有の設定ファイルで使用されるキーは、 _MD5 ハッシュ_に渡されたキーの`SecureStorage`の API です。

## <a name="api-level-23-and-higher"></a>API レベル 23 以上

新しい API レベルで、 **AES**キーは、Android キーストアから取得したし、は併用、 **AES/GCM/NoPadding**共有の設定ファイルに保存する前に、値を暗号化する暗号。

## <a name="api-level-22-and-lower"></a>API レベル 22 と下限

以前の API レベルで Android キーストアのみがサポートを格納する**RSA**と共に使用されるキー、 **RSA/ECB/PKCS1Padding**を暗号化する暗号、 **AES** (ランダムにキー実行時に生成されます)、キーの下で、共有の設定ファイルに格納されていると_SecureStorageKey_いずれかが既に生成されていない場合は、します。

すべての暗号化された値は、デバイスからアプリのアンインストール時に削除されます。

# <a name="iostabios"></a>[iOS](#tab/ios)

[キーチェーン](https://developer.xamarin.com/api/type/Security.SecKeyChain/)iOS デバイスで安全に値を格納するために使用します。  `SecRecord`値を格納するために使用が、`Service`値に設定 **[、-アプリ-バンドル-ID] .xamarinessentials**です。

場合によってはキーチェーン データは iCloud と同期され、アプリケーションをアンインストールする可能性がありますいない値を削除、セキュリティで保護された iCloud とユーザーの他のデバイスからです。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[DataProtectionProvider](https://docs.microsoft.com/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider) UWP デバイスで安全に暗号化されている値を使用します。

暗号化された値が格納されている`ApplicationData.Current.LocalSettings`、という名前でコンテナーの内部 **[、アプリ ID] .xamarinessentials**です。

アプリケーションのアンインストールにより、 _LocalSettings_、および暗号化されたすべての値も削除します。

-----

## <a name="limitations"></a>制限事項

この API は、少量のテキストを格納するためのものです。  パフォーマンスは、大量のテキストを保存するため使用しようとする場合は低速可能性があります。

## <a name="api"></a>API

- [SecureStorage ソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/SecureStorage)
- [SecureStorage API ドキュメント](xref:Xamarin.Essentials.SecureStorage)
