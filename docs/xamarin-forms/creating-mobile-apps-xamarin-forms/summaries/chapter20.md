---
title: "20 章の概要です。 Async およびファイル I/O"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D595862D-64FD-4C0D-B0AD-C1F440564247
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 86ae56fc2baac3eab0fbf375c5f67f7b2327721a
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-20-async-and-file-io"></a>20 章の概要です。 Async およびファイル I/O

 グラフィカル ユーザー インターフェイスは、順番にユーザー入力イベントに応答する必要があります。 つまり、ユーザー入力イベントのすべての処理は、多くの場合と呼ばれる 1 つのスレッドで行う必要があります、*メイン スレッド*または*UI スレッド*です。

ユーザーは、グラフィカル ユーザー インターフェイスが応答してを期待します。 これは、プログラムを迅速にユーザー入力イベントを処理する必要があることを意味します。 可能でない場合にセカンダリ スレッドし、処理を移行する必要があります。

このブック内のいくつかのサンプル プログラムを使用した、 [ `WebRequest` ](https://developer.xamarin.com/api/type/System.Net.WebRequest/)クラスです。 このクラスで、 [ `BeginGetReponse` ](https://developer.xamarin.com/api/member/System.Net.WebRequest.BeginGetResponse/p/System.AsyncCallback/System.Object/)メソッドは、完了すると、コールバック関数を呼び出すワーカー スレッドを開始します。 ただし、そのコールバック関数は、実行、ワーカー スレッドで、プログラムで呼び出す必要があります[ `Device.BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/)ユーザー インターフェイスにアクセスするメソッド。

非同期処理する最新の方法は、.NET および c# で使用可能です。 これは、ためには、 [ `Task` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.Task/)と[ `Task<TResult>` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.Task%3CTResult%3E/)クラス、およびその他の型、 [ `System.Threading` ](https://developer.xamarin.com/api/namespace/System.Threading/)と[ `System.Threading.Tasks` ](https://developer.xamarin.com/api/namespace/System.Threading.Tasks/) 、名前空間 (C#) 5.0 および`async`と`await`キーワード。 この章の焦点です。

## <a name="from-callbacks-to-await"></a>待機するコールバックから

`Page`クラス自体には、アラートのボックスを表示する次の 3 つの非同期メソッドが含まれています。

- [`DisplayAlert`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/) 返します、`Task`オブジェクト
- [`DisplayAlert`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/System.String/) 返します、`Task<bool>`オブジェクト
- [`DisplayActionSheet`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet/p/System.String/System.String/System.String/System.String[]/) 返します、`Task<string>`オブジェクト

`Task`オブジェクトは、これらのメソッドがタップと呼ばれる、タスクベースの非同期パターンを実装することを示します。 これら`Task`オブジェクトが、メソッドからすぐに返されます。 `Task<T>`値の構成"promise"を返す型の値`TResult`タスクが完了したときに使用可能になります。 `Task`返す値を示します非同期のアクションの値を持たない完全が返されることになります。

これらすべてのケースで、`Task`が完了すると、ユーザーは警告する ボックスを閉じます。  

### <a name="an-alert-with-callbacks"></a>コールバックのアラート

[ **AlertCallbacks** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertCallbacks)サンプルを処理する方法を示します`Task<bool>`オブジェクトを返すと`Device.BeginInvokeOnMainThread`呼び出しコールバック メソッドを使用します。

### <a name="an-alert-with-lambdas"></a>ラムダのアラート

[ **AlertLambdas** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertLambdas)サンプルを処理するための匿名のラムダ関数を使用する方法を示します`Task`と`Device.BeginInvokeOnMainThread`呼び出しです。  

### <a name="an-alert-with-await"></a>Await のアラート

さらにわかりやすい方法では、`async`と`await`c# 5 で導入されるキーワード。 [ **AlertAwait** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertAwait)サンプルでは、使用できます。

### <a name="an-alert-with-nothing"></a>何もアラート

非同期のメソッドを返す場合`Task`なく`Task<TResult>`プログラムは非同期タスクの完了時を知る必要がある場合は、これらの手法のいずれかを使用する必要があります。 [ **NothingAlert** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/NothingAlert)サンプルを示します。

### <a name="saving-program-settings-asynchronously"></a>プログラム設定を保存して非同期的に

[ **SaveProgramChanges** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/SaveProgramSettings)サンプルでの使用、 [ `SavePropertiesAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.SavePropertiesAsync()/)メソッドの`Application`せずに変更すると、プログラムの設定を保存するにはオーバーライドする、`OnSleep`メソッドです。

### <a name="a-platform-independent-timer"></a>プラットフォームに依存しないタイマー

使用することは[ `Task.Delay` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.Delay/p/System.Int32/)プラットフォームに依存しないタイマーを作成します。 [ **TaskDelayClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TaskDelayClock)サンプルを示します。

## <a name="file-inputoutput"></a>入力/出力ファイル

.NET 従来、 [ `System.IO` ](https://developer.xamarin.com/api/namespace/System.IO/)名前空間がファイル I/O のサポートのソースをされました。 この名前空間の一部のメソッドは、非同期操作をサポートするがほとんど必要ありません。 名前空間には、高度なファイル I/O 機能を実行するいくつかの単純なメソッド呼び出しもサポートしています。

### <a name="good-news-and-bad-news"></a>良いニュースと短所

Xamarin.Forms サポート アプリケーションのローカル記憶域 & #x 2014; でサポートされるすべてのプラットフォームアプリケーションに対してプライベート ストレージです。

Xamarin.iOS および Xamarin.Android ライブラリには、これら 2 つのプラットフォームの Xamarin が明示的に対応した .NET のバージョンが含まれます。 クラスが含まれます`System.IO`これら 2 つのプラットフォームでのアプリケーションのローカル記憶域を持つファイル入出力の実行に使用することできます。

ただし、これらを検索する場合`System.IO`Xamarin.Forms PCL にクラス、見つからないにします。 この問題は、Windows ランタイム API を完全に刷新 Microsoft ファイル I/O です。 Windows 8.1、Windows Phone 8.1、およびユニバーサル Windows プラットフォームを対象とするプログラムは使用しないでください`System.IO`ファイル I/O です。

つまり、使用する必要があります、 [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) (最初は、後ほど[**章 9 です。プラットフォーム固有の API 呼び出し**](chapter09.md)ファイル I/O を実装します。

### <a name="a-first-shot-at-cross-platform-file-io"></a>クロス プラットフォームのファイル I/O の最初のショット

[ **TextFileTryout** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileTryout)サンプルでは定義、 [ `IFileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/TextFileTryout/TextFileTryout/TextFileTryout/IFileHelper.cs)ファイル I/O、およびすべてのプラットフォームでは、このインターフェイスの実装のためのインターフェイスです。 ただし、Windows ランタイムの実装は、Windows ランタイムのファイル I/O メソッドが非同期であるために、このインターフェイスのメソッドで動作しません。

### <a name="accommodating-windows-runtime-file-io"></a>Windows ランタイムのファイル I/O に対応

Windows ランタイムで実行されるプログラム クラスを使用して、 [ `Windows.Storage` ](https://msdn.microsoft.com/library/windows/apps/windows.storage.aspx)と[ `Windows.Storage.Streams` ](https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.aspx)ファイル I/O、アプリケーションのローカル記憶域を含む名前空間。 Microsoft では、50 個を超える (ミリ秒) を UI スレッドがブロックされないようにを非同期にする必要がありますを必要とするすべての操作を特定、ためこれらのファイル I/O メソッドは非同期にほとんどの場合。

他のアプリケーションで使用できるように、この新しい方法を示すコードはライブラリになります。

## <a name="platform-specific-libraries"></a>プラットフォーム固有のライブラリ

ライブラリに再利用可能なコードを格納することをお勧めします。 これは、機能は、再利用可能なコードの各部分がまったく異なるオペレーティング システムが明らかに困難です。

[ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform)ソリューションが 1 つの方法を示します。 このソリューションには、7 つの異なるプロジェクトが含まれています。

- [**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform)、通常、Xamarin.Forms PCL
- [**Xamarin.FormsBook.Platform.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS)、iOS クラス ライブラリ
- [**Xamarin.FormsBook.Platform.Android**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android), an Android class library
- [**Xamarin.FormsBook.Platform.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.UWP)、ユニバーサル Windows クラス ライブラリ
- [**Xamarin.FormsBook.Platform.Windows**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Windows), a PCL for Windows 8.1.
- [**Xamarin.FormsBook.Platform.WinPhone**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinPhone), a PCL for Windows Phone 8.1
- [**Xamarin.FormsBook.Platform.WinRT**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT)、すべての Windows プラットフォームに共通するコードの共有プロジェクト

個々 のプラットフォームのすべてのプロジェクト (の例外を除いて**Xamarin.FormsBook.Platform.WinRT**) を参照して**Xamarin.FormsBook.Platform**です。 3 つの Windows プロジェクトへの参照がある**Xamarin.FormsBook.Platform.WinRT**です。

すべてのプロジェクトでは、静的な`Toolkit.Init`メソッドを直接ではなく Xamarin.Forms アプリケーション ソリューション内のプロジェクトによって参照されている場合、ライブラリが読み込まれていることを確認します。

**Xamarin.FormsBook.Platform**プロジェクトが含まれていますが、新しい[ `IFileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/IFileHelper.cs)インターフェイスです。 すべてのメソッドは今すぐに名前を持つ`Async`サフィックスと戻り値`Task`オブジェクト。

**Xamarin.FormsBook.Platform.WinRT**プロジェクトに含まれる、 [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) Windows ランタイム クラスです。

**Xamarin.FormsBook.Platform.iOS**プロジェクトに含まれる、 [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/FileHelper.cs) iOS 用のクラスです。 これらのメソッドは非同期になります。 定義されているメソッドの非同期バージョンを使用して、メソッドの一部`StreamWriter`と`StreamReader`: [ `WriteAsync` ](https://developer.xamarin.com/api/member/System.IO.StreamWriter.WriteAsync/p/System.String/)と[ `ReadToEndAsync`](https://developer.xamarin.com/api/member/System.IO.StreamReader.ReadToEndAsync()/)です。 他のユーザーに変換結果を`Task`オブジェクトを使用して、 [ `FromResult` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.FromResult%7BTResult%7D/p/TResult/)メソッドです。

**Xamarin.FormsBook.Platform.Android**プロジェクトが含まれていますが、類似[ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/FileHelper.cs) for Android のクラスです。

**Xamarin.FormsBook.Platform**プロジェクトも含まれています、 [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/FileHelper.cs)の使用を容易にするクラス、`DependencyService`オブジェクト。

これらのライブラリを使用するのには、アプリケーションのソリューションが内のすべてのプロジェクトを含める必要があります、 **Xamarin.FormsBook.Platform**ソリューション、および各アプリケーション プロジェクトに対応するライブラリへの参照をいる必要があります**Xamarin.FormsBook.Platform**です。

[ **TextFileAsync** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileAsync)ソリューションを使用する方法を示しています、 **Xamarin.FormsBook.Platform**ライブラリです。 各プロジェクトへの呼び出しには`Toolkit.Init`します。 アプリケーションが、非同期ファイル I/O を使用する関数。

### <a name="keeping-it-in-the-background"></a>バック グラウンドで維持すること

複数の非同期メソッド & #x 2014; への呼び出しを構成するライブラリ内のメソッドなど、`WriteFileAsync`と`ReadFileASync`メソッドは、Windows ランタイムで[ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs)クラス & #x 2014; にできる効率よりもやや詳細を使用して、 [ `ConfigureAwait` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task%3CTResult%3E.ConfigureAwait/p/System.Boolean/)メソッドユーザー インターフェイス スレッドへ切り替えしないでください。

### <a name="dont-block-the-ui-thread"></a>UI スレッドをブロックしません。

使用を回避する動かされることもあります`ContinueWith`または`await`を使用して、 [ `Result` ](https://developer.xamarin.com/api/property/System.Threading.Tasks.Task%3CTResult%3E.Result/)プロパティ、メソッドにします。 UI スレッドをブロックすることも、アプリケーションをハングものこれを回避する必要があります。

## <a name="your-own-awaitable-methods"></a>独自の待機可能メソッド

一部のコードを非同期的に実行するにはのいずれかに渡すことによって、 [ `Task.Run` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.Run/p/System.Action/)メソッドです。 呼び出すことができます`Task.Run`オーバーヘッドの一部を処理する非同期メソッド内で。

さまざまな`Task.Run`パターンを次に説明します。

### <a name="the-basic-mandelbrot-set"></a>基本的なマンデルブロ設定

リアルタイムで設定マンデルブロを描画する、 [ **Xamarin.Forms.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリには、 [ `Complex` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/Complex.cs)内と同様の構造、 `System.Numerics`名前空間です。

[ **MandelbrotSet** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotSet)サンプルには、`CalculateMandeblotAsync`基本白黒マンデルブロ セットの計算を使用してその分離コード ファイル内のメソッド[ `BmpMaker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs)ビットマップに配置します。

### <a name="marking-progress"></a>進行状況をマークします。

進行状況を報告する非同期メソッドからインスタンス化することができます、 [ `Progress<T>` ](https://developer.xamarin.com/api/type/System.Progress%3CT%3E/)クラスし、非同期メソッドの型の引数を定義する[ `IProgress<T>`](https://developer.xamarin.com/api/type/System.IProgress%3CT%3E/)です。 これに示されている、 [ **MandelbrotProgress** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotProgress)サンプルです。

### <a name="cancelling-the-job"></a>ジョブを取り消しています

キャンセル可能である非同期メソッドを記述することもできます。 という名前のクラスから始める[ `CancellationTokenSource`](https://developer.xamarin.com/api/type/System.Threading.CancellationTokenSource/)です。 [ `Token` ](https://developer.xamarin.com/api/property/System.Threading.CancellationTokenSource.Token/)プロパティ型の値は、 [ `CancellationToken`](https://developer.xamarin.com/api/type/System.Threading.CancellationToken/)です。 これは、非同期関数に渡されます。 プログラムが呼び出す、 [ `Cancel` ](https://developer.xamarin.com/api/member/System.Threading.CancellationTokenSource.Cancel()/)メソッドの`CancellationTokenSource`(一般的に、ユーザーがアクションに応答) 非同期関数をキャンセルします。

非同期のメソッドは定期的にチェックできます、 [ `IsCancellationRequested` ](https://developer.xamarin.com/api/property/System.Threading.CancellationToken.IsCancellationRequested/)プロパティの`CancellationToken`プロパティの場合を終了し、 `true`、または単に呼び出す、 [ `ThrowIfCancellationRequested` ](https://developer.xamarin.com/api/member/System.Threading.CancellationToken.ThrowIfCancellationRequested()/)メソッドで、終わる場合、メソッド、 [ `OperationCancelledException`](https://developer.xamarin.com/api/type/System.OperationCanceledException/)です。

[ **MandelbrotCancellation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotCancellation)サンプルでは、キャンセル可能な関数を使用します。

### <a name="an-mvvm-mandelbrot"></a>MVVM マンデルブロ

[ **MandelbrotXF** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotXF)サンプルより広範なユーザー インターフェイスと、これはほとんどの場合に基づいて、 [ `MandelbrotModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotModel.cs)と[ `MandelbrotViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotViewModel.cs)クラス。

[![マンデルブロ X F のトリプル スクリーン ショット](images/ch20fg13-small.png "MVVM マンデルブロ")](images/ch20fg13-large.png#lightbox "MVVM マンデルブロ")

## <a name="back-to-the-web"></a>Web に戻る

[ `WebRequest` ](https://developer.xamarin.com/api/type/System.Net.WebRequest/)一部のサンプルで使用されるクラスが非同期プログラミング モデルまたは APM と呼ばれる従来の非同期プロトコルを使用します。 このようなクラスのいずれかを使用して最新の TAP プロトコルに変換することができます、`FromAsync`内のメソッド、 [ `TaskFactory` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.TaskFactory%3CTResult%3E/)クラス。 [ **ApmToTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/ApmToTap)サンプルを示します。



## <a name="related-links"></a>関連リンク

- [20 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf)
- [20 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20)
- [ファイルの処理](~/xamarin-forms/app-fundamentals/files.md)
