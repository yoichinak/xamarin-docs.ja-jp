---
title: Xamarin.Forms MessagingCenter
description: この記事では、Xamarin.Forms MessagingCenter を使用して、モデルの表示などのクラス間のカップリングを削減する、メッセージを送信する方法について説明します。
ms.prod: xamarin
ms.assetid: EDFE7B19-C5FD-40D5-816C-FAE56532E885
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: 7fef4443cacba0fa8bdb8d5df070c4244730b4f5
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675176"
---
# <a name="xamarinforms-messagingcenter"></a>Xamarin.Forms MessagingCenter

_Xamarin.Forms には、メッセージを送受信する場合は、単純なメッセージング サービスが含まれています。_

<a name="Overview" />

## <a name="overview"></a>概要

Xamarin.Forms`MessagingCenter`により表示モデルとその他のコンポーネントを簡単なメッセージ コントラクトだけでなく相互について何も知らなくても通信します。

<a name="How_the_MessagingCenter_Works" />

## <a name="how-the-messagingcenter-works"></a>MessagingCenter のしくみ

2 つの部分に`MessagingCenter`:

-  **サブスクライブ**- 特定のシグネチャを持つメッセージをリッスンし、受信したときに何らかのアクションを実行します。 複数のサブスクライバーは、同じメッセージをリッスンしていることができます。
-  **送信**-リスナーに対して操作を実行するためにメッセージを発行します。 サブスクライブしているリスナーがない場合は、メッセージは無視されます。


`MessagingService`静的クラスで、`Subscribe`と`Send`ソリューション全体で使用されるメソッド。

メッセージ文字列がある`message`する方法として使用されるパラメーター*アドレス*メッセージ。 `Subscribe`と`Send`方法では、ジェネリック パラメーターを使用して、さらにメッセージを配信する方法を制御する - 2 つのメッセージを同じ`message`テキストが別のジェネリック型引数は同じサブスクライバーに配信されません。

用の API`MessagingCenter`は単純です。

- `Subscribe<TSender> (object subscriber, string message, Action<TSender> callback, TSender source = null)`
- `Subscribe<TSender, TArgs> (object subscriber, string message, Action<TSender, TArgs> callback, TSender source = null)`
- `Send<TSender> (TSender sender, string message)`
- `Send<TSender, TArgs> (TSender sender, string message, TArgs args)`
- `Unsubscribe<TSender, TArgs> (object subscriber, string message)`
- `Unsubscribe<TSender> (object subscriber, string message)`

これらのメソッドについて以下に説明します。

<a name="Using_the_MessagingCenter" />

## <a name="using-the-messagingcenter"></a>MessagingCenter を使用します。

ユーザー操作 (ボタン クリックしてなど、(コントロールの状態を変更する) などのシステム イベントまたは他のいくつかのインシデント (非同期のダウンロードを完了する) などの結果としてメッセージを送信できます。 サブスクライバーは、ユーザー インターフェイスの外観を変更、データを保存またはその他の操作をトリガーにリッスンしている可能性があります。

### <a name="simple-string-message"></a>単純な文字列メッセージ

最も簡単なメッセージには、単に文字列が含まれています、`message`パラメーター。 A`Subscribe`メソッドを*リッスン*に注意してください、送信者の種類のことが必要ですが指定するジェネリック型の単純な文字列メッセージが表示されます - 以下の`MainPage`します。 ソリューション内のすべてのクラスは、この構文を使用してメッセージをサブスクライブできます。

```csharp
MessagingCenter.Subscribe<MainPage> (this, "Hi", (sender) => {
    // do something whenever the "Hi" message is sent
});
```

`MainPage`次のコードをクラス*送信*メッセージ。 `this`パラメーターのインスタンスである`MainPage`します。

```csharp
MessagingCenter.Send<MainPage> (this, "Hi");
```

文字列が変更されないことを示します、*メッセージの種類*どのサブスクライバーに通知を決定するために使用されます。 このようなメッセージが「アップロードが完了しました」などいくつかのイベントが発生したことを示すため、さらに情報は必要ありません。

### <a name="passing-an-argument"></a>引数を渡す

メッセージの引数を渡す引数の型を指定で、`Subscribe`ジェネリック引数とアクションのシグネチャでします。

```csharp
MessagingCenter.Subscribe<MainPage, string> (this, "Hi", (sender, arg) => {
    // do something whenever the "Hi" message is sent
    // using the 'arg' parameter which is a string
});
```

引数を持つメッセージを送信する型のジェネリック パラメーターと引数の値を含める、`Send`メソッドの呼び出し。

```csharp
MessagingCenter.Send<MainPage, string> (this, "Hi", "John");
```

この単純な例では、`string`引数が、いずれかのC#オブジェクトを渡すことができます。

### <a name="unsubscribe"></a>サブスクリプションの解除します。

オブジェクトは、今後このメッセージが配信されないように、メッセージの署名を取り消しできます。 `Unsubscribe`メソッドの構文は、メッセージの署名を反映する必要があります (そのため必要がありますが message 引数のジェネリック型パラメーターが含まれます)。

```csharp
MessagingCenter.Unsubscribe<MainPage> (this, "Hi");
MessagingCenter.Unsubscribe<MainPage, string> (this, "Hi");
```

<a name="Summary" />

## <a name="summary"></a>まとめ

MessagingCenter は、特にビュー モデル間の結合を減らす簡単な方法です。 これは、送信に単純なメッセージを受信する場合は、クラス間の引数を渡すかを使用できます。 クラス不要になった受信を希望するメッセージの登録を解除する必要があります。


## <a name="related-links"></a>関連リンク

- [MessagingCenterSample](https://developer.xamarin.com/samples/UsingMessagingCenter)
- [Xamarin.Forms のサンプル](https://github.com/xamarin/xamarin-forms-samples)
