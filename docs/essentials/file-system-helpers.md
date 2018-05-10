---
title: Xamarin.Essentials ファイル システムのヘルパー
description: FileSystem クラスには、アプリ パッケージ内でファイルを開き、アプリケーションのキャッシュおよびデータ ディレクトリを検索するヘルパーのシリーズが含まれています。
ms.assetid: B3EC2DE0-EFC0-410C-AF71-7410AE84CF84
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 2d660e4c325b72817c7386c43c0d1dde909c4921
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-file-system-helpers"></a>Xamarin.Essentials ファイル システムのヘルパー

![プレリリース NuGet](~/media/shared/pre-release.png)

**FileSystem**クラスには、アプリケーションのキャッシュおよびデータ ディレクトリを検索して、アプリ パッケージ内でファイルを開くヘルパーのシリーズが含まれています。

## <a name="using-file-system-helpers"></a>ファイル システム ヘルパーの使用

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

格納するアプリケーションのディレクトリを取得する**データ キャッシュ**です。 すべてのデータに一時的なデータよりも長く保持する必要がありますが、正しく動作するために必要なデータをすることはできません、キャッシュ データを使用できます。

```csharp
var cacheDir = FileSystem.CacheDirectory;
```

ユーザー データ ファイルではないすべてのファイルについて、アプリケーションの最上位 diredctory を取得します。 これらのファイルはフレームワークを同期しているオペレーティング システムと共にバックアップされます。 プラットフォームの実装における次を参照してください。

```csharp
var mainDir = FileSystem.AppDataDirectory;
```

アプリケーション パッケージにバンドルされているファイルを開きます。

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

- **CacheDirectory** – を返します、 [CacheDir](https://developer.android.com/reference/android/content/Context.html#getCacheDir)現在のコンテキスト。
- **AppDataDirectory** – を返します、 [FilesDir](https://developer.android.com/reference/android/content/Context.html#getFilesDir)は、現在のコンテキストのバックアップを使用して[Autu バックアップ](https://developer.android.com/guide/topics/data/autobackup.html)API 23 以降を起動します。

任意のファイルを追加、**資産**フォルダーに、Android プロジェクトおよびとしてビルド アクションをマーク**AndroidAsset**と共に使用する`OpenAppPackageFileAsync`です。

# <a name="iostabios"></a>[iOS](#tab/ios)

- **CacheDirectory** – を返します、[ライブラリ/キャッシュ](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html)ディレクトリ。
- **AppDataDirectory** – を返します、[ライブラリ](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html)iTunes と iCloud でバックアップされているディレクトリ。

任意のファイルを追加、**リソース**フォルダー、iOS にプロジェクトし、としてビルド アクションをマーク**BundledResource**と共に使用する`OpenAppPackageFileAsync`です。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

- **CacheDirectory** – を返します、 [LocalCacheFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localcachefolder#Windows_Storage_ApplicationData_LocalCacheFolder)ディレクトリ.
- **AppDataDirectory** – を返します、 [LocalFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localfolder#Windows_Storage_ApplicationData_LocalFolder)をクラウドにバックアップされるディレクトリ。

UWP プロジェクトのルートにすべてのファイルを追加し、ビルド アクションとしてのマーク**コンテンツ**と共に使用する`OpenAppPackageFileAsync`です。

--------------

## <a name="api"></a>API

- [ファイル システム ヘルパーのソース コード](https://github.com/xamarin/Essentials/tree/master/Essentials/FileSystem)
- [ファイル システム API ドキュメント](xref:Xamarin.Essentials.FileSystem)
