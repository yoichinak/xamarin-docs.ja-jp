---
title: リソース修飾子と視覚化オプション
description: このトピックでは、いくつかの修飾子の値が一致する場合にのみ使用されるリソースを定義する方法について説明します。 簡単な例は、文字列の言語で修飾されたリソースです。 その他の言語に使用する定義されているその他の代替のリソースで、既定値として文字列リソースを定義できます。 レイアウトの種類を含む、すべてのリソースの種類を修飾できます。
ms.prod: xamarin
ms.assetid: 2111C18A-3EDA-3787-25E1-3869FF4BE441
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
ms.openlocfilehash: 9d99e6a59b57b59d585b32befdadc0890d41448c
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115634"
---
# <a name="resource-qualifiers-and-visualization-options"></a>リソース修飾子と視覚化オプション

_このトピックでは、いくつかの修飾子の値が一致する場合にのみ使用されるリソースを定義する方法について説明します。簡単な例は、文字列の言語で修飾されたリソースです。その他の言語に使用する定義されているその他の代替のリソースで、既定値として文字列リソースを定義できます。レイアウトの種類を含む、すべてのリソースの種類を修飾できます。_


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="resource-qualifier-options"></a>リソース修飾子のオプション

**リソース修飾子のオプション**の右側にある省略記号アイコンをクリックしてアクセスできる、**ランドス ケープ**モード ボタンをクリックします。

