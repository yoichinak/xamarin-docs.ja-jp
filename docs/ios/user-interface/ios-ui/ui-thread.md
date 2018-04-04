---
title: UI スレッドの操作
ms.prod: xamarin
ms.assetid: 98762ACA-AD5A-4E1E-A536-7AF3BE36D77E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 72f161001509519fb02a652f23eaa7805a55f7ca
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-the-ui-thread"></a>UI スレッドの操作

アプリケーションのユーザー インターフェイスはシングル スレッドで、マルチ スレッド化されたデバイスであっても、常に – 画面の 1 つだけ表現があるとへ表示される内容の変更を単一 'アクセス ポイント' を通して調整する必要があります。 これは複数のスレッドが (たとえば)、同時に同一のピクセルを更新しようとすることを防ぎます。

コードを変更するユーザーのメイン インターフェイス コントロール (または UI) スレッドのみ行います。 (コールバックまたはバック グラウンドのスレッド) などの別のスレッドで発生する UI の更新は、画面にレンダリング取得されない可能性があります。 またはクラッシュが生じる場合もします。

## <a name="ui-thread-execution"></a>UI スレッドの実行

ビューでは、コントロールを作成したり、タッチなどのユーザーが開始したイベントを処理するとき、コードは既に UI スレッドのコンテキストで実行します。

コードがタスクまたはコールバックでバック グラウンド スレッドで実行されている場合は、メイン UI スレッドで実行されていない可能性があります。 ここへの呼び出しで、コードをラップする必要があります`InvokeOnMainThread`または`BeginInvokeOnMainThread`次のようにします。

```csharp
InvokeOnMainThread ( () => {
    // manipulate UI controls
});
```

`InvokeOnMainThread`でメソッドが定義された`NSObject`のため、その UIKit オブジェクト (ビューまたはビュー コント ローラーの場合) などに定義されているメソッド内から呼び出すことができます。

Xamarin.iOS アプリケーションをデバッグ中に、コードが間違ったスレッドから UI コントロールにアクセスを試みた場合にエラーがスローされます。 追跡および InvokeOnMainThread メソッドを使用してこれらの問題を修復することができます。 これのみデバッグ中に発生して、リリース ビルドでエラーはスローされません。 次のように、エラー メッセージが表示されます。

 ![](ui-thread-images/image10.png "UI スレッドの実行")

 <a name="Background_Thread_Example" />


## <a name="background-thread-example"></a>バック グラウンド スレッドの使用例

ユーザー インターフェイス コントロールにアクセスしようとする例を次に示します (、 `UILabel`) 単純なスレッドを使用して、バック グラウンド スレッドから。

```csharp
new System.Threading.Thread(new System.Threading.ThreadStart(() => {
    label1.Text = "updated in thread"; // should NOT reference UILabel on background thread!
})).Start();
```

コードがスローされます、`UIKitThreadAccessException`デバッグ中にします。 問題を修正する (ユーザー インターフェイス コントロールにのみ、メイン UI スレッドからアクセスすることを確認してください)、内部の UI コントロールを参照しているコードをラップする`InvokeOnMainThread`次のような式。

```csharp
new System.Threading.Thread(new System.Threading.ThreadStart(() => {
    InvokeOnMainThread (() => {
        label1.Text = "updated in thread"; // this works!
    });
})).Start();
```

しない必要がありますの例では、このドキュメントの残りの部分は、アプリがネットワーク要求を行うときに注意して重要な概念を使用して、通知センターまたは他の場所で実行される完了ハンドラーを必要とするその他のメソッドを使用スレッドです。

 <a name="Async_Await_Example" />


## <a name="asyncawait-example"></a>Async と Await の例

C# 5 async と await キーワードを使用するときに`InvokeOnMainThread`は不要なため、待機中のタスクが完了すると、メソッドの呼び出し元のスレッドに続行されます。

(純粋なデモの目的で、遅延メソッドの呼び出しで待機) をこのコードの例では、(TouchUpInside ハンドラーでは)、UI スレッドで呼び出される非同期のメソッドを示します。 テキストの設定などを含むメソッドが呼び出されるため、UI スレッドで、UI 操作、`UILabel`または表示されている、`UIAlertView`バック グラウンド スレッドで非同期操作が完了した後に安全に呼び出すことができます。

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

バック グラウンド スレッド (いないメイン UI スレッド) から非同期メソッドが呼び出された場合、`InvokeOnMainThread`も必要になります。


## <a name="related-links"></a>関連リンク

- [コントロール (サンプル)](https://developer.xamarin.com/samples/Controls/)
- [スレッド化](~/ios/app-fundamentals/threading.md)
