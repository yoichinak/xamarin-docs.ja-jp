---
title: IOS アプリでのデータの使用
description: このドキュメントでは、DataAccess_Adv サンプルについて説明します。このサンプルでは、Xamarin iOS アプリでユーザー入力を収集し、作成、読み取り、更新、削除 (CRUD) データベース操作を実行する方法を示します。
ms.prod: xamarin
ms.assetid: 2CB8150E-CD2C-4E97-8605-1EE8CBACFEEC
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 10/11/2016
ms.openlocfilehash: c888c132748c4212b1e52413647614ca83897d75
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938516"
---
# <a name="using-data-in-an-ios-app"></a>IOS アプリでのデータの使用

**DataAccess_Adv**サンプルでは、データベース機能のユーザー入力と*CRUD* (作成、読み取り、更新、および削除) を可能にする実用的なアプリケーションを示します。 アプリケーションは、リストとデータ入力フォームの2つの画面で構成されます。 すべてのデータアクセスコードは、iOS および Android では変更なしで再利用できます。

データを追加した後、アプリケーション画面は次のようになります。

 ![iOS のサンプル一覧](using-data-in-an-app-images/image9.png)

 ![iOS のサンプルの詳細](using-data-in-an-app-images/image10.png)

IOS プロジェクトを以下に示します。このセクションで示すコードは、 **Orm**ディレクトリに含まれています。

 ![iOS プロジェクトツリー](using-data-in-an-app-images/image13.png)

IOS の ViewControllers のネイティブ UI コードは、このドキュメントの対象外です。
UI コントロールの詳細については、「 [iOS のテーブルとセルの操作](~/ios/user-interface/controls/tables/index.md)ガイド」を参照してください。

## <a name="read"></a>Read

サンプルには、次のような読み取り操作がいくつかあります。

- リストを読み取っています
- 個々のレコードの読み取り

クラスの2つのメソッド `StockDatabase` は次のとおりです。

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

iOS では、としてデータが異なる方法で表示され `UITableView` ます。

## <a name="create-and-update"></a>作成と更新

アプリケーションコードを簡略化するために、PrimaryKey が設定されているかどうかに応じて挿入または更新を行う1つの save メソッドが用意されています。 プロパティは属性でマークされているので、 `Id` `[PrimaryKey]` コードで設定しないでください。
このメソッドは、(主キープロパティをチェックして) 値が以前に保存されているかどうかを検出し、それに応じてオブジェクトを挿入または更新します。

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

実際のアプリケーションでは、通常、いくつかの検証 (必須フィールド、最小長などのビジネスルール) が必要になります。
優れたクロスプラットフォームアプリケーションでは、共有コードで可能な限り多くの検証論理を実装し、検証エラーをプラットフォームの機能に従って UI に渡すことができます。

## <a name="delete"></a>削除

`Insert`メソッドとメソッドとは異なり `Update` 、メソッドは、 `Delete<T>` 完全なオブジェクトではなく、主キーの値のみを受け入れることができ `Stock` ます。
この例では、 `Stock` オブジェクトはメソッドに渡されますが、Id プロパティのみがメソッドに渡され `Delete<T>` ます。

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>事前設定された SQLite データベースファイルの使用

一部のアプリケーションには、既にデータが格納されているデータベースが付属しています。
モバイルアプリケーションでこれを簡単に実現するには、既存の SQLite データベースファイルをアプリに発送し、書き込み可能なディレクトリにコピーしてからアクセスします。 SQLite は多くのプラットフォームで使用される標準のファイル形式であるため、SQLite データベースファイルを作成するために使用できるツールがいくつかあります。

- **SQLite Manager Firefox Extension** – Mac および Windows で動作し、IOS および Android と互換性のあるファイルを生成します。
- **コマンドライン**–「 [Www.sqlite.org/sqlite.html](https://www.sqlite.org/sqlite.html) 」を参照してください。

アプリケーションと共に配布するデータベースファイルを作成する場合は、テーブルと列の名前付けを使用して、コードが期待するものと一致することを確認します。特に、C# のクラスとプロパティ (または関連するカスタム属性) と名前が一致することを期待する SQLite.NET を使用する場合は特にそうです。

IOS の場合は、アプリケーションに sqlite ファイルを含め、**ビルドアクション: コンテンツ**でマークされていることを確認します。 `FinishedLaunching`データメソッドを呼び出す*前に*、ファイルを書き込み可能なディレクトリにコピーするコードをに配置します。 次のコードでは、 **data. sqlite**という名前の既存のデータベースがまだ存在しない場合にのみコピーします。

```csharp
// Copy the database across (if it doesn't exist)
var appdir = NSBundle.MainBundle.ResourcePath;
var seedFile = Path.Combine (appdir, "data.sqlite");
if (!File.Exists (Database.DatabaseFilePath))
{
  File.Copy (seedFile, Database.DatabaseFilePath);
}
```

この完了後に実行されるデータアクセスコード (ADO.NET または using SQLite.NET) は、事前に設定されたデータにアクセスできます。

## <a name="related-links"></a>関連リンク

- [このような場合の基本 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [詳細設定 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS データレシピ](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin. フォームデータアクセス](~/xamarin-forms/data-cloud/data/databases.md)
