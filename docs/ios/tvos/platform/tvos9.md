---
title: "TvOS 9 の概要"
description: "この記事では、Xamarin.tvOS 開発者向けのすべての新しいまたは変更された Api と tvOS 9 で使用できる機能を紹介します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: A7E738E1-9F94-489B-918F-7DF8F0810987
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
ms.openlocfilehash: 55e83658e09bc7e5c12bb3ef3f508497651ec46c
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="introduction-to-tvos-9"></a>TvOS 9 の概要

_この記事では、Xamarin.tvOS 開発者向けのすべての新しいまたは変更された Api と tvOS 9 で使用できる機能を紹介します。_


Apple に Apple TV のハードウェア再設計された、タッチを有効にするリモートを機能させると、システムを実行して、新しい tvOS オペレーティング (iOS 9 に基づく) の第 4 世代がリリースされました。

初めて、tvOS ができるようにすると、豊富な実体験のアプリを作成し、書き込みと解放を行う、iTunes アプリを使用した iOS 用アプリのエクスペリエンスと同様のプロセスで Apple TV の組み込みアプリ ストアを介してリリースには開発者に Apple TV のプラットフォームを開きますストア。

Xamarin.iOS 開発に詳しい場合は、非常に単純な tvOS への移行が検索する必要があります。 Api と機能のほとんどは、同じ、ただし、多くの一般的な Api がご利用いただけません (WebKit) などです。 さらに、操作、Siri リモートと課題がいくつかのデザインがタッチ スクリーン ベースの iOS デバイスではありません。

