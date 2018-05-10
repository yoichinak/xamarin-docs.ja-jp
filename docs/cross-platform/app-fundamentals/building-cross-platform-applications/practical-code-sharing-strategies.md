---
title: パート 5 - 実用的なコードの戦略を共有
ms.prod: xamarin
ms.assetid: 328D042A-FF78-A7B6-1574-B5AF49A1AADB
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 234f6399a163572538755c41e4c58cf0a80e0d3e
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
---
# <a name="part-5---practical-code-sharing-strategies"></a>パート 5 - 実用的なコードの戦略を共有

このセクションでは、一般的なアプリケーション シナリオのコードを共有する方法の例を示します。



## <a name="data-layer"></a>データ層

データ層は、ストレージ エンジンと情報を読み書きするメソッドで構成されます。 パフォーマンス、柔軟性、プラットフォーム間の互換性、SQLite は、データベース エンジンは Xamarin クロス プラットフォーム アプリケーションをお勧めします。
さまざまな Windows、Android、iOS ファルダなどのプラットフォームで実行されます。


### <a name="sqlite"></a>SQLite

SQLite は、オープン ソース データベースの実装です。 ソースとのドキュメントにある[SQLite.org](http://www.sqlite.org/)です。SQLite のサポートは、各モバイル プラットフォームで入手できます。

-  **iOS** – オペレーティング システムに組み込まれています。
- **Android** – Android 2.2 (API レベル 10) 以降のオペレーティング システムに組み込まれています。
- **Windows** – を参照してください、[ユニバーサル Windows プラットフォームの拡張機能に対して SQLite](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936)です。


すべてのプラットフォームで利用可能なデータベース エンジンでも、データベースにアクセスするネイティブなメソッドが異なります。 両方の iOS と Android からにアクセスするために使用できる SQLite Xamarin.iOS、Xamarin.Android または組み込みの Api を提供して、ただしネイティブな SDK メソッドを使用してでは提供されないを (おそらく、SQL クエリ自体を文字列として格納されていると仮定した場合) 以外のコードを共有する機能. ネイティブ データベース機能の検索の詳細について`CoreData`iOS または Android の`SQLiteOpenHelper`クラスです。 このドキュメントの範囲を超えているため、これらのオプションは、プラットフォーム間ではありません。



### <a name="adonet"></a>ADO.NET

Xamarin.iOS と Xamarin.Android の両方のサポート`System.Data`と`Mono.Data.Sqlite`(、Xamarin.iOS を参照してください[ドキュメント](~/ios/data-cloud/system.data.md)の詳細)。
これらの名前空間を使用するには、両方のプラットフォームで動作する ADO.NET コードを記述することができます。 含めるプロジェクトの参照を編集`System.Data.dll`と`Mono.Data.Sqlite.dll`をコードにステートメントを使用して、これらの追加。

```csharp
using System.Data;
using Mono.Data.Sqlite;
```

その次のサンプル コードは機能します。

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "items.db3");
bool exists = File.Exists (dbPath);
if (!exists)
    SqliteConnection.CreateFile (dbPath);
