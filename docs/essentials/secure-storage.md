---
title: 'Xamarin.Essentials: セキュリティで保護されたストレージ'
description: このドキュメントでは Xamarin.Essentials の SecureStorage クラスについて説明します。これは単純なキーと値のペアを安全に格納するのに役立ちます。 ここでは、クラスの使用方法、プラットフォームの実装の詳細、および制限事項について説明します。
ms.assetid: 78856C0D-76BB-406E-A880-D5A3987B7D64
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: d9367b7bbaa906ce39d1fafb3d09e56bfd0dc238
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675227"
---
# <a name="xamarinessentials-secure-storage"></a>Xamarin.Essentials: セキュリティで保護されたストレージ

![プレリリースの NuGet](~/media/shared/pre-release.png)

**SecureStorage** クラスは、単純なキーと値のペアを安全に格納するのに役立ちます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

**SecureStorage** の機能にアクセスするには、次のプラットフォーム固有の設定が必要です。

# <a name="androidtabandroid"></a>[Android](#tab/android)

> [!TIP]
> [アプリの自動バックアップ](https://developer.android.com/guide/topics/data/autobackup)は Android 6.0 (API レベル 23) 以降の機能です。ユーザーのアプリ データ (共有の設定、アプリの内部ストレージ内のファイル、その他の特定のファイル) がバックアップされます。 アプリが新しいデバイスに再インストールまたはインストールされると、データが復元されます。 これは、バックアップされ、復元するときに暗号化解除できない共有の設定を利用する `SecureStorage` に影響を与えます。 Xamarin.Essentials では、リセットできるようにキーを削除することで自動的にこのケースを処理しますが、自動バックアップを無効にすることで追加の手順を実行できます。

### <a name="enable-or-disable-backup"></a>バックアップを有効または無効にする
`AndroidManifest.xml` ファイルの `android:allowBackup` 設定を false に設定することで、アプリケーション全体の自動バックアップを無効にすることができます。 この方法が推奨されるのは、データを復元する別の方法を計画する場合のみです。

```xml
<manifest ... >
    ...
    <application android:allowBackup="false" ... >
        ...
    </application>
</manifest>
```

### <a name="selective-backup"></a>選択的バックアップ
自動バックアップを構成して、特定のコンテンツのバックアップを無効にすることができます。 カスタム規則セットを作成して、バックアップ対象から `SecureStore` 項目を除外することができます。

1. **AndroidManifest.xml** で `android:fullBackupContent` 属性を設定します。

    ```xml
    <application ...
        android:fullBackupContent="@xml/auto_backup_rules">
    </application>
    ```

2. **auto_backup_rules.xml** という名前の新しい XML ファイルを **Resources/xml** ディレクトリに作成します。 次に、`SecureStorage` を除くすべての共有の設定を含める次の内容を設定します。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <full-backup-content>
        <include domain="sharedpref" path="."/>
        <exclude domain="sharedpref" path="${applicationId}.xamarinessentials.xml"/>
    </full-backup-content>
    ```

# <a name="iostabios"></a>[iOS](#tab/ios)

**iOS シミュレーター**上で開発している場合は、**キーチェーン** エンタイトルメントを有効にし、アプリケーションのバンドル ID に対してキーチェーンのアクセス グループを追加します。 

iOS プロジェクト内の **Entitlements.plist** を開き、**キーチェーン** エンタイトルメントを見つけて有効にします。 これで、アプリケーションの ID がグループとして自動的に追加されます。

プロジェクトのプロパティの **[iOS バンドル署名]** で、**[カスタムの権利]** を **Entitlements.plist** に設定します。

> [!TIP]
> iOS デバイスに展開する場合は、このエンタイトルメントは不要であり、削除する必要があります。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

追加の設定は必要ありません。

-----

## <a name="using-secure-storage"></a>セキュリティで保護されたストレージの使用

自分のクラスの Xamarin.Essentials に参照を追加します。

```csharp
using Xamarin.Essentials;
```

指定された_キー_に対する値をセキュリティで保護されたストレージに保存するには:

```csharp
try
{
  await SecureStorage.SetAsync("oauth_token", "secret-oauth-token-value");
}
catch (Exception ex)
{
  // Possible that device doesn't support secure storage on device.
}
```

セキュリティで保護されたストレージから値を取得するには:

```csharp
try
{
  var oauthToken = await SecureStorage.GetAsync("oauth_token");
}
catch (Exception ex)
{
  // Possible that device doesn't support secure storage on device.
}
```

> [!NOTE]
> 要求されたキーに関連付けられている値が存在しない場合、`GetAsync` は `null` を返します。

特定のキーを削除するには、次を呼び出します。

```csharp
SecureStorage.Remove("oauth_token");
```

すべてのキーを削除するには、次を呼び出します。

```csharp
SecureStorage.RemoveAll();
```


## <a name="platform-implementation-specifics"></a>プラットフォームの実装の詳細

# <a name="androidtabandroid"></a>[Android](#tab/android)

[Android キーストア](https://developer.android.com/training/articles/keystore.html)は、[共有の設定](https://developer.android.com/training/data-storage/shared-preferences.html)に **[アプリのパッケージ ID].xamarinessentials** というファイル名で保存する前に、値を暗号化するための暗号キーを格納するために使用されます。  共有の設定ファイルで使用されるキーは、`SecureStorage` API に渡されるキーの _MD5 Hash_ です。

## <a name="api-level-23-and-higher"></a>API レベル 23 以上

新しい API レベルでは、Android キーストアから **AES** キーを取得し、**AES/GCM/NoPadding** 暗号と共に使用して、共有の設定ファイルに格納する前に値を暗号化します。

## <a name="api-level-22-and-lower"></a>API レベル 22 以下

古い API レベルでは、Android キーストアは **RSA** キーの格納のみをサポートしています。これは **RSA/ECB/PKCS1Padding** 暗号と共に使用され、**AES** キー (実行時にランダムで生成されます) を暗号化し、キー _SecureStorageKey_ の下で共有の設定ファイルに格納されます (まだ作成されていない場合)。

**SecureStorage** は [Preferences](preferences.md) API を使用し、[Preferences](preferences.md#persistence) ドキュメントで説明されているのと同じデータ永続化に従います。 デバイスが API レベル 22 以下から API レベル 23 以上にアップグレードされる場合、アプリをアンインストールするか **RemoveAll** を呼び出さない限り、この種類の暗号化が使用され続けます。

# <a name="iostabios"></a>[iOS](#tab/ios)

iOS デバイスに値を安全に格納するために、[キーチェーン](https://developer.xamarin.com/api/type/Security.SecKeyChain/)が使用されます。  値を格納するために使用される `SecRecord` は、**[アプリのバンドル ID].xamarinessentials** に設定された `Service` 値を持ちます。

場合によっては、キーチェーン データが iCloud と同期され、アプリケーションをアンインストールしても iCloud やユーザーのその他のデバイスからセキュリティで保護された値が削除されない場合があります。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

暗号化された値に対して、UWP デバイス上で [DataProtectionProvider](https://docs.microsoft.com/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider) が安全に使用されます。

暗号化された値は、`ApplicationData.Current.LocalSettings` の、**[アプリの ID].xamarinessentials** という名前のコンテナーの内部に格納されます。

**SecureStorage** は [Preferences](preferences.md) API を使用し、[Preferences](preferences.md#persistence) ドキュメントで説明されているのと同じデータ永続化に従います。

-----

## <a name="limitations"></a>制限事項

この API は、少量のテキストを格納することを想定しています。  大量のテキストを格納するためにこれを使用しようとすると、パフォーマンスが低下する可能性があります。

## <a name="api"></a>API

- [SecureStorage のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/SecureStorage)
- [SecureStorage API ドキュメント](xref:Xamarin.Essentials.SecureStorage)
