---
title: 非同期サポートの概要
description: このドキュメントは、async を使用したプログラミングについて説明し、await で導入された概念C#5 非同期コードを記述するが容易にします。
ms.prod: xamarin
ms.assetid: F87BF587-AB64-4C60-84B1-184CAE36ED65
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 0a72dead1b6c001f1514f1a089df9b407eb90644
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57671879"
---
# <a name="async-support-overview"></a>非同期サポートの概要

_C#導入された非同期プログラミングを簡略化する 2 つのキーワードの 5: async と await します。これらのキーワードでは、別のスレッドで (ネットワーク アクセス) などの実行時間の長い操作を実行するには、タスク並列ライブラリを使用する単純なコードを記述し、完了時に結果を簡単にアクセスできます。最新のバージョンの Xamarin.iOS と Xamarin.Android が非同期のサポートし、await - 説明、および Xamarin を使用した新しい構文を使用する例を説明します。_

Xamarin の非同期サポートは Mono 3.0 基盤上に構築し、API プロファイルをモバイル フレンドリ バージョンの .NET 4.5 のモバイル フレンドリ バージョンである Silverlight の中からアップグレードします。

## <a name="overview"></a>概要

このドキュメントでは、新しい非同期を紹介し、await のキーワード、Xamarin.iOS および Xamarin.Android での非同期メソッドを実装する簡単な例を見ていきます。

新しい非同期機能の詳細な説明のC#MSDN ドキュメントを参照してください (多くのサンプルとさまざまな使用シナリオを含む) 5 [Async および Await を使用した非同期プログラミング](https://msdn.microsoft.com/library/vstudio/hh191443.aspx)します。

