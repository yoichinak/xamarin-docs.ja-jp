---
title: "リソース修飾子と視覚エフェクト オプション"
description: "このトピックでは、いくつかの修飾子の値が一致する場合にのみ使用されるリソースを定義する方法について説明します。 単純な例は、文字列の言語で修飾されたリソースです。 文字列リソースは、その他の代替のリソース定義に追加の言語を使用すると、既定値として定義できます。 レイアウトの種類を含む、すべてのリソース型を修飾できます。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2111C18A-3EDA-3787-25E1-3869FF4BE441
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/29/2018
ms.openlocfilehash: 56fee71f2ed36b682d323bae1225430ff991f140
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="resource-qualifiers-and-visualization-options"></a>リソース修飾子と視覚エフェクト オプション

_このトピックでは、いくつかの修飾子の値が一致する場合にのみ使用されるリソースを定義する方法について説明します。単純な例は、文字列の言語で修飾されたリソースです。文字列リソースは、その他の代替のリソース定義に追加の言語を使用すると、既定値として定義できます。レイアウトの種類を含む、すべてのリソース型を修飾できます。_

<a name="Custom_Device_Configurations" />

## <a name="custom-device-configurations"></a>カスタムのデバイスの構成

Android では、多種多様なデバイスと画面解像度でご確認いただけます。
多くのデバイスを対象にしたデザイン ユーザー インターフェイスのため、デザイナーに組み込まれているデバイスの構成のさまざまなが付属します。 その他のデバイスの構成を追加することもサポートしますこれらの構成がに基づいて*修飾子*別のデバイス構成が 1 つを区別するために指定することです。 さまざまな種類の修飾子があります。 これらのリソースの種類の詳細については、次を参照してください。 [Android リソース](~/android/app-fundamentals/resources-in-android/index.md)です。

メニューは、デバイスのセレクターの下部にある、**カスタマイズ**次に示すようにオプションします。


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![デバイスのセレクター メニュー](resource-qualifiers-images/vs/01-device-selector-sml.png)](resource-qualifiers-images/vs/01-device-selector.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![デバイスのセレクター メニュー](resource-qualifiers-images/xs/01-device-selector-sml.png)](resource-qualifiers-images/xs/01-device-selector.png)

-----


選択すると**カスタマイズ**ダイアログが表示されますを通じて使用可能なデバイス構成を参照するために使用できることです。 クリックすると、**デバイス定義** タブで、すべての既知のデバイスの定義の一覧が表示されます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![AVD Manager](resource-qualifiers-images/vs/02-device-definitions-sml.png)](resource-qualifiers-images/vs/02-device-definitions.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![AVD Manager](resource-qualifiers-images/xs/02-device-definitions-sml.png)](resource-qualifiers-images/xs/02-device-definitions.png)

-----


デザイナーで構成済みのデバイスを変更できません。 ただし、クリックして**デバイス作成しています.**をカスタムのデバイスの定義を定義します。 または、既存の定義を選択し、をクリックして**複製しています.**に新しい定義の開始点として使用します。
たとえばを選択すると、 **Nexus 5**定義しをクリックすると**複製しています.**次のダイアログ ボックスを表示します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![デバイスを複製します。](resource-qualifiers-images/vs/03-clone-sml.png)](resource-qualifiers-images/vs/03-clone.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![デバイスを複製します。](resource-qualifiers-images/xs/03-clone-sml.png)](resource-qualifiers-images/xs/03-clone.png)

-----


次のスクリーン ショットに名前を変更**Nexus 5 カスタム**し、新しいカスタムのデバイスの定義を作成するデバイス パラメーターを変更します。 この例では**縦**デバイスの定義がランドス ケープ専用になるようには無効になっています。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![カスタムのデバイス](resource-qualifiers-images/vs/04-custom-sml.png)](resource-qualifiers-images/vs/04-custom.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![カスタムのデバイス](resource-qualifiers-images/xs/04-custom-sml.png)](resource-qualifiers-images/xs/04-custom.png)

