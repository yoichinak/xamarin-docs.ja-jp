---
title: Xamarin.iOS での SQLite の構成
description: このドキュメントでは、Xamarin.iOS アプリケーションでの SQLite データベース ファイルの場所を決定する方法について説明します。 これらの概念は、選択したデータ アクセス メカニズムに関係します。
ms.prod: xamarin
ms.assetid: E5582F4B-AD74-420F-9E6D-B07CFB420B3A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/11/2016
ms.openlocfilehash: ce9822b9c6c6a360f8c289f46d4d777d80b898e1
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61217428"
---
# <a name="configuring-sqlite-in-xamarinios"></a>Xamarin.iOS での SQLite の構成

Xamarin.iOS アプリケーションでの SQLite の使用には、データベース ファイルの正しいファイルの場所を決定する必要があります。

## <a name="database-file-path"></a>データベース ファイルのパス

使用するデータ アクセス方法に関係なく、SQLite の使用データを格納できる前に、データベース ファイルを作成する必要があります。 対象とするプラットフォームに応じて、ファイルの場所が変更されます。 IOS 用の次のコード スニペットに示すように、有効なパスを構築するのに環境のクラスを使用できます。

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "database.db3");
// dbPath contains a valid file path for the database file to be stored
```

データベース ファイルを格納する場所を決定する際に考慮するその他の点があります。 IOS、データベースのバックアップに自動的に (またはない) にする場合があります。

クロスプラット フォーム アプリケーションでは、各プラットフォーム上で別の場所を使用する場合はディレクティブを使用するコンパイラに示すようプラットフォームごとに別のパスを生成します。

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

参照してください、[ファイル システム操作](~/ios/app-fundamentals/file-system.md)iOS で使用するどのようなファイルの場所の詳細については資料。 参照してください、[クロス プラットフォーム アプリケーションの構築](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)コンパイラ ディレクティブを使用して、それぞれのプラットフォームに固有のコードを記述する方法についてのドキュメント。

## <a name="threading"></a>スレッド

複数のスレッド間で同じの SQLite データベース接続を使用する必要があります。 開き、使用し、同じスレッドで作成するすべての接続を閉じるように注意します。

コードが複数のスレッドから同時に SQLite データベースへのアクセスを試みていないことを確認するには、手動でロックを取得したら、次のように、データベースにアクセスするたびに。

```csharp
object locker = new object(); // class level private field
// rest of class code
lock (locker){
    // Do your query or insert here
}
```

すべてのデータベース アクセス (読み取り、書き込み、更新プログラムなど) は、同じロックでラップする必要があります。 ロック句内での作業が単純な保持され、ロックがかかる場合がありますもその他の方法呼び出しませんことを確認して、デッドロックを回避するために注意する必要があります。


## <a name="related-links"></a>関連リンク

- [DataAccess Basic (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [データ アクセスの詳細 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS データ レシピ](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin.Forms のデータ アクセス](~/xamarin-forms/app-fundamentals/databases.md)
