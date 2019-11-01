---
title: tvOS 9 の概要
description: この記事では、tvOS 9 for Xamarin. tvOS 開発者向けの新しい Api と変更された Api と機能をすべて紹介します。
ms.prod: xamarin
ms.assetid: A7E738E1-9F94-489B-918F-7DF8F0810987
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/07/2016
ms.openlocfilehash: 34f332eb712f479f9f9565a3894212e3cdd5aaf6
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030546"
---
# <a name="introduction-to-tvos-9"></a>tvOS 9 の概要

_この記事では、tvOS 9 for Xamarin. tvOS 開発者向けの新しい Api と変更された Api と機能をすべて紹介します。_

Apple は、新しい tvOS オペレーティングシステム (iOS 9 に基づく) を実行して、再設計されたタッチ対応のリモート機能を使用して、Apple TV ハードウェアの第4世代をリリースしました。

TvOS は、初めて Apple TV platform を開発者にオープンします。これにより、iTunes アプリを使用して iOS 用アプリを作成してリリースするのと同様のプロセスで、Apple TV の組み込みアプリストアを使用して、豊富なイマーシブアプリを作成し、それらをリリースすることができます。ソース.

Xamarin の開発に慣れている場合は、tvOS が非常に単純なものに移行することをお勧めします。 Api と機能のほとんどは同じですが、多くの一般的な Api は使用できません (WebKit など)。 さらに、Siri リモコンでを使用すると、タッチスクリーンベースの iOS デバイスには存在しない設計上の課題が生じます。