-----


**[Clone Device]\(デバイスの複製\)** をクリックすると、新しいデバイス定義が作成され、次のように **[Device Definitions]\(デバイス定義\)** リストに表示されます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![更新されたデバイスの定義](resource-qualifiers-images/vs/05-updated-device-definitions-sml.png)](resource-qualifiers-images/vs/05-updated-device-definitions.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![更新されたデバイスの定義](resource-qualifiers-images/xs/05-updated-device-definitions-sml.png)](resource-qualifiers-images/xs/05-updated-device-definitions.png)

-----


上記のように、各デバイスのユーザーが作成した定義が緑のアイコンで表示されることに注意してください。 戻ったら、**デバイス**セレクター メニュー、新しいカスタムのデバイスの定義が表示されるリストの最上位のセクションで (カスタム デバイスの構成でこの一覧で、IDE を再起動してみてくださいが表示されない) 場合。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![カスタムのデバイスがデバイスの一覧に表示されます。](resource-qualifiers-images/vs/06-nexus-5-custom-sml.png)](resource-qualifiers-images/vs/06-nexus-5-custom.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![カスタムのデバイスがデバイスの一覧に表示されます。](resource-qualifiers-images/xs/06-nexus-5-custom-sml.png)](resource-qualifiers-images/xs/06-nexus-5-custom.png)

-----


このデバイスの構成を選択すると前 (この場合、ランドス ケープ専用モードで) 作成、カスタマイズ設定に準拠するようにレイアウトを変更します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![使用中のカスタムのデバイス](resource-qualifiers-images/vs/07-custom-in-use-sml.png)](resource-qualifiers-images/vs/07-custom-in-use.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![使用中のカスタムのデバイス](resource-qualifiers-images/xs/07-custom-in-use-sml.png)](resource-qualifiers-images/xs/07-custom-in-use.png)

-----


<a name="resource_qualifier_options" />

## <a name="resource-qualifier-options"></a>リソース修飾子のオプション

**リソース修飾子のオプション**の右側に下向きの三角形アイコンをクリックしてアクセスできる、**デバイス構成**オプション。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![リソース修飾子のオプション](resource-qualifiers-images/vs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/vs/08-resource-qual-opt.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![リソース修飾子のオプション](resource-qualifiers-images/xs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/xs/08-resource-qual-opt.png)

-----


このダイアログ ボックスには、次のリソース修飾子のプルダウン メニューが表示されます。

-   **言語**&ndash;利用可能な言語のリソースを表示し、言語/地域の新しいリソースを追加するオプションを提供します。

-   **UI モード**&ndash;リスト表示モード (など**自動車ドッキング**と**デスク ドッキング**) レイアウトの方向とします。

このプルダウン メニューの各は、新しいダイアログ ボックスを選択して、(次に説明されている) とリソース修飾子を構成する場所を開きます。


<a name="Language_and_Region" />

### <a name="language"></a>言語

**言語**プルダウン メニューには、定義されているリソースを持つ言語のみが一覧表示 (または**すべての言語**、既定値)。 ただしはまた、**言語/地域を追加しています.**オプションをリストに新しい言語を追加することができます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![言語/地域を追加します。](resource-qualifiers-images/vs/09-add-language-region-sml.png)](resource-qualifiers-images/vs/09-add-language-region.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![言語/地域を追加します。](resource-qualifiers-images/xs/09-add-language-region-sml.png)](resource-qualifiers-images/xs/09-add-language-region.png)

-----


クリックすると、**言語/地域を追加しています.**、**言語の選択**使用可能な言語および地域のドロップダウン リストを表示するダイアログが表示されます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![言語の一覧](resource-qualifiers-images/vs/10-languages.png "言語の一覧")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![言語の一覧](resource-qualifiers-images/xs/10-languages-sml.png)](resource-qualifiers-images/xs/10-languages.png)

