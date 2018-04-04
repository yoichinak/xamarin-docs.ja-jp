---
title: Google Cloud Messaging
description: Google Cloud Messaging (GCM) は、モバイル アプリとサーバー アプリケーション間のメッセージングを容易にするサービスです。 この記事は、GCM のしくみの概要を説明し、GCM をアプリケーションで使用できるように、Google サービスを構成する方法についても説明します。
ms.prod: xamarin
ms.assetid: DF8EF401-F63D-4BA0-B2C6-B22DF8FD60CB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 73ff82f3bf36aa54422c1693c6bf07731480b7f7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="google-cloud-messaging"></a>Google Cloud Messaging

_Google Cloud Messaging (GCM) は、モバイル アプリとサーバー アプリケーション間のメッセージングを容易にするサービスです。この記事は、GCM のしくみの概要を説明し、GCM をアプリケーションで使用できるように、Google サービスを構成する方法についても説明します。_

[![Google Cloud Messaging のロゴ](google-cloud-messaging-images/preview-sml.png)](google-cloud-messaging-images/preview.png#lightbox)

このトピックでは、アプリと、アプリ サーバーの間でメッセージをルーティングする Google Cloud Messaging 方法の概要を説明し、アプリは、GCM サービスを使用できるように、資格情報を取得するための手順を追って説明を提供します。


## <a name="overview"></a>概要

Google Cloud Messaging (GCM) は、処理、送信する、ルーティング、およびサーバー アプリケーションとモバイル クライアント アプリの間でメッセージをキュー処理するサービスです。 A*クライアント アプリ*デバイスで実行されている GCM を有効にしたアプリです。 *アプリ サーバー* (またはお客様の会社によって提供される) は、GCM 経由でクライアント アプリで通信する GCM が有効なサーバー。

[![GCM がクライアント アプリケーションと、アプリ サーバーの間に存在します。](google-cloud-messaging-images/01-server-gcm-app-sml.png)](google-cloud-messaging-images/01-server-gcm-app.png#lightbox)

GCM では、アプリ サーバーは、1 台のデバイス、デバイスのグループまたはトピックにサブスクライブしているデバイスの数にメッセージを送信できます。 クライアント アプリは、GCM を使用して、(たとえば、リモートの通知を受信する場合)、アプリ サーバーからのダウン ストリーム メッセージをサブスクライブすることができます。 また、GCM できるようになりますクライアント アプリをアップ ストリームのメッセージをアプリケーション サーバーに送信します。

GCM 用のアプリ サーバーの実装方法の詳細については、次を参照してください。 [GCM 接続サーバーに関する](https://developers.google.com/cloud-messaging/server)です。



## <a name="google-cloud-messaging-in-action"></a>Google Cloud アクションでのメッセージング

ダウン ストリームのメッセージは、アプリ サーバーからクライアント アプリに、アプリ サーバーは、メッセージを送信、 *GCM 接続サーバー*; GCM 接続サーバーはさらに、クライアント アプリを実行しているデバイスにメッセージを転送します。 HTTP 経由でメッセージを送信できますまたは[XMPP](https://developers.google.com/cloud-messaging/ccs) (拡張のメッセージングおよびプレゼンス プロトコル) です。 クライアント アプリは常に接続されていないためか、実行されて、GCM 接続サーバーのキューに入れます、ストア メッセージ、再接続し、使用可能になると、クライアント アプリに送信することです。 同様に、GCM はエンキュー アプリ サーバーにクライアント アプリケーションからメッセージをアップ ストリームのアプリ サーバーが使用できない場合。

GCM では、次の資格情報を使用して、アプリ サーバーとクライアント アプリでは、識別し、GCM 経由のメッセージのトランザクションを承認するためにこれらの資格情報を使用します。

-   **API キー** &ndash; 、 *API キー* Google サービスへのアクセスをアプリケーション サーバーを提供GCM は、アプリ サーバーの認証にこのキーを使用します。
    GCM サービスを使用することができます、前にまずから API キーを取得する必要があります、 [Google Developer Console](https://console.developers.google.com/)作成することで、*プロジェクト*です。 API キーはセキュリティを確保します。API キーを保護する方法についての詳細については、次を参照してください。[のベスト プラクティスの API キーを使用した安全な](https://support.google.com/cloud/answer/6310037?hl=en)します。

-   **送信者 ID** &ndash; 、*送信者 ID*クライアント アプリにアプリのサーバーを承認&ndash;はメッセージを送信するクライアント アプリケーションに許可されているアプリケーション サーバーを識別する一意の番号。
    送信者 ID も、プロジェクトの数です。プロジェクトを登録するときに、Google Developers Console から送信者 ID を取得します。

-   **登録トークン** &ndash; 、*登録トークン*に特定のデバイスでクライアント アプリの GCM id です。 実行時に、登録トークンが生成された&ndash;アプリは、デバイスで実行中 GCM と最初に登録するときに、登録トークンを受信します。 登録トークンは、GCM からメッセージを受信する (その特定のデバイスで実行されている) クライアント アプリのインスタンスを許可します。

-   **アプリケーション ID** &ndash; GCM からメッセージを受信登録する独立した、特定のデバイスのクライアント アプリの id。 Android、アプリケーション ID は、パッケージ名に記録される**AndroidManifest.xml**など`com.xamarin.gcmexample`です。

[設定を Google Cloud Messaging](#settingup) (このガイドで後述) プロジェクトを作成し、これらの資格情報の生成の詳細な手順を提供します。

次のセクションでは、これらの資格情報を使用してクライアント アプリは、GCM を通じてアプリ サーバーと通信する方法について説明します。



### <a name="registration-with-gcm"></a>GCM との登録

メッセージングを行う前に、デバイスにインストールされているクライアント アプリの登録は GCM と最初必要があります。 クライアント アプリでは、次の図に示すように登録手順を実行する必要があります。

[![アプリの登録ステップ](google-cloud-messaging-images/02-app-registration-sml.png)](google-cloud-messaging-images/02-app-registration.png#lightbox)

1.  クライアント アプリ連絡先 GCM 登録トークン、GCM する送信者 ID を渡すことを取得します。

2.  GCM は、クライアント アプリを登録トークンを返します。

3.  クライアント アプリでは、アプリ サーバーの登録トークンを転送します。

アプリケーション サーバーでは、クライアント アプリケーションとの通信を後続の登録トークンをキャッシュします。 必要に応じて、アプリケーション サーバーでは、登録トークンを受信したことを示すためにクライアント アプリケーションに戻る受信確認を送信できます。 このハンドシェイクが行われた後、クライアント アプリできますからメッセージを受信 (またはメッセージを送信) アプリケーション サーバー。

不要になった、クライアント アプリケーションは、アプリ サーバーからメッセージを受信したいと考えていると、場合に、登録トークンを削除するアプリケーション サーバーに要求を送信、ことができます。 クライアント アプリケーションがメッセージのトピック (この記事の後半で説明します) 場合、は、トピックを取り消しできます。
クライアント アプリがデバイスからアンインストールされると、GCM はこれを検出し、自動的にサーバーに通知する、アプリケーション登録トークンを削除します。

Google の[クライアント アプリを登録する](https://developers.google.com/cloud-messaging/registration)さらに詳しくは、登録プロセスを説明します。 登録を解除し、サブスクリプションの解除、それについて説明し、クライアント アプリがアンインストールされるときに登録解除のプロセスについて説明します。



### <a name="downstream-messaging"></a>ダウン ストリームのメッセージング

アプリケーション サーバーは、クライアント アプリをダウン ストリーム メッセージを送信するときに、次の図に示す手順が次に示します。

[![ダウン ストリームのメッセージング ストアと転送の図](google-cloud-messaging-images/03-downstream-sml.png)](google-cloud-messaging-images/03-downstream.png#lightbox)

1.  アプリケーション サーバーは、GCM にメッセージを送信します。

2.  クライアント デバイスが利用できない場合は、GCM サーバーは、後から送信用のキューにメッセージを格納します。

3.  クライアント デバイスが利用できる場合、GCM は、そのデバイス上のクライアント アプリに、メッセージを送信します。

4.  クライアント アプリでは、GCM からメッセージを受信し、それに応じて処理を行います。 たとえば、メッセージがリモートの通知の場合は、ユーザーに表示されます。

(ここで、アプリ サーバーにメッセージを送信、1 つのクライアント アプリ) このメッセージング シナリオでメッセージが 4 kB までの長さを指定できます。

詳細については (コード サンプルを含む) で Android のダウン ストリームの GCM メッセージを受信を参照してください。[リモート通知](~/android/data-cloud/google-messaging/remote-notifications-with-gcm.md)です。


#### <a name="topic-messaging"></a>トピックのメッセージング

*トピックのメッセージング*ダウン ストリーム メッセージの種類は、アプリケーション サーバーが (天気予報) などのトピックを定期受信する複数のクライアント アプリ デバイスを 1 つのメッセージを送信します。 アプリごとの最大 100万サブスクリプションをサポートしているトピックのメッセージング、トピック メッセージは 2 KB までの長さにできます。 GCM をメッセージング トピックに対してのみ使用されている場合、クライアント アプリでは、アプリ サーバーに登録トークンを送信する必要はありません。 Google の[トピックのメッセージングを実装する](https://developers.google.com/cloud-messaging/topic-messaging)特定のトピックを定期受信する複数のデバイスにアプリのサーバーからメッセージを送信する方法について説明します。



#### <a name="group-messaging"></a>グループのメッセージング

*メッセージをグループ化*ダウン ストリーム メッセージの種類は、アプリケーション サーバーが、グループ (たとえば、1 人のユーザーに属しているデバイスのグループ) に属している複数のクライアント アプリのデバイスに 1 つのメッセージを送信します。 グループのメッセージは、iOS デバイス用の長さは最大 2 KB、最大で 4 KB の Android デバイス用の長さとします。 グループは、最大 20 のメンバーに制限されます。 Google の[デバイス グループのメッセージング](https://developers.google.com/cloud-messaging/notifications)グループに属しているデバイスで実行されている複数のクライアント アプリのインスタンスに、アプリ サーバーが 1 つのメッセージを送信する方法について説明します。


### <a name="upstream-messaging"></a>アップ ストリームのメッセージング

クライアント アプリはサポートしているサーバーに接続[XMPP](https://developers.google.com/cloud-messaging/ccs)、次の図に示すように、アプリ サーバーにメッセージを送信できます。

[![アップ ストリームのメッセージング ダイアグラム](google-cloud-messaging-images/04-upstream-sml.png)](google-cloud-messaging-images/04-upstream.png#lightbox)

1.  クライアント アプリでは、GCM XMPP 接続のサーバーにメッセージを送信します。

2.  アプリケーション サーバーが切断されている場合、GCM サーバーは、後で転送するためのキューにメッセージを格納します。

3.  アプリ サーバーが再度接続している場合、GCM は、アプリ サーバーにメッセージを転送します。

4.  アプリケーション サーバーがクライアント アプリの id を確認するメッセージを解析し、「確認」メッセージを確認するための GCM に送信します。

5.  アプリケーション サーバーでは、メッセージを処理します。

Google の[上流メッセージ](https://developers.google.com/cloud-messaging/ccs#upstream)JSON でエンコードされたメッセージを構成し、Google の XMPP ベース クラウド接続のサーバーを実行しているアプリケーション サーバーに送信する方法について説明します。


<a name="settingup" />

## <a name="setting-up-google-cloud-messaging"></a>Google Cloud Messaging 設定

GCM サービスを使用するには、アプリで、前に、最初に Google の GCM サーバーにアクセスするための資格情報を取得する必要があります。 次のセクションでは、このプロセスを完了に必要な手順について説明します。



### <a name="enable-google-services-for-your-app"></a>アプリが Google サービスを有効にします。

1.  サインイン、 [Google Developers Console](https://developers.google.com/mobile/add?platform=android)アカウント (つまり、gmail アドレス)、Google にし、新しいプロジェクトを作成します。 既存のプロジェクトがある場合は、するのには、GCM を有効にしたプロジェクトを選択します。 次の例では、新しいプロジェクトと呼ばれる**XamarinGCM**を作成します。

    [![XamarinGCM プロジェクトを作成します。](google-cloud-messaging-images/05-create-gcm-app-sml.png)](google-cloud-messaging-images/05-create-gcm-app.png#lightbox)

2.  次に、アプリのパッケージ名を入力 (この例では、パッケージ名は**com.xamarin.gcmexample**) をクリック**の選択を続行するサービスを構成および**:

    [![パッケージ名を入力します。](google-cloud-messaging-images/06-package-name-sml.png)](google-cloud-messaging-images/06-package-name.png#lightbox)

    このパッケージ名は、アプリのアプリケーション ID でも注意してください。

3.  **を選択してサービスを構成および**セクションには、アプリに追加できる Google サービスが一覧表示します。 をクリックして**メッセージング クラウド**:

    [![クラウドを選択メッセージング](google-cloud-messaging-images/07-choose-gcm-service-sml.png)](google-cloud-messaging-images/07-choose-gcm-service.png#lightbox)

4.  次に、をクリックして**GOOGLE CLOUD MESSAGING を有効にする**:

    [![Google Cloud Messaging を有効にします。](google-cloud-messaging-images/08-enable-gcm-sml.png)](google-cloud-messaging-images/08-enable-gcm.png#lightbox)

5.  A**サーバー API キー**と**送信者 ID**アプリ用に生成されます。 これらの値を記録し、をクリックして**閉じる**:

    [![サーバーの API キーと送信者 ID が表示されます。](google-cloud-messaging-images/09-get-api-key-and-id-sml.png)](google-cloud-messaging-images/09-get-api-key-and-id.png#lightbox)

    API キーを保護する&ndash;一般的な使用をものではありません。 API キーが侵害された場合、承認されていないサーバーは、クライアント アプリケーションにメッセージを公開する可能性があります。
    [ベスト プラクティスの API キーを使用した安全な](https://support.google.com/cloud/answer/6310037?hl=en)API キーを保護するため役立つガイドラインを提供します。



### <a name="view-your-project-settings"></a>プロジェクト設定を表示します。

サインインして、いつでもプロジェクトの設定を表示することができます、 [Google Cloud コンソール](https://console.cloud.google.com/)プロジェクトを選択するとします。 たとえば、表示することができます、**送信者 ID**ページの上部にあるドロップダウン リスト ボックスで、プロジェクトを選択して (この例では、プロジェクトと呼ばれる**XamarinGCM**)。 送信者 ID はこのスクリーン ショットに示すようにプロジェクトの数値 (送信者 ID をここでは、 **9349932736**)。

[![送信者 ID を表示します。](google-cloud-messaging-images/10-view-server-id-sml.png)](google-cloud-messaging-images/10-view-server-id.png#lightbox)

表示する、 **API キー**をクリックして**API マネージャー**  をクリックし、**資格情報**:

[![API キーを表示します。](google-cloud-messaging-images/11-view-credentials-sml.png)](google-cloud-messaging-images/11-view-credentials.png#lightbox)



## <a name="for-further-reading"></a>関連項目

-   Google の[クライアント アプリを登録する](https://developers.google.com/cloud-messaging/registration)の詳細については、クライアント登録プロセスを説明します。 自動再試行の構成と登録の状態の同期を保つに関する情報を提供します。

-   [RFC 6120](https://tools.ietf.org/html/rfc6120)と[RFC 6121](https://tools.ietf.org/html/rfc6121)について説明し、拡張可能なメッセージングおよびプレゼンス プロトコル (XMPP) を定義します。



## <a name="summary"></a>まとめ

この記事では、Google Cloud Messaging (GCM) の概要が提供されています。 識別し、アプリ サーバーとクライアント アプリケーション間のメッセージングを承認に使用されるさまざまな資格情報を説明します。 最も一般的なメッセージング シナリオを説明し、GCM GCM サービスを使用すると、アプリを登録する手順について詳しく説明します。


## <a name="related-links"></a>関連リンク

- [クラウドのメッセージング](https://developers.google.com/cloud-messaging/)
