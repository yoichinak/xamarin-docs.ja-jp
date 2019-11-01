---
title: 構成
ms.prod: xamarin
ms.assetid: 44526226-4E4E-4FFF-9A16-CA7B1E01BB8F
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 10/11/2016
ms.openlocfilehash: 29bbd5c52a5af6c0ac9f9790dc182d3d806c6b5f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73024026"
---
# <a name="configuration"></a>構成

SQLite を Xamarin Android アプリケーションで使用するには、データベースファイルの正しいファイルの場所を決定する必要があります。

## <a name="database-file-path"></a>データベースファイルのパス

使用するデータアクセス方法に関係なく、SQLite を使用してデータを格納する前に、データベースファイルを作成する必要があります。 対象となるプラットフォームによって、ファイルの場所が異なります。 Android の場合、次のコードスニペットに示すように、環境クラスを使用して有効なパスを構築できます。

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "database.db3");
// dbPath contains a valid file path for the database file to be stored
```

データベースファイルの格納場所を決定する際には、他にも考慮する必要があります。 たとえば、Android では、内部ストレージと外部ストレージのどちらを使用するかを選択できます。

クロスプラットフォームアプリケーションの各プラットフォームで別の場所を使用する場合は、次に示すようにコンパイラディレクティブを使用して、プラットフォームごとに異なるパスを生成できます。

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

Android でファイルシステムを使用する場合のヒントについては、[ファイルの参照](https://github.com/xamarin/recipes/tree/master/Recipes/android/data/files/browse_files)に関するレシピを参照してください。 コンパイラディレクティブを使用して各プラットフォームに固有のコードを記述する方法の詳細については、[クロスプラットフォームアプリケーションのビルド](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)に関するドキュメントを参照してください。

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
- [Android データレシピ](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin. フォームデータアクセス](~/xamarin-forms/data-cloud/data/databases.md)
