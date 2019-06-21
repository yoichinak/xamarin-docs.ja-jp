---
title: iOS プラットフォームの機能の概要
description: このドキュメントは、iOS のさまざまなバージョンで導入された機能とその他の iOS プラットフォーム機能を記述するさまざまなガイドにリンクしています。
ms.prod: xamarin
ms.assetid: 9F6A27E5-8A87-ADE2-D1EF-5684E7B8C999
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/25/2018
ms.openlocfilehash: 9628821be5a979777614eb4f7ad8605087093ed3
ms.sourcegitcommit: a153623a69b5cb125f672df8007838afa32e9edf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2019
ms.locfileid: "67268843"
---
# <a name="ios-platform-features-overview"></a>iOS プラットフォームの機能の概要

最近使用した iOS の解放 Apple のフレームワークの一部を強調表示とこのページの一覧に Xamarin.iOS でアクセスできます。

## <a name="ios-releases"></a>iOS リリース

|  |  |
|-------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [iOS 12 の概要](~/ios/platform/introduction-to-ios12/index.md) | このドキュメントは iOS 12 を説明するときに使用できる機能が使用 buildingXamarin.iOS アプリケーション。|
| [iOS 11 の概要](~/ios/platform/introduction-to-ios11/index.md) | このドキュメントでは、iOS 11、および ARKit、Core ML、Core NFC、ドラッグ アンド ドロップ、MapKit、PDFKit、SiriKit、やビジョンなどの Xcode 9 で新規および更新された機能について説明します。 これらの featureswith Xamarin.iOS を使用する方法を説明するガイドにリンクします。 |
| [iOS 10 の概要](~/ios/platform/introduction-to-ios10/index.md) | iOS 10 では、いくつかの新しい Api や新しい機能および機能を備えたアプリを開発するためのサービスが含まれています。 10、iOS では、アプリは、マップ、メッセージ、電話および Siri を拡張するなどの新機能をあります。 このセクションでは、Xamarin.iOS アプリでこれらの機能を利用する方法を示します。 |
| [iOS 9 の概要](~/ios/platform/introduction-to-ios9/index.md)   | このセクションでは、Xamarin.iOS アプリでこれらの機能を使用する方法と iOS 8 からアップグレードする場合は、iOS 9 で行った変更を定義します。 |
| [iOS 8 の概要](~/ios/platform/introduction-to-ios8.md)         | iOS 8 には、iOS 7 からオペレーティング システムに多数の変更が行われます。 ここでは何か、およびその使用方法を紹介します。 |
| [iOS 7 の概要](~/ios/platform/introduction-to-ios7/index.md)   | IOS 7 で導入された主要な新しい Api をに関するなどのビュー コント ローラーへの移行 UIView アニメーション、UIKit Dynamics、およびテキスト キットの機能強化。 |
| [iOS 6 の概要](~/ios/platform/introduction-to-ios6/index.md)   | IOS 6、コレクション ビュー、渡すキット、イベント キット、およびソーシャル フレームワークなどで導入された機能について説明します。 |

## <a name="apple-payiosplatformapple-paymd"></a>[Apple Pay](~/ios/platform/apple-pay.md)

Apple Pay が導入されましたと共に iOS 8、食品、エンターテイメント、自分の iOS デバイスを使用してメンバーシップなどの物理的な商品の支払いにユーザーを有効にします。 IPhone 6 および iPhone 6 では利用また、ストア内での購入の Apple Watch とペアにもなりますが。 IPhone で使用すると、確認し、ユーザーのクレジット_カードまたはデビット カードにトランザクションを承認する方法として Touch ID を使用します。

## <a name="callkitiosplatformcallkitmd"></a>[CallKit](~/ios/platform/callkit.md)

IOS 10 で新しい CallKit API は、VOIP アプリを iPhone UI と統合し使い慣れたインターフェイスを提供し、エンドユーザーに発生するための手段を提供します。 この API を使用したユーザーが表示し、iOS デバイスのロック画面から VOIP 通話との対話および連絡先の Phone アプリの使用を管理**お気に入り**と**も最近使ったもの**ビュー。

