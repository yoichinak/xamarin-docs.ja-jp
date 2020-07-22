---
title: '第 20 章の概要: 非同期およびファイル I/O'
description: 'Xamarin.Forms でモバイル アプリを作成する: 第 20 章の概要: 非同期およびファイル I/O'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D595862D-64FD-4C0D-B0AD-C1F440564247
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ad71dc5f5389f1676698a761a138b3f76ffa9fa0
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136683"
---
# <a name="summary-of-chapter-20-async-and-file-io"></a>第 20 章の概要: 非同期およびファイル I/O

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20)

> [!NOTE] 
> このページの注記では、Xamarin.Forms が本に記載されている資料と異なる部分が示されています。

 グラフィカル ユーザー インターフェイスは、ユーザー入力イベントに順番に応答する必要があります。 これは、ユーザー入力イベントのすべての処理が、1 つのスレッド ("*メイン スレッド*" または "*UI スレッド*" と呼ばれることが多い) 内で発生する必要があることを示しています。

ユーザーは、グラフィカル ユーザー インターフェイスがレスポンシブであることを期待しています。 これは、プログラムで迅速にユーザー入力イベントを処理する必要があることを意味しています。 これが不可能な場合、処理は実行のセカンダリ スレッドに回される必要があります。

この書籍に含まれているいくつかのサンプル プログラムでは、[`WebRequest`](xref:System.Net.WebRequest) クラスが使用されています。 このクラスでは、[`BeginGetResponse`](xref:System.Net.WebRequest.BeginGetResponse(System.AsyncCallback,System.Object)) メソッドによってワーカー スレッドが開始されます。これが完了すると、コールバック関数が呼び出されます。 ただし、そのコールバック関数はワーカー スレッドで実行されるため、プログラムではユーザー インターフェイスにアクセスするために、[`Device.BeginInvokeOnMainThread`](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)) メソッドを呼び出す必要があります。

> [!NOTE]
> Xamarin.Forms プログラムでは、インターネット経由でファイルにアクセスするために、[`WebRequest`](xref:System.Net.WebRequest) ではなく [`HttpClient`](xref:System.Net.Http.HttpClient) を使用する必要があります。 `HttpClient` では非同期操作がサポートされています。

.NET および C# では、非同期処理に対するより新しいアプローチを使用できます。 そこでは、[`Task`](xref:System.Threading.Tasks.Task) および [`Task<TResult>`](xref:System.Threading.Tasks.Task`1) クラス、[`System.Threading`](xref:System.Threading) および [`System.Threading.Tasks`](xref:System.Threading.Tasks) 名前空間のその他の型、C# 5.0 の `async` および `await` キーワードが使用されます。 この章では、これに焦点を当てます。

## <a name="from-callbacks-to-await"></a>コールバックから await まで

`Page` クラス自体に、アラート ボックスを表示するための 3 つの非同期メソッドが含まれています。

- [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)): `Task` オブジェクトを返します
- [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String,System.String)): `Task<bool>` オブジェクトを返します
- [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])): `Task<string>` オブジェクトを返します

`Task` オブジェクトは、これらのメソッドでタスク ベースの非同期パターン (TAP と呼ばれます) が実装されていることを示します。 これらの `Task` オブジェクトは、メソッドからすぐに返されます。 戻り値 `Task<T>` は、タスクの完了時に `TResult` 型の値を使用できるようになる "約束" を構成します。 戻り値 `Task` は非同期アクションを示します。これは完了しても、値は返されません。

これらのケースすべてにおいて、`Task` は、ユーザーがアラート ボックスを閉じたときに完了します。  

### <a name="an-alert-with-callbacks"></a>コールバックを使用したアラート

[**AlertCallbacks**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertCallbacks) サンプルは、`Task<bool>` の戻り値オブジェクトと `Device.BeginInvokeOnMainThread` 呼び出しを、コールバック メソッドを使用して処理する方法を示しています。

### <a name="an-alert-with-lambdas"></a>ラムダを使用したアラート

[**AlertLambdas**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertLambdas) サンプルは、`Task` と `Device.BeginInvokeOnMainThread` の呼び出しを処理するために、匿名のラムダ関数を使用する方法を示しています。  

### <a name="an-alert-with-await"></a>await を使用したアラート

よりわかりやすい方法は、C# 5 で導入された `async` および `await` キーワードを使用するものです。 [**AlertAwait**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertAwait) サンプルは、それらの使用方法を示しています。

