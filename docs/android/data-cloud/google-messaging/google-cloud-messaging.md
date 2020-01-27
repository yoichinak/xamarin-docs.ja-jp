---
title: Google Cloud Messaging
description: Google Cloud Messaging (GCM) は、モバイルアプリとサーバーアプリケーション間のメッセージングを容易にするサービスです。 この記事では、GCM の動作の概要について説明します。また、アプリで GCM を使用できるように Google サービスを構成する方法についても説明します。
ms.prod: xamarin
ms.assetid: DF8EF401-F63D-4BA0-B2C6-B22DF8FD60CB
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/02/2019
ms.openlocfilehash: e9b0337c9cdcfbd8f738a11c5dffff427df620bc
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76723662"
---
# <a name="google-cloud-messaging"></a>Google Cloud Messaging

> [!WARNING]
> Google 非推奨 GCM (2018 年4月10日時点)。 次のドキュメントとサンプルプロジェクトは、管理されなくなる可能性があります。 Google の GCM サーバーとクライアント Api は、2019年5月29日の時点で削除される予定です。 Google は、GCM アプリを焼討 Base Cloud Messaging (FCM) に移行することを推奨しています。 GCM の廃止と移行の詳細については、「 [Google Deprecated Cloud Messaging](https://developers.google.com/cloud-messaging/)」を参照してください。
>
> Xamarin での焼討 Base Cloud Messaging の使用を開始するには、「[焼討 Base Cloud messaging](firebase-cloud-messaging.md)」を参照してください。

_Google Cloud Messaging (GCM) は、モバイルアプリとサーバーアプリケーション間のメッセージングを容易にするサービスです。この記事では、GCM の動作の概要について説明します。また、アプリで GCM を使用できるように Google サービスを構成する方法についても説明します。_

[![Google Cloud Messaging ロゴ](google-cloud-messaging-images/preview-sml.png)](google-cloud-messaging-images/preview.png#lightbox)

このトピックでは、Google Cloud Messaging によるアプリとアプリサーバー間でのメッセージのルーティング方法の概要について説明します。また、アプリで GCM サービスを使用できるように資格情報を取得するための詳細な手順についても説明します。

## <a name="overview"></a>の概要

Google Cloud Messaging (GCM) は、サーバーアプリケーションとモバイルクライアントアプリ間でのメッセージの送信、ルーティング、およびキューを処理するサービスです。 *クライアントアプリ*は、デバイスで実行される GCM 対応アプリです。 *アプリサーバー* (お客様または会社が提供) は、クライアントアプリが gcm を介して通信する gcm 対応サーバーです。

[クライアントアプリとアプリサーバーの間に GCM が存在する ![](google-cloud-messaging-images/01-server-gcm-app-sml.png)](google-cloud-messaging-images/01-server-gcm-app.png#lightbox)

GCM を使用すると、アプリサーバーは、1つのデバイス、デバイスのグループ、またはトピックにサブスクライブしている複数のデバイスにメッセージを送信できます。 クライアントアプリは GCM を使用して、アプリサーバーからのダウンストリームメッセージをサブスクライブできます (たとえば、リモート通知を受信します)。 また、GCM を使用すると、クライアントアプリがアップストリームメッセージをアプリケーションサーバーに送信できるようになります。

## <a name="google-cloud-messaging-in-action"></a>Google Cloud Messaging の動作

ダウンストリームメッセージがアプリサーバーからクライアントアプリに送信されると、アプリサーバーから*GCM 接続サーバー*にメッセージが送信されます。その後、GCM 接続サーバーは、クライアントアプリを実行しているデバイスにメッセージを転送します。 メッセージは、HTTP または[Xmpp](https://firebase.google.com/docs/cloud-messaging/xmpp-server-ref) (拡張可能なメッセージングおよびプレゼンスプロトコル) を介して送信できます。 クライアントアプリは常時接続または実行されていないため、GCM 接続サーバーはメッセージをキューに入れて保存し、再接続して使用できるようになったときにクライアントアプリに送信します。 同様に、アプリサーバーが使用できない場合、GCM はクライアントアプリからのアップストリームメッセージをアプリサーバーにエンキューします。

GCM は、次の資格情報を使用してアプリサーバーとクライアントアプリを識別し、これらの資格情報を使用して GCM 経由のメッセージトランザクションを承認します。

- **Api キー &ndash;** *api キー*を使用すると、アプリサーバーは Google サービスにアクセスできます。GCM は、このキーを使用してアプリサーバーを認証します。
    GCM サービスを使用するには、まず、*プロジェクト*を作成して、 [Google DEVELOPER Console](https://console.developers.google.com/)から API キーを取得する必要があります。 API キーは安全に保つ必要があります。API キーの保護の詳細については、「 [api キーを安全に使用するためのベストプラクティス](https://support.google.com/cloud/answer/6310037?hl=en)」を参照してください。

- 送信**者 id** &ndash;*送信者 id*は、アプリサーバーをクライアントアプリに対して承認し &ndash; クライアントアプリへのメッセージの送信が許可されているアプリサーバーを識別する一意の番号です。
    送信者 ID もプロジェクト番号です。プロジェクトを登録するときに、Google 開発者コンソールから送信者 ID を取得します。

- **登録トークン &ndash;** *登録トークン*は、特定のデバイス上のクライアントアプリの GCM id です。 登録トークンは実行時に生成され &ndash; アプリは、デバイスでの実行中に GCM に最初に登録するときに登録トークンを受け取ります。 登録トークンは、(特定のデバイスで実行されている) クライアントアプリのインスタンスに対して、GCM からメッセージを受信することを承認します。

- **アプリケーション ID**は、GCM からメッセージを受信するように登録されているクライアントアプリの Id &ndash; ます (特定のデバイスに依存しません)。 Android では、アプリケーション ID は、`com.xamarin.gcmexample`など、 **Androidmanifest .xml**に記録されたパッケージ名です。

[Google Cloud Messaging の設定](#settingup)(このガイドの後半) では、プロジェクトを作成し、これらの資格情報を生成するための詳細な手順について説明します。

次のセクションでは、クライアントアプリが GCM を介してアプリサーバーと通信するときに、これらの資格情報を使用する方法について説明します。

### <a name="registration-with-gcm"></a>GCM による登録

デバイスにインストールされているクライアントアプリは、メッセージングを実行する前に、まず GCM に登録する必要があります。 クライアントアプリは、次の図に示す登録手順を完了する必要があります。

[![アプリの登録手順](google-cloud-messaging-images/02-app-registration-sml.png)](google-cloud-messaging-images/02-app-registration.png#lightbox)

1. クライアントアプリは GCM に接続して登録トークンを取得し、送信者 ID を GCM に渡します。

2. GCM は、クライアントアプリに登録トークンを返します。

3. クライアントアプリは、登録トークンをアプリサーバーに転送します。

アプリサーバーは、クライアントアプリとの後続の通信用に登録トークンをキャッシュします。 必要に応じて、アプリサーバーは、登録トークンを受信したことを示す受信確認をクライアントアプリに返信できます。 このハンドシェイクが行われると、クライアントアプリはアプリサーバーからメッセージを受信する (またはメッセージを送信する) ことができます。

クライアントアプリがアプリサーバーからメッセージを受信する必要がなくなった場合は、アプリサーバーに要求を送信して、登録トークンを削除できます。 クライアントアプリがトピックメッセージを受信している場合 (この記事の後半で説明します)、トピックの購読を解除できます。
クライアントアプリがデバイスからアンインストールされると、GCM はこれを検出し、登録トークンを削除するようアプリサーバーに自動的に通知します。

### <a name="downstream-messaging"></a>ダウンストリームメッセージング

アプリケーションサーバーは、ダウンストリームメッセージをクライアントアプリに送信するときに、次の図に示す手順に従います。

[ダウンストリームメッセージングストアアンドフォワードダイアグラムの ![](google-cloud-messaging-images/03-downstream-sml.png)](google-cloud-messaging-images/03-downstream.png#lightbox)

1. アプリサーバーから GCM にメッセージが送信されます。

2. クライアントデバイスが使用できない場合、GCM サーバーはメッセージをキューに格納して後で送信します。

3. クライアントデバイスが使用可能な場合、GCM はそのデバイス上のクライアントアプリにメッセージを送信します。

4. クライアントアプリは GCM からメッセージを受信し、それに応じて処理します。 たとえば、メッセージがリモート通知の場合は、ユーザーに表示されます。

このメッセージングシナリオ (アプリサーバーが1つのクライアントアプリにメッセージを送信する場合) では、メッセージの最大サイズは 4 Kb です。

Android での下流の GCM メッセージの受信に関する詳細 (コードサンプルを含む) については、「[リモート通知](~/android/data-cloud/google-messaging/remote-notifications-with-gcm.md)」を参照してください。

#### <a name="topic-messaging"></a>トピックメッセージング

*トピックメッセージング*はダウンストリームメッセージングの一種であり、アプリサーバーが1つのメッセージを複数のクライアントアプリデバイス (天気予報など) にサブスクライブしているデバイスに送信します。 トピックのメッセージの長さは最大で 1 KB で、トピックメッセージングではアプリあたり最大100万のサブスクリプションがサポートされます。 GCM がトピックメッセージングにのみ使用されている場合、クライアントアプリは、アプリサーバーに登録トークンを送信する必要はありません。

#### <a name="group-messaging"></a>グループメッセージング

*グループメッセージング*は、1つのグループに属する複数のクライアントアプリデバイス (たとえば、1人のユーザーに属するデバイスのグループ) に、アプリサーバーが1つのメッセージを送信するダウンストリームメッセージングの一種です。 グループメッセージの最大サイズは、iOS デバイスの場合は最大2KB、Android デバイスの場合は最大 4 KB です。 グループは、最大20個のメンバーに制限されます。

### <a name="upstream-messaging"></a>アップストリームメッセージング

クライアントアプリが[Xmpp](https://firebase.google.com/docs/cloud-messaging/xmpp-server-ref)をサポートするサーバーに接続する場合、次の図に示すように、アプリケーションサーバーにメッセージを返信できます。

[アップストリームメッセージングの ![ダイアグラム](google-cloud-messaging-images/04-upstream-sml.png)](google-cloud-messaging-images/04-upstream.png#lightbox)

1. クライアントアプリは GCM XMPP 接続サーバーにメッセージを送信します。

2. アプリサーバーが切断されている場合、GCM サーバーはメッセージをキューに格納して後で転送します。

3. アプリサーバーが再接続されると、GCM はメッセージをアプリサーバーに転送します。

4. アプリサーバーは、メッセージを解析してクライアントアプリの id を検証した後、"ack" を GCM に送信して、メッセージの受信確認を受け取ります。

5. アプリサーバーはメッセージを処理します。

Google の[上流メッセージ](https://firebase.google.com/docs/cloud-messaging/xmpp-server-ref#upstream)では、JSON でエンコードされたメッセージを構造化し、GOOGLE の xmpp ベースのクラウド接続サーバーを実行するアプリサーバーに送信する方法について説明します。

<a name="settingup" />

## <a name="setting-up-google-cloud-messaging"></a>Google Cloud Messaging の設定

アプリで GCM サービスを使用するには、Google の GCM サーバーにアクセスするための資格情報を最初に取得する必要があります。 次のセクションでは、このプロセスを完了するために必要な手順について説明します。

### <a name="enable-google-services-for-your-app"></a>アプリの Google サービスを有効にする

1. Google アカウント (gmail のアドレスなど) を使用して[Google 開発者コンソール](https://developers.google.com/mobile/add?platform=android)にサインインし、新しいプロジェクトを作成します。 既存のプロジェクトがある場合は、GCM が有効になるプロジェクトを選択します。 次の例では、 **XamarinGCM**という名前の新しいプロジェクトが作成されます。

    [XamarinGCM プロジェクトの作成 ![](google-cloud-messaging-images/05-create-gcm-app-sml.png)](google-cloud-messaging-images/05-create-gcm-app.png#lightbox)

2. 次に、アプリのパッケージ名 (この例では、パッケージ名は " **com. xamarin. gcmexample**) を入力し、[続行] をクリックし**てサービスを選択して構成**します。

    [パッケージ名の入力 ![](google-cloud-messaging-images/06-package-name-sml.png)](google-cloud-messaging-images/06-package-name.png#lightbox)

    このパッケージ名は、アプリのアプリケーション ID でもあることに注意してください。

3. **[サービスの選択と構成**] セクションには、アプリに追加できる Google サービスが一覧表示されます。 **[クラウドメッセージング]** をクリックします。

    [クラウドメッセージングを選択 ![には](google-cloud-messaging-images/07-choose-gcm-service-sml.png)](google-cloud-messaging-images/07-choose-gcm-service.png#lightbox)

4. 次に、 **[GOOGLE CLOUD MESSAGING を有効にする]** をクリックします。

    [Google Cloud Messaging を有効に ![には](google-cloud-messaging-images/08-enable-gcm-sml.png)](google-cloud-messaging-images/08-enable-gcm.png#lightbox)

5. **サーバー API キー**と**送信者 ID**がアプリに対して生成されます。 これらの値を記録し、 **[閉じる]** をクリックします。

    [![サーバー API キーと送信者 ID が表示されます](google-cloud-messaging-images/09-get-api-key-and-id-sml.png)](google-cloud-messaging-images/09-get-api-key-and-id.png#lightbox)

    API キーは、一般に使用するためのものではない &ndash; 保護します。 API キーが侵害されると、承認されていないサーバーがクライアントアプリケーションにメッセージを公開する可能性があります。
    Api キー[を安全に使用するためのベストプラクティス](https://support.google.com/cloud/answer/6310037?hl=en)では、api キーを保護するためのガイドラインについて説明します。

### <a name="view-your-project-settings"></a>プロジェクトの設定を表示する

[Google Cloud Console](https://console.cloud.google.com/)にサインインしてプロジェクトを選択すると、いつでもプロジェクトの設定を表示できます。 たとえば、ページの上部にあるプルダウンメニューでプロジェクトを選択して、**送信者 ID**を表示できます (この例では、プロジェクトは**XamarinGCM**と呼ばれています)。 このスクリーンショットに示されているように、Sender ID はプロジェクト番号です (送信者 ID は**9349932736**)。

[送信者 ID の表示 ![](google-cloud-messaging-images/10-view-server-id-sml.png)](google-cloud-messaging-images/10-view-server-id.png#lightbox)

**Api キー**を表示するには、 **[api マネージャー]** をクリックし、 **[資格情報]** をクリックします。

[API キーの表示 ![](google-cloud-messaging-images/11-view-credentials-sml.png)](google-cloud-messaging-images/11-view-credentials.png#lightbox)

## <a name="for-further-reading"></a>関連項目

- [Rfc 6120](https://tools.ietf.org/html/rfc6120)および[rfc 6121](https://tools.ietf.org/html/rfc6121)では、拡張可能なメッセージングとプレゼンスプロトコル (xmpp) について説明し、定義します。

## <a name="summary"></a>要約

この記事では、Google Cloud Messaging (GCM) の概要について説明しました。 ここでは、アプリサーバーとクライアントアプリ間のメッセージングを識別および承認するために使用されるさまざまな資格情報について説明しました。 ここでは、最も一般的なメッセージングシナリオについて説明し、gcm サービスを使用するためにアプリケーションを GCM に登録する手順について詳しく説明します。

## <a name="related-links"></a>関連リンク

- [クラウドメッセージング](https://developers.google.com/cloud-messaging/)
