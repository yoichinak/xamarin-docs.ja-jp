---
title: Xamarin. iOS のサブスクリプションとレポート
description: このドキュメントでは、非更新サブスクリプション、無料のサブスクリプション、自動更新可能なサブスクリプションについて説明し、iTunes Connect を使用してこれらのアイテムに関するレポートを作成します。
ms.prod: xamarin
ms.assetid: 27EE4234-07F5-D2CD-DC1C-86E27C20141E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 534ecb6a2f779875a6934306c7f9956450d880ed
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938503"
---
# <a name="subscriptions-and-reporting-in-xamarinios"></a>Xamarin. iOS のサブスクリプションとレポート

## <a name="about-non-renewing-subscriptions"></a>更新以外のサブスクリプションについて

非更新サブスクリプションは、時間制限付きのサービスの販売を表す製品 (ナビゲーションアプリケーションへの1週間のアクセス、またはデータアーカイブへの期間限定のアクセスなど) を対象としています。   

更新以外のサブスクリプションとその他の製品の種類の主な違いは次のとおりです。

- ITunes Connect の製品定義には、という用語は含まれていません。 アプリケーションコードは、プロダクト ID から有効期間を推測できる必要があります。
- 複数回購入できます (使用可能な製品など)。 アプリケーションは、サブスクリプションの有効期限と更新を管理し、ユーザーが重複するサブスクリプションを購入できないようにする必要があります。
- この購入は、StoreKit の復元機能ではサポートされていません。 すべてのユーザーのデバイスでサブスクリプションを使用できるようにする必要がある場合は、この機能をリモートサーバーと組み合わせて設計および実装する必要があります。 また、アプリケーションは、デバイスがバックアップから復元された場合に、サブスクリプションの状態をバックアップする役割も担います。
- 実装の概要
- 更新以外のサブスクリプションは、通常、サーバー側で配信されるワークフローと、使用可能なマネージド製品を使用して実装する必要があります。

## <a name="about-free-subscriptions"></a>無料サブスクリプションについて

無料サブスクリプションを使用すると、開発者は無料のコンテンツを Newsstand アプリに配置できます (Newsstand 以外のアプリでは使用できません)。 無料のサブスクリプションが開始されると、すべてのユーザーのデバイスで使用できるようになります。 無料サブスクリプションは期限切れになりません。アプリケーションがアンインストールされたときにのみ終了します。

### <a name="implementation-overview"></a>実装の概要

無料サブスクリプションは、自動更新サブスクリプションと同じように動作します。 アプリケーションには、iTunes Connect で ' purchase ' に使用できる無料のサブスクリプション製品が必要です。 ユーザーが購入した場合は、無料サブスクリプションの購入を、自動更新可能なサブスクリプション製品と同様に検証する必要があります。 無料のサブスクリプショントランザクションは復元できます。

## <a name="about-auto-renewable-subscriptions"></a>自動更新サブスクリプションについて

自動更新可能なサブスクリプションは主に Newsstand アプリケーションで使用されます。 これらは、iTunes Connect で構成されている一定期間、ユーザーに動的コンテンツへのアクセスを許可する製品を表します (期間は7日から1年に設定します)。 サブスクリプションは自動的に更新されます。ユーザーが不在になった場合を除き、各サブスクリプション期間の終了時にユーザーの Apple ID が課金されます。この製品の種類は、サブスクリプションが有効である間に発行された各問題にユーザーがアクセスできる、雑誌または news サブスクリプションに適しています。

### <a name="implementation-overview"></a>実装の概要

自動更新可能なサブスクリプションは、サーバー配信製品ワークフローを使用して実装する必要があります (「確認*メッセージとサーバー配信製品*」セクションを参照してください)。

#### <a name="shared-secret"></a>共有シークレット

サーバーで自動更新可能なサブスクリプションを確認するときは、アプリ内購入共有シークレットを JSON 要求で使用する必要があります。 共有シークレットは、iTunes Connect を使用して作成またはアクセスされます。