var connection = new SqliteConnection ("Data Source=" + dbPath);
connection.Open ();
if (!exists) {
    // This is the first time the app has run and/or that we need the DB.
    // Copy a "template" DB from your assets, or programmatically create one like this:
    var commands = new[]{
        "CREATE TABLE [Items] (Key ntext, Value ntext);",
        "INSERT INTO [Items] ([Key], [Value]) VALUES ('sample', 'text')"
    };
    foreach (var command in commands) {
        using (var c = connection.CreateCommand ()) {
            c.CommandText = command;
            c.ExecuteNonQuery ();
        }
    }
}
// use `connection`... here, we'll just append the contents to a TextView
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT [Key], [Value] from [Items]";
    var r = contents.ExecuteReader ();
    while (r.Read ())
        Console.Write("\n\tKey={0}; Value={1}",
                r ["Key"].ToString (),
                r ["Value"].ToString ());
}
connection.Close ();
```

ADO.NET の実際の実装は、明らかにさまざまな方法および (この例はデモの目的でのみ) クラス間で分割されします。



### <a name="sqlite-net--cross-platform-orm"></a>SQLite NET – クロスプラット フォーム ORM

ORM (またはオブジェクト リレーショナル マッパー) は、クラスでモデル化されたデータの記憶域簡素化を試みます。 はなく手動でそのテーブルの作成または選択は、SQL クエリを記述、挿入および削除のデータから手動で抽出されたクラスのフィールドとプロパティを使って、ORM をするを実行するコードのレイヤーを追加します。 リフレクションを使用して、既存のクラス構造を調べて、ORM 自動的に作成できますテーブルおよび列をクラスと一致し、データを読み書きするクエリを生成します。 これにより、アプリケーション コードを単純に送信し、内部でのすべての SQL 操作を行います ORM をオブジェクト インスタンスを取得します。

SQLite NET は、保存および SQLite 内のクラスを取得することを許可する単純な ORM として機能します。 クロス プラットフォーム SQLite アクセス コンパイラ ディレクティブとその他のテクニックの組み合わせでの複雑さが非表示にします。

SQLite NET の機能:

-  テーブルは、モデルのクラスに属性を追加することによって定義されます。
-  データベース インスタンスは、のサブクラスで表される`SQLiteConnection`、SQLite net-library のメイン クラスです。
-  クエリおよび削除済みオブジェクトを使用して、データを挿入できます。 SQL ステートメントは必要ありません (ただし、必要な場合は、SQL ステートメントを記述することができますです)。
-  SQLite NET によって返されるコレクションに対して基本的な Linq クエリを実行できます。


ソース コードと SQLite NET のドキュメントは、「 [github SQLite ネット](https://github.com/praeclarum/sqlite-net)両方のケース スタディが実装されているとします。 SQLite NET コードの簡単な例 (から、 *Tasky Pro*ケース スタディ) を次に示します。

最初に、`TodoItem`クラスでは、属性を使用して、データベースのプライマリ キーとして使用するフィールドを定義します。

```csharp
public class TodoItem : IBusinessEntity
{
    public TodoItem () {}
    [PrimaryKey, AutoIncrement]
    public int ID { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
}
```

これにより、`TodoItem`コード (および SQL ステートメント) の次の行で作成するテーブル、`SQLiteConnection`インスタンス。

```csharp
CreateTable<TodoItem> ();
```

テーブル内のデータも操作できる他のメソッドでの`SQLiteConnection`(ここでも、SQL ステートメントを必要とする) なし。

```csharp
Insert (TodoItem); // 'task' is an instance with data populated in its properties
Update (TodoItem); // Primary Key field must be populated for Update to work
Table<TodoItem>.ToList(); // returns all rows in a collection
```

完全な例について、ケース スタディのソース コードを参照してください。



## <a name="file-access"></a>ファイル アクセス

ファイルへのアクセスは、すべてのアプリケーションの重要な部分を特定します。 アプリケーション include の一部になる可能性があるファイルの一般的な例:

-  SQLite データベース ファイルです。
-  ユーザーが生成したデータ (テキスト、イメージ、サウンド、ビデオ)。
-  (画像、html または PDF ファイル) をキャッシュにダウンロードされたデータ。




### <a name="systemio-direct-access"></a>System.IO の直接アクセス

Xamarin.iOS と Xamarin.Android の両方でクラスを使用してファイル システム アクセスを許可する、`System.IO`名前空間。

各プラットフォームには、考慮の対象に考慮しなければならない別のアクセス制限があります。

-  iOS アプリケーションは、非常に制限されているファイル システム アクセス権を持つサンド ボックス内で実行されます。 Apple さらには、バックアップは、特定の場所 (とはない他のユーザー) を指定して、ファイル システムを使用する方法を決定します。 参照してください、 [Xamarin.iOS 内のファイル システム操作](~/ios/app-fundamentals/file-system.md)詳細についてガイドします。
-  Android のアプリケーションに関連する特定のディレクトリにアクセスも制限しますが、もサポートします。 外部メディア (例です。 SD カード) と共有データへのアクセスします。
-  Windows Phone 8 (Silverlight) は、ファイルに直接アクセスを許可しない – を使用してファイルを操作することができますのみ`IsolatedStorage`です。
-  WinRT の Windows 8.1 および Windows 10 UWP プロジェクトのみを使用して非同期のファイル操作を提供する`Windows.Storage`Api では、他のプラットフォームとは異なります。

#### <a name="example-for-ios-and-android"></a>IOS および Android 用の例

テキスト ファイルを読み書きするための簡単な例は、以下に示します。
使用して`Environment.GetFolderPath`により同じコードが iOS および Android、それぞれが返されます、filesystem 規則に基づいて有効なディレクトリで実行します。

```csharp
string filePath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "MyFile.txt");
System.IO.File.WriteAllText (filePath, "Contents of text file");
Console.WriteLine (System.IO.ReadAllText (filePath));
```

Xamarin.iOS を参照してください[、ファイル システムで作業](~/ios/app-fundamentals/file-system.md)iOS に固有のファイル システム機能の詳細については、ドキュメントです。 クロス プラットフォームのファイル アクセス コードを記述する場合は、ファイル システムによっては、大文字と別のディレクトリの区切り記号があることに注意してください。 常にファイル名に同じ大文字と小文字を使用することをお勧めおよび`Path.Combine()`メソッド ファイルまたはディレクトリのパスを構築するときにします。



### <a name="windowsstorage-for-windows-8-and-windows-10"></a>Windows 8 および Windows 10 Windows.Storage

*Xamarin.Forms を使用したモバイル アプリを作成する* [book](https://developer.xamarin.com/r/xamarin-forms/book/)
[章 20 です。Async およびファイル I/O](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf)が含まれています[Windows 8.1 および Windows 10 用のサンプル](https://github.com/xamarin/xamarin-forms-book-preview-2/tree/master/Chapter20)です。

使用して、 [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md)これらのプラットフォームでサポートされている Api を使用してファイルを読み取ったりすることは。

```csharp
StorageFolder localFolder = ApplicationData.Current.LocalFolder;
IStorageFile storageFile = await localFolder.CreateFileAsync("MyFile.txt",
                                        CreationCollisionOption.ReplaceExisting);
