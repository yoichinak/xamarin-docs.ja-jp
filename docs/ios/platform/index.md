---
title: iOS プラットフォームの機能の概要
description: このドキュメントでは、さまざまなバージョンの iOS とその他の iOS プラットフォーム機能に導入された機能について説明するさまざまなガイドにリンクしています。
ms.prod: xamarin
ms.assetid: 9F6A27E5-8A87-ADE2-D1EF-5684E7B8C999
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/25/2018
ms.openlocfilehash: a70095c5897163d3699d03ec42c33826f6d40419
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997151"
---
# <a name="ios-platform-features-overview"></a>iOS プラットフォームの機能の概要

このページには、最近の iOS リリースと、Xamarin iOS でアクセスできる Apple フレームワークの一部が強調表示されています。

## <a name="ios-releases"></a>iOS リリース

| Release | 説明 |
|-------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [iOS 13 の概要](~/ios/platform/ios13/index.md) | このドキュメントでは、Xamarin. iOS 13 について説明します。|
| [iOS 12 の概要](~/ios/platform/introduction-to-ios12/index.md) | このドキュメントでは、Xamarin iOS アプリケーションをビルドするときに使用できる iOS 12 の機能について説明します。|
| [iOS 11 の概要](~/ios/platform/introduction-to-ios11/index.md) | このドキュメントでは、ARKit、Core ML、コア NFC、ドラッグアンドドロップ、MapKit、PDFKit、SiriKit、ビジョンなど、iOS 11 および Xcode 9 の新機能と更新された機能について説明します。 これらの機能を Xamarin で使用する方法について説明しているガイドにリンクしています。 |
| [iOS 10 の概要](~/ios/platform/introduction-to-ios10/index.md) | iOS 10 には、新しい機能を備えたアプリの開発を可能にする新しい Api とサービスがいくつか含まれています。 IOS 10 では、マップ、メッセージ、電話、Siri の拡張などの新機能がアプリに搭載されています。 このセクションでは、他社アプリでこれらの機能を利用する方法について説明します。 |
| [iOS 9 の概要](~/ios/platform/introduction-to-ios9/index.md)   | このセクションでは、ios 8 からアップグレードするときに iOS 9 で加えられた変更と、Xamarin iOS アプリでこれらの機能を使用する方法を定義します。 |
| [iOS 8 の概要](~/ios/platform/introduction-to-ios8.md)         | iOS 8 では、iOS 7 からオペレーティングシステムに多数の変更を加えました。 ここでは、それらの内容とその使用方法を示します。 |
| [iOS 7 の概要](~/ios/platform/introduction-to-ios7/index.md)   | IOS 7 で導入された主要な新しい Api について説明します。これには、ビューコントローラーの遷移、UIView アニメーションの機能強化、Uiview Dynamics、テキストキットが含まれます。 |
| [iOS 6 の概要](~/ios/platform/introduction-to-ios6/index.md)   | コレクションビュー、パススルーキット、イベントキット、ソーシャルフレームワークなど、iOS 6 で導入された機能について説明します。 |

## <a name="apple-pay"></a>[Apple Pay](~/ios/platform/apple-pay.md)

Apple Pay は、iOS 8 と共に導入され、ユーザーは iOS デバイスを介して食品、エンターテイメント、メンバーシップなどの物理的な商品を支払うことができます。 IPhone 6 および iPhone 6 Plus で利用できます。また、ストア内購入の Apple Watch と組み合わせて使用することもできます。 IPhone で使用する場合、ユーザーのクレジットカードまたはデビットカードに対するトランザクションを確認して承認する手段として、Touch ID を使用します。

## <a name="callkit"></a>[CallKit](~/ios/platform/callkit.md)

IOS 10 の新しい CallKit API は、VOIP アプリを iPhone UI と統合し、使い慣れたインターフェイスとエクスペリエンスをエンドユーザーに提供するための手段を提供します。 この API を使用すると、ユーザーは iOS デバイスのロック画面から VOIP 通話を表示して操作したり、電話アプリの [**お気に入り**] ビューと [**受信者**] ビューを使用して連絡先を管理したりできます。

