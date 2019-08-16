---
title: リソース修飾子と視覚化のオプション
description: このトピックでは、一部の修飾子値が一致した場合にのみ使用されるリソースを定義する方法について説明します。 単純な例として、言語で修飾された文字列リソースがあります。 文字列リソースは既定値として定義でき、他の言語で使用するために他のリソースが定義されています。 すべてのリソースの種類は、レイアウトそのものを含めて修飾できます。
ms.prod: xamarin
ms.assetid: 2111C18A-3EDA-3787-25E1-3869FF4BE441
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
ms.openlocfilehash: 750cf801d8ae9dfe63f9b2259d4a3f6a386a4404
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69523235"
---
# <a name="resource-qualifiers-and-visualization-options"></a>リソース修飾子と視覚化オプション

_このトピックでは、一部の修飾子値が一致した場合にのみ使用されるリソースを定義する方法について説明します。単純な例として、言語で修飾された文字列リソースがあります。文字列リソースは既定値として定義でき、他の言語で使用するために他のリソースが定義されています。すべてのリソースの種類は、レイアウトそのものを含めて修飾できます。_


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="resource-qualifier-options"></a>リソース修飾子のオプション

**リソース修飾子のオプション**にアクセスするには、**横**モードボタンの右側にある省略記号アイコンをクリックします。