### <a name="an-alert-with-nothing"></a>何も使用しないアラート

非同期メソッドが `Task<TResult>` ではなく `Task` を返した場合、プログラムで非同期タスクがいつ完了するかを知る必要がなければ、これらの手法を使用する必要はありません。 [**NothingAlert**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/NothingAlert) サンプルはこのケースを示しています。

### <a name="saving-program-settings-asynchronously"></a>プログラムの設定を非同期に保存する

[**SaveProgramChanges**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/SaveProgramSettings) サンプルは、`Application` の [`SavePropertiesAsync`](xref:Xamarin.Forms.Application.SavePropertiesAsync) メソッドを使用して、`OnSleep` メソッドをオーバーライドせずに、プログラムの設定が変更されたときにその設定を保存する方法を示しています。

### <a name="a-platform-independent-timer"></a>プラットフォームに依存しないタイマー

[`Task.Delay`](xref:System.Threading.Tasks.Task.Delay(System.Int32)) を使用して、プラットフォームに依存しないタイマーを作成することができます。 [**TaskDelayClock**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TaskDelayClock) サンプルはこの方法を示しています。

## <a name="file-inputoutput"></a>ファイルの入出力

従来は、.NET の [`System.IO`](xref:System.IO) 名前空間がファイル I/O のサポートのソースでした。 この名前空間の一部のメソッドでは非同期操作がサポートされていますが、ほとんどのメソッドではサポートされていません。 この名前空間では、高度なファイル I/O 関数を実行する、いくつかのシンプルなメソッド呼び出しもサポートされています。

### <a name="good-news-and-bad-news"></a>良いニュースと悪いニュース

Xamarin.Forms でサポートされているすべてのプラットフォームでは、アプリケーションのローカル ストレージ、すなわち、アプリケーション専用のストレージがサポートされています。

Xamarin.iOS と Xamarin.Android のライブラリには、これらの 2 つのプラットフォーム向けに Xamarin によって特別に調整されたバージョンの .NET が含まれています。 これらには `System.IO` のクラスが含まれています。これらを使用すると、これら 2 つのプラットフォームでアプリケーションのローカル ストレージでのファイル I/O を実行できます。

しかし、Xamarin.Forms PCL でこれらの `System.IO` クラスを検索しても、見つかりません。 問題は、Microsoft によって、Windows ランタイム API のファイル I/O が完全に改良されたことです。 Windows 8.1、Windows Phone 8.1、およびユニバーサル Windows プラットフォームをターゲットとするプログラムでは、ファイル I/O に `System.IO` が使用されません。

つまり、[`DependencyService`](xref:Xamarin.Forms.DependencyService) を使用して (最初に「[**第 9 章: プラットフォーム固有の API 呼び出し**](chapter09.md)」で説明されています)、ファイル I/O を実装する必要があります。

> [!NOTE]
> ポータブル クラス ライブラリは .NET Standard 2.0 ライブラリに置き換えられています。 .NET Standard 2.0 では、すべての Xamarin.Forms プラットフォーム向けに [`System.IO`](xref:System.IO) 型がサポートされています。 ほとんどのファイル I/O タスクでは、`DependencyService` を使用する必要がなくなりました。 ファイル I/O に対するより新しいアプローチについては、「[Xamarin.Forms でのファイル処理](~/xamarin-forms/data-cloud/data/files.md)」をご覧ください。

### <a name="a-first-shot-at-cross-platform-file-io"></a>最初のクロスプラットフォーム ファイル I/O

