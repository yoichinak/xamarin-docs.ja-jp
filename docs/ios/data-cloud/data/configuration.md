---
title: Xamarin.iOS で SQLite を構成します。
description: このドキュメントでは、アプリケーションで Xamarin.iOS SQLite データベース ファイルの場所を特定する方法について説明します。 これらの概念は、選択したデータのアクセス メカニズムに関係なく適用されます。
ms.prod: xamarin
ms.assetid: E5582F4B-AD74-420F-9E6D-B07CFB420B3A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: 57645c0bb947a60a3d74436b752210d1bac18124
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784382"
---
# <a name="configuring-sqlite-in-xamarinios"></a>Xamarin.iOS で SQLite を構成します。

Xamarin.iOS アプリで SQLite を使用するには、データベース ファイルの正しいファイルの場所を特定する必要があります。

## <a name="database-file-path"></a>データベース ファイルのパス

使用するデータのアクセス方法に関係なく SQLite にデータを格納できる前に、データベース ファイルを作成する必要があります。 対象とするプラットフォームに応じて、ファイルの場所が変更されます。 IOS 用の次のコード スニペットに示すように、有効なパスを構築するために環境クラスを使用できます。

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "database.db3");
// dbPath contains a valid file path for the database file to be stored
```

データベース ファイルを格納する場所を決定する際に考慮するその他の点があります。 IOS では、データベースをバックアップに自動的に (またはない) ができます。

場合は、クロス プラットフォーム アプリケーションでは、各プラットフォームで、別の場所を使用することができますディレクティブを使用するコンパイラようにを各プラットフォーム用の別のパスを生成します。

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

参照してください、 [、ファイル システムで作業](~/ios/app-fundamentals/file-system.md)で iOS を使用するどのようなファイルの場所の詳細については資料です。 参照してください、[クロス プラットフォーム アプリケーションの構築](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)コンパイラ ディレクティブを使用して、それぞれのプラットフォームに固有のコードを記述する方法の詳細については、ドキュメントです。

## <a name="threading"></a>スレッド

複数のスレッドで同じ SQLite データベース接続を使用する必要がありますできません。 開き、使用し、同じスレッドで作成するすべての接続を閉じるように注意します。

コードが、同時に複数のスレッドから SQLite データベースにアクセスを試みていないことを手動でようにロック次のように、データベースにアクセスしようとするたびに実行します。

```csharp
object locker = new object(); // class level private field
// rest of class code
lock (locker){
    // Do your query or insert here
}
```

すべてのデータベース アクセス (読み取り、書き込み、更新プログラムなど) は、同じロックでラップする必要があります。 デッドロックを回避するようにすることによって、ロック句内の作業が単純に保持されるロックがかかる場合がありますも他のメソッド呼び出しませんに注意が必要です。


## <a name="related-links"></a>関連リンク

- [DataAccess Basic (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [データ アクセスの詳細 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS データ レシピ](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Xamarin.Forms データ アクセス](~/xamarin-forms/app-fundamentals/databases.md)
