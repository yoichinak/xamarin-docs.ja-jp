---
title: Android アプリでのデータの使用
ms.prod: xamarin
ms.assetid: D5932AEB-0B6E-4F37-8B32-9BE4775AEE85
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/08/2018
ms.openlocfilehash: c0ff15c516fa2eb85ac9748004df5a14425b510c
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73023733"
---
# <a name="using-data-in-an-app"></a>アプリでのデータの使用

**DataAccess_Adv**サンプルは、データベース機能のユーザー入力と CRUD (作成、読み取り、更新、および削除) を可能にする実用的なアプリケーションを示しています。 アプリケーションは、リストとデータ入力フォームの2つの画面で構成されます。 すべてのデータアクセスコードは、iOS および Android では変更なしで再利用できます。

データを追加した後、アプリケーション画面は次のようになります。 Android では次のようになります。

![Android のサンプル一覧](using-data-in-an-app-images/image11.png "Android のサンプル一覧")

![Android のサンプルの詳細](using-data-in-an-app-images/image12.png "Android のサンプルの詳細")

このセクションに示されているコードが**Orm**ディレクトリ内に含まれる &ndash;、Android プロジェクトは次のようになります。

![Android プロジェクトツリー](using-data-in-an-app-images/image14.png "Android プロジェクトツリー")

Android でのアクティビティのネイティブ UI コードは、このドキュメントの対象外です。 UI コントロールの詳細については、 [Android ListViews And Adapters](~/android/user-interface/layouts/list-view/index.md)ガイドを参照してください。

## <a name="read"></a>読み取り

サンプルには、次のような読み取り操作がいくつかあります。

- リストを読み取っています
- 個々のレコードの読み取り

`StockDatabase` クラスの2つのメソッドは次のとおりです。

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

Android では、データを `ListView` としてレンダリングします。

## <a name="create-and-update"></a>作成と更新

アプリケーションコードを簡略化するために、PrimaryKey が設定されているかどうかに応じて挿入または更新を行う1つの save メソッドが用意されています。 `Id` プロパティは `[PrimaryKey]` 属性でマークされているので、コードで設定しないでください。 このメソッドは、(主キープロパティをチェックして) 値が以前に保存されているかどうかを検出し、それに応じてオブジェクトを挿入または更新します。

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

実際のアプリケーションでは、通常、いくつかの検証 (必須フィールド、最小長などのビジネスルール) が必要になります。 優れたクロスプラットフォームアプリケーションでは、共有コードで可能な限り多くの検証論理を実装し、検証エラーをプラットフォームの機能に従って UI に渡すことができます。

## <a name="delete"></a>削除

`Insert` メソッドと `Update` メソッドとは異なり、`Delete<T>` メソッドは、完全な `Stock` オブジェクトではなく、主キーの値のみを受け入れることができます。 この例では、`Stock` オブジェクトがメソッドに渡されますが、`Delete<T>` メソッドに渡されるのは Id プロパティだけです。

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>事前設定された SQLite データベースファイルの使用

一部のアプリケーションには、既にデータが格納されているデータベースが付属しています。 モバイルアプリケーションでこれを簡単に実現するには、既存の SQLite データベースファイルをアプリに発送し、書き込み可能なディレクトリにコピーしてからアクセスします。 SQLite は多くのプラットフォームで使用される標準のファイル形式であるため、SQLite データベースファイルを作成するために使用できるツールがいくつかあります。

- **SQLite Manager Firefox 拡張**&ndash; は、Mac と Windows で動作し、IOS および Android と互換性のあるファイルを生成します。

- **コマンドライン**&ndash; 「 [Www.sqlite.org/sqlite.html](https://www.sqlite.org/sqlite.html) 」を参照してください。

アプリケーションと共に配布するデータベースファイルを作成する場合は、テーブルと列の名前付けを使用して、コードが期待するものと一致することを確認します。特に、SQLite.NET C#を使用していて、名前がクラスやプロパティと一致することを期待する場合は特に、または、関連付けられているカスタム属性)。

Android アプリ内の他のコードよりも前に実行されるコードがあることを確認するには、最初に読み込むアクティビティにコードを配置するか、アクティビティの前に読み込まれる `Application` サブクラスを作成します。 次のコードは、既存のデータベースファイルデータをコピーする `Application` サブクラスを示して**います。 sqlite**は、 **/また**はディレクトリにあります。

```csharp
[Application]
public class YourAndroidApp : Application {
    public override void OnCreate ()
    {
        base.OnCreate ();
        var docFolder = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
        Console.WriteLine ("Data path:" + Database.DatabaseFilePath);
        var dbFile = Path.Combine(docFolder, "data.sqlite"); // FILE NAME TO USE WHEN COPIED
        if (!System.IO.File.Exists(dbFile)) {
            var s = Resources.OpenRawResource(Resource.Raw.data);  // DATA FILE RESOURCE ID
            FileStream writeStream = new FileStream(dbFile, FileMode.OpenOrCreate, FileAccess.Write);
            ReadWriteStream(s, writeStream);
        }
    }
    // readStream is the stream you need to read
    // writeStream is the stream you want to write to
    private void ReadWriteStream(Stream readStream, Stream writeStream)
    {
        int Length = 256;
        Byte[] buffer = new Byte[Length];
        int bytesRead = readStream.Read(buffer, 0, Length);
        // write the required bytes
        while (bytesRead > 0)
        {
            writeStream.Write(buffer, 0, bytesRead);
            bytesRead = readStream.Read(buffer, 0, Length);
        }
        readStream.Close();
        writeStream.Close();
    }
}
```

## <a name="related-links"></a>関連リンク

- [このような場合の基本 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [詳細設定 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android データレシピ](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin. フォームデータアクセス](~/xamarin-forms/data-cloud/data/databases.md)
