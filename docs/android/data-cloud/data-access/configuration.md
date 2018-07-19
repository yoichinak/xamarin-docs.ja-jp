---
title: 構成
ms.prod: xamarin
ms.assetid: 44526226-4E4E-4FFF-9A16-CA7B1E01BB8F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 10/11/2016
ms.openlocfilehash: cf474015b28d9708d69719b38348391091040a28
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30762585"
---
# <a name="configuration"></a>構成

SQLite を Xamarin.Android アプリケーションで使用するには、データベース ファイルの正しいファイルの場所を特定する必要があります。

## <a name="database-file-path"></a>Database File Path

使用するデータのアクセス方法に関係なく SQLite にデータを格納できる前に、データベース ファイルを作成する必要があります。 対象とするプラットフォームに応じて、ファイルの場所が変更されます。 Android 用の次のコード スニペットに示すように、有効なパスを構築するために環境のクラスを使用できます。

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "database.db3");
// dbPath contains a valid file path for the database file to be stored
```

データベース ファイルを格納する場所を決定する際に考慮するその他の点があります。 たとえば、Android で選択できます内部または外部の記憶域を使用するかどうか。

場合は、クロス プラットフォーム アプリケーションでは、各プラットフォームで、別の場所を使用することができますディレクティブを使用するコンパイラようにを各プラットフォーム用の別のパスを生成します。

```csharp
var sqliteFilename = "MyDatabase.db3";
#if __ANDROID__
// Just use whatever directory SpecialFolder.Personal returns
string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); ;
#else
// we need to put in /Library/ on iOS5.1 to meet Apple's iCloud terms
// (they don't want non-user-generated data in Documents)
string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
string libraryPath = Path.Combine (documentsPath, "..", "Library"); // Library folder instead
#endif
var path = Path.Combine (libraryPath, sqliteFilename);
```

Android でのファイル システムの使用に関するヒントを参照してください、[ファイルの参照](https://developer.xamarin.com/recipes/android/data/Files/Browse_Files)レシピします。 参照してください、[クロス プラットフォーム アプリケーションの構築](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)コンパイラ ディレクティブを使用して、それぞれのプラットフォームに固有のコードを記述する方法の詳細については、ドキュメントです。

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
- [Android データ レシピ](https://developer.xamarin.com/recipes/android/data/)
- [Xamarin.Forms データ アクセス](~/xamarin-forms/app-fundamentals/databases.md)