## <a name="contacts-and-contactsuiiosplatformcontactsmd"></a>[連絡先と ContactsUI](~/ios/platform/contacts.md)

Apple iOS 9 の導入に伴い、2 つの新しいフレームワークをリリースしました`Contacts`と`ContactsUI`置換時に、既存のアドレス帳、および iOS 8 およびそれ以前でアドレス帳の UI フレームワークを使用します。

## <a name="document-pickeriosplatformdocument-pickermd"></a>[ドキュメント ピッカー](~/ios/platform/document-picker.md)

ドキュメント ピッカーにより、アプリ間で共有するドキュメントです。 ICloud または別のアプリのディレクトリに、これらのドキュメントを格納できます。 ドキュメントのセットを使用して共有[ドキュメント プロバイダーの拡張機能](~/ios/platform/extensions.md)ユーザーが自分のデバイスにインストールします。

## <a name="eventkitiosplatformeventkitmd"></a>[EventKit](~/ios/platform/eventkit.md)

iOS が 2 つのカレンダーに関連のアプリケーション組み込み: カレンダー アプリケーション、およびアラーム アプリケーション。 カレンダー アプリケーションが、予定表のデータを管理する方法を理解するのに十分なと簡単ですが、アラーム アプリケーションはあまり知られていません。 アラームが完了すると、期限になったら、それらの条件に関連する日付を持つことができますなど。IOS が予定表のすべてのデータを格納するカレンダーのイベントまたは通知と呼ばれる 1 つの場所であるかどうかなど、*カレンダー データベース*します。

## <a name="ios-extensionsiosplatformextensionsmd"></a>[iOS の拡張機能](~/ios/platform/extensions.md)

拡張機能、iOS 8 で導入されたは、特化`UIViewControllers`使用される標準コンテキスト内での iOS でなど以内、**通知センター**を実行するユーザーによって要求されたカスタムのキーボードの種類に特化した、入力または他のコンテキストなどの特殊効果のフィルターを拡張機能が提供できる写真を編集します。

## <a name="graphics-and-animation-in-iosiosplatformgraphics-animation-iosindexmd"></a>[IOS のグラフィックスとアニメーション](~/ios/platform/graphics-animation-ios/index.md)

IOS のグラフィックスとアニメーション CoreImage、Core Graphics コア アニメーションなどの iOS の中核となるグラフィックスの概念を説明します。

## <a name="handoffiosplatformhandoffmd"></a>[Handoff](~/ios/platform/handoff.md)

Apple は、同じアプリまたは同じアクティビティをサポートする別のアプリを実行している別のデバイスに iOS 8、およびユーザーが自分のデバイスのいずれかで開始されたアクティビティに転送の一般的なメカニズムを提供する OS X Yosemite (10.10) ハンドオフを導入しました。

## <a name="healthkitiosplatformhealthkitmd"></a>[HealthKit](~/ios/platform/healthkit.md)

正常性のキットでは、正常性関連情報については、ユーザーのセキュリティで保護されたデータ ストアを提供します。 キット アプリの正常性は、ユーザーの明示的なアクセス許可を持つ読み取りおよび書き込みこのデータストアにおよび関連データが追加されたときに通知を受信します。 アプリがデータを表示またはユーザーは、Apple の指定された正常性のアプリを使用して、すべてのデータのダッシュ ボードを表示します。

## <a name="homekitiosplatformhomekitmd"></a>[HomeKit](~/ios/platform/homekit.md)

Apple は iOS 8 を検出して、ユーザーの自宅のホーム オートメーション デバイスと通信するための一般的なフレームワークを提供するで HomeKit を導入しました。 HomeKit では、デバイスを構成して、それらを制御するアクションを設定するための一般的なプラットフォームを提供します。

## <a name="in-app-purchasingiosplatformin-app-purchasingindexmd"></a>[アプリ内購入](~/ios/platform/in-app-purchasing/index.md)

