---
title: IOS アプリでのデータの使用
description: Xamarin.iOS アプリでの操作を読み取り、更新、および削除 (CRUD) データベースをこのドキュメントは、サンプルでは、ユーザー入力を収集しを実行する方法を示します作成 DataAccess_Adv を説明します。
ms.prod: xamarin
ms.assetid: 2CB8150E-CD2C-4E97-8605-1EE8CBACFEEC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: 5c9eab9316539ecf5988c8768bef9ef2cd61513e
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784541"
---
# <a name="using-data-in-an-ios-app"></a>IOS アプリでのデータの使用

**DataAccess_Adv**例では、ユーザー入力を許可する作業アプリケーションおよび*CRUD* (作成、読み取り、更新、削除) のデータベース機能です。 アプリケーションは、2 つの画面で構成されます。 一覧と、データ エントリ フォーム。 すべてのデータ アクセス コードは変更せず、iOS および Android で再利用可能です。

アプリケーション画面は、一部のデータを追加した後、iOS で次のようになります。

 ![](using-data-in-an-app-images/image9.png "iOS のサンプル一覧")

 ![](using-data-in-an-app-images/image10.png "iOS サンプルの詳細")

IOS プロジェクトを次に示します: ここに示すようにコードが含まれる、 **Orm**ディレクトリ。

 ![](using-data-in-an-app-images/image13.png "iOS プロジェクトのツリー")

IOS で ViewControllers のネイティブの UI コードは、このドキュメントの対象外です。
参照してください、 [iOS テーブルとセル操作](~/ios/user-interface/controls/tables/index.md)の詳細については、UI コントロールのガイドです。

## <a name="read"></a>読み取り

サンプルの読み取り操作のいくつかあります。

-  一覧の読み取り
-  個々 のレコードの読み取り


2 つの方法で、`StockDatabase`クラスには。

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

iOS データの表示と異なる方法で、`UITableView`です。

## <a name="create-and-update"></a>作成および更新

アプリケーション コードを簡略化 save メソッドの 1 つが、Insert は提供または 更新、主キーが設定されているかどうかによって異なります。 `Id`プロパティが付いて、`[PrimaryKey]`属性のコードに設定しないでください。
このメソッドがされたかどうかの値以前 (主キーのプロパティをチェック) によって保存されるかを検出および挿入するか、それに応じてオブジェクトを更新します。

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



実際のアプリケーションは (必要なフィールド、最低限の長さやその他のビジネス ルール) などのいくつかの検証に通常必要とします。
適切なクロスプラット フォーム アプリケーションは、できるだけ多くの共有コード、プラットフォームの機能に従って表示するための UI へのバックアップの検証エラーを渡すことで可能な論理の検証の実装します。

## <a name="delete"></a>削除

異なり、`Insert`と`Update`メソッド、`Delete<T>`メソッドが完全ではなく、プライマリ キーの値だけを受け入れる`Stock`オブジェクト。
この例では、`Stock`オブジェクトがメソッドに渡されますに Id プロパティのみが渡され、`Delete<T>`メソッドです。

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>事前設定済み SQLite データベース ファイルの使用

一部のアプリケーションには、既にデータが設定されるデータベースは同梱されています。
簡単にこれを行うモバイル アプリでアプリを既存の SQLite データベース ファイルを配布してアクセスする前に書き込み可能なディレクトリにコピーすることです。 SQLite は、多くのプラットフォームで使用される標準的なファイル形式であるため、SQLite データベース ファイルの作成に使用できるツールの数があります。

-  **SQLite Manager Firefox 拡張**– iOS および Android と互換性があるファイルを Mac および Windows が生成されます。
-  **コマンド ライン**– を参照してください[www.sqlite.org/sqlite.html](http://www.sqlite.org/sqlite.html)です。


一致するどのようなコードが必要ですが、c# のクラスおよびプロパティと一致する名前を必要となる SQLite.NET を使用している場合に特にことを確認するテーブルと列の名前付けでアプリを配布用のデータベース ファイルを作成する際は注意 (または、関連付けられているカスタム属性の場合)。

Ios の場合、アプリケーションで、sqlite ファイルを含めるとマークされていることを確認**ビルド アクション: コンテンツ**です。 内のコードを配置、 `FinishedLaunching` 、ファイルを書き込み可能なディレクトリにコピーする*する前に*データ メソッドを呼び出します。 次のコードと呼ばれる、既存のデータベースのコピーは**data.sqlite**が存在しない場合にのみ、します。

```csharp
// Copy the database across (if it doesn't exist)
var appdir = NSBundle.MainBundle.ResourcePath;
var seedFile = Path.Combine (appdir, "data.sqlite");
if (!File.Exists (Database.DatabaseFilePath))
{
  File.Copy (seedFile, Database.DatabaseFilePath);
}
```

任意のデータ アクセス コード (かどうか ADO.NET または SQLite.NET を使用して) このことは、事前設定済みのデータへのアクセスは完了した後に実行します。


## <a name="related-links"></a>関連リンク

- [DataAccess Basic (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [データ アクセスの詳細 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS データ レシピ](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Xamarin.Forms データ アクセス](~/xamarin-forms/app-fundamentals/databases.md)