## <a name="contacts-and-contactsui"></a>[連絡先と ContactsUI](~/ios/platform/contacts.md)

IOS 9 の導入により、Apple は2つの新しいフレームワークをリリースしました。これは、 `Contacts` `ContactsUI` ios 8 以前で使用されていた既存のアドレス帳とアドレス帳の UI フレームワークを置き換えるものです。

## <a name="document-picker"></a>[ドキュメント ピッカー](~/ios/platform/document-picker.md)

ドキュメントピッカーでは、アプリ間でドキュメントを共有できます。 これらのドキュメントは iCloud または別のアプリのディレクトリに格納されている場合があります。 ドキュメントは、ユーザーがデバイスにインストールした[ドキュメントプロバイダーの拡張機能](~/ios/platform/extensions.md)のセットを介して共有されます。

## <a name="eventkit"></a>[EventKit](~/ios/platform/eventkit.md)

iOS には、カレンダーアプリケーションとアラームアプリケーションという2つのカレンダー関連アプリケーションが組み込まれています。 カレンダーアプリケーションがカレンダーデータをどのように管理するかを理解するのは簡単ですが、リマインダーアプリケーションはあまり明確ではありません。 アラームには、実際に期限切れになったとき、完了したときなどに、日付を関連付けることができます。そのため、iOS では、予定表のイベントかリマインダーかにかかわらず、カレンダー*データベース*と呼ばれるすべてのカレンダーデータが1か所に格納されます。

## <a name="ios-extensions"></a>[iOS の拡張機能](~/ios/platform/extensions.md)

IOS 8 で導入された拡張機能は、 `UIViewControllers` ios によって、**通知センター**内などの標準コンテキスト内で、特殊な入力を実行するためにユーザーによって要求されたカスタムキーボードの種類、または拡張機能が特殊効果フィルターを提供できる画像の編集などの他のコンテキストに特化したものです。

## <a name="graphics-and-animation-in-ios"></a>[iOS のグラフィックスとアニメーション](~/ios/platform/graphics-animation-ios/index.md)

IOS のグラフィックスとアニメーションでは、CoreImage、コアグラフィックス、コアアニメーションなど、iOS の主要なグラフィックスの概念が説明されています。

## <a name="handoff"></a>[ハンドオフ](~/ios/platform/handoff.md)

Apple では、iOS 8 および OS X ヨーク Semite (10.10) にハンドオフが導入されました。ユーザーは、デバイスの1つで開始されたアクティビティを、同じアプリを実行している別のデバイス、または同じアクティビティをサポートする別のデバイスに転送するための一般的なメカニズムを提供します。

## <a name="healthkit"></a>[HealthKit](~/ios/platform/healthkit.md)

Health Kit は、ユーザーの正常性に関連する情報のセキュリティで保護されたデータストアを提供します。 正常性キットアプリは、ユーザーの明示的なアクセス許可、このデータストアへの読み取りと書き込み、関連データが追加されたときに通知を受け取ることができます。 アプリはデータを提示することができます。また、ユーザーは Apple が提供する正常性アプリを使用して、すべてのデータのダッシュボードを表示できます。

## <a name="homekit"></a>[HomeKit](~/ios/platform/homekit.md)

Apple では、ユーザーの自宅でホームオートメーションデバイスを検出して通信するための共通のフレームワークを提供するために、iOS 8 のホームキットを導入しました。 ホームキットは、デバイスを構成し、それらを制御するアクションを設定するための共通プラットフォームを提供します。

## <a name="in-app-purchasing"></a>[アプリ内購入](~/ios/platform/in-app-purchasing/index.md)

