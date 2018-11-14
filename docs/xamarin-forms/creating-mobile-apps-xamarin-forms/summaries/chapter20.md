---
title: 第 20 章の概要です。 非同期およびファイル I/O
description: 'Xamarin.Forms によるモバイル アプリの作成: 第 20 章の概要。 非同期およびファイル I/O'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D595862D-64FD-4C0D-B0AD-C1F440564247
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: a795b382b9bcc727b0b0872d29d30a501cfed0a6
ms.sourcegitcommit: f3f28722198e172d81c16bdeab0cb0a581a08dd0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/13/2018
ms.locfileid: "51598900"
---
# <a name="summary-of-chapter-20-async-and-file-io"></a>第 20 章の概要です。 非同期およびファイル I/O

> [!NOTE] 
> このページに関する注意事項は、この本で説明されている内容が Xamarin.Forms が異なっている領域を示しています。

 グラフィカル ユーザー インターフェイスは、順番にユーザー入力イベントに応答する必要があります。 つまり、ユーザー入力イベントの処理はすべてが、多くの場合と呼ばれる 1 つのスレッドで発生しなければならない、*メイン スレッド*または*UI スレッド*します。

ユーザーは、グラフィカル ユーザー インターフェイスが応答を期待します。 つまり、プログラムの処理でユーザー入力イベントがすばやく処理する必要があります。 方法が使用できない場合にセカンダリ スレッド処理を移行する必要があります。

この本でいくつかのサンプル プログラムが使用される、 [ `WebRequest` ](xref:System.Net.WebRequest)クラス。 このクラスで、 [ `BeginGetResponse` ](xref:System.Net.WebRequest.BeginGetResponse(System.AsyncCallback,System.Object))メソッドは、これが完了すると、コールバック関数を呼び出すワーカー スレッドを開始します。 ただし、そのコールバック関数で実行されるワーカー スレッド、ため、プログラムを呼び出す必要があります[ `Device.BeginInvokeOnMainThread` ](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action))ユーザー インターフェイスにアクセスするメソッド。

> [!NOTE]
> Xamarin.Forms のプログラムを使用する必要があります[ `HttpClient` ](xref:System.Net.Http.HttpClient)なく[ `WebRequest` ](xref:System.Net.WebRequest)インターネット経由でファイルにアクセスするためです。 `HttpClient` 非同期操作をサポートしています。

非同期処理するための最新のアプローチは .NET と c# で使用できます。 これは、ためには、 [ `Task` ](xref:System.Threading.Tasks.Task)と[ `Task<TResult>` ](xref:System.Threading.Tasks.Task`1)クラス、およびその他の種類で、 [ `System.Threading` ](xref:System.Threading)と[ `System.Threading.Tasks` ](xref:System.Threading.Tasks) 、名前空間だけでなくC#5.0`async`と`await`キーワード。 この章の説明です。

## <a name="from-callbacks-to-await"></a>Await にコールバックから

`Page`クラス自体には、警告ボックスを表示する 3 つの非同期メソッドが含まれています。

- [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) 返します、`Task`オブジェクト
- [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String,System.String)) 返します、`Task<bool>`オブジェクト
- [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])) 返します、`Task<string>`オブジェクト

`Task`オブジェクトは、これらのメソッドがタップと呼ばれる、タスクベースの非同期パターンを実装することを示します。 これら`Task`オブジェクトは、メソッドからすぐに返されます。 `Task<T>`値の構成を「約束」を返す型の値`TResult`タスクの完了時に提供されます。 `Task`戻り値非同期アクションを完了しましたが、値はありませんが返されることになります。

これらすべてのケースで、`Task`が完了すると、ユーザーが、警告ボックスを閉じます。  

### <a name="an-alert-with-callbacks"></a>コールバックのアラート

[ **AlertCallbacks** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertCallbacks)サンプルを処理する方法を示します`Task<bool>`オブジェクトを返すと`Device.BeginInvokeOnMainThread`呼び出しコールバック メソッドを使用します。

### <a name="an-alert-with-lambdas"></a>ラムダのアラート

[ **AlertLambdas** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertLambdas)サンプル匿名のラムダ関数の処理を使用する方法を示します`Task`と`Device.BeginInvokeOnMainThread`呼び出し。  

