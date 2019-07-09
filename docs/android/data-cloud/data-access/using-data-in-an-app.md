---
title: Android アプリでデータを使用
ms.prod: xamarin
ms.assetid: D5932AEB-0B6E-4F37-8B32-9BE4775AEE85
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/08/2018
ms.openlocfilehash: 7d402e6f665baa8db68d571945490a8d1ae18881
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/08/2019
ms.locfileid: "67649554"
---
# <a name="using-data-in-an-app"></a>アプリでのデータの使用

**DataAccess_Adv**サンプルにより、ユーザー入力とデータベース機能の CRUD (作成、読み取り、更新、削除) する作業アプリケーションを示しています。 アプリケーションは、2 つの画面で構成されています。 一覧と、データ エントリ フォーム。 すべてのデータ アクセス コードは変更しなくても iOS と Android で再利用できます。

一部のデータを追加した後アプリケーション画面は Android で次のようになります。

![Android のサンプル一覧](using-data-in-an-app-images/image11.png "Android のサンプル一覧")

![Android のサンプルの詳細](using-data-in-an-app-images/image12.png "Android のサンプルの詳細")

Android プロジェクトが次に示す&ndash;内でこのセクションで示すコードが含まれている、 **Orm**ディレクトリ。

![プロジェクトを android ツリー](using-data-in-an-app-images/image14.png "プロジェクトが Android ツリー")

Android でのアクティビティのネイティブの UI コードは、このドキュメントの対象外です。 参照してください、 [Android Listview と Adapter](~/android/user-interface/layouts/list-view/index.md) UI コントロールの詳細についてはガイド。

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

Android としてデータを表示する、`ListView`します。

## <a name="create-and-update"></a>作成し、更新

アプリケーション コードを簡素化するには、1 つの save メソッドが挿入は提供または 更新の主キーが設定されているかどうかによって異なります。 `Id`プロパティが付いて、`[PrimaryKey]`属性は、コードで設定しないでください。 このメソッドが検出されたかどうか、値が前 (主キー プロパティをチェック) を保存し、挿入するか、それに応じてオブジェクトを更新、します。

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

実践的なアプリケーションは (必要なフィールド、最低限の長さやその他のビジネス ルール) などのいくつかの検証に通常必要とします。 優れたクロスプラット フォーム対応のアプリケーションは、共有コード、プラットフォームの機能に従って表示するための UI へのバックアップの検証エラーを渡すことで可能な論理の検証の実装します。

## <a name="delete"></a>削除

異なり、`Insert`と`Update`、メソッド、`Delete<T>`メソッドが完全ではなく主キーの値だけを受け入れる`Stock`オブジェクト。 この例では、`Stock`オブジェクトは、メソッドに渡されますに Id プロパティのみが渡され、`Delete<T>`メソッド。

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>事前設定された SQLite データベース ファイルを使用します。

一部のアプリケーションには、データに読み込まれているデータベースは同梱されています。 簡単にこれを行うモバイル アプリケーションで、アプリで既存の SQLite データベース ファイルを配布して書き込み可能なディレクトリにアクセスする前にコピーすることです。 SQLite は、多くのプラットフォームで使用される標準的なファイル形式であるためには、さまざまな SQLite データベース ファイルの作成に使用できるツールがあります。

-   **Firefox 拡張機能の SQLite Manager** &ndash; iOS および Android と互換性があるファイルを Mac と Windows が生成されます。

-   **コマンド ライン**&ndash;を参照してください[www.sqlite.org/sqlite.html](http://www.sqlite.org/sqlite.html)します。

アプリを配布用のデータベース ファイルを作成するときと確実に、コードでは、c# のクラスおよびプロパティに一致する名前を必要とする SQLite.NET を使用している場合に特にテーブルと列の名前付けを使用処理 (または、関連付けられているカスタム属性の場合)。

いくつかのコードは、前に、Android アプリで何が実行されることを確認するには、読み込むには、最初のアクティビティに配置することができますまたは作成することができます、`Application`任意のアクティビティの前に読み込まれるサブクラスです。 次のコードを`Application`既存のデータベース ファイルをコピーするサブクラス**data.sqlite**のうち、 **/ResourcesRaw/** ディレクトリ。

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

- [DataAccess Basic (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [データ アクセスの詳細 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android のデータのレシピ](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin.Forms のデータ アクセス](~/xamarin-forms/data-cloud/data/databases.md)
