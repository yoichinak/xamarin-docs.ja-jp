---
title: Xamarin のプロパティリストの操作
description: このドキュメントでは、Visual Studio for Mac のグラフィカルおよび詳細プロパティリスト (plist) エディターについて説明します。このエディターでは、Info.plist と Entitlements.plist を操作できます。 ここでは、Visual Studio for Mac 内から iOS アプリケーションのアイコンと起動イメージを設定する方法について説明します。
ms.prod: xamarin
ms.assetid: 5E687043-0443-377C-9A12-9C5A05958646
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 2060e0786b5401b44217318b647dfa7412f934f4
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73009874"
---
# <a name="working-with-property-lists-in-xamarinios"></a>Xamarin のプロパティリストの操作

_このドキュメントでは、Visual Studio for Mac のグラフィカルおよび詳細プロパティリスト (plist) エディターについて説明します。このエディターでは、Info.plist と Entitlements.plist を操作できます。ここでは、Visual Studio for Mac 内から iOS アプリケーションのアイコンと起動イメージを設定する方法について説明します。_

Visual Studio for Mac には、アプリのプロパティや機能を簡単に編集できるグラフィカルな plist エディターが用意されています。 Visual Studio for Mac には、アプリのプロパティとアイコンを編集するための plists-`Info.plist` と、アプリの機能を管理するための `Entitlements.plist` があります。 このガイドでは、plists について説明し、Visual Studio for Mac での作業の概要を示します。 権利の詳細については、「[権利の使用](~/ios/deploy-test/provisioning/entitlements.md)」ガイドを参照してください。

## <a name="infoplist"></a>Info.plist

