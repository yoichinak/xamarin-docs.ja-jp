---
title: コンポーネント間の通信を疎結合
description: 'この章では、eShopOnContainers モバイル アプリの発行を実装する方法について説明します-定期受信パターン、オブジェクトと型の参照によってリンクするときに便利ではないコンポーネント間のメッセージ ベースの通信を許可します。 '
ms.prod: xamarin
ms.assetid: 1194af33-8a91-48d2-88b5-b84d77f2ce69
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 797032d17babe986de1357c6ac3291a4960d87ff
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245082"
---
# <a name="communicating-between-loosely-coupled-components"></a>コンポーネント間の通信を疎結合

発行の定期受信パターンが、発行元がサブスクライバーと呼ばれる、すべての受信者のナレッジをしなくてもメッセージを送信するメッセージング パターン。 同様に、サブスクライバーはパブリッシャーの知識がなくてせず、特定のメッセージを待機します。

.NET でのイベントを実装、発行の定期受信パターン、および最も単純で簡単なアプローチ通信レイヤーの疎結合が、コントロールと含まれているページなど、必要でない場合、コンポーネント間では、します。 ただし、パブリッシャーとサブスクライバーの有効期間は、相互にオブジェクト参照で結合され、サブスクライバーの種類は、パブリッシャーの種類への参照を持つ必要があります。 メモリ管理に関する問題は、静的であるか有効期間が長いオブジェクトのイベントにサブスクライブ存続期間の短いオブジェクトがある場合に特ににこの作成できます。 イベント ハンドラーが削除されていない場合、サブスクライバーはによって維持されなければ、パブリッシャーでそれへの参照を実行し、これを防ぐまたはサブスクライバーのガベージ コレクションは遅延を実行します。

## <a name="introduction-to-messagingcenter"></a>MessagingCenter の概要

Xamarin.Forms [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/)クラスは、発行、実装の定期受信パターン、オブジェクトと型の参照によってリンクするときに便利ではないコンポーネント間のメッセージ ベースの通信を許可します。 このメカニズムにより、パブリッシャーとサブスクライバーは、コンポーネントを個別に開発、テストすることもできますしながら、コンポーネント間の依存関係を削減すること、相互に参照しなくても通信します。

[ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/)クラスは、マルチキャスト パブリッシュ/サブスクライブ機能を提供します。 これは、1 つのメッセージを公開する複数のパブリッシャーがあるし、同じメッセージをリッスンしている複数のサブスクライバーがあることを意味します。 図 4-1 は、このリレーションシップを示しています。

![](communicating-between-loosely-coupled-components-images/messagingcenter.png "マルチキャスト公開/定期受信機能")

**図 4-1:** マルチキャスト公開/定期受信機能

発行元を使用してメッセージを送信する、 [ `MessagingCenter.Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender%7D/p/TSender/System.String/)メソッド、サブスクライバーを使用してメッセージを待機中に、 [ `MessagingCenter.Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/)メソッドです。 さらに、サブスクライバーまたメッセージ サブスクリプションから、必要な場合に、 [ `MessagingCenter.Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender%7D/p/System.Object/System.String/)メソッドです。

内部的には、 [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/)クラスは、弱い参照を使用します。 つまりことはない維持オブジェクト、およびガベージ コレクションを可能にします。 したがって、する必要がありますのみ必要になるクラスがメッセージを受信するが不要になったときに、メッセージのサブスクリプションを解除します。

EShopOnContainers モバイル アプリの使用、 [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/)疎間の通信にクラスは、コンポーネントを結合します。 アプリには、3 つのメッセージが定義されています。

-   `AddProduct`によってメッセージが発行された、`CatalogViewModel`買い物かごに項目が追加されるクラスです。 代わりに、`BasketViewModel`クラスがメッセージをサブスクライブし、応答で買い物かごの中の項目の数をインクリメントします。 さらに、`BasketViewModel`クラスは、このメッセージからもアンサブスク ライブします。
-   `Filter`によってメッセージが発行された、`CatalogViewModel`クラスのユーザーにいつ適用される、ブランド、または型のフィルターをカタログから表示する項目。 代わりに、`CatalogView`クラスがメッセージをサブスクライブし、フィルター条件に一致する項目のみが表示されるように、UI を更新します。
-   `ChangeTab`によってメッセージが発行された、`MainViewModel`場合にクラス、`CheckoutViewModel`に移動、`MainViewModel`次の作成に成功し、新しい注文の送信。 代わりに、`MainView`クラスを定期受信メッセージと、UI の更新プログラムようにする、**プロフィール**タブは、アクティブなユーザーの注文を表示します。

> [!NOTE]
> 中に、 [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/)クラスは疎結合のクラス間の通信を許可、アーキテクチャのみこの問題の解決策を提供しません。 たとえば、ビュー モデルとビューの間の通信は、バインディング エンジンによって、プロパティの変更通知を介したにも実現できます。 さらに、2 つのビュー モデル間の通信はナビゲーション中にデータを渡すことによっても実現できます。

EShopOnContainers のモバイル アプリで[`MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/)別のクラスで発生している操作への応答で ui を更新するために使用します。 そのため、メッセージは、UI スレッドで同じスレッドでメッセージを受信するサブスクライバーにパブリッシュされます。