await FileIO.WriteTextAsync(storageFile, "Contents of text file");
```

参照してください、[の本の章](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf)詳細についてはします。


<a name="Isolated_Storage" />

### <a name="isolated-storage-on-windows-phone-7--8-silverlight"></a>Windows Phone 7 と 8 (Silverlight) の分離ストレージ

分離ストレージは、一般的な API を保存およびすべての iOS、Android、および Windows Phone な古いプラットフォーム間でファイルを読み込んでいます。

既定のメカニズムが実装されている、Xamarin.iOS および Xamarin.Android を使用する共通ファイルへのアクセス コードを記述する Windows Phone (Silverlight) 内のファイル アクセスすることをお勧めします。 `System.IO.IsolatedStorage`で 3 つすべてのプラットフォーム間でクラスを参照することができます、[共有プロジェクト](~/cross-platform/app-fundamentals/shared-projects.md)です。

参照してください、[分離ストレージの概要の Windows Phone](http://msdn.microsoft.com/library/windowsphone/develop/ff402541(v=vs.105).aspx)詳細についてはします。

分離ストレージ Api では使用できない[ポータブル クラス ライブラリ](~/cross-platform/app-fundamentals/pcl.md)です。 PCL の 1 つの代替手段は、 [PCLStorage NuGet](https://pclstorage.codeplex.com/)



### <a name="cross-platform-file-access-in-pcls"></a>Pcl でクロスプラット フォーム ファイル アクセス

PCL と互換性のある Nuget – [PCLStorage](https://www.nuget.org/packages/PCLStorage/) – Xamarin でサポートされているプラットフォームと最新の Windows Api の施設が設備クロスプラット フォーム ファイル アクセスをします。


## <a name="network-operations"></a>ネットワーク操作

ほとんどのモバイル アプリケーションが得られますネットワー キング コンポーネントは、例を示します。

-  イメージをダウンロード、ビデオやオーディオなどです。 サムネイル、写真、音楽)。
-  (ドキュメントのダウンロード HTML、PDF)。
-  (写真やテキスト) などのユーザー データをアップロードしています。
-  Web サービスまたはサード パーティの Api (などの SOAP、XML または JSON) へのアクセス。


.NET Framework では、ネットワーク リソースにアクセスするため、いくつかの異なるクラスが用意されています: `HttpClient`、 `WebClient`、および`HttpWebRequest`です。

### <a name="httpclient"></a>HttpClient

`HttpClient`クラス内で、`System.Net.Http`名前空間は、Xamarin.iOS、Xamarin.Android、およびほとんどの Windows プラットフォームで使用できます。 [Microsoft HTTP Client Library Nuget](https://www.nuget.org/packages/Microsoft.Net.Http/)ポータブル クラス ライブラリ (および Windows Phone 8 Silverlight) をこの API をさせるために使用できます。

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get, "https://xamarin.com");
var response = await myClient.SendAsync(request);
```

