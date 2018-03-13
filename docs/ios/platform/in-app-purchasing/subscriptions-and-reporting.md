---
title: "サブスクリプションとレポート"
ms.topic: article
ms.prod: xamarin
ms.assetid: 27EE4234-07F5-D2CD-DC1C-86E27C20141E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 237a986d6db2fb6984e99c6265fbbc212b35a351
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="subscriptions-and-reporting"></a>サブスクリプションとレポート

## <a name="about-non-renewing-subscriptions"></a>以外のサブスクリプションを更新する方法

サブスクリプションの非更新は、時間制限 (1 週間のナビゲーション アプリケーションへのアクセス) やデータのアーカイブに時間制限付きのアクセスなどのサービスの売上を表す製品を意図しています。   
   
   
   
 非更新サブスクリプションやその他の製品の種類の主な違いは:

-  ITunes Connect で製品の定義では、という用語は含まれません。 アプリケーション コードは、製品の ID からの有効期間を推論できる必要があります。 
-  これらを複数回 (製品と同様、消耗) 購入できます。 アプリケーションは、サブスクリプション期間/有効期限と、ユーザーが更新を管理し、ユーザーが重複しているサブスクリプションを購入するを防ぐ必要があります。 
-  StoreKit 復元関数では、購入記録はサポートされていません。 サブスクリプションは利用できなくなった場合、すべてのユーザーのデバイス間で、アプリケーションを設計およびリモート サーバーと連携してこの機能を実装する必要があります。 アプリケーションでは、バックアップの場合、サブスクリプションの状態デバイスがバックアップされるときにも、復元からのバックアップ。 
-  実装の概要
-  サーバーによって提供されるワークフローを使用して、利用できる製品と同様に管理、サブスクリプションの非更新を実装通常必要があります。 


## <a name="about-free-subscriptions"></a>無料サブスクリプションについて

無料サブスクリプションでは、開発者が (Newsstand 以外のアプリで、使用できません) Newsstand アプリで無料のコンテンツを配置できるようにします。 無料のサブスクリプションが開始されるとは、すべてのユーザーのデバイスで利用可能があります。 無料サブスクリプションの有効期限はありません。これらはのみ、アプリケーションのアンインストール時に終了します。

### <a name="implementation-overview"></a>実装の概要

無料サブスクリプションは、自動更新可能なサブスクリプションのように動作します。 アプリケーションによっては、iTunes Connect で '購入' できる無料のサブスクリプション商品が必要です。 ユーザーがの購入時に、自動更新可能なサブスクリプションの製品と同様に無料のサブスクリプションの購入を検証する必要があります。 無料のサブスクリプションのトランザクションを復元することができます。


## <a name="about-auto-renewable-subscriptions"></a>自動更新可能なサブスクリプションについて

自動更新可能なサブスクリプションは、Newsstand アプリケーションで主に使用されます。 これらは、iTunes Connect (7 日から 1 年までの期間を設定) で構成されている時間の指定した期間に動的なコンテンツへのユーザー アクセスを付与する製品を表します。 サブスクリプションは、ユーザー opts アウトしない限り、各サブスクリプション期間の最後に、ユーザーの Apple ID を充電中、自動的に更新します。この製品の種類に適しています雑誌やニュースのサブスクリプションでは、ユーザーが自らのサブスクリプションが有効では、パブリッシュされた各問題へのアクセスを取得します。

### <a name="implementation-overview"></a>実装の概要

Server-Delivered 製品ワークフローを使用して自動更新可能なサブスクリプションを実装する必要があります (を参照してください、*受信確認と Server-Delivered 製品*セクション)。

#### <a name="shared-secret"></a>共有シークレット

アプリ内購入共有シークレットは、サーバー上の自動更新可能なサブスクリプションを検証するときに、JSON 要求で使用する必要があります。 共有シークレットは、iTunes Connect を使用して作成/アクセスです。

