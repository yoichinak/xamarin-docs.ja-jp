---
title: Firebase Cloud Messaging
description: 焼討 base Cloud Messaging (FCM) は、モバイルアプリとサーバーアプリケーション間のメッセージングを容易にするサービスです。 この記事では、FCM のしくみの概要について説明します。また、アプリで FCM を使用できるように Google サービスを構成する方法についても説明します。
ms.prod: xamarin
ms.assetid: E5314D7F-2AAC-40DA-BEBA-27C834F078DD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/31/2018
ms.openlocfilehash: ab42e190f5348de13610955f1175eb01531a280a
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70754551"
---
# <a name="firebase-cloud-messaging"></a>Firebase Cloud Messaging

_焼討 base Cloud Messaging (FCM) は、モバイルアプリとサーバーアプリケーション間のメッセージングを容易にするサービスです。この記事では、FCM のしくみの概要について説明します。また、アプリで FCM を使用できるように Google サービスを構成する方法についても説明します。_

[![焼討 base Cloud Messaging ヒーローの画像](firebase-cloud-messaging-images/preview.png)](firebase-cloud-messaging-images/preview.png#lightbox)

このトピックでは、アプリケーションで FCM サービスを使用できるように、焼討 Base Cloud Messaging がメッセージを Xamarin Android アプリとアプリサーバー間でルーティングする方法の概要を説明します。また、資格情報を取得するための詳細な手順についても説明します。

## <a name="overview"></a>概要

焼討 base Cloud Messaging (FCM) は、サーバーアプリケーションとモバイルクライアントアプリ間でのメッセージの送信、ルーティング、およびキューを処理するクロスプラットフォームサービスです。 FCM は Google Cloud Messaging (GCM) の後継であり、Google Play 開発者サービス上に構築されています。

次の図に示すように、FCM は、メッセージの送信者とクライアントの仲介役として機能します。 *クライアントアプリ*は、デバイス上で実行される fcm 対応アプリです。 *アプリサーバー* (お客様または会社が提供) は、クライアントアプリが fcm 経由で通信する fcm 対応のサーバーです。 GCM とは異なり、FCM を使用すると、焼討 Base コンソール通知 GUI を使用して直接クライアントアプリにメッセージを送信できます。

[![クライアントアプリとアプリサーバーの間に FCM がある](firebase-cloud-messaging-images/01-server-fcm-app-sml.png)](firebase-cloud-messaging-images/01-server-fcm-app.png#lightbox)

FCM を使用すると、アプリサーバーは、1つのデバイス、デバイスのグループ、またはトピックにサブスクライブしている複数のデバイスにメッセージを送信できます。 クライアントアプリでは、FCM を使用して、アプリサーバーからのダウンストリームメッセージをサブスクライブできます (たとえば、リモート通知を受信します)。 さまざまな種類の焼討 Base メッセージの詳細については、「 [FCM メッセージについ](https://firebase.google.com/docs/cloud-messaging/concept-options)て」を参照してください。

## <a name="fcm-in-action"></a>動作中の焼討 base Cloud Messaging

アプリケーションサーバーからクライアントアプリにダウンストリームメッセージが送信されると、アプリサーバーは Google によって提供される*Fcm 接続サーバー*にメッセージを送信します。さらに、FCM 接続サーバーは、クライアントアプリを実行しているデバイスにメッセージを転送します。 メッセージは、HTTP または[Xmpp](https://developers.google.com/cloud-messaging/ccs) (拡張可能なメッセージングおよびプレゼンスプロトコル) を介して送信できます。 クライアントアプリは常に接続または実行されていないため、FCM 接続サーバーはメッセージをキューに入れて格納し、再接続して使用できるようになったときにクライアントアプリに送信します。 同様に、アプリサーバーが使用できない場合は、FCM によって、クライアントアプリからのアップストリームメッセージがアプリサーバーにエンキューされます。 FCM 接続サーバーの詳細については、「サービス[につい](https://firebase.google.com/docs/cloud-messaging/server)て」を参照してください。

FCM は、次の資格情報を使用してアプリサーバーとクライアントアプリを識別し、これらの資格情報を使用して FCM 経由でメッセージトランザクションを承認します。

- <a name="fcm-in-action-sender-id"></a>**送信者 ID**Sender ID は、焼討ベースプロジェクトを作成するときに割り当てられる一意の数値です。 &ndash; 送信者 ID は、クライアントアプリにメッセージを送信できる各アプリサーバーを識別するために使用されます。 送信者 ID もプロジェクト番号です。プロジェクトを登録するときに、焼討 Base コンソールから送信者 ID を取得します。 送信者 ID の例は`496915549731`です。

- <a name="fcm-in-action-api-key"></a>**API キー**API キーを使用すると、アプリサーバーは、焼討 base サービスにアクセスできます。 &ndash;FCM は、このキーを使用してアプリサーバーを認証します。 この資格情報は、*サーバーキー*または*Web API キー*とも呼ばれます。 API キーの例として`AJzbSyCTcpfRT1YRqbz-jIwp1h06YdauvewGDzk`は、があります。

- <a name="fcm-in-action-app-id"></a>**アプリ ID**&ndash; Fcm からメッセージを受信するように登録されているクライアントアプリの id (特定のデバイスに依存しません)。 アプリ ID の例として`1:415712510732:android:0e1eb7a661af2460`は、があります。

- <a name="fcm-in-action-registration-token"></a>**登録トークン**登録トークン (*インスタンス ID*とも呼ばれます) は、特定のデバイス上のクライアントアプリの fcm id です。 &ndash; 登録トークンは、実行&ndash;時にアプリがデバイスで実行中に fcm に最初に登録するときに登録トークンを受け取るときに生成されます。 登録トークンは、(特定のデバイスで実行されている) クライアントアプリのインスタンスに対して、FCM からのメッセージの受信を承認します。
    登録トークンの例として`fkBQTHxKKhs:AP91bHuEedxM4xFAUn0z ... JKZS`は、(非常に長い文字列) などがあります。

[焼討 Base Cloud Messaging の設定](#setup_fcm)(このガイドの後半) では、プロジェクトを作成し、これらの資格情報を生成するための詳細な手順について説明します。 新しいプロジェクトを[焼討 base コンソール](https://console.firebase.google.com/)で作成すると、「 [fcm を使用したリモート通知](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md)」で説明&ndash;され**ているよう**に、このファイルを Xamarin. Android プロジェクトに追加します。

次のセクションでは、クライアントアプリが FCM 経由でアプリサーバーと通信するときに、これらの資格情報を使用する方法について説明します。

<a name="registration" />

### <a name="registration-with-fcm"></a>FCM への登録

クライアントアプリは、メッセージングを実行する前に、まず FCM に登録する必要があります。 クライアントアプリは、次の図に示す登録手順を完了する必要があります。

[![アプリの登録手順の図](firebase-cloud-messaging-images/02-app-registration-sml.png)](firebase-cloud-messaging-images/02-app-registration.png#lightbox)

1. クライアントアプリは、FCM に接続して登録トークンを取得し、送信者 ID、API キー、およびアプリ ID を FCM に渡します。

2. FCM は、クライアントアプリに登録トークンを返します。

3. クライアントアプリ (必要に応じて) は、登録トークンをアプリサーバーに転送します。

アプリサーバーは、クライアントアプリとの後続の通信用に登録トークンをキャッシュします。 アプリサーバーは、登録トークンを受信したことを示す受信確認をクライアントアプリに返信できます。 このハンドシェイクが行われると、クライアントアプリはアプリサーバーからメッセージを受信する (またはメッセージを送信する) ことができます。 古いトークンが侵害された場合、クライアントアプリは新しい登録トークンを受け取ることがあります (アプリが登録トークンの更新を受け取る方法の例については、「 [FCM を使用したリモート通知](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md)」を参照してください)。

クライアントアプリがアプリサーバーからメッセージを受信する必要がなくなった場合は、アプリサーバーに要求を送信して、登録トークンを削除できます。 クライアントアプリがデバイスからアンインストールされると、FCM はこれを検出し、登録トークンを削除するようにアプリサーバーに自動的に通知します。

### <a name="downstream-messaging"></a>ダウンストリームメッセージング

次の図は、焼討 Base Cloud Messaging が下流のメッセージを格納および転送する方法を示しています。

[![FCM はダウンストリームメッセージングにストアアンドフォワードを使用します](firebase-cloud-messaging-images/03-downstream-sml.png)](firebase-cloud-messaging-images/03-downstream.png#lightbox)

アプリケーションサーバーは、ダウンストリームメッセージをクライアントアプリに送信するときに、上の図で列挙されている次の手順を使用します。

1. アプリサーバーは、メッセージを FCM に送信します。

2. クライアントデバイスが使用できない場合、FCM サーバーはメッセージをキューに格納して後で送信します。 メッセージは、最大4週間で FCM ストレージに保持されます (詳細については、「[メッセージの有効期間の設定](https://firebase.google.com/docs/cloud-messaging/concept-options#ttl)」を参照してください)。

3. クライアントデバイスが利用可能になると、FCM はそのデバイス上のクライアントアプリにメッセージを転送します。

4. クライアントアプリは、FCM からメッセージを受信して処理し、ユーザーに表示します。 たとえば、メッセージがリモート通知の場合、通知領域にユーザーに表示されます。

このメッセージングシナリオ (アプリサーバーが1つのクライアントアプリにメッセージを送信する場合) では、メッセージの最大サイズは 4 Kb です。

Android で下流の FCM メッセージを受信する方法の詳細については、「 [FCM を使用したリモート通知](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md)」を参照してください。

### <a name="topic-messaging"></a>トピックメッセージング

*トピックメッセージング*を使用すると、アプリサーバーは、特定のトピックにオプトインされている複数のデバイスにメッセージを送信できます。 また、焼討 Base コンソール通知 GUI を使用して、トピックメッセージを作成して送信することもできます。 FCM は、サブスクライブされたクライアントへのトピックメッセージのルーティングと配信を処理します。 この機能は、気象アラート、株価情報、ヘッドラインニュースなどのメッセージに使用できます。

[![トピックのメッセージングの図](firebase-cloud-messaging-images/04-topic-messaging-sml.png)](firebase-cloud-messaging-images/04-topic-messaging.png#lightbox)

トピックのメッセージングでは、次の手順を使用します (前に説明したように、クライアントアプリが登録トークンを取得した後)。

1. クライアントアプリは、サブスクライブメッセージを FCM に送信することによってトピックをサブスクライブします。

2. アプリサーバーは、配布用にトピックメッセージを FCM に送信します。

3. FCM は、そのトピックをサブスクライブしているクライアントにトピックメッセージを転送します。

焼討ベーストピックメッセージングの詳細については、Google の[トピック「Android でのメッセージング](https://firebase.google.com/docs/cloud-messaging/android/topic-messaging)」を参照してください。

<a name="setup_fcm" />

## <a name="setting-up-firebase-cloud-messaging"></a>焼討 Base Cloud Messaging の設定

アプリで FCM サービスを使用するに[は、新しい](https://console.firebase.google.com/)プロジェクトを作成する (または既存のプロジェクトをインポートする) 必要があります。 次の手順を使用して、アプリのための焼討 Base Cloud Messaging プロジェクトを作成します。

1. Google アカウント (Gmail のアドレスなど) を使用して、[焼討 Base コンソール](https://console.firebase.google.com/)にサインインし、 **[新しいプロジェクトの作成]** をクリックします。

    [![[新しいプロジェクトの作成] ボタン](firebase-cloud-messaging-images/05-firebase-console-sml.png)](firebase-cloud-messaging-images/05-firebase-console.png#lightbox)

    既存のプロジェクトがある場合は、 **[Google プロジェクトのインポート]** をクリックします。

2. **[プロジェクトの作成]** ダイアログボックスで、プロジェクトの名前を入力し、 **[プロジェクトの作成]** をクリックします。 次の例では、 **XamarinFCM**という名前の新しいプロジェクトが作成されます。

    [![プロジェクトの作成ダイアログ](firebase-cloud-messaging-images/06-create-a-project-sml.png)](firebase-cloud-messaging-images/06-create-a-project.png#lightbox)

3. 焼討 Base コンソールの **[概要]** で、 **[アドインを Android アプリに追加する]** をクリックします。

    [![Android アプリに焼討 Base を追加する](firebase-cloud-messaging-images/07-add-firebase-sml.png)](firebase-cloud-messaging-images/07-add-firebase.png#lightbox)

4. 次の画面で、アプリのパッケージ名を入力します。 この例では、パッケージ名は " **com. xamarin. fcmexample**" です。 この値は、Android アプリのパッケージ名と一致する必要があります。 アプリのニックネームは、**アプリのニックネーム**フィールドにも入力できます。

    [![アプリのニックネームとしての FCM の例の入力](firebase-cloud-messaging-images/08-package-name-sml.png)](firebase-cloud-messaging-images/08-package-name.png#lightbox)

5. アプリで動的リンク、招待、または Google Auth を使用している場合は、デバッグ署名証明書も入力する必要があります。 署名証明書の検索の詳細について[は、「キーストアの MD5 または SHA1 署名の検索](~/android/deploy-test/signing/keystore-signature.md)」を参照してください。
    この例では、署名証明書は空白のままになっています。

6. **[アプリの追加]** をクリックします。

    [![[アプリの追加] ボタンをクリックする](firebase-cloud-messaging-images/09-add-app-sml.png)](firebase-cloud-messaging-images/09-add-app.png#lightbox)

    サーバー API キーとクライアント ID がアプリに対して自動的に生成されます。 この情報は、 **[アプリの追加]** をクリックすると自動的にダウンロードされる**google services の json**ファイルにパッケージ化されます。
    このファイルは必ず安全な場所に保存してください。

Android で FCM プッシュ通知メッセージを受信するためにアプリプロジェクトに**google-** services.msc を追加する方法の詳細な例については、「 [fcm を使用したリモート通知](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md)」を参照してください。

## <a name="for-further-reading"></a>参考資料

- Google の[焼討 Base Cloud messaging](https://firebase.google.com/docs/cloud-messaging/)は、焼討 Base cloud messaging の主要な機能の概要、しくみの説明、およびセットアップ手順を提供します。

- Google の[ビルドアプリサーバーの送信要求](https://firebase.google.com/docs/cloud-messaging/send-message)では、アプリサーバーでメッセージを送信する方法について説明しています。

- [Rfc 6120](https://tools.ietf.org/html/rfc6120)および[rfc 6121](https://tools.ietf.org/html/rfc6121)では、拡張可能なメッセージングとプレゼンスプロトコル (xmpp) について説明し、定義します。

- 「 [FCM メッセージについ](https://firebase.google.com/docs/cloud-messaging/concept-options)て」では、焼討 Base Cloud Messaging で送信できるさまざまな種類のメッセージについて説明しています。

## <a name="summary"></a>まとめ

この記事では、焼討 Base Cloud Messaging (FCM) の概要について説明しました。 ここでは、アプリサーバーとクライアントアプリ間のメッセージングを識別および承認するために使用されるさまざまな資格情報について説明しました。 ここでは、登録とダウンストリームのメッセージングシナリオについて説明し、fcm サービスを使用するようにアプリケーションを FCM に登録する手順について詳しく説明します。

## <a name="related-links"></a>関連リンク

- [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/)
