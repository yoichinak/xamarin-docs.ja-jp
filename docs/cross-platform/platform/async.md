---
title: 非同期サポートの概要
description: このドキュメントでは、非同期コードの記述を容易にするために、C# 5 で導入された async および await を使用したプログラミングについて説明します。
ms.prod: xamarin
ms.assetid: F87BF587-AB64-4C60-84B1-184CAE36ED65
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 9821519c680de61767792c8122938bf826e457f5
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91453247"
---
# <a name="async-support-overview"></a>非同期サポートの概要

_C# 5 では、非同期プログラミングを簡略化するために、async と await の2つのキーワードが導入されました。これらのキーワードを使用すると、タスク並列ライブラリを利用して、別のスレッドで長時間実行される操作 (ネットワークアクセスなど) を実行し、完了時に結果に簡単にアクセスできる単純なコードを記述できます。最新バージョンの Xamarin. iOS および Xamarin. Android サポート async と await-このドキュメントでは、Xamarin で新しい構文を使用する方法について説明し、例を示します。_

Xamarin の非同期サポートは Mono 3.0 foundation 上に構築されており、API プロファイルをモバイル対応バージョンの Silverlight から .NET 4.5 のモバイル対応バージョンにアップグレードします。

## <a name="overview"></a>概要

このドキュメントでは、新しい async キーワードと await キーワードについて説明し、Xamarin と Xamarin の非同期メソッドを実装する簡単な例をいくつか紹介します。

C# 5 の新しい非同期機能 (多くのサンプルとさまざまな使用シナリオを含む) の詳細については、「 [非同期プログラミング](/dotnet/csharp/async)」を参照してください。