デジタル製品やサービス StoreKit – 自分の Apple id。 を使用するユーザーと財務の取り引きを Apple のサーバーと通信する iOS によって提供される Api のセットを使用して iOS アプリケーションを販売できます。 Storekit や Api は製品情報の取得、トランザクションを実行すると、主な検討事項-ユーザー インターフェイス コンポーネントではありません。 アプリ内購入を実装するアプリケーションは、独自のユーザー インターフェイスを構築し、ユーザーに必要な製品やサービスを提供するカスタム コードでの購入済みの項目を追跡する必要があります。

## <a name="ios-gaming-apisiosplatformgamingindexmd"></a>[iOS ゲーム Api](~/ios/platform/gaming/index.md)

Apple は iOS 9 での Api のゲーム プレイするゲームのグラフィックとオーディオを Xamarin.iOS アプリに実装するより簡単にするいくつかの技術的な強化されています。 高レベルのフレームワークの速度の向上とグラフィック機能、iOS デバイスの GPU の力を活用して開発の両方の容易さが含まれます。

## <a name="message-app-integrationiosplatformmessage-app-integrationindexmd"></a>[メッセージ アプリの統合](~/ios/platform/message-app-integration/index.md)

新しいメッセージ アプリ拡張機能が統合には、10、iOS、**メッセージ**をユーザーに新しい機能がアプリを提示しています。 拡張機能には、テキスト、ステッカー、メディア ファイル、および対話型メッセージを送信できます。

## <a name="multitasking-for-ipadiosplatformmultitaskingmd"></a>[iPad のマルチタスキング](~/ios/platform/multitasking.md)

iOS 9 iPad の特定のハードウェア上で同時に 2 つのアプリを実行するためのマルチタス キングのサポートを追加します。 IPad のマルチタス キングは、次の機能を使用してサポートされています。上のスライド、分割ビューと Picture in Picture。

## <a name="passkitiosplatformpasskitmd"></a>[PassKit](~/ios/platform/passkit.md)

Passbook は Iphone 用のアプリと、iPod は、iOS 6 と接触します。 格納し、バーコードと携帯電話が"real world"の顧客トランザクションをリンクするには、その他の情報が表示されます。 パスの加盟店によって生成され、電子メールでの Url または内から顧客に送信される、商人の iOS アプリ。 Passbook は、格納し、スマート フォンでのすべてのパスを編成し、日付/時刻またはデバイスの場所に応じてロック画面に成功通知が表示されます。

このドキュメントでは、API を使用して渡すキット、Xamarin.iOS を使用して、Passbook を紹介し、サーバー上のパスを実装する方法について説明します。

## <a name="photokitiosplatformphotokitmd"></a>[PhotoKit](~/ios/platform/photokit.md)

フォト キットは、アプリケーションをシステムのイメージのライブラリを照会し、その内容を表示したり、カスタム ユーザー インターフェイスを作成できる新しいフレームワークです。 これには、多数アルバムやフォルダーなどの資産のコレクションだけでなく、イメージおよびビデオ資産を表すクラスにはが含まれます。

## <a name="request-app-reviewiosplatformrequest-app-reviewmd"></a>[アプリ レビューを要求します。](~/ios/platform/request-app-review.md)

Ios 10.3、新しい、`RequestReview()`メソッド iOS アプリに許可することを確認したり評価ユーザーに確認します。 配布アプリケーションでは、ユーザーがアプリ ストアからインストールされているこのメソッドが呼び出されると、iOS 10 が全体の評価を処理し、開発者のプロセスを確認してください。 このプロセスは、アプリ ストアのポリシーによって管理されますが、ため、アラートは可能性があります。 または、表示されない場合があります。

## <a name="search-apisiosplatformsearchindexmd"></a>[API の検索](~/ios/platform/search/index.md)

検索は、iOS 9 で、提供情報と Xamarin.iOS アプリ内の機能にアクセスする新しい方法で拡張されています。 新しいアプリの検索 Api を使用して、アプリのコンテンツは、Siri アラームと提案など、スポット ライトと Safari の検索結果を検索可能なされます。 これにより、アクティビティと、アプリ内で詳細な情報にすばやくアクセスできます。

## <a name="sirikitiosplatformsirikitindexmd"></a>[SiriKit](~/ios/platform/sirikit/index.md)

