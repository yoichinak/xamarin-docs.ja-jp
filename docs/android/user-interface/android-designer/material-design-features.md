---
title: 素材のデザイン機能
description: このトピックでは、材料設計に準拠したレイアウトを作成する開発者向けのより簡単にするデザイナーの機能について説明します。 このセクションでは、紹介し、資料グリッド、マテリアルのカラー パレット、傍点を文字のスケールおよびテーマ エディターを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: AC55E1B2-C239-4019-B0C3-A16F6CF0D6E0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: a764efe7f2cadd8c777f8427c0220e45eec662c9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30773632"
---
# <a name="material-design-features"></a>素材のデザイン機能

_このトピックでは、材料設計に準拠したレイアウトを作成する開発者向けのより簡単にするデザイナーの機能について説明します。このセクションでは、紹介し、資料グリッド、マテリアルのカラー パレット、傍点を文字のスケールおよびテーマ エディターを使用する方法について説明します。_


> [!Video https://youtube.com/embed/E3_ZjIOzVzY]

**2016 を展開します。 すべてのユーザーを作成できますすばらしいアプリ素材の設計と**

## <a name="overview"></a>概要

Xamarin.Android デザイナーには、材料、設計に準拠しないレイアウトを作成するためのより簡単にする機能が含まれています。 マテリアルのデザインに慣れていない場合は、次を参照してください。、[デザインの概要の資料](https://www.google.com/design/spec/material-design/introduction.html)です。

このガイドで必要がある、次のデザイナー機能を見る。

-   *材料グリッド*&ndash;グリッド、間隔、およびマテリアルのデザイン ガイドラインに従ってレイアウト ウィジェットを配置するための主線を表示するデザイン サーフェイスにオーバーレイします。

-   *素材のカラー パレット*&ndash;する公式の資料デザイン パレットから色を選択するために役立ちます A プロパティ パッド ダイアログ。

-   *傍点を文字スケール* &ndash; A プロパティ パッド ダイアログ用の材料デザイン準拠設定の選択肢を提供する、`textAppearance`テキスト フィールドのプロパティです。

-   *テーマ エディター* &ndash;できるようにする小さなカラー リソース エディター テーマのサブセットの色の情報を設定します。 たとえば、プレビューおよびなどマテリアルの色を変更できます`colorPrimary`、 `colorPrimaryDark`、および`colorAccent`です。

これらの各機能確認がおされその使用方法の例を示します。



## <a name="material-design-grid"></a>材料デザイン グリッド

素材デザイン グリッド メニューは、デザイナーの上部にあるツールバーから利用。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![材料デザイン グリッド](material-design-features-images/vs/01-material-design-grid-sml.png)](material-design-features-images/vs/01-material-design-grid.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![材料デザイン グリッド](material-design-features-images/xs/01-material-design-grid-sml.png)](material-design-features-images/xs/01-material-design-grid.png#lightbox)

-----

素材デザイン グリッド アイコンをクリックすると、デザイナーがデザイン サーフェイスを次の要素を含むオーバーレイが表示されます。

-   主線 (オレンジ色の線)

-   間隔 (緑の領域)

-   グリッド (青い線)

これらの要素は、次のスクリーン ショットに表示されることができます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![断裁、間隔、およびグリッド](material-design-features-images/vs/02-grid-and-keylines-sml.png)](material-design-features-images/vs/02-grid-and-keylines.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![断裁、間隔、およびグリッド](material-design-features-images/xs/02-grid-and-keylines-sml.png)](material-design-features-images/xs/02-grid-and-keylines.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

これらの各オーバーレイ項目は構成可能です。 素材デザイン グリッド メニューの横にある省略記号ボタンをクリックするとダイアログ重なって開きます、グリッドの有効/無効にする主線の配置を構成し、間隔を設定することができます。 すべての値で表現される注`dp`(密度に依存しないピクセル単位)。

![グリッド、断裁、および間隔の構成](material-design-features-images/vs/03-grid-configuration.png)

新しい断裁を追加するには、新しいオフセット値を入力、**オフセット**ボックスで、場所を選択 (**左**、**上部**、**右**、または**下部にある**) をクリックし、+ 新しい断裁を追加するアイコン。

同様に、新しい間隔を追加する (dp) 内のオフセットとサイズに入力、**サイズ**と**オフセット**ボックスに、それぞれします。 場所を選択 (**左**、**上部**、**右**、または**下部**) をクリックし、+ 新しい間隔を追加する アイコン。

これらの構成値を変更するとレイアウトの XML ファイルに保存され、レイアウトを再び開くときに再利用します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

これらの各オーバーレイ項目は構成可能です。 素材デザイン グリッド メニューの横にある下向きの三角形をクリックするとダイアログ重なって開きます、グリッドの有効/無効にする主線の配置を構成し、間隔を設定することができます。 すべての値で表現される注`dp`(密度に依存しないピクセル単位)。

[![グリッド、断裁、および間隔の構成](material-design-features-images/xs/03-grid-configuration-sml.png)](material-design-features-images/xs/03-grid-configuration.png#lightbox)

新しい断裁を追加するには、新しいオフセット値を入力、**オフセット**ボックスで、場所を選択 (**左**、**上部**、**右**、または**下部にある**) をクリックし、+ 新しい断裁を追加するアイコン。

同様に、新しい間隔を追加する (dp) 内のオフセットとサイズに入力、**サイズ**と**オフセット**ボックスに、それぞれします。 場所を選択 (**左**、**上部**、**右**、または**下部**) をクリックし、+ 新しい間隔を追加する アイコン。

これらの構成値を変更するとレイアウトの XML ファイルに保存され、レイアウトを再び開くときに再利用します。


## <a name="material-design-color-palette"></a>材料デザイン カラー パレット

すべてのプロパティ パネル項目を色を受け付けるようになりましたには、このスクリーン ショットに示すようを開くには、マテリアル デザインのカラー パレットを使用するその他のアイコンがあります。

[![色のアイコン](material-design-features-images/xs/04-new-color-icon-sml.png)](material-design-features-images/xs/04-new-color-icon.png#lightbox)

このアイコンをクリックするとダイアログ重なってが開きますマテリアル デザインのカラー パレットからそのプロパティの色を構成するためにできるようになります。

[![素材のデザインのカラー パレット](material-design-features-images/xs/05-material-palette-sml.png)](material-design-features-images/xs/05-material-palette.png#lightbox)

カラー パレットの上部では、パレットの下部に、選択した色の色合いの範囲が表示されますが、プライマリ マテリアル デザインの色が表示されます。 たとえば、選択**Indigo**、一連の**Indigo**色合いがダイアログの下部に表示されます。
色合いを選択するときに、プロパティの色が選択された色合いに変更されます。 次の例で、`Background Tint`ボタンの変更は*Indigo 500*:

[![Indigo 500 を選択します。](material-design-features-images/xs/06-indigo-sml.png)](material-design-features-images/xs/06-indigo.png#lightbox)

`Background Tint` カラー コードに設定されている*Indigo 500* (`#ff3f51b5`)、し、デザイナーは、この変更を反映するように、ボタンの背景色を更新します。

[![背景の濃淡の変化](material-design-features-images/xs/07-background-tint-sml.png)](material-design-features-images/xs/07-background-tint.png#lightbox)

マテリアルのデザインのカラー パレットの詳細については、マテリアル デザインを参照してください。[色パレット ガイド](http://www.google.com/design/spec/style/color.html#color-color-palette)です。

## <a name="typographic-scale"></a>傍点を文字のスケール

**テキストの外観**のセクション、**プロパティ**パッド**スタイル**タブにできるようにするアイコンから選択して、`TextAppearance`マテリアル設計に準拠しているスタイル指定:

[![[スタイル] タブ](material-design-features-images/xs/08-typo-scale-icon-sml.png)](material-design-features-images/xs/08-typo-scale-icon.png#lightbox)

このアイコンをクリックすると開きます、**傍点を文字のスケール**ダイアログ重なっては、一覧から選択できる構成済みのテキストのスタイルを表します。

[![テキスト スタイルの選択](material-design-features-images/xs/09-text-appearance-sml.png)](material-design-features-images/xs/09-text-appearance.png#lightbox)

クリックすると、次の例で**ディスプレイ 1**のより大きなフォントにボタンのテキストを変更**ディスプレイ 1**:

[![1 のスタイルを表示します。](material-design-features-images/xs/10-display-1-sml.png)](material-design-features-images/xs/10-display-1.png#lightbox)

テキストのスタイル、**傍点を文字スケール**に従って、**テーマ**設定します。 たとえば場合、**ライト**テーマは、デザイナーの使用可能なテキストのスタイルのミラーの一覧で選択された、**ライト**テーマ。

[![ライト テーマ](material-design-features-images/xs/11-light-theme-sml.png)](material-design-features-images/xs/11-light-theme.png#lightbox)

-----



## <a name="theme-editor"></a>テーマ エディター

**テーマ エディター**テーマ属性のサブセットの色の情報をカスタマイズすることができます。 開くには、**テーマ エディター**ツールバーで、ペイント ブラシ アイコンをクリックします。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![テーマ エディター アイコン](material-design-features-images/vs/04-theme-editor-icon.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![テーマ エディター アイコン](material-design-features-images/xs/12a-theme-editor-icon-sml.png)](material-design-features-images/xs/12a-theme-editor-icon.png#lightbox)

-----

ただし、**テーマ エディター**はツールバーからアクセスできるすべてのターゲットの Android バージョンおよび API レベルを次に示す機能のサブセットがある場合は、ターゲット API レベルは API 21 (Android 5.0 より前ロリポップ)。

左側のパネル、**テーマ エディター**現在選択されているテーマを構成する色の一覧が表示されます (この例では使用、 `Default Theme`)。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![テーマ エディター](material-design-features-images/vs/05-theme-editor-sml.png)](material-design-features-images/vs/05-theme-editor.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![テーマ エディター](material-design-features-images/xs/12b-theme-editor-sml.png)](material-design-features-images/xs/12b-theme-editor.png#lightbox)

-----

選択すると、色が左側、右側のパネルは、その色を編集するための次のタブを提供します。

-   **継承**&ndash;選択した色のスタイルの継承図を表示し、解決済みの色とそのテーマの色に割り当てられた色コードを一覧表示します。

-   **カラー ピッカー** &ndash;選択した任意の値を色に変更することができます。

-   **材料パレット**&ndash;マテリアルの設計に準拠した値に色を選択を変更することができます。

-   **リソース**&ndash;色のテーマでのリソースを他の既存のいずれかに選択した色を変更できます。

これらのタブの詳細のいずれかを見てみましょう。



### <a name="inherit-tab"></a>タブを継承します。

次の例のように、**継承** タブのスタイルの継承を一覧表示、**背景**の色、**既定のテーマ**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![タブを継承します。](material-design-features-images/vs/06-inherit-tab-sml.png)](material-design-features-images/vs/06-inherit-tab.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![タブを継承します。](material-design-features-images/xs/13-inherit-sml.png)](material-design-features-images/xs/13-inherit.png#lightbox)

-----

この例では、**既定のテーマ**を使用するスタイルを継承`@color/background_material_dark`をオーバーライドしますが、`color/material_grey_850`のカラー コード値を持つ`#ff303030`します。
スタイルの継承の詳細については、次を参照してください。[スタイルとテーマ](http://developer.android.com/guide/topics/ui/themes.html#Inheritance)です。



### <a name="color-picker"></a>カラー ピッカー

次のスクリーン ショットを示しています、**カラー ピッカー**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![カラー ピッカー](material-design-features-images/vs/07-color-picker-sml.png)](material-design-features-images/vs/07-color-picker.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![カラー ピッカー](material-design-features-images/xs/14-color-picker-sml.png)](material-design-features-images/xs/14-color-picker.png#lightbox)

-----

この例では、**背景**色は、さまざまな方法で任意の値を変更できます。

-   色を直接クリックするとします。
-   色合い、鮮やかさ、明るさの値を入力します。
-   RGB (赤、緑、青) の値を 10 進数で入力します。
-   選択した色のアルファ (透明度) を設定します。
-   16 進カラー コードを直接入力します。

カラー ピッカーで選択した色が*いない*制限されているかを使用できる色リソースのセットにマテリアルの設計ガイドラインにします。


### <a name="resources"></a>リソース

**リソース** タブには、既にテーマで提供されている色リソースの一覧が提供しています。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![リソース](material-design-features-images/vs/08-resources-sml.png)](material-design-features-images/vs/08-resources.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![リソース](material-design-features-images/xs/15-resources-sml.png)](material-design-features-images/xs/15-resources.png#lightbox)

-----

使用して、**リソース** タブは、この色のリストにユーザーの選択を制限します。 ある場合は、テーマの別の部分に既に割り当てられている色リソースを選択すると、UI の 2 つの隣接する要素実行可能性があります"一緒に"(がある、同じ色) に注意してくださいを区別するユーザーが困難になるとします。



### <a name="material-palette"></a>材料パレット

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

**マテリアル パレット**タブが開き、**マテリアル デザインのカラー パレット**です。 マテリアルのデザインのガイドラインと一貫性ができるように、色の選択を制約するこのパレットから色値を選択します。

[![材料パレット](material-design-features-images/vs/09-material-palette-sml.png)](material-design-features-images/vs/09-material-palette.png#lightbox)

カラー パレットの上部では、パレットの下部に、選択した色の色合いの範囲が表示されますが、プライマリ マテリアル デザインの色が表示されます。 たとえば、選択**Indigo**、一連の**Indigo**色合いがダイアログの下部に表示されます。
色合いを選択するときに、プロパティの色が選択された色合いに変更されます。 次の例で、`Background Tint`ボタンの変更は*Indigo 500*:

![Indigo 500 を選択します。](material-design-features-images/vs/10-indigo.png)

`Background Tint` カラー コードに設定されている*Indigo 500* (`#ff3f51b5`)、し、デザイナーは、この変更を反映するように背景色を更新します。

[![背景の濃淡が変更されました。](material-design-features-images/vs/11-background-tint-sml.png)](material-design-features-images/vs/11-background-tint.png#lightbox)

マテリアルのデザインのカラー パレットの詳細については、マテリアル デザインを参照してください。[色パレット ガイド](http://www.google.com/design/spec/style/color.html#color-color-palette)です。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

**マテリアル パレット**タブが開き、**マテリアル デザインのカラー パレット**説明[以前](#material_palette)です。 マテリアルのデザインのガイドラインと一貫性ができるように、色の選択を制約するこのパレットから色値を選択します。

[![材料パレット](material-design-features-images/xs/16-material-palette-sml.png)](material-design-features-images/xs/16-material-palette.png#lightbox)

-----



### <a name="creating-a-new-theme"></a>新しいテーマを作成します。

次の例を新しいカスタム テーマを作成するのに、マテリアル パレットを使用します。 最初に、変更しましょう、**バック グラウンド**する色*青 900*:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![青い 900 に背景を変更します。](material-design-features-images/vs/12-change-background-to-blue.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![青い 900 に背景を変更します。](material-design-features-images/xs/17-change-background-to-blue-sml.png)](material-design-features-images/xs/17-change-background-to-blue.png#lightbox)

-----


色リソースが変更されたときにメッセージがポップアップ表示されたメッセージを *、現在のテーマの変更が保存されて*:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![警告未保存の変更](material-design-features-images/vs/13-unsaved-changes-sml.png)](material-design-features-images/vs/13-unsaved-changes.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![警告未保存の変更](material-design-features-images/xs/18-unsaved-changes-sml.png)](material-design-features-images/xs/18-unsaved-changes.png#lightbox)

-----


**背景**デザイナーでの色を新しい色の選択を変更したが、この変更が保存されていません。 変更を処理する方法については、2 つの選択肢が提供されています。

-   **変更を破棄**&ndash;新しい色の選択 (オプション) を破棄し、元の状態には、テーマを元に戻します。

-   **新しいテーマを作成する**&ndash;は現在選択されているテーマに由来で行われた色の上書きを使用して新しいテーマを作成、**テーマ エディター**です。

クリックすると、**新しいテーマの作成**を次のいずれかが発生する、選択したテーマに応じて。

-   デザイナーで現在選択されているテーマがプロジェクトのテーマでない場合は、新しいリソース ファイル (カスタマイズしたテーマの親として選択したテーマを使用)、カスタマイズされたテーマを作成するオプションが表示されます。 作成した後、カスタマイズしたテーマをデザイナーに切り替えます。

-   現在選択されているテーマが 1 つの場所で定義されているプロジェクトのテーマの場合が表示されます、**更新テーマ**オプションです。 新規に作成するのではなく、現在選択されているテーマは、このオプションをクリックすると更新されます。

-   複数の場所で定義されているプロジェクト テーマは現在選択されている場合 (などの異なる方法で-サフィックス**値**などフォルダー**値 v21**)、確認を求めるダイアログが表示されますカスタマイズしたテーマを保存するための新しい位置。

クリックすると、前の例を続行**新しいテーマの作成**新しいテーマの作成時に結果と呼ばれる**カスタム**:


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![カスタム テーマを追加](material-design-features-images/vs/14-custom-theme-sml.png)](material-design-features-images/vs/14-custom-theme.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![カスタム テーマを追加](material-design-features-images/xs/19-custom-theme-sml.png)](material-design-features-images/xs/19-custom-theme.png#lightbox)

-----


現在選択されているテーマは、プロジェクトのテーマではないために、選択したテーマを更新するか、新しい場所を指定するダイアログはありません。


## <a name="summary"></a>まとめ

このトピックでは、Xamarin.Android デザイナーで使用できる材料デザイン機能について説明します。 これは、有効化し、材料デザイン グリッドを構成する方法、マテリアル デザインのカラー パレットを使用して、色のプロパティを編集する方法、および文字体裁スケール セレクターを使用して、テキストのプロパティを構成する方法について説明します。 テーマ エディターを使用して、マテリアルのデザイン ガイドラインに準拠した新しいカスタム テーマを作成する方法も示されています。 マテリアルのデザインの Xamarin.Android サポートの詳細については、次を参照してください。[マテリアル テーマ](~/android/user-interface/material-theme.md)です。



## <a name="related-links"></a>関連リンク

- [素材のテーマ](~/android/user-interface/material-theme.md)
- [素材のデザインの概要](https://www.google.com/design/spec/material-design/introduction.html)
