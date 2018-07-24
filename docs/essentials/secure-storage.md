---
title: 'Xamarin.Essentials: セキュリティで保護されたストレージ'
description: このドキュメントでは、Xamarin.Essentials で、単純なキー/値ペアを安全に格納できるで SecureStorage クラスについて説明します。 これには、クラス、プラットフォームの実装の詳細、および制限事項を使用する方法について説明します。
ms.assetid: 78856C0D-76BB-406E-A880-D5A3987B7D64
author: redth
ms.author: jodick
ms.date: 05/04/2018
ms.openlocfilehash: fae5f5f0f15d80e2f3bdce26b8beb5f6fae2f81f
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38830454"
---
# <a name="xamarinessentials-secure-storage"></a>Xamarin.Essentials: セキュリティで保護されたストレージ

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

**SecureStorage**クラスは、単純なキー/値ペアを安全に保存します。

## <a name="getting-started"></a>作業の開始

アクセスする、 **SecureStorage**機能では、次のプラットフォームに固有の設定が必要です。

# <a name="androidtabandroid"></a>[Android](#tab/android)

追加の設定が必要です。

# <a name="iostabios"></a>[iOS](#tab/ios)

IOS シミュレーターで開発する場合に有効にする、**キーチェーン**権利し、アプリケーションのバンドル識別子のキーチェーン アクセス グループを追加します。

開く、 **Entitlements.plist**で iOS プロジェクトを見つけて、**キーチェーン**権利を有効にします。 アプリケーションの識別子をグループとして自動的に追加されます。

プロジェクトのプロパティで  **iOS バンドル署名**設定、**カスタム権利**に**Entitlements.plist**します。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

追加の設定が必要です。

-----

## <a name="using-secure-storage"></a>セキュリティで保護されたストレージを使用します。

クラスで Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

値を保存する、指定された_キー_でセキュリティで保護された記憶域。

```csharp
await SecureStorage.SetAsync("oauth_token", "secret-oauth-token-value");
```

セキュリティで保護された記憶域から値を取得します。

```csharp
var oauthToken = await SecureStorage.GetAsync("oauth_token");
```

> [!NOTE]
> 要求されたキーに関連付けられている値がない場合`GetAsync`戻ります`null`します。

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

[Android キーストア](https://developer.android.com/training/articles/keystore.html)に保存されるまで、値の暗号化に使用される暗号キーを格納するために使用、[共有設定](https://developer.android.com/training/data-storage/shared-preferences.html)のファイル名を持つ **[YOUR-アプリのパッケージの ID] .xamarinessentials**.  共有設定ファイルで使用されるキーは、 _MD5 ハッシュ_に渡されたキーの`SecureStorage`の API。

## <a name="api-level-23-and-higher"></a>API レベル 23 以上

新しい API レベルで、 **AES**キーが、Android キーストアから取得したし、は併用、 **AES/GCM/NoPadding**共有設定ファイルに格納されているが、前に、値を暗号化する暗号。

## <a name="api-level-22-and-lower"></a>API レベル 22 と下限

以前の API レベルで、Android キーストアのみをサポートを格納する**RSA**と共に使用される、キー、 **RSA/ECB/PKCS1Padding**を暗号化する暗号、 **AES** (ランダムにキー実行時に生成)、キーの下で、共有設定ファイルに格納されていると_SecureStorageKey_、1 つ既に生成されていない場合。

すべての暗号化された値は、デバイスからアプリをアンインストールしても削除されます。

# <a name="iostabios"></a>[iOS](#tab/ios)

[キーチェーン](https://developer.xamarin.com/api/type/Security.SecKeyChain/)iOS デバイスで安全に値を格納するために使用します。  `SecRecord`値を格納するために使用が、`Service`値に設定 **[YOUR-アプリのバンドルの ID] .xamarinessentials**します。

場合によってはキーチェーン データが iCloud と同期され、アプリケーションのアンインストール削除ことはできません、セキュリティで保護された値から iCloud やその他のユーザーのデバイス。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[DataProtectionProvider](https://docs.microsoft.com/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider) UWP デバイスで安全に暗号化された値を使用します。

暗号化された値が格納されている`ApplicationData.Current.LocalSettings`、という名前のコンテナーの内部 **[YOUR APP ID] .xamarinessentials**します。

により、アプリケーションのアンインストール、 _LocalSettings_と暗号化されたすべての値を削除することもできます。

-----

## <a name="limitations"></a>制限事項

この API は、少量のテキストを格納するものです。  パフォーマンスは、大量のテキストの格納に使用しようとする場合は低速可能性があります。

## <a name="api"></a>API

- [SecureStorage ソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/SecureStorage)
- [SecureStorage API ドキュメント](xref:Xamarin.Essentials.SecureStorage)
