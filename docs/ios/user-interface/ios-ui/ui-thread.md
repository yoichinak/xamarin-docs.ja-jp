---
title: Xamarin で UI スレッドを操作する
description: このドキュメントでは、Xamarin. iOS で UI スレッドを操作する方法について説明します。 UI スレッドの実行について説明し、バックグラウンドスレッドの例を示し、async/await を調べます。
ms.prod: xamarin
ms.assetid: 98762ACA-AD5A-4E1E-A536-7AF3BE36D77E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: e91b7fdf99e8eb69cca240253f169ba16b0b11c4
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91432006"
---
# <a name="working-with-the-ui-thread-in-xamarinios"></a>Xamarin で UI スレッドを操作する

アプリケーションユーザーインターフェイスは、マルチスレッドデバイスでも、常にシングルスレッド化されます。画面の表現は1つだけで、表示される内容に対する変更は、1つの "アクセスポイント" によって調整する必要があります。 これにより、複数のスレッドが同じピクセルを同時に更新しようとするのを防ぐことができます (たとえば、)。

コードでは、メイン (または UI) スレッドからのユーザーインターフェイスコントロールに対してのみ変更を行う必要があります。 別のスレッド (コールバックやバックグラウンドスレッドなど) で発生した UI の更新が画面に表示されない場合や、クラッシュが発生する場合もあります。

## <a name="ui-thread-execution"></a>UI スレッドの実行

コントロールをビューで作成したり、タッチなどのユーザーによって開始されるイベントを処理したりする場合、コードは既に UI スレッドのコンテキストで実行されています。

コードがバックグラウンドスレッドで実行されている場合、タスクまたはコールバックでは、メイン UI スレッドで実行されていない可能性があります。 この場合は、次のように、またはの呼び出しでコードをラップする必要があり `InvokeOnMainThread` `BeginInvokeOnMainThread` ます。

```csharp
InvokeOnMainThread ( () => {
    // manipulate UI controls
});
```

メソッドは、 `InvokeOnMainThread` 任意の `NSObject` uikit オブジェクト (ビューコントローラーやビューコントローラーなど) で定義されたメソッド内から呼び出すことができるように、で定義されています。

Xamarin の iOS アプリケーションのデバッグ中に、コードが間違ったスレッドから UI コントロールにアクセスしようとした場合に、エラーがスローされます。 これは、InvokeOnMainThread メソッドを使用して、これらの問題を追跡して修正するのに役立ちます。 これは、デバッグ中にのみ発生し、リリースビルドではエラーをスローしません。 次のようなエラーメッセージが表示されます。

 ![UI スレッドの実行](ui-thread-images/image10.png)

 <a name="Background_Thread_Example"></a>

## <a name="background-thread-example"></a>バックグラウンドスレッドの例

次に、 `UILabel` 単純なスレッドを使用してバックグラウンドスレッドからユーザーインターフェイスコントロール (a) にアクセスしようとする例を示します。

```csharp
new System.Threading.Thread(new System.Threading.ThreadStart(() => {
    label1.Text = "updated in thread"; // should NOT reference UILabel on background thread!
})).Start();
```

デバッグ中に、そのコードによってがスローされ `UIKitThreadAccessException` ます。 この問題を解決する (およびユーザーインターフェイスコントロールにメイン UI スレッドからのみアクセスできるようにする) には、次のように、式の中で UI コントロールを参照するすべてのコードをラップし `InvokeOnMainThread` ます。

```csharp
new System.Threading.Thread(new System.Threading.ThreadStart(() => {
    InvokeOnMainThread (() => {
        label1.Text = "updated in thread"; // this works!
    });
})).Start();
```

このドキュメントの残りの例では、これを使用する必要はありませんが、アプリがネットワーク要求を行うときに覚えておく必要がある重要な概念であり、通知センターや、別のスレッドで実行される完了ハンドラーを必要とするその他のメソッドを使用します。

 <a name="Async_Await_Example"></a>

## <a name="asyncawait-example"></a>Async/Await の例

C# 5 async/await キーワードを使用する `InvokeOnMainThread` 必要はありません。待機中のタスクが完了すると、呼び出し元のスレッドでメソッドが続行されるためです。

このコード例では、(単にデモンストレーション目的で) 遅延メソッドの呼び出しを待機していますが、UI スレッドで呼び出される非同期メソッド (TouchUpInside ハンドラー) を示しています。 外側のメソッドは UI スレッドで呼び出されるため、のテキストの設定やを表示するなどの UI 操作は、 `UILabel` `UIAlertView` バックグラウンドスレッドで非同期操作が完了した後に安全に呼び出すことができます。

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

非同期メソッドが (メイン UI スレッドではなく) バックグラウンドスレッドから呼び出された場合 `InvokeOnMainThread` でも、が必要になります。

## <a name="related-links"></a>関連リンク

- [コントロール (サンプル)](/samples/xamarin/ios-samples/controls)
- [スレッド化](~/ios/app-fundamentals/threading.md)