-----


この例では、選択した**fr (フランス語)**言語と**BE** (ベルギー) 地域がフランス語の言語の。 なお、**地域**フィールドは省略可能な特定の地域に関係なく、多くの言語を指定できます。 ときに、**言語**プルダウン メニューをもう一度開くと、言語/地域の新しく追加したリソースが表示されます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![言語および地域の選択](resource-qualifiers-images/vs/11-language-region-added.png "言語および地域の選択")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![言語および地域の選択](resource-qualifiers-images/xs/11-language-region-added-sml.png)](resource-qualifiers-images/xs/11-language-region-added.png)

-----


新しい言語を追加しても、表示は、不要になったに追加した言語は、次回の新しいリソースを作成しない場合は、プロジェクトが開くことに注意してください。


<a name="ui_mode" />

### <a name="ui-mode"></a>UI モード

クリックすると、 **UI モード**などプルダウン メニュー、モードの一覧が表示されます**標準**、**自動車ドッキング**、**デスク ドッキング**、 **テレビ**、**アプライアンス**、および**ウォッチ**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![UI モード メニュー](resource-qualifiers-images/vs/12-ui-mode-sml.png)](resource-qualifiers-images/vs/12-ui-mode.png)

夜間モードは、この一覧の下**いない夜**と**夜**レイアウトの方向と、その後**左から右に**と**右から左へ**(について**左から右に**と**右から左へ**オプションを参照してください[LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/)です。
最後の項目を**リソース修飾子のオプション**ダイアログ ボックスは、**画面を丸める**(用、Android を着用で使用) または**画面丸められない**(ラウンドについてと非ラウンド スクリーンを参照してください[レイアウト](https://developer.android.com/training/wearables/ui/layouts.html))。
Android の UI モードの詳細については、次を参照してください。 [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/)です。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![UI モード メニュー](resource-qualifiers-images/xs/12-ui-mode-sml.png)](resource-qualifiers-images/xs/12-ui-mode.png)

夜間モードは、この一覧の下**いない夜**と**夜**レイアウトの方向と、その後**左から右に**と**右から左へ**です。 Android の UI モードの詳細については、次を参照してください。 [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/)です。
について**左から右に**と**右から左へ**オプションを参照してください[LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/)です。

### <a name="round-screen"></a>Round 画面

最後の項目、**リソース修飾子のオプション**ダイアログは、**画面ラウンド**メニュー。 このメニューでは、いずれかを選択することができます**画面を丸める**(用、Android を着用で使用) または**四角形の画面**:

[ ![Round 画面メニュー](resource-qualifiers-images/xs/13-round-screen-sml.png)](resource-qualifiers-images/xs/13-round-screen.png)

-----


<a name="Action_Bar" />

## <a name="action-bar-settings"></a>アクションのバーの設定

**動作バー設定**アイコンはペイント ブラシ (テーマ エディター) アイコンの左側に使用します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![操作バー設定](resource-qualifiers-images/vs/14-action-bar.png "アクション バーの設定")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![アクションのバーの設定](resource-qualifiers-images/xs/13b-action-bar-sml.png)](resource-qualifiers-images/xs/13b-action-bar.png)

-----


このアイコンは、次の 3 つのアクションのバー モードのいずれかから選択する方法を提供するダイアログ重なってを開きます。

-   **標準的な**&ndash;ロゴまたはアイコンとタイトルのいずれかのテキスト オプション字幕とで構成されます。

-   **リスト**&ndash;リスト ナビゲーション モード。 静的なタイトルのテキストではなくこのモードは、アクティビティ内のナビゲーションのリスト メニューを表示 (つまり、表現できるドロップダウン リストとしてユーザーに)。

-   **タブ**&ndash;タブ ナビゲーション モード。 静的なタイトルのテキストの代わりには、このモードは、一連のアクティビティ内のナビゲーションのタブを表示します。


<a name="Themes" />

## <a name="themes"></a>テーマ

**テーマ**ドロップダウン メニューでは、すべてのプロジェクトで定義されているテーマが表示されます。 選択すると**よりテーマ**次に示すように、インストールされている Android SDK から利用可能なすべてのテーマの一覧がダイアログ ボックスを開きます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![複数のテーマ リスト](resource-qualifiers-images/vs/15-theme-menu-sml.png "よりテーマ ボックスの一覧")](resource-qualifiers-images/vs/15-theme-menu.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![複数のテーマのリスト](resource-qualifiers-images/xs/14-theme-menu-sml.png)](resource-qualifiers-images/xs/14-theme-menu.png)

