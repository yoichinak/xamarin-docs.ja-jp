---
title: サブスクリプションと Xamarin.iOS でのレポート
description: このドキュメントでは、サブスクリプションの更新以外、無料のサブスクリプションを自動更新可能なサブスクリプション、および iTunes Connect を使用してこれらのアイテムをレポートするについて説明します。
ms.prod: xamarin
ms.assetid: 27EE4234-07F5-D2CD-DC1C-86E27C20141E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 4e63894cb862db3b5b5a1e7a2bebd79160c311a9
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121231"
---
# <a name="subscriptions-and-reporting-in-xamarinios"></a>サブスクリプションと Xamarin.iOS でのレポート

## <a name="about-non-renewing-subscriptions"></a>非更新サブスクリプションについて

サブスクリプションの非更新は、時間制限 (1 週間のナビゲーション アプリケーションへのアクセス) やデータのアーカイブに時間制限付きアクセスなどのサービスの売上を表す製品を対象としています。   
   
サブスクリプションの非更新とその他の製品の種類の間の主な違い:

-  ITunes Connect での製品定義では、という用語は含まれません。 アプリケーション コードは、製品の ID からの有効期間を推測できる必要があります。 
-  これらは、複数回 (など、消耗品) 購入できます。 アプリケーションは、サブスクリプション期間/有効期限と更新を管理し、ユーザーが重複しているサブスクリプションを購入するを防ぐ必要があります。 
-  StoreKit 復元関数では、購入記録はサポートされていません。 場合は、サブスクリプションは、ユーザーのすべてのデバイスでも利用できる必要があります、アプリケーションが設計およびリモート サーバーと連携してこの機能を実装する必要があります。 アプリケーションもをバックアップする場合、サブスクリプションの状態、デバイスがバックアップを担当し、復元からのバックアップ。 
-  実装の概要
-  サブスクリプションの非更新は、サーバーによって提供されるワークフローを使用して、コンシューマブル製品と同様に管理に通常実装する必要があります。 


## <a name="about-free-subscriptions"></a>無料サブスクリプションについて

無料サブスクリプションを使用すると、(Newsstand 以外のアプリで、使用できません) Newsstand アプリで無料のコンテンツを配置できます。 無料のサブスクリプションが開始されるとは、すべてのユーザーのデバイスで利用可能があります。 無料のサブスクリプションの有効期限はありません。アプリケーションのアンインストール時にのみ終了します。

### <a name="implementation-overview"></a>実装の概要

無料サブスクリプションは、自動更新可能なサブスクリプションのように動作します。 アプリケーションによっては、iTunes Connect で 'purchase' の使用可能な無料のサブスクリプション製品が必要です。 ユーザーが購入した、ときに、自動更新可能なサブスクリプション製品のような無料のサブスクリプションの購入を検証する必要があります。 無料のサブスクリプションのトランザクションを復元できます。


## <a name="about-auto-renewable-subscriptions"></a>自動更新可能なサブスクリプションについて

自動更新可能なサブスクリプションは、Newsstand アプリケーションで主に使用されます。 これらは、iTunes Connect (7 日から 1 年に至るまでの期間を設定する) で構成されて、指定した期間、動的なコンテンツへのユーザー アクセスを付与する製品を表します。 サブスクリプションは、各サブスクリプション期間の最後に、ユーザーの Apple ID を充電ユーザー opts アウトしない限り、自動的に更新されます。この製品の種類は雑誌やニュースのサブスクリプションに適して、ユーザーが自分のサブスクリプションが有効な間に発行された各問題へのアクセスを取得します。

### <a name="implementation-overview"></a>実装の概要

Server-Delivered 製品ワークフローを使用して自動更新可能なサブスクリプションを実装する必要があります (を参照してください、*受信確認と Server-Delivered 製品*セクション)。

#### <a name="shared-secret"></a>共有シークレット

アプリ内購入の共有シークレットは、サーバー上の自動更新可能なサブスクリプションを検証するときに、JSON 要求で使用する必要があります。 共有シークレットは、iTunes Connect を使用して作成されたアクセスです。