Information プロパティリスト (`Info.plist`) は、アプリケーションの構成に関する情報をシステムに提供する必須の iOS ファイルです。 Visual Studio for Mac のカスタム `Info.plist` エディターには、エディターウィンドウの左下にあるタブで制御される3つのパネルがあります。

 [![](property-lists-images/tabs.png "The Info.plist editor tabs at the bottom left of the editor window")](property-lists-images/tabs.png#lightbox)

各パネルでは、次に示すように、さまざまなプロパティを制御します。

- **アプリケーションパネル**-共通のアプリケーションプロパティだけでなく、アイコンと起動イメージを設定するためのグラフィカルインターフェイスです。マップの統合モードとバックグラウンド処理モードを指定します。
- **[詳細設定] パネル**-[詳細設定] パネルは、サポートされているドキュメントの種類、uti、および URL の種類を指定する場所です。
- [**ソース] パネル**-[ソース] パネルでは、アプリケーションのカスタムプロパティだけでなく、あまり一般的ではないプロパティも制御します。

次の3つのセクションでは、各パネルの機能について詳しく説明します。

## <a name="application-panel"></a>アプリケーションパネル

Visual Studio for Mac は、アプリケーションの共通 `Info.plist` エントリを編集するためのグラフィカルインターフェイスを備えています。

1. アプリケーションのプロパティ
1. サポートされているデバイスの種類
1. デバイスの種類ごとのサポートの向き
1. ステータスバーのスタイルと色
1. アイコンと起動画面
1. マップとバックグラウンドモード

これらの詳細については、次のセクションで詳しく説明します。

 <a name="iOS_Application_Target" />

### <a name="ios-application-target"></a>iOS アプリケーションのターゲット

このセクションには、アプリケーションについて説明する重要な情報が含まれています。
ここに格納されている**識別子**は、iTunes Connect (app Store アプリの場合) および IOS プロビジョニングポータルのアプリ id リストと、開発および配布証明書に入力されたバンドル id と一致する必要があります。

 [![](property-lists-images/image24.png "iOS Application Target")](property-lists-images/image24.png#lightbox)

### <a name="device-deployment"></a>デバイスの展開

 [![](property-lists-images/deployment.png "Device Deployment")](property-lists-images/deployment.png#lightbox)

上の **[アプリケーションターゲット]** セクションの **[デバイス]** ドロップダウンで選択した内容に応じて、デバイスの **[展開]** 情報 セクションが選択的に表示されます。 **メインのインターフェイス**ドロップダウンは、ストーリーボード駆動型アプリケーションでは**mainstoryboard.storyboard ファイル**に設定されます。 ユーザーインターフェイスがコードで完全に記述されている場合は、空白のままにすることができます。

### <a name="supported-device-orientations"></a>サポートされているデバイスの向き

 **サポートされているデバイスの向き**は、アプリがデバイスのローテーションにどのように応答するかを制御します。 IPhone/iPad アプリでは、**縦向き**だけをサポートしていますが、すべてが**反転**しています。 一般に、ゲーム以外のすべての iPad アプリケーションでは、すべての向きをサポートする必要があります。

### <a name="status-bar-styles"></a>ステータスバーのスタイル

**[ステータスバーのスタイル]** セクションは、アプリケーションの `UIStatusBarStyle`を編集するためのグラフィカルインターフェイスです。

 [![](property-lists-images/status.png "Status Bar Styles")](property-lists-images/status.png#lightbox)

 <a name="Icons" />

### <a name="icons-launch-images-and-itunes-artwork"></a>アイコン、起動画像、iTunes アートワーク

情報 plist ファイルでのアイコン、画像、およびアートワークの使用方法については、「[イメージの操作](~/ios/app-fundamentals/images-icons/index.md)」ガイドを参照してください。

### <a name="maps-integration-and-background-modes"></a>統合とバックグラウンドモードのマップ

`Info.plist` には、マップの統合モードとバックグラウンド処理モードを指定するための特別なセクションが含まれています。 サポートするオプションを選択すると、必要なプロパティがアプリケーションに追加されます。

 [![](property-lists-images/maps.png "Maps Integration")](property-lists-images/maps.png#lightbox)

マップの操作の詳細については、Xamarin [IOS maps](~/ios/user-interface/controls/ios-maps/index.md)ガイドを参照してください。

 [![](property-lists-images/bging.png "Background Modes")](property-lists-images/bging.png#lightbox)

バックグラウンドモードの詳細については、「iOS 用 Xamarin[バックグラウンド処理](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md)」ガイドを参照してください。

## <a name="advanced-panel"></a>[詳細設定] パネル

[詳細] パネルでは、アプリケーションがサポートするドキュメントの種類と URL スキームを制御します。

 [![](property-lists-images/image34.png "Advanced Panel")](property-lists-images/image34.png#lightbox)

 <a name="Document_Types" />

## <a name="document-types"></a>ドキュメントの種類

特定の種類のファイルを開くことをサポートするアプリケーションでは、iOS によって `CFBundleDocumentTypes` キーが提供されます。 アプリケーションで特定の既知のファイルの種類 (例: Pdf) をサポートする場合は、PDF の値をキーに追加します。 このセクションでは、`Info.plist` ファイルの `CFBundleDocumentTypes` キーに格納されるデータを入力する便利な方法を示します。

これらの値を構成する方法の詳細については、アプリでサポートされている[ファイルの種類の登録](https://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html)に関するドキュメントを参照してください。

## <a name="utis"></a>Uti

場合によっては、アプリケーションでカスタムファイルの種類を開くことがサポートされている必要があります。 たとえば、カスタムの拡張機能を使用してイメージファイルを開くことができ*ます。* カスタムファイルの種類を指定するには、`UIExportedTypeDeclarations` キーを使用して、カスタムの UTI 型識別子を作成します。 次のスクリーンショットは、UTI 拡張機能のカスタムの作成方法を示しています。

 [![](property-lists-images/uti.png "UTIs Editor")](property-lists-images/uti.png#lightbox)

エクスポートされた型 Uti は、アプリに固有のカスタム Uti を指定するのと同様に、インポートされる*型 uti* (`UIImportedTypeDeclarations` キー) はサポートされているものの、アプリケーションで所有されていないカスタム型を指定します。

カスタム Uti の使用方法の詳細については、「アプリでサポートされる Apple の[登録ファイルの種類](https://developer.apple.com/library/ios/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_declare/understand_utis_declare.html#//apple_ref/doc/uid/TP40001319-CH204-SW1)」を参照してください。

## <a name="custom-urls"></a>カスタム Url

Url の最初の部分は、URL スキーム名 (プロトコルとも呼ばれます) です。 たとえば、`http://` と `https://` は一般的な URL スキームです。 アプリケーションのカスタム URL スキームを作成することもできます。 カスタム URL スキームは、他のアプリケーションとの間で送受信されるデータのやり取りに使用されます。 次のスクリーンショットは、`monkeys://`という名前の新しいカスタム URL スキームを作成する方法を示しています。

 [![](property-lists-images/url.png "Custom URLs")](property-lists-images/url.png#lightbox)

カスタム URL スキームの実装の詳細については、[このガイドの「Apple のカスタム Url スキームの実装」](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)を参照してください。

## <a name="source-panel"></a>ソースパネル

`Info.plist` ファイルの **[ソース]** タブでは、カスタム値を追加または編集できます。 Visual Studio for Mac では、最も一般的なプロパティの一覧が表示されます。

 [![](property-lists-images/image31.png "Adding a new property from a dropdown")](property-lists-images/image31.png#lightbox)

既知のプロパティについては、次のスクリーンショットに示すように、Visual Studio for Mac に有効な値の一覧が表示されます。

 [![](property-lists-images/image32.png "Select a value from a know value list")](property-lists-images/image32.png#lightbox)

次に示すように、Visual Studio for Mac もプロパティの型を検出します。

 [![](property-lists-images/image33.png "The available property types")](property-lists-images/image33.png#lightbox)

オプションのプロパティの詳細については、Apple の[アプリ関連リソース](https://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html)のリンクを確認してください。

 <a name="Entitlements" />

## <a name="summary"></a>まとめ

この記事では、グラフィカルおよび上級の plist エディターを使用して、共通のアプリ構成を編集すると共に、アイコンと起動イメージを指定する方法について説明します。 また、アプリの機能を追加および管理するための `Entitlements.plist` も導入されました。

## <a name="related-links"></a>関連リンク

- [NETBEANS](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide)
- [アプリ関連のリソース](https://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html)
- [アプリでサポートされているファイルの種類を登録しています](https://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html)
- [カスタム URL スキームの実装](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)
- [アセットカタログ形式のリファレンス](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_ref-Asset_Catalog_Format/index.html#//apple_ref/doc/uid/TP40015170-CH18-SW1)
