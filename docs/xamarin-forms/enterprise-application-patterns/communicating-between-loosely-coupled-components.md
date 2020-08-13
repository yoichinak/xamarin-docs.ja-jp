---
title: 疎結合コンポーネント間の通信
description: 'この章では、eShopOnContainers モバイルアプリがパブリッシュ/サブスクライブパターンを実装する方法について説明します。これにより、オブジェクトと型の参照によってリンクするのが不便なコンポーネント間でのメッセージベースの通信が可能になります。 '
ms.prod: xamarin
ms.assetid: 1194af33-8a91-48d2-88b5-b84d77f2ce69
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: bf0315d2d077e06ff3ded4d66814afe050fdfad4
ms.sourcegitcommit: f7fe46c0236a7130b63a33d9d1670d5111582dd2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/13/2020
ms.locfileid: "88186201"
---
# <a name="communicating-between-loosely-coupled-components"></a>疎結合コンポーネント間の通信

発行/サブスクライブ パターンは、パブリッシャーがサブスクライバーと呼ばれる受信者を知らずに、メッセージを送信するメッセージング パターンです。 同様に、サブスクライバーは、パブリッシャーを知らずに特定のメッセージをリッスンします。

.NET のイベントでは、発行/サブスクライブ パターンが実装されます。こうしたイベントは、コントロールやそれを含むページなど、疎結合が不要な場合に、コンポーネント間の通信レイヤーに対して最もシンプルで簡単な方法です。 ただし、パブリッシャーとサブスクライバーの有効期間はオブジェクト参照によって相互に結合され、サブスクライバーの種類にはパブリッシャーの種類への参照が含まれている必要があります。 これにより、特に、静的なオブジェクトまたは有効期間の長いオブジェクトのイベントをサブスクライブする、有効期間が短いオブジェクトがある場合に、メモリ管理の問題が発生する可能性があります。 イベント ハンドラーが削除されていない場合、サブスクライバーは、パブリッシャー内のサブスクライバーへの参照によって保持されます。これにより、サブスクライバーのガベージ コレクションが妨げられるか、または遅延します。

## <a name="introduction-to-messagingcenter"></a>MessagingCenter の概要

Xamarin.Forms の [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) クラスでは、発行/サブスクライブ パターンが実装され、オブジェクトと型の参照によってリンクしにくいコンポーネント間で、メッセージ ベースの通信を行うことができます。 このメカニズムにより、パブリッシャーとサブスクライバーは相互に参照がなくても通信できるようになり、コンポーネント間の依存関係を軽減しながら、コンポーネントを個別に開発およびテストすることができます。

[`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) クラスでは、マルチキャストの発行/サブスクライブ機能を提供します。 つまり、1つのメッセージをパブリッシュする複数のパブリッシャーが存在し、同じメッセージをリッスンしている複数のサブスクライバーが存在する可能性があります。 図4-1 は、この関係を示しています。

![マルチキャストの発行/サブスクライブ機能](communicating-between-loosely-coupled-components-images/messagingcenter.png)

**図 4-1:** マルチキャスト発行/サブスクライブ機能

パブリッシャーは [`MessagingCenter.Send`](xref:Xamarin.Forms.MessagingCenter.Send*) メソッドを使用してメッセージを送信しますが、サブスクライバーは [`MessagingCenter.Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) メソッドを使用してメッセージをリッスンします。 さらに、サブスクライバーは、必要に応じて [`MessagingCenter.Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) メソッドを使用して、メッセージ サブスクリプションを解除することもできます。

内部的には、[`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) クラスでは弱参照が使用されます。 つまり、オブジェクトはアクティブに保持されず、ガベージ コレクションの実行が可能になります。 そのため、クラスでメッセージを受信する必要がなくなった場合にのみ、メッセージのサブスクリプションを解除する必要があります。

EShopOnContainers モバイルアプリでは、クラスを使用して [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) 疎結合コンポーネント間の通信を行います。 アプリでは、次の3つのメッセージを定義します。

- この `AddProduct` メッセージは、 `CatalogViewModel` 買い物かごに項目が追加されたときにクラスによって発行されます。 返されたクラスは、 `BasketViewModel` メッセージをサブスクライブし、応答としてショッピングカート内の項目の数を増やします。 また、 `BasketViewModel` クラスもこのメッセージからアンサブスクライブします。
- この `Filter` メッセージは、 `CatalogViewModel` ユーザーがカタログから表示される項目にブランドまたは種類のフィルターを適用すると、クラスによって発行されます。 返されたクラスは、 `CatalogView` メッセージをサブスクライブし、フィルター条件に一致する項目のみが表示されるように UI を更新します。
- `ChangeTab` `MainViewModel` が `CheckoutViewModel` `MainViewModel` 新しい注文の作成と送信に成功した後に、クラスによってメッセージが発行されます。 返されたクラスは、 `MainView` メッセージをサブスクライブし、 **[プロファイル**] タブがアクティブになるように UI を更新して、ユーザーの注文を表示します。

> [!NOTE]
> クラスは [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) 疎結合クラス間の通信を許可しますが、この問題に対する唯一のアーキテクチャソリューションを提供するわけではありません。 たとえば、ビューモデルとビュー間の通信は、バインディングエンジンとプロパティ変更通知を使用して実現することもできます。 また、ナビゲーション中にデータを渡すことによって、2つのビューモデル間の通信を実現することもできます。

EShopOnContainers モバイルアプリでは、を [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) 使用して、別のクラスで発生するアクションに応答して UI でを更新します。 そのため、メッセージは UI スレッドで公開され、サブスクライバーは同じスレッドでメッセージを受信します。

