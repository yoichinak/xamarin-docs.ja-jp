---
title: "アプリでのデータの使用"
ms.topic: article
ms.prod: xamarin
ms.assetid: D5932AEB-0B6E-4F37-8B32-9BE4775AEE85
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 5350317c82d1073d7679843272e3215634f91217
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="using-data-in-an-app"></a>アプリでのデータの使用

**DataAccess_Adv**サンプルは、ユーザー入力と CRUD (作成、読み取り、更新、削除) データベースの機能を許可する作業アプリケーションを示しています。 アプリケーションは、2 つの画面で構成されます。 一覧と、データ エントリ フォーム。 すべてのデータ アクセス コードは変更せず、iOS および Android で再利用可能です。

アプリケーション画面は、一部のデータを追加した後、Android で次のようになります。

![Android のサンプル一覧](using-data-in-an-app-images/image11.png "Android のサンプル一覧")

![Android のサンプルの詳細](using-data-in-an-app-images/image12.png "Android のサンプルの詳細")

Android のプロジェクトが次に示す&ndash;ここに示すようにコードが含まれる、 **Orm**ディレクトリ。

![Android プロジェクト ツリー](using-data-in-an-app-images/image14.png "Android プロジェクトのツリー")

Android のアクティビティのネイティブの UI コードは、このドキュメントの対象外です。 参照してください、 [Android の Listview およびアダプター](~/android/user-interface/layouts/list-view/index.md) UI コントロールの詳細についてガイドします。

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

Android としてデータを表示する、`ListView`です。

## <a name="create-and-update"></a>作成および更新

アプリケーション コードを簡略化 save メソッドの 1 つが、Insert は提供または 更新、主キーが設定されているかどうかによって異なります。 `Id`プロパティが付いて、`[PrimaryKey]`属性のコードに設定しないでください。 このメソッドがされたかどうかの値以前 (主キーのプロパティをチェック) によって保存されるかを検出および挿入するか、それに応じてオブジェクトを更新します。

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

実際のアプリケーションは (必要なフィールド、最低限の長さやその他のビジネス ルール) などのいくつかの検証に通常必要とします。 適切なクロスプラット フォーム アプリケーションは、できるだけ多くの共有コード、プラットフォームの機能に従って表示するための UI へのバックアップの検証エラーを渡すことで可能な論理の検証の実装します。

## <a name="delete"></a>削除

異なり、`Insert`と`Update`メソッド、`Delete<T>`メソッドが完全ではなく、プライマリ キーの値だけを受け入れる`Stock`オブジェクト。 この例では、`Stock`オブジェクトがメソッドに渡されますに Id プロパティのみが渡され、`Delete<T>`メソッドです。

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>事前設定済み SQLite データベース ファイルの使用

一部のアプリケーションには、既にデータが設定されるデータベースは同梱されています。 簡単にこれを行うモバイル アプリでアプリを既存の SQLite データベース ファイルを配布してアクセスする前に書き込み可能なディレクトリにコピーすることです。 SQLite は、多くのプラットフォームで使用される標準的なファイル形式であるため、SQLite データベース ファイルの作成に使用できるツールの数があります。

-   **SQLite Manager Firefox 拡張** &ndash; iOS および Android と互換性があるファイルを Mac および Windows が生成されます。

-   **コマンド ライン**&ndash;を参照してください[www.sqlite.org/sqlite.html](http://www.sqlite.org/sqlite.html)です。

一致するどのようなコードが必要ですが、c# のクラスおよびプロパティと一致する名前を必要となる SQLite.NET を使用している場合に特にことを確認するテーブルと列の名前付けでアプリを配布用のデータベース ファイルを作成する際は注意 (または、関連付けられているカスタム属性の場合)。

Android アプリで他の何らかの前にいくつかのコードを実行するためには、ロードする最初のアクティビティに配置できますまたは作成することができます、`Application`任意のアクティビティの前に読み込まれるサブクラスです。 次のコード、`Application`既存のデータベース ファイルをコピーするサブクラス**data.sqlite**のうち、 **/ResourcesRaw/**ディレクトリ。

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
- [Android データ レシピ](https://developer.xamarin.com/recipes/android/data/)
- [Xamarin.Forms データ アクセス](~/xamarin-forms/app-fundamentals/databases.md)