新しい SiriKit により、アプリ拡張機能と、新しいを使用して iOS デバイスで Siri とマップ アプリを使用してユーザーがアクセスできるサービスを提供する iOS アプリを iOS 10、**インテント**と**Intents UI**フレームワーク。

## <a name="social-frameworkiosplatformsocial-frameworkmd"></a>[ソーシャル フレームワーク](~/ios/platform/social-framework.md)

ソーシャル フレームワークなどのソーシャル ネットワークと対話するための統一された API を提供します_Twitter_と_Facebook_、だけでなく_SinaWeibo_中国でのユーザー。

## <a name="speech-recognitioniosplatformspeechmd"></a>[音声認識](~/ios/platform/speech.md)

iOS 10 には、アプリを継続的な音声認識をサポートし、議事録の作成 (生または録画のオーディオ ストリーム) からの音声をテキストに許可する新しい Speech API が含まれています。

## <a name="textkitiosplatformtextkitmd"></a>[TextKit](~/ios/platform/textkit.md)

テキストのキットは、強力なテキスト レイアウトとレンダリングの機能を提供する新しい API です。 低レベルの主要なテキストのフレームワーク上に構築されますが、使用する主要なテキストよりもはるかに簡単です。

## <a name="3d-touchiosplatform3d-touchmd"></a>[3D Touch](~/ios/platform/3d-touch.md)

この記事では説明し、6 s と iPhone 6 s 新しい iPhone で実行されている Xamarin.iOS アプリに負荷の機密性の高いジェスチャを追加する新しい 3D タッチ Api の使用の概要についてさらにデバイス。

## <a name="touch-idiosplatformtouchidmd"></a>[タッチ ID](~/ios/platform/touchid.md)

Touch ID は、ユーザーのパスコードのような認証の手段として、iOS 7 で導入されました。 ただし、デバイスのロックを解除、App Store を使用して、iTunes を使用して、iCloud キーチェーンのみの認証に制限されていました。

## <a name="user-notificationsiosplatformuser-notificationsindexmd"></a>[ユーザー通知](~/ios/platform/user-notifications/index.md)

新しい ios 10 では、ユーザー通知の配信とローカルとリモート通知の処理のフレームワークを使用します。 このフレームワークを使用して、アプリまたはアプリ拡張機能をスケジュールできますローカル通知の配信の場所などの条件のセットまたは 1 日の時刻を指定することで。

## <a name="wide-coloriosplatformwide-colormd"></a>[広色域](~/ios/platform/wide-color.md)

iOS 10 デバイスと macOS Sierra 拡張範囲のピクセル形式とコア グラフィックス、Core のイメージ、金属製および AVFoundation などのフレームワークを含めて、システム全体で全体の色域スペースのサポートが強化されます。 ワイド カラー ディスプレイを使用したデバイスのサポートはさらに、全体のグラフィックス スタック全体でこの動作を提供することで緩和されました。

## <a name="binding-objective-cbinding-objective-cindexmd"></a>[Objective-C のバインド](binding-objective-c/index.md)

IOS での作業時に、サード パーティ製の Objective C ライブラリを使用する必要がある場合があります。 これらの状況では、ネイティブの OBJECTIVE-C ライブラリを c# バインディングを作成するのに MonoTouch のバインド プロジェクトを使用できます。 プロジェクトが iOS Api を使用する同じツールを使用するC#します。 このドキュメントでは、Objective C Api をバインドする方法について説明します。

## <a name="referencing-native-librariesnative-interopmd"></a>[ネイティブ ライブラリを参照します。](native-interop.md)

Xamarin.iOS では、ネイティブの C ライブラリと OBJECTIVE-C ライブラリの両方とのリンクをサポートしています。 このドキュメントでは、Xamarin.iOS プロジェクト、ネイティブの C ライブラリにリンクする方法について説明します。

## <a name="embedded-frameworksembedded-frameworksmd"></a>[埋め込みフレームワーク](embedded-frameworks.md)

Objective C のユーザーのフレームワークを Xamarin.iOS アプリに埋め込む方法について説明します。