ITunes Connect のホームページから、[**マイアプリ**] を選択します。   

 [![マイアプリを選択する](subscriptions-and-reporting-images/image2.png)](subscriptions-and-reporting-images/image2.png#lightbox)  

アプリケーションを選択し、[**アプリ内購入**] タブをクリックします。

[![[アプリ内購入] タブをクリックします。](subscriptions-and-reporting-images/image6.png)](subscriptions-and-reporting-images/image6.png#lightbox)

ページの下部にある [**共有シークレットの表示または生成**] を選択します。

 [![[共有シークレットの表示または生成] を選択します。](subscriptions-and-reporting-images/image40.png)](subscriptions-and-reporting-images/image40.png#lightbox)

 [![共有シークレットを生成する](subscriptions-and-reporting-images/image41.png)](subscriptions-and-reporting-images/image41.png#lightbox)   

共有シークレットを使用するには、次のように、自動更新可能なサブスクリプションのアプリ内購入確認を検証するときに、Apple のサーバーに送信される JSON ペイロードにそれを含めます。

```csharp
{
   "receipt-data" : "(receipt bytes here)",
   "password"     : "(shared secret bytes here)"
}
```

購入が有効な場合、応答の状態フィールドは0になり、他の種類の製品と同様になります。

#### <a name="downloading-items-after-the-initial-subscription-term"></a>最初のサブスクリプション期間の後に項目をダウンロードする

サブスクリプション製品の配信の一部として、コードは Apple のサーバーに対する最新の既知の受信確認を頻繁に検証する必要があります。 前回の検証以降にサブスクリプションが自動更新された場合、JSON 応答には、発生したトランザクションをアプリケーションに通知する追加のフィールド (サブスクリプションの有効性を延長する必要があります) が含まれます。 JSON 応答には次のものが含まれます。

```csharp
{
   "status" : 0,
   "receipt" : { (receipt here) },
   "latest_receipt" : "(base-64 encoded receipt here)",
   "latest_receipt_info" : { (latest receipt info here) }
}
```

状態がゼロの場合、サブスクリプションは引き続き有効であり、他のフィールドに有効なデータが保持されます。 状態が21006の場合、サブスクリプションの有効期限が切れています。 その他のエラーコードについては、「自動更新可能[なサブスクリプション](https://developer.apple.com/library/ios/releasenotes/General/ValidateAppStoreReceipt/Chapters/ValidateRemotely.html)の確認」ドキュメントを参照してください。

#### <a name="restoring-auto-renewable-subscriptions"></a>自動更新可能なサブスクリプションを復元しています

複数のトランザクションが返されます。つまり、元の購入トランザクションと、サブスクリプションが更新された期間ごとに個別のトランザクションが返されます。 有効期間を把握するには、開始日と使用条件を追跡する必要があります。   

SKPaymentTransaction オブジェクトにはサブスクリプションの用語が含まれていません。用語ごとに異なる製品 ID を使用し、トランザクションの購入日からサブスクリプション期間を推定できるコードを記述する必要があります。

#### <a name="testing-auto-renewal"></a>自動更新のテスト

サブスクリプションのテストを容易にするために、サンドボックスでテストを行うと、その期間が圧縮されます。 1週間のサブスクリプションは3分ごとに更新し、1年間のサブスクリプションは1時間ごとに更新します。 サブスクリプションは、サンドボックスでのテスト中に最大6回自動更新されます。

## <a name="reporting"></a>レポート

iTunes Connect ( [itunesconnect.apple.com](https://itunesconnect.apple.com)) には次のものがあります。   

 **[売上と傾向**] –アプリのダウンロード、更新プログラム、アプリ内購入の詳細が表示されます。   

 **支払いと財務報告**–お客様のアプリによって獲得された収入の詳細と、お客様に対して行われた支払いとお支払い額の一覧を表示します。

次に、売上および傾向レポートの例を示します。   

 [![売上および傾向レポートの例](subscriptions-and-reporting-images/image42.png)](subscriptions-and-reporting-images/image42.png#lightbox)   

 また、 **ITC Connect Mobile** iOS アプリもあります。 使用できる統計情報の一部については、iPhone のスクリーンショットを次に示します。   

 [![利用可能な統計情報の一部に関する iPhone のスクリーンショット](subscriptions-and-reporting-images/image43.png)](subscriptions-and-reporting-images/image43.png#lightbox)