サンプルアプリケーションは、単純な非同期 web 要求 (メインスレッドをブロックせずに) を作成し、ダウンロードした html と文字数を使用して UI を更新します。

 [![サンプルアプリケーションは、メインスレッドをブロックせずに単純な非同期 web 要求を作成し、ダウンロードした html と文字数を使用して UI を更新します。](async-images/AsyncAwait_427x368.png)](async-images/AsyncAwait.png#lightbox)

Xamarin の非同期サポートは Mono 3.0 foundation 上に構築されており、API プロファイルをモバイル対応バージョンの Silverlight から .NET 4.5 のモバイル対応バージョンにアップグレードします。

## <a name="requirements"></a>必要条件

C# 5 の機能には、Xamarin に含まれる Mono 3.0 が必要です。 iOS 6.4 および Xamarin. Android 4.8。 この機能を利用するには、Mono、Xamarin、Xamarin、および Xamarin. Mac をアップグレードするように求められます。

## <a name="using-async-amp-await"></a>Async await の使用 &amp;

 `async` と `await` は、タスク並列ライブラリと連携して動作する新しい C# 言語機能で、アプリケーションのメインスレッドをブロックせずに、長時間実行されるタスクを実行するスレッドコードを簡単に記述できます。

## <a name="async"></a>async

### <a name="declaration"></a>宣言

キーワードは、 `async` 非同期的に実行できるコードが含まれていることを示すために (またはラムダまたは匿名メソッドに) 配置されます。呼び出し元のスレッドをブロックしません。

でマークされたメソッドに `async` は、await 式またはステートメントが少なくとも1つ含まれている必要があります。 `await`メソッドにステートメントが存在しない場合は、同期的に実行されます (修飾子がなかった場合と同じ `async` です)。 これにより、コンパイラの警告も発生します (ただし、エラーは発生しません)。

### <a name="return-types"></a>戻り値の型

非同期メソッドは `Task` 、、 `Task<TResult>` またはを返し `void` ます。

メソッドが `Task` 他の値を返さない場合は、戻り値の型を指定します。

`Task<TResult>`メソッドが値を返す必要があるかどうかを指定し `TResult` ます。は、返される型 ( `int` たとえば、など) です。

`void`戻り値の型は、主にそれを必要とするイベントハンドラーで使用されます。 Void を返す非同期メソッドを呼び出すコードは `await` 、結果に対しては実行できません。

### <a name="parameters"></a>パラメーター

非同期メソッドで `ref` は、またはパラメーターを宣言できません `out` 。

## <a name="await"></a>await

Await 演算子は、async とマークされたメソッド内のタスクに適用できます。 これにより、メソッドはその時点で実行を停止し、タスクが完了するまで待機します。

Await を使用しても、呼び出し元のスレッドがブロックされることはありません。呼び出し元に制御が返されます。 これは、呼び出し元のスレッドがブロックされないことを意味します。たとえば、タスクを待機しているときにユーザーインターフェイススレッドがブロックされることはありません。

タスクが完了すると、メソッドはコード内の同じ位置で実行を再開します。 これには、try-catch ブロック (存在する場合) の try スコープに戻ることが含まれます。 await を catch または finally ブロックで使用することはできません。

[Await](/dotnet/csharp/language-reference/keywords/await)の詳細については、Microsoft Docs を参照してください。

## <a name="exception-handling"></a>例外処理

非同期メソッド内で発生した例外はタスクに格納され、タスクが ed のときにスローされ `await` ます。 これらの例外は、try-catch ブロック内でキャッチして処理できます。

## <a name="cancellation"></a>キャンセル

完了までに長い時間がかかる非同期メソッドは、キャンセルをサポートする必要があります。 通常、キャンセルは次のように呼び出されます。

- `CancellationTokenSource`オブジェクトが作成されます。
- `CancellationTokenSource.Token`インスタンスは、キャンセル可能な非同期メソッドに渡されます。
- キャンセルは、メソッドを呼び出すことによって要求され `CancellationTokenSource.Cancel` ます。

その後、タスクが取り消され、取り消しが確認されます。

取り消しの詳細については、「[Fine Tuning Your Async Application (C#) (非同期アプリケーションの微調整 (C#))](/dotnet/csharp/programming-guide/concepts/async/fine-tuning-your-async-application)」を参照してください。

## <a name="example"></a>例

サンプルの [Xamarin ソリューション](/samples/xamarin/mobile-samples/asyncawait/) (IOS と Android の両方) をダウンロードして、 `async` mobile apps での実際の例を参照してください `await` 。 コード例については、このセクションで詳しく説明します。

### <a name="writing-an-async-method"></a>非同期メソッドの記述

次のメソッドは、ed タスクを使用してメソッドをコーディングする方法を示してい `async` `await` ます。

```csharp
public async Task<int> DownloadHomepage()
{
    var httpClient = new HttpClient(); // Xamarin supports HttpClient!

    Task<string> contentsTask = httpClient.GetStringAsync("https://visualstudio.microsoft.com/xamarin"); // async method!

    // await! control returns to the caller and the task continues to run on another thread
    string contents = await contentsTask;

    ResultEditText.Text += "DownloadHomepage method continues after async call. . . . .\n";

    // After contentTask completes, you can calculate the length of the string.
    int exampleInt = contents.Length;

    ResultEditText.Text += "Downloaded the html and found out the length.\n\n\n";

    ResultEditText.Text += contents; // just dump the entire HTML

    return exampleInt; // Task<TResult> returns an object of type TResult, in this case int
}
```

次の点に注意してください。

- メソッドの宣言には、キーワードが含まれてい  `async` ます。
- 戻り値の型は、  `Task<int>` 呼び出し元のコードが  `int` このメソッドで計算された値にアクセスできるようにするためです。
- Return ステートメントは整数オブジェクトであり、  `return exampleInt;` メソッドが返す事実は  `Task<int>` 言語の機能強化の一部です。

### <a name="calling-an-async-method-1"></a>非同期メソッドの呼び出し1

このボタンクリックイベントハンドラーは、上記で説明したメソッドを呼び出す Android サンプルアプリケーションにあります。

```csharp
GetButton.Click += async (sender, e) => {

    Task<int> sizeTask = DownloadHomepage();

    ResultTextView.Text = "loading...";
    ResultEditText.Text = "loading...\n";

    // await! control returns to the caller
    var intResult = await sizeTask;

    // when the Task<int> returns, the value is available and we can display on the UI
    ResultTextView.Text = "Length: " + intResult ;
    // "returns" void, since it's an event handler
};
```

メモ:

- 匿名デリゲートには、async キーワードプレフィックスがあります。
- 非同期メソッド DownloadHomepage は、 \<int> sizetask 変数に格納されているタスクを返します。
- このコードは、sizeTask 変数を待機しています。  *これ* は、メソッドが中断された場所であり、非同期タスクが独自のスレッドで終了するまで、呼び出し元のコードに制御が返されます。
- タスクが作成されているにもかかわらず、メソッドの最初の行にタスクが作成されても、実行は一時停止し  *ません* 。 Await キーワードは、実行が一時停止されている場所を示します。
- 非同期タスクが完了すると、intResult が設定され、await 行から元のスレッドで実行が続行されます。

### <a name="calling-an-async-method-2"></a>非同期メソッド2の呼び出し

IOS サンプルアプリケーションでは、別の方法を示すために、この例は少し異なる方法で記述されています。 この例では、匿名デリゲートを使用するのではなく、 `async` 通常のイベントハンドラーのように割り当てられたイベントハンドラーを宣言しています。

```csharp
GetButton.TouchUpInside += HandleTouchUpInside;
```

次に示すように、イベントハンドラーメソッドが定義されます。

```csharp
async void HandleTouchUpInside (object sender, EventArgs e)
{
    ResultLabel.Text = "loading...";
    ResultTextView.Text = "loading...\n";

    // await! control returns to the caller
    var intResult = await DownloadHomepage();

    // when the Task<int> returns, the value is available and we can display on the UI
    ResultLabel.Text = "Length: " + intResult ;
}
```

いくつかの重要な点:

- メソッドはとしてマークされ  `async` ますが、はを返し  `void` ます。 これは通常、イベントハンドラーに対してのみ実行されます (それ以外の場合  `Task` は、またはを返し  `Task<TResult>` ます)。
- メソッドのキーワードは、 `await`  `DownloadHomepage` `intResult` 前の例とは異なり、タスクを参照するために中間変数を使用した場合とは異なり、変数 () に直接割り当てられ  `Task<int>` ます。  *これ* は、非同期メソッドが別のスレッドで完了するまで、制御が呼び出し元に返される場所です。
- 非同期メソッドが完了して戻ると、で実行が再開されます  `await` 。これは、整数の結果が返され、UI ウィジェットで表示されることを意味します。

## <a name="summary"></a>まとめ

Async と await を使用すると、メインスレッドをブロックせずに、バックグラウンドスレッドで長時間実行される操作を生成するために必要なコードを大幅に簡略化できます。 また、タスクの完了時に結果に簡単にアクセスすることもできます。

このドキュメントでは、Xamarin と Xamarin Android の新しい言語キーワードと例の概要について説明しました。

## <a name="related-links"></a>関連リンク

- [AsyncAwait (サンプル)](/samples/xamarin/mobile-samples/asyncawait/)
- ["Generation To ステートメント" としてのコールバック](https://tirania.org/blog/archive/2013/Aug-15.html)
- [データ (iOS) (サンプル)](/samples/xamarin/ios-samples/data/)
- [HttpClient (iOS) (サンプル)](/samples/xamarin/ios-samples/httpclient/)
- [MapKitSearch (iOS) (サンプル)](https://github.com/xamarin/monotouch-samples/tree/master/MapKitSearch)
- [非同期プログラミング](/dotnet/csharp/async)
- [非同期アプリケーションの微調整 (C#)](/dotnet/csharp/programming-guide/concepts/async/fine-tuning-your-async-application)
- [Await、UI、およびデッドロック。あらららら！](https://devblogs.microsoft.com/pfxteam/await-and-ui-and-deadlocks-oh-my/)
- [タスクの完了時の処理](https://devblogs.microsoft.com/pfxteam/processing-tasks-as-they-complete/)
- [タスク ベースの非同期パターン (TAP)](/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)
- [C# 5 の非同期性 (Eric Lippert のブログ) –キーワードの概要について](/archive/blogs/ericlippert/asynchrony-in-c-5-part-six-whither-async)