### <a name="an-alert-with-await"></a>Await のアラート

簡単なアプローチでは、`async`と`await`c# 5 で導入されるキーワード。 [ **AlertAwait** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertAwait)サンプルでは、それらを使用します。

### <a name="an-alert-with-nothing"></a>何もアラート

非同期のメソッドを返す場合`Task`なく`Task<TResult>`プログラムは、非同期タスクが完了したときを知る必要がある場合は、これらの手法のいずれかを使用する必要があります。 [ **NothingAlert** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/NothingAlert)のサンプルで例示します。

### <a name="saving-program-settings-asynchronously"></a>プログラム設定を保存して非同期的に

[ **SaveProgramChanges** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/SaveProgramSettings)サンプルの使用、 [ `SavePropertiesAsync` ](xref:Xamarin.Forms.Application.SavePropertiesAsync)メソッドの`Application`せず、変更時に、プログラムの設定を保存するにはオーバーライドする、`OnSleep`メソッド。

### <a name="a-platform-independent-timer"></a>プラットフォームに依存しないタイマー

使用することは[ `Task.Delay` ](xref:System.Threading.Tasks.Task.Delay(System.Int32))プラットフォームに依存しないタイマーを作成します。 [ **TaskDelayClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TaskDelayClock)のサンプルで例示します。

## <a name="file-inputoutput"></a>ファイルの入力/出力

これまでは、.NET [ `System.IO` ](xref:System.IO)名前空間は、ソースのファイル I/O のサポートされています。 この名前空間の一部のメソッドは、非同期操作をサポート、ほとんどは必要ありません。 名前空間には、高度なファイル I/O 機能を実行するいくつかの単純なメソッド呼び出しもサポートしています。

### <a name="good-news-and-bad-news"></a>良い知らせと悪い知らせ

Xamarin.Forms のサポートのアプリケーションのローカル ストレージでサポートされているすべてのプラットフォーム&mdash;は、アプリケーションをプライベート ストレージ。

Xamarin.iOS および Xamarin.Android ライブラリには、これら 2 つのプラットフォームの Xamarin が明示的に対応した .NET のバージョンが含まれます。 クラスが含まれます`System.IO`これら 2 つのプラットフォームでアプリケーションのローカル記憶域のファイル I/O を実行することを行えます。

ただし、これらを検索する場合`System.IO`Xamarin.Forms PCL にクラスがありませんにします。 問題は、その Microsoft が完全に一新ファイル I/O、Windows ランタイム API です。 Windows 8.1、Windows Phone 8.1、およびユニバーサル Windows プラットフォームを対象とするプログラムは使用しないでください`System.IO`ファイル I/O 用です。

