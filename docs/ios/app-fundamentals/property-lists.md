---
title: "プロパティ リストの使用"
description: "このドキュメントでは、Info.plist と Entitlements.plist の操作に Mac のグラフィックと高度なプロパティ一覧 (.plist) のエディターの Visual Studio を紹介します。 示されている設定アイコンと起動イメージからの iOS アプリケーションを Visual Studio for mac 内"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5E687043-0443-377C-9A12-9C5A05958646
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 778e70f6817b71e5910aa85425d46261dfe9c803
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-property-lists"></a>プロパティ リストの使用

_このドキュメントでは、Info.plist と Entitlements.plist の操作に Mac のグラフィックと高度なプロパティ一覧 (.plist) のエディターの Visual Studio を紹介します。示されている設定アイコンと起動イメージからの iOS アプリケーションを Visual Studio for mac 内_

Visual Studio for Mac アプリのプロパティおよび機能を簡単に編集を行うグラフィカル .plist エディターの機能します。 Visual Studio for Mac が 2 つの .plists -`Info.plist`アプリのプロパティと、アイコンを編集して`Entitlements.plist`アプリの機能を管理するためです。 このガイドは、Info.plists を紹介し、for mac を Visual Studio で操作の概要を示します Entitlements.plist については、次を参照してください。、[権利に関する作業](~/ios/deploy-test/provisioning/entitlements.md)ガイドです。

## <a name="infoplist"></a>Info.plist

