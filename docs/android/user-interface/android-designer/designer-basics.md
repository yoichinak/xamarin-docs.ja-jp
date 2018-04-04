---
title: デザイナーの基本
description: このトピックは、デザイナーの機能が導入されています、デザイナーを起動する方法について説明します、デザイン画面をについて説明します、およびプロパティ ペインを使用して、ウィジェットのプロパティを編集する方法について詳しく説明をします。
ms.prod: xamarin
ms.assetid: 48B20C9A-B2A2-AE82-76B2-A3C1E5A4050D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 6bac16a8ce9859e819299689489d9aad982c1f7f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="designer-basics"></a>デザイナーの基本

_このトピックは、デザイナーの機能が導入されています、デザイナーを起動する方法について説明します、デザイン画面をについて説明します、およびプロパティ ペインを使用して、ウィジェットのプロパティを編集する方法について詳しく説明をします。_


## <a name="launching-the-designer"></a>デザイナーを起動します。

デザイナーがレイアウトを作成すると、または既存の .axml ファイルをダブルクリックして起動できるときに自動的に起動します。 たとえば、ダブルクリックすると**Main.axml**で、**リソース > レイアウト**フォルダーは、次のように、デザイナーに読み込まれます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Visual Studio でのデザイナー画面](designer-basics-images/vs/01-open-designer-sml.png)](designer-basics-images/vs/01-open-designer.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Mac 用の Visual Studio でのデザイナー画面](designer-basics-images/xs/01-open-designer-sml.png)](designer-basics-images/xs/01-open-designer.png#lightbox)

-----


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

同様を右クリックして新しいレイアウトを追加することができます、**レイアウト**内のフォルダー、**ソリューション エクスプ ローラー**を選択して**追加 > 新しい項目 > Android レイアウト**:

[![新しい項目 ダイアログ ボックスを追加します。](designer-basics-images/vs/02-add-new-layout-sml.png)](designer-basics-images/vs/02-add-new-layout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

同様を右クリックして新しいレイアウトを追加することができます、**レイアウト**内のフォルダー、**ソリューション パッド**を選択して**追加 > 新しいファイル > Android > レイアウト**:

[![新しいファイル ダイアログ ボックスを追加します。](designer-basics-images/xs/02-add-new-layout-sml.png)](designer-basics-images/xs/02-add-new-layout.png#lightbox)

-----

これにより、新しい .axml ファイルを作成し、デザイン サーフェイスに読み込みます。



## <a name="designer-features"></a>デザイナーの機能

デザイナーでは、次のスクリーン ショットに示すように、いくつかのセクションでは、さまざまな機能をサポートするので構成されます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![デザイナー ウィンドウの図](designer-basics-images/vs/03-designer-features-sml.png)](designer-basics-images/vs/03-designer-features.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![デザイナー ウィンドウの図](designer-basics-images/xs/03-designer-features-sml.png)](designer-basics-images/xs/03-designer-features.png#lightbox)

-----

デザイナーのレイアウトを編集するときに、次の機能を使用する、設計の図形を作成しています。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   **デザイン画面**&ndash;のデバイスでレイアウトがどのように表示されるか、編集可能な表現を提供することで、視覚的に、ユーザー インターフェイスの作成を容易になります。

-   **ツールバー** &ndash;セレクターの一覧を表示:**デバイス**、**バージョン**、**テーマ**レイアウトの構成、およびアクションのバーの設定。 ツールバーには、テーマ エディターを起動するため、マテリアル デザイン グリッドを有効にするためのアイコンも含まれています。

-   **ツールボックス**&ndash;ウィジェットとドラッグしてデザイン サーフェイスにドロップしたレイアウトの一覧を示します。

-   **[プロパティ] ウィンドウ**&ndash;選択したウィジェットの表示および編集するためのプロパティを一覧表示します。

-   **ドキュメント アウトライン**&ndash;レイアウトを作成できるウィジェットのツリーが表示されます。 発生し、デザイナーで選択するツリー内の項目をクリックすることができます。 また、ツリー内の項目をクリックすると、[プロパティ] ウィンドウに、項目のプロパティを読み込みます。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   **デザイン画面**&ndash;のデバイスでレイアウトがどのように表示されるか、編集可能な表現を提供することで、視覚的に、ユーザー インターフェイスの作成を容易になります。

-   **ツールバー** &ndash;セレクターの一覧を表示:**デバイス**、**バージョン**、**テーマ**レイアウトの構成、およびアクションのバーの設定。 ツールバーには、テーマ エディターを起動するため、マテリアル デザイン グリッドを有効にするためのアイコンも含まれています。

-   **ツールボックス**&ndash;ウィジェットとドラッグしてデザイン サーフェイスにドロップしたレイアウトの一覧を示します。

-   **プロパティのパッド**&ndash;選択したウィジェットの表示および編集するためのプロパティを一覧表示します。

-   **ドキュメント アウトライン**&ndash;レイアウトを作成できるウィジェットのツリーが表示されます。 発生し、デザイナーで選択するツリー内の項目をクリックすることができます。 また、ツリー内の項目をクリックすると、プロパティ パッドにアイテムのプロパティを読み込みます。

-----



## <a name="toolbar"></a>ツール バー

ツールバー (デザイン サーフェイス上に配置) は、構成のセレクターとツールのメニューが表示されます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![デザイナーのツールバーのダイアグラム](designer-basics-images/vs/04-toolbar-sml.png)](designer-basics-images/vs/04-toolbar.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![デザイナーのツールバーのダイアグラム](designer-basics-images/xs/04-toolbar-sml.png)](designer-basics-images/xs/04-toolbar.png#lightbox)

-----


ツールバーには、次の機能にアクセスします。

-   **代替のレイアウト セレクター** &ndash;別のレイアウトのバージョンからを選択することができます。

-   **デバイスのセレクター** &ndash;画面のサイズ、解像度、およびキーボード可用性などの特定のデバイスに関連付けられている修飾子のセットを定義します。 追加し、新しいデバイスを削除することもできます。

-   **Android バージョン セレクター** &ndash;レイアウトが対象とする、Android のバージョン。 デザイナーは、選択されている Android バージョンに従ってレイアウトで表示されます。

-   **テーマのセレクター** &ndash;レイアウトの UI のテーマを選択します。

-   **構成セレクター** &ndash;など、デバイスの構成の選択*縦*または*ランドス ケープ*です。

-   **リソース修飾子のオプション**&ndash;を選択するためのドロップダウン メニューに表示するダイアログ ボックスを開きます*言語*、 *UI モード*、*夜モード*、および*Round 画面*オプション。

-   **操作バーの設定**&ndash;レイアウトの操作バーの設定を構成します。

-   **テーマ エディター** &ndash;開きます、*テーマ エディター*、選択したテーマの要素をカスタマイズするためにできるようになります。

-   **材料デザイン グリッド**&ndash;を有効または無効になります、*マテリアル デザイン グリッド*です。 マテリアルのデザイン グリッドに隣接するドロップダウン メニュー項目は、グリッドをカスタマイズすることができますのダイアログを開きます。

これらのトピックで詳しくは、これらの各機能について説明します。

[リソース修飾子と視覚エフェクト オプション](~/android/user-interface/android-designer/resource-qualifiers.md)に関する詳細な情報を提供、**デバイス セレクター**、 **Android バージョン セレクター**、**テーマ セレクター**、**構成セレクター**、**リソース資格オプション**、および**操作バー設定**です。

[レイアウト ビューの代替](~/android/user-interface/android-designer/alternative-layout-views.md)を使用する方法について説明します、**代替レイアウト セレクター**です。

[素材のデザイン機能](~/android/user-interface/android-designer/material-design-features.md)の包括的な概要を説明、**テーマ エディター**と**マテリアル デザイン グリッド**です。



## <a name="design-surface"></a>デザイン サーフェイス

デザイナーをドラッグし、ウィジェットをツールボックスからデザイン サーフェイスにドロップできます。 デザイナーでのウィジェットと対話するには (新しいウィジェットを追加するか、既存の位置の変更) して、使用可能な挿入ポイントをマークする垂直線と水平線が表示されます。 次の例では、新しい`Button`ウィジェットがデザイン画面にドラッグされています。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![デザイン サーフェイス上のカーソル行の例](designer-basics-images/vs/05-insertion-points-sml.png)](designer-basics-images/vs/05-insertion-points.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![デザイン サーフェイス上のカーソル行の例](designer-basics-images/xs/05-insertion-points-sml.png)](designer-basics-images/xs/05-insertion-points.png#lightbox)

-----

ウィジェットをさらに、コピーすることができます。 コピーを使用すると、ウィジェットをコピー、貼り付けをドラッグ アンド ドロップのキーを押しているときに既存のウィジェット、 <kbd>Ctrl</kbd>キー。


### <a name="context-menu-commands"></a>コンテキスト メニュー コマンド

コンテキスト メニューでは、デザイン サーフェイスと ドキュメント アウトラインの両方があります。 このメニューには、選択されたウィジェットと容易 (にくいものが常に、デザイン サーフェイス上の選択) をコンテナーでの操作を実行するため、コンテナーで利用可能なコマンドが表示されます。 コンテキスト メニューの例を次に示します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![デザイン サーフェイスを右クリックしたときの例のコンテキスト メニュー](designer-basics-images/vs/06-context-menu-sml.png)](designer-basics-images/vs/06-context-menu.png#lightbox)

この例では右クリックし、`TextView`いくつかのオプションを提供するコンテキスト メニューを開きます。

-   **LinearLayout** &ndash;を編集するためのサブメニューを開き、`LinearLayout`の親、`TextView`です。

-   **削除**、**コピー**、および**切り取り** &ndash; 、右クリックしたときに適用される操作`TextView`です。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![デザイン サーフェイスを右クリックしたときの例のコンテキスト メニュー](designer-basics-images/xs/06-context-menu-sml.png)](designer-basics-images/xs/06-context-menu.png#lightbox)

この例では右クリックし、`TextView`いくつかのオプションを提供するコンテキスト メニューを開きます。

-   **[相対レイアウト]** &ndash;を編集するためのサブメニューを開き、`RelativeLayout`の親、`TextView`です。

-   **LinearLayout** &ndash;を編集するためのサブメニューを開き、`LinearLayout`の親、`TextView`です。

-   **削除**、**コピー**、および**切り取り** &ndash; 、右クリックしたときに適用される操作`TextView`です。

-----

この例では右クリックし、`TextView`いくつかのオプションを提供するコンテキスト メニューを開きます。

-   **LinearLayout** &ndash;を編集するためのサブメニューを開き、`LinearLayout`の親、`TextView`です。

-   **削除**、**コピー**、および**切り取り** &ndash; 、右クリックしたときに適用される操作`TextView`です。



### <a name="zoom-controls"></a>ズーム コントロール

ように、いくつかのコントロールを使用してズーム、デザイン画面をサポートします。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![デザイン サーフェイスのズーム コントロールの図](designer-basics-images/vs/07-zoom-controls-sml.png)](designer-basics-images/vs/07-zoom-controls.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![デザイン サーフェイスのズーム コントロールの図](designer-basics-images/xs/07-zoom-controls-sml.png)](designer-basics-images/xs/07-zoom-controls.png#lightbox)

-----

これらのコントロールをやすくデザイナーのユーザー インターフェイスの特定の領域を参照してください。

-   **コンテナーを強調表示**&ndash;が拡大および縮小するときに見つけやすくなるように、デザイン画面上のコンテナーを強調表示します。

-   **標準サイズ**&ndash;選択したデバイスの解像度でレイアウトがどのように表示されるかを認識できるようにレイアウトのピクセルをレンダリングします。

-   **ウィンドウに合わせる**&ndash;全体のレイアウトがデザイン サーフェイスに表示されるように、ズーム レベルを設定します。

-   **ズーム イン**&ndash;ズームインします。 インクリメンタル クリックするたびに、レイアウトを拡大します。

-   **ズーム アウト**&ndash;増分ズームアウトしてクリックするたびに、デザイン サーフェイスに小さく表示され、レイアウトを作成します。

選択したズーム設定が実行時にアプリケーションのユーザー インターフェイスに影響を及ぼさないように注意してください。


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="property-pad"></a>プロパティの埋め込み

デザイナーは、ウィジェットのプロパティの編集をサポートしている、**プロパティ パッド**です。 に応じて、ウィジェットがデザイナー画面で選択したプロパティ パッドの変更時に表示されるプロパティです。 ときに、`Button`前の例では、選択すると、そのプロパティ`Button`ウィジェットが表示されます。

[![パッドのプロパティのスクリーン ショット](designer-basics-images/xs/08-property-pad-sml.png)](designer-basics-images/xs/08-property-pad.png#lightbox)

-----


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="properties-window"></a>[プロパティ] ウィンドウ

デザイナーは、ウィジェットのプロパティの編集をサポートしている、**プロパティ ウィンドウ**します。 プロパティ、プロパティ ウィンドウの変更に応じてウィジェットが選択されているデザイン サーフェイスに表示します。
ときに、`Button`前の例では、選択すると、そのプロパティ`Button`ウィジェットが表示されます。

![[プロパティ] ウィンドウのスクリーン ショット](designer-basics-images/vs/08-property-pad.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="property-pad-sections"></a>埋め込みのセクションではプロパティ

プロパティのパッドは同様のプロパティをグループ化するいくつかのセクションに分かれています&ndash;これにより、目的のプロパティを見つけやすく。

-   **ウィジェット**&ndash;ウィジェットのメイン プロパティなど`id`、 `visibility`、 `text`, などです。ウィジェットのコンテンツを管理するためのプロパティは通常は、ここに配置されます。

-   **スタイル**&ndash;など、ウィジェットの外観を変更するプロパティ`font`、 `text color`、 `background`, などです。

-   **レイアウト**&ndash;ウィジェットのサイズと場所を設定するプロパティです。

-   **スクロール**&ndash;プロパティをスクロールします。

-   **動作**&ndash;ウィジェットの動作を設定するフラグ。

-----



### <a name="default-values"></a>既定値

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

ほとんどのウィジェットのプロパティが空白になって、**プロパティ**ウィンドウの値が選択されている Android テーマから継承するためです。
**プロパティ**ウィンドウは、選択されたウィジェットを明示的に設定されている値にのみ表示されます。 テーマから継承されている値は表示されません。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

ほとんどのウィジェットのプロパティが空白になって、**プロパティ パッド**それらの値が選択されている Android テーマから継承するためです。 **プロパティ パッド**選択したウィジェットの明示的に設定されている値のみが表示されます。 テーマから継承されている値は表示されません。

-----


### <a name="referencing-resources"></a>リソースの参照

一部のプロパティは、レイアウト以外のファイルで定義されているリソースを参照できます**.axml**ファイル。 この型の場合、最も一般的な状況が`string`と`drawable`リソース。 ただし、参照も使用できますの他のリソースなど`Boolean`値およびディメンションです。
リソース参照、[参照] アイコンのときに対応するプロパティ (省略記号を&hellip;) プロパティに入力したテキストの横に表示されます。
このボタンは、クリックされたときに、リソース セレクターを開きます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

たとえば、次のスクリーン ショットを示しています使用可能なリソースのテキスト フィールドの右側にある省略記号をクリックすると、`Button`でウィジェット、**プロパティ**ウィンドウ。

[![例に示す 2 つのリソースのリソース スクリーン ショット](designer-basics-images/vs/09-resources-sml.png)](designer-basics-images/vs/09-resources.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

たとえば、次のスクリーン ショットを示しています使用可能なリソースのテキスト フィールドの右側にある省略記号をクリックすると、`Button`でウィジェット、**プロパティ パッド**:

[![例に示す 2 つのリソースのリソース スクリーン ショット](designer-basics-images/xs/09-resources-sml.png)](designer-basics-images/xs/09-resources.png#lightbox)

-----

次の例のリソースのセレクターを示しています、`Src`のプロパティ、 `ImageView`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![リソースのセレクター、ImageView のアイコン リソースを一覧表示します。](designer-basics-images/vs/10-src-resource-sml.png)](designer-basics-images/vs/10-src-resource.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![リソースのセレクター、ImageView のアイコン リソースを一覧表示します。](designer-basics-images/xs/10-src-resource-sml.png)](designer-basics-images/xs/10-src-resource.png#lightbox)

-----



### <a name="boolean-property-references"></a>ブール型プロパティの参照

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

*ブール*プロパティは、通常、プロパティ ウィンドウでプルダウン メニューから選択します。 選択することができます、`true`または`false`値、またはをクリックしてプロパティの参照を選択できます**リソースを選択しています.**.など、値を入力することができますも直接`true`または`false`です。

![ブール型プロパティを設定する例](designer-basics-images/vs/11-boolean.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

*ブール*プロパティが通常プロパティ パッドでチェック ボックスとして表示されます。 ときに、`Boolean`プロパティは、リソースの参照をサポートしている、プロパティの横に小さな チェック ボックスが表示されます。 チェックされたチェック ボックスを意味`true`および空のボックスの手段を`false`です。 など、値を入力することができますも直接`true`または`false`です。 マウスを動かすと、入力、小さいテキスト フィールドのアイコンが表示されます。 次の値を手動で入力する場合は、それをクリックすることができます。

[![ブール型プロパティを設定する例](designer-basics-images/xs/12-boolean-sml.png)](designer-basics-images/xs/12-boolean.png#lightbox)


## <a name="grouped-properties"></a>グループ化されたプロパティ

一部のウィジェットがグループ化する複数値プロパティがあります (など`Padding`など)。 これらのプロパティ値は、「、**プロパティ パッド**、展開可能な 1 つの行にします。 これらのプロパティの一部で編集できます直接、グループ化された行など、`Padding`次に示すプロパティ。

[![Padding プロパティの設定の例](designer-basics-images/xs/13-padding-property-sml.png)](designer-basics-images/xs/13-padding-property.png#lightbox)

-----


## <a name="editing-properties-inline"></a>プロパティをインラインの編集

Android デザイナーは、直接編集するデザイン サーフェイス上の特定のプロパティ (ため、これらのプロパティのプロパティの一覧で検索する必要はありません) をサポートします。 直接編集可能なプロパティには、テキスト、余白、およびサイズが含まれます。

### <a name="text"></a>テキスト

一部のウィジェットのテキストのプロパティ (など`Button`と`TextView`)、デザイン サーフェイスで直接編集できます。 ウィジェットをダブルクリックすると、入れて、編集モードでは、次のように。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![こんにちは文字列のテキスト リソース](designer-basics-images/vs/12-text-resource.png "テキスト リソース")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![こんにちは文字列のテキストのリソース](designer-basics-images/xs/14-text-resource-sml.png)](designer-basics-images/xs/14-text-resource.png#lightbox)

-----

新しいテキスト値を入力するか、新しいリソース文字列を入力することができます。 次の例で、`@string/hello`テキストで、置き換えられるリソース`CLICK THIS BUTTON`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Shift キーを押しながら、新しいリソースにテキストを自動的にリンクを入力してください。](designer-basics-images/vs/13-shift-enter-resource.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Shift キーを押しながら、新しいリソースにテキストを自動的にリンクを入力してください。](designer-basics-images/xs/15-shift-enter-resource-sml.png)](designer-basics-images/xs/15-shift-enter-resource.png#lightbox)

-----

この変更は、ウィジェットの`text`プロパティに割り当てられた値は変更されません、`@string/hello`リソース。
新しいテキスト文字列を入力するキーを押すと<kbd>shift キーを押し</kbd> +
<kbd>Enter</kbd>入力したテキストと、新しいリソースに自動的にリンクします。


### <a name="margin"></a>Margin

ウィジェットを選択すると、デザイナーには、サイズまたはウィジェットの余白を対話的に変更するためのハンドルが表示されます。 クリックすると、ウィジェットが選択されているときに切り替わります余白編集モード サイズ編集モードです。

ウィジェットを初めてクリックすると、余白ハンドルが表示されます。 デザイナーがハンドルを変更するプロパティを表示、ハンドルのいずれかにマウスを移動する場合 (次に示すように、`layout_marginLeft`プロパティ)。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![デザイナーで処理する余白を示すスクリーン ショット](designer-basics-images/vs/15-margin-handles.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![デザイナーで処理する余白を示すスクリーン ショット](designer-basics-images/xs/16-margin-handles-sml.png)](designer-basics-images/xs/16-margin-handles.png#lightbox)

-----

余白は既に設定されて、点線が表示されます、余白を占有する領域を示します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![ボタンの周囲のスペースをマーキング点線の例](designer-basics-images/vs/16-margins-set.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![ボタンの周囲のスペースをマーキング点線の例](designer-basics-images/xs/17-margins-set-sml.png)](designer-basics-images/xs/17-margins-set.png#lightbox)

-----



### <a name="size"></a>サイズ

前述のようにが既に選択されているときに、ウィジェットをクリックして、サイズ編集モードに切り替えることができます。 指定された次元のサイズを設定する三角形のハンドルをクリックして`wrap_content`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![ラップのコンテンツおよびサイズ変更ハンドル](designer-basics-images/vs/17-wrap-content.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![ラップのコンテンツおよびサイズ変更ハンドル](designer-basics-images/xs/18-wrap-content-sml.png)](designer-basics-images/xs/18-wrap-content.png#lightbox)

-----

クリックすると、**コンテンツのラップ**ハンドルがようにを超える囲まれたコンテンツをラップするために必要なディメンションにウィジェットを縮小します。 この例では、ボタンのテキストは次のスクリーン ショットに示すように水平方向に縮小されます。

サイズの値を設定すると**コンテンツをラップ**、デザイナーには、逆の方向にサイズを変更するポイント三角形のハンドルが表示されます`match_parent`:。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![一致する親ハンドル](designer-basics-images/vs/18-match-parent.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![一致する親ハンドル](designer-basics-images/xs/19-match-parent-sml.png)](designer-basics-images/xs/19-match-parent.png#lightbox)

-----

クリックすると、**一致親**ハンドルができるように、同じ親ウィジェットでは、その次元のサイズを復元します。

また、ドラッグできます循環サイズ変更ハンドルに示すように、上記のスクリーン ショット) ウィジェットに任意のサイズを変更する`dp`値。 こうと、両方とも**コンテンツをラップ**と**一致親**そのディメンションのハンドルが表示されます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![循環サイズ変更ハンドル](designer-basics-images/vs/19-resize-dp.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![循環サイズ変更ハンドル](designer-basics-images/xs/20-resize-dp-sml.png)](designer-basics-images/xs/20-resize-dp.png#lightbox)

-----

編集することはないすべてのコンテナー、`Size`ウィジェットのです。 たとえば、いることを確認では、次のスクリーン ショット、`LinearLayout`選択すると、サイズ変更ハンドルは表示されません。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![サイズ変更ハンドルのないです。](designer-basics-images/vs/20-no-resize-handles.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![サイズ変更ハンドルのないです。](designer-basics-images/xs/21-no-resize-handles-sml.png)](designer-basics-images/xs/20-no-resize-handles.png#lightbox)

-----



## <a name="document-outline"></a>[ドキュメント アウトライン]

**ドキュメント アウトライン**レイアウトのウィジェットの階層を表示します。
次の例では、含まれている`LinearLayout`ウィジェットが選択されています。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![ドキュメント アウトライン](designer-basics-images/vs/21-document-outline.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![ドキュメント アウトライン](designer-basics-images/xs/22-outline-view-sml.png)](designer-basics-images/xs/22-outline-view.png#lightbox)

-----

選択されているウィジェットのアウトライン (ここで、 `LinearLayout`) がデザイン画面でも強調表示されます。 ドキュメント アウトラインで選択したウィジェットがデザイン サーフェイス上の対応する同期します。 これは、グループの表示、常に簡単にデザイン サーフェイス上の選択ではないを選択するため便利です。

ドキュメント アウトラインは、コピーと貼り付けをサポートしています。 または、ドラッグ アンド ドロップすることができます。 およびドキュメント アウトラインにデザイン画面をデザイン画面に ドキュメント アウトラインからのドラッグ アンド ドロップはサポートされています。 また、ドキュメント アウトラインの項目を右クリックしてには、その項目 (その同じウィジェット、デザイン サーフェイスを右クリックしたときに表示されるコンテキスト メニュー) のコンテキスト メニューが表示されます。
