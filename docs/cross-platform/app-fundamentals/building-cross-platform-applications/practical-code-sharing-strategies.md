---
title: パート 5 - 実践的なコード共有戦略
description: このドキュメントでは、実践的なコードを共有データベース、ファイルへのアクセス、ネットワークの操作および非同期コードのようなシナリオの戦略について説明します。
ms.prod: xamarin
ms.assetid: 328D042A-FF78-A7B6-1574-B5AF49A1AADB
author: conceptdev
ms.author: crdun
ms.date: 03/23/2017
ms.openlocfilehash: 9f0a4d0367142be500aeae67041feb8cd3bbca76
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70288800"
---
# <a name="part-5---practical-code-sharing-strategies"></a>パート 5 - 実践的なコード共有戦略

このセクションでは、一般的なアプリケーション シナリオのコードを共有する方法の例を示します。



## <a name="data-layer"></a>データ層

データ層は、ストレージ エンジンと情報を読み書きするメソッドで構成されます。 パフォーマンス、柔軟性、クロス プラットフォームの互換性、SQLite データベース エンジンは Xamarin クロスプラット フォーム対応のアプリケーションをお勧めします。
さまざまな Windows、Android、iOS、およびファルダなどのプラットフォームで実行されます。


### <a name="sqlite"></a>SQLite

