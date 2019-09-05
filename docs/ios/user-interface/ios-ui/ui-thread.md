---
title: Xamarin で UI スレッドを操作する
description: このドキュメントでは、Xamarin. iOS で UI スレッドを操作する方法について説明します。 UI スレッドの実行について説明し、バックグラウンドスレッドの例を示し、async/await を調べます。
ms.prod: xamarin
ms.assetid: 98762ACA-AD5A-4E1E-A536-7AF3BE36D77E
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: 76733d4efd4ce292da2781c97aef963fb68e3974
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70287871"
---
# <a name="working-with-the-ui-thread-in-xamarinios"></a>Xamarin で UI スレッドを操作する

アプリケーションのユーザー インターフェイスは、マルチ スレッドのデバイスであっても、常にシングル スレッドです – 画面の表示は 1 つで、表示される内容の変更は 1 つの 'アクセス ポイント' を介して調整される必要があります。 これは複数のスレッドが (たとえば) 同時に同じピクセルを更新しようとすることを防ぎます。

コードでは、メイン (または UI) スレッドからのユーザーインターフェイスコントロールに対してのみ変更を行う必要があります。 別のスレッド (コールバックやバックグラウンドスレッドなど) で発生した UI の更新が画面に表示されない場合や、クラッシュが発生する場合もあります。

## <a name="ui-thread-execution"></a>UI スレッドの実行

コントロールをビューで作成したり、タッチなどのユーザーによって開始されるイベントを処理したりする場合、コードは既に UI スレッドのコンテキストで実行されています。

コードがバックグラウンドスレッドで実行されている場合、タスクまたはコールバックでは、メイン UI スレッドで実行されていない可能性があります。 この場合は、次のように`InvokeOnMainThread` 、または`BeginInvokeOnMainThread`の呼び出しでコードをラップする必要があります。

```csharp
InvokeOnMainThread ( () => {
    // manipulate UI controls
});
```

メソッド`InvokeOnMainThread`は、任意の`NSObject` uikit オブジェクト (ビューコントローラーやビューコントローラーなど) で定義されたメソッド内から呼び出すことができるように、で定義されています。

Xamarin の iOS アプリケーションのデバッグ中に、コードが間違ったスレッドから UI コントロールにアクセスしようとした場合に、エラーがスローされます。 これは、InvokeOnMainThread メソッドを使用して、これらの問題を追跡して修正するのに役立ちます。 これは、デバッグ中にのみ発生し、リリースビルドではエラーをスローしません。 次のようなエラーメッセージが表示されます。

 ![](ui-thread-images/image10.png "UI スレッドの実行")

 <a name="Background_Thread_Example" />


## <a name="background-thread-example"></a>バックグラウンドスレッドの例

次に、単純なスレッドを使用してバックグラウンドスレッドからユーザー `UILabel`インターフェイスコントロール (a) にアクセスしようとする例を示します。

```csharp
new System.Threading.Thread(new System.Threading.ThreadStart(() => {
    label1.Text = "updated in thread"; // should NOT reference UILabel on background thread!
})).Start();
```

デバッグ中に、その`UIKitThreadAccessException`コードによってがスローされます。 この問題を解決する (およびユーザーインターフェイスコントロールにメイン UI スレッドからのみアクセスできるようにする) には、次のように、 `InvokeOnMainThread`式の中で UI コントロールを参照するすべてのコードをラップします。

```csharp
new System.Threading.Thread(new System.Threading.ThreadStart(() => {
    InvokeOnMainThread (() => {
        label1.Text = "updated in thread"; // this works!
    });
})).Start();
```

このドキュメントの残りの例では、これを使用する必要はありませんが、アプリがネットワーク要求を行うときに覚えておく必要がある重要な概念であり、通知センターや、別のユーザーで実行される完了ハンドラーを必要とするその他の方法を使用します。レッド.

 <a name="Async_Await_Example" />


## <a name="asyncawait-example"></a>Async/Await の例

5つのC# async/await キーワード`InvokeOnMainThread`を使用する必要はありません。待機中のタスクが完了すると、呼び出し元のスレッドでメソッドが続行されるためです。

このコード例では、(単にデモンストレーション目的で) 遅延メソッドの呼び出しを待機していますが、UI スレッドで呼び出される非同期メソッド (TouchUpInside ハンドラー) を示しています。 外側のメソッドは ui スレッドで呼び出されるため、のテキスト`UILabel`の設定やを`UIAlertView`表示するなどの ui 操作は、バックグラウンドスレッドで非同期操作が完了した後に安全に呼び出すことができます。

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

非同期メソッドが (メイン UI スレッドではなく) `InvokeOnMainThread`バックグラウンドスレッドから呼び出された場合でも、が必要になります。


## <a name="related-links"></a>関連リンク

- [コントロール (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/controls)
- [スレッド化](~/ios/app-fundamentals/threading.md)
