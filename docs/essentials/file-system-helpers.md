---
title: 'Xamarin.Essentials: ファイル システム ヘルパー'
description: Xamarin.Essentials でファイルシステム クラスには、一連アプリ パッケージ内のファイルを開き、アプリケーションのキャッシュおよびデータ ディレクトリを検索するヘルパーにはが含まれています。
ms.assetid: B3EC2DE0-EFC0-410C-AF71-7410AE84CF84
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 13293ec05261cbdc1e70fd278002d1af18654851
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815619"
---
# <a name="xamarinessentials-file-system-helpers"></a>Xamarin.Essentials: ファイル システム ヘルパー

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

**FileSystem**クラスには、一連アプリ パッケージ内のファイルを開き、アプリケーションのキャッシュおよびデータ ディレクトリを検索するヘルパーにはが含まれています。

## <a name="using-file-system-helpers"></a>ファイル システム ヘルパーの使用

クラスで Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

格納するアプリケーションのディレクトリを取得する**データ キャッシュ**します。 一時的なデータよりも長く保持する必要がありますが、正しく動作するのには必要なデータをすることはできませんのデータのキャッシュ データを使用できます。

```csharp
var cacheDir = FileSystem.CacheDirectory;
```

ユーザー データ ファイルではないすべてのファイルのアプリケーションの最上位レベルのディレクトリを取得します。 これらのファイルは、同期フレームワーク、オペレーティング システムでバックアップされます。 プラットフォームの実装における次を参照してください。

```csharp
var mainDir = FileSystem.AppDataDirectory;
```

アプリケーション パッケージにバンドルされているファイルを開く。

```csharp
 using (var stream = await FileSystem.OpenAppPackageFileAsync(templateFileName))
 {
    using (var reader = new StreamReader(stream))
    {
        var fileContents = await reader.ReadToEndAsync();
    }
 }
```

## <a name="platform-implementation-specifics"></a>プラットフォームの実装の詳細

# <a name="androidtabandroid"></a>[Android](#tab/android)

- **CacheDirectory** – 返します、 [CacheDir](https://developer.android.com/reference/android/content/Context.html#getCacheDir)の現在のコンテキスト。
- **AppDataDirectory** – 返します、 [FilesDir](https://developer.android.com/reference/android/content/Context.html#getFilesDir)は、現在のコンテキストのバックアップを使用して[自動バックアップ](https://developer.android.com/guide/topics/data/autobackup.html)API 23 以降に開始します。

ファイルに追加して、**資産**フォルダーは、android プロジェクトし、ビルド アクションとしてのマーク**AndroidAsset**と共に使用する`OpenAppPackageFileAsync`します。

# <a name="iostabios"></a>[iOS](#tab/ios)

- **CacheDirectory** – 返します、[ライブラリ/キャッシュ](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html)ディレクトリ。
- **AppDataDirectory** – 返します、[ライブラリ](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html)iTunes と iCloud でバックアップされるディレクトリ。

ファイルに追加して、**リソース**フォルダーで、iOS プロジェクトし、ビルド アクションとしてのマーク**BundledResource**と共に使用する`OpenAppPackageFileAsync`します。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

- **CacheDirectory** – 返します、 [LocalCacheFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localcachefolder#Windows_Storage_ApplicationData_LocalCacheFolder)ディレクトリ.
- **AppDataDirectory** – 返します、 [LocalFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localfolder#Windows_Storage_ApplicationData_LocalFolder)ディレクトリをクラウドにバックアップします。

UWP プロジェクトのルートに任意のファイルを追加し、ビルド アクションとしてのマーク**コンテンツ**と共に使用する`OpenAppPackageFileAsync`します。

--------------

## <a name="api"></a>API

- [ファイル システム ヘルパーのソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/FileSystem)
- [ファイル システム API ドキュメント](xref:Xamarin.Essentials.FileSystem)