ITunes Connect ホーム ページの選択から**Myapps**:   
   
 [![](subscriptions-and-reporting-images/image2.png "[My Apps] を選びます")](subscriptions-and-reporting-images/image2.png#lightbox)  
 
アプリケーションを選択し、をクリックして、**アプリ内購入** タブ。

[![](subscriptions-and-reporting-images/image6.png "アプリ内購入 タブをクリックします。")](subscriptions-and-reporting-images/image6.png#lightbox)

ページの下部にある、次のように選択します**ビューまたは共有シークレットを生成**:。
   
 [![](subscriptions-and-reporting-images/image40.png "ビューを選択するか、共有シークレットを生成します。")](subscriptions-and-reporting-images/image40.png#lightbox)

 [![](subscriptions-and-reporting-images/image41.png "共有シークレットを生成します。")](subscriptions-and-reporting-images/image41.png#lightbox)   
   
   
   
 共有シークレットを使用するには、自動更新可能サブスクリプションでは、次のように、アプリ内購入の確認メッセージを検証するときに、Apple のサーバーに送信される JSON ペイロードに含めます。

```csharp
{
   "receipt-data" : "(receipt bytes here)",
   "password"     : "(shared secret bytes here)"
}
```

場合は、購入が他の製品の種類として有効では、応答の状態 フィールドを 0 になります。

#### <a name="downloading-items-after-the-initial-subscription-term"></a>最初のサブスクリプション期間の後に項目をダウンロード

サブスクリプション製品の一部として、コードは頻繁に Apple のサーバーに対しての最新の既知確認メッセージを確認する必要があります。 JSON 応答が発生したトランザクションのアプリケーションに通知する追加のフィールドを含める場合は、サブスクリプションが自動更新、前回の認証以降、(これは、サブスクリプション有効期間を延長する必要があります)。 JSON 応答が含まれます。

```csharp
{
   "status" : 0,
   "receipt" : { (receipt here) },
   "latest_receipt" : "(base-64 encoded receipt here)",
   "latest_receipt_info" : { (latest receipt info here) }
}
```

状態が 0 の場合は、サブスクリプションがまだ有効し、他のフィールドが有効なデータを保持します。 状態が 21006 の場合は、サブスクリプションが終了しました。 参照してください、 [、自動更新可能なサブスクリプションの受信を確認する](https://developer.apple.com/library/ios/releasenotes/General/ValidateAppStoreReceipt/Chapters/ValidateRemotely.html)のドキュメントを他のエラー コード。

#### <a name="restoring-auto-renewable-subscriptions"></a>復元中の自動更新可能なサブスクリプション

表示されます – 複数のトランザクションを元の購入トランザクションと、別のトランザクションの各期間のサブスクリプションが更新されました。 有効期間を理解するには、開始日と用語を追跡する必要があります。   
   
   
   
 SKPaymentTransaction オブジェクトでは、サブスクリプション期間は含まれません: 各用語の別の製品 ID を使用し、トランザクションの購入日からサブスクリプションの期間を推定するコードを記述する必要があります。

#### <a name="testing-auto-renewal"></a>自動更新をテストします。

サブスクリプションのテストを容易にできるように、その期間は、サンド ボックス内でテストするときに圧縮されます。 1 週間サブスクリプションでは、3 分ごと、1 年間のサブスクリプション 1 時間ごとの更新が更新されます。 サブスクリプションは自動更新、サンド ボックス内でのテスト中に 6 回の最大数。

## <a name="reporting"></a>レポート

iTunes Connect ( [itunesconnect.apple.com](http://itunesconnect.apple.com)) を提供します。   
   
 **売上および傾向**– ダウンロード、更新プログラムとアプリ内購入アプリの詳細が表示されます。   
   
 **支払いと会計報告書**– し、支払う金額はどの程度行われた一覧の支払いおよび、アプリによって達成 income について詳しく説明します。

売上および傾向レポートの使用例は、以下に示します。   

 [![](subscriptions-and-reporting-images/image42.png "売上および傾向レポートの使用例")](subscriptions-and-reporting-images/image42.png#lightbox)   
   
 [ **ITC 接続 Mobile**iOS アプリ (iTunes のリンク)](http://itunes.apple.com/us/app/itunes-connect-mobile/id376771144?mt=8)します。
使用可能な統計情報の一部の iPhone スクリーン ショットを次に示します。   
   
 [![](subscriptions-and-reporting-images/image43.png "使用可能な統計情報の一部の iPhone スクリーン ショット")](subscriptions-and-reporting-images/image43.png#lightbox)