Xamarin.tvOS 開発者向けのこのガイドの概要についてはすべて、新しいまたは変更された Api と tvOS 9 で使用できる機能のようになります。 TvOS の詳細については、Apple を参照してください[新しい Apple tv の開発](https://developer.apple.com/tvos/)ドキュメント。

<a name="Supported-and-Unsupported-Capabilities" />

## <a name="supported-and-unsupported-capabilities"></a>サポートされており、サポートされていない機能

tvOS アプリ Apple TV で実行されているが、次のサポートされている機能と特長。

 - アプリ グループ
 - バックグラウンド モード
 - データの保護
 - Game Center
 - ゲーム コント ローラー
 - iCloud
 - アプリ内購入
 - キーチェーンの共有

次の機能と機能がサポートされていません。

 - Apple Pay
 - アプリのサンド ボックス
 - 関連付けられているドメイン
 - HealthKit
 - HomeKit
 - Inter-App オーディオ
 - マップ
 - 個人の VPN
 - プッシュ通知
 - ウォレット
 - Wireless Accessory Configuration

参照してください、[サポートされているアセンブリ](~/ios/tvos/internals/assemblies.md)と[フレームワークのサポートされている](~/ios/tvos/internals/frameworks.md)詳細についてはドキュメントです。

<a name="Apple-TV-Hardware" />

## <a name="apple-tv-hardware"></a>Apple TV のハードウェア

新しい Apple TV には、次のハードウェアの仕様があります。

 - 64 ビット A8 プロセッサ
 - 32 GB または 64 GB のストレージ
 - 2 GB の RAM
 - 10/100 mbps イーサネット
 - WiFi 802.11a/b/g/n/ac
 - 1080p の解像度
 - HDMI
 - USB C ポート (開発者および診断の使用のみ)
 - 新しい Siri リモートまたは Apple TV リモートの (地域に基づく)

### <a name="siri-remote"></a>Siri リモート

リージョンに基づき、指定された Apple TV リモート今後、いずれか 1 つの構成: Siri リモートまたは Apple TV リモートです。

Siri リモコンは、次の国で現在使用です。

 - オーストラリア
 - カナダ
 - フランス
 - ドイツ
 - 日本
 - スペイン
 - 英国
 - 米国

その他の国、Apple TV リモート Siri ボタンに置き換えるを検索するためのテキスト入力の既定の検索画面を表示する検索ボタンが表示されます。

[![](tvos9-images/remote02.png "Siri リモート")](tvos9-images/remote02.png#lightbox)

詳細についてを参照してください、 [Siri リモート コンピューターと Bluetooth コント ローラー](~/ios/tvos/platform/remote-bluetooth.md)ドキュメント。

<a name="Apple-TV-Provisioning" />

## <a name="apple-tv-provisioning"></a>Apple TV のプロビジョニング

IOS 向けの開発と同じように新しい tvOS は開発と、チームのメンバーシップと署名の Id を既に確立されている Apple での配布の両方の適切なプロビジョニング プロファイルが必要です。

適切なプロビジョニングも iCloud KVS、CloudKit データ ストアなど、tvOS 機能にアクセスするために必要です。 参照してください、[リソースとデータ記憶域](~/ios/tvos/app-fundamentals/resources-data-storage.md)Xamarin.tvOS アプリの iCloud をサポートする方法についてです。

プロビジョニング プロファイルが作成され、Xamarin.iOS アプリでの作業と同じ方法をインストールします。 そのため、iOS を参照してください[デバイスのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)詳細についてはドキュメントです。

<a name="Apple-TV-Apps" />

## <a name="apple-tv-apps"></a>Apple TV Apps

新しい Apple TV のハードウェアと tvOS 9 は、2 種類のアプリをサポートしています: 従来とクライアント サーバー アプリ。

<a name="Traditional-Apps" />

### <a name="traditional-apps"></a>従来のアプリ

従来のアプリでは、Apple TV のアプリ ストアから購入して、デバイス上で直接インストールします。 これらのアプリには、ゲーム、ユーティリティまたは Xamarin.iOS アプリとして、同じフレームワークと手法を使用して開発されたメディア アプリを指定できます。

Apple TV のアプリは、200 MB の最大サイズを持ち、さらに 2 GB のオンデマンド リソースを使用してコンテンツをダウンロードできます。 参照してください、[リソースとデータ記憶域](~/ios/tvos/app-fundamentals/resources-data-storage.md)詳細についてはします。

参照してください、[こんにちは、tvOS クイック スタート ガイド](~/ios/tvos/get-started/hello-tvos.md)ツールと Xamarin.tvOS tvOS アプリを開発するために必要な概念を理解します。

<a name="Summary" />

### <a name="client-server-apps"></a>クライアント サーバー アプリ

Apple TV は従来のインストール済みのアプリだけでなくを簡単に web ベースのクライアントとサーバーのメディア ストリーム配信の (HTTPS、XML、および JavaScript) の web テクノロジを使用してアプリを作成します。 Apple の TVML マークアップ言語を使用してユーザー インターフェイスを設計して TVMLKit を使用して、アプリの動作を定義する JavaScript を使用します。

詳細については、Apple を参照してください[Apple TV のマークアップ言語リファレンス](https://developer.apple.com/library/prerelease/tvos/documentation/LanguagesUtilities/Conceptual/ATV_Template_Guide/index.html#//apple_ref/doc/uid/TP40015064)、 [TVJS フレームワーク参照](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLJS/Reference/TVJSFrameworkReference/index.html#//apple_ref/doc/uid/TP40016076)、 [TVMLKit フレームワーク参照](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLKit/Reference/TVMLKit_Collection/index.html#//apple_ref/doc/uid/TP40016429)、[についてHTTP ライブ ストリーミング](https://developer.apple.com/library/prerelease/tvos/referencelibrary/GettingStarted/AboutHTTPLiveStreaming/about/about.html#//apple_ref/doc/uid/TP40013978)と[Apple TV の HLS オーサリング仕様](https://developer.apple.com/services-account/download?path=/Documentation/HLS_Authoring_Specification_for_Apple_TV/HLS_Authoring_Specification_for_Apple_TV.pdf)ドキュメント。

<a name="User-Interface-Challenges" />

## <a name="user-interface-challenges"></a>ユーザー インターフェイスの課題

IOS または OS X とは異なり、タッチ スクリーンまたは直接を選択し、アプリまたはその内容との対話をユーザーに許可するマウス Apple テレビがありません。 代わりに、ユーザー アプリのユーザー インターフェイスを移動するには、新しい Siri リモートまたは Bluetooth ゲーム コント ローラー。 詳細についてを参照してください、 [Siri リモート コンピューターと Bluetooth コント ローラー](~/ios/tvos/platform/remote-bluetooth.md)ドキュメント。

さらに、全体的なユーザー エクスペリエンスは、iOS や Mac アプリ傾向がある 1 人のユーザー エクスペリエンスをよりも大幅に異なるです。 Apple TV のユーザー エクスペリエンスは、本質的によりソーシャル複数のユーザーを 1 つのアプリと相互のやり取りのソファに座ってする可能性があります場所である傾向があります。 成功した Apple TV アプリ エクスペリエンス (新しいアプリまたは既存の移植) を設計するには、これらの変更は考慮される必要があります。 

<a name="Summary" />

### <a name="working-with-focus-and-parallax-images"></a>フォーカスと視差のイメージを使用します。

前述のように、Xamarin.tvOS アプリのユーザーがいないするインターフェイスとの対話の直接として ios をタップしたいないデバイスの画面上のイメージが直接から Siri リモコンを使用して、部屋の向こう側です。 存在し、このユーザーの操作を処理、Apple TV はフォーカスに基づくモデルを使用します。 

フォーカスが変更された場合、現在フォーカスがあるユーザー インターフェイス項目を明確に識別する繊細なアニメーションと (イメージに視差効果) などの効果が使用されます。

した場合、ユーザーが低速で循環ジェスチャ Siri リモート、フォーカスされた項目がこの移動への応答でリアルタイムかきたてるされます。 発生して、影響を与える、明るい光沢が際立つに表示される画面を作成、イメージに適用されます。 非アクティブな時間が一定時間後に焦点のコンテンツをすべて使用できなくなります、Focused 項目をそれ以上に拡張されます。

詳細についてを参照してください、[ナビゲーションとフォーカス](~/ios/tvos/app-fundamentals/navigation-focus.md)と[アイコンとイメージ処理](~/ios/tvos/app-fundamentals/icons-images.md)ドキュメント。

<a name="The-Home-Screen" />

### <a name="the-home-screen"></a>ホーム画面

Apple TV ホーム画面がインストールされているし、ユーザー設定にアクセスする方法を提供するすべてのアプリを示しています。

[![](tvos9-images/home01.png "ホーム画面")](tvos9-images/home01.png#lightbox)

ユーザーは、フォーカスを使用してアプリを選択し、これを起動する Siri リモート タッチ ジェスチャを使用してアプリのアイコンのグリッドを移動します。 アプリのアイコンは、最初の潜在的なユーザーに最適な印象を与える可能性し、一目でアプリの目的を伝える必要があります。

すべてのアプリには、小さなと、アプリ アイコンのサイズの大きいバージョンの両方を指定する必要があります。 小さいアイコンは、アプリがインストールされているときに、Apple テレビ ホーム画面で使用されます。 大規模なバージョンは、アプリ ストアによって使用されます。 大規模なアプリのアイコンは、小さいアイコンのバージョンのルック アンド フィールを模倣する必要があります。

詳細についてを参照してください、[アイコンとイメージ処理](~/ios/tvos/app-fundamentals/icons-images.md)ドキュメント。

<a name="The-Top-Shelf" />

### <a name="the-top-shelf"></a>上段

ユーザーに、Apple テレビ ホーム画面上の行で Xamarin.tvOS アプリが配置されている場合、アプリがユーザーによって選択されたとき、大規模な上位棚イメージが表示されます。 このイメージは、アプリの機能を強調表示したり、そのコンテンツへの直接のリンクを提供しないでください。

[![](tvos9-images/topshelf01.png "上段")](tvos9-images/topshelf01.png#lightbox)

上部棚のイメージを 1 つの静的として指定するか、できます`.png`または`.lsr`ファイルであるか動的に作成できます実行時にフォーカスを設定できるアイテムの単一の行として。

上部棚に静的な画像を表示するには、代わりに動的な行またはフォーカス可能な項目、スクロールのバナーの動的なセットを格納できます。 アプリまたはその最も使用されている機能へのジャンプで提供されているコンテンツを強調表示をこれらの動的なスタイルの両方ができます。

詳細についてを参照してください、[アイコンとイメージ処理](~/ios/tvos/app-fundamentals/icons-images.md)ドキュメントと Apple の[TVServices フレームワーク参照](https://developer.apple.com/library/prerelease/tvos/documentation/TVServices/Reference/TVServices_Ref/index.html#//apple_ref/doc/uid/TP40016412)上部棚の拡張機能を提供するアプリを追加する方法についての動的な上位棚のコンテンツ。






## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリケーション プログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [Xamarin (ビデオ) を使用した tvOS 用アプリの構築](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
