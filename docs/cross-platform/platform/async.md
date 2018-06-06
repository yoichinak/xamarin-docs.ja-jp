---
title: 非同期サポートの概要
description: このドキュメントでは、非同期のプログラミングについて説明し、待機、非同期コードを記述するが容易に c# 5 で導入された概念。
ms.prod: xamarin
ms.assetid: F87BF587-AB64-4C60-84B1-184CAE36ED65
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 22878695d93ae79bbbfe1b99961587ff0bf957be
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782009"
---
# <a name="async-support-overview"></a>非同期サポートの概要

_導入された非同期プログラミングを簡略化する 2 つのキーワードの c# 5: async と await です。これらのキーワードでは、別のスレッドで実行時間の長い操作 (ネットワーク アクセスなど) を実行するには、タスク並列ライブラリを使用する単純なコードを記述し、完了時に結果を簡単にアクセスできます。Xamarin.iOS および Xamarin.Android の最新バージョン サポート async および await - 説明、および Xamarin を使用した新しい構文の使用例を説明します。_

Xamarin の非同期サポートは、Mono 3.0 をベースに構築され、モバイル フレンドリなバージョンの .NET 4.5 のモバイル対応バージョンで Silverlight の中から API プロファイルをアップグレードします。

## <a name="overview"></a>概要

このドキュメントは、新しい非同期を導入し、await キーワード、Xamarin.iOS および Xamarin.Android で非同期メソッドを実装する簡単な例ではについて説明します。

C# 5 (多くのサンプルとさまざまな使用シナリオを含む) の新しい非同期機能の詳細については、MSDN ドキュメントを参照してください[Async および Await を使用した非同期プログラミング](http://msdn.microsoft.com/library/vstudio/hh191443.aspx)です。