iOS アプリケーションでは、StoreKit を使用してデジタル製品またはサービスを販売できます。 iOS が提供する一連の Api は、Apple のサーバーと通信して、ユーザーが Apple ID を介して財務トランザクションを実行します。 StoreKit Api は、主に製品情報の取得とトランザクションの実行に関係しています。ユーザーインターフェイスコンポーネントはありません。 アプリ内購入を実装するアプリケーションは、独自のユーザーインターフェイスを構築し、ユーザーに必要な製品またはサービスを提供するカスタムコードを使用して購入した項目を追跡する必要があります。

## <a name="ios-gaming-apis"></a>[iOS ゲーム Api](~/ios/platform/gaming/index.md)

Apple は、iOS 9 のゲーム Api に技術的にいくつかの機能強化を行っています。これにより、Xamarin iOS アプリでゲームグラフィックスとオーディオを簡単に実装できるようになりました。 これには、高レベルのフレームワークを使用した簡単な開発と、iOS デバイスの GPU 機能を活用して速度とグラフィック能力を向上させることが含まれます。

## <a name="message-app-integration"></a>[メッセージアプリの統合](~/ios/platform/message-app-integration/index.md)

IOS 10 を初めて使用する場合、メッセージアプリ拡張機能は**Messages**アプリと統合され、ユーザーに新しい機能を提供します。 拡張機能は、テキスト、ステッカー、メディアファイル、および対話型メッセージを送信できます。

## <a name="multitasking-for-ipad"></a>[iPad のマルチタスキング](~/ios/platform/multitasking.md)

iOS 9 では、特定の iPad ハードウェアで同時に2つのアプリを実行するためのマルチタスキングサポートが追加されています。 IPad のマルチタスキングは、次の機能を使用してサポートされています。スライド上、分割ビュー & 画像の画像です。

## <a name="passkit"></a>[PassKit](~/ios/platform/passkit.md)

Pass book は、iphone のアプリであり、iOS 6 を使用します。 電話の顧客トランザクションを ' real world ' とリンクするために、バーコードやその他の情報を保存して表示します。 パスは、商人によって生成され、電子メール、Url、またはマーチャントの iOS アプリ内から顧客に送信されます。 電話のすべてのパスを保管して整理し、日付/時刻またはデバイスの場所に応じて、ロック画面に通知を表示します。

このドキュメントでは、Pass Kit API と Xamarin を使用したパススルーブックについて説明し、サーバーでパスを実装する方法について説明します。

## <a name="photokit"></a>[PhotoKit](~/ios/platform/photokit.md)

Photo Kit は、アプリケーションがシステムイメージライブラリに対してクエリを実行し、その内容を表示および変更するためのカスタムユーザーインターフェイスを作成できるようにする新しいフレームワークです。 これには、イメージとビデオ資産を表す多数のクラスと、アルバムやフォルダーなどの資産のコレクションが含まれます。

## <a name="request-app-review"></a>[アプリレビューの要求](~/ios/platform/request-app-review.md)

IOS 10.3 を初めて使用する場合、メソッドを使用すると、 `RequestReview()` ios アプリでユーザーに評価または確認を求めることができます。 ユーザーが App Store からインストールした出荷アプリでこのメソッドが呼び出されると、iOS 10 は開発者の評価およびレビュープロセス全体を処理します。 このプロセスは App Store ポリシーによって管理されているため、アラートが表示されない場合があります。

## <a name="search-apis"></a>[Search API](~/ios/platform/search/index.md)

IOS 9 では Search が拡張されており、Xamarin iOS アプリ内の情報や機能にアクセスするための新しい方法を提供しています。 新しいアプリ検索 Api を使用すると、アプリのコンテンツはスポットライトと Safari の検索結果、ハンドオフおよび Siri のリマインダーと提案によって検索可能になります。 これにより、ユーザーはアプリ内のアクティビティや情報にすばやくアクセスできます。

## <a name="sirikit"></a>[SiriKit](~/ios/platform/sirikit/index.md)

IOS 10 の新機能である SiriKit を使用すると、ios アプリは、アプリ拡張機能と新しい**インテント**および**インテント UI**フレームワークを使用して、Ios デバイスで siri と Maps アプリを使用して、ユーザーがアクセスできるサービスを提供できます。

