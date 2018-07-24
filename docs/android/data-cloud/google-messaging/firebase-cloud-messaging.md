---
title: Firebase クラウド メッセージング
description: Firebase クラウド メッセージング (FCM) は、モバイル アプリとサーバー アプリケーション間のメッセージングを容易にするサービスです。 この記事は、FCM のしくみの概要を説明および FCM をアプリケーションで使用できるように、Google サービスを構成する方法について説明します。
ms.prod: xamarin
ms.assetid: E5314D7F-2AAC-40DA-BEBA-27C834F078DD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: ef2c23d16545d03dc267054a96f8b0f8883afcf1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30769820"
---
# <a name="firebase-cloud-messaging"></a>Firebase クラウド メッセージング

_Firebase クラウド メッセージング (FCM) は、モバイル アプリとサーバー アプリケーション間のメッセージングを容易にするサービスです。この記事は、FCM のしくみの概要を説明および FCM をアプリケーションで使用できるように、Google サービスを構成する方法について説明します。_

[![Firebase Cloud Messaging のヒーローの画像](firebase-cloud-messaging-images/preview.png)](firebase-cloud-messaging-images/preview.png#lightbox)

Xamarin.Android アプリと、アプリ サーバー間でメッセージをルーティングする Firebase Cloud Messaging 方法の大まかな概要を説明し、アプリが FCM サービスを使用できるように、資格情報を取得するための手順を追って説明を提供します。



## <a name="overview"></a>概要

Firebase クラウド メッセージング (FCM) は、クロス プラットフォーム サービスを送信する、ルーティング、およびサーバー アプリケーションとモバイル クライアント アプリの間でメッセージ キューを処理します。 FCM は後続する Google Cloud Messaging (GCM)、および Google Play サービスに基づいて構築されています。

次の図にあるよう、FCM はメッセージの送信者とクライアント間の媒介として機能します。 A*クライアント アプリ*デバイス上で実行される FCM が有効なアプリケーションです。 *アプリ サーバー* (またはお客様の会社によって提供される) FCM 経由でクライアント アプリで通信する FCM が有効なサーバーです。 GCM とは異なり FCM できるようになります Firebase コンソール通知 GUI を使用して直接クライアント アプリケーションにメッセージを送信します。

[![クライアント アプリケーションと、アプリ サーバーの間に位置 FCM](firebase-cloud-messaging-images/01-server-fcm-app-sml.png)](firebase-cloud-messaging-images/01-server-fcm-app.png#lightbox)

FCM を使用すると、アプリ サーバー メッセージ送信できます 1 台のデバイス、デバイスのグループまたはトピックにサブスクライブしているデバイスの数。 クライアント アプリでは、(たとえば、リモートの通知を受信する場合)、アプリ サーバーからのダウン ストリーム メッセージをサブスクライブする FCM を使用できます。 Firebase メッセージのさまざまな種類の詳細については、次を参照してください。 [FCM メッセージについて](https://firebase.google.com/docs/cloud-messaging/concept-options)です。



## <a name="firebase-cloud-messaging-in-action"></a>アクションでメッセージング firebase クラウド

アプリケーション サーバーが、メッセージを送信するアプリのサーバーからクライアント アプリをダウン ストリーム メッセージが送信されると、 *FCM 接続サーバー* ; Google から提供される、FCM 接続サーバー、さらに、メッセージ転送が実行されているデバイスにはクライアント アプリです。 HTTP 経由でメッセージを送信できますまたは[XMPP](https://developers.google.com/cloud-messaging/ccs) (拡張のメッセージングおよびプレゼンス プロトコル) です。 クライアント アプリは常に接続されていないためか、実行されて、FCM 接続サーバーのキューに入れます、ストア メッセージ、再接続し、使用可能になると、クライアント アプリに送信することです。 同様に、FCM はエンキュー アプリ サーバーにクライアント アプリケーションからメッセージをアップ ストリームのアプリ サーバーが使用できない場合。 FCM 接続サーバーの詳細についてを参照してください。[に関する Firebase クラウド メッセージング サーバー](https://firebase.google.com/docs/cloud-messaging/server)です。

FCM では、次の資格情報を使用して、アプリ サーバーとクライアント アプリを識別し、FCM 経由のメッセージのトランザクションを承認するためにこれらの資格情報を使用します。

-   **送信者 ID** &ndash; 、*送信者 ID* Firebase プロジェクトを作成するときに割り当てられる一意の数値します。 送信者 ID を使用して、クライアント アプリケーションにメッセージを送信する各アプリケーション サーバーを識別します。 送信者 ID も、プロジェクトの数です。プロジェクトを登録するときに、Firebase コンソールから送信者の ID を取得します。 送信者 ID の例は`496915549731`します。

-   **API キー** &ndash; 、 *API キー* Firebase services; へのアプリ サーバーへのアクセスを提供FCM は、アプリ サーバーの認証にこのキーを使用します。 この資格情報とも呼ばれる、*サーバー キー*または*Web API キー*です。 API キーの例は`AJzbSyCTcpfRT1YRqbz-jIwp1h06YdauvewGDzk`します。

-   **アプリ ID** &ndash; FCM からメッセージを受信登録する独立した、特定のデバイスのクライアント アプリの id。 アプリ ID の例は`1:415712510732:android:0e1eb7a661af2460`します。

-   **登録トークン** &ndash; 、*登録トークン*(とも呼びます、*インスタンス ID*) に特定のデバイスでクライアント アプリの FCM id です。 実行時に、登録トークンが生成された&ndash;アプリは、最初に登録した FCM デバイスで実行中に、登録トークンを受信します。 登録トークンでは、(その特定のデバイスで実行されている) クライアント アプリのインスタンスを承認 FCM からメッセージを受信します。
    登録トークンの例は`fkBQTHxKKhs:AP91bHuEedxM4xFAUn0z ... JKZS`(非常に長い文字列)。

[設定を Firebase Cloud Messaging](#setup_fcm) (このガイドで後述) プロジェクトを作成し、これらの資格情報の生成の詳細な手順を提供します。 新しいプロジェクトを作成する場合、 [Firebase コンソール](https://console.firebase.google.com/)、資格情報ファイルと呼ばれる**google services.json**が作成される&ndash;で説明したように、このファイルをXamarin.Androidプロジェクトに追加[リモート FCM 通知](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md)です。

次のセクションでは、これらの資格情報を使用してクライアント アプリが FCM を通じてアプリ サーバーと通信する方法について説明します。


<a name="registration" />

### <a name="registration-with-fcm"></a>FCM の登録

メッセージングを行う前に、クライアント アプリの登録は FCM でまず必要があります。 クライアント アプリでは、次の図に示すように登録手順を実行する必要があります。

[![アプリの登録の手順のダイアグラム](firebase-cloud-messaging-images/02-app-registration-sml.png)](firebase-cloud-messaging-images/02-app-registration.png#lightbox)

1.  クライアント アプリにアクセス FCM FCM に送信者の ID、API キーおよびアプリ ID を渡す、登録トークンを取得します。

2.  FCM は、クライアント アプリを登録トークンを返します。

3.  クライアント アプリでは、アプリ サーバーの登録トークン (必要に応じて) 転送します。

アプリケーション サーバーでは、クライアント アプリケーションとの通信を後続の登録トークンをキャッシュします。 アプリケーション サーバーは、登録トークンを受信したことを示すためにクライアント アプリケーションに戻る受信確認を送信できます。 このハンドシェイクが行われた後、クライアント アプリできますからメッセージを受信 (またはメッセージを送信) アプリケーション サーバー。 クライアント アプリを古いトークンが侵害された場合、新しい登録トークンが表示される可能性があります (を参照してください[FCM リモート通知](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md)アプリが登録トークンの更新プログラムを受信する方法の例については)。

不要になった、クライアント アプリケーションは、アプリ サーバーからメッセージを受信したいと考えていると、場合に、登録トークンを削除するアプリケーション サーバーに要求を送信、ことができます。 クライアント アプリがデバイスからアンインストールされると、FCM はこれを検出し、自動的にサーバーに通知する、アプリケーション登録トークンを削除します。



### <a name="downstream-messaging"></a>ダウン ストリームのメッセージング

次の図は、どの Firebase クラウド メッセージングを格納およびダウン ストリーム メッセージ転送を示しています。

[![FCM のダウン ストリームのメッセージング ストア アンド フォワードを使用します。](firebase-cloud-messaging-images/03-downstream-sml.png)](firebase-cloud-messaging-images/03-downstream.png#lightbox)

アプリケーション サーバーは、クライアント アプリをダウン ストリーム メッセージを送信するときに、上記の図に列挙として、次の手順を使用します。

1.  アプリケーション サーバーは、FCM にメッセージを送信します。

2.  クライアント デバイスが使用できない場合、FCM サーバーは、後から送信のキューにメッセージを格納します。 メッセージは、最大 4 週間の FCM ストレージに保持されます (詳細については、次を参照してください。[メッセージの有効期間を設定](https://firebase.google.com/docs/cloud-messaging/concept-options#ttl))。

3.  クライアント デバイスが利用できる場合、FCM はそのデバイス上のクライアント アプリにメッセージを転送します。

4.  クライアント アプリでは、FCM からメッセージを受信、処理、および、ユーザーに表示します。 たとえば、メッセージがリモートの通知の場合は、通知領域内のユーザーに表示されます。

(ここで、アプリ サーバーにメッセージを送信、1 つのクライアント アプリ) このメッセージング シナリオでメッセージが 4 kB までの長さを指定できます。

Android でのダウン ストリームの FCM メッセージの受信の詳細については、次を参照してください。 [FCM リモート通知](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md)です。

### <a name="topic-messaging"></a>トピックのメッセージング

*トピックのメッセージング*アプリ サーバーの特定のトピックをで選択した複数のデバイスに、メッセージを送信できるようになります。 作成し、Firebase コンソール通知 GUI を使用してトピックのメッセージを送信できます。 FCM は、ルーティングとサブスクライブしているクライアントをトピック メッセージの配信を処理します。 この機能は、気象アラート、株価情報、ヘッドラインなどのメッセージを使用できます。

[![トピックのメッセージング ダイアグラム](firebase-cloud-messaging-images/04-topic-messaging-sml.png)](firebase-cloud-messaging-images/04-topic-messaging.png#lightbox)

次の手順は、(クライアント アプリでは、上の説明に従って登録トークンを取得) 後に、トピックのメッセージングで使用されます。

1.  クライアント アプリは、FCM に購読メッセージを送信することによって、トピックにサブスクライブします。

2.  アプリケーション サーバーでは、配布用 FCM をトピック メッセージを送信します。

3.  FCM は、トピックのメッセージをそのトピックにサブスクライブしているクライアントに転送します。

Firebase トピック メッセージングの詳細については、Google を参照してください。[トピックが Android でメッセージング](https://firebase.google.com/docs/cloud-messaging/android/topic-messaging)です。


<a name="setup_fcm" />

## <a name="setting-up-firebase-cloud-messaging"></a>Firebase セットアップ クラウド メッセージング

FCM サービスを使用するには、アプリで、前にする必要があります、新しいプロジェクトを作成する (またはインポートする既存のプロジェクト) を使用して、 [Firebase コンソール](https://console.firebase.google.com/)です。 Firebase Cloud Messaging アプリのプロジェクトを作成するのにには、次の手順を使用します。

1.  サインイン、 [Firebase コンソール](https://console.firebase.google.com/)Google アカウント (つまり、Gmail アドレス) とクリックで**プロジェクトの新規作成**:

    [![新しいプロジェクト ボタンを作成します。](firebase-cloud-messaging-images/05-firebase-console-sml.png)](firebase-cloud-messaging-images/05-firebase-console.png#lightbox)

    既存のプロジェクトがあれば、クリックして**Google プロジェクトのインポート**です。

2.  **プロジェクトを作成する**ダイアログ ボックスで、プロジェクトの名前を入力し、クリックして**プロジェクトの作成**です。 次の例では、新しいプロジェクトと呼ばれる**XamarinFCM**を作成します。

    [![プロジェクト ダイアログ ボックスを作成します。](firebase-cloud-messaging-images/06-create-a-project-sml.png)](firebase-cloud-messaging-images/06-create-a-project.png#lightbox)

3.  Firebase コンソールで**概要**をクリックして**の Android アプリを追加 Firebase**:

    [![Android アプリに追加して Firebase](firebase-cloud-messaging-images/07-add-firebase-sml.png)](firebase-cloud-messaging-images/07-add-firebase.png#lightbox)

4.  次の画面で、アプリのパッケージ名を入力します。 この例では、パッケージ名は**com.xamarin.fcmexample**です。 この値は、Android アプリのパッケージ名と一致する必要があります。 アプリのニックネームを入力することも、**アプリ ニックネーム**フィールド。

    [![アプリのニックネームとして FCM 例を入力します。](firebase-cloud-messaging-images/08-package-name-sml.png)](firebase-cloud-messaging-images/08-package-name.png#lightbox)

5.  アプリでは、動的リンク、招待、または Google の認証を使用する場合は、署名証明書、デバッグも入力する必要があります。 詳細については、署名証明書を検索する、次を参照してください。[キーストアの MD5 または SHA1 署名検索](~/android/deploy-test/signing/keystore-signature.md)です。
    この例では、署名証明書は空白のままにします。

6.  をクリックして**追加アプリ**:

    [![[アプリの追加] をクリックします。](firebase-cloud-messaging-images/09-add-app-sml.png)](firebase-cloud-messaging-images/09-add-app.png#lightbox)

    サーバー API キーと、クライアント ID は、アプリの自動的に生成されます。 この情報がパッケージ、 **google services.json**をクリックすると自動的にダウンロードされるファイル**アプリの追加**です。
    このファイルを安全な場所に保存することを確認します。

追加する方法の詳細な例について**google services.json** Android で FCM プッシュ通知のメッセージを受信するアプリのプロジェクトを参照してください。 [FCM 使用して通知をリモート](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md)です。



## <a name="for-further-reading"></a>関連項目

-   Google の[Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) Firebase Cloud Messaging の主な機能の概要、しくみ、およびセットアップ手順の説明を提供します。

-   Google の[要求送信アプリ サーバーのビルド](https://firebase.google.com/docs/cloud-messaging/send-message)アプリ サーバーとのメッセージを送信する方法について説明します。

-   [RFC 6120](https://tools.ietf.org/html/rfc6120)と[RFC 6121](https://tools.ietf.org/html/rfc6121)について説明し、拡張可能なメッセージングおよびプレゼンス プロトコル (XMPP) を定義します。



## <a name="summary"></a>まとめ

この記事では、Firebase クラウド メッセージング (FCM) の概要が提供されています。 識別し、アプリ サーバーとクライアント アプリケーション間のメッセージングを承認に使用されるさまざまな資格情報を説明します。 登録およびダウン ストリームのメッセージング シナリオを示すことと、FCM FCM サービスを使用すると、アプリを登録する手順について詳しく説明します。


## <a name="related-links"></a>関連リンク

- [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/)