情報プロパティ一覧 ( `Info.plist`) は、必要な iOS ファイル システムに、アプリケーションの構成に関する情報を提供します。 Visual Studio for Mac のカスタム`Info.plist`エディター ウィンドウの下部にあるタブによって制御される 3 つのパネルが左エディターの機能。

 [![](property-lists-images/tabs.png "エディター ウィンドウの下部にある Info.plist エディターのタブが左")](property-lists-images/tabs.png#lightbox)

各パネルは、以下のように、さまざまなプロパティやを制御します。

-  **アプリケーションのパネル**-一般的なアプリケーションのプロパティだけでなくアイコンと起動を設定するためのグラフィカル インターフェイスがイメージ; マップ統合と backgrounding モードを指定します。
-  **パネルを高度な**-高度なパネルはサポートされているドキュメントの種類、UTIs を指定する場所と URL の種類。
-  **パネルをソース**-ソース パネルは、一般的ではないプロパティだけでなく、アプリケーションのカスタム プロパティを制御します。


次の 3 つのセクションでは、さらに詳しくは、各パネルの機能を調査します。

## <a name="application-panel"></a>アプリケーションのパネル

Mac 用の visual Studio 機能の一般的な編集のためのグラフィカル インターフェイス`Info.plist`アプリケーションのエントリ。

1.  アプリケーションのプロパティ
1.  サポートされているデバイスの種類
1.  各デバイスの種類の向きをサポートします。
1.  ステータス バーのスタイルと色
1.  アイコンと起動画面
1.  マップとバック グラウンド モード


これらは、次のセクションで詳しく説明します。

 <a name="iOS_Application_Target" />


### <a name="ios-application-target"></a>iOS アプリケーションのターゲット

このセクションには、アプリケーションを説明する重要な情報が含まれています。
**識別子**格納されている (App Store アプリ) の iTunes Connect でと、iOS プロビジョニング ポータルのアプリ Id の一覧との開発と配布の証明書にも入力されているバンドル Id がここで一致する必要があります。

 [![](property-lists-images/image24.png "iOS アプリケーションのターゲット")](property-lists-images/image24.png#lightbox)

### <a name="device-deployment"></a>デバイスの展開

 [![](property-lists-images/deployment.png "デバイスの展開")](property-lists-images/deployment.png#lightbox)

デバイス**展開**で選択した項目に応じて、選択的に情報のセクションが表示されます、**デバイス**ドロップダウン リストで、 **Application Target**前のセクションです。 **Main インターフェイス**ドロップダウンに設定されている**MainStoryboard**ストーリー ボード駆動型アプリケーションでします。 ユーザー インターフェイスがコードで完全に記述されている場合、これを空白のままです。

### <a name="supported-device-orientations"></a>サポートされているデバイスの向き

 **デバイスの向きをサポート**アプリがデバイスのローテーションに応答する方法を制御します。 IPhone または iPad アプリのみをサポートする、非常に一般的**縦**、またはすべてが**上下**です。 一般にゲームを除くすべての iPad アプリケーションでは、すべての向きをサポートする必要があります。

### <a name="status-bar-styles"></a>ステータス バーのスタイル

**ステータス バー スタイル**セクションが編集するアプリケーションのためのグラフィカル インターフェイス`UIStatusBarStyle`:

 [![](property-lists-images/status.png "ステータス バーのスタイル")](property-lists-images/status.png#lightbox)

 <a name="Icons" />


### <a name="icons-launch-images-and-itunes-artwork"></a>アイコン、起動イメージ、および iTunes のアートワーク

Info.plist ファイル内のアイコン、イメージ、およびアートワークの使用の詳細については含まれて、[イメージを操作](~/ios/app-fundamentals/images-icons/index.md)ガイドです。




### <a name="maps-integration-and-background-modes"></a>マップ統合とバック グラウンド モード

`Info.plist`マップ統合と backgrounding モードを指定する特別なセクションが含まれます。 サポートするオプションを選択すると、アプリを実行するのに必要なプロパティが追加されます。

 [![](property-lists-images/maps.png "マップの統合")](property-lists-images/maps.png#lightbox)

Xamarin を参照して、マップ操作の詳細については、 [iOS マップ](~/ios/user-interface/controls/ios-maps/index.md)ガイドです。

 [![](property-lists-images/bging.png "バック グラウンド モード")](property-lists-images/bging.png#lightbox)

バック グラウンド モードの詳細については、Xamarin を参照してください[ios Backgrounding](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md)ガイドです。

## <a name="advanced-panel"></a>[詳細] パネル

高度なパネルは、ドキュメントの種類と、アプリケーションがサポートするための URL スキームを制御します。

 [![](property-lists-images/image34.png "[詳細] パネル")](property-lists-images/image34.png#lightbox)

 <a name="Document_Types" />


## <a name="document-types"></a>ドキュメントの種類

特定の種類のファイルを開くことがサポートするアプリケーションでは、iOS が用意されており、`CFBundleDocumentTypes`キー。 特定種類 - Pdf などの既知のファイルをサポートするようにアプリケーションをたい場合は、キーに PDF 値を追加おとします。 このセクションでは、データを入力する便利な方法で格納される、`CFBundleDocumentTypes`キー、`Info.plist`ファイル。

マニュアルを参照して[ファイルの種類のアプリがサポートを登録する](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html)をこれらの値を構成する方法の詳細。

## <a name="utis"></a>UTIs

アプリケーションは、カスタムのファイルの種類を開くことがサポートする必要があります。 たとえば、することも、カスタム拡張機能でイメージ ファイルを開く*.xam*です。 カスタム ファイルの種類を指定するを作成、カスタムの汎用型識別子のユーティリ ティーを使用して、`UIExportedTypeDeclarations`キー。 次のスクリーン ショットは、.xam 拡張機能のカスタム ユーティリ ティーを作成する方法を示しています。

 [![](property-lists-images/uti.png "UTIs エディター")](property-lists-images/uti.png#lightbox)

だけにエクスポートされた型 UTIs カスタム UTIs、アプリに特定の指定、*型 UTIs をインポート*(`UIImportedTypeDeclarations`キー) サポートされていますが、アプリケーションによって所有されていないカスタムの型を指定します。

カスタム UTIs を使用する方法については、Apple を参照してください[登録ファイルの種類、アプリケーションがサポートする](https://developer.apple.com/library/ios/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_declare/understand_utis_declare.html#//apple_ref/doc/uid/TP40001319-CH204-SW1)ガイドです。

## <a name="custom-urls"></a>カスタム Url

(プロトコルとも呼ばれます)、URL のスキーム名は、URL の最初の部分です。 たとえば、`http://`と`https://`URL スキームは共通します。 アプリケーションのカスタム URL スキームを作成するオプションがあります。 カスタムの URL スキームは、通信し、他のアプリケーションと前後のデータの送信に使用されます。 次のスクリーン ショットと呼ばれる新しいカスタム URL スキームの作成を示しています`monkeys://`:。

 [![](property-lists-images/url.png "カスタム Url")](property-lists-images/url.png#lightbox)



カスタムの URL スキームを実装する方法については、Apple を参照してください[カスタム URL のスキームを実装するこのガイドの「](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)

## <a name="source-panel"></a>ソース パネル

**ソース**のタブ、`Info.plist`ファイルが追加または編集するカスタムの値を使用します。 Visual Studio for Mac は、最も一般的なプロパティの一覧を示します。

 [![](property-lists-images/image31.png "ドロップダウン リストから新しいプロパティを追加します。")](property-lists-images/image31.png#lightbox)

Mac 用の Visual Studio の既知のプロパティの次のスクリーン ショットに示すように、有効な値の一覧はします。

 [![](property-lists-images/image32.png "既知の値リストから値を選択します。")](property-lists-images/image32.png#lightbox)

Visual Studio for Mac はようにもプロパティの型を検出します。

 [![](property-lists-images/image33.png "使用可能なプロパティの型")](property-lists-images/image33.png#lightbox)

Apple の確認[アプリ リソースの関連する](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html)省略可能なプロパティに関する追加情報へのリンク。

 <a name="Entitlements" />

## <a name="summary"></a>まとめ

この記事では、グラフィカルと高度な形式の .plist エディターを使用してアイコンを指定して、イメージを起動する一般的なアプリの構成もを編集する示されています。 導入されましたが、`Entitlements.plist`を追加すると、アプリの機能を管理します。


## <a name="related-links"></a>関連リンク

- [IDE](https://developer.xamarin.com/recipes/cross-platform/ide)
- [アプリの関連リソース](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html)
- [アプリがサポートする型ファイルを登録します。](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html)
- [カスタム URL スキームを実装します。](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)
- [資産カタログについて](https://developer.apple.com/library/ioshttps://developer.xamarin.com/recipes/xcode_help-image_catalog-1.0/Recipe.html)
