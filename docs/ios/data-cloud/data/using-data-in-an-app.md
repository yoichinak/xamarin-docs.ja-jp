---
title: IOS アプリでデータを使用
description: このドキュメントには、作成するサンプルについては、ユーザー入力を収集し、実行する方法を示します DataAccess_Adv がについて説明します、読み取り、更新、および削除 (CRUD) のデータベースで Xamarin.iOS アプリを操作します。
ms.prod: xamarin
ms.assetid: 2CB8150E-CD2C-4E97-8605-1EE8CBACFEEC
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/11/2016
ms.openlocfilehash: a47ab26777c594658810b014025477486bbe5cc2
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/08/2019
ms.locfileid: "67650150"
---
# <a name="using-data-in-an-ios-app"></a>IOS アプリでデータを使用

**DataAccess_Adv**サンプル ユーザー入力を許可する実用アプリケーションの表示と*CRUD* (作成、読み取り、更新、削除) のデータベース機能です。 アプリケーションは、2 つの画面で構成されています。 一覧と、データ エントリ フォーム。 すべてのデータ アクセス コードは変更しなくても iOS と Android で再利用できます。

アプリケーションの画面は、いくつかのデータを追加した後、iOS のようなります。

 ![](using-data-in-an-app-images/image9.png "iOS のサンプル一覧")

 ![](using-data-in-an-app-images/image10.png "iOS のサンプルの詳細")

IOS プロジェクトを次に示します: このセクションで示すコードが含まれる、 **Orm**ディレクトリ。

 ![](using-data-in-an-app-images/image13.png "iOS プロジェクトのツリー")

IOS で ViewControllers のネイティブの UI コードは、このドキュメントの対象外です。
参照してください、 [iOS テーブルとセル操作](~/ios/user-interface/controls/tables/index.md)UI コントロールの詳細についてはガイド。

## <a name="read"></a>読み取り

このサンプルでの読み取り操作のいくつかがあります。

-  一覧の読み取り
-  個々 のレコードの読み取り


2 つのメソッド、`StockDatabase`クラスには。

```csharp
public IEnumerable<Stock> GetStocks ()
{
    lock (locker) {
        return (from i in Table<Stock> () select i).ToList ();
    }
}
public Stock GetStock (int id)
{
    lock (locker) {
        return Table<Stock>().FirstOrDefault(x => x.Id == id);
    }
}
```

iOS には、データがレンダリングされます。 異なる方法でとしてを`UITableView`。

## <a name="create-and-update"></a>作成し、更新

アプリケーション コードを簡素化するには、1 つの save メソッドが挿入は提供または 更新の主キーが設定されているかどうかによって異なります。 `Id`プロパティが付いて、`[PrimaryKey]`属性は、コードで設定しないでください。
このメソッドが検出されたかどうか、値が前 (主キー プロパティをチェック) を保存し、挿入するか、それに応じてオブジェクトを更新、します。

```csharp
public int SaveStock (Stock item)
{
    lock (locker) {
        if (item.Id != 0) {
            Update (item);
            return item.Id;
    } else {
            return Insert (item);
        }
    }
}
```



実践的なアプリケーションは (必要なフィールド、最低限の長さやその他のビジネス ルール) などのいくつかの検証に通常必要とします。
優れたクロスプラット フォーム対応のアプリケーションは、共有コード、プラットフォームの機能に従って表示するための UI へのバックアップの検証エラーを渡すことで可能な論理の検証の実装します。

## <a name="delete"></a>削除

異なり、`Insert`と`Update`、メソッド、`Delete<T>`メソッドが完全ではなく主キーの値だけを受け入れる`Stock`オブジェクト。
この例では、`Stock`オブジェクトは、メソッドに渡されますに Id プロパティのみが渡され、`Delete<T>`メソッド。

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>事前設定された SQLite データベース ファイルを使用します。

一部のアプリケーションには、データに読み込まれているデータベースは同梱されています。
簡単にこれを行うモバイル アプリケーションで、アプリで既存の SQLite データベース ファイルを配布して書き込み可能なディレクトリにアクセスする前にコピーすることです。 SQLite は、多くのプラットフォームで使用される標準的なファイル形式であるためには、さまざまな SQLite データベース ファイルの作成に使用できるツールがあります。

-  **Firefox 拡張機能の SQLite Manager** – iOS および Android と互換性があるファイルを Mac と Windows が生成されます。
-  **コマンド ライン**– を参照してください[www.sqlite.org/sqlite.html](http://www.sqlite.org/sqlite.html)します。


アプリを配布用のデータベース ファイルを作成するときと確実に、コードでは、c# のクラスおよびプロパティに一致する名前を必要とする SQLite.NET を使用している場合に特にテーブルと列の名前付けを使用処理 (または、関連付けられているカスタム属性の場合)。

Ios の場合、アプリケーションに sqlite ファイルを含めるし、が付いていることを確認**ビルド アクション。コンテンツ**します。 内のコードを配置、`FinishedLaunching`ファイルを書き込み可能なディレクトリにコピーする*する前に*データ メソッドを呼び出します。 次のコードと呼ばれる既存のデータベースのコピーは**data.sqlite**存在しない場合にのみ、します。

```csharp
// Copy the database across (if it doesn't exist)
var appdir = NSBundle.MainBundle.ResourcePath;
var seedFile = Path.Combine (appdir, "data.sqlite");
if (!File.Exists (Database.DatabaseFilePath))
{
  File.Copy (seedFile, Database.DatabaseFilePath);
}
```

データ アクセス コード (かどうか ADO.NET または SQLite.NET を使用して) があるあらかじめ設定されているデータへのアクセスは完了した後に実行します。


## <a name="related-links"></a>関連リンク

- [DataAccess Basic (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [データ アクセスの詳細 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS データ レシピ](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin.Forms のデータ アクセス](~/xamarin-forms/data-cloud/data/databases.md)