## <a name="social-framework"></a>[ソーシャルフレームワーク](~/ios/platform/social-framework.md)

ソーシャルフレームワークは、 _Twitter_や_Facebook_などのソーシャルネットワークや中国のユーザー向けの_sinaweibo_と対話するための統一された API を提供します。

## <a name="speech-recognition"></a>[音声認識](~/ios/platform/speech.md)

iOS 10 には新しい Speech API が含まれています。これにより、アプリは、音声の音声認識と議事録 (ライブまたは録音されたオーディオストリーム) をテキストにすることができます。

## <a name="textkit"></a>[TextKit](~/ios/platform/textkit.md)

テキストキットは、強力なテキストレイアウトとレンダリング機能を提供する新しい API です。 これは、低レベルのコアテキストフレームワークの上に構築されていますが、コアテキストよりもはるかに使いやすくなっています。

## <a name="3d-touch"></a>[3D Touch](~/ios/platform/3d-touch.md)

この記事では、新しい 3D Touch Api を使用して、Xamarin に負荷の高いジェスチャを追加する方法について説明します。新しい iPhone 6s と iPhone 6s Plus デバイスで実行されている iOS アプリ。

## <a name="touch-id-and-face-id-with-xamarinios"></a>[Xamarin でタッチ ID と顔 ID を使用する](~/ios/platform/touch-id-face-id.md)

タッチ ID と顔 ID は、iOS 8 以降で使用できる生体認証システムです。 この記事とサンプルでは、Xamarin iOS で Touch ID と Face ID を使用する方法について説明します。

## <a name="user-notifications"></a>[ユーザーへの通知](~/ios/platform/user-notifications/index.md)

IOS 10 の新機能であるユーザー通知フレームワークを使用すると、ローカルおよびリモートの通知を配信および処理することができます。 このフレームワークを使用すると、アプリまたはアプリ拡張機能は、場所や時間などの条件のセットを指定することによって、ローカル通知の配信をスケジュールすることができます。

## <a name="wide-color"></a>[広色域](~/ios/platform/wide-color.md)

iOS 10 と macOS Sierra は、コアグラフィックス、コアイメージ、メタル、AVFoundation などのフレームワークを含む、システム全体にわたる拡張範囲のピクセル形式と広い範囲の色空間のサポートを強化します。 グラフィックススタック全体でこの動作を提供することにより、さまざまな色で表示されるデバイスのサポートがさらに緩和さます。

## <a name="binding-objective-c"></a>[Objective-C のバインディング](binding-objective-c/index.md)

IOS で作業している場合、サードパーティの目標 C ライブラリを使用することが必要になる場合があります。 このような状況では、Monotouch.dialog のバインドプロジェクトを使用して、ネイティブの目的 C ライブラリへの C# バインドを作成できます。 このプロジェクトでは、iOS Api を C# に持ち込むために使用するのと同じツールを使用します。 このドキュメントでは、目標 C Api をバインドする方法について説明します。

## <a name="bind-ios-swift-libraries"></a>[iOS Swift ライブラリをバインドする](binding-swift/index.md)

このドキュメントでは、コードを Swift にする C# バインディングを作成する方法について説明します。これにより、ネイティブライブラリを使用したり、Xamarin. iOS アプリケーションで併置したりすることができます。

## <a name="referencing-native-libraries"></a>[参照 (ネイティブライブラリを)](native-interop.md)

Xamarin. iOS は、ネイティブ C ライブラリと目的 C ライブラリの両方を使用したリンクをサポートします。 このドキュメントでは、ネイティブ C ライブラリを Xamarin. iOS プロジェクトにリンクする方法について説明します。

## <a name="embedded-frameworks"></a>[埋め込みフレームワーク](embedded-frameworks.md)

Xamarin iOS アプリに、目的の C ユーザーフレームワークを埋め込む方法について説明します。