> [!TIP]
> 更新プログラムの UI を実行する場合に、UI スレッドにマーシャ リングします。 呼び出すことによって、サブスクライバーで、UI スレッドでメッセージの処理はバック グラウンド スレッドから送信されるメッセージが、UI を更新する必要な場合、 [ `Device.BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/)メソッドです。

詳細については[ `MessagingCenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/)を参照してください[MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md)です。

## <a name="defining-a-message"></a>メッセージを定義します。

[`MessagingCenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) メッセージは、メッセージを識別するために使用する文字列です。 次のコード例は、eShopOnContainers モバイル アプリ内で定義されているメッセージを示しています。

```csharp
public class MessengerKeys  
{  
    // Add product to basket  
    public const string AddProduct = "AddProduct";  

    // Filter  
    public const string Filter = "Filter";  

    // Change selected Tab programmatically  
    public const string ChangeTab = "ChangeTab";  
}
```

この例では、定数を使用するメッセージが定義されます。 この方法の利点は、コンパイル時の型の安全性とリファクタリングのサポートを提供することです。

## <a name="publishing-a-message"></a>メッセージを公開します。

パブリッシャーのいずれかのメッセージのサブスクライバーの通知、 [ `MessagingCenter.Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/)オーバー ロードします。 次のコード例に示します発行、`AddProduct`メッセージ。

```csharp
MessagingCenter.Send(this, MessengerKeys.AddProduct, catalogItem);
```

この例では、 [ `Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/)メソッドは、3 つの引数を指定します。

-   最初の引数は、送信元のクラスを指定します。 メッセージの受信を希望するすべてのサブスクライバーでは、送信元のクラスを指定してください。
-   2 番目の引数は、メッセージを指定します。
-   3 番目の引数は、サブスクライバーに送信されるペイロード データを指定します。 この場合、ペイロード データは、`CatalogItem`インスタンス。

[ `Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/)メソッドは、メッセージ、および火災忘れたアプローチを使用して、そのペイロード データを発行します。 そのため、メッセージの受信に登録されているサブスクライバーが存在しない場合でも、メッセージが送信されます。 このような状況では、送信されたメッセージは無視されます。

> [!NOTE]
> [ `MessagingCenter.Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/)方法は、ジェネリック パラメーターを使用してメッセージを配信する方法を制御することができます。 したがって、異なるサブスクライバーでさまざまなペイロードのデータ型を送信では、メッセージ id を共有する複数のメッセージを受信できます。

## <a name="subscribing-to-a-message"></a>メッセージのサブスクライブ

いずれかを使用してメッセージを受信するサブスクライバーを登録できます、 [ `MessagingCenter.Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/)オーバー ロードします。 次のコード例は、eShopOnContainers モバイル アプリをサブスクライブしているし、処理を示しています、`AddProduct`メッセージ。

```csharp
MessagingCenter.Subscribe<CatalogViewModel, CatalogItem>(  
    this, MessageKeys.AddProduct, async (sender, arg) =>  
{  
    BadgeCount++;  

    await AddCatalogItemAsync(arg);  
});
```

この例では、 [ `Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/)をサブスクライブする方法、`AddProduct`メッセージ、およびメッセージを受信したときにコールバック デリゲートを実行します。 ラムダ式として指定された、このコールバック デリゲートは、UI を更新するコードを実行します。

> [!TIP]
> 変更できないペイロード データを使用してください。 複数のスレッドにアクセスすること、受信したデータに同時にあるために、コールバック デリゲート内からペイロード データを変更しようとしてはありません。 このシナリオでは、ペイロード データは変更できません同時実行エラーを回避します。

サブスクライバーが、公開されたメッセージのすべてのインスタンスを処理する必要がありますいないと、こので指定されたジェネリック型引数によって制御できます、 [ `Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/)メソッドです。 この例では、サブスクライバーのみを受け取ります`AddProduct`から送信されるメッセージ、`CatalogViewModel`クラス、ペイロード データが含まれる、`CatalogItem`インスタンス。

## <a name="unsubscribing-from-a-message"></a>メッセージの登録解除

サブスクライバーは、メッセージを受信する必要がなくなったを取り消しできます。 いずれかでこれを実現する、 [ `MessagingCenter.Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender,TArgs%7D/p/System.Object/System.String/)次のコード例に示すように、オーバー ロードします。

```csharp
MessagingCenter.Unsubscribe<CatalogViewModel, CatalogItem>(this, MessengerKeys.AddProduct);
```

この例では、 [ `Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender,TArgs%7D/p/System.Object/System.String/)メソッド構文を受信登録するときに指定された型引数を反映して、`AddProduct`メッセージ。

## <a name="summary"></a>まとめ

Xamarin.Forms [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/)クラスは、発行、実装の定期受信パターン、オブジェクトと型の参照によってリンクするときに便利ではないコンポーネント間のメッセージ ベースの通信を許可します。 このメカニズムにより、パブリッシャーとサブスクライバーは、コンポーネントを個別に開発、テストすることもできますしながら、コンポーネント間の依存関係を削減すること、相互に参照しなくても通信します。


## <a name="related-links"></a>関連リンク

- [電子 (2 Mb PDF) のダウンロードします。](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