[![リソース修飾子のオプション](resource-qualifiers-images/vs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/vs/08-resource-qual-opt.png#lightbox)

このダイアログ ボックスでは、次のリソース修飾子のプルダウン メニューが表示されます。

-   **言語**&ndash;利用可能な言語リソースを表示し、言語/地域の新しいリソースを追加するオプションを提供します。

-   **UI モード**&ndash;リストの表示モード (など**自動車ドック**と**デスク ドック**) とレイアウトの方向。

これらのプルダウン メニューには、新しいダイアログ ボックスを選択および (説明を次に) するように、リソース修飾子を構成できますが開きます。

### <a name="language"></a>言語

**言語**プルダウン メニューには、リソースが定義されている言語のみが表示されます (または**すべての言語**、既定値)。 ただしはまた、**言語/地域を追加しています.** オプションを一覧に新しい言語を追加することができます。

[![言語/地域を追加します。](resource-qualifiers-images/vs/09-add-language-region-sml.png)](resource-qualifiers-images/vs/09-add-language-region.png#lightbox)

クリックすると**言語/地域を追加しています.**、**言語の選択**使用可能な言語と地域のドロップダウン リストを表示するダイアログが表示されます。

![言語の一覧](resource-qualifiers-images/vs/10-languages.png "言語の一覧")

この例では選択しました**fr (フランス語)** 言語と**BE** (ベルギー) の地域の言語がフランス語の。 なお、**リージョン**フィールドは省略可能です、特定のリージョンに関係なく、多くの言語を指定できます。 ときに、**言語**プルダウン メニューをもう一度開くと、新しく追加された言語/地域のリソースが表示されます。

![言語および地域の選択](resource-qualifiers-images/vs/11-language-region-added.png "言語および地域の選択")

新しい言語を追加しても、表示は、不要になったに追加された言語は、次回の新しいリソースを作成しない場合は、プロジェクトが開くことに注意してください。

### <a name="ui-mode"></a>UI モード

クリックすると、 **UI モード**など、プルダウン メニュー モードの一覧が表示されます**標準**、**自動車ドック**、**デスク ドック**、 **テレビ**、**アプライアンス**、および**ウォッチ**:


[![UI モード メニュー](resource-qualifiers-images/vs/12-ui-mode-sml.png)](resource-qualifiers-images/vs/12-ui-mode.png#lightbox)

この一覧の下では、夜モード**ない夜**と**夜**レイアウトの方向と、その後**左から右に**と**右から左に**(について**左から右に**と**右から左に**オプションを参照してください[LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/))。
内の最後の項目、**リソース修飾子のオプション**ダイアログ ボックスは、**ラウンド画面**(用、Android Wear で使用) または**いませんラウンド画面**します。
Round と非ラウンド画面の詳細については、次を参照してください。[レイアウト](https://developer.android.com/training/wearables/ui/layouts.html)します。
Android の UI モードの詳細については、次を参照してください。 [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/)します。

## <a name="action-bar-settings"></a>アクション バーの設定

**アクション バーの設定**アイコンはペイント ブラシ (テーマ エディター) アイコンの左側に使用します。

![アクション バーの設定](resource-qualifiers-images/vs/14-action-bar.png "アクション バーの設定")

このアイコンは、次の 3 つの操作バー モードのいずれかから選択する方法を提供するダイアログ ポップ オーバーを開きます。

-   **標準**&ndash;ロゴまたはオプションの字幕で、アイコンとタイトルのテキストのいずれかで構成されます。

-   **リスト**&ndash;ナビゲーション モードの一覧。 このモード静的タイトルのテキストではなくアクティビティ内のナビゲーションのリスト メニューを表示します (つまり、表現できるドロップダウン リストとしてユーザーに)。

-   **タブ**&ndash;タブ ナビゲーション モード。 このモードは、静的なタイトルのテキストではなく、一連のアクティビティ内のナビゲーションのタブを表示します。

## <a name="themes"></a>テーマ

**テーマ**ドロップダウン メニューでは、すべてのプロジェクトで定義されているテーマが表示されます。 選択**テーマより**次に示すように、インストールされている Android SDK の使用可能なすべてのテーマの一覧がダイアログを開きます。

[![複数のテーマ一覧](resource-qualifiers-images/vs/15-theme-menu-sml.png "よりテーマ ボックスの一覧")](resource-qualifiers-images/vs/15-theme-menu.png#lightbox)

テーマを選択すると、デザイン画面が更新され、新しいテーマが反映されます。 この変更が行われた永続的なされてことに注意してください。 場合にのみ、 **OK**のボタンがクリックされた、**テーマ**ダイアログ。 テーマを選択すると後に、追加される、**テーマ**ドロップダウン メニューに示すように下。

![ライト テーマが利用できるようになりました](resource-qualifiers-images/vs/16-light-theme.png "ライト テーマが利用できるようになりました")

## <a name="android-version"></a>Android バージョン

Android**バージョン**セレクターは、デザイナーでレイアウトを表示するために使用される Android のバージョンを設定します。 セレクターは、プロジェクトのターゲット フレームワークのバージョンと互換性があるすべてのバージョンが表示されます。

![Android のバージョンの一覧](resource-qualifiers-images/vs/17-android-version.png "一覧の Android バージョン")

ターゲット フレームワークのバージョンは、プロジェクトの設定で設定できます**プロパティ > アプリケーション > Android バージョンを使用してコンパイル**します。 ターゲット フレームワークのバージョンの詳細については、次を参照してください。 [Understanding Android API Levels](~/android/app-fundamentals/android-api-levels.md)します。

ツールボックスで使用可能なウィジェットのセットは、プロジェクトのターゲット フレームワークのバージョンによって決定されます。 使用できるプロパティの場合は true。 これはまた、**プロパティ ウィンドウ**します。 ウィジェットの使用可能な一覧は*いない*で選択した値によって決定されます、**バージョン**ツールバーのセレクター。 たとえば、Android 4.4 をプロジェクトのターゲット バージョンを設定した場合で Android 6.0 では、プロジェクトがどのように表示するツールバー バージョン セレクターで Android 6.0 を選択できますが、Android 6.0 に固有のウィジェットを追加することはできません&ndash; Android 4.4 で使用できるウィジェットに制限があります。

リソースの種類の詳細については、次を参照してください。 [Android リソース](~/android/app-fundamentals/resources-in-android/index.md)します。



# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="resource-qualifier-options"></a>リソース修飾子のオプション

**リソース修飾子のオプション**の右側にある省略記号アイコンをクリックしてアクセスできる、**ランドス ケープ**モード ボタンをクリックします。

[![リソース修飾子のオプション](resource-qualifiers-images/xs/08-resource-qual-opt-m75-sml.png)](resource-qualifiers-images/xs/08-resource-qual-opt-m75.png#lightbox)

このダイアログ ボックスでは、次のリソース修飾子のプルダウン メニューが表示されます。

-   **言語**&ndash;利用可能な言語リソースを表示し、言語/地域の新しいリソースを追加するオプションを提供します。

-   **UI モード**&ndash;リストの表示モード (など**自動車ドック**と**デスク ドック**) とレイアウトの方向。

これらのプルダウン メニューには、新しいダイアログ ボックスを選択および (説明を次に) するように、リソース修飾子を構成できますが開きます。

### <a name="language"></a>言語

**言語**プルダウン メニューには、リソースが定義されている言語のみが表示されます (または**すべての言語**、既定値)。 ただしはまた、**言語/地域を追加しています.** オプションを一覧に新しい言語を追加することができます。

[![言語/地域を追加します。](resource-qualifiers-images/xs/09-add-language-region-m75-sml.png)](resource-qualifiers-images/xs/09-add-language-region-m75.png#lightbox)

クリックすると**言語/地域を追加しています.**、**言語の選択**使用可能な言語と地域のドロップダウン リストを表示するダイアログが表示されます。

[![言語の一覧](resource-qualifiers-images/xs/10-languages-m75-sml.png)](resource-qualifiers-images/xs/10-languages-m75.png#lightbox)

この例では選択しました**fr (フランス語)** 言語と**BE** (ベルギー) の地域の言語がフランス語の。 なお、**リージョン**フィールドは省略可能です、特定のリージョンに関係なく、多くの言語を指定できます。 ときに、**言語**プルダウン メニューをもう一度開くと、新しく追加された言語/地域のリソースが表示されます。

[![言語および地域の選択](resource-qualifiers-images/xs/11-language-region-added-m75-sml.png)](resource-qualifiers-images/xs/11-language-region-added-m75.png#lightbox)

新しい言語を追加しても、表示は、不要になったに追加された言語は、次回の新しいリソースを作成しない場合は、プロジェクトが開くことに注意してください。

### <a name="ui-mode"></a>UI モード

クリックすると、 **UI モード**など、プルダウン メニュー モードの一覧が表示されます**標準**、**自動車ドック**、**デスク ドック**、 **テレビ**、**アプライアンス**、および**ウォッチ**:

[![UI モード メニュー](resource-qualifiers-images/xs/12-ui-mode-m75-sml.png)](resource-qualifiers-images/xs/12-ui-mode-m75.png#lightbox)

この一覧の下では、夜モード**ない夜**と**夜**レイアウトの方向と、その後**左から右に**と**右から左に**します。 オプションの最後の組み合わせでは、いずれかを選択できます。**ラウンド画面**または**四角形の画面**(デバイスの Android Wear のに便利です)。

Android の UI モードの詳細については、次を参照してください。 [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/)します。
について**左から右に**と**右から左に**オプションを参照してください[LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/)します。


## <a name="action-bar-settings"></a>アクション バーの設定

**アクション バーの設定**アイコンはペイント ブラシ (テーマ エディター) アイコンの左側に使用します。

[![アクション バーの設定](resource-qualifiers-images/xs/13-action-bar-m75-sml.png)](resource-qualifiers-images/xs/13-action-bar-m75.png#lightbox)

このアイコンは、次の 3 つの操作バー モードのいずれかから選択する方法を提供するダイアログ ポップ オーバーを開きます。

-   **標準**&ndash;ロゴまたはアイコンとタイトルのいずれかのテキストとオプションのサブタイトルので構成されています。

-   **リスト**&ndash;ナビゲーション モードの一覧。 このモード静的タイトルのテキストではなくアクティビティ内のナビゲーションのリスト メニューを表示します (つまり、表現できるドロップダウン リストとしてユーザーに)。

-   **タブ**&ndash;タブ ナビゲーション モード。 このモードは、静的なタイトルのテキストではなく、一連のアクティビティ内のナビゲーションのタブを表示します。

## <a name="themes"></a>テーマ

**テーマ**ドロップダウン メニューでは、すべてのプロジェクトで定義されているテーマが表示されます。 選択**テーマより**次に示すように、インストールされている Android SDK の使用可能なすべてのテーマの一覧がダイアログを開きます。

[![複数のテーマのリスト](resource-qualifiers-images/xs/14-theme-menu-m75-sml.png)](resource-qualifiers-images/xs/14-theme-menu-m75.png#lightbox)

テーマを選択すると、デザイン画面が更新され、新しいテーマが反映されます。 この変更が行われた永続的なされてことに注意してください。 場合にのみ、 **OK**のボタンがクリックされた、**テーマ**ダイアログ。 テーマを選択すると後に、追加される、**テーマ**ドロップダウン メニューに示すように下。

[![ライト テーマが利用できるようになりました](resource-qualifiers-images/xs/15-light-theme-m75-sml.png)](resource-qualifiers-images/xs/15-light-theme-m75.png#lightbox)

## <a name="android-version"></a>Android バージョン

Android**バージョン**セレクターは、デザイナーでレイアウトを表示するために使用される Android のバージョンを設定します。 セレクターは、プロジェクトのターゲット フレームワークのバージョンと互換性があるすべてのバージョンが表示されます。

[![Android のバージョンの一覧](resource-qualifiers-images/xs/16-android-version-m75-sml.png)](resource-qualifiers-images/xs/16-android-version-m75.png#lightbox)

プロジェクトの設定でターゲット フレームワークのバージョンを設定することができます、**プロジェクト オプション > ビルド > 全般**セクション。 ターゲット フレームワークのバージョンの詳細については、次を参照してください。 [Understanding Android API Levels](~/android/app-fundamentals/android-api-levels.md)します。

ツールボックスで使用可能なウィジェットのセットは、プロジェクトのターゲット フレームワークのバージョンによって決定されます。 使用できるプロパティの場合は true。 これはまた、**プロパティ パッド**します。 ウィジェットの使用可能な一覧は*いない*で選択した値によって決定されます、**バージョン**ツールバーのセレクター。 たとえば、Android 4.4 をプロジェクトのターゲット バージョンを設定した場合で Android 6.0 では、プロジェクトがどのように表示するツールバー バージョン セレクターで Android 6.0 を選択できますが、Android 6.0 に固有のウィジェットを追加することはできません&ndash; Android 4.4 で使用できるウィジェットに制限があります。

リソースの種類の詳細については、次を参照してください。 [Android リソース](~/android/app-fundamentals/resources-in-android/index.md)します。

-----
