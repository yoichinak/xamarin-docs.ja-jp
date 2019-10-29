---
title: Xamarin で UI スレッドを操作する
description: このドキュメントでは、Xamarin. iOS で UI スレッドを操作する方法について説明します。 UI スレッドの実行について説明し、バックグラウンドスレッドの例を示し、async/await を調べます。
ms.prod: xamarin
ms.assetid: 98762ACA-AD5A-4E1E-A536-7AF3BE36D77E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: ee7ab7c5d0503cffd2c12a493f314f191d912e92
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73002843"
---
# <a name="working-with-the-ui-thread-in-xamarinios"></a>Xamarin で UI スレッドを操作する

アプリケーションユーザーインターフェイスは、マルチスレッドデバイスでも、常にシングルスレッド化されます。画面の表現は1つだけで、表示される内容に対する変更は、1つの "アクセスポイント" によって調整する必要があります。 これにより、複数のスレッドが同じピクセルを同時に更新しようとするのを防ぐことができます (たとえば、)。

コードでは、メイン (または UI) スレッドからのユーザーインターフェイスコントロールに対してのみ変更を行う必要があります。 別のスレッド (コールバックやバックグラウンドスレッドなど) で発生した UI の更新が画面に表示されない場合や、クラッシュが発生する場合もあります。

## <a name="ui-thread-execution"></a>UI スレッドの実行

コントロールをビューで作成したり、タッチなどのユーザーによって開始されるイベントを処理したりする場合、コードは既に UI スレッドのコンテキストで実行されています。

コードがバックグラウンドスレッドで実行されている場合、タスクまたはコールバックでは、メイン UI スレッドで実行されていない可能性があります。 この場合は、次のように `InvokeOnMainThread` または `BeginInvokeOnMainThread` の呼び出しでコードをラップする必要があります。

```csharp
InvokeOnMainThread ( () => {
    // manipulate UI controls
});
```

`InvokeOnMainThread` メソッドは `NSObject` で定義されているため、ビューやビューコントローラーなど、任意の UIKit オブジェクトで定義されたメソッド内から呼び出すことができます。

Xamarin の iOS アプリケーションのデバッグ中に、コードが間違ったスレッドから UI コントロールにアクセスしようとした場合に、エラーがスローされます。 これは、InvokeOnMainThread メソッドを使用して、これらの問題を追跡して修正するのに役立ちます。 これは、デバッグ中にのみ発生し、リリースビルドではエラーをスローしません。 次のようなエラーメッセージが表示されます。

 ![](ui-thread-images/image10.png "UI Thread Execution")

 <a name="Background_Thread_Example" />

## <a name="background-thread-example"></a>バックグラウンドスレッドの例

次に示すのは、単純なスレッドを使用してバックグラウンドスレッドからユーザーインターフェイスコントロール (`UILabel`) にアクセスしようとする例です。

```csharp
new System.Threading.Thread(new System.Threading.ThreadStart(() => {
    label1.Text = "updated in thread"; // should NOT reference UILabel on background thread!
})).Start();
```

このコードは、デバッグ中に `UIKitThreadAccessException` をスローします。 この問題を解決する (およびユーザーインターフェイスコントロールにメイン UI スレッドからのみアクセスできるようにする) には、次のように `InvokeOnMainThread` 式内で UI コントロールを参照するすべてのコードをラップします。

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

5つのC# async/await キーワードを使用する場合`InvokeOnMainThread`は必要ありません。待機中のタスクが完了すると、呼び出し元のスレッドでメソッドが続行されるためです。

このコード例では、(単にデモンストレーション目的で) 遅延メソッドの呼び出しを待機していますが、UI スレッドで呼び出される非同期メソッド (TouchUpInside ハンドラー) を示しています。 外側のメソッドが UI スレッドで呼び出されるため、`UILabel` のテキストの設定や `UIAlertView` の表示などの UI 操作は、バックグラウンドスレッドで非同期操作が完了した後に安全に呼び出すことができます。

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

非同期メソッドが (メイン UI スレッドではなく) バックグラウンドスレッドから呼び出された場合でも、`InvokeOnMainThread` が必要になります。

## <a name="related-links"></a>関連リンク

- [コントロール (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/controls)
- [スレッド化](~/ios/app-fundamentals/threading.md)