### <a name="webclient"></a>WebClient

`WebClient`クラスには、リモート サーバーからリモート データを取得する単純な API が用意されています。

ユニバーサル Windows Platofrm operations*必要があります*async をする場合でも、Xamarin.iOS および Xamarin.Android (をバック グラウンド スレッドで実行できます)。 同期の操作をサポートします。

単純な非同期のコード`WebClient`操作します。

```csharp
var webClient = new WebClient ();
webClient.DownloadStringCompleted += (sender, e) =>
{
    var resultString = e.Result;
    // do something with downloaded string, do UI interaction on main thread
};
webClient.Encoding = System.Text.Encoding.UTF8;
webClient.DownloadStringAsync (new Uri ("http://some-server.com/file.xml"));
```

 `WebClient` `DownloadFileCompleted`と`DownloadFileAsync`バイナリ データを取得するためです。

<a name="HttpWebRequest" />

### <a name="httpwebrequest"></a>HttpWebRequest

`HttpWebRequest` 多くのカスタマイズを提供`WebClient`その結果を使用するコードを増やす必要があります。

シンプルな同期コード`HttpWebRequest`操作します。

```csharp
var request = HttpWebRequest.Create(@"http://some-server.com/file.xml ");
request.ContentType = "text/xml";
request.Method = "GET";
using (HttpWebResponse response = request.GetResponse() as HttpWebResponse)
{
    if (response.StatusCode != HttpStatusCode.OK)
        Console.WriteLine("Error fetching data. Server returned status code: {0}", response.StatusCode);
    using (StreamReader reader = new StreamReader(response.GetResponseStream()))
    {
        var content = reader.ReadToEnd();
        // do something with downloaded string, do UI interaction on main thread
    }
}
```

例がある、 [Web Services のドキュメント](~/cross-platform/data-cloud/web-services/index.md)です。

 <a name="Reachability" />


### <a name="reachability"></a>到達可能性情

モバイル デバイスは、さまざまな高速 Wi-fi からネットワーク条件または劣悪な受信領域と低速のエッジ データ リンクの 4 G 接続で動作します。 このためは、ネットワークが利用できるかどうかと、かどうかは、ネットワークの種類が使用可能なリモート サーバーに接続する前に検出することをお勧めします。

このような状況でモバイル アプリがかかる場合がありますアクションは次のとおりです。

-  ネットワークが使用できない場合、ユーザーに通知します。 手動で無効になっていること (場合 機内モードまたは Wi-fi の電源を切る) し、問題を解決することができます。
-  接続が 3 G の場合は、アプリケーションの動作が異なります (たとえば、Apple を許可しないアプリで 3 G をダウンロードする 20 Mb より大きい)。 アプリケーションがこの情報を使用して過度のダウンロードについてユーザーに警告する大きなファイルを取得するときにタイムアウトします。
-  ネットワークが利用可能な場合でもは、他の要求を開始する前に対象サーバーとの接続を確認することをお勧めします。 アプリのネットワーク操作がタイムアウトを繰り返し防止されも、ユーザーに表示するのにもっとわかりやすいエラー メッセージを許可します。


