---
title: パート 5 - 実践的なコード共有戦略
description: このドキュメントでは、実践的なコードを共有データベース、ファイルへのアクセス、ネットワークの操作および非同期コードのようなシナリオの戦略について説明します。
ms.prod: xamarin
ms.assetid: 328D042A-FF78-A7B6-1574-B5AF49A1AADB
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 0e37e138607fb0e00fbdc463ac7c53facf81395d
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76723631"
---
# <a name="part-5---practical-code-sharing-strategies"></a>パート 5 - 実践的なコード共有戦略

このセクションでは、一般的なアプリケーション シナリオのコードを共有する方法の例を示します。

## <a name="data-layer"></a>データ層

データ層は、ストレージ エンジンと情報を読み書きするメソッドで構成されます。 パフォーマンス、柔軟性、クロス プラットフォームの互換性、SQLite データベース エンジンは Xamarin クロスプラット フォーム対応のアプリケーションをお勧めします。
さまざまな Windows、Android、iOS、およびファルダなどのプラットフォームで実行されます。

### <a name="sqlite"></a>SQLite

SQLite とは、オープン ソース データベースの実装です。 ソースとドキュメントについては、 [SQLite.org](https://www.sqlite.org/)を参照してください。SQLite のサポートは、各モバイルプラットフォームで利用できます。

- **iOS** –オペレーティングシステムに組み込まれています。
- **Android – android** 2.2 (API レベル 10) 以降、オペレーティングシステムに組み込まれています。
- **Windows** –[ユニバーサル Windows プラットフォーム拡張機能の SQLite を](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936)参照してください。

すべてのプラットフォームで利用可能なデータベース エンジンでも、データベースにアクセスするネイティブなメソッドが異なります。 両方の iOS と Android からにアクセスするために使用できる SQLite Xamarin.iOS または Xamarin.Android の組み込みの API を提供して、ただし、ネイティブ SDK メソッドを使用して (おそらく、SQL クエリ自体を文字列として格納されていると仮定した場合) 以外のコードを共有する機能はありません. ネイティブデータベース機能の詳細については、iOS または Android の `SQLiteOpenHelper` クラスの `CoreData` を検索してください。これらのオプションはクロスプラットフォームではないため、このドキュメントの範囲を超えています。

### <a name="adonet"></a>ADO.NET

Xamarin と Xamarin Android はどちらも `System.Data` と `Mono.Data.Sqlite` をサポートしています (詳細については、Xamarin iOS の[ドキュメント](~/ios/data-cloud/system.data.md)を参照してください)。
これらの名前空間を使用して両方のプラットフォームで動作する ADO.NET コードを記述することができます。 プロジェクトの参照を編集して `System.Data.dll` と `Mono.Data.Sqlite.dll` を含め、次の using ステートメントをコードに追加します。

```csharp
using System.Data;
using Mono.Data.Sqlite;
```

次のサンプル コードを使用します。

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

ADO.NET の実際の実装は、さまざまなメソッドおよびクラス (この例では、デモンストレーションのみを目的の) の間で明らかに分割します。

### <a name="sqlite-net--cross-platform-orm"></a>SQLite NET – クロスプラット フォーム対応の ORM

ORM (またはオブジェクト リレーショナル マッパー) は、クラスでモデル化されたデータのストレージを簡略化を試行します。 はなく手動でそのテーブルの作成または選択は、SQL クエリを記述、INSERT および DELETE のデータから手動で抽出されたクラスのフィールドとプロパティ、ORM の処理を行うコードのレイヤーを追加します。 リフレクションを使用して、クラスの構造を調べて、ORM 自動的に作成できますテーブルおよび列をクラスと一致し、データを読み書きするクエリを生成します。 これにより、アプリケーション コードを単純に送信し、内部的には、すべての SQL 操作を処理すると、ORM をオブジェクト インスタンスを取得できます。

SQLite NET は、保存および SQLite 内のクラスを取得することを許可する単純な ORM として機能します。 クロス コンパイラ ディレクティブおよびその他のトリックを組み合わせたプラットフォーム SQLite アクセスの複雑さが非表示にします。

SQLite NET の機能:

- テーブルは、モデル クラスに属性を追加して定義されます。
- データベースインスタンスは、SQLite-Net ライブラリのメインクラスである `SQLiteConnection` のサブクラスによって表されます。
- クエリおよびオブジェクトを使用して削除された、データを挿入できます。 SQL ステートメントは必要ありません (ただし、必要な場合は、SQL ステートメントを記述することができますです)。
- SQLite NET によって返されるコレクションでは、基本的な Linq クエリを実行できます。

SQLite-NET のソースコードとドキュメントは、 [github の sqlite-net](https://github.com/praeclarum/sqlite-net)で入手でき、両方のケーススタディで実装されています。 ( *Tasky Pro*ケーススタディからの) SQLite NET コードの簡単な例を次に示します。

まず、`TodoItem` クラスは属性を使用して、データベースの主キーとしてフィールドを定義します。

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

これにより、`SQLiteConnection` インスタンスで次のコード行 (および SQL ステートメントなし) を使用して `TodoItem` テーブルを作成できます。

```csharp
CreateTable<TodoItem> ();
```

テーブル内のデータは、`SQLiteConnection` の他のメソッドで操作することもできます (この場合も、SQL ステートメントを必要としません)。

```csharp
Insert (TodoItem); // 'task' is an instance with data populated in its properties
Update (TodoItem); // Primary Key field must be populated for Update to work
Table<TodoItem>.ToList(); // returns all rows in a collection
```

完全な例のケース スタディのソース コードを参照してください。

## <a name="file-access"></a>ファイル アクセス

ファイルへのアクセスは、任意のアプリケーションの重要な部分を特定します。 アプリケーションのインクルードの一部である可能性がありますファイルの一般的な例:

- SQLite データベース ファイルです。
- ユーザーが生成したデータ (テキスト、画像、サウンド、ビデオ)。
- (イメージ、html や PDF ファイル) をキャッシュにダウンロードされたデータ。

### <a name="systemio-direct-access"></a>System.IO の直接アクセス

Xamarin と Xamarin の両方で、`System.IO` 名前空間のクラスを使用してファイルシステムへのアクセスを許可します。

各プラットフォームには、考慮事項に考慮しなければならない別のアクセス制限があります。

- iOS アプリケーションは、非常に制限されているファイル システム アクセス権を持つサンド ボックスで実行します。 さらに Apple では、バックアップを特定の場所 (およびその他のない) を指定することで、ファイル システムを使用する方法を決定します。 詳細については、「 [Xamarin. iOS でのファイルシステム](~/ios/app-fundamentals/file-system.md)の使用」ガイドを参照してください。
- Android は、アプリケーションに関連する特定のディレクトリにアクセスを制限することもが、外部メディア (例。 SD カード) および共有データへのアクセスします。
- Windows Phone 8 (Silverlight) では、ファイルへの直接アクセスは許可されません。ファイルを操作できるのは `IsolatedStorage`を使用した場合のみです。
- Windows 8.1 WinRT プロジェクトと Windows 10 UWP プロジェクトは、他のプラットフォームとは異なる `Windows.Storage` Api による非同期ファイル操作のみを提供します。

#### <a name="example-for-ios-and-android"></a>IOS および Android 用の例

テキスト ファイルを読み書きするための簡単な例は、以下に示します。
`Environment.GetFolderPath` を使用すると、同じコードを iOS と Android で実行できるため、それぞれがファイルシステムの規則に基づいて有効なディレクトリを返します。

```csharp
string filePath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "MyFile.txt");
System.IO.File.WriteAllText (filePath, "Contents of text file");
Console.WriteLine (System.IO.File.ReadAllText (filePath));
```

IOS 固有のファイルシステムの機能の詳細については、「Xamarin. iOS の[ファイルシステム](~/ios/app-fundamentals/file-system.md)ドキュメントの操作」を参照してください。 クロスプラット フォームでファイル アクセス コードを記述する場合は、一部のファイル システムが大文字小文字を区別し、別のディレクトリの区切り記号があることに注意してください。 ファイルまたはディレクトリのパスを構築するときは、必ず同じ大文字小文字を使用し、ファイル名と `Path.Combine()` メソッドを使用することをお勧めします。

### <a name="windowsstorage-for-windows-8-and-windows-10"></a>Windows 8 および Windows 10 の Windows.Storage

「 *Xamarin. Forms book を使用した Mobile Apps の作成* [book](https://developer.xamarin.com/r/xamarin-forms/book/) 」
第20章を参照して[ください。非同期 i/o とファイル i/o](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf)には[、Windows 8.1 と Windows 10 のサンプル](https://github.com/xamarin/xamarin-forms-book-preview-2/tree/master/Chapter20)が含まれています。

[`DependencyService`](~/xamarin-forms/app-fundamentals/dependency-service/index.md)を使用すると、次のサポートされている api を使用して、これらのプラットフォーム上のファイルを読み取ってファイルを読み込むことができます。

```csharp
StorageFolder localFolder = ApplicationData.Current.LocalFolder;
IStorageFile storageFile = await localFolder.CreateFileAsync("MyFile.txt",
                                        CreationCollisionOption.ReplaceExisting);
await FileIO.WriteTextAsync(storageFile, "Contents of text file");
```

詳細については、[書籍の章](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf)を参照してください。

<a name="Isolated_Storage" />

### <a name="isolated-storage-on-windows-phone-7--8-silverlight"></a>Windows Phone 7 および 8 (Silverlight) の分離ストレージ

分離ストレージは、保存して、すべての iOS、Android、および以前の Windows Phone プラットフォーム間でファイルの読み込みを共通の API です。

Xamarin.iOS および Xamarin.Android ファイル アクセスの一般的なコードの許可に書き込まれるで実装されている Windows Phone (Silverlight) におけるファイル アクセスの既定のメカニズムです。 `System.IO.IsolatedStorage` クラスは、[共有プロジェクト](~/cross-platform/app-fundamentals/shared-projects.md)内の3つのすべてのプラットフォームで参照できます。

詳細については、 [Windows Phone の分離ストレージの概要](https://msdn.microsoft.com/library/windowsphone/develop/ff402541(v=vs.105).aspx)に関するトピックを参照してください。

分離ストレージ Api は、[ポータブルクラスライブラリ](~/cross-platform/app-fundamentals/pcl.md)では使用できません。 PCL の代替手段として[Pclstorage NuGet](https://pclstorage.codeplex.com/)を使用することもあります。

### <a name="cross-platform-file-access-in-pcls"></a>Pcl でクロスプラット フォームでファイル アクセス

また、PCL 互換の NuGet – [Pclstorage](https://www.nuget.org/packages/PCLStorage/)があります。これにより、Xamarin でサポートされているプラットフォームと最新の Windows api のクロスプラットフォームファイルアクセスが可能になります。

## <a name="network-operations"></a>ネットワーク運用担当者

ほとんどのモバイル アプリケーションがネットワーク コンポーネントは、たとえば。

- イメージをダウンロード、ビデオおよびオーディオ (例。 サムネイル、写真、音楽)。
- (例: ドキュメントのダウンロード HTML、PDF)。
- (写真やテキスト) などのユーザー データをアップロードしています。
- Web サービスまたはサード パーティ (などの SOAP、XML または JSON) の Api にアクセスします。

.NET Framework には、`HttpClient`、`WebClient`、および `HttpWebRequest`のネットワークリソースにアクセスするためのいくつかの異なるクラスが用意されています。

### <a name="httpclient"></a>HttpClient

`System.Net.Http` 名前空間の `HttpClient` クラスは、Xamarin、Xamarin、Android、およびほとんどの Windows プラットフォームで使用できます。 この API をポータブルクラスライブラリ (および Windows Phone 8 Silverlight) に取り込むために使用できる[MICROSOFT HTTP クライアントライブラリ NuGet](https://www.nuget.org/packages/Microsoft.Net.Http/)が用意されています。

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get, "https://xamarin.com");
var response = await myClient.SendAsync(request);
```

### <a name="webclient"></a>WebClient

`WebClient` クラスには、リモートサーバーからリモートデータを取得するための単純な API が用意されています。

ユニバーサル Windows プラットフォーム操作は非同期である*必要がありますが*、Xamarin と xamarin Android では同期操作 (バックグラウンドスレッドで実行可能) がサポートされています。

単純な非同期 `WebClient` 操作のコードは次のとおりです。

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

 `WebClient` には、バイナリデータを取得するための `DownloadFileCompleted` と `DownloadFileAsync` もあります。

<a name="HttpWebRequest" />

### <a name="httpwebrequest"></a>HttpWebRequest

`HttpWebRequest` は、`WebClient` よりも多くのカスタマイズを提供します。結果として、より多くのコードを使用する必要があります。

単純な同期 `HttpWebRequest` 操作のコードは次のとおりです。

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

[Web サービスのドキュメント](~/cross-platform/data-cloud/web-services/index.md)には、例があります。

 <a name="Reachability" />

### <a name="reachability"></a>経由で到達可能

モバイル デバイスは、さまざまなから高速の Wi-fi ネットワークの状態または不適切な受付および EDGE データへの低速リンクへの 4 G 接続の下で動作します。 このためは、ネットワークが利用可能かどうかと、そのため、ネットワークの種類があるか、リモート サーバーに接続する前に検出することをお勧めします。

このような状況でモバイル アプリがかかる場合がありますアクションは次のとおりです。

- ネットワークが利用できない場合は、ユーザーにお勧めします。 手動で無効になっていること (例: 場合 機内モードまたは Wi-fi をオフにする) し、問題を解決することができます。
- 接続が 3 G の場合は、アプリケーションの動作が異なります (たとえば、Apple を許可しないアプリ 3 G を超えるダウンロードを 20 Mb より大きい)。 アプリケーションがこの情報を使用して過度のダウンロードについてユーザーに警告する大きなファイルを取得するときにタイムアウトします。
- ネットワークを使用できる場合でも、他の要求を開始する前に、ターゲット サーバーとの接続を確認することをお勧めを勧めします。 アプリのネットワーク操作がタイムアウトを繰り返し防止されより多くの情報のエラー メッセージ、ユーザーに表示することもできます。

## <a name="webservices"></a>WebServices

Xamarin を使用した REST、SOAP、WCF エンドポイントへのアクセスについては、 [Web サービスの](~/cross-platform/data-cloud/web-services/index.md)使用に関するドキュメントを参照してください。 Web サービスの要求を自ら作成することができますしがはるかに簡単、Azure、RestSharp、ServiceStack などに使用可能なライブラリがあります、応答を解析します。 基本的な WCF 操作は、Xamarin アプリでアクセスできます。

### <a name="azure"></a>Azure

Microsoft Azure とは、さまざまなデータ ストレージとの同期、プッシュ通知など、モバイル アプリのサービスを提供するクラウド プラットフォームです。

[Azure.microsoft.com](https://azure.microsoft.com/)にアクセスして無料で試すことができます。

### <a name="restsharp"></a>RestSharp

RestSharp は、web サービスへのアクセスを簡素化するための REST クライアントを提供するモバイル アプリケーションに含めることができる .NET ライブラリです。 データを要求して、REST 応答を解析する単純な API を提供することができます。 RestSharp が役に立ちます

[RestSharp web サイト](http://restsharp.org/)には、RestSharp を使用した REST クライアントの実装方法に関する[ドキュメント](https://github.com/restsharp/RestSharp/wiki)が含まれています。
RestSharp には、 [github](https://github.com/restsharp/RestSharp/)のサンプルが用意されています。

[Web サービスのドキュメント](~/cross-platform/data-cloud/web-services/index.md)には、Xamarin の iOS コードスニペットもあります。

 <a name="ServiceStack" />

### <a name="servicestack"></a>ServiceStack

ServiceStack とは異なり RestSharp、それらのサービスにアクセスするモバイル アプリケーションで実装できるクライアント ライブラリと同様に、web サービスをホストするサーバー側ソリューション両方。

[Servicestack の web サイト](http://servicestack.net/)では、プロジェクトの目的と、ドキュメントとコードサンプルへのリンクが説明されています。 例には、それにアクセスできるさまざまなクライアント側アプリケーションと同様に、web サービスの完全なサーバー側実装が含まれます。

### <a name="wcf"></a>WCF

Xamarin ツールでは、いくつかの Windows Communication Foundation (WCF) サービスを使用できます。 一般に、Xamarin では、同じクライアント側、Silverlight ランタイムに付属する WCF のサブセットをサポートしています。 これには、`BasicHttpBinding`を使用した、HTTP トランスポートプロトコルを介した WCF: テキストエンコード SOAP メッセージの最も一般的なエンコードおよびプロトコル実装が含まれます。

により、サイズと、WCF フレームワークの複雑さは、Xamarin のクライアントのサブセットのドメインでサポートされている範囲外で分類されます現在および将来のサービスの実装が可能性があります。 さらに、WCF のサポートには、プロキシを生成する Windows 環境でのみ使用できるツールの使用が必要です。

 <a name="Threading" />

## <a name="threading"></a>スレッド

アプリケーションの応答性はモバイル アプリケーションにとって重要です – ユーザーが読み込みをすばやく実行するアプリケーションを期待します。 ユーザー入力の受け入れを停止しますが、アプリケーションがクラッシュするを示すため、ネットワーク要求などの実行時間の長いブロッキング呼び出しや低速のローカル操作 (ファイルを解凍) などの UI スレッドを占有しないことが重要に表示される '固定' 画面。 具体的には、起動プロセスは実行時間の長いタスクを含めることはできません: すべてのモバイル プラットフォームは読み込みに時間がかかるとするアプリを終了します。

つまり、進行状況インジケーターまたはクイックを表示するには、それ以外の場合 '有効' の UI とバック グラウンド操作を実行する非同期タスクのユーザー インターフェイスを実装する必要があります。 バック グラウンド タスクを実行するには、バック グラウンド タスクのニーズを進行状況を示すためにメイン スレッドへの通信方法を意味するかが完了すると、スレッドの使用が必要です。

 <a name="Parallel_Task_Library" />

### <a name="parallel-task-library"></a>タスク並列ライブラリ

タスク並列ライブラリで作成されたタスクは、非同期に実行され、ユーザー インターフェイスをブロックすることがなく長時間実行される操作をトリガーするために非常に便利なため、呼び出し元のスレッドで返すことができます。

単純な並列タスクの操作は、このようになります。

```csharp
using System.Threading.Tasks;
void MainThreadMethod ()
{
    Task.Factory.StartNew (() => wc.DownloadString ("http://...")).ContinueWith (
        t => label.Text = t.Result, TaskScheduler.FromCurrentSynchronizationContext()
    );
}
```

キーは `TaskScheduler.FromCurrentSynchronizationContext()`、そのスレッドへの呼び出しをマーシャリングする手段として、メソッド (`MainThreadMethod`を実行しているメインスレッド) を呼び出すスレッドの SynchronizationContext を再利用します。 これは、UI スレッドでメソッドが呼び出されると、UI スレッドで `ContinueWith` 操作が実行されることを意味します。

コードが他のスレッドからタスクを開始する場合、UI スレッドへの参照を作成する次のパターンを使用し、タスクことができますもコールバックに。

```csharp
static Context uiContext = TaskScheduler.FromCurrentSynchronizationContext();
```

 <a name="Invoking_on_the_UI_Thread" />

### <a name="invoking-on-the-ui-thread"></a>UI スレッドで呼び出す

並列タスクのライブラリを使用しないコードの場合は、各プラットフォーム独自の構文の UI スレッドにマーシャ リングの操作があります。

- **iOS** – `owner.BeginInvokeOnMainThread(new NSAction(action))`
- **Android** – `owner.RunOnUiThread(action)`
- **Xamarin. フォーム**– `Device.BeginInvokeOnMainThread(action)`
- **Windows** – `Deployment.Current.Dispatcher.BeginInvoke(action)`

IOS と Android の構文の両方には、使用できる、コードは、このオブジェクトを UI スレッドでコールバックを必要とする任意のメソッドに渡す必要があることを意味する 'context' クラスが必要です。

共有コードで UI スレッドを呼び出すには、 [IDispatchOnUIThread の例](https://www.slideshare.net/follesoe/cross-platform-mobile-apps-using-net)( [@follesoe](https://twitter.com/follesoe)を参照) に従ってください。 次に示すように、共有コード内の `IDispatchOnUIThread` インターフェイスを宣言してプログラムを宣言し、プラットフォーム固有のクラスを実装します。

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

Xamarin. フォーム開発者は、共通コード (共有プロジェクトまたは PCL) で[`Device.BeginInvokeOnMainThread`](~/xamarin-forms/platform/device.md#interact-with-the-ui-from-background-threads)を使用する必要があります。

 <a name="Platform_and_Device_Capabilities_and_Degradation" />

## <a name="platform-and-device-capabilities-and-degradation"></a>プラットフォームとデバイスの機能低下

さらに処理するさまざまな機能の具体的な例については、プラットフォームの機能のドキュメントで提供されます。 さまざまな機能や、アプリは、その可能性を最大限に動作できない場合でも、優れたユーザー エクスペリエンスを提供するアプリケーションを適切に低下する方法を検出するを処理します。
