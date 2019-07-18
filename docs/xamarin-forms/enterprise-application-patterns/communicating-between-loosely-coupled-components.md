---
title: 疎結合コンポーネント間の通信
description: 'この章では、eShopOnContainers のモバイル アプリで、発行の実装方法について説明します-サブスクライブ パターンでは、オブジェクトと型を参照することでリンクするは便利ではないコンポーネント間のメッセージ ベースの通信を許可します。 '
ms.prod: xamarin
ms.assetid: 1194af33-8a91-48d2-88b5-b84d77f2ce69
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 9848d2b832990032bc7eb7f2e3a93c896457134c
ms.sourcegitcommit: e95296f9e516975f5f32d822c323a71fd84007b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538696"
---
# <a name="communicating-between-loosely-coupled-components"></a>疎結合コンポーネント間の通信

発行-サブスクライブ パターンはパブリッシャーがサブスクライバーと呼ばれる、すべての受信者の知識をしなくてもメッセージを送信するメッセージング パターンです。 同様に、サブスクライバーはパブリッシャーの知識がなくてせず、特定のメッセージを待機します。

.NET でのイベントの実装、発行-サブスクライブ パターンでは、最も単純でわかりやすいアプローチ通信レイヤーの疎結合が必要ない場合、コントロールを含むページなどのコンポーネント間で、します。 ただし、パブリッシャーとサブスクライバーの有効期間は、相互にオブジェクト参照によって結合されて、サブスクライバーの種類は、パブリッシャーの種類への参照をいる必要があります。 メモリ管理の問題は、静的または有効期間が長いオブジェクトのイベントにサブスクライブする存続期間の短いオブジェクトがある場合に特ににこの作成できます。 イベント ハンドラー削除されない場合、サブスクライバーが維持される、パブリッシャーでは、それへの参照によって、これを防止またはサブスクライバーのガベージ コレクションは遅延されます。

## <a name="introduction-to-messagingcenter"></a>MessagingCenter の概要

Xamarin.Forms [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter)クラスは、発行-サブスクライブ パターンでは、オブジェクトと型を参照することでリンクするは便利ではないコンポーネント間のメッセージ ベースの通信を許可します。 このメカニズムは、コンポーネントを個別に開発してテストできるようになります、コンポーネント間の依存関係を削減すること、相互に参照をしなくても通信するには、パブリッシャーとサブスクライバーを使用します。

[ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter)クラスは、マルチキャスト パブリッシュ/サブスクライブ機能を提供します。 これは、1 つのメッセージを公開する複数のパブリッシャーがあり、複数のサブスクライバーが、同じメッセージをリッスンしている可能性があることを意味します。 図 4-1 は、この関係を示しています。

![](communicating-between-loosely-coupled-components-images/messagingcenter.png "マルチキャスト パブリッシュ/サブスクライブの機能")

**図 4-1:** マルチキャスト パブリッシュ/サブスクライブの機能

パブリッシャーを使用してメッセージを送信、 [ `MessagingCenter.Send` ](xref:Xamarin.Forms.MessagingCenter.Send*)メソッドを使用してメッセージのサブスクライバーを待機中に、 [ `MessagingCenter.Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*)メソッド。 さらに、サブスクライバーも登録を解除できますメッセージのサブスクリプションから、必要な場合に、 [ `MessagingCenter.Unsubscribe` ](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*)メソッド。

内部的には、 [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter)クラスは、弱い参照を使用します。 つまりはないオブジェクトを生かしておく、およびガベージ コレクションを可能に。 そのため、する必要がありますのみ必要になるクラスがメッセージを受信するが不要になったときに、メッセージから登録を解除します。

EShopOnContainers のモバイル アプリでは、 [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter)クラス間の通信を疎結合コンポーネント。 アプリでは、3 つのメッセージを定義します。

-   `AddProduct`によってメッセージが発行された、`CatalogViewModel`買い物かごにアイテムが追加されるクラスします。 代わりに、`BasketViewModel`クラスは、メッセージをサブスクライブして、応答で買い物かごにアイテムの数をインクリメントします。 さらに、`BasketViewModel`クラスは、このメッセージからもアンサブスク ライブします。
-   `Filter`によってメッセージが発行された、`CatalogViewModel`クラスのユーザーが、カタログから表示される項目にブランドまたは型のフィルターを適用するとき。 代わりに、`CatalogView`クラスは、メッセージをサブスクライブして、フィルター条件に一致する項目のみが表示されるように、UI を更新します。
-   `ChangeTab`によってメッセージが発行された、`MainViewModel`クラス、`CheckoutViewModel`に移動、`MainViewModel`が正常に作成、新しい注文の送信。 代わりに、`MainView`クラス サブスクライブ メッセージと、UI の更新プログラムにこれを**プロフィール** タブがアクティブで、ユーザーの注文を表示します。

> [!NOTE]
> 中に、 [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter)クラス疎結合のクラス間の通信を許可する、この問題にのみアーキテクチャのソリューションを提供しません。 たとえば、ビュー モデルとビュー間の通信は、バインディング エンジンとプロパティの変更通知にも実現できます。 さらに、2 つのビュー モデル間の通信は、ナビゲーション中にデータを渡すことによっても実現できます。

EShopOnContainers のモバイル アプリで[ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter)別のクラスで発生している、アクションに対する応答では、UI の更新に使用されます。 そのため、メッセージは、同じスレッドでメッセージを受信するサブスクライバーで、UI スレッドで発行されます。

> [!TIP]
> 更新プログラムの UI を実行するときに、UI スレッドにマーシャ リングします。 バック グラウンド スレッドから送信されるメッセージが UI を更新する必要な場合は、呼び出すことによって、サブスクライバーで、UI スレッドでメッセージを処理、 [ `Device.BeginInvokeOnMainThread` ](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action))メソッド。

詳細については[ `MessagingCenter`](xref:Xamarin.Forms.MessagingCenter)を参照してください[MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md)します。

## <a name="defining-a-message"></a>メッセージを定義します。

[`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) メッセージは、メッセージを識別するために使用される文字列です。 次のコード例では、eShopOnContainers のモバイル アプリ内で定義されているメッセージを示します。

