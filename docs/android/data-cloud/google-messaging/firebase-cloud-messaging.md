---
title: Firebase Cloud Messaging
description: Firebase Cloud Messaging (FCM) は、モバイル アプリとサーバー アプリケーション間のメッセージングを容易にするサービスです。 この記事では、FCM のしくみの概要と、アプリは、FCM を使用できるように、Google Services を構成する方法について説明します。
ms.prod: xamarin
ms.assetid: E5314D7F-2AAC-40DA-BEBA-27C834F078DD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/31/2018
ms.openlocfilehash: bf0523a98c8e68691228b6e305265277e2925fa3
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61017485"
---
# <a name="firebase-cloud-messaging"></a>Firebase Cloud Messaging

_Firebase Cloud Messaging (FCM) は、モバイル アプリとサーバー アプリケーション間のメッセージングを容易にするサービスです。この記事では、FCM のしくみの概要と、アプリは、FCM を使用できるように、Google Services を構成する方法について説明します。_

[![Firebase Cloud Messaging ヒーローのイメージ](firebase-cloud-messaging-images/preview.png)](firebase-cloud-messaging-images/preview.png#lightbox)

このトピックでは Firebase Cloud Messaging、Xamarin.Android アプリと、アプリのサーバー間でメッセージのルーティング方法の概要を示し、FCM services アプリで使用できるように、資格情報を取得するための手順を説明します。

## <a name="overview"></a>概要

Firebase Cloud Messaging (FCM) は、送信、ルーティング、およびサーバー アプリケーションとモバイル クライアント アプリ間でメッセージ キューを処理するクロス プラットフォーム サービスです。 FCM は後続する Google Cloud Messaging (GCM)、し、Google play 開発者サービスを基盤に構築します。

次の図のように、FCM はメッセージの送信側とクライアント間の媒介として機能します。 A*クライアント アプリ*FCM 対応のアプリをデバイスで実行されているのです。 *アプリ サーバー* (またはお客様の会社によって提供される) は、クライアント アプリケーションが FCM を介して通信する FCM が有効なサーバーです。 FCM は GCM とは異なり、Firebase コンソール通知 GUI を使用して直接クライアント アプリにメッセージを送信することが可能。

[![クライアント アプリとアプリ サーバー間、FCM](firebase-cloud-messaging-images/01-server-fcm-app-sml.png)](firebase-cloud-messaging-images/01-server-fcm-app.png#lightbox)

FCM を使用して、アプリのサーバー メッセージを送信できます 1 つのデバイス、デバイス グループまたはトピックにサブスクライブしているデバイス数。 クライアント アプリでは、アプリケーション サーバー (たとえば、リモート通知を受信する場合など) からのダウン ストリームのメッセージをサブスクライブする FCM を使用できます。 Firebase のメッセージのさまざまな種類の詳細については、次を参照してください。 [FCM メッセージについて](https://firebase.google.com/docs/cloud-messaging/concept-options)します。

## <a name="fcm-in-action"></a>アクションの firebase Cloud Messaging

アプリケーション サーバーからクライアント アプリにダウン ストリーム メッセージが送信されると、アプリ サーバーが、メッセージを送信、 *FCM 接続サーバー* ; Google から提供される、FCM 接続サーバーを実行しているデバイスにメッセージをさらに、転送、クライアント アプリ。 HTTP 経由でメッセージを送信できるまたは[XMPP](https://developers.google.com/cloud-messaging/ccs) (拡張可能なメッセージングとプレゼンス プロトコル)。 クライアント アプリは常に接続されていないため、または実行中、FCM 接続サーバー エンキューとストア メッセージ、再接続し、使用可能になるクライアント アプリに送信します。 同様に、FCM はエンキュー アプリ サーバーにクライアント アプリからアップ ストリームのメッセージ アプリ サーバーが使用できない場合。 詳細については、FCM 接続のサーバーは、次を参照してください。 [Firebase クラウド メッセージング サーバーについて](https://firebase.google.com/docs/cloud-messaging/server)します。

FCM がアプリ サーバーと、クライアント アプリを識別するために次の資格情報を使用して、FCM を使用したトランザクション メッセージを承認するためにこれらの資格情報を使用しています。

-   <a name="fcm-in-action-sender-id"></a>**Sender ID** &ndash; 、 *Sender ID* Firebase プロジェクトを作成するときに割り当てられる一意の数値します。 送信者 ID は、クライアント アプリにメッセージを送信する各アプリケーション サーバーを識別するために使用されます。 Sender ID も、プロジェクトの数。プロジェクトを登録するときに、Firebase コンソールから送信者 ID を取得します。 Sender ID の例は、`496915549731`します。

-   <a name="fcm-in-action-api-key"></a>**API キー** &ndash; 、 *API キー* Firebase サービスへのアプリ サーバーへのアクセスを提供FCM では、アプリ サーバーの認証にこのキーを使用します。 この資格情報と呼ばれることも、*サーバー キー*または*Web API キー*します。 API キーの例は、`AJzbSyCTcpfRT1YRqbz-jIwp1h06YdauvewGDzk`します。

-   <a name="fcm-in-action-app-id"></a>**アプリ ID** &ndash; FCM からメッセージを受信登録する (任意指定のデバイスに依存しない) クライアント アプリの id。 アプリ ID の例は、`1:415712510732:android:0e1eb7a661af2460`します。

-   <a name="fcm-in-action-registration-token"></a>**登録トークン** &ndash; 、*登録トークン*(とも呼ばれます、*インスタンス ID*) は、特定のデバイス上のクライアント アプリの FCM id です。 登録トークンは、実行時に生成される&ndash;アプリがデバイス上で実行中に FCM で最初に登録する際に、登録トークンを受信します。 登録トークンは、FCM からメッセージを受信する (その特定のデバイスで実行されている) クライアント アプリのインスタンスを許可します。
    登録トークンの例は、 `fkBQTHxKKhs:AP91bHuEedxM4xFAUn0z ... JKZS` (非常に長い文字列)。

[設定を Firebase Cloud Messaging](#setup_fcm)プロジェクトを作成し、これらの資格情報の生成の詳細な手順を提供します (このガイドで後述)。 新しいプロジェクトを作成するときに、 [Firebase Console](https://console.firebase.google.com/)、資格情報ファイルと呼ばれる**google-services.json**が作成される&ndash;で説明したように、このファイルをXamarin.Androidプロジェクトに追加[FCM を使用したリモート通知](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md)します。

次のセクションでは、これらの資格情報を使用してクライアント アプリケーションが FCM を通じてアプリ サーバーと通信する方法について説明します。


<a name="registration" />

### <a name="registration-with-fcm"></a>FCM に登録

メッセージングを行う前に、クライアント アプリの登録は FCM で最初必要があります。 クライアント アプリでは、次の図に示すように登録手順を実行する必要があります。

[![アプリ登録の手順の図](firebase-cloud-messaging-images/02-app-registration-sml.png)](firebase-cloud-messaging-images/02-app-registration.png#lightbox)

1.  クライアント アプリは、FCM に送信者の ID、API キー、およびアプリ ID を渡す、登録トークンを取得する FCM にアクセスします。

2.  FCM では、クライアント アプリを登録トークンを返します。

3.  クライアント アプリは、(必要に応じて) アプリ サーバーに登録トークンを転送します。

アプリ サーバーは、クライアント アプリとの通信を後続の登録トークンをキャッシュします。 アプリ サーバーは、登録トークンを受信したことを示すためにクライアント アプリに受信確認を送信することができます。 このハンドシェイクが行われた後、クライアント アプリできますからメッセージを受信 (またはメッセージを送信) アプリ サーバー。 クライアント アプリでは、古いトークンが侵害された場合に新しい登録トークンを受け取ることがあります (を参照してください[FCM を使用したリモート通知](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md)アプリが登録トークンの更新プログラムを受信する方法の例については)。

クライアント アプリは、不要になったアプリ サーバーからメッセージを受信する場合、登録トークンを削除するアプリのサーバーに要求を送信できます。 クライアント アプリがデバイスからアンインストールされると、FCM はこれを検出し、登録トークンを削除するアプリのサーバーを自動的に通知します。



### <a name="downstream-messaging"></a>ダウン ストリームのメッセージング

次の図は、どの Firebase Cloud Messaging を格納し、ダウン ストリーム メッセージ転送には。

[![FCM のダウン ストリームのメッセージング ストア アンド フォワードを使用します。](firebase-cloud-messaging-images/03-downstream-sml.png)](firebase-cloud-messaging-images/03-downstream.png#lightbox)

アプリ サーバーは、クライアント アプリにダウン ストリーム メッセージを送信するときに、上記の図に列挙として、次の手順を使用します。

1.  アプリ サーバーは、FCM にメッセージを送信します。

2.  クライアント デバイスが利用できない場合、FCM サーバーは、後でキューにメッセージを格納します。 メッセージは、最大 4 週間の FCM ストレージで保持されます (詳細については、次を参照してください。[メッセージの有効期間を設定](https://firebase.google.com/docs/cloud-messaging/concept-options#ttl))。

3.  クライアント デバイスが利用できる場合、FCM は、そのデバイス上のクライアント アプリにメッセージを転送します。

4.  クライアント アプリが FCM からメッセージを受信、処理、およびユーザーに表示します。 たとえば、メッセージがリモート通知の場合は、通知領域でユーザーに表示されます。

(アプリ サーバーが送信する場所、メッセージを 1 つのクライアント アプリに) このメッセージング シナリオでメッセージが最大 4 kB の長さを指定できます。

Android でダウン ストリームの FCM メッセージの受信の詳細については、次を参照してください。 [FCM を使用したリモート通知](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md)します。

### <a name="topic-messaging"></a>トピックのメッセージング

*トピックの「Messaging*アプリ サーバーで特定トピックを選択する複数デバイス メッセージを送信することになります。 作成し、Firebase コンソール通知 GUI を使用してトピックのメッセージを送信できます。 FCM では、ルーティングとトピックのメッセージのサブスクライブしているクライアントへの配信を処理します。 この機能は、気象アラート、株価情報、ニュースのヘッドラインなどのメッセージを使用できます。

[![トピックの「メッセージング ダイアグラム](firebase-cloud-messaging-images/04-topic-messaging-sml.png)](firebase-cloud-messaging-images/04-topic-messaging.png#lightbox)

次の手順は、(後にクライアント アプリは、前述のように、登録トークンを取得します)、メッセージングのトピックで使用されます。

1.  クライアント アプリは、FCM に購読のメッセージを送信してトピックをサブスクライブします。

2.  アプリ サーバーは、配布、FCM にトピックのメッセージを送信します。

3.  FCM では、トピックのメッセージをそのトピックにサブスクライブしているクライアントに転送します。

Firebase トピック メッセージングの詳細については、Google を参照してください。[トピックでは、Android でメッセージング](https://firebase.google.com/docs/cloud-messaging/android/topic-messaging)します。


<a name="setup_fcm" />

## <a name="setting-up-firebase-cloud-messaging"></a>Firebase Cloud Messaging 設定

FCM サービスを使用するには、アプリで、前にする必要があります、新しいプロジェクトの作成 (またはインポートする既存のプロジェクト) を使用して、 [Firebase Console](https://console.firebase.google.com/)します。 Firebase Cloud Messaging、アプリのプロジェクトを作成するのにには、次の手順を使用します。

1.  サインイン、 [Firebase Console](https://console.firebase.google.com/) 、Google アカウント (つまり、Gmail アドレス) クリックと**プロジェクトの新規作成**:

    [![新しいプロジェクト ボタンを作成します。](firebase-cloud-messaging-images/05-firebase-console-sml.png)](firebase-cloud-messaging-images/05-firebase-console.png#lightbox)

    既存のプロジェクトがあれば、クリックして**Google プロジェクトのインポート**します。

2.  **プロジェクトを作成する**ダイアログ ボックスで、プロジェクトの名前を入力し、クリックして**プロジェクトの作成**です。 次の例では、新しいプロジェクトと呼ばれる**XamarinFCM**が作成されます。

    [![プロジェクト ダイアログを作成します。](firebase-cloud-messaging-images/06-create-a-project-sml.png)](firebase-cloud-messaging-images/06-create-a-project.png#lightbox)

3.  Firebase コンソールで**概要**、 をクリックして**して Android アプリに Firebase を追加**:

    [![Android アプリに Firebase を追加します。](firebase-cloud-messaging-images/07-add-firebase-sml.png)](firebase-cloud-messaging-images/07-add-firebase.png#lightbox)

4.  次の画面で、アプリのパッケージの名前を入力します。 この例では、パッケージ名は**com.xamarin.fcmexample**します。 この値は、Android アプリのパッケージ名に一致する必要があります。 アプリのニックネームを入力することも、**アプリ ニックネーム**フィールド。

    [![アプリのニックネーム FCM 例を入力します。](firebase-cloud-messaging-images/08-package-name-sml.png)](firebase-cloud-messaging-images/08-package-name.png#lightbox)

5.  アプリでは、動的リンク、招待、または Google の認証を使用する場合、デバッグの署名証明書を入力する必要もあります。 署名証明書を見つける方法についての詳細については、次を参照してください。[キーストアの MD5 または SHA1 署名の検索](~/android/deploy-test/signing/keystore-signature.md)します。
    この例で、署名証明書が空白になります。

6.  クリックして**アプリの追加**:

    [![アプリの追加 をクリックします。](firebase-cloud-messaging-images/09-add-app-sml.png)](firebase-cloud-messaging-images/09-add-app.png#lightbox)

    サーバーの API キーとクライアント ID は、アプリの自動的に生成されます。 この情報がパッケージ化、 **google-services.json**をクリックすると自動的にダウンロードされるファイル**アプリの追加**します。
    必ず安全な場所にこのファイルを保存してください。

追加する方法の詳細な例について**google-services.json** android FCM プッシュ通知メッセージを受信するアプリのプロジェクトを参照してください。 [FCM を使用したリモート通知](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md)します。



## <a name="for-further-reading"></a>参考資料

-   Google の[Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) Firebase Cloud Messaging の主な機能の概要、しくみ、およびセットアップ手順の説明を提供します。

-   Google の[アプリ サーバーの送信要求のビルド](https://firebase.google.com/docs/cloud-messaging/send-message)アプリ サーバーとのメッセージを送信する方法について説明します。

-   [RFC 6120](https://tools.ietf.org/html/rfc6120)と[RFC 6121](https://tools.ietf.org/html/rfc6121)について説明し、拡張可能なメッセージングとプレゼンス プロトコル (XMPP) を定義します。

-   [FCM メッセージについて](https://firebase.google.com/docs/cloud-messaging/concept-options)Firebase Cloud Messaging と送信できるメッセージのさまざまな種類について説明します。

## <a name="summary"></a>まとめ

この記事では、Firebase Cloud Messaging (FCM) の概要を提供します。 特定し、アプリ サーバーとクライアント アプリ間のメッセージングを承認に使用されるさまざまな資格情報がについて説明します。 登録とダウン ストリームのメッセージング シナリオを示すことと、FCM FCM サービスを使用すると、アプリを登録する手順について詳しく説明します。


## <a name="related-links"></a>関連リンク

- [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/)