[**TextFileTryout**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileTryout) サンプルでは、ファイル I/O 用の [`IFileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/TextFileTryout/TextFileTryout/TextFileTryout/IFileHelper.cs) インターフェイスと、すべてのプラットフォームでのこのインターフェイスの実装が定義されています。 しかし、Windows ランタイムの実装を、このインターフェイスのメソッドと連動させることはできません。Windows ランタイムのファイル I/O メソッドは非同期であるためです。

### <a name="accommodating-windows-runtime-file-io"></a>Windows ランタイムのファイル I/O に対応する

Windows ランタイムの下で実行されているプログラムでは、ファイル I/O 用に [`Windows.Storage`](/uwp/api/Windows.Storage) および [`Windows.Storage.Streams`](/uwp/api/Windows.Storage.Streams) 名前空間のクラスが使用されます。これにはアプリケーションのローカル ストレージが含まれます。 UI スレッドのブロックを防止するために、Microsoft によって、50 ミリ秒を超える処理を必要とする操作はすべて非同期にすることが決められています。そのため、これらのファイル I/O メソッドはほとんどが非同期です。

この新しいアプローチを示すコードは、他のアプリケーションで使用できるようにライブラリに追加されます。

## <a name="platform-specific-libraries"></a>プラットフォーム固有のライブラリ

再利用可能なコードをライブラリに保存しておくと便利です。 これは当然、再利用可能なコードのさまざまな部分がまったく異なるオペレーティング システム用である場合、より困難になります。

[**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) ソリューションは、1 つの方法を示しています。 このソリューションには、7 つの異なるプロジェクトが含まれています。

- [**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform) (通常の Xamarin.Forms PCL)
- [**Xamarin.FormsBook.Platform.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS) (iOS クラス ライブラリ)
- [**Xamarin.FormsBook.Platform.Android**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android) (Android クラス ライブラリ)
- [**Xamarin.FormsBook.Platform.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.UWP) (ユニバーサル Windows クラス ライブラリ)
- [**Xamarin.FormsBook.Platform.WinRT**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT) (すべての Windows プラットフォームに共通するコード用の共有プロジェクト)

すべての個別のプラットフォーム用のプロジェクト (**Xamarin.FormsBook.Platform.WinRT** を除く) には、**Xamarin.FormsBook.Platform** への参照が含まれています。 3 つの Windows プロジェクトには、**Xamarin.FormsBook.Platform.WinRT** への参照が含まれています。

ライブラリが、Xamarin.Forms アプリケーション ソリューション内のプロジェクトによって直接参照されていない場合に読み込まれるように、すべてのプロジェクトには静的な `Toolkit.Init` メソッドが含まれています。

**Xamarin.FormsBook.Platform** プロジェクトには、新しい [`IFileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/IFileHelper.cs) インターフェイスが含まれています。 すべてのメソッドの名前に `Async` サフィックスが付けられ、`Task` オブジェクトが返されるようになりました。

**Xamarin.FormsBook.Platform.WinRT** プロジェクトには、Windows ランタイム用の [`FileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) クラスが含まれています。

**Xamarin.FormsBook.Platform.iOS** プロジェクトには、iOS 用の [`FileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/FileHelper.cs) クラスが含まれています。 これらのメソッドは非同期になっています。 一部のメソッドでは、`StreamWriter` と `StreamReader` で定義されているメソッドの非同期バージョン: [`WriteAsync`](xref:System.IO.StreamWriter.WriteAsync(System.String)) と [`ReadToEndAsync`](xref:System.IO.StreamReader.ReadToEndAsync) が使用されています。 その他では、[`FromResult`](xref:System.Threading.Tasks.Task.FromResult*) メソッドを使用して、結果が `Task` オブジェクトに変換されます。

**Xamarin.FormsBook.Platform.Android** プロジェクトには、Android 用の類似した [`FileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/FileHelper.cs) クラスが含まれています。

**Xamarin.FormsBook.Platform** プロジェクトには、`DependencyService` オブジェクトの使用を容易にする [`FileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/FileHelper.cs) クラスも含まれています。

これらのライブラリを使用するには、アプリケーション ソリューションに **Xamarin.FormsBook.Platform** ソリューション内のすべてのプロジェクトを含め、また各アプリケーション プロジェクトに **Xamarin.FormsBook.Platform** 内の対応するライブラリへの参照を含める必要があります。

[**TextFileAsync**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileAsync) ソリューションは、**Xamarin.FormsBook.Platform** ライブラリの使用方法を示しています。 各プロジェクトには `Toolkit.Init` の呼び出しが含まれています。 アプリケーションでは、非同期ファイル I/O 関数が使用されます。

### <a name="keeping-it-in-the-background"></a>これをバックグラウンドに保持する