[Xamarin.iOS サンプル](https://github.com/xamarin/monotouch-samples/tree/master/ReachabilitySample)使用可能な (これは Apple に基づいて[到達可能性情サンプル コード](http://developer.apple.com/library/ios/#samplecode/Reachability/Introduction/Intro.html)) ネットワークの可用性を検出するためにします。


## <a name="webservices"></a>WebServices

このドキュメントを参照して[Web サービスを使用](~/cross-platform/data-cloud/web-services/index.md)をカバーする、残りの部分にアクセスする Xamarin.iOS を使用して SOAP および WCF のエンドポイント。 作れる web サービス要求することし、ただし、これが簡単、Azure、RestSharp、ServiceStack などに使用可能なライブラリ、応答を解析します。 基本的な WCF 操作は、Xamarin アプリにアクセスできます。

### <a name="azure"></a>Azure

Microsoft Azure は、さまざまなデータ ストレージとの同期、プッシュ通知など、モバイル アプリのサービスを提供するクラウド プラットフォームです。

参照してください[azure.microsoft.com](https://azure.microsoft.com/)無料してみてください。

### <a name="restsharp"></a>RestSharp

RestSharp は、REST クライアントを簡単に web サービスへのアクセスを提供するモバイル アプリケーションに含めることが .NET ライブラリです。 データを要求して、残りの部分応答を解析する単純な API を提供することで助けになります。 RestSharp が役に立ちます

[RestSharp web サイト](http://restsharp.org/)が含まれています[ドキュメント](https://github.com/restsharp/RestSharp/wiki)RestSharp を使用して、REST クライアントの実装方法です。
RestSharp については、Xamarin.iOS と Xamarin.Android 例[github](https://github.com/restsharp/RestSharp/)です。

Xamarin.iOS コード スニペットではまた、 [Web Services のドキュメント](~/cross-platform/data-cloud/web-services/index.md)です。

 <a name="ServiceStack" />


### <a name="servicestack"></a>ServiceStack

ServiceStack とは異なり RestSharp、それらのサービスにアクセスするモバイル アプリケーションで実装できるクライアント ライブラリと同様に、web サービスをホストする、両方のサーバー側ソリューション。

[ServiceStack web サイト](http://servicestack.net/)ドキュメントとサンプル コードをプロジェクトとリンクの目的を説明します。 例には、アクセスできるさまざまなクライアント側のアプリケーションと同様に、web サービスの完全なサーバー側実装が含まれます。

[Xamarin.iOS 例](http://www.servicestack.net/monotouch/remote-info/)ServiceStack web サイトとのコード スニペットに、 [Web Services のドキュメント](~/cross-platform/data-cloud/web-services/index.md)です。


### <a name="wcf"></a>WCF

Xamarin ツールでは、いくつかの Windows Communication Foundation (WCF) サービスを利用できます。 一般に、Xamarin では、Silverlight ランタイムに付属する WCF の同じクライアント側のサブセットをサポートしています。 これには、WCF の最も一般的なエンコード、およびプロトコルの実装が含まれます: HTTP 経由の SOAP メッセージのテキストでエンコードされたトランスポート プロトコルを使用して、`BasicHttpBinding`です。

サイズと、WCF フレームワークの複雑さ、原因は、Xamarin のクライアントのサブセットのドメインでサポートされているスコープの外に分類されます現在および将来のサービスの実装が可能性があります。 さらに、WCF のサポートには、プロキシを生成するための Windows 環境でのみ使用できるツールの使用が必要です。

 <a name="Threading" />


## <a name="threading"></a>スレッド

アプリケーションの応答性は、モバイル アプリケーションにとって重要: ユーザーの期待を読み込んでをすばやく実行するアプリケーション。 ネットワーク要求などの実行時間の長いブロッキング呼び出しや低速のローカル操作 (ファイルを解凍) などの UI スレッドを占有しない重要なアプリケーションがクラッシュを示すために表示されるユーザー入力の受け入れを停止 '固定' 画面。 具体的には、スタートアップ プロセスは実行時間の長いタスクを含めることはできません-すべてのモバイル プラットフォームが読み込みに時間がかかるとするアプリを強制終了します。

つまり、バック グラウンド操作を実行するには、進行状況インジケーターまたはクイックを表示するには '有効' それ以外の場合の UI と非同期タスク、ユーザー インターフェイスを実装する必要があります。 バック グラウンド タスクを実行するには、進行状況を示すために、メイン スレッドに通知するためにバック グラウンド タスクのニーズを意味するか、完了しているときに、スレッドの使用が必要です。

 <a name="Parallel_Task_Library" />


### <a name="parallel-task-library"></a>タスク並列ライブラリ

タスク並列ライブラリで作成したタスクは、非同期に実行され、ユーザー インターフェイスをブロックすることがなく長時間実行処理をトリガーするに非常に役立つように、呼び出し元のスレッドで返すことができます。

単純な並列タスク操作は、次のようになります。

```csharp
using System.Threading.Tasks;
void MainThreadMethod ()
{
    Task.Factory.StartNew (() => wc.DownloadString ("http://...")).ContinueWith (
        t => label.Text = t.Result, TaskScheduler.FromCurrentSynchronizationContext()
    );
}
```

キーが`TaskScheduler.FromCurrentSynchronizationContext()`メソッドを呼び出すスレッドの SynchronizationContext.Current が再利用されます (ここでは、メイン スレッドを実行している`MainThreadMethod`) そのスレッドに戻る呼び出しをマーシャ リングする手段として。 つまり、UI スレッドでメソッドを呼び出した場合は実行、 `ContinueWith` UI スレッドに戻す操作します。

コードは、他のスレッドからタスクを開始するが、UI スレッドへの参照を作成する、次のパターンを使用し、タスクできますもコールバックに。

```csharp
static Context uiContext = TaskScheduler.FromCurrentSynchronizationContext();
```

 <a name="Invoking_on_the_UI_Thread" />


### <a name="invoking-on-the-ui-thread"></a>UI スレッドでの呼び出し

タスク並列ライブラリを使用しないコードの場合は、各プラットフォームには、UI スレッドにマーシャ リングの操作のための独自の構文。

-  **iOS** – `owner.BeginInvokeOnMainThread(new NSAction(action))`
-  **Android** – `owner.RunOnUiThread(action)`
-  **Xamarin.Forms** – `Device.BeginInvokeOnMainThread(action)`
-  **Windows** – `Deployment.Current.Dispatcher.BeginInvoke(action)`



IOS と Android の構文の両方には、使用する、コードをこのオブジェクトを UI スレッドでコールバックを必要とする任意のメソッドに渡す必要があることを意味する 'context' クラスが必要です。

UI スレッドの呼び出しをする共有コードで、次の[IDispatchOnUIThread 例](http://www.slideshare.net/follesoe/cross-platform-mobile-apps-using-net)(厚意の[ @follesoe ](http://jonas.follesoe.no/))。 宣言してプログラムを`IDispatchOnUIThread`インターフェイスは、共有コードでし、次のように、プラットフォーム固有のクラスを実装します。

```csharp
// program to the interface in shared code
public interface IDispatchOnUIThread {
    void Invoke (Action action);
}
// iOS
public class DispatchAdapter : IDispatchOnUIThread {
    public readonly NSObject owner;
    public DispatchAdapter (NSObject owner) {
        this.owner = owner;
    }
    public void Invoke (Action action) {
        owner.BeginInvokeOnMainThread(new NSAction(action));
    }
}
// Android
public class DispatchAdapter : IDispatchOnUIThread {
    public readonly Activity owner;
    public DispatchAdapter (Activity owner) {
        this.owner = owner;
    }
    public void Invoke (Action action) {
        owner.RunOnUiThread(action);
    }
}
// WP7
public class DispatchAdapter : IDispatchOnUIThread {
    public void Invoke (Action action) {
        Deployment.Current.Dispatcher.BeginInvoke(action);
    }
}
```

Xamarin.Forms を使用してください[ `Device.BeginInvokeOnMainThread` ](~/xamarin-forms/platform/device.md#Device_BeginInvokeOnMainThread)一般的なコード (共有プロジェクトまたは PCL) にします。

 <a name="Platform_and_Device_Capabilities_and_Degradation" />


## <a name="platform-and-device-capabilities-and-degradation"></a>プラットフォームとデバイスの機能との低下

さらにさまざまな機能を処理する場合の具体的な例については、プラットフォームの機能のドキュメントで表されます。 さまざまな機能と能力を最大限にアプリを操作できない場合でも、優れたユーザー エクスペリエンスを提供するアプリケーションを正常に低下する方法を検出することについて説明します。
