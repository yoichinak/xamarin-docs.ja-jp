---
title: リソース修飾子と視覚化オプション
description: このトピックでは、いくつかの修飾子の値が一致する場合にのみ使用されるリソースを定義する方法について説明します。 簡単な例は、文字列の言語で修飾されたリソースです。 その他の言語に使用する定義されているその他の代替のリソースで、既定値として文字列リソースを定義できます。 レイアウトの種類を含む、すべてのリソースの種類を修飾できます。
ms.prod: xamarin
ms.assetid: 2111C18A-3EDA-3787-25E1-3869FF4BE441
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/29/2018
ms.openlocfilehash: 7bc8c1066e557085c1bf34f77765edbb2259ba7a
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403300"
---
# <a name="resource-qualifiers-and-visualization-options"></a>リソース修飾子と視覚化オプション

_このトピックでは、いくつかの修飾子の値が一致する場合にのみ使用されるリソースを定義する方法について説明します。簡単な例は、文字列の言語で修飾されたリソースです。その他の言語に使用する定義されているその他の代替のリソースで、既定値として文字列リソースを定義できます。レイアウトの種類を含む、すべてのリソースの種類を修飾できます。_


## <a name="custom-device-configurations"></a>カスタムのデバイスの構成

Android は、多種多様なデバイスと画面解像度でご確認いただけます。
デザイン ユーザー インターフェイスを多数のデバイスを対象とするために、デザイナー、さまざまなデバイス構成が組み込まれていますが付属します。 その他のデバイスの構成を追加することもサポートされます。これらの構成に基づく*修飾子*別のデバイスの構成を 1 つを区別するために指定することです。 修飾子のさまざまな種類があります。 これらのリソースの種類の詳細については、次を参照してください。 [Android リソース](~/android/app-fundamentals/resources-in-android/index.md)します。