```csharp
public class MessengerKeys  
{  
    // Add product to basket  
    public const string AddProduct = "AddProduct";  

    // Filter  
    public const string Filter = "Filter";  

    // Change selected Tab programmatically  
    public const string ChangeTab = "ChangeTab";  
}
```

この例では、メッセージは、定数を使用して定義されます。 このアプローチの利点は、コンパイル時の型の安全性とリファクタリングのサポートを提供します。

## <a name="publishing-a-message"></a>メッセージを公開します。

発行元のいずれかのメッセージのサブスクライバーに通知、 [ `MessagingCenter.Send` ](xref:Xamarin.Forms.MessagingCenter.Send*)オーバー ロードします。 次のコード例に示します発行、`AddProduct`メッセージ。

```csharp
MessagingCenter.Send(this, MessengerKeys.AddProduct, catalogItem);
```

この例で、 [ `Send` ](xref:Xamarin.Forms.MessagingCenter.Send*)メソッドは、3 つの引数を指定します。

-   最初の引数には、送信元のクラスを指定します。 メッセージの受信を希望するすべてのサブスクライバーでは、送信元のクラスを指定してください。
-   2 番目の引数には、メッセージを指定します。
-   3 番目の引数には、サブスクライバーに送信されるペイロード データを指定します。 この場合、ペイロード データは、`CatalogItem`インスタンス。

[ `Send` ](xref:Xamarin.Forms.MessagingCenter.Send*)メソッドが、メッセージと、ファイア アンド フォーゲット アプローチを使用して、そのペイロード データに発行されます。 そのため、メッセージを受信する登録されているサブスクライバーがない場合でも、メッセージが送信されます。 このような状況では、送信されたメッセージは無視されます。

> [!NOTE]
> [ `MessagingCenter.Send` ](xref:Xamarin.Forms.MessagingCenter.Send*)メソッドでジェネリック パラメーターを使用してメッセージを配信する方法を制御できます。 そのため、異なるサブスクライバーで、メッセージ id を共有しますが、さまざまなペイロードのデータ型を送信する複数のメッセージを受信できます。

## <a name="subscribing-to-a-message"></a>メッセージのサブスクライブ

いずれかを使用してメッセージを受信するサブスクライバーを登録できます、 [ `MessagingCenter.Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*)オーバー ロードします。 次のコード例を示します、eShopOnContainers のモバイル アプリのサブスクライブし、処理、`AddProduct`メッセージ。

```csharp
MessagingCenter.Subscribe<CatalogViewModel, CatalogItem>(  
    this, MessageKeys.AddProduct, async (sender, arg) =>  
{  
    BadgeCount++;  

    await AddCatalogItemAsync(arg);  
});
```

この例で、 [ `Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*)メソッドをサブスクライブする、`AddProduct`メッセージ、および応答メッセージを受信するコールバック デリゲートを実行します。 ラムダ式として指定された、このコールバック デリゲートは、UI を更新するコードを実行します。

> [!TIP]
> 変更できないペイロード データを使用してください。 複数のスレッドにアクセスすること、受信したデータに同時にため、コールバック デリゲート内からペイロード データを変更する避けてください。 このシナリオでは、ペイロード データを同時実行エラーを回避するために変更できません必要があります。

サブスクライバーがパブリッシュされたメッセージのすべてのインスタンスを処理する必要はありませんし、これは、ジェネリック型引数で指定されているで制御できます、 [ `Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*)メソッド。 この例で、サブスクライバーが受信のみが`AddProduct`から送信されるメッセージ、`CatalogViewModel`ペイロード データが、クラス、`CatalogItem`インスタンス。

## <a name="unsubscribing-from-a-message"></a>メッセージから登録を解除します。

サブスクライバーからメッセージを受信する必要がなくなった登録を解除できます。 いずれかでこれは、 [ `MessagingCenter.Unsubscribe` ](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*)次のコード例に示すように、オーバー ロードします。

```csharp
MessagingCenter.Unsubscribe<CatalogViewModel, CatalogItem>(this, MessengerKeys.AddProduct);
```

この例で、 [ `Unsubscribe` ](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*)メソッド構文がサブスクライブを受信するときに指定された型引数を反映して、`AddProduct`メッセージ。

## <a name="summary"></a>まとめ

Xamarin.Forms [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter)クラスは、発行-サブスクライブ パターンでは、オブジェクトと型を参照することでリンクするは便利ではないコンポーネント間のメッセージ ベースの通信を許可します。 このメカニズムは、コンポーネントを個別に開発してテストできるようになります、コンポーネント間の依存関係を削減すること、相互に参照をしなくても通信するには、パブリッシャーとサブスクライバーを使用します。


## <a name="related-links"></a>関連リンク

- [(2 Mb の PDF) 電子ブックをダウンロードします。](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