> [!TIP]
> UI の更新を実行するときに UI スレッドにマーシャリングします。 バックグラウンドスレッドから送信されたメッセージが UI を更新する必要がある場合は、メソッドを呼び出して、サブスクライバーの UI スレッドでメッセージを処理し [`Device.BeginInvokeOnMainThread`](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)) ます。

の詳細について [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) は、「 [Messagingcenter](~/xamarin-forms/app-fundamentals/messaging-center.md)」を参照してください。

## <a name="defining-a-message"></a>メッセージの定義

[`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter)メッセージは、メッセージを識別するために使用される文字列です。 次のコード例は、eShopOnContainers モバイルアプリ内で定義されているメッセージを示しています。

```csharp
public class MessageKeys  
{  
    // Add product to basket  
    public const string AddProduct = "AddProduct";  

    // Filter  
    public const string Filter = "Filter";  

    // Change selected Tab programmatically  
    public const string ChangeTab = "ChangeTab";  
}
```

この例では、メッセージは定数を使用して定義されています。 この方法の利点は、コンパイル時の型の安全性とリファクタリングのサポートが提供されることです。

## <a name="publishing-a-message"></a>メッセージの公開

パブリッシャーは、[`MessagingCenter.Send`](xref:Xamarin.Forms.MessagingCenter.Send*) オーバーロードの 1 つを使用してメッセージをサブスクライバーに通知します。 次のコード例は、メッセージを公開する方法を示してい `AddProduct` ます。

```csharp
MessagingCenter.Send(this, MessageKeys.AddProduct, catalogItem);
```

この例では、 [`Send`](xref:Xamarin.Forms.MessagingCenter.Send*) メソッドは次の3つの引数を指定します。

- 最初の引数は、sender クラスを指定します。 Sender クラスは、メッセージの受信を希望するすべてのサブスクライバーによって指定される必要があります。
- 2 番目の引数では、メッセージを指定します。
- 3番目の引数は、サブスクライバーに送信されるペイロードデータを指定します。 この場合、ペイロードデータは `CatalogItem` インスタンスです。

メソッドは、 [`Send`](xref:Xamarin.Forms.MessagingCenter.Send*) 火災および忘れる方法を使用して、メッセージとそのペイロードデータを公開します。 そのため、メッセージを受信するために登録されたサブスクライバーがない場合でも、メッセージが送信されます。 この状態では、送信されたメッセージは無視されます。

> [!NOTE]
> メソッドは、ジェネリックパラメーターを使用して、 [`MessagingCenter.Send`](xref:Xamarin.Forms.MessagingCenter.Send*) メッセージの配信方法を制御できます。 そのため、メッセージ id を共有し、異なるペイロードデータ型を送信する複数のメッセージを、異なるサブスクライバーで受信できます。

## <a name="subscribing-to-a-message"></a>メッセージのサブスクライブ

サブスクライバーは、[`MessagingCenter.Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) オーバーロードのいずれかを使用して、メッセージを受信するように登録できます。 次のコード例は、eShopOnContainers モバイルアプリがメッセージをサブスクライブし、処理する方法を示してい `AddProduct` ます。

```csharp
MessagingCenter.Subscribe<CatalogViewModel, CatalogItem>(  
    this, MessageKeys.AddProduct, async (sender, arg) =>  
{  
    BadgeCount++;  

    await AddCatalogItemAsync(arg);  
});
```

この例では、 [`Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) メソッドはメッセージをサブスクライブ `AddProduct` し、メッセージの受信に応答してコールバックデリゲートを実行します。 このコールバックデリゲートは、ラムダ式として指定され、UI を更新するコードを実行します。

> [!TIP]
> 変更できないペイロードデータを使用することを検討してください。 複数のスレッドが受信したデータに同時にアクセスする可能性があるため、コールバックデリゲート内からペイロードデータを変更しないようにしてください。 このシナリオでは、同時実行エラーを回避するためにペイロードデータが不変である必要があります。

サブスクライバーは、パブリッシュされたメッセージのすべてのインスタンスを処理する必要はなく、これは [`Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) メソッドで指定されたジェネリック型引数によって制御できます。 この例では、サブスクライバーは、 `AddProduct` `CatalogViewModel` ペイロードデータがインスタンスであるクラスから送信されたメッセージのみを受信し `CatalogItem` ます。

## <a name="unsubscribing-from-a-message"></a>メッセージのサブスクライブを解除する

サブスクライバーは、受信する必要がなくなったメッセージのサブスクライブを解除することができます。 これは、 [`MessagingCenter.Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) 次のコード例に示すように、オーバーロードのいずれかを使用して実現されます。

```csharp
MessagingCenter.Unsubscribe<CatalogViewModel, CatalogItem>(this, MessageKeys.AddProduct);
```

この例では、 [`Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) メソッドの構文は、メッセージを受信するためにサブスクライブするときに指定された型引数を反映して `AddProduct` います。

## <a name="summary"></a>まとめ

Xamarin.Forms の [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) クラスでは、発行/サブスクライブ パターンが実装され、オブジェクトと型の参照によってリンクしにくいコンポーネント間で、メッセージ ベースの通信を行うことができます。 このメカニズムにより、パブリッシャーとサブスクライバーは相互に参照がなくても通信できるようになり、コンポーネント間の依存関係を軽減しながら、コンポーネントを個別に開発およびテストすることができます。

## <a name="related-links"></a>関連リンク

- [電子ブックのダウンロード (2 Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