複数の非同期メソッドの呼び出しを行うライブラリ内のメソッド (Windows ランタイム [`FileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) クラスの `WriteFileAsync` メソッドや `ReadFileASync` メソッドなど) は、[`ConfigureAwait`](xref:System.Threading.Tasks.Task`1.ConfigureAwait(System.Boolean)) メソッドを使用してユーザー インターフェイス スレッドへの切り替えを回避することで、多少効率を上げることができます。

### <a name="dont-block-the-ui-thread"></a>UI スレッドをブロックしない

場合によっては、メソッドの [`Result`](xref:System.Threading.Tasks.Task`1.Result) プロパティを使用して、`ContinueWith` や `await` の使用を回避したくなることがあります。 これは、UI スレッドをブロックしたり、アプリケーションをハングさせたりする可能性があるため、行わないでください。

## <a name="your-own-awaitable-methods"></a>独自の待機可能メソッド

一部のコードを非同期に実行するには、それを [`Task.Run`](xref:System.Threading.Tasks.Task.Run(System.Action)) メソッドのいずれかに渡します。 オーバーヘッドの一部を処理する非同期メソッド内で、`Task.Run` を呼び出すことができます。

以下では、`Task.Run` のさまざまなパターンについて説明します。

### <a name="the-basic-mandelbrot-set"></a>基本的なマンデルブロ集合

マンデルブロ集合をリアルタイムで描画するために、[ **Xamarin.Forms.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ライブラリには、`System.Numerics` 名前空間に含まれているものと同様の [`Complex`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/Complex.cs) 構造が含まれています。

[**MandelbrotSet**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotSet) サンプルでは、分離コード ファイルに `CalculateMandeblotAsync` メソッドが含まれています。これによって基本的な黒と白のマンデルブロ集合が計算され、[`BmpMaker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs) を使用してビットマップに配置されます。

### <a name="marking-progress"></a>進行状況をマークする

非同期メソッドから進行状況を報告するには、[`Progress<T>`](xref:System.Progress`1) クラスのインスタンスを作成し、[`IProgress<T>`](xref:System.IProgress`1) 型の引数を持つように非同期メソッドを定義することができます。 これは [**MandelbrotProgress**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotProgress) サンプルで示されています。

### <a name="cancelling-the-job"></a>ジョブの取り消し

取り消し可能な非同期メソッドを作成することもできます。 まず [`CancellationTokenSource`](xref:System.Threading.CancellationTokenSource) という名前のクラスから開始します。 [`Token`](xref:System.Threading.CancellationTokenSource.Token) プロパティは [`CancellationToken`](xref:System.Threading.CancellationToken) 型の値です。 これは、非同期関数に渡されます。 プログラムでは、非同期関数を取り消すために、(通常はユーザーのアクションに応答して) `CancellationTokenSource` の [`Cancel`](xref:System.Threading.CancellationTokenSource.Cancel) メソッドが呼び出されます。

非同期メソッドでは、`CancellationToken` の [`IsCancellationRequested`](xref:System.Threading.CancellationToken.IsCancellationRequested) プロパティを定期的にチェックし、プロパティが `true` の場合に終了させることができます。または、単純に [`ThrowIfCancellationRequested`](xref:System.Threading.CancellationToken.ThrowIfCancellationRequested) メソッドを呼び出すことができます。この場合、メソッドは [`OperationCancelledException`](xref:System.OperationCanceledException) で終了します。

[**MandelbrotCancellation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotCancellation) サンプルは、取り消し可能な関数の使用方法を示しています。

### <a name="an-mvvm-mandelbrot"></a>MVVM のマンデルブロ

[**MandelbrotXF**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotXF) サンプルには、より豊富なユーザー インターフェイスが用意されています。これは、ほとんどの場合、[`MandelbrotModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotModel.cs) クラスと [`MandelbrotViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotViewModel.cs) クラスに基づいています。

[![Mandelbrot X F のトリプル スクリーンショット](images/ch20fg13-small.png "MVVM のマンデルブロ")](images/ch20fg13-large.png#lightbox "MVVM のマンデルブロ")

## <a name="back-to-the-web"></a>Web に戻る

一部のサンプルで使用されている [`WebRequest`](xref:System.Net.WebRequest) クラスでは、非同期プログラミング モデルまたは APM と呼ばれる、旧式の非同期プロトコルが使用されています。 [`TaskFactory`](xref:System.Threading.Tasks.TaskFactory`1) クラスの `FromAsync` メソッドのいずれかを使用して、このようなクラスを最新の TAP プロトコルに変換することができます。 [**ApmToTap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/ApmToTap) サンプルでは、この方法が示されています。

## <a name="related-links"></a>関連リンク

- [第 20 章の全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf)
- [第 20 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20)
- [ファイルの処理](~/xamarin-forms/data-cloud/data/files.md)