メニューは、デバイス セレクターの下部にある、**カスタマイズ**次に示すようにオプションします。


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![デバイス セレクター メニュー](resource-qualifiers-images/vs/01-device-selector-sml.png)](resource-qualifiers-images/vs/01-device-selector.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![デバイス セレクター メニュー](resource-qualifiers-images/xs/01-device-selector-sml.png)](resource-qualifiers-images/xs/01-device-selector.png#lightbox)

-----


選択すると**カスタマイズ**ダイアログを表示する使用可能なデバイス構成を参照するために使用できます。 クリックすると、**デバイス定義** タブで、すべての既知のデバイス定義の一覧が表示されます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![AVD Manager](resource-qualifiers-images/vs/02-device-definitions-sml.png)](resource-qualifiers-images/vs/02-device-definitions.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![AVD Manager](resource-qualifiers-images/xs/02-device-definitions-sml.png)](resource-qualifiers-images/xs/02-device-definitions.png#lightbox)

-----


デザイナーで事前構成済みのデバイスを変更することはできません。 ただし、クリックして**デバイスの作成.** カスタム デバイス定義を定義します。 または、既存の定義を選択し、クリックして**複製しています.** に新しい定義の開始点として使用します。
たとえばを選択すると、 **Nexus 5**定義とクリックすると**複製しています.** 次のダイアログ ボックスを表示します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![デバイスを複製します。](resource-qualifiers-images/vs/03-clone-sml.png)](resource-qualifiers-images/vs/03-clone.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![デバイスを複製します。](resource-qualifiers-images/xs/03-clone-sml.png)](resource-qualifiers-images/xs/03-clone.png#lightbox)

-----


次のスクリーン ショットでは名前に変更が**Nexus 5 Custom**デバイス パラメーターを変更して、新しいカスタム デバイス定義を作成しているとします。 この例で**縦**デバイス定義がランドス ケープ専用ようには無効になります。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![カスタム デバイス](resource-qualifiers-images/vs/04-custom-sml.png)](resource-qualifiers-images/vs/04-custom.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![カスタム デバイス](resource-qualifiers-images/xs/04-custom-sml.png)](resource-qualifiers-images/xs/04-custom.png#lightbox)

-----


**[Clone Device]\(デバイスの複製\)** をクリックすると、新しいデバイス定義が作成され、次のように **[Device Definitions]\(デバイス定義\)** リストに表示されます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![更新されたデバイスの定義](resource-qualifiers-images/vs/05-updated-device-definitions-sml.png)](resource-qualifiers-images/vs/05-updated-device-definitions.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![更新されたデバイスの定義](resource-qualifiers-images/xs/05-updated-device-definitions-sml.png)](resource-qualifiers-images/xs/05-updated-device-definitions.png#lightbox)

-----


上記の緑のアイコンで各デバイスのユーザーが作成した定義が表示されることに注意してください。 戻ると、**デバイス**セレクター メニュー、新しいカスタム デバイス定義が表示される一覧の最上位のセクションでは (カスタム デバイス構成には、この一覧で、IDE を再起動してみてくださいが表示されない) 場合。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![カスタムのデバイスがデバイスの一覧が表示されます。](resource-qualifiers-images/vs/06-nexus-5-custom-sml.png)](resource-qualifiers-images/vs/06-nexus-5-custom.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![カスタムのデバイスがデバイスの一覧が表示されます。](resource-qualifiers-images/xs/06-nexus-5-custom-sml.png)](resource-qualifiers-images/xs/06-nexus-5-custom.png#lightbox)

-----


このデバイスの構成を選択する前 (この場合、ランドス ケープ専用モード) で作成したカスタマイズに準拠するレイアウトを変更します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![使用中のカスタムのデバイス](resource-qualifiers-images/vs/07-custom-in-use-sml.png)](resource-qualifiers-images/vs/07-custom-in-use.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![使用中のカスタムのデバイス](resource-qualifiers-images/xs/07-custom-in-use-sml.png)](resource-qualifiers-images/xs/07-custom-in-use.png#lightbox)

-----



## <a name="resource-qualifier-options"></a>リソース修飾子のオプション

**リソース修飾子のオプション**の右側に 3 つのドットをクリックしてアクセスできる、**デバイス構成**オプション。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![リソース修飾子のオプション](resource-qualifiers-images/vs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/vs/08-resource-qual-opt.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![リソース修飾子のオプション](resource-qualifiers-images/xs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/xs/08-resource-qual-opt.png#lightbox)

-----


このダイアログ ボックスでは、次のリソース修飾子のプルダウン メニューが表示されます。

-   **言語**&ndash;利用可能な言語リソースを表示し、言語/地域の新しいリソースを追加するオプションを提供します。

-   **UI モード**&ndash;リストの表示モード (など**自動車ドック**と**デスク ドック**) とレイアウトの方向。

これらのプルダウン メニューには、新しいダイアログ ボックスを選択および (説明を次に) するように、リソース修飾子を構成できますが開きます。



### <a name="language"></a>言語

**言語**プルダウン メニューには、リソースが定義されている言語のみが表示されます (または**すべての言語**、既定値)。 ただしはまた、**言語/地域を追加しています.** オプションを一覧に新しい言語を追加することができます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![言語/地域を追加します。](resource-qualifiers-images/vs/09-add-language-region-sml.png)](resource-qualifiers-images/vs/09-add-language-region.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![言語/地域を追加します。](resource-qualifiers-images/xs/09-add-language-region-sml.png)](resource-qualifiers-images/xs/09-add-language-region.png#lightbox)

-----


クリックすると**言語/地域を追加しています.**、**言語の選択**使用可能な言語と地域のドロップダウン リストを表示するダイアログが表示されます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![言語の一覧](resource-qualifiers-images/vs/10-languages.png "言語の一覧")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![言語の一覧](resource-qualifiers-images/xs/10-languages-sml.png)](resource-qualifiers-images/xs/10-languages.png#lightbox)

-----


この例では選択しました**fr (フランス語)** 言語と**BE** (ベルギー) の地域の言語がフランス語の。 なお、**リージョン**フィールドは省略可能です、特定のリージョンに関係なく、多くの言語を指定できます。 ときに、**言語**プルダウン メニューをもう一度開くと、新しく追加された言語/地域のリソースが表示されます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![言語および地域の選択](resource-qualifiers-images/vs/11-language-region-added.png "言語および地域の選択")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![言語および地域の選択](resource-qualifiers-images/xs/11-language-region-added-sml.png)](resource-qualifiers-images/xs/11-language-region-added.png#lightbox)

-----


新しい言語を追加しても、表示は、不要になったに追加された言語は、次回の新しいリソースを作成しない場合は、プロジェクトが開くことに注意してください。



### <a name="ui-mode"></a>UI モード

クリックすると、 **UI モード**など、プルダウン メニュー モードの一覧が表示されます**標準**、**自動車ドック**、**デスク ドック**、 **テレビ**、**アプライアンス**、および**ウォッチ**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![UI モード メニュー](resource-qualifiers-images/vs/12-ui-mode-sml.png)](resource-qualifiers-images/vs/12-ui-mode.png#lightbox)

この一覧の下では、夜モード**ない夜**と**夜**レイアウトの方向と、その後**左から右に**と**右から左に**(について**左から右に**と**右から左に**オプションを参照してください[LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/)します。
最後の項目、**リソース修飾子のオプション**ダイアログ ボックスは、**ラウンド画面**(Android Wear を使用) するためまたは**いませんラウンド画面**(ラウンドについては、非ラウンド画面を参照してください[レイアウト](https://developer.android.com/training/wearables/ui/layouts.html))。
Android の UI モードの詳細については、次を参照してください。 [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/)します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![UI モード メニュー](resource-qualifiers-images/xs/12-ui-mode-sml.png)](resource-qualifiers-images/xs/12-ui-mode.png#lightbox)

この一覧の下では、夜モード**ない夜**と**夜**レイアウトの方向と、その後**左から右に**と**右から左に**します。 Android の UI モードの詳細については、次を参照してください。 [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/)します。
について**左から右に**と**右から左に**オプションを参照してください[LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/)します。

### <a name="round-screen"></a>ラウンド画面

最後の項目、**リソース修飾子のオプション**ダイアログは、**ラウンド画面**メニュー。 このメニューでは、いずれかを選択できます**ラウンド画面**(用、Android Wear で使用) または**四角形の画面**:。

[![ラウンド画面メニュー](resource-qualifiers-images/xs/13-round-screen-sml.png)](resource-qualifiers-images/xs/13-round-screen.png#lightbox)

-----



## <a name="action-bar-settings"></a>アクション バーの設定

**アクション バーの設定**アイコンはペイント ブラシ (テーマ エディター) アイコンの左側に使用します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![アクション バーの設定](resource-qualifiers-images/vs/14-action-bar.png "アクション バーの設定")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![アクション バーの設定](resource-qualifiers-images/xs/13b-action-bar-sml.png)](resource-qualifiers-images/xs/13b-action-bar.png#lightbox)

-----


このアイコンは、次の 3 つの操作バー モードのいずれかから選択する方法を提供するダイアログ ポップ オーバーを開きます。

-   **標準**&ndash;ロゴまたはアイコンとタイトルのいずれかのテキストとオプションのサブタイトルので構成されています。

-   **リスト**&ndash;ナビゲーション モードの一覧。 このモード静的タイトルのテキストではなくアクティビティ内のナビゲーションのリスト メニューを表示します (つまり、表現できるドロップダウン リストとしてユーザーに)。

-   **タブ**&ndash;タブ ナビゲーション モード。 このモードは、静的なタイトルのテキストではなく、一連のアクティビティ内のナビゲーションのタブを表示します。



## <a name="themes"></a>テーマ

**テーマ**ドロップダウン メニューでは、すべてのプロジェクトで定義されているテーマが表示されます。 選択**テーマより**次に示すように、インストールされている Android SDK の使用可能なすべてのテーマの一覧がダイアログを開きます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![複数のテーマ一覧](resource-qualifiers-images/vs/15-theme-menu-sml.png "よりテーマ ボックスの一覧")](resource-qualifiers-images/vs/15-theme-menu.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![複数のテーマのリスト](resource-qualifiers-images/xs/14-theme-menu-sml.png)](resource-qualifiers-images/xs/14-theme-menu.png#lightbox)

-----


テーマを選択すると、デザイン画面が更新され、新しいテーマが反映されます。 この変更が行われた永続的なされてことに注意してください。 場合にのみ、 **OK**のボタンがクリックされた、**テーマ**ダイアログ。 テーマを選択すると後に、追加される、**テーマ**ドロップダウン メニューに示すように下。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![ライト テーマが利用できるようになりました](resource-qualifiers-images/vs/16-light-theme.png "ライト テーマが利用できるようになりました")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![ライト テーマが利用できるようになりました](resource-qualifiers-images/xs/15-light-theme-sml.png)](resource-qualifiers-images/xs/15-light-theme.png#lightbox)

-----



## <a name="android-version"></a>Android バージョン

Android**バージョン**セレクターは、デザイナーでレイアウトを表示するために使用される Android のバージョンを設定します。 セレクターは、プロジェクトのターゲット フレームワークのバージョンと互換性があるすべてのバージョンが表示されます。


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Android のバージョンの一覧](resource-qualifiers-images/vs/17-android-version.png "一覧の Android バージョン")

ターゲット フレームワークのバージョンは、プロジェクトの設定で設定できます**プロパティ > アプリケーション > Android バージョンを使用してコンパイル**します。 ターゲット フレームワークのバージョンの詳細については、次を参照してください。 [Understanding Android API Levels](~/android/app-fundamentals/android-api-levels.md)します。

ツールボックスで使用可能なウィジェットのセットは、プロジェクトのターゲット フレームワークのバージョンによって決定されます。 使用できるプロパティの場合は true。 これはまた、**プロパティ ウィンドウ**します。 ウィジェットの使用可能な一覧は*いない*で選択した値によって決定されます、**バージョン**ツールバーのセレクター。 たとえば、Android 4.4 をプロジェクトのターゲット バージョンを設定した場合で Android 6.0 では、プロジェクトがどのように表示するツールバー バージョン セレクターで Android 6.0 を選択できますが、Android 6.0 に固有のウィジェットを追加することはできません&ndash; Android 4.4 で使用できるウィジェットに制限があります。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Android のバージョンの一覧](resource-qualifiers-images/xs/16-android-version-sml.png)](resource-qualifiers-images/xs/16-android-version.png#lightbox)

プロジェクトの設定でターゲット フレームワークのバージョンを設定することができます、**プロジェクト オプション > ビルド > 全般**セクション。 ターゲット フレームワークのバージョンの詳細については、次を参照してください。 [Understanding Android API Levels](~/android/app-fundamentals/android-api-levels.md)します。

ツールボックスで使用可能なウィジェットのセットは、プロジェクトのターゲット フレームワークのバージョンによって決定されます。 使用できるプロパティの場合は true。 これはまた、**プロパティ パッド**します。 ウィジェットの使用可能な一覧は*いない*で選択した値によって決定されます、**バージョン**ツールバーのセレクター。 たとえば、Android 4.4 をプロジェクトのターゲット バージョンを設定した場合で Android 6.0 では、プロジェクトがどのように表示するツールバー バージョン セレクターで Android 6.0 を選択できますが、Android 6.0 に固有のウィジェットを追加することはできません&ndash; Android 4.4 で使用できるウィジェットに制限があります。

-----