[![リソース修飾子のオプション](resource-qualifiers-images/vs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/vs/08-resource-qual-opt.png#lightbox)

このダイアログボックスには、次のリソース修飾子のプルダウンメニューが表示されます。

- **言語**&ndash;使用可能な言語リソースが表示され、新しい言語/地域リソースを追加するためのオプションが用意されています。

- **UI モード**表示モード ( **Car dock**や**Desk dock**など) とレイアウトの方向の一覧を示します。 &ndash;

これらの各プルダウンメニューでは、新しいダイアログボックスが開き、リソース修飾子を選択して構成できます (次の説明をご覧ください)。

### <a name="language"></a>言語

**[言語]** プルダウンメニューには、リソースが定義されている言語 (既定では**すべての言語**) のみが表示されます。 ただし、新しい言語を一覧に追加することができる **[言語/地域の追加...]** オプションもあります。

[![言語/地域の追加](resource-qualifiers-images/vs/09-add-language-region-sml.png)](resource-qualifiers-images/vs/09-add-language-region.png#lightbox)

**[言語/地域の追加...]** をクリックすると、 **[言語の選択]** ダイアログボックスが開き、使用可能な言語と地域のドロップダウンリストが表示されます。

![言語の一覧](resource-qualifiers-images/vs/10-languages.png "言語の一覧")

この例では、フランス語の地域言語に対して**fr (フランス語)** を選択しています。 特定の地域に関係なく多くの言語を指定できるため、 **Region**フィールドは省略可能です。 **[言語]** プルダウンメニューを再び開くと、新しく追加された言語/地域のリソースが表示されます。

選択された![言語と地域]選択された(resource-qualifiers-images/vs/11-language-region-added.png "言語と地域")

新しい言語を追加しても、新しい言語用のリソースを作成しない場合、次回プロジェクトを開いたときに追加された言語は表示されなくなります。

### <a name="ui-mode"></a>UI モード

**[UI モード]** プルダウンメニューをクリックすると、**通常**、**車ドック**、**机ドック**、**テレビ**、**アプライアンス**、**ウォッチ**などのモードの一覧が表示されます。


[![UI モードメニュー](resource-qualifiers-images/vs/12-ui-mode-sml.png)](resource-qualifiers-images/vs/12-ui-mode.png#lightbox)

この一覧の下には、夜と夜では**なく**、レイアウトの方向が**左**から右 、右から左に表示されます (**左から**右および右から左へのオプションについては、「」を参照してください [)。LayoutDirection](xref:Android.Util.LayoutDirection))。
**[リソース修飾子のオプション]** ダイアログの最後の項目は、**ラウンド画面**です (Android の磨耗で使用)。または、**ラウンドスクリーンではありません**。
ラウンド画面と非ラウンドスクリーンの詳細については、「[レイアウト](https://developer.android.com/training/wearables/ui/layouts.html)」を参照してください。
Android UI モードの詳細については、「 [Uimodemanager](xref:Android.App.UiModeManager)」を参照してください。

## <a name="action-bar-settings"></a>操作バーの設定

**[アクションバーの設定]** アイコンは、ペイントブラシ (テーマエディター) アイコンの左側にあります。

![操作バーの設定](resource-qualifiers-images/vs/14-action-bar.png "操作バーの設定")

このアイコンは、次の3つの操作バーモードのいずれかを選択する方法を提供するダイアログ segue を開きます。

- **標準**&ndash;ロゴ、またはオプションの字幕を含むアイコンとタイトルのテキストで構成されます。

- **リスト**&ndash;リストナビゲーションモード。 このモードでは、静的なタイトルテキストではなく、アクティビティ内のナビゲーションのリストメニューが表示されます (つまり、ドロップダウンリストとしてユーザーに表示できます)。

- **タブ**&ndash;タブナビゲーションモード。 このモードでは、静的なタイトルテキストではなく、アクティビティ内のナビゲーションのための一連のタブが表示されます。

## <a name="themes"></a>テーマ

**[テーマ]** ドロップダウンメニューには、プロジェクトで定義されているすべてのテーマが表示されます。 **[その他のテーマ]** を選択すると、次に示すように、インストールされている Android SDK で使用可能なすべてのテーマの一覧を含むダイアログが開きます。

[![その他のテーマの一覧](resource-qualifiers-images/vs/15-theme-menu-sml.png "その他のテーマの一覧")](resource-qualifiers-images/vs/15-theme-menu.png#lightbox)

テーマを選択すると、デザインサーフェイスが更新され、新しいテーマの効果が表示されます。 この変更は、 **[テーマ]** ダイアログで **[OK** ] ボタンをクリックした場合にのみ、永続的になります。 選択したテーマは、次に示すように、**テーマ**のドロップダウンメニューに表示されます。

![明るいテーマを使用できるようになりました](resource-qualifiers-images/vs/16-light-theme.png "明るいテーマを使用できるようになりました")

## <a name="android-version"></a>Android バージョン

Android**バージョン**セレクターは、デザイナーでレイアウトを表示するために使用される android バージョンを設定します。 セレクターには、プロジェクトのターゲットフレームワークバージョンと互換性のあるすべてのバージョンが表示されます。

![Android バージョンの一覧](resource-qualifiers-images/vs/17-android-version.png "Android バージョンの一覧")

ターゲットフレームワークのバージョンは、プロジェクトの設定の **プロパティ > アプリケーション > Android バージョンを使用してコンパイル**するように設定できます。 ターゲットフレームワークのバージョンの詳細については、「 [ANDROID API レベル](~/android/app-fundamentals/android-api-levels.md)について」を参照してください。

ツールボックスで使用できるウィジェットのセットは、プロジェクトのターゲットフレームワークのバージョンによって決まります。 これは、[**プロパティ] ウィンドウ**で使用できるプロパティにも当てはまります。 使用できるウィジェットの一覧は、ツールバーの **[バージョン]** セレクターで選択した値によって決まります。 たとえば、プロジェクトのターゲットバージョンを Android 4.4 に設定した場合でも、ツールバーバージョンセレクターで Android 6.0 を選択して、Android 6.0 でのプロジェクトの外観を確認できますが、Android 6.0 &ndash;に固有のウィジェットを追加することはできません。 Android 4.4 で利用できるウィジェットに限定されます。

リソースの種類の詳細については、「 [Android Resources](~/android/app-fundamentals/resources-in-android/index.md)」を参照してください。



# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="resource-qualifier-options"></a>リソース修飾子のオプション

**リソース修飾子のオプション**にアクセスするには、**横**モードボタンの右側にある省略記号アイコンをクリックします。

[![リソース修飾子のオプション](resource-qualifiers-images/xs/08-resource-qual-opt-m75-sml.png)](resource-qualifiers-images/xs/08-resource-qual-opt-m75.png#lightbox)

このダイアログボックスには、次のリソース修飾子のプルダウンメニューが表示されます。

- **言語**&ndash;使用可能な言語リソースが表示され、新しい言語/地域リソースを追加するためのオプションが用意されています。

- **UI モード**表示モード ( **Car dock**や**Desk dock**など) とレイアウトの方向の一覧を示します。 &ndash;

これらの各プルダウンメニューでは、新しいダイアログボックスが開き、リソース修飾子を選択して構成できます (次の説明をご覧ください)。

### <a name="language"></a>言語

**[言語]** プルダウンメニューには、リソースが定義されている言語 (既定では**すべての言語**) のみが表示されます。 ただし、新しい言語を一覧に追加することができる **[言語/地域の追加...]** オプションもあります。

[![言語/地域の追加](resource-qualifiers-images/xs/09-add-language-region-m75-sml.png)](resource-qualifiers-images/xs/09-add-language-region-m75.png#lightbox)

**[言語/地域の追加...]** をクリックすると、 **[言語の選択]** ダイアログボックスが開き、使用可能な言語と地域のドロップダウンリストが表示されます。

[![言語の一覧](resource-qualifiers-images/xs/10-languages-m75-sml.png)](resource-qualifiers-images/xs/10-languages-m75.png#lightbox)

この例では、フランス語の地域言語に対して**fr (フランス語)** を選択しています。 特定の地域に関係なく多くの言語を指定できるため、 **Region**フィールドは省略可能です。 **[言語]** プルダウンメニューを再び開くと、新しく追加された言語/地域のリソースが表示されます。

[![選択された言語と地域](resource-qualifiers-images/xs/11-language-region-added-m75-sml.png)](resource-qualifiers-images/xs/11-language-region-added-m75.png#lightbox)

新しい言語を追加しても、新しい言語用のリソースを作成しない場合、次回プロジェクトを開いたときに追加された言語は表示されなくなります。

### <a name="ui-mode"></a>UI モード

**[UI モード]** プルダウンメニューをクリックすると、**通常**、**車ドック**、**机ドック**、**テレビ**、**アプライアンス**、**ウォッチ**などのモードの一覧が表示されます。

[![UI モードメニュー](resource-qualifiers-images/xs/12-ui-mode-m75-sml.png)](resource-qualifiers-images/xs/12-ui-mode-m75.png#lightbox)

この一覧の下には、夜と夜では**なく**、レイアウトの方向が左から右、**右から左** **に**なる夜間モードがあります。 オプションの最後のペアでは、**ラウンドスクリーン**または**四角形の画面**のいずれかを選択できます (Android の磨耗デバイスの場合に便利です)。

Android UI モードの詳細については、「 [Uimodemanager](xref:Android.App.UiModeManager)」を参照してください。
**左から右**、**右から左**へのオプションの詳細については、「 [layoutdirection](xref:Android.Util.LayoutDirection)」を参照してください。


## <a name="action-bar-settings"></a>操作バーの設定

**[アクションバーの設定]** アイコンは、ペイントブラシ (テーマエディター) アイコンの左側にあります。

[![操作バーの設定](resource-qualifiers-images/xs/13-action-bar-m75-sml.png)](resource-qualifiers-images/xs/13-action-bar-m75.png#lightbox)

このアイコンは、次の3つの操作バーモードのいずれかを選択する方法を提供するダイアログ segue を開きます。

- **標準**&ndash;ロゴ、アイコン、およびタイトルのテキストで構成され、オプションのサブタイトルを使用します。

- **リスト**&ndash;リストナビゲーションモード。 このモードでは、静的なタイトルテキストではなく、アクティビティ内のナビゲーションのリストメニューが表示されます (つまり、ドロップダウンリストとしてユーザーに表示できます)。

- **タブ**&ndash;タブナビゲーションモード。 このモードでは、静的なタイトルテキストではなく、アクティビティ内のナビゲーションのための一連のタブが表示されます。

## <a name="themes"></a>テーマ

**[テーマ]** ドロップダウンメニューには、プロジェクトで定義されているすべてのテーマが表示されます。 **[その他のテーマ]** を選択すると、次に示すように、インストールされている Android SDK で使用可能なすべてのテーマの一覧を含むダイアログが開きます。

[![その他のテーマの一覧](resource-qualifiers-images/xs/14-theme-menu-m75-sml.png)](resource-qualifiers-images/xs/14-theme-menu-m75.png#lightbox)

テーマを選択すると、デザインサーフェイスが更新され、新しいテーマの効果が表示されます。 この変更は、 **[テーマ]** ダイアログで **[OK** ] ボタンをクリックした場合にのみ、永続的になります。 選択したテーマは、次に示すように、**テーマ**のドロップダウンメニューに表示されます。

[![明るいテーマを使用できるようになりました](resource-qualifiers-images/xs/15-light-theme-m75-sml.png)](resource-qualifiers-images/xs/15-light-theme-m75.png#lightbox)

## <a name="android-version"></a>Android バージョン

Android**バージョン**セレクターは、デザイナーでレイアウトを表示するために使用される android バージョンを設定します。 セレクターには、プロジェクトのターゲットフレームワークバージョンと互換性のあるすべてのバージョンが表示されます。

[![Android バージョンの一覧](resource-qualifiers-images/xs/16-android-version-m75-sml.png)](resource-qualifiers-images/xs/16-android-version-m75.png#lightbox)

ターゲットフレームワークのバージョンは、プロジェクトの設定の **[プロジェクトオプション > ビルド > 全般]** セクションで設定できます。 ターゲットフレームワークのバージョンの詳細については、「 [ANDROID API レベル](~/android/app-fundamentals/android-api-levels.md)について」を参照してください。

ツールボックスで使用できるウィジェットのセットは、プロジェクトのターゲットフレームワークのバージョンによって決まります。 これは、**プロパティパッド**で使用できるプロパティにも当てはまります。 使用できるウィジェットの一覧は、ツールバーの **[バージョン]** セレクターで選択した値によって決まります。 たとえば、プロジェクトのターゲットバージョンを Android 4.4 に設定した場合でも、ツールバーバージョンセレクターで Android 6.0 を選択して、Android 6.0 でのプロジェクトの外観を確認できますが、Android 6.0 &ndash;に固有のウィジェットを追加することはできません。 Android 4.4 で利用できるウィジェットに限定されます。

リソースの種類の詳細については、「 [Android Resources](~/android/app-fundamentals/resources-in-android/index.md)」を参照してください。

-----