SQLite とは、オープン ソース データベースの実装です。 ソースとドキュメントをご覧ください[SQLite.org](http://www.sqlite.org/)します。SQLite のサポートは、各モバイル プラットフォームで入手できます。

- **iOS** – オペレーティング システムに組み込まれています。
- **Android** – Android 2.2 (API レベル 10) 以降、オペレーティング システムに組み込まれています。
- **Windows** – を参照してください、[拡張機能のユニバーサル Windows プラットフォーム用 SQLite](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936)します。


すべてのプラットフォームで利用可能なデータベース エンジンでも、データベースにアクセスするネイティブなメソッドが異なります。 両方の iOS と Android からにアクセスするために使用できる SQLite Xamarin.iOS または Xamarin.Android の組み込みの API を提供して、ただし、ネイティブ SDK メソッドを使用して (おそらく、SQL クエリ自体を文字列として格納されていると仮定した場合) 以外のコードを共有する機能はありません. 詳細については、ネイティブ データベース機能を探し`CoreData`で iOS または Android の`SQLiteOpenHelper`クラスは、このドキュメントの範囲を超えているため、これらのオプションは、クロス プラットフォームではありません。



### <a name="adonet"></a>ADO.NET

Xamarin.iOS と Xamarin.Android の両方のサポート`System.Data`と`Mono.Data.Sqlite`(Xamarin.iOS を参照してください。[ドキュメント](~/ios/data-cloud/system.data.md)の詳細)。
これらの名前空間を使用して両方のプラットフォームで動作する ADO.NET コードを記述することができます。 編集、プロジェクトの参照を含める`System.Data.dll`と`Mono.Data.Sqlite.dll`し、次の using ステートメントをコードに追加します。

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
- データベース インスタンスは、のサブクラスで表される`SQLiteConnection`、SQLite net-library のメイン クラスです。
- クエリおよびオブジェクトを使用して削除された、データを挿入できます。 SQL ステートメントは必要ありません (ただし、必要な場合は、SQL ステートメントを記述することができますです)。
- SQLite NET によって返されるコレクションでは、基本的な Linq クエリを実行できます。


ソース コードと SQLite NET のドキュメントは、 [SQLite-Net github](https://github.com/praeclarum/sqlite-net)し、両方のケース スタディに実装されています。 SQLite NET コードの単純な例 (から、 *Tasky Pro*ケース スタディ) を次に示します。

最初に、`TodoItem`クラスでは、属性を使用して、データベースの主キー フィールドを定義します。

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

これにより、`TodoItem`上のコード (および SQL ステートメント) は、次の行で作成するテーブル、`SQLiteConnection`インスタンス。

```csharp
CreateTable<TodoItem> ();
```

テーブル内のデータが他の方法で操作することもできます、 `SQLiteConnection` (ここでも、なしの SQL ステートメントを必要とする)。

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

Xamarin.iOS と Xamarin.Android の両方でクラスを使用してファイル システム アクセスを許可する、`System.IO`名前空間。

各プラットフォームには、考慮事項に考慮しなければならない別のアクセス制限があります。

- iOS アプリケーションは、非常に制限されているファイル システム アクセス権を持つサンド ボックスで実行します。 さらに Apple では、バックアップを特定の場所 (およびその他のない) を指定することで、ファイル システムを使用する方法を決定します。 参照してください、 [Xamarin.iOS でのファイル システム操作](~/ios/app-fundamentals/file-system.md)詳細はガイド。
- Android は、アプリケーションに関連する特定のディレクトリにアクセスを制限することもが、外部メディア (例。 SD カード) および共有データへのアクセスします。
- Windows Phone 8 (Silverlight) は、ファイルに直接アクセスを許可しない – を使用してファイルを操作することができますのみ`IsolatedStorage`します。
- WinRT の Windows 8.1 および Windows 10 UWP プロジェクトのみを使用して非同期のファイル操作を提供する`Windows.Storage`API で、他のプラットフォームとは異なります。

#### <a name="example-for-ios-and-android"></a>IOS および Android 用の例

テキスト ファイルを読み書きするための簡単な例は、以下に示します。
使用して`Environment.GetFolderPath`iOS と Android の各を返すことが、ファイル システム規則に基づく有効なディレクトリで実行する同じコードを使用します。

```csharp
string filePath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "MyFile.txt");
System.IO.File.WriteAllText (filePath, "Contents of text file");
Console.WriteLine (System.IO.ReadAllText (filePath));
```

Xamarin.iOS を参照してください[ファイル システム操作](~/ios/app-fundamentals/file-system.md)iOS 固有のファイル システムの機能の詳細についてはドキュメント。 クロスプラット フォームでファイル アクセス コードを記述する場合は、一部のファイル システムが大文字小文字を区別し、別のディレクトリの区切り記号があることに注意してください。 常にファイル名の同じ大文字小文字の区別を使用することをお勧め、`Path.Combine()`メソッド ファイルまたはディレクトリのパスを構築するときにします。



### <a name="windowsstorage-for-windows-8-and-windows-10"></a>Windows 8 および Windows 10 の Windows.Storage

*を Xamarin.Forms での Mobile Apps の作成*[帳](https://developer.xamarin.com/r/xamarin-forms/book/)
[第 20 章です。Async およびファイル I/O](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf)が含まれています[Windows 8.1 および Windows 10 用のサンプル](https://github.com/xamarin/xamarin-forms-book-preview-2/tree/master/Chapter20)します。

使用して、 [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md)サポートされている API を使用してこれらのプラットフォーム上のファイルのファイルを読み書きすることは。

```csharp
StorageFolder localFolder = ApplicationData.Current.LocalFolder;
IStorageFile storageFile = await localFolder.CreateFileAsync("MyFile.txt",
                                        CreationCollisionOption.ReplaceExisting);
await FileIO.WriteTextAsync(storageFile, "Contents of text file");
```

参照してください、[書籍の章](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf)の詳細。


<a name="Isolated_Storage" />

### <a name="isolated-storage-on-windows-phone-7--8-silverlight"></a>Windows Phone 7 および 8 (Silverlight) の分離ストレージ

分離ストレージは、保存して、すべての iOS、Android、および以前の Windows Phone プラットフォーム間でファイルの読み込みを共通の API です。

Xamarin.iOS および Xamarin.Android ファイル アクセスの一般的なコードの許可に書き込まれるで実装されている Windows Phone (Silverlight) におけるファイル アクセスの既定のメカニズムです。 `System.IO.IsolatedStorage`で 3 つすべてのプラットフォーム間でクラスを参照できる、[共有プロジェクト](~/cross-platform/app-fundamentals/shared-projects.md)します。

参照してください、[分離ストレージの概要の Windows Phone](https://msdn.microsoft.com/library/windowsphone/develop/ff402541(v=vs.105).aspx)詳細についてはします。

分離ストレージ API では使用できない[ポータブル クラス ライブラリ](~/cross-platform/app-fundamentals/pcl.md)します。 PCL の代わり、[化できる PCLStorage NuGet](https://pclstorage.codeplex.com/)



### <a name="cross-platform-file-access-in-pcls"></a>Pcl でクロスプラット フォームでファイル アクセス

PCL と互換性のある Nuget – はも[PCLStorage](https://www.nuget.org/packages/PCLStorage/) – Xamarin でサポートされているプラットフォームと最新の Windows API の施設クロスプラット フォームでファイルのアクセス。


## <a name="network-operations"></a>ネットワーク操作

ほとんどのモバイル アプリケーションがネットワーク コンポーネントは、たとえば。

- イメージをダウンロード、ビデオおよびオーディオ (例。 サムネイル、写真、音楽)。
- (例: ドキュメントのダウンロード HTML、PDF)。
- (写真やテキスト) などのユーザー データをアップロードしています。
- Web サービスまたはサード パーティ (などの SOAP、XML または JSON) の API にアクセスします。


.NET Framework のネットワーク リソースにアクセスするため、いくつかの異なるクラスを提供します。 `HttpClient`、 `WebClient`、および`HttpWebRequest`します。

### <a name="httpclient"></a>HttpClient

`HttpClient`クラス、`System.Net.Http`名前空間は、Xamarin.iOS、Xamarin.Android、およびほとんどの Windows プラットフォームで使用できます。 [Microsoft HTTP クライアント ライブラリ Nuget](https://www.nuget.org/packages/Microsoft.Net.Http/)ポータブル クラス ライブラリ (および Windows Phone 8 Silverlight) にこの API を使用できます。

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get, "https://xamarin.com");
var response = await myClient.SendAsync(request);
```

### <a name="webclient"></a>WebClient

`WebClient`クラスには、リモート サーバーからリモート データを取得する単純な API が用意されています。

ユニバーサル Windows プラットフォームの運用*する必要があります*async をする場合でも、Xamarin.iOS および Xamarin.Android (これは、バック グラウンド スレッドで実行できます)。 同期の操作をサポートします。

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

 `WebClient` また`DownloadFileCompleted`と`DownloadFileAsync`バイナリ データを取得するためです。

<a name="HttpWebRequest" />

### <a name="httpwebrequest"></a>HttpWebRequest

`HttpWebRequest` もさらにカスタマイズを提供しています。 `WebClient` 、結果として、多くのコードを使用する必要があります。

簡単な同期コード`HttpWebRequest`操作します。

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

例では、 [Web Services のドキュメント](~/cross-platform/data-cloud/web-services/index.md)します。

 <a name="Reachability" />


### <a name="reachability"></a>経由で到達可能

モバイル デバイスは、さまざまなから高速の Wi-fi ネットワークの状態または不適切な受付および EDGE データへの低速リンクへの 4 G 接続の下で動作します。 このためは、ネットワークが利用可能かどうかと、そのため、ネットワークの種類があるか、リモート サーバーに接続する前に検出することをお勧めします。

このような状況でモバイル アプリがかかる場合がありますアクションは次のとおりです。

- ネットワークが利用できない場合は、ユーザーにお勧めします。 手動で無効になっていること (例: 場合 機内モードまたは Wi-fi をオフにする) し、問題を解決することができます。
- 接続が 3 G の場合は、アプリケーションの動作が異なります (たとえば、Apple を許可しないアプリ 3 G を超えるダウンロードを 20 Mb より大きい)。 アプリケーションがこの情報を使用して過度のダウンロードについてユーザーに警告する大きなファイルを取得するときにタイムアウトします。
- ネットワークを使用できる場合でも、他の要求を開始する前に、ターゲット サーバーとの接続を確認することをお勧めを勧めします。 アプリのネットワーク操作がタイムアウトを繰り返し防止されより多くの情報のエラー メッセージ、ユーザーに表示することもできます。


[Xamarin.iOS サンプル](https://github.com/xamarin/monotouch-samples/tree/master/ReachabilitySample)使用可能な (Apple のに基づいて[サンプル コードの到達可能性](https://developer.apple.com/library/ios/#samplecode/Reachability/Introduction/Intro.html)) ネットワークの可用性を検出するためにします。


## <a name="webservices"></a>WebServices

ドキュメントを参照してください[Web サービスを使用する](~/cross-platform/data-cloud/web-services/index.md)が含まれています、残りの部分にアクセスする Xamarin.iOS を使用して SOAP および WCF のエンドポイントには。 Web サービスの要求を自ら作成することができますしがはるかに簡単、Azure、RestSharp、ServiceStack などに使用可能なライブラリがあります、応答を解析します。 基本的な WCF 操作は、Xamarin アプリでアクセスできます。

### <a name="azure"></a>Azure

Microsoft Azure とは、さまざまなデータ ストレージとの同期、プッシュ通知など、モバイル アプリのサービスを提供するクラウド プラットフォームです。

参照してください[azure.microsoft.com](https://azure.microsoft.com/)を無料で試します。

### <a name="restsharp"></a>RestSharp

RestSharp は、web サービスへのアクセスを簡素化するための REST クライアントを提供するモバイル アプリケーションに含めることができる .NET ライブラリです。 データを要求して、REST 応答を解析する単純な API を提供することができます。 RestSharp が役に立ちます

[RestSharp web サイト](http://restsharp.org/)を含む[ドキュメント](https://github.com/restsharp/RestSharp/wiki)RestSharp を使用して REST クライアントを実装する方法についてはします。
RestSharp で Xamarin.iOS と Xamarin.Android の例を提供する[github](https://github.com/restsharp/RestSharp/)します。

Xamarin.iOS のコード スニペットはまた、 [Web Services のドキュメント](~/cross-platform/data-cloud/web-services/index.md)します。

 <a name="ServiceStack" />


### <a name="servicestack"></a>ServiceStack

ServiceStack とは異なり RestSharp、それらのサービスにアクセスするモバイル アプリケーションで実装できるクライアント ライブラリと同様に、web サービスをホストするサーバー側ソリューション両方。

[ServiceStack web サイト](http://servicestack.net/)ドキュメントおよびコード サンプルへのプロジェクトとリンクの目的を説明します。 例には、それにアクセスできるさまざまなクライアント側アプリケーションと同様に、web サービスの完全なサーバー側実装が含まれます。

[Xamarin.iOS 例](http://www.servicestack.net/monotouch/remote-info/)ServiceStack web サイトでのコード スニペット、 [Web Services のドキュメント](~/cross-platform/data-cloud/web-services/index.md)します。


### <a name="wcf"></a>WCF

Xamarin ツールでは、いくつかの Windows Communication Foundation (WCF) サービスを使用できます。 一般に、Xamarin では、同じクライアント側、Silverlight ランタイムに付属する WCF のサブセットをサポートしています。 WCF の最も一般的なエンコード、およびプロトコルの実装が含まれます。 HTTP 経由の SOAP メッセージのテキストでエンコードされたトランスポート プロトコルを使用して、`BasicHttpBinding`します。

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

キーが`TaskScheduler.FromCurrentSynchronizationContext()`メソッドを呼び出すスレッドの SynchronizationContext.Current は再利用する (ここでは、メイン スレッドを実行している`MainThreadMethod`) をそのスレッドに戻る呼び出しをマーシャ リングする方法として。 つまり、メソッドは、UI スレッドで呼び出されると、実行、`ContinueWith`戻り、UI スレッドで操作します。

コードが他のスレッドからタスクを開始する場合、UI スレッドへの参照を作成する次のパターンを使用し、タスクことができますもコールバックに。

```csharp
static Context uiContext = TaskScheduler.FromCurrentSynchronizationContext();
```

 <a name="Invoking_on_the_UI_Thread" />


### <a name="invoking-on-the-ui-thread"></a>UI スレッドで呼び出す

並列タスクのライブラリを使用しないコードの場合は、各プラットフォーム独自の構文の UI スレッドにマーシャ リングの操作があります。

- **iOS** – `owner.BeginInvokeOnMainThread(new NSAction(action))`
- **Android** – `owner.RunOnUiThread(action)`
- **Xamarin.Forms** – `Device.BeginInvokeOnMainThread(action)`
- **Windows** – `Deployment.Current.Dispatcher.BeginInvoke(action)`



IOS と Android の構文の両方には、使用できる、コードは、このオブジェクトを UI スレッドでコールバックを必要とする任意のメソッドに渡す必要があることを意味する 'context' クラスが必要です。

共有コードでは、UI スレッドの呼び出しを行う、に従って、 [IDispatchOnUIThread 例](https://www.slideshare.net/follesoe/cross-platform-mobile-apps-using-net)(からの引用です[ @follesoe ](http://jonas.follesoe.no/))。 宣言し、プログラムを`IDispatchOnUIThread`共有コードでインターフェイスを次に示すようにプラットフォーム固有のクラスを実装します。

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

Xamarin.Forms の開発を使用する必要があります[ `Device.BeginInvokeOnMainThread` ](~/xamarin-forms/platform/device.md#interact-with-the-ui-from-background-threads)一般的なコード (共有プロジェクトまたは PCL) にします。

 <a name="Platform_and_Device_Capabilities_and_Degradation" />


## <a name="platform-and-device-capabilities-and-degradation"></a>プラットフォームとデバイスの機能低下

さらに処理するさまざまな機能の具体的な例については、プラットフォームの機能のドキュメントで提供されます。 さまざまな機能や、アプリは、その可能性を最大限に動作できない場合でも、優れたユーザー エクスペリエンスを提供するアプリケーションを適切に低下する方法を検出するを処理します。
