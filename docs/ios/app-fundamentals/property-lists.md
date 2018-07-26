---
title: Xamarin.iOS でプロパティ リストを使用します。
description: このドキュメントは、Info.plist と Entitlements.plist を操作するため、Mac のグラフィックと高度なプロパティ一覧 (.plist) のエディターの Visual Studio を紹介します。 設定アイコンと起動イメージから iOS アプリケーションを示しています Visual Studio for mac 内。
ms.prod: xamarin
ms.assetid: 5E687043-0443-377C-9A12-9C5A05958646
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: b102f268fd457ca42f3d64b5766a2b3824e3849d
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242330"
---
# <a name="working-with-property-lists-in-xamarinios"></a>Xamarin.iOS でプロパティ リストを使用します。

_このドキュメントは、Info.plist と Entitlements.plist を操作するため、Mac のグラフィックと高度なプロパティ一覧 (.plist) のエディターの Visual Studio を紹介します。設定アイコンと起動イメージから iOS アプリケーションを示しています Visual Studio for mac 内。_

Visual Studio for Mac には、アプリのプロパティおよび機能を簡単に編集する .plist グラフィカル エディターが機能します。 Visual Studio for Mac が 2 つの .plists -`Info.plist`アプリのプロパティと、アイコンの編集と`Entitlements.plist`アプリの機能を管理するためです。 このガイドは、Info.plists を紹介しで作業して Visual Studio for mac の概要を説明します Entitlements.plist については、次を参照してください。、[権利の使用](~/ios/deploy-test/provisioning/entitlements.md)ガイド。

## <a name="infoplist"></a>Info.plist

