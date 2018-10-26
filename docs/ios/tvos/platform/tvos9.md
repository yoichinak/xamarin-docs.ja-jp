---
title: TvOS 9 の概要
description: この記事では、Xamarin.tvOS 開発者向けのすべての新規および変更した Api と tvOS 9 で使用できる機能を紹介します。
ms.prod: xamarin
ms.assetid: A7E738E1-9F94-489B-918F-7DF8F0810987
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/07/2016
ms.openlocfilehash: dda197f71b2a2ab3e0d61a838ab85d79b7a078c7
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104311"
---
# <a name="introduction-to-tvos-9"></a>TvOS 9 の概要

_この記事では、Xamarin.tvOS 開発者向けのすべての新規および変更した Api と tvOS 9 で使用できる機能を紹介します。_

Apple では、再設計された、タッチ対応のリモートを備えた、新しい tvOS オペレーティング システム (iOS 9 に基づく) を実行して、Apple TV ハードウェアの第 4 世代がリリースされました。

初めて tvOS が開き、Apple TV プラットフォームは、開発者は、多彩で魅力的なアプリを作成し、記述と iTunes アプリを使用した iOS 用アプリのリリースのエクスペリエンスと同様のプロセスで Apple TV の組み込みアプリ ストアからそれらをリリースすることができます。ストア。

Xamarin.iOS の開発に詳しい場合は、非常に単純な tvOS への移行が検索する必要があります。 Api と機能のほとんどは、同じ、ただし、多くの一般的な Api を利用できません (WebKit) など。 さらに、操作、Siri のリモートではタッチ スクリーン ベースの iOS デバイスに存在しないいくつかのデザインの課題をもたらします。

