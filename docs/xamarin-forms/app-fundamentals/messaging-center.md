---
title: Xamarin.Forms の MessagingCenter
description: Xamarin.Forms の MessagingCenter クラスでは、発行/サブスクライブ パターンが実装され、オブジェクトと型の参照によってリンクしにくいコンポーネント間で、メッセージ ベースの通信を行うことができます。
ms.prod: xamarin
ms.assetid: EDFE7B19-C5FD-40D5-816C-FAE56532E885
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/08/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c817936c77764b95842226b9a9a31c26667d6d0f
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937450"
---
# <a name="xamarinforms-messagingcenter"></a>Xamarin.Forms の MessagingCenter

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/usingmessagingcenter)

発行/サブスクライブ パターンは、パブリッシャーがサブスクライバーと呼ばれる受信者を知らずに、メッセージを送信するメッセージング パターンです。 同様に、サブスクライバーは、パブリッシャーを知らずに特定のメッセージをリッスンします。

.NET のイベントでは、発行/サブスクライブ パターンが実装されます。こうしたイベントは、コントロールやそれを含むページなど、疎結合が不要な場合に、コンポーネント間の通信レイヤーに対して最もシンプルで簡単な方法です。 ただし、パブリッシャーとサブスクライバーの有効期間はオブジェクト参照によって相互に結合され、サブスクライバーの種類にはパブリッシャーの種類への参照が含まれている必要があります。 これにより、特に、静的なオブジェクトまたは有効期間の長いオブジェクトのイベントをサブスクライブする、有効期間が短いオブジェクトがある場合に、メモリ管理の問題が発生する可能性があります。 イベント ハンドラーが削除されていない場合、サブスクライバーは、パブリッシャー内のサブスクライバーへの参照によって保持されます。これにより、サブスクライバーのガベージ コレクションが妨げられるか、または遅延します。

Xamarin.Forms の [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) クラスでは、発行/サブスクライブ パターンが実装され、オブジェクトと型の参照によってリンクしにくいコンポーネント間で、メッセージ ベースの通信を行うことができます。 このメカニズムにより、パブリッシャーとサブスクライバーは相互に参照することなく通信できるため、相互の依存関係を減らすことができます。

[`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) クラスでは、マルチキャストの発行/サブスクライブ機能を提供します。 つまり、1 つのメッセージをパブリッシュする複数のパブリッシャーが存在し、同じメッセージをリッスンしている複数のサブスクライバーが存在する可能性があるということです。

![マルチキャストの発行/サブスクライブ機能](messaging-center-images/messaging-center.png)

パブリッシャーは [`MessagingCenter.Send`](xref:Xamarin.Forms.MessagingCenter.Send*) メソッドを使用してメッセージを送信しますが、サブスクライバーは [`MessagingCenter.Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) メソッドを使用してメッセージをリッスンします。 さらに、サブスクライバーは、必要に応じて [`MessagingCenter.Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) メソッドを使用して、メッセージ サブスクリプションを解除することもできます。

> [!IMPORTANT]
> 内部的には、[`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) クラスでは弱参照が使用されます。 つまり、オブジェクトはアクティブに保持されず、ガベージ コレクションの実行が可能になります。 そのため、クラスでメッセージを受信する必要がなくなった場合にのみ、メッセージのサブスクリプションを解除する必要があります。

## <a name="publish-a-message"></a>メッセージの発行

