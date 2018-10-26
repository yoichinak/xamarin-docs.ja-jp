---
title: Google Cloud Messaging
description: Google Cloud Messaging (GCM) は、モバイル アプリとサーバー アプリケーション間のメッセージングを容易にするサービスです。 この記事では、GCM のしくみの概要と、アプリに GCM を使用できるように、Google Services を構成する方法について説明します。
ms.prod: xamarin
ms.assetid: DF8EF401-F63D-4BA0-B2C6-B22DF8FD60CB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/12/2018
ms.openlocfilehash: c2ca567ffcb247622d1b3e8f3e0136c453723b96
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50119112"
---
# <a name="google-cloud-messaging"></a>Google Cloud Messaging

_Google Cloud Messaging (GCM) は、モバイル アプリとサーバー アプリケーション間のメッセージングを容易にするサービスです。この記事では、GCM のしくみの概要と、アプリに GCM を使用できるように、Google Services を構成する方法について説明します。_

[![Google Cloud Messaging のロゴ](google-cloud-messaging-images/preview-sml.png)](google-cloud-messaging-images/preview.png#lightbox)

このトピックでは Google Cloud Messaging、アプリと、アプリのサーバー間でメッセージのルーティング方法の概要と、アプリは、GCM サービスを使用できるように、資格情報を取得するための手順を説明します。

> [!NOTE]
> GCM がによって置き換えられた[Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) (FCM)。
> GCM サーバーおよびクライアント Api[非推奨とされました](https://firebase.googleblog.com/2018/04/time-to-upgrade-from-gcm-to-fcm.html)は利用できなく、2019 年 4 月 11 日早くとします。

## <a name="overview"></a>概要

Google Cloud Messaging (GCM) は、送信、ルーティング、およびサーバー アプリケーションとモバイル クライアント アプリ間でメッセージ キューを処理するサービスです。 A*クライアント アプリ*GCM 対応のアプリをデバイスで実行されているのです。 *アプリ サーバー* (またはお客様の会社によって提供される) GCM に対応するサーバーを介して GCM クライアント アプリと通信です。

[![GCM がクライアント アプリとアプリ サーバーが存在します。](google-cloud-messaging-images/01-server-gcm-app-sml.png)](google-cloud-messaging-images/01-server-gcm-app.png#lightbox)

GCM を使用して、アプリ サーバーは 1 つのデバイス、デバイスのグループまたはさまざまなトピックにサブスクライブしているデバイスにメッセージを送信できます。 クライアント アプリは、GCM を使用して、アプリケーション サーバー (たとえば、リモート通知を受信する場合など) からのダウン ストリームのメッセージをサブスクライブすることができます。 また、GCM により、クライアント アプリのアップ ストリームのメッセージをアプリケーション サーバーに送信することにします。

GCM 用のアプリケーション サーバーの実装方法の詳細については、次を参照してください。 [GCM 接続サーバーについて](https://developers.google.com/cloud-messaging/server)します。



## <a name="google-cloud-messaging-in-action"></a>Google Cloud Messaging の動作

ダウン ストリームのメッセージは、クライアント アプリに、アプリ サーバーから送信された、アプリ サーバーは、メッセージを送信、 *GCM 接続サーバー*; GCM 接続サーバーで、さらに、クライアント アプリを実行しているデバイスにメッセージを転送します。 HTTP 経由でメッセージを送信できるまたは[XMPP](https://developers.google.com/cloud-messaging/ccs) (拡張可能なメッセージングとプレゼンス プロトコル)。 クライアント アプリは常に接続されていないため、または実行中、GCM 接続サーバー エンキューとストア メッセージ、再接続し、使用可能になるクライアント アプリに送信します。 同様に、GCM はエンキュー アプリ サーバーにクライアント アプリからアップ ストリームのメッセージ アプリ サーバーが使用できない場合。

GCM がアプリ サーバーとクライアント アプリを識別するために次の資格情報を使用して、GCM を使用したトランザクション メッセージを承認するためにこれらの資格情報を使用しています。

-   **API キー** &ndash; 、 *API キー* Google サービスへのアクセス権をアプリ サーバーの提供GCM では、アプリ サーバーの認証にこのキーを使用します。
    API キーを取得する必要があります最初、GCM サービスを使用する前に、 [Google Developer Console](https://console.developers.google.com/)を作成して、*プロジェクト*します。 API キーは、セキュリティで保護された; 保持する必要があります。API キーを保護する方法の詳細については、次を参照してください。 [API キーを安全に使用するためのベスト プラクティス](https://support.google.com/cloud/answer/6310037?hl=en)します。

-   **Sender ID** &ndash; 、 *Sender ID*クライアント アプリにアプリ サーバーを承認&ndash;クライアント アプリへのメッセージの送信を許可されているアプリのサーバーを識別する一意の数します。
    Sender ID も、プロジェクトの数。プロジェクトを登録するときに、Google 開発者コンソールから送信者 ID を取得します。

-   **登録トークン** &ndash; 、*登録トークン*は特定のデバイス上のクライアント アプリケーションの GCM id です。 登録トークンは、実行時に生成される&ndash;アプリがデバイス上で実行中に GCM で最初に登録する際に、登録トークンを受信します。 登録トークンは、GCM からメッセージを受信する (その特定のデバイスで実行されている) クライアント アプリのインスタンスを許可します。

-   **アプリケーション ID** &ndash; GCM からメッセージを受信登録する (任意指定のデバイスに依存しない) クライアント アプリの id。 Android では、アプリケーション ID に記録されたパッケージの名前を**AndroidManifest.xml**など`com.xamarin.gcmexample`します。

[設定を Google Cloud Messaging](#settingup)プロジェクトを作成し、これらの資格情報の生成の詳細な手順を提供します (このガイドで後述)。

次のセクションでは、これらの資格情報を使用してクライアント アプリケーションの GCM を通じてアプリ サーバーと通信する方法について説明します。



### <a name="registration-with-gcm"></a>GCM 登録

メッセージングを行う前に、デバイスにインストールされているクライアント アプリの登録は gcm 最初必要があります。 クライアント アプリでは、次の図に示すように登録手順を実行する必要があります。

[![アプリの登録手順](google-cloud-messaging-images/02-app-registration-sml.png)](google-cloud-messaging-images/02-app-registration.png#lightbox)

1.  クライアント アプリでは、GCM に送信者の ID を渡す、登録トークンの取得に GCM を接続します。

2.  GCM は、クライアント アプリを登録トークンを返します。

3.  クライアント アプリでは、アプリ サーバーに登録トークンを転送します。

アプリ サーバーは、クライアント アプリとの通信を後続の登録トークンをキャッシュします。 必要に応じて、アプリ サーバーは、登録トークンを受信したことを示すためにクライアント アプリに受信確認を送信することができます。 このハンドシェイクが行われた後、クライアント アプリできますからメッセージを受信 (またはメッセージを送信) アプリ サーバー。

クライアント アプリは、不要になったアプリ サーバーからメッセージを受信する場合、登録トークンを削除するアプリのサーバーに要求を送信できます。 クライアント アプリでは、(この記事の後半で説明する) トピックのメッセージが受信されることが場合、は、トピックから取り消しできます。
クライアント アプリがデバイスからアンインストールされると、GCM はこれを検出し、登録トークンを削除するアプリのサーバーを自動的に通知します。

Google の[クライアント アプリの登録](https://developers.google.com/cloud-messaging/registration)詳細; に登録プロセスを説明します。 登録を解除し、サブスクリプションの解除、について説明し、クライアント アプリがアンインストールされるときに登録解除のプロセスについて説明します。



### <a name="downstream-messaging"></a>ダウン ストリームのメッセージング

アプリ サーバーは、クライアント アプリにダウン ストリーム メッセージを送信するときに次の図に示す手順に従います。

[![ダウン ストリームのメッセージング ストアと順方向の図](google-cloud-messaging-images/03-downstream-sml.png)](google-cloud-messaging-images/03-downstream.png#lightbox)

1.  アプリ サーバーは、GCM にメッセージを送信します。

2.  クライアント デバイスが利用できない場合、GCM サーバーは、後で伝送のためのキュー、メッセージを格納します。

3.  クライアント デバイスが利用できる場合、GCM は、そのデバイス上のクライアント アプリに、メッセージを送信します。

4.  クライアント アプリでは、GCM からメッセージを受信し、それに応じて処理を行います。 たとえば、メッセージがリモート通知の場合は、ユーザーに表示されます。

(アプリ サーバーが送信する場所、メッセージを 1 つのクライアント アプリに) このメッセージング シナリオでメッセージが最大 4 kB の長さを指定できます。

Android でダウン ストリームの GCM メッセージの受信について (コード サンプルを含む) 詳細なは、次を参照してください。[リモート通知](~/android/data-cloud/google-messaging/remote-notifications-with-gcm.md)します。


#### <a name="topic-messaging"></a>トピックのメッセージング

*トピックの「Messaging*ダウン ストリーム メッセージの種類は、アプリ サーバーが (天気予報) などのトピックをサブスクライブする複数のクライアント アプリ デバイスを 1 つメッセージを送信します。 トピックのメッセージは 2 KB までの長さを指定でき、トピックの「メッセージング アプリごとに最大 100万サブスクリプションをサポートしていますいます。 GCM を使用しているトピックの「メッセージングのみの場合、クライアント アプリでは、登録トークンをアプリ サーバーに送信する必要はありません。 Google の[トピックのメッセージングを実装する](https://developers.google.com/cloud-messaging/topic-messaging)特定のトピックをサブスクライブする複数のデバイスにアプリ サーバーからメッセージを送信する方法について説明します。



#### <a name="group-messaging"></a>グループのメッセージング

*メッセージング グループ*アプリ サーバーが 1 つのメッセージを送信します (たとえば、1 人のユーザーに属しているデバイスのグループ) をグループに属している複数のクライアント アプリのデバイスには、ダウン ストリーム メッセージの種類。 最大 2 KB で、iOS デバイス用の長さは、メッセージをグループ化し、最大で 4 KB の Android デバイス用の長さ。 グループは、最大 20 のメンバーに制限されています。 Google の[デバイス グループのメッセージング](https://developers.google.com/cloud-messaging/notifications)アプリ サーバーがグループに属しているデバイスで実行されている複数のクライアント アプリのインスタンスを 1 つのメッセージを送信する方法について説明します。


### <a name="upstream-messaging"></a>アップ ストリームのメッセージング

かどうか、クライアント アプリはサポートしているサーバーに接続[XMPP](https://developers.google.com/cloud-messaging/ccs)、次の図に示すようにアプリ サーバーにメッセージを送信できます。

[![アップ ストリームのメッセージングのダイアグラム](google-cloud-messaging-images/04-upstream-sml.png)](google-cloud-messaging-images/04-upstream.png#lightbox)

1.  クライアント アプリでは、GCM の XMPP 接続のサーバーにメッセージを送信します。

2.  アプリ サーバーが切断された場合、GCM サーバーは、後で転送するためのキューにメッセージを格納します。

3.  アプリ サーバーが接続されている再、GCM は、アプリ サーバーにメッセージを転送します。

4.  アプリ サーバーは、クライアント アプリの id を確認するメッセージを解析し、「確認」メッセージを確認するための GCM に送信します。

5.  アプリ サーバーは、メッセージを処理します。

Google の[アップ ストリーム メッセージ](https://developers.google.com/cloud-messaging/ccs#upstream)JSON でエンコードされたメッセージを構成して、Google の XMPP に基づくクラウド接続のサーバーを実行しているアプリケーション サーバーに送信する方法について説明します。


<a name="settingup" />

## <a name="setting-up-google-cloud-messaging"></a>Google Cloud Messaging 設定

GCM のサービスを使用するには、アプリで、前にまず Google の GCM サーバーへのアクセスの資格情報を取得する必要があります。 次のセクションでは、このプロセスを完了に必要な手順について説明します。



### <a name="enable-google-services-for-your-app"></a>アプリの Google サービスを有効にします。

1.  サインイン、 [Google Developers Console](https://developers.google.com/mobile/add?platform=android) Google アカウント (つまり、gmail アドレス) と、新しいプロジェクトを作成します。 既存のプロジェクトがある場合は、するのには、GCM に対応するプロジェクトを選択します。 次の例では、新しいプロジェクトと呼ばれる**XamarinGCM**が作成されます。

    [![XamarinGCM プロジェクトを作成します。](google-cloud-messaging-images/05-create-gcm-app-sml.png)](google-cloud-messaging-images/05-create-gcm-app.png#lightbox)

2.  次に、アプリのパッケージの名前を入力します (この例では、パッケージ名は**com.xamarin.gcmexample**) をクリック**選択を続行するサービスを構成して**:

    [![パッケージ名を入力します。](google-cloud-messaging-images/06-package-name-sml.png)](google-cloud-messaging-images/06-package-name.png#lightbox)

    このパッケージ名は、アプリのアプリケーション ID もことに注意してください。

3.  **選択し、サービスの構成**セクションには、アプリに追加できる Google サービスが一覧表示します。 クリックして**クラウド メッセージング**:

    [![クラウドを選択するメッセージング](google-cloud-messaging-images/07-choose-gcm-service-sml.png)](google-cloud-messaging-images/07-choose-gcm-service.png#lightbox)

4.  次に、クリックして**GOOGLE CLOUD MESSAGING を有効にする**:

    [![Google Cloud Messaging を有効にします。](google-cloud-messaging-images/08-enable-gcm-sml.png)](google-cloud-messaging-images/08-enable-gcm.png#lightbox)

5.  A**サーバーの API キー**と**Sender ID**アプリ用に生成されます。 これらの値を記録し、をクリックして**閉じる**:

    [![サーバーの API キーと送信者 ID が表示されます。](google-cloud-messaging-images/09-get-api-key-and-id-sml.png)](google-cloud-messaging-images/09-get-api-key-and-id.png#lightbox)

    API キーを保護する&ndash;を公開して使用するものではありません。 API キーが侵害された場合、承認されていないサーバーは、クライアント アプリケーションにメッセージを発行する可能性があります。
    [API キーを安全に使用するためのベスト プラクティス](https://support.google.com/cloud/answer/6310037?hl=en)API キーを保護するための有益なガイドラインを提供します。



### <a name="view-your-project-settings"></a>プロジェクトの設定を表示します。

サインインして、いつでも、プロジェクトの設定を表示することができます、 [Google Cloud Console](https://console.cloud.google.com/)し、プロジェクトを選択します。 などを表示することができます、 **Sender ID**プルダウン メニュー、ページの上部にあるプロジェクトを選択して (この例で、プロジェクトと呼ばれる**XamarinGCM**)。 送信者 ID がこのスクリーン ショットで示すようにプロジェクト番号 (送信者 ID をここでは、 **9349932736**)。

[![Sender ID の表示](google-cloud-messaging-images/10-view-server-id-sml.png)](google-cloud-messaging-images/10-view-server-id.png#lightbox)

表示する、 **API キー**、 をクリックして**API Manager**  をクリックし、**資格情報**:

[![API キーを表示します。](google-cloud-messaging-images/11-view-credentials-sml.png)](google-cloud-messaging-images/11-view-credentials.png#lightbox)



## <a name="for-further-reading"></a>関連項目

-   Google の[クライアント アプリの登録](https://developers.google.com/cloud-messaging/registration)の詳細については、クライアントの登録プロセスについて説明し、自動再試行を構成して、登録状態の同期を維持する方法について説明しています。

-   [RFC 6120](https://tools.ietf.org/html/rfc6120)と[RFC 6121](https://tools.ietf.org/html/rfc6121)について説明し、拡張可能なメッセージングとプレゼンス プロトコル (XMPP) を定義します。



## <a name="summary"></a>まとめ

この記事では、Google Cloud Messaging (GCM) の概要を提供します。 特定し、アプリ サーバーとクライアント アプリ間のメッセージングを承認に使用されるさまざまな資格情報がについて説明します。 最も一般的なメッセージング シナリオを説明し、GCM GCM サービスを使用すると、アプリを登録する手順について詳しく説明します。


## <a name="related-links"></a>関連リンク

- [クラウド メッセージング](https://developers.google.com/cloud-messaging/)