サンプル アプリケーションでは、(メイン スレッドをブロック) なしで単純な非同期 web 要求を作成し、ダウンロードした html および文字数で UI を更新します。

 [![](async-images/AsyncAwait_427x368.png "サンプル アプリケーションはメイン スレッドをブロックすることがなく、単純な非同期 web 要求を作成し、ダウンロードした html および文字数で UI を更新")](async-images/AsyncAwait.png#lightbox)

Xamarin の非同期サポートは Mono 3.0 基盤上に構築し、API プロファイルをモバイル フレンドリ バージョンの .NET 4.5 のモバイル フレンドリ バージョンである Silverlight の中からアップグレードします。

## <a name="requirements"></a>必要条件

C#5 つの機能では、Mono 3.0 6.4 の Xamarin.iOS と Xamarin.Android 4.8 に含まれている必要があります。 これを活用するために、Mono、Xamarin.iOS、Xamarin.Android、Xamarin.Mac にアップグレードするように促されます。

## <a name="using-async-amp-await"></a>非同期を使用して&amp;await

 `async` `await`は新しいC#言語機能を簡単に、アプリケーションのメイン スレッドをブロックすることがなく実行時間の長いタスクを実行するスレッドのコードを記述し、タスク並列ライブラリと連携して動作をします。

## <a name="async"></a>async

### <a name="declaration"></a>宣言

`async`キーワードがメソッドの宣言で (またはラムダ式、または匿名メソッド) に配置を非同期的に実行できるコードが含まれているかを示す ie。 呼び出し元のスレッドをブロックします。

マークされたメソッド`async`少なくとも 1 つ含める必要がありますの await 式またはステートメント。 いない場合`await`は同期的に実行し、s が、メソッド内に存在 (同じがあった場合とない`async`修飾子)。 これが、コンパイラの警告 (ただし、エラーではなく) にも発生します。

### <a name="return-types"></a>戻り値の型

非同期メソッドが返す必要があります、 `Task`、`Task<TResult>`または`void`します。

指定、`Task`メソッドがその他の値を返さない場合、型を返します。

指定`Task<TResult>`メソッドが値を返す必要がある場合、`TResult`が返される型 (など、`int`など)。

`void`戻り値の型が必要とするイベント ハンドラーの主に使用します。 Void を返す非同期メソッドを呼び出すコードをできません`await`結果にします。

### <a name="parameters"></a>パラメーター

非同期メソッドを宣言できません`ref`または`out`パラメーター。

## <a name="await"></a>await

Await 演算子は、async とマークされたメソッド内のタスクに適用できます。 その時点での実行を停止し、タスクが完了するまでの待機メソッドが発生します。

Await を使用して、呼び出し元のスレッド – をブロックしませんではなくコントロールが呼び出し元に返されます。 つまり、たとえば、ユーザー インターフェイス スレッドはブロックされませんタスクを待っているときに、呼び出し元のスレッドがブロックされていないこと。

タスクが完了したら、同じ時点で、コードを実行するメソッドを再開します。 これは、(1 つが存在する) 場合は、try – catch – finally ブロックの try スコープに戻るが含まれます。 await finally ブロックまたは catch で使用できません。

詳細をご覧ください[msdn await](https://msdn.microsoft.com/library/vstudio/hh156528.aspx)します。

## <a name="exception-handling"></a>例外処理

非同期メソッド内で発生する例外は、タスクに格納され、タスクの場合にスロー `await`ed します。 これらの例外をキャッチして、try-catch ブロック内で処理できます。

## <a name="cancellation"></a>キャンセル

完了に長い時間がかかる非同期のメソッドは、キャンセルをサポートする必要があります。 通常、キャンセルが次のように呼び出されます。

- A`CancellationTokenSource`オブジェクトが作成されます。
- `CancellationTokenSource.Token`インスタンスがキャンセル可能な非同期メソッドに渡されます。
- 呼び出すことによって取り消しが要求される、`CancellationTokenSource.Cancel`メソッド。

タスクは自体をキャンセルし、キャンセルを確認します。

キャンセルの詳細については、次を参照してください。[非同期タスクを取り消す方法](https://msdn.microsoft.com/library/vstudio/jj155761.aspx)msdn です。

## <a name="example"></a>例

ダウンロード、[例 Xamarin ソリューション](https://developer.xamarin.com/samples/mobile/AsyncAwait/)(iOS と Android の両方) に対して作業例について`async`と`await`モバイル アプリでします。 コード例については、このセクションで詳しく説明します。

### <a name="writing-an-async-method"></a>非同期メソッドの記述

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
-  戻り値の型は`Task<int>`呼び出しコードにアクセスできるように、`int`このメソッドで計算される値。
-  Return ステートメントが`return exampleInt;`整数オブジェクト – メソッドによって返されるという事実は`Task<int>`は言語の機能強化の一部です。


### <a name="calling-an-async-method-1"></a>1 非同期のメソッドを呼び出す

このボタン クリックしてイベント ハンドラーは、前に説明したメソッドを呼び出す Android のサンプル アプリケーションで見つかることができます。

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

-  匿名のデリゲートは、async キーワードのプレフィックスを持ちます。
-  非同期メソッド DownloadHomepage がタスクを返します<int>sizeTask 変数に格納されています。
-  コードは、sizeTask 変数で待機します。  *これは、* は、メソッドが中断され、独自のスレッドで非同期タスクが完了するまで呼び出し元のコードに制御が返される場所です。
-  実行は*いない*が作成されるタスクに関係なく、メソッドの最初の行で、タスクが作成されるときは一時停止します。 Await キーワードでは、実行が一時停止している場所を示します。
-  非同期タスクが完了したら、intResult が設定されているし、await の行から、元のスレッドで実行が続行されます。


### <a name="calling-an-async-method-2"></a>2 非同期のメソッドを呼び出す

IOS のサンプル アプリケーションの例に書き込まれますとは若干異なります代替の方法を示しています。 はなく、匿名デリゲートを使用してよりもこの例で宣言、`async`通常のイベント ハンドラーのように割り当てられているイベント ハンドラー。

```csharp
GetButton.TouchUpInside += HandleTouchUpInside;
```

次のように、イベント ハンドラー メソッドは、定義されています。

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

-  メソッドをマーク`async`返しますが、`void`します。 これは、通常のみのイベント ハンドラー (は返しますそれ以外の場合、`Task`または`Task<TResult>`)。
-  コード`await`上、`DownloadHomepage`メソッド、変数への代入を直接 ( `intResult` ) 中間を使用して、前の例とは異なり`Task<int>`タスクを参照する変数。  *これは、* は制御が返される位置、呼び出し元に、非同期メソッドが完了するまで別のスレッドでの場所です。
-  実行が再開される非同期メソッドが完了しを返します、ときに、`await`つまり、整数の結果が返され、UI ウィジェットでレンダリングされます。


## <a name="summary"></a>まとめ

非同期を使用して、await、メイン スレッドをブロックすることがなくバック グラウンド スレッドで実行時間の長い操作の生成に必要なコードを大幅に簡略化します。 簡単に、タスクが完了したら、結果にアクセスします。

このドキュメントでは、Xamarin.iOS と Xamarin.Android の両方に、新しい言語キーワードと例の概要が提供できます。



## <a name="related-links"></a>関連リンク

- [AsyncAwait (サンプル)](https://developer.xamarin.com/samples/mobile/AsyncAwait/)
- [ステートメントに移動し、生成結果としてコールバック](https://tirania.org/blog/archive/2013/Aug-15.html)
- [データ (iOS) (サンプル)](https://developer.xamarin.com/samples/monotouch/Data/)
- [HttpClient (iOS) (サンプル)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
- [MapKitSearch (iOS) (サンプル)](https://github.com/xamarin/monotouch-samples/tree/master/MapKitSearch)
- [ウェビナー:C#IOS と Android (ビデオ) での非同期](http://xamarin.wistia.com/medias/k27mc627xz)
- [非同期を使用した非同期プログラミングと Await (MSDN)](https://msdn.microsoft.com/library/vstudio/hh191443.aspx)
- [(MSDN) 非同期アプリケーションの微調整](https://msdn.microsoft.com/library/vstudio/jj155761.aspx)
- [Await と UI、およびデッドロック!あらららら！(MSDN)](http://blogs.msdn.com/b/pfxteam/archive/2011/01/13/10115163.aspx)
- [(MSDN) を完了するタスクの処理](http://blogs.msdn.com/b/pfxteam/archive/2012/08/02/processing-tasks-as-they-complete.aspx)
- [タスク ベースの非同期パターン (TAP)](https://msdn.microsoft.com/library/hh873175.aspx)
- [非同期性C#キーワードの導入について 5 (Eric Lippert のブログ)。](http://blogs.msdn.com/b/ericlippert/archive/2010/11/11/whither-async.aspx)