[`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) メッセージは文字列です。 パブリッシャーは、[`MessagingCenter.Send`](xref:Xamarin.Forms.MessagingCenter.Send*) オーバーロードの 1 つを使用してメッセージをサブスクライバーに通知します。 次のコード例では、`Hi` メッセージを発行します。

```csharp
MessagingCenter.Send<MainPage>(this, "Hi");
```

この例では、[`Send`](xref:Xamarin.Forms.MessagingCenter.Send*) メソッドによって、送信者を表すジェネリック引数を指定します。 また、メッセージを受信するには、サブスクライバーが同じジェネリック引数を指定する必要もあります。これは、その送信者からのメッセージをリッスンしていることを示します。 さらに、この例では 2 つのメソッド引数を指定しています。

- 最初の引数では、送信者インスタンスを指定します。
- 2 番目の引数では、メッセージを指定します。

また、ペイロード データは、メッセージと共に送信することもできます。

```csharp
MessagingCenter.Send<MainPage, string>(this, "Hi", "John");
```

この例では、[`Send`](xref:Xamarin.Forms.MessagingCenter.Send*) メソッドで 2 つのジェネリック引数を指定します。 1 つ目はメッセージを送信している型で、2 つ目は送信されているペイロード データの型です。 メッセージを受信するには、サブスクライバーでも同じジェネリック引数を指定する必要があります。 これにより、メッセージ ID を共有するものの、異なるペイロード データ型を送信する複数のメッセージを、異なるサブスクライバーが受信することができます。 さらに、この例では、サブスクライバーに送信されるペイロード データを表す 3 番目のメソッド引数を指定しています。 この場合、ペイロード データは `string` です。

[`Send`](xref:Xamarin.Forms.MessagingCenter.Send*) メソッドでは、ファイア アンド フォーゲット アプローチを使用して、メッセージと任意のペイロード データが発行されます。 そのため、メッセージを受信するために登録されたサブスクライバーがない場合でも、メッセージが送信されます。 この状態では、送信されたメッセージは無視されます。

## <a name="subscribe-to-a-message"></a>メッセージのサブスクライブ

サブスクライバーは、[`MessagingCenter.Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) オーバーロードのいずれかを使用して、メッセージを受信するように登録できます。 次のコード例は、この例を示しています。

```csharp
MessagingCenter.Subscribe<MainPage> (this, "Hi", (sender) =>
{
    // Do something whenever the "Hi" message is received
});
```

この例では、[`Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) メソッドは、`MainPage` 型によって送信された `Hi` メッセージに対して `this` オブジェクトをサブスクライブし、メッセージの受信に応答してコールバック デリゲートを実行します。 ラムダ式として指定されたコールバック デリゲートは、UI を更新したり、データを保存したり、他の操作をトリガーしたりするコードになる可能性があります。

> [!NOTE]
> サブスクライバーは、パブリッシュされたメッセージのすべてのインスタンスを処理する必要はなく、これは [`Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) メソッドで指定されたジェネリック型引数によって制御できます。

次の例は、ペイロード データを含むメッセージをサブスクライブする方法を示しています。

```csharp
MessagingCenter.Subscribe<MainPage, string>(this, "Hi", async (sender, arg) =>
{
    await DisplayAlert("Message received", "arg=" + arg, "OK");
});
```

この例では、[`Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) メソッドでペイロード データが `string` である `MainPage` 型によって送信された `Hi` メッセージをサブスクライブします。 コールバック デリゲートは、このようなメッセージの受信に応答して実行されます。これにより、アラートのペイロード データが表示されます。

> [!IMPORTANT]
> `Subscribe` メソッドによって実行されるデリゲートは、`Send` メソッドを使用してメッセージを発行するのと同じスレッド上で実行されます。

## <a name="unsubscribe-from-a-message"></a>メッセージのサブスクリプションの解除

サブスクライバーは、受信する必要がなくなったメッセージのサブスクライブを解除することができます。 これは、いずれかの [`MessagingCenter.Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) オーバーロードで実現されます。

```csharp
MessagingCenter.Unsubscribe<MainPage>(this, "Hi");
```

この例では、[`Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) メソッドで、`MainPage` 型によって送信された `Hi` メッセージから `this` オブジェクトのサブスクリプションを解除します。

ペイロード データを含むメッセージは、次の 2 つのジェネリック引数を指定する [`Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) オーバーロードを使用して、サブスクリプションを解除する必要があります。

```csharp
MessagingCenter.Unsubscribe<MainPage, string>(this, "Hi");
```

この例では、[`Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) メソッドで、ペイロード データが `string` である `MainPage` 型によって送信された `Hi` メッセージから `this` オブジェクトのサブスクリプションを解除します。

## <a name="related-links"></a>関連リンク

- [MessagingCenterSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/usingmessagingcenter)
