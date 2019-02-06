---
title: Xamarin.iOS で UI スレッドの操作
description: このドキュメントでは、Xamarin.iOS で UI スレッドを操作する方法について説明します。 UI スレッドの実行について説明します、バック グラウンド スレッドの使用例を提供し、非同期/待機を検査します。
ms.prod: xamarin
ms.assetid: 98762ACA-AD5A-4E1E-A536-7AF3BE36D77E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 6dd55f5c4316ed8f1d4f16d9e282cc2647350518
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104857"
---
# <a name="working-with-the-ui-thread-in-xamarinios"></a>Xamarin.iOS で UI スレッドの操作

アプリケーションのユーザー インターフェイスは、マルチ スレッドのデバイスであっても、常にシングル スレッドです – 画面の表示は 1 つだけですし、表示される内容の変更は 1 つの 'アクセス ポイント' を介して調整される必要があります。 これは複数のスレッドが (たとえば) 同時に同じのピクセルを更新しようとすることを防ぎます。

コード変更をユーザー インターフェイス コントロールのメイン (または UI) スレッドのみことをする必要があります。 (コールバックまたはバック グラウンドのスレッド) などの別のスレッドで発生する UI の更新は、画面にレンダリング取得されない可能性があります。 またはクラッシュを起こす可能性があります。

## <a name="ui-thread-execution"></a>UI スレッドの実行

ビューでは、コントロールの作成またはタッチなどのユーザーが開始したイベントを処理するとき、コードは既に UI スレッドのコンテキストで実行します。

コードは、タスクまたはコールバックでのバック グラウンド スレッドで実行している場合は、メイン UI スレッドで実行されていない可能性があります。 ここでの呼び出しで、コードをラップする必要があります`InvokeOnMainThread`または`BeginInvokeOnMainThread`次のようにします。

```csharp
InvokeOnMainThread ( () => {
    // manipulate UI controls
});
```

`InvokeOnMainThread`でメソッドが定義されている`NSObject`のため、任意の UIKit オブジェクト (ビューやビュー コント ローラー) で定義されたメソッド内から呼び出すことができます。

Xamarin.iOS アプリケーションをデバッグ中に、コードが間違ったスレッドから UI コントロールにアクセスしようしている場合にエラーがスローされます。 これにより、追跡して InvokeOnMainThread メソッドを使用してこれらの問題を修正することができます。 これのみ、デバッグ中に発生し、リリース ビルドでエラーをスローしません。 このようなエラー メッセージが表示されます。

 ![](ui-thread-images/image10.png "UI スレッドの実行")

 <a name="Background_Thread_Example" />


## <a name="background-thread-example"></a>バック グラウンド スレッドの使用例

ユーザー インターフェイス コントロールにアクセスしようとする例を次に示します (、 `UILabel`) 単純なスレッドを使用してバック グラウンド スレッドから。

```csharp
new System.Threading.Thread(new System.Threading.ThreadStart(() => {
    label1.Text = "updated in thread"; // should NOT reference UILabel on background thread!
})).Start();
```

コードがスローされます、`UIKitThreadAccessException`デバッグ中にします。 問題を修正 (およびユーザー インターフェイス コントロールにのみ、メイン UI スレッドからアクセスすることを確認します)、内部の UI コントロールを参照するコードをラップする`InvokeOnMainThread`このような式。

```csharp
new System.Threading.Thread(new System.Threading.ThreadStart(() => {
    InvokeOnMainThread (() => {
        label1.Text = "updated in thread"; // this works!
    });
})).Start();
```

行う必要がありますしないアプリがネットワーク要求を行う場合に注意する重要な概念は、これが、このドキュメントの例の残りの部分を使用して通知センターまたは別の実行完了ハンドラーを必要とするその他のメソッドを使用します。スレッドです。

 <a name="Async_Await_Example" />


## <a name="asyncawait-example"></a>非同期/待機の例

使用する場合、 C# 5 async/await キーワード`InvokeOnMainThread`必要でないため、待機中のタスクが完了したとき、メソッドが呼び出し元のスレッドで続行されます。

(これは純粋なデモンストレーションのために、遅延メソッドの呼び出しで待機) このコードの例では、(TouchUpInside ハンドラーは)、UI スレッドで呼び出される非同期のメソッドを示します。 UI スレッドでそれを含むメソッドを呼び出したため、UI などの操作に、テキストを設定、`UILabel`または表示されている、`UIAlertView`バック グラウンド スレッドで非同期操作が完了した後に安全に呼び出すことができます。

```csharp
async partial void button2_TouchUpInside (UIButton sender)
{
    textfield1.ResignFirstResponder ();
    textfield2.ResignFirstResponder ();
    textview1.ResignFirstResponder ();
    label1.Text = "async method started";
    await Task.Delay(1000); // example purpose only
    label1.Text = "1 second passed";
    await Task.Delay(2000);
    label1.Text = "2 more seconds passed";
    await Task.Delay(1000);
    new UIAlertView("Async method complete", "This method", 
               null, "Cancel", null)
        .Show();
    label1.Text = "async method completed";
}
```

バック グラウンド スレッド (いないメイン UI スレッド) から非同期メソッドが呼び出された場合、`InvokeOnMainThread`必要なことができるようにします。


## <a name="related-links"></a>関連リンク

- [コントロール (サンプル)](https://developer.xamarin.com/samples/Controls/)
- [スレッド化](~/ios/app-fundamentals/threading.md)