-----


テーマを選択すると、デザイン画面が更新され、新しいテーマの結果を表示します。 この変更が永続化ことに注意してください場合にのみ、 **OK**でボタンがクリックされた、**テーマ**ダイアログ。 テーマを選択するに含まれます、**テーマ**ドロップダウン メニューとして表示される以下。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![ライト テーマが利用できるようになりました](resource-qualifiers-images/vs/16-light-theme.png "ライト テーマが利用できるようになりました")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![ライト テーマが利用できるようになりました](resource-qualifiers-images/xs/15-light-theme-sml.png)](resource-qualifiers-images/xs/15-light-theme.png)

-----


<a name="Android_Version" />

## <a name="android-version"></a>Android バージョン

Android**バージョン**セレクターは、デザイナーのレイアウトを表示するために使用される Android バージョンを設定します。 セレクターには、プロジェクトのターゲット フレームワークのバージョンと互換性があるすべてのバージョンが表示されます。


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Android のバージョンの一覧](resource-qualifiers-images/vs/17-android-version.png "一覧の Android バージョン")

プロジェクトの設定でターゲット フレームワークのバージョンを設定できます**プロパティ > アプリケーション > Android バージョンを使用してコンパイル**です。 ターゲット フレームワークのバージョンの詳細については、次を参照してください。 [Android API レベルの理解](~/android/app-fundamentals/android-api-levels.md)です。

ツールボックスで使用可能なウィジェットのセットは、プロジェクトのターゲット フレームワークのバージョンによって決定されます。 使用できるプロパティの場合は true。 これはまた、**プロパティ ウィンドウ**します。 ウィジェットの使用可能な一覧は*いない*で選択した値によって決定されます、**バージョン**ツールバーのセレクター。 たとえば、Android 4.4 にプロジェクトのターゲット バージョンを設定する場合は、Android 6.0 で、プロジェクトがどのように表示するツールバー バージョン セレクターで Android 6.0 を選択することもできますが、Android 6.0 に固有のウィジェットを追加することはできません&ndash; Android 4.4 で使用できるウィジェットに制限があります。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Android のバージョンの一覧](resource-qualifiers-images/xs/16-android-version-sml.png)](resource-qualifiers-images/xs/16-android-version.png)

プロジェクトの設定でターゲット フレームワークのバージョンを設定することができます、**プロジェクトのオプション > ビルド > 全般**セクションです。 ターゲット フレームワークのバージョンの詳細については、次を参照してください。 [Android API レベルの理解](~/android/app-fundamentals/android-api-levels.md)です。

ツールボックスで使用可能なウィジェットのセットは、プロジェクトのターゲット フレームワークのバージョンによって決定されます。 使用できるプロパティの場合は true。 これはまた、**プロパティ パッド**です。 ウィジェットの使用可能な一覧は*いない*で選択した値によって決定されます、**バージョン**ツールバーのセレクター。 たとえば、Android 4.4 にプロジェクトのターゲット バージョンを設定する場合は、Android 6.0 で、プロジェクトがどのように表示するツールバー バージョン セレクターで Android 6.0 を選択することもできますが、Android 6.0 に固有のウィジェットを追加することはできません&ndash; Android 4.4 で使用できるウィジェットに制限があります。

-----