サンプル アプリケーションは、(メイン スレッドをブロックするには) なしで簡単な非同期 web 要求を実行し、ダウンロードした html と文字数で UI を更新します。

 [![](async-images/AsyncAwait_427x368.png "サンプル アプリケーションはメイン スレッドをブロックすることがなく、単純な非同期 web 要求を作成してからダウンロードした html と文字数で UI を更新")](async-images/AsyncAwait.png#lightbox)

Xamarin の非同期サポートは、Mono 3.0 をベースに構築され、モバイル フレンドリなバージョンの .NET 4.5 のモバイル対応バージョンで Silverlight の中から API プロファイルをアップグレードします。

## <a name="requirements"></a>必要条件

C# 5 機能では、Mono 3.0 6.4 の Xamarin.iOS と Xamarin.Android 4.8 に含まれている必要があります。 これを活用するために、モノラル、Xamarin.iOS、Xamarin.Android、Xamarin.Mac をアップグレードするように促されます。

## <a name="using-async-amp-await"></a>非同期を使用して&amp;await

 `async` および`await`c# 言語する新機能、アプリケーションのメイン スレッドをブロックすることがなく実行時間の長いタスクを実行するスレッドのコードを記述するが簡単に、タスク並列ライブラリと連携して動作します。

## <a name="async"></a>async

### <a name="declaration"></a>宣言

`async`メソッドの宣言で (またはラムダ式、または匿名メソッド) キーワードが配置されて、非同期的に実行できるコードが含まれることを示すために ie。 呼び出し元のスレッドをブロックします。

マークされたメソッド`async`少なくとも 1 つ含める必要があります式またはステートメントを待機します。 ない場合は`await`s 内にあるメソッドは同期的に実行し、(同じがあった場合にない`async`修飾子)。 これが、コンパイラの警告 (ただし、エラーではなく) でも発生します。

### <a name="return-types"></a>戻り値の型

非同期のメソッドが返す必要があります、 `Task`、`Task<TResult>`または`void`です。

指定して、`Task`メソッドがその他の値を返さない場合は、型を返します。

指定`Task<TResult>`メソッドは、値を返す必要がある場合、`TResult`は返される型です。 (など、`int`など)。

`void`戻り値の型が必要とするイベント ハンドラーの主に使用します。 Void を返す非同期メソッドを呼び出すコードをできません`await`結果にします。

### <a name="parameters"></a>パラメーター

非同期のメソッドを宣言できません`ref`または`out`パラメーター。

## <a name="await"></a>await

Await 演算子は、非同期としてマークされているメソッド内のタスクに適用できます。 メソッドをその時点で実行を停止し、タスクが完了するまで待機します。

Await を使用して –、呼び出し元のスレッドをブロックしませんではなく、呼び出し元に制御が戻ります。 つまり、たとえば、ユーザー インターフェイス スレッドはブロックされませんタスクを待機するときに、呼び出し元のスレッドがブロックされていないこと。

タスクが完了したら、メソッドは、コード内の同じポイントで実行を再開します。 これには、(存在する) 場合、try ブロックのスコープ、try – catch – finally を返すことが含まれます。 await finally ブロックまたは catch では使用できません。

詳細について[MSDN の await](http://msdn.microsoft.com/library/vstudio/hh156528.aspx)です。

## <a name="exception-handling"></a>例外処理

非同期メソッド内で発生する例外がタスクに格納されているし、タスクがスローされます`await`ed です。 これらの例外をキャッチされ、try-catch ブロック内で処理されることができます。

## <a name="cancellation"></a>キャンセル

完了に時間がかかるを非同期メソッドは、キャンセルをサポートする必要があります。 通常、キャンセルが次のように呼び出されます。

- A`CancellationTokenSource`オブジェクトを作成します。
- `CancellationTokenSource.Token`インスタンスがキャンセル可能な非同期メソッドに渡されます。
- 呼び出すことによって取り消しが要求されて、`CancellationTokenSource.Cancel`メソッドです。

タスクは次が取り消されたし、キャンセルを確認します。

取り消し処理の詳細については、次を参照してください。[非同期タスクを取り消す方法](http://msdn.microsoft.com/library/vstudio/jj155761.aspx)msdn です。

## <a name="example"></a>例

ダウンロード、[例 Xamarin ソリューション](https://developer.xamarin.com/samples/mobile/AsyncAwait/)(iOS および Android の両方) 用の実際の例を表示する`async`と`await`モバイル アプリでします。 コード例は、このセクションで詳しく説明しています。

### <a name="writing-an-async-method"></a>非同期のメソッドを記述します。

次のメソッドではコードの記述方法、`async`メソッドを`await`ed タスク。

```csharp
public async Task<int> DownloadHomepage()
{
    var httpClient = new HttpClient(); // Xamarin supports HttpClient!

    Task<string> contentsTask = httpClient.GetStringAsync("http://xamarin.com"); // async method!

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

これらの点に注意してください。

-  メソッドの宣言が含まれています、`async`キーワード。
-  戻り値の型は`Task<int>`呼び出しコードにアクセスできるように、`int`この方法で計算される値。
-  Return ステートメントが`return exampleInt;`整数オブジェクト – メソッドが返すファクトでは`Task<int>`言語の機能強化の一部です。


### <a name="calling-an-async-method-1"></a>1、非同期のメソッドを呼び出す

このボタン クリックしてイベント ハンドラーは、上記で説明したメソッドを呼び出して、Android のサンプル アプリケーションで見つかることができます。

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

-  匿名デリゲートは、async キーワードのプレフィックスを持ちます。
-  非同期メソッド DownloadHomepage がタスクを返します<int>sizeTask 変数に格納されています。
-  コードは、sizeTask 変数で待機します。  *これは、* メソッドが中断され、独自のスレッドで非同期のタスクが終了するまで呼び出し元のコードに制御が移るする場所です。
-  実行は*いない*作成中のタスクがあるにもかかわらず、メソッドの最初の行で、タスクが作成されるときに一時停止します。 Await キーワードは、実行が一時停止位置を示します。
-  非同期タスクが完了したら、intResult が設定され、await 行から、元のスレッドで実行が続行されます。


### <a name="calling-an-async-method-2"></a>2 非同期のメソッドを呼び出す

IOS サンプル アプリケーションの例に書き込まれますわずかに異なるその他の方法を示します。 はなく匿名デリゲートを使用してよりもこの例で宣言、`async`正規のイベント ハンドラーのように割り当てられているイベントのハンドラー。

```csharp
GetButton.TouchUpInside += HandleTouchUpInside;
```

次のように、イベント ハンドラー メソッドを定義し、されます。

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

いくつかの重要なポイント:

-  メソッドとしてマークされている`async`を返しますが、`void`です。 これは、通常のみのイベント ハンドラー (戻るそれ以外の場合、`Task`または`Task<TResult>`)。
-  コード`await`で s、`DownloadHomepage`変数への代入で直接メソッド ( `intResult` ) 中間を使用して、前の例とは異なり`Task<int>`タスクを参照する変数。  *これは、* 制御が返される位置、呼び出し元に、非同期メソッドが完了するまで別のスレッドの場所です。
-  実行が再開されると、非同期メソッドの完了を返します、`await`つまり、整数の結果が返され、その UI ウィジェットで表示されます。


## <a name="summary"></a>まとめ

非同期を使用して、await メイン スレッドをブロックすることがなくバック グラウンド スレッドで実行時間の長い操作の起動に必要なコードを大幅に簡略化します。 これらもしやすいタスクが完了すると、結果にアクセスします。

このドキュメントでは、Xamarin.iOS と Xamarin.Android の両方の新しい言語キーワードと例の概要が指定します。



## <a name="related-links"></a>関連リンク

- [AsyncAwait (サンプル)](https://developer.xamarin.com/samples/mobile/AsyncAwait/)
- [ステートメントには、ジェネレーションのとしてコールバック](http://tirania.org/blog/archive/2013/Aug-15.html)
- [データ (iOS) (サンプル)](https://developer.xamarin.com/samples/monotouch/Data/)
- [HttpClient (iOS) (サンプル)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
- [MapKitSearch (iOS) (サンプル)](https://github.com/xamarin/monotouch-samples/tree/master/MapKitSearch)
- [ウェビナー: c# Async iOS および Android (ビデオ)](http://xamarin.wistia.com/medias/k27mc627xz)
- [非同期を使用した非同期プログラミングおよび Await (MSDN)](http://msdn.microsoft.com/library/vstudio/hh191443.aspx)
- [(MSDN) 非同期アプリケーションの微調整](http://msdn.microsoft.com/library/vstudio/jj155761.aspx)
- [Await と UI、およびデッドロック!あらららら！(MSDN)](http://blogs.msdn.com/b/pfxteam/archive/2011/01/13/10115163.aspx)
- [(MSDN)、完了したタスクの処理](http://blogs.msdn.com/b/pfxteam/archive/2012/08/02/processing-tasks-as-they-complete.aspx)
- [タスク ベースの非同期パターン (TAP)](http://msdn.microsoft.com/library/hh873175.aspx)
- [キーワードの概要については、c# 5 (Eric Lippert のブログ) – での非同期性](http://blogs.msdn.com/b/ericlippert/archive/2010/11/11/whither-async.aspx)
