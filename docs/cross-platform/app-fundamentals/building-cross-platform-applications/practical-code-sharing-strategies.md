---
title: パート 5 - 実践的なコード共有戦略
description: このドキュメントでは、データベース、ファイルアクセス、ネットワーク操作、非同期コードなどのシナリオでの実際のコード共有方法について説明します。
ms.prod: xamarin
ms.assetid: 328D042A-FF78-A7B6-1574-B5AF49A1AADB
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: fdc9fd6eac8c7b0c9ec91eb66b5d6723cda71006
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016836"
---
# <a name="part-5---practical-code-sharing-strategies"></a>パート 5 - 実践的なコード共有戦略

このセクションでは、一般的なアプリケーションシナリオでコードを共有する方法の例を示します。

## <a name="data-layer"></a>データ層

データ層は、ストレージエンジンと、情報の読み取りと書き込みを行うメソッドで構成されます。 パフォーマンス、柔軟性、クロスプラットフォームの互換性については、Xamarin のクロスプラットフォームアプリケーションに対して SQLite データベースエンジンを使用することをお勧めします。
Windows、Android、iOS、Mac などのさまざまなプラットフォームで実行されます。

### <a name="sqlite"></a>SQLite

SQLite は、オープンソースのデータベースの実装です。 ソースとドキュメントについては、 [SQLite.org](https://www.sqlite.org/)を参照してください。SQLite のサポートは、各モバイルプラットフォームで利用できます。

- **iOS** –オペレーティングシステムに組み込まれています。
- **Android – android** 2.2 (API レベル 10) 以降、オペレーティングシステムに組み込まれています。
- **Windows** –[ユニバーサル Windows プラットフォーム拡張機能の SQLite を](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936)参照してください。

データベースエンジンをすべてのプラットフォームで使用できる場合でも、データベースにアクセスするためのネイティブメソッドは異なります。 IOS と Android はどちらも、Xamarin. iOS または Xamarin. Android から使用できる SQLite にアクセスするための Api を提供しています。ただし、ネイティブ SDK メソッドを使用すると、コードを共有することはできません (文字列として格納されていると仮定して、SQL クエリ自体ではありません). ネイティブデータベース機能の詳細については、iOS または Android の `SQLiteOpenHelper` クラスの `CoreData` を検索してください。これらのオプションはクロスプラットフォームではないため、このドキュメントの範囲を超えています。

### <a name="adonet"></a>ADO.NET

Xamarin と Xamarin Android はどちらも `System.Data` と `Mono.Data.Sqlite` をサポートしています (詳細については、Xamarin iOS の[ドキュメント](~/ios/data-cloud/system.data.md)を参照してください)。
これらの名前空間を使用すると、両方のプラットフォームで動作する ADO.NET コードを記述できます。 プロジェクトの参照を編集して `System.Data.dll` と `Mono.Data.Sqlite.dll` を含め、次の using ステートメントをコードに追加します。

```csharp
using System.Data;
using Mono.Data.Sqlite;
```

次のサンプルコードが動作します。

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

ADO.NET の実際の実装は、当然、さまざまなメソッドやクラスに分割されています (この例は、デモンストレーションの目的でのみ使用されています)。

### <a name="sqlite-net--cross-platform-orm"></a>SQLite-NET –クロスプラットフォームの ORM

ORM (またはオブジェクトリレーショナルマッパー) は、クラスでモデル化されたデータの格納を簡略化しようとします。 テーブルを作成したり、クラスのフィールドやプロパティから手動で抽出されたデータを選択、挿入、削除する SQL クエリを手動で作成するのではなく、ORM はそのようなコードのレイヤーを追加します。 クラスの構造を調べるためにリフレクションを使用すると、ORM はクラスに一致するテーブルと列を自動的に作成し、データの読み取りと書き込みを行うためのクエリを生成できます。 これにより、アプリケーションコードは単に ORM にオブジェクトインスタンスを送信および取得できるようになり、内部のすべての SQL 操作が処理されます。

SQLite-NET は、SQLite でクラスを保存および取得できる単純な ORM として機能します。 これにより、コンパイラディレクティブとその他のテクニックを組み合わせて、クロスプラットフォームの SQLite アクセスの複雑さが隠蔽されます。

SQLite-NET の機能:

- テーブルは、モデルクラスに属性を追加することによって定義されます。
- データベースインスタンスは、SQLite-Net ライブラリのメインクラスである `SQLiteConnection` のサブクラスによって表されます。
- オブジェクトを使用してデータを挿入、照会、削除することができます。 SQL ステートメントは必要ありません (ただし、必要に応じて SQL ステートメントを記述できます)。
- 基本的な Linq クエリは、SQLite-NET によって返されるコレクションに対して実行できます。

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

完全な例については、「ケーススタディのソースコード」を参照してください。

## <a name="file-access"></a>ファイル アクセス

ファイルアクセスは、アプリケーションの重要な部分です。 アプリケーションに含まれる可能性のある一般的なファイルの例を次に示します。

- SQLite データベースファイル。
- ユーザーが生成したデータ (テキスト、画像、サウンド、ビデオ)。
- キャッシュ用にダウンロードしたデータ (画像、html、または PDF ファイル)。

### <a name="systemio-direct-access"></a>System.IO 直接アクセス

Xamarin と Xamarin の両方で、`System.IO` 名前空間のクラスを使用してファイルシステムへのアクセスを許可します。

各プラットフォームには、次の点を考慮する必要があるさまざまなアクセス制限があります。

- iOS アプリケーションは、ファイルシステムへのアクセスが非常に制限されたサンドボックスで実行されます。 Apple では、バックアップする特定の場所 (および以外の場所) を指定することによって、ファイルシステムの使用方法をさらに決定します。 詳細については、「 [Xamarin. iOS でのファイルシステム](~/ios/app-fundamentals/file-system.md)の使用」ガイドを参照してください。
- Android では、アプリケーションに関連する特定のディレクトリへのアクセスも制限されますが、外部メディア ( SD カード) を利用し、共有データにアクセスします。
- Windows Phone 8 (Silverlight) では、ファイルへの直接アクセスは許可されません。ファイルを操作できるのは `IsolatedStorage`を使用した場合のみです。
- Windows 8.1 WinRT プロジェクトと Windows 10 UWP プロジェクトは、他のプラットフォームとは異なる `Windows.Storage` Api による非同期ファイル操作のみを提供します。

#### <a name="example-for-ios-and-android"></a>IOS と Android の例

次に、テキストファイルの書き込みと読み取りを行う簡単な例を示します。
`Environment.GetFolderPath` を使用すると、同じコードを iOS と Android で実行できるため、それぞれがファイルシステムの規則に基づいて有効なディレクトリを返します。

```csharp
string filePath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "MyFile.txt");
System.IO.File.WriteAllText (filePath, "Contents of text file");
Console.WriteLine (System.IO.ReadAllText (filePath));
```

IOS 固有のファイルシステムの機能の詳細については、「Xamarin. iOS の[ファイルシステム](~/ios/app-fundamentals/file-system.md)ドキュメントの操作」を参照してください。 クロスプラットフォームのファイルアクセスコードを記述する場合、一部のファイルシステムでは大文字と小文字が区別され、ディレクトリの区切り記号が異なることに注意してください。 ファイルまたはディレクトリのパスを構築するときは、必ず同じ大文字小文字を使用し、ファイル名と `Path.Combine()` メソッドを使用することをお勧めします。

### <a name="windowsstorage-for-windows-8-and-windows-10"></a>Windows 8 および windows 10 用のストレージ

「*Xamarin. Forms* [book](https://developer.xamarin.com/r/xamarin-forms/book/)
[ を使用した Mobile Apps の作成」第20章を参照してください。Async および File i/o](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf)には、[Windows 8.1 と Windows 10 のサンプル](https://github.com/xamarin/xamarin-forms-book-preview-2/tree/master/Chapter20)が含まれています。

[`DependencyService`](~/xamarin-forms/app-fundamentals/dependency-service/index.md)を使用すると、次のサポートされている api を使用して、これらのプラットフォーム上のファイルを読み取ってファイルを読み込むことができます。

```csharp
StorageFolder localFolder = ApplicationData.Current.LocalFolder;
IStorageFile storageFile = await localFolder.CreateFileAsync("MyFile.txt",
                                        CreationCollisionOption.ReplaceExisting);
await FileIO.WriteTextAsync(storageFile, "Contents of text file");
```

詳細については、[書籍の章](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf)を参照してください。

<a name="Isolated_Storage" />

### <a name="isolated-storage-on-windows-phone-7--8-silverlight"></a>Windows Phone 7 & 8 (Silverlight) の分離ストレージ

分離ストレージは、すべての iOS、Android、および古い Windows Phone プラットフォームでファイルを保存および読み込みするための一般的な API です。

これは、Xamarin に実装されている Windows Phone (Silverlight) のファイルアクセスの既定のメカニズムであり、一般的なファイルアクセスコードを記述できるようにします。 `System.IO.IsolatedStorage` クラスは、[共有プロジェクト](~/cross-platform/app-fundamentals/shared-projects.md)内の3つのすべてのプラットフォームで参照できます。

詳細については、 [Windows Phone の分離ストレージの概要](https://msdn.microsoft.com/library/windowsphone/develop/ff402541(v=vs.105).aspx)に関するトピックを参照してください。

分離ストレージ Api は、[ポータブルクラスライブラリ](~/cross-platform/app-fundamentals/pcl.md)では使用できません。 PCL の代替手段として[Pclstorage NuGet](https://pclstorage.codeplex.com/)を使用することもあります。

### <a name="cross-platform-file-access-in-pcls"></a>PCLs でのクロスプラットフォームファイルアクセス

また、PCL 互換の Nuget – [Pclstorage](https://www.nuget.org/packages/PCLStorage/)があります。これにより、Xamarin でサポートされているプラットフォームと最新の Windows api のクロスプラットフォームファイルアクセスが可能になります。

## <a name="network-operations"></a>ネットワーク操作

ほとんどのモバイルアプリケーションでは、次のようなネットワークコンポーネントが使用されます。

- イメージ、ビデオ、およびオーディオのダウンロード (例 サムネイル、写真、音楽)。
- ドキュメントのダウンロード (例 HTML、PDF)。
- ユーザーデータ (写真やテキストなど) をアップロードしています。
- Web サービスまたはサードパーティの Api (SOAP、XML、JSON など) へのアクセス。

.NET Framework には、`HttpClient`、`WebClient`、および `HttpWebRequest`のネットワークリソースにアクセスするためのいくつかの異なるクラスが用意されています。

### <a name="httpclient"></a>HttpClient

`System.Net.Http` 名前空間の `HttpClient` クラスは、Xamarin、Xamarin、Android、およびほとんどの Windows プラットフォームで使用できます。 この API をポータブルクラスライブラリ (および Windows Phone 8 Silverlight) に取り込むために使用できる[MICROSOFT HTTP クライアントライブラリ Nuget](https://www.nuget.org/packages/Microsoft.Net.Http/)が用意されています。

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

### <a name="reachability"></a>的

モバイルデバイスは、高速 Wi-fi または4G 接続から、受信領域が不足し、エッジデータリンクが遅い場合に、さまざまなネットワーク状態で動作します。 このため、ネットワークが使用可能かどうかを検出し、リモートサーバーへの接続を試行する前に、使用可能なネットワークの種類を確認することをお勧めします。

次のような場合に、モバイルアプリで実行されるアクション。

- ネットワークが使用できない場合は、ユーザーにアドバイスします。 手動で無効にした場合 ( 飛行機モードまたは Wi-fi をオフにすると、問題を解決できるようになります。
- 接続が3G の場合、アプリケーションの動作が異なる場合があります (たとえば、Apple では、20 Mb を超えるアプリをダウンロードすることはできません)。 アプリケーションでは、この情報を使用して、大きなファイルを取得するときにダウンロードが過剰になることをユーザーに警告できます。
- ネットワークが使用可能な場合でも、他の要求を開始する前に、対象サーバーとの接続を確認することをお勧めします。 これにより、アプリのネットワーク操作が繰り返しタイムアウトするのを防ぐことができ、さらに有益なエラーメッセージをユーザーに表示することもできます。

ネットワークの可用性を検出するために使用できる[Xamarin. iOS サンプル](https://github.com/xamarin/monotouch-samples/tree/master/ReachabilitySample)(Apple の[到達可能性サンプルコード](https://developer.apple.com/library/ios/#samplecode/Reachability/Introduction/Intro.html)に基づいています) があります。

## <a name="webservices"></a>WebServices

Xamarin を使用した REST、SOAP、WCF エンドポイントへのアクセスについては、 [Web サービスの](~/cross-platform/data-cloud/web-services/index.md)使用に関するドキュメントを参照してください。 Web サービス要求を手動で作成して応答を解析することは可能ですが、Azure、RestSharp、ServiceStack など、これをより簡単にするためのライブラリが用意されています。 基本的な WCF 操作にも、Xamarin アプリでアクセスできます。

### <a name="azure"></a>Azure

Microsoft Azure は、データストレージと同期、プッシュ通知など、モバイルアプリ用のさまざまなサービスを提供するクラウドプラットフォームです。

[Azure.microsoft.com](https://azure.microsoft.com/)にアクセスして無料で試すことができます。

### <a name="restsharp"></a>RestSharp

RestSharp は、web サービスへのアクセスを簡略化する REST クライアントを提供するためにモバイルアプリケーションに含めることができる .NET ライブラリです。 これは、データを要求し、REST 応答を解析するための単純な API を提供するのに役立ちます。 RestSharp が役に立つ

[RestSharp web サイト](http://restsharp.org/)には、RestSharp を使用した REST クライアントの実装方法に関する[ドキュメント](https://github.com/restsharp/RestSharp/wiki)が含まれています。
RestSharp には、 [github](https://github.com/restsharp/RestSharp/)のサンプルが用意されています。

[Web サービスのドキュメント](~/cross-platform/data-cloud/web-services/index.md)には、Xamarin の iOS コードスニペットもあります。

 <a name="ServiceStack" />

### <a name="servicestack"></a>ServiceStack

RestSharp とは異なり、ServiceStack は、web サービスをホストするためのサーバー側のソリューションであり、モバイルアプリケーションに実装してこれらのサービスにアクセスするためのクライアントライブラリでもあります。

[Servicestack の web サイト](http://servicestack.net/)では、プロジェクトの目的と、ドキュメントとコードサンプルへのリンクが説明されています。 例としては、web サービスの完全なサーバー側実装や、それにアクセスできるさまざまなクライアント側アプリケーションがあります。

ServiceStack web サイトには[Xamarin. iOS の例](http://www.servicestack.net/monotouch/remote-info/)があり、 [Web サービスのドキュメント](~/cross-platform/data-cloud/web-services/index.md)にはコードスニペットがあります。

### <a name="wcf"></a>WCF

Xamarin ツールは、いくつかの Windows Communication Foundation (WCF) サービスを利用するのに役立ちます。 一般に、Xamarin では、Silverlight ランタイムに付属する WCF のクライアント側の同じサブセットをサポートしています。 これには、`BasicHttpBinding`を使用した、HTTP トランスポートプロトコルを介した WCF: テキストエンコード SOAP メッセージの最も一般的なエンコードおよびプロトコル実装が含まれます。

WCF フレームワークのサイズと複雑さにより、Xamarin のクライアントサブセットドメインでサポートされているスコープの外部になる、現在および将来のサービス実装が存在する場合があります。 さらに、WCF サポートでは、プロキシを生成するために Windows 環境でのみ使用可能なツールを使用する必要があります。

 <a name="Threading" />

## <a name="threading"></a>スレッド

アプリケーションの応答性はモバイルアプリケーションにとって重要であり、ユーザーはアプリケーションの読み込みと実行を迅速に行うことが期待されます。 ユーザー入力の受け入れを停止する ' 凍結された ' 画面は、アプリケーションがクラッシュしたことを示しています。そのため、UI スレッドとネットワーク要求やローカルでの低速な操作 (ファイルの解凍など) の実行時間の長いブロック呼び出しを関連付けることは重要です。 特に、スタートアッププロセスには長時間実行されるタスクを含めないでください。すべてのモバイルプラットフォームは、読み込みに時間がかかりすぎるアプリを強制終了します。

つまり、ユーザーインターフェイスでは、"進行状況インジケーター" を実装する必要があります。または、簡単に表示できるように、またはバックグラウンド操作を実行する非同期タスクを実装する必要があります。 バックグラウンドタスクを実行するにはスレッドを使用する必要があります。これは、バックグラウンドタスクがメインスレッドに返信して、進行状況や完了したタイミングを示す方法を必要とすることを意味します。

 <a name="Parallel_Task_Library" />

### <a name="parallel-task-library"></a>並列タスクライブラリ

並列タスクライブラリを使用して作成されたタスクは、非同期に実行して呼び出し元のスレッドで返すことができるため、ユーザーインターフェイスをブロックすることなく、長時間実行される操作をトリガーするのに非常に便利です。

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

キーは `TaskScheduler.FromCurrentSynchronizationContext()`、そのスレッドへの呼び出しをマーシャリングする手段として、メソッド (`MainThreadMethod`を実行しているメインスレッド) を呼び出すスレッドの SynchronizationContext を再利用します。 これは、UI スレッドでメソッドが呼び出されると、UI スレッドで `ContinueWith` 操作が実行されることを意味します。

コードが他のスレッドからタスクを開始している場合は、次のパターンを使用して UI スレッドへの参照を作成します。タスクは、それに対してコールバックを実行できます。

```csharp
static Context uiContext = TaskScheduler.FromCurrentSynchronizationContext();
```

 <a name="Invoking_on_the_UI_Thread" />

### <a name="invoking-on-the-ui-thread"></a>UI スレッドでの呼び出し

並列タスクライブラリを使用しないコードの場合、各プラットフォームには、UI スレッドに操作をマーシャリングするための独自の構文があります。

- **iOS** – `owner.BeginInvokeOnMainThread(new NSAction(action))`
- **Android** – `owner.RunOnUiThread(action)`
- **Xamarin. フォーム**– `Device.BeginInvokeOnMainThread(action)`
- **Windows** – `Deployment.Current.Dispatcher.BeginInvoke(action)`

IOS と Android の両方の構文では、' context ' クラスを使用できるようにする必要があります。これは、コードが UI スレッドでコールバックを必要とするメソッドにこのオブジェクトを渡す必要があることを意味します。

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

## <a name="platform-and-device-capabilities-and-degradation"></a>プラットフォームとデバイスの機能とパフォーマンスの低下

さまざまな機能を処理する具体的な例については、プラットフォーム機能に関するドキュメントを参照してください。 さまざまな機能を検出し、アプリケーションを適切に低下させて適切なユーザーエクスペリエンスを提供する方法について説明します。アプリが完全な可能性を持つことができない場合でも同様です。