このガイドは Xamarin.tvOS 開発者向けのすべての新規および変更した Api と tvOS 9 で使用できる機能の概要を提供します。 TvOS の詳細については、Apple を参照してください[新しい Apple TV 用の開発](https://developer.apple.com/tvos/)ドキュメント。

<a name="Supported-and-Unsupported-Capabilities" />

## <a name="supported-and-unsupported-capabilities"></a>サポートされているとサポートされていない機能

tvOS アプリの Apple TV で実行されているが、次のサポート機能と機能。

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

新しい Apple TV では、次のハードウェアの仕様があります。

 - 64 ビットの A8 プロセッサ
 - 32 GB または 64 GB のストレージ
 - 2 GB の RAM
 - 10/100 mbps イーサネット
 - WiFi 802.11a/b/g/n/ac
 - 1080p の解像度
 - HDMI
 - (開発者と診断の使用のみ) 用の USB C ポート
 - 新しい Siri のリモートまたは Apple TV リモートの (リージョンに基づく)

### <a name="siri-remote"></a>Siri のリモート

リージョンに基づき、指定された Apple TV リモートになるいずれか 1 つの構成: Siri のリモートまたは Apple TV リモートです。

Siri のリモートは、次の国で現在使用できません。

 - オーストラリア
 - カナダ
 - フランス
 - ドイツ
 - 日本
 - スペイン
 - 英国
 - 米国

その他すべての国、Apple TV リモート Siri ボタンに置き換えるを検索するためのテキスト入力の既定の検索画面を表示する検索 ボタンが表示されます。

[![](tvos9-images/remote02.png "Siri のリモート")](tvos9-images/remote02.png#lightbox)

詳細についてを参照してください、 [Siri のリモートと Bluetooth コント ローラー](~/ios/tvos/platform/remote-bluetooth.md)ドキュメント。

<a name="Apple-TV-Provisioning" />

## <a name="apple-tv-provisioning"></a>Apple TV のプロビジョニング

IOS 向けの開発と同じように新しい tvOS は開発と配布は、チームのメンバーシップと署名 Id を既に確立されている Apple との両方の適切なプロビジョニング プロファイルが必要です。

適切なプロビジョニングは、iCloud KVS または CloudKit データ ストアなどの tvOS 機能にアクセスするために必要なはもです。 参照してください、[リソースとデータの記憶域](~/ios/tvos/app-fundamentals/resources-data-storage.md)Xamarin.tvOS アプリで iCloud をサポートする方法について。

プロビジョニング プロファイルが作成され、Xamarin.iOS アプリでの作業と同じ方法をインストールします。 そのため、iOS を参照してください[Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md)詳細についてはドキュメントです。

<a name="Apple-TV-Apps" />

## <a name="apple-tv-apps"></a>Apple TV Apps

新しい Apple TV のハードウェアと tvOS 9 は、2 種類のアプリをサポートしています。 従来とクライアント サーバー アプリ。

<a name="Traditional-Apps" />

### <a name="traditional-apps"></a>従来のアプリ

従来のアプリでは、Apple TV App Store から購入したが、デバイス上で直接インストールされます。 これらのアプリには、ゲーム、ユーティリティ、または同じフレームワークと手法を使用して Xamarin.iOS アプリとして開発されたメディア アプリができます。

Apple TV のアプリは、200 MB の最大サイズを持ち、さらに 2 GB のオンデマンド リソースを使用してコンテンツをダウンロードできます。 参照してください、[リソースとデータの記憶域](~/ios/tvos/app-fundamentals/resources-data-storage.md)詳細についてはします。

参照してください、[はじめての tvOS クイック スタート ガイド](~/ios/tvos/get-started/hello-tvos.md)ツールと Xamarin.tvOS を使用して、tvOS アプリを開発するために必要な概念を理解します。

<a name="Summary" />

### <a name="client-server-apps"></a>クライアント サーバー アプリ

Apple TV はインストールされている従来のアプリだけでなく、簡単に web ベースのクライアントとサーバーのメディア ストリーミングの web テクノロジ (HTTPS、XML、および JavaScript) を使用してアプリを作成できます。 Apple の TVML マークアップ言語を使用してユーザー インターフェイスを設計して TVMLKit を使用して、アプリの動作を定義する JavaScript を使用します。

詳細については、Apple を参照してください[Apple TV のマークアップ言語リファレンス](https://developer.apple.com/library/prerelease/tvos/documentation/LanguagesUtilities/Conceptual/ATV_Template_Guide/index.html#//apple_ref/doc/uid/TP40015064)、 [TVJS フレームワーク参照](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLJS/Reference/TVJSFrameworkReference/index.html#//apple_ref/doc/uid/TP40016076)、 [TVMLKit フレームワーク参照](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLKit/Reference/TVMLKit_Collection/index.html#//apple_ref/doc/uid/TP40016429)、[についてHTTP ライブ ストリーミング](https://developer.apple.com/library/prerelease/tvos/referencelibrary/GettingStarted/AboutHTTPLiveStreaming/about/about.html#//apple_ref/doc/uid/TP40013978)と[Apple TV の HLS オーサリング仕様](https://developer.apple.com/services-account/download?path=/Documentation/HLS_Authoring_Specification_for_Apple_TV/HLS_Authoring_Specification_for_Apple_TV.pdf)ドキュメント。

<a name="User-Interface-Challenges" />

## <a name="user-interface-challenges"></a>ユーザー インターフェイスの課題

IOS または OS X とは異なり、Apple TV がタッチまたはマウスを直接選択して、アプリまたはそのコンテンツとの対話ユーザーに許可します。 代わりに、ユーザー、アプリのユーザー インターフェイスを操作するには、新しい Siri のリモートまたは Bluetooth ゲーム コント ローラー。 詳細についてを参照してください、 [Siri のリモートと Bluetooth コント ローラー](~/ios/tvos/platform/remote-bluetooth.md)ドキュメント。

さらに、全体的なユーザー エクスペリエンスが iOS または Mac アプリ 1 人のユーザー エクスペリエンスにする傾向があるよりも大幅に異なるです。 Apple TV では、ユーザー エクスペリエンスを複数のユーザーを 1 つのアプリと相互の対話のソファに座ってする可能性があります、その他のソーシャル、本質的にする傾向があります。 (新しいアプリまたは既存の移植)、正常な Apple TV アプリ エクスペリエンスを設計するには、これらの変更を考慮に実行する必要があります。 

<a name="Summary" />

### <a name="working-with-focus-and-parallax-images"></a>フォーカスと視差のイメージを使用します。

前述のように、Xamarin.tvOS アプリのユーザーはインターフェイスを直接として iOS の場所をタップしたいないデバイスの画面で、イメージが直接から Siri リモコンを使用して部屋の対話できません。 提示し、このユーザーの操作を処理、Apple TV はフォーカス ベースのモデルを使用します。 

フォーカスの変更として繊細なアニメーションと (イメージの視差効果) などの効果は、現在フォーカスがあるユーザー インターフェイス項目を明確に識別するために使用されます。

ユーザーが低速、循環ジェスチャを行った Siri のリモート、重点を置いた項目がこの移動への応答でリアルタイム sway されます。 Sway が発生すると、明るい光沢が真価を発揮する表示画面を作成、イメージに適用されます。 一定の非アクティブですが後の焦点のずれたコンテンツが淡色表示し、Focused 項目がさらに大きく成長します。

詳細についてを参照してください、[ナビゲーションとフォーカス](~/ios/tvos/app-fundamentals/navigation-focus.md)と[アイコンとイメージを使用する](~/ios/tvos/app-fundamentals/icons-images.md)ドキュメント。

<a name="The-Home-Screen" />

### <a name="the-home-screen"></a>ホーム画面

Apple TV ホーム画面は、ユーザー設定にアクセスする手段を提供します。 インストールされているすべてのアプリを示しています。

[![](tvos9-images/home01.png "ホーム画面")](tvos9-images/home01.png#lightbox)

ユーザーは、フォーカスを使用して、アプリを選択し、これを起動する Siri のリモートでタッチ ジェスチャを使用してアプリ アイコンのグリッドを移動します。 アプリ アイコンは、最初の潜在的なユーザーに優れた印象を与える可能性し、ひとめでアプリの目的を伝える必要があります。

すべてのアプリには、小さいと、アプリ アイコンの大きなバージョンの両方を指定する必要があります。 アプリがインストールされている場合、Apple TV ホーム画面の小さいアイコンが使用されます。 App Store では、大きなバージョンを使用します。 大きなアプリ アイコンには、小さいアイコンのバージョンのルック アンド フィールを模倣する必要があります。

詳細についてを参照してください、[アイコンとイメージを使用する](~/ios/tvos/app-fundamentals/icons-images.md)ドキュメント。

<a name="The-Top-Shelf" />

### <a name="the-top-shelf"></a>トップ シェルフ

ユーザーは、Apple TV ホーム画面の上部の行に Xamarin.tvOS アプリを配置するが場合、は、アプリがユーザーによって選択されたときに大きなトップ シェルフのイメージが表示されます。 このイメージは、アプリの機能を強調表示したり、そのコンテンツへの直接リンクを提供しないでください。

[![](tvos9-images/topshelf01.png "トップ シェルフ")](tvos9-images/topshelf01.png#lightbox)

トップ シェルフのイメージを単一の静的なを指定できますか。`.png`または`.lsr`ファイル、または動的に作成できます実行時にフォーカスを設定できる項目の 1 つの行として。

静的トップ シェルフのイメージを表示するには、代わりに動的な行またはフォーカスを設定できる項目またはバナーのスクロールの動的なセットを格納できます。 これらの動的なスタイルの両方で、アプリや最も使用されているその機能へのジャンプが提供するコンテンツを強調表示できます。

詳細についてを参照してください、[アイコンとイメージを使用する](~/ios/tvos/app-fundamentals/icons-images.md)ドキュメントと Apple の[TVServices フレームワーク参照](https://developer.apple.com/library/prerelease/tvos/documentation/TVServices/Reference/TVServices_Ref/index.html#//apple_ref/doc/uid/TP40016412)上部棚の拡張機能を提供するアプリを追加する方法の詳細についてトップ シェルフの動的なコンテンツ。






## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 用のアプリのプログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [(ビデオ) Xamarin で tvOS のアプリの構築](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
