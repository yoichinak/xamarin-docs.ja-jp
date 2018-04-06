---
title: Google ライセンス サービス
ms.prod: xamarin
ms.assetid: E96BDCC3-454A-A797-5819-905E2BB1AC41
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 12/20/2017
ms.openlocfilehash: ebc557ce16858c675b291f17620616a2dba4c054
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="google-licensing-services"></a>Google ライセンス サービス

Google Play より前は、Android アプリケーションは、承認されたユーザーのみが自分のデバイスでアプリケーションを実行できることを保証するために、Google マーケットで提供されるレガシ コピー防止に依存していました。 コピー防止機構の制限によって、アプリケーションの保護は理想的とは言えないソリューションになっていました。

Google ライセンスは、このレガシ コピー防止機構に代わるものです。
Google ライセンスは、アプリケーションが特定のデバイスで実行できるようにライセンスされているかどうかを判断するために、Android アプリケーションがクエリを行う可能性がある、柔軟かつセキュリティで保護されたネットワーク ベースのサービスです。

Google ライセンスは、Android アプリケーションでライセンスを確認するタイミング、ライセンスを確認する頻度、ライセンス サーバーからの応答を処理する方法を完全に制御できるという点から柔軟です。

Google ライセンスは、各応答が Google Play サーバーとアプリケーション間で排他的に共有される RSA キー ペアを使用して署名されるという点から、セキュリティで保護されています。 Google Play では、Android アプリケーション内に埋め込まれた、応答を認証するために使用する秘密キーを開発者に提供します。 Google Play サーバーは、秘密キーを内部的に保持します。

Google ライセンスを実装したアプリケーションは、デバイス上の Google Play アプリケーションによってホストされるサービスへの要求を作成します。 次に、Google Play は、ライセンスの状態に応答する、Google ライセンス サーバーにこの要求を送信します。 

[![ライセンス サーバーのワークフロー図](google-licensing-services-images/gp-licensing-service-overview.png)](google-licensing-services-images/gp-licensing-service-overview.png#lightbox)

上の図は、このワークフローを示しています。 

-   アプリケーションは、パッケージ名、サーバーの応答を検証するために使用する *nonce* (暗号化認証子)、および非同期的に応答を処理できるコールバックを提供します。 

-   Google Play は、Google アカウントやデバイス自体の情報を提供します (IMSI 番号など)。 

また、Google ライセンス サービスは、APK 拡張ファイルの主要なコンポーネントでもあります (このドキュメントで後述されます)。 APK 拡張ファイルでは、Google ライセンス サービスを使用して、ダウンロードされる拡張ファイルの URL を取得します。


## <a name="requirements"></a>必要条件

Google Play を介して購入していないアプリケーションは、Google ライセンス サービスの恩恵を受けることはできません。 Google Play がデバイスにインストールされていない場合は、ライセンス サービスを使用するアプリケーションは、そのデバイス上で引き続き正常に動作します。

Google Play は、機能させるためにインターネット アクセスが必要です。 アプリケーションは、デバイスに Google Play ライセンス サーバーへのアクセス許可がない場合、シナリオに対応するライセンスをキャッシュできます。

アプリケーションが APK 拡張ファイルを使用する場合、無料のアプリケーションに必要なのは、Google ライセンスのみです。
