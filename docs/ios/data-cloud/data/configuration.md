---
title: Xamarin での SQLite の構成
description: このドキュメントでは、Xamarin iOS アプリケーションで SQLite データベースファイルの場所を確認する方法について説明します。 これらの概念は、選択したデータアクセスメカニズムに関係なく適用されます。
ms.prod: xamarin
ms.assetid: E5582F4B-AD74-420F-9E6D-B07CFB420B3A
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 10/11/2016
ms.openlocfilehash: c0a8f57e3f4f351cf5b874ded2639b975ea71cad
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70281906"
---
# <a name="configuring-sqlite-in-xamarinios"></a>Xamarin での SQLite の構成

使用している Xamarin アプリケーションで SQLite を使用するには、データベースファイルの正しいファイルの場所を決定する必要があります。

## <a name="database-file-path"></a>データベースファイルのパス

使用するデータアクセス方法に関係なく、SQLite を使用してデータを格納する前に、データベースファイルを作成する必要があります。 対象となるプラットフォームによって、ファイルの場所が異なります。 IOS の場合、次のコードスニペットに示すように、環境クラスを使用して有効なパスを構築できます。

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "database.db3");
// dbPath contains a valid file path for the database file to be stored
```

データベースファイルの格納場所を決定する際には、他にも考慮する必要があります。 IOS では、データベースを自動的にバックアップすることができます (または無効)。

クロスプラットフォームアプリケーションの各プラットフォームで別の場所を使用する場合は、次に示すようにコンパイラディレクティブを使用して、プラットフォームごとに異なるパスを生成できます。

```csharp
var sqliteFilename = "MyDatabase.db3";
#if __ANDROID__
// Just use whatever directory SpecialFolder.Personal returns
string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); ;
#else
// we need to put in /Library/ on iOS5.1+ to meet Apple's iCloud terms
// (they don't want non-user-generated data in Documents)
string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
string libraryPath = Path.Combine (documentsPath, "..", "Library"); // Library folder instead
#endif
var path = Path.Combine (libraryPath, sqliteFilename);
```

IOS で使用するファイルの場所の詳細については、「[ファイルシステムの操作](~/ios/app-fundamentals/file-system.md)」を参照してください。 コンパイラディレクティブを使用して各プラットフォームに固有のコードを記述する方法の詳細については、[クロスプラットフォームアプリケーションのビルド](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)に関するドキュメントを参照してください。

## <a name="threading"></a>スレッド

複数のスレッド間で同じ SQLite データベース接続を使用しないでください。 同じスレッドで作成したすべての接続を開いて使用し、閉じるように注意してください。

コードが同時に複数のスレッドから SQLite データベースにアクセスしようとしていないことを確認するには、次のように、データベースにアクセスするたびにロックを手動で行います。

```csharp
object locker = new object(); // class level private field
// rest of class code
lock (locker){
    // Do your query or insert here
}
```

すべてのデータベースアクセス (読み取り、書き込み、更新など) は、同じロックでラップする必要があります。 デッドロックの状況を回避するには、ロック句内の作業を単純なままにして、ロックを受け取る可能性のある他のメソッドを呼び出さないようにする必要があります。


## <a name="related-links"></a>関連リンク

- [このような場合の基本 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [詳細設定 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS データレシピ](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin.Forms データアクセス](~/xamarin-forms/data-cloud/data/databases.md)