情報のプロパティ リスト ( `Info.plist`) が必要な iOS ファイル システムに、アプリケーションの構成に関する情報を提供します。 Visual Studio for Mac のカスタム`Info.plist`エディター ウィンドウの下部にあるタブによって制御される 3 つのパネルの左エディターの機能。

 [![](property-lists-images/tabs.png "下部にある Info.plist エディターのタブがエディター ウィンドウの左")](property-lists-images/tabs.png#lightbox)

各パネルは、以下に示すとおり、別のプロパティを制御します。

-  **アプリケーション パネル**-一般的なアプリケーションのプロパティだけでなくアイコンと起動設定できるグラフィカル インターフェイスをイメージ; マップ統合とモードをバック グラウンド処理を指定します。
-  **パネルを高度な**-高度なパネルは、サポートされているドキュメントの種類の Uti を指定する場所と URL の種類。
-  **パネルをソース**-[ソース] パネルは、あまり一般的なプロパティだけでなく、アプリケーションのカスタム プロパティを制御します。


次の 3 つのセクションでは、さらに詳しくは、各パネルの機能を調査します。

## <a name="application-panel"></a>アプリケーション パネル

Visual Studio for Mac は、一般的な編集のためのグラフィカル インターフェイスを機能`Info.plist`アプリケーションのエントリ。

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
**識別子**格納ここで、iTunes Connect (App Store アプリ) 用と iOS プロビジョニング ポータルのアプリ Id の一覧と開発と配布証明書で入力されたバンドル識別子に一致する必要があります。

 [![](property-lists-images/image24.png "iOS アプリケーションのターゲット")](property-lists-images/image24.png#lightbox)

### <a name="device-deployment"></a>デバイスのデプロイ

 [![](property-lists-images/deployment.png "デバイスのデプロイ")](property-lists-images/deployment.png#lightbox)

デバイス**展開**で選択内容に応じて選択的に、情報のセクションが表示されます、**デバイス** ドロップダウン ボックスで、**アプリケーション ターゲット**前のセクション。 **メイン インターフェイス**ドロップダウンに設定されている**MainStoryboard**ストーリー ボード駆動型アプリケーションにします。 ユーザー インターフェイスがコードで完全に記述されている場合、これを空白のままです。

### <a name="supported-device-orientations"></a>サポートされているデバイスの向き

 **デバイスの向きをサポートされている**アプリがデバイスの回転にどのように応答する方法を制御します。 IPhone または iPad アプリのみをサポートするは非常に一般的なが**縦**、またはすべてが**上下**。 一般にゲームを除くすべての iPad アプリケーションでは、すべての向きをサポートする必要があります。

### <a name="status-bar-styles"></a>ステータス バーのスタイル

**ステータス バー スタイル**セクションが編集アプリケーションのためのグラフィカル インターフェイス`UIStatusBarStyle`:

 [![](property-lists-images/status.png "ステータス バーのスタイル")](property-lists-images/status.png#lightbox)

 <a name="Icons" />


### <a name="icons-launch-images-and-itunes-artwork"></a>アイコン、起動イメージ、および iTunes アートワーク

Info.plist ファイルのアイコン、イメージ、およびアートワークの使用に関する情報が記載されて、[イメージを操作](~/ios/app-fundamentals/images-icons/index.md)ガイド。




### <a name="maps-integration-and-background-modes"></a>マップ統合とバック グラウンド モード

`Info.plist`マップ統合とモードをバック グラウンド処理を指定する特別なセクションが含まれます。 サポートするオプションを選択すると、アプリケーションに必要なプロパティが追加されます。

 [![](property-lists-images/maps.png "マップ統合")](property-lists-images/maps.png#lightbox)

マップの操作方法の詳細については、Xamarin を参照してください[iOS マップ](~/ios/user-interface/controls/ios-maps/index.md)ガイド。

 [![](property-lists-images/bging.png "バック グラウンド モード")](property-lists-images/bging.png#lightbox)

バック グラウンド モードの詳細については、Xamarin を参照してください[ios バック グラウンド処理](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md)ガイド。

## <a name="advanced-panel"></a>[詳細] パネル

[詳細] パネルでは、ドキュメントの種類と、アプリケーションがサポートするための URL スキームを制御します。

 [![](property-lists-images/image34.png "[詳細] パネル")](property-lists-images/image34.png#lightbox)

 <a name="Document_Types" />


## <a name="document-types"></a>ドキュメントの種類

特定の種類のファイルを開くことがサポートするアプリケーションでは、iOS の提供、`CFBundleDocumentTypes`キー。 Pdf などの-特定の既知のファイル種類をサポートするためにする場合は、キーに PDF の値を追加しますが。 このセクションに格納されるデータを入力する便利な方法を提供する、`CFBundleDocumentTypes`キー、`Info.plist`ファイル。

ドキュメントを参照して[ファイルの種類のアプリケーションがサポートを登録する](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html)これらの値を構成する方法の詳細について。

## <a name="utis"></a>Uti

アプリケーションは、カスタム ファイルの種類を開くことがサポートする必要があります。 たとえば、カスタム拡張機能でイメージ ファイルを開くしたい場合があります *.xam*します。 カスタム ファイルの種類を指定するカスタムの UTI の Universal Type Identifier の使用を作成します、`UIExportedTypeDeclarations`キー。 次のスクリーン ショットは、カスタムの UTI .xam 拡張機能を作成する方法を示しています。

 [![](property-lists-images/uti.png "Uti エディター")](property-lists-images/uti.png#lightbox)

同様にエクスポートされたタイプの Uti、アプリに固有のカスタムの Uti を指定する、*タイプの Uti をインポート*(`UIImportedTypeDeclarations`キー) サポートされていますが、アプリケーションが所有していないカスタムの型を指定します。

カスタムの Uti の使用に関する詳細については、Apple を参照してください[登録ファイルの種類のアプリケーションがサポート](https://developer.apple.com/library/ios/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_declare/understand_utis_declare.html#//apple_ref/doc/uid/TP40001319-CH204-SW1)ガイド。

## <a name="custom-urls"></a>カスタム Url

URL スキーム名 (プロトコルとも呼ばれます) は、URL の最初の部分です。 たとえば、`http://`と`https://`URL スキームは共通です。 アプリケーションのカスタム URL スキームを作成するオプションがあります。 カスタムの URL スキームは、通信、他のアプリケーションで前後へのデータの送信に使用されます。 次のスクリーン ショットと呼ばれる新しいカスタム URL スキームの作成を示しています`monkeys://`:。

 [![](property-lists-images/url.png "カスタム Url")](property-lists-images/url.png#lightbox)



カスタムの URL スキームを実装する方法の詳細については、Apple を参照してください[このガイドのセクションをカスタムの URL スキームを実装します。](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)

## <a name="source-panel"></a>ソース パネル

**ソース**のタブ、`Info.plist`ファイルが追加または編集するカスタムの値を使用できます。 Visual Studio for Mac では、最も一般的なプロパティの一覧を示します。

 [![](property-lists-images/image31.png "ドロップダウン リストから新しいプロパティを追加します。")](property-lists-images/image31.png#lightbox)

Visual Studio for Mac の既知のプロパティには、次のスクリーン ショットに示すように、有効な値の一覧は。

 [![](property-lists-images/image32.png "既知の値リストから値を選択します。")](property-lists-images/image32.png#lightbox)

Visual Studio for Mac は、示すようにも、プロパティの型を検出します。

 [![](property-lists-images/image33.png "使用可能なプロパティの型")](property-lists-images/image33.png#lightbox)

Apple の確認[アプリの関連リソース](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html)省略可能なプロパティの詳細については、リンクします。

 <a name="Entitlements" />

## <a name="summary"></a>まとめ

この記事では、グラフィカルと高度な形式の .plist エディターを使用してアイコンを指定し、画像を起動したりも共通のアプリ構成を編集する示されています。 導入されましたが、`Entitlements.plist`を追加すると、アプリの機能を管理します。


## <a name="related-links"></a>関連リンク

- [IDE](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide)
- [アプリの関連リソース](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html)
- [ファイルを登録、アプリケーションがサポートを種類します。](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html)
- [カスタムの URL スキームを実装します。](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)
- [資産カタログ形式のリファレンス](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_ref-Asset_Catalog_Format/index.html#//apple_ref/doc/uid/TP40015170-CH18-SW1)