接続のホーム ページの選択、iTunes から**My Apps**:   
   
 [![](subscriptions-and-reporting-images/image2.png "マイ アプリを選択します。")](subscriptions-and-reporting-images/image2.png#lightbox)  
 
アプリケーションを選択し、をクリックして、**アプリ内購入** タブ。

[![](subscriptions-and-reporting-images/image6.png "アプリ内購入 タブをクリックします。")](subscriptions-and-reporting-images/image6.png#lightbox)

ページの下部から次のように選択します**ビュー共有シークレットを生成または**:。
   
 [![](subscriptions-and-reporting-images/image40.png "ビューを選択するか、共有シークレットを生成します。")](subscriptions-and-reporting-images/image40.png#lightbox)

 [![](subscriptions-and-reporting-images/image41.png "共有シークレットを生成します")](subscriptions-and-reporting-images/image41.png#lightbox)   
   
   
   
 共有シークレットを使用するのには、自動更新可能なサブスクリプションでは、次のように、アプリ内購入受信確認を検証するときに、Apple のサーバーに送信される JSON ペイロードに含めます。

```csharp
{
   "receipt-data" : "(receipt bytes here)",
   "password"     : "(shared secret bytes here)"
}
```

場合は、注文書として有効では、その他の製品の種類では、応答の状態 フィールドを 0 になります。

#### <a name="downloading-items-after-the-initial-subscription-term"></a>初期サブスクリプション期間の後に項目をダウンロードします。

サブスクリプションの製品を提供するの一環として、そのコードは頻繁に Apple のサーバーに対しての最新の既知確認メッセージを確認してください。 JSON 応答が発生したトランザクションのアプリケーションに通知する追加のフィールドを含むサブスクリプションが自動で更新された場合、最後の検証以降、(サブスクリプションの有効性を拡張する必要があります)。 JSON 応答が含まれます。

```csharp
{
   "status" : 0,
   "receipt" : { (receipt here) },
   "latest_receipt" : "(base-64 encoded receipt here)",
   "latest_receipt_info" : { (latest receipt info here) }
}
```

状態が 0 の場合は、サブスクリプションがまだ有効し、その他のフィールドに有効なデータを保持します。 状態が 21006 場合は、サブスクリプションは期限切れです。 参照してください、[自動更新不可のサブスクリプション確認メッセージを確認する](https://developer.apple.com/library/ios/releasenotes/General/ValidateAppStoreReceipt/Chapters/ValidateRemotely.html)のドキュメントをその他のエラー コード。

#### <a name="restoring-auto-renewable-subscriptions"></a>自動更新可能なサブスクリプションを復元します。

表示されます: 元の注文書のトランザクションと、別のトランザクション サブスクリプションの更新の各期間の複数のトランザクション。 有効期間を理解するには、開始日と条件を追跡する必要があります。   
   
   
   
 SKPaymentTransaction オブジェクトでは、サブスクリプション期間は含まれません: 各用語のさまざまな製品 ID を使用して、トランザクションの購入日からサブスクリプション期間を推定するコードを記述する必要があります。

#### <a name="testing-auto-renewal"></a>自動更新のテスト

サブスクリプションのテストを容易にできるように、サンド ボックスにテストするときに、その期間は圧縮されます。 1 週間のサブスクリプションでは、3 分ごと、1 年間の 1 時間ごとの更新サブスクリプションを更新します。 サブスクリプションが自動更新を最大 6 回、サンド ボックス内でのテスト中にします。

## <a name="reporting"></a>レポート

iTunes Connect ( [itunesconnect.apple.com](http://itunesconnect.apple.com)) を提供します。   
   
 **売上と傾向**-ダウンロード、更新プログラムとアプリ内購入アプリの詳細が表示されます。   
   
 **支払いと財務レポート**– 一覧支払いを処理しを借りている程度を行われたや、アプリが達成 income について詳しく説明します。

売上およびの傾向レポートの使用例を次に示します。   

 [![](subscriptions-and-reporting-images/image42.png "売上およびの傾向レポートの例")](subscriptions-and-reporting-images/image42.png#lightbox)   
   
 [ **ITC 接続 Mobile**iOS アプリ (iTunes リンク)](http://itunes.apple.com/us/app/itunes-connect-mobile/id376771144?mt=8)です。
使用可能な統計情報の一部の iPhone スクリーン ショットを次に示します。   
   
 [![](subscriptions-and-reporting-images/image43.png "使用可能な統計情報の一部の iPhone スクリーン ショット")](subscriptions-and-reporting-images/image43.png#lightbox)
