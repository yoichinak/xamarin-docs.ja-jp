---
title: Xamarin.Forms の MessagingCenter
description: この記事では、Xamarin.Forms の MessagingCenter を使用してメッセージを送信および受信し、ビュー モデルなどのクラス間の結合を減らす方法について説明します。
ms.prod: xamarin
ms.assetid: EDFE7B19-C5FD-40D5-816C-FAE56532E885
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: ecd3fe7256eeaa51baf1bc2c367ff7560db51b0c
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53055816"
---
# <a name="xamarinforms-messagingcenter"></a>Xamarin.Forms の MessagingCenter

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/UsingMessagingCenter)

_Xamarin.Forms には、メッセージを送受信するためのシンプルなメッセージング サービスが含まれています。_

<a name="Overview" />

## <a name="overview"></a>概要

Xamarin.Forms の `MessagingCenter` を使うと、ビュー モデルとその他のコンポーネントの通信を、シンプルなメッセージ コントラクト以外は、相互について何も知らなくても実現できます。

<a name="How_the_MessagingCenter_Works" />

## <a name="how-the-messagingcenter-works"></a>MessagingCenter のしくみ

`MessagingCenter` には 2 つの部分があります。

-  **Subscribe** - 特定のシグネチャを持つメッセージをリッスンし、受信したら何らかのアクションを実行します。 複数のサブスクライバーで同じメッセージをリッスンできます。
-  **Send** - リスナーが処理するメッセージを発行します。 サブスクライブしているリスナーがない場合、メッセージは無視されます。


`MessagingService` は、ソリューション全体で使用される `Subscribe` メソッドと `Send` メソッドを含む静的クラスです。

メッセージには文字列の `message` パラメーターがあり、メッセージの "*アドレスを指定する*" 手段として使用されます。 `Subscribe` メソッドと `Send` メソッドでは、ジェネリック パラメーターを使用して、メッセージの配信方法がさらに制御されます。`message` のテキストが同じでも、ジェネリック型引数が異なる 2 つのメッセージは、同じサブスクライバーに配信されません。

`MessagingCenter` 用の API はシンプルです。

- `Subscribe<TSender> (object subscriber, string message, Action<TSender> callback, TSender source = null)`
- `Subscribe<TSender, TArgs> (object subscriber, string message, Action<TSender, TArgs> callback, TSender source = null)`
- `Send<TSender> (TSender sender, string message)`
- `Send<TSender, TArgs> (TSender sender, string message, TArgs args)`
- `Unsubscribe<TSender, TArgs> (object subscriber, string message)`
- `Unsubscribe<TSender> (object subscriber, string message)`

以下ではこれらのメソッドについて説明します。

<a name="Using_the_MessagingCenter" />

## <a name="using-the-messagingcenter"></a>MessagingCenter を使用する

メッセージは、ユーザー操作 (ボタン クリックなど)、システム イベント (コントロールの状態の変化など)、または他の何らかのインシデント (非同期ダウンロードの完了など) の結果として、送信される場合があります。 サブスクライバーは、ユーザー インターフェイスの外観の変更、データの保存、他の何らかの操作のトリガーなどのために、リッスンしている可能性があります。

### <a name="simple-string-message"></a>シンプルな文字列メッセージ

最もシンプルなメッセージは、`message` パラメーターに文字列だけが含まれるものです。 シンプルな文字列メッセージを "*リッスン*" する `Subscribe` メソッドを次に示します。送信者を指定するジェネリック型は `MainPage` 型と想定されていることに注意してください。 ソリューション内のすべてのクラスは、次の構文を使用してメッセージをサブスクライブすることができます。

```csharp
MessagingCenter.Subscribe<MainPage> (this, "Hi", (sender) => {
    // do something whenever the "Hi" message is sent
});
```

`MainPage` クラスでは、次のコードでメッセージが "*送信*" されます。 `this` パラメーターは `MainPage` のインスタンスです。

```csharp
MessagingCenter.Send<MainPage> (this, "Hi");
```

文字列は変更されません。この文字列は "*メッセージの種類*" を示し、通知先のサブスクライバーの決定に使用されます。 この種のメッセージは、"アップロード完了" などの何らかのイベントの発生を示し、それ以上の情報は必要ない場合に使用されます。

### <a name="passing-an-argument"></a>引数を渡す

メッセージで引数を渡すには、`Subscribe` ジェネリック引数と Action シグネチャで引数の型を指定します。

```csharp
MessagingCenter.Subscribe<MainPage, string> (this, "Hi", (sender, arg) => {
    // do something whenever the "Hi" message is sent
    // using the 'arg' parameter which is a string
});
```

引数を持つメッセージを送信するには、型のジェネリック パラメーターと引数の値を、`Send` メソッドの呼び出しに含めます。

```csharp
MessagingCenter.Send<MainPage, string> (this, "Hi", "John");
```

この簡単な例では `string` 引数を使用していますが、任意の C# オブジェクトを渡すことができます。

### <a name="unsubscribe"></a>サブスクライブを解除する

オブジェクトでは、メッセージのシグネチャからサブスクライブを解除し、それ以上メッセージが配信されないようにできます。 `Unsubscribe` メソッドの構文は、メッセージのシグネチャを反映している必要があります (そのため、メッセージの引数に対するジェネリック型パラメーターを含めることが必要な場合があります)。

```csharp
MessagingCenter.Unsubscribe<MainPage> (this, "Hi");
MessagingCenter.Unsubscribe<MainPage, string> (this, "Hi");
```

<a name="Summary" />

## <a name="summary"></a>まとめ

MessagingCenter は、結合を減らす簡単な方法です (特にビュー モデル間)。 シンプルなメッセージを送受信したり、クラス間で引数を受け渡したりするために使用できます。 クラスで受信する必要がなくなったときは、メッセージのサブスクライブを解除する必要があります。


## <a name="related-links"></a>関連リンク

- [MessagingCenterSample](https://developer.xamarin.com/samples/UsingMessagingCenter)
- [Xamarin.Forms のサンプル](https://github.com/xamarin/xamarin-forms-samples)
