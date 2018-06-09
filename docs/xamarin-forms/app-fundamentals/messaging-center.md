---
title: Xamarin.Forms MessagingCenter
description: この記事では、Xamarin.Forms MessagingCenter を使用してをモデルの表示などのクラス間の結合を減らすために、メッセージを送受信する方法について説明します。
ms.prod: xamarin
ms.assetid: EDFE7B19-C5FD-40D5-816C-FAE56532E885
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: 71f526f87a2536110a6d2292cd66a0f4d81c0bfc
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240338"
---
# <a name="xamarinforms-messagingcenter"></a>Xamarin.Forms MessagingCenter

_Xamarin.Forms には、メッセージを送受信する場合は、単純なメッセージング サービスが含まれています。_

<a name="Overview" />

## <a name="overview"></a>概要

Xamarin.Forms`MessagingCenter`によりモデルと単純なメッセージ コントラクトだけでなく相互についての情報を知らなくてもと通信するには、その他のコンポーネントを表示します。

<a name="How_the_MessagingCenter_Works" />

## <a name="how-the-messagingcenter-works"></a>MessagingCenter のしくみ

2 つの部分を`MessagingCenter`:

-  **サブスクライブ**- 特定のシグネチャを持つメッセージをリッスンし、受信したときに何らかのアクションを実行します。 複数のサブスクライバーは、同じメッセージをリッスンしていることができます。
-  **送信**-に対して操作を実行するためのリスナーにメッセージを発行します。 サブスクライブしているリスナーがない場合は、メッセージは無視されます。


`MessagingService`静的クラスで、`Subscribe`と`Send`はソリューション全体で使用されるメソッド。

メッセージ文字列がある`message`する方法として使用されるパラメーター*アドレス*メッセージ。 `Subscribe`と`Send`方法では、ジェネリック パラメーターを使用して、さらにメッセージを配信する方法を制御する - 2 つのメッセージを同じ`message`テキストが別のジェネリック型の引数は配信されません同じサブスクライバーにします。

用の API`MessagingCenter`は単純です。

-  サブスクライブ&lt;TSender > (オブジェクトのサブスクライバー、文字列メッセージをアクション&lt;TSender > コールバック、それらのソース = null)
-  サブスクライブ&lt;TSender, TArgs > (オブジェクトのサブスクライバー、文字列メッセージをアクション&lt;TSender, TArgs > コールバック、それらのソース = null)
-  送信&lt;TSender > (TSender 送信者、文字列メッセージ)
-  送信&lt;TSender, TArgs > (TSender 送信者、文字列メッセージ TArgs args)
-  サブスクリプションの解除&lt;TSender, TArgs > (オブジェクトのサブスクライバー、文字列メッセージ)
-  サブスクリプションの解除&lt;TSender > (オブジェクトのサブスクライバー、文字列メッセージ)


これらのメソッドについて以下に説明します。

<a name="Using_the_MessagingCenter" />

## <a name="using-the-messagingcenter"></a>使用して、MessagingCenter

ユーザーの相互作用 (ボタンをクリックして) 同じように、システム イベント (コントロールと同様に状態を変更する) または (非同期のダウンロードの完了、) などの他のいくつかのインシデントの結果としてメッセージを送信する可能性があります。 サブスクライバーは、ユーザー インターフェイスの外観を変更、データを保存またはその他の操作をトリガーにリッスンしている可能性があります。

### <a name="simple-string-message"></a>単純な文字列メッセージ

最も簡単なメッセージには、単に文字列が含まれています、`message`パラメーター。 A`Subscribe`メソッドを*リッスン*単純な文字列のメッセージが、次に示すは、送信者が型にする期待を指定してジェネリック型に注意してください`MainPage`です。 ソリューション内のすべてのクラスは、この構文を使用してメッセージにサブスクライブできます。

```csharp
MessagingCenter.Subscribe<MainPage> (this, "Hi", (sender) => {
    // do something whenever the "Hi" message is sent
});
```

`MainPage`クラスの次のコード*送信*メッセージ。 `this`パラメーターのインスタンスは、`MainPage`です。

```csharp
MessagingCenter.Send<MainPage> (this, "Hi");
```

文字列は変更されません - ことを示します、*メッセージの種類*をどのサブスクライバーに通知を決定するために使用されます。 このようなメッセージが「のアップロードが完了しました」など一部のイベントが発生したことを示すために使用されるより詳細な情報は必要ありません。

### <a name="passing-an-argument"></a>引数を渡し

渡すには、メッセージの引数、引数の型を指定で、`Subscribe`汎用引数とアクションの署名にします。

```csharp
MessagingCenter.Subscribe<MainPage, string> (this, "Hi", (sender, arg) => {
    // do something whenever the "Hi" message is sent
    // using the 'arg' parameter which is a string
});
```

引数を持つメッセージを送信するなどの型のジェネリック パラメーターの引数の値、`Send`メソッドの呼び出しです。

```csharp
MessagingCenter.Send<MainPage, string> (this, "Hi", "John");
```

この簡単な例を使用して、`string`引数が任意の c# オブジェクトを渡すことができます。

### <a name="unsubscribe"></a>サブスクリプションの解除します。

オブジェクトは、今後、メッセージは配信されませんできるように、メッセージの署名を取り消しできます。 `Unsubscribe`メソッドの構文は、メッセージの署名を反映する必要があります (したがって必要があります、メッセージの引数にジェネリック型パラメーターが含まれます)。

```csharp
MessagingCenter.Unsubscribe<MainPage> (this, "Hi");
MessagingCenter.Unsubscribe<MainPage, string> (this, "Hi");
```

<a name="Summary" />

## <a name="summary"></a>まとめ

MessagingCenter は、特にビュー モデルとの間の結合を削減する簡単な方法です。 これは、送信を単純なメッセージを受信する場合は、クラス間の引数を渡すかを使用できます。 クラスは、メッセージを受信する必要がないからアンサブスク ライブする必要があります。


## <a name="related-links"></a>関連リンク

- [MessagingCenterSample](https://developer.xamarin.com/samples/UsingMessagingCenter)
- [Xamarin.Forms のサンプル](https://github.com/xamarin/xamarin-forms-samples)