つまり、使用する必要があります、 [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) (に説明した[**第 9 章です。プラットフォーム固有の API 呼び出し**](chapter09.md)ファイル I/O を実装します。

> [!NOTE]
> ポータブル クラス ライブラリは、.NET Standard 2.0 ライブラリ、置き換えられましたが、.NET Standard 2.0 をサポートしています[ `System.IO` ](xref:System.IO)のすべての Xamarin.Forms プラットフォームの種類。 使用する必要ができなくなった、`DependencyService`のほとんどのファイル I/O タスク。 参照してください[Xamarin.Forms でのファイル処理](~/xamarin-forms/app-fundamentals/files.md)ファイル I/O には最新のアプローチです。

### <a name="a-first-shot-at-cross-platform-file-io"></a>クロス プラットフォーム ファイル I/O の最初のショット

[ **TextFileTryout** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileTryout)サンプルを定義、 [ `IFileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/TextFileTryout/TextFileTryout/TextFileTryout/IFileHelper.cs)ファイル I/O、およびすべてのプラットフォームでは、このインターフェイスの実装のためのインターフェイス。 ただし、Windows ランタイムの実装は、Windows ランタイムのファイルの I/O メソッドは非同期であるために、このインターフェイスのメソッドで機能しません。

### <a name="accommodating-windows-runtime-file-io"></a>Windows ランタイムのファイル I/O を考慮に入れるため

Windows ランタイムで実行されるプログラム クラスを使用して、 [ `Windows.Storage` ](/uwp/api/Windows.Storage)と[ `Windows.Storage.Streams` ](/uwp/api/Windows.Storage.Streams)ファイル I/O、アプリケーションのローカル記憶域を含む名前空間。 Microsoft では、50 を超える (ミリ秒) を UI スレッドがブロックされないようにするために非同期にする必要がありますが必要なすべての操作を特定、ため、これらのファイル I/O メソッドは非同期ほとんどの場合です。

他のアプリケーションで使用できるように、この新しいアプローチを示すコードは、ライブラリになります。

## <a name="platform-specific-libraries"></a>プラットフォーム固有のライブラリ

ライブラリの再利用可能なコードを格納することをお勧めします。 これは、機能は、まったく別のオペレーティング システムの再利用可能なコードの異なる部分が明らかに困難です。

[ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform)ソリューションは 1 つの方法を示します。 このソリューションには、7 つの異なるプロジェクトが含まれています。

- [**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform)、通常、Xamarin.Forms PCL
- [**Xamarin.FormsBook.Platform.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS)iOS のクラス ライブラリ
- [**Xamarin.FormsBook.Platform.Android**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android), an Android class library
- [**Xamarin.FormsBook.Platform.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.UWP)、ユニバーサル Windows クラス ライブラリ
- [**Xamarin.FormsBook.Platform.WinRT**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT)、コードでは、すべての Windows プラットフォームに共通の共有プロジェクト

すべての個別のプラットフォーム プロジェクト (例外として**Xamarin.FormsBook.Platform.WinRT**) への参照がある**Xamarin.FormsBook.Platform**します。 3 つの Windows プロジェクトへの参照がある**Xamarin.FormsBook.Platform.WinRT**します。

すべてのプロジェクトでは、静的な`Toolkit.Init`メソッドを直接ではなく Xamarin.Forms アプリケーションのソリューション内のプロジェクトによって参照されている場合、ライブラリが読み込まれていることを確認します。

**Xamarin.FormsBook.Platform**プロジェクトが含まれていますが、新しい[ `IFileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/IFileHelper.cs)インターフェイス。 すべてのメソッドを持つ名前ある`Async`サフィックスと戻り値`Task`オブジェクト。

**Xamarin.FormsBook.Platform.WinRT**プロジェクトが含まれています、 [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) for Windows Runtime クラス。

**Xamarin.FormsBook.Platform.iOS**プロジェクトが含まれています、 [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/FileHelper.cs) iOS 用のクラス。 これらのメソッドは非同期になります。 一部のメソッドで定義されているメソッドの非同期バージョンを使用して、`StreamWriter`と`StreamReader`: [ `WriteAsync` ](xref:System.IO.StreamWriter.WriteAsync(System.String))と[ `ReadToEndAsync`](xref:System.IO.StreamReader.ReadToEndAsync)します。 他のユーザーの変換結果を`Task`オブジェクトを使用して、 [ `FromResult` ](xref:System.Threading.Tasks.Task.FromResult*)メソッド。

**Xamarin.FormsBook.Platform.Android**プロジェクトが含まれていますが、類似[ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/FileHelper.cs) for Android のクラス。

**Xamarin.FormsBook.Platform**プロジェクトも含まれています、 [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/FileHelper.cs)クラスの使用を容易にする、`DependencyService`オブジェクト。

これらのライブラリを使用するアプリケーションのソリューションが内のすべてのプロジェクトを含める必要があります、 **Xamarin.FormsBook.Platform**ソリューション、および各アプリケーション プロジェクトに対応するライブラリへの参照をいる必要があります**Xamarin.FormsBook.Platform**します。

[ **TextFileAsync** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileAsync)ソリューションが使用する方法を示します、 **Xamarin.FormsBook.Platform**ライブラリ。 呼び出しが、プロジェクトの各`Toolkit.Init`します。 非同期ファイル I/O の使用は、アプリケーションは機能します。

### <a name="keeping-it-in-the-background"></a>バック グラウンドで維持します。

複数の非同期メソッドの呼び出しを行うライブラリのメソッド&mdash;など、`WriteFileAsync`と`ReadFileASync`Windows ランタイム メソッド[ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs)クラス&mdash;いくらか行んだことができます使用してより効率的な[ `ConfigureAwait` ](xref:System.Threading.Tasks.Task`1.ConfigureAwait(System.Boolean))ユーザー インターフェイス スレッドへの切り替えを回避するためです。

### <a name="dont-block-the-ui-thread"></a>UI スレッドをブロックしないでください。

使用を回避する場合があります`ContinueWith`または`await`を使用して、 [ `Result` ](xref:System.Threading.Tasks.Task`1.Result)プロパティ、メソッドにします。 これは、UI スレッドをブロック、でも、アプリケーションのハングを避ける必要があります。

## <a name="your-own-awaitable-methods"></a>待機可能なメソッド

いくつかのコードを非同期的に実行するにはのいずれかに渡すことによって、 [ `Task.Run` ](xref:System.Threading.Tasks.Task.Run(System.Action))メソッド。 呼び出すことができます`Task.Run`オーバーヘッドの一部を処理する非同期メソッド内で。

さまざまな`Task.Run`パターンが以下で説明します。

### <a name="the-basic-mandelbrot-set"></a>基本的なマンデルブロ集合

マンデルブロ集合をリアルタイムに描画するために、 [ **Xamarin.Forms.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリには、 [ `Complex` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/Complex.cs)構造でのような`System.Numerics`名前空間。

[ **MandelbrotSet** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotSet)サンプルでは、`CalculateMandeblotAsync`メソッドを使用して基本的なモノクロ マンデルブロ集合を計算しますその分離コード ファイルで[ `BmpMaker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs)。ビットマップに配置します。

### <a name="marking-progress"></a>進行状況をマークします。

非同期のメソッドからの進行状況を報告するインスタンス化することができます、 [ `Progress<T>` ](xref:System.Progress`1)クラスし、型の引数を使用して、非同期メソッドを定義する[ `IProgress<T>`](xref:System.IProgress`1)します。 これは、方法については、 [ **MandelbrotProgress** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotProgress)サンプル。

### <a name="cancelling-the-job"></a>ジョブを取り消しています

キャンセル可能な非同期メソッドを記述することもできます。 という名前のクラスを開始する[ `CancellationTokenSource`](xref:System.Threading.CancellationTokenSource)します。 [ `Token` ](xref:System.Threading.CancellationTokenSource.Token)プロパティ型の値は、 [ `CancellationToken`](xref:System.Threading.CancellationToken)します。 これは、非同期関数に渡されます。 プログラムが呼び出す、 [ `Cancel` ](xref:System.Threading.CancellationTokenSource.Cancel)メソッドの`CancellationTokenSource`(一般的に、ユーザーがアクションに応答) 非同期関数をキャンセルします。

非同期のメソッドを定期的に確認できる、 [ `IsCancellationRequested` ](xref:System.Threading.CancellationToken.IsCancellationRequested)プロパティの`CancellationToken`終了プロパティの場合と`true`を呼び出すだけで、または、 [ `ThrowIfCancellationRequested` ](xref:System.Threading.CancellationToken.ThrowIfCancellationRequested)メソッドで、終わる場合は、メソッド、 [ `OperationCancelledException`](xref:System.OperationCanceledException)します。

[ **MandelbrotCancellation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotCancellation)キャンセル可能な関数の使用を示します。

### <a name="an-mvvm-mandelbrot"></a>MVVM のマンデル ブロー

[ **MandelbrotXF** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotXF)サンプルより広範なユーザー インターフェイスと、これは主に基づいて、 [ `MandelbrotModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotModel.cs)と[ `MandelbrotViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotViewModel.cs)クラス。

[![マンデルブロ X F の 3 倍になるスクリーン ショット](images/ch20fg13-small.png "MVVM マンデルブロ")](images/ch20fg13-large.png#lightbox "MVVM マンデルブロ")

## <a name="back-to-the-web"></a>Web に戻る

[ `WebRequest` ](xref:System.Net.WebRequest)一部のサンプルで使用されるクラスは、非同期プログラミング モデル、または APM と呼ばれる昔ながらの非同期プロトコルを使用します。 このようなクラスのいずれかを使用して、最新のタップ プロトコルに変換できます、`FromAsync`メソッド、 [ `TaskFactory` ](xref:System.Threading.Tasks.TaskFactory`1)クラス。 [ **ApmToTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/ApmToTap)のサンプルで例示します。



## <a name="related-links"></a>関連リンク

- [第 20 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf)
- [第 20 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20)
- [ファイルの処理](~/xamarin-forms/app-fundamentals/files.md)
