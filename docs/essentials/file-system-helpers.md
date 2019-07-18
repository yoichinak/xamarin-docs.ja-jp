---
title: 'Xamarin.Essentials: ファイル システム ヘルパー'
description: Xamarin.Essentials の FileSystem クラスには、アプリケーションのキャッシュ ディレクトリやデータ ディレクトリを検索したり、アプリ パッケージ内のファイルを開いたりする、一連のヘルパーが含まれています。
ms.assetid: B3EC2DE0-EFC0-410C-AF71-7410AE84CF84
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 74089d55006555168d9774924c484cb8866cfb37
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2018
ms.locfileid: "52898851"
---
# <a name="xamarinessentials-file-system-helpers"></a>Xamarin.Essentials: ファイル システム ヘルパー

**FileSystem** クラスには、アプリケーションのキャッシュ ディレクトリやデータ ディレクトリを検索したり、アプリ パッケージ内のファイルを開いたりする、一連のヘルパーが含まれています。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-file-system-helpers"></a>FileSystem の使用

自分のクラスに Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

**キャッシュ データ**を格納するアプリケーションのディレクトリを取得するには、次のようにします。 キャッシュ データは、一時的なデータより長く保持する必要があるデータに対して使用できますが、正しく動作する必要のあるデータには使用できません。

```csharp
var cacheDir = FileSystem.CacheDirectory;
```

ユーザー データ ファイルではないファイルに対するアプリケーションの最上位ディレクトリを取得するには、次のようにします。 これらのファイルは、オペレーティング システムの同期フレームワークでバックアップされます。 以下のプラットフォームの実装の詳細をご覧ください。

```csharp
var mainDir = FileSystem.AppDataDirectory;
```

アプリケーション パッケージにバンドルされているファイルを開くには:

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

- **CacheDirectory** – 現在のコンテキストの [CacheDir](https://developer.android.com/reference/android/content/Context.html#getCacheDir) を返します。
- **AppDataDirectory** – 現在のコンテキストの [FilesDir](https://developer.android.com/reference/android/content/Context.html#getFilesDir) を返します。API 23 以降では、[自動バックアップ](https://developer.android.com/guide/topics/data/autobackup.html)を使用してバックアップされます。

ファイルを Android プロジェクトの **Assets** フォルダーに追加し、`OpenAppPackageFileAsync` で使用されるようにビルド アクションを **AndroidAsset** としてマークします。

# <a name="iostabios"></a>[iOS](#tab/ios)

- **CacheDirectory** – [Library/Caches](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) ディレクトリを返します。
- **AppDataDirectory** – iTunes と iCloud でバックアップされる [Library](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) ディレクトリを返します。

ファイルを iOS プロジェクトの **Resources** フォルダーに追加し、`OpenAppPackageFileAsync` で使用されるようにビルド アクションを **BundledResource** としてマークします。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

- **CacheDirectory** – [LocalCacheFolder](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localcachefolder#Windows_Storage_ApplicationData_LocalCacheFolder) ディレクトリを返します.
- **AppDataDirectory** – クラウドにバックアップされる [LocalFolder](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder#Windows_Storage_ApplicationData_LocalFolder) ディレクトリを返します。

UWP プロジェクトのルートにファイルを追加し、`OpenAppPackageFileAsync` で使用されるようにビルド アクションを **Content** としてマークします。

--------------

## <a name="api"></a>API

- [FileSystem のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/FileSystem)
- [FileSystem API のドキュメント](xref:Xamarin.Essentials.FileSystem)