このガイドでは、tvOS 9 for Xamarin. tvOS 開発者向けの新しい Api と変更された Api と機能をすべて紹介します。 TvOS の詳細については、apple の[開発に関する新しい APPLE TV のドキュメントを](https://developer.apple.com/tvos/)参照してください。

<a name="Supported-and-Unsupported-Capabilities" />

## <a name="supported-and-unsupported-capabilities"></a>サポートされる機能とサポートされない機能

Apple TV で実行される tvOS アプリには、次のサポートされている機能と機能があります。

- アプリ グループ
- バックグラウンド モード
- データの保護
- Game Center
- ゲームコントローラー
- iCloud
- アプリ内購入
- キーチェーンの共有

次の機能はサポートされていません。

- Apple Pay
- アプリサンドボックス
- 関連付けられているドメイン
- HealthKit
- HomeKit
- Inter-App オーディオ
- マップ
- 個人の VPN
- プッシュ通知
- ウォレット
- Wireless Accessory Configuration

詳細については、[サポートされ](~/ios/tvos/internals/assemblies.md)ているアセンブリと[サポートされているフレームワーク](~/ios/tvos/internals/frameworks.md)に関するドキュメントを参照してください。

<a name="Apple-TV-Hardware" />

## <a name="apple-tv-hardware"></a>Apple TV ハードウェア

新しい Apple TV には、次のハードウェア仕様があります。

- 64ビット A8 プロセッサ
- 32 GB または 64 GB の記憶域
- 2 GB の RAM
- 10/100 Mbps イーサネット
- WiFi 802.11 a/b/g/n/ac
- 1080p 解像度
- HDMI
- USB C ポート (開発者および診断用にのみ使用)
- 新しい Siri リモートまたは Apple TV リモコン (地域に基づく)

### <a name="siri-remote"></a>Siri リモート

リージョンに基づいて、指定された Apple TV リモコンは、Siri リモートまたは Apple TV リモコンのいずれか1つの構成になります。

Siri リモートは、現在、次の国でご利用いただけます。

- オーストラリア
- カナダ
- フランス
- ドイツ
- 日本
- スペイン
- 英国
- 米国

他のすべての国は、Siri ボタンを検索ボタンに置き換える Apple TV リモコンを受け取ります。このボタンをクリックすると、既定の検索画面に検索用のテキスト入力が表示されます。

[![](tvos9-images/remote02.png "Siri Remote")](tvos9-images/remote02.png#lightbox)

詳細については、 [Siri リモートおよび Bluetooth コントローラー](~/ios/tvos/platform/remote-bluetooth.md)のドキュメントを参照してください。

<a name="Apple-TV-Provisioning" />

## <a name="apple-tv-provisioning"></a>Apple TV のプロビジョニング

IOS 用の開発と同様に、新しい tvOS では、Apple で既に確立したチームメンバーシップと署名 Id に基づいて、開発と配布の両方に適したプロビジョニングプロファイルが必要になります。

ICloud KVS や CloudKit データストアなどの tvOS 機能にアクセスするには、適切なプロビジョニングも必要です。 TvOS アプリで iCloud をサポートする方法の詳細については、[リソースとデータストレージ](~/ios/tvos/app-fundamentals/resources-data-storage.md)に関する記事をご覧ください。

プロビジョニングプロファイルは、Xamarin iOS アプリを使用する場合と同じ方法で作成およびインストールされます。 そのため、詳細については、iOS[デバイスのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)に関するドキュメントを参照してください。

<a name="Apple-TV-Apps" />

## <a name="apple-tv-apps"></a>Apple TV アプリ

新しい Apple TV ハードウェアと tvOS 9 は、従来のアプリとクライアント/サーバーアプリの2種類のアプリをサポートしています。

<a name="Traditional-Apps" />

### <a name="traditional-apps"></a>従来のアプリ

従来のアプリは Apple TV App Store から購入され、デバイスに直接インストールされます。 これらのアプリは、Xamarin iOS アプリと同じフレームワークと手法を使用して開発されたゲーム、ユーティリティ、またはメディアアプリにすることができます。

Apple TV アプリの最大サイズは 200 MB で、オンデマンドリソースを使用してさらに 2 GB のコンテンツをダウンロードできます。 詳細については、microsoft の[リソースとデータストレージ](~/ios/tvos/app-fundamentals/resources-data-storage.md)を参照してください。

TvOS を使用して tvOS アプリを開発するために必要なツールと概念について理解を深めるには、「 [Hello, tvOS クイックスタートガイド](~/ios/tvos/get-started/hello-tvos.md)」を参照してください。

<a name="Summary" />

### <a name="client-server-apps"></a>クライアント/サーバーアプリ

Apple TV では、インストールされている従来のアプリに加えて、web テクノロジ (HTTPS、XML、JavaScript) を使用した web ベースのクライアントサーバーメディアストリーミングアプリの作成が簡単になります。 Apple の TVML マークアップ言語を使用してユーザーインターフェイスをデザインし、JavaScript を使用して TVMLKit を使用してアプリの動作を定義します。

詳細については、apple の apple [Tv Markup Language リファレンス](https://developer.apple.com/library/prerelease/tvos/documentation/LanguagesUtilities/Conceptual/ATV_Template_Guide/index.html#//apple_ref/doc/uid/TP40015064)、 [TVJS framework](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLJS/Reference/TVJSFrameworkReference/index.html#//apple_ref/doc/uid/TP40016076)リファレンス、 [TVMLKIT framework リファレンス](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLKit/Reference/TVMLKit_Collection/index.html#//apple_ref/doc/uid/TP40016429)、 [apple Tv の HTTP ライブストリーミングと HLS オーサリングの仕様](https://developer.apple.com/services-account/download?path=/Documentation/HLS_Authoring_Specification_for_Apple_TV/HLS_Authoring_Specification_for_Apple_TV.pdf)[に関する説明](https://developer.apple.com/library/prerelease/tvos/referencelibrary/GettingStarted/AboutHTTPLiveStreaming/about/about.html#//apple_ref/doc/uid/TP40013978)を参照してください。書.

<a name="User-Interface-Challenges" />

## <a name="user-interface-challenges"></a>ユーザーインターフェイスの課題

IOS や OS X とは異なり、Apple TV には、ユーザーがアプリやそのコンテンツを直接選択して操作できるタッチスクリーンまたはマウスがありません。 代わりに、アプリのユーザーインターフェイスを移動するために、新しい Siri リモートまたは Bluetooth ゲームコントローラーをユーザーに対して行います。 詳細については、 [Siri リモートおよび Bluetooth コントローラー](~/ios/tvos/platform/remote-bluetooth.md)のドキュメントを参照してください。

さらに、ユーザーエクスペリエンス全体は、シングルユーザーエクスペリエンスとなる傾向がある iOS または Mac アプリとは大きく異なります。 Apple TV では、ユーザーエクスペリエンスが本質的により社会的になる傾向があります。これは、複数のユーザーが1つのアプリと相互作用するソファに座っている可能性があります。 Apple TV アプリのエクスペリエンス (新しいアプリまたは既存のアプリの移植) を設計するには、これらの変更を考慮する必要があります。 

<a name="Summary" />

### <a name="working-with-focus-and-parallax-images"></a>フォーカスと視差イメージの操作

前述のように、tvOS アプリのユーザーは、iOS と直接やり取りするのではなく、デバイスの画面上のイメージをタップしますが、Siri リモートを使用してルーム間で間接的に移動します。 このユーザーの操作を提示して処理するために、Apple TV はフォーカスベースのモデルを使用します。 

フォーカスが変更されると、微妙なアニメーションや効果 (画像への視差効果など) を使用して、現在フォーカスがあるユーザーインターフェイス項目を明確に識別できます。

ユーザーが Siri リモートで低速で円形のジェスチャを実行した場合、フォーカスがある項目は、この移動に応じてリアルタイムで表示されます。 Sway が発生すると、光の光沢が画像に適用され、表面が光に見えるようになります。 一定量の非アクティブな状態が続くと、フォーカスのないコンテンツが淡色表示され、フォーカスがある項目はさらに大きくなります。

詳細については、「[ナビゲーションの操作](~/ios/tvos/app-fundamentals/navigation-focus.md)」および「[アイコンとイメージの操作](~/ios/tvos/app-fundamentals/icons-images.md)」のドキュメントを参照してください。

<a name="The-Home-Screen" />

### <a name="the-home-screen"></a>ホーム画面

Apple TV ホーム画面には、インストールされているすべてのアプリが表示され、ユーザー設定にアクセスする方法が用意されています。

[![](tvos9-images/home01.png "The Home Screen")](tvos9-images/home01.png#lightbox)

ユーザーは、フォーカスを使用してアプリを選択して起動することにより、Siri リモートでタッチジェスチャを使用してアプリアイコンのグリッドに移動します。 アプリアイコンは、潜在的なユーザーに対して優れた印象を与え、アプリの目的を一目で把握するための最初の機会です。

すべてのアプリで、アプリアイコンの小さいバージョンと大きなバージョンの両方を提供する必要があります。 小さいアイコンは、アプリのインストール時に、Apple TV ホーム画面で使用されます。 大規模なバージョンは、App Store によって使用されます。 大きいアプリアイコンは、小さいアイコンバージョンのルックアンドフィールを模倣します。

詳細については、[アイコンとイメージの操作に](~/ios/tvos/app-fundamentals/icons-images.md)関するドキュメントを参照してください。

<a name="The-Top-Shelf" />

### <a name="the-top-shelf"></a>上部の棚

ユーザーが Apple TV ホーム画面の一番上の行に tvOS アプリを配置した場合は、ユーザーがアプリを選択すると、大きな上部の棚の画像が表示されます。 このイメージは、アプリの機能を強調表示したり、コンテンツへの直接リンクを提供したりする必要があります。

[![](tvos9-images/topshelf01.png "The Top Shelf")](tvos9-images/topshelf01.png#lightbox)

一番上の棚の画像は、1つの静的な `.png` または `.lsr` ファイルとして提供するか、または実行時にフォーカス可能な項目の1行として動的に作成することができます。

静的なトップシェルフイメージを表示する代わりに、動的な行またはフォーカス可能な項目、またはスクロール用の動的な一連のバナーを含めることができます。 どちらの動的スタイルでも、アプリによって提供されるコンテンツを強調表示したり、最も使用されている機能にジャンプしたりすることができます。

詳細については、「[アイコンとイメージの操作](~/ios/tvos/app-fundamentals/icons-images.md)」のドキュメントと Apple の[TVServices Framework リファレンス](https://developer.apple.com/library/prerelease/tvos/documentation/TVServices/Reference/TVServices_Ref/index.html#//apple_ref/doc/uid/TP40016412)を参照してください。これにより、アプリに上位棚の拡張機能を追加して、動的な上位シェルフコンテンツを提供する方法についての詳細を確認できます。

## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマンインターフェイスガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリプログラミングガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [Xamarin を使用した tvOS 用アプリの構築 (ビデオ)](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
