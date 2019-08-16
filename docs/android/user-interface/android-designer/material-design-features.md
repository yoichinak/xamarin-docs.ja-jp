---
title: Xamarin. Android Designer 素材のデザイン機能
description: このトピックでは、開発者が素材デザインに準拠したレイアウトを簡単に作成できるようにするデザイナー機能について説明します。 このセクションでは、素材グリッド、素材カラーパレット、文字体裁スケール、およびテーマエディターの使用方法について説明します。
ms.prod: xamarin
ms.assetid: AC55E1B2-C239-4019-B0C3-A16F6CF0D6E0
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
ms.openlocfilehash: 77b0bc28bc4156092cb2b12d0c8b234d3f021239
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69523441"
---
# <a name="xamarinandroid-designer-material-design-features"></a>Xamarin. Android Designer 素材のデザイン機能

_このトピックでは、開発者が素材デザインに準拠したレイアウトを簡単に作成できるようにするデザイナー機能について説明します。このセクションでは、素材グリッド、素材カラーパレット、文字体裁スケール、およびテーマエディターの使用方法について説明します。_

> [!Video https://youtube.com/embed/E3_ZjIOzVzY]

**Evolve 2016:すべてのユーザーが、素材設計で美しいアプリを作成できます**

## <a name="overview"></a>概要

Android Designer には、マテリアルデザインに準拠したレイアウトの作成を容易にする機能が含まれています。 素材のデザインに慣れていない場合は、[資料の設計の概要](https://material.io/design/introduction)を参照してください。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

このガイドでは、次のデザイナー機能について説明します。

- *素材グリッド*&ndash;素材のデザインガイドラインに従ってレイアウトウィジェットを配置する際に役立つグリッド、スペース、および keylines を表示するデザインサーフェイスのオーバーレイ。

- *テーマエディター*&ndash;テーマのサブセットの色情報を設定できる、小さいカラーリソースエディター。 たとえば、 `colorPrimary`、 `colorPrimaryDark`、 `colorAccent`などの素材の色をプレビューおよび変更できます。

これらの各機能について説明し、その使用方法の例を示します。

## <a name="material-design-grid"></a>素材のデザイングリッド

[マテリアルデザイン] グリッドメニューは、デザイナーの上部にあるツールバーから使用できます。

[![素材のデザイングリッド](material-design-features-images/vs/01-material-design-grid-w158-sml.png)](material-design-features-images/vs/01-material-design-grid-w158.png#lightbox)

[素材デザイン] グリッドアイコンをクリックすると、次の要素を含むデザインサーフェイスにオーバーレイが表示されます。

- Keylines (オレンジ色の線)

- 間隔 (緑の領域)

- グリッド (青い線)

これらの要素は、前のスクリーンショットで確認できます。 これらの各オーバーレイ項目は構成可能です。 [マテリアルデザイン] グリッドメニューの横にある省略記号をクリックすると、ダイアログ segue が開きます。このダイアログボックスでは、グリッドを無効/有効にしたり、キー行の配置を構成したり、spacings を設定したりできます。 すべての値は (密度に`dp`依存しないピクセル) で表されることに注意してください。

[![Grid、keyline、および spacing の構成](material-design-features-images/vs/03-grid-configuration-w158-sml.png)](material-design-features-images/vs/03-grid-configuration-w158.png#lightbox)

新しい keyline を追加するには、 **[オフセット]** ボックスに新しいオフセット値を入力し、場所 (**left**、 **top**、 **right**、または**bottom**) を選択し、[+] アイコンをクリックして新しい keyline を追加します。 同様に、新しいスペースを追加するには、サイズ ボックスと **オフセット**ボックスにそれぞれサイズとオフセット (dp) を入力します。 場所 (**左**、**上**、**右**、**下**) を選択し、[+] アイコンをクリックして新しい間隔を追加します。

これらの構成値を変更すると、レイアウト XML ファイルに保存され、再度レイアウトを開いたときに再利用されます。


## <a name="theme-editor"></a>テーマエディター

**テーマエディター**では、テーマ属性のサブセットの色情報をカスタマイズできます。 **テーマエディター**を開くには、ツールバーの [ペイントブラシ] アイコンをクリックします。

[![テーマエディターのアイコン](material-design-features-images/vs/04-theme-editor-icon-w158-sml.png)](material-design-features-images/vs/04-theme-editor-icon-w158.png#lightbox)

**テーマエディター**には、対象となるすべての Android バージョンと api レベルのツールバーからアクセスできますが、ターゲット api レベルが API 21 (Android 5.0 ロリポップ) よりも前の場合は、以下で説明する機能のサブセットのみを使用できます。

**テーマエディター**の左側のパネルには、現在選択されているテーマを構成する色の一覧が表示されます (この例`Default Theme`ではを使用しています)。

[![テーマエディター](material-design-features-images/vs/05-theme-editor-w158-sml.png)](material-design-features-images/vs/05-theme-editor-w158.png#lightbox)

左側の色を選択すると、右側のパネルには、その色を編集するための次のタブが表示されます。

- **継承**&ndash;選択した色のスタイルの継承ダイアグラムを表示し、そのテーマの色に割り当てられている解決済みの色と色のコードを表示します。

- **カラーピッカー**&ndash;選択した色を任意の値に変更できます。

- **素材パレット**&ndash;選択した色を、マテリアルデザインに準拠した値に変更できます。

- **リソース**&ndash;選択した色を、テーマ内の他の既存の色リソースの1つに変更できます。

これらのタブのそれぞれを詳しく見てみましょう。

### <a name="inherit-tab"></a>[継承] タブ

次の例に示すように、 **[継承]** タブには、**既定のテーマ**の**背景**色のスタイル継承が表示されます。

[![[継承] タブ](material-design-features-images/vs/06-inherit-tab-w158-sml.png)](material-design-features-images/vs/06-inherit-tab-w158.png#lightbox)

この例では、**既定のテーマ**はを使用`@color/background_material_light`するスタイルを継承します`color/material_grey_50`が、色の`#fffafafa`コード値がであるでオーバーライドします。
スタイルの継承の詳細については、「[スタイルとテーマ](https://developer.android.com/guide/topics/ui/themes.html#Inheritance)」を参照してください。

### <a name="color-picker"></a>カラー ピッカー

次のスクリーンショットは、**カラーピッカー**を示しています。

[![カラーピッカー](material-design-features-images/vs/07-color-picker-w158-sml.png)](material-design-features-images/vs/07-color-picker-w158.png#lightbox)

この例では、さまざまな方法で**背景**色を任意の値に変更できます。

- 色を直接クリックします。
- 色合い、鮮やかさ、および明るさの値を入力します。
- RGB (赤、緑、青) の値を10進数で入力します。
- 選択した色のアルファ (不透明度) を設定します。
- 16進数のカラーコードを直接入力します。

カラーピッカーで選択した色は、素材のデザインガイドラインや使用可能な色リソースのセットに限定され*ません*。

### <a name="resources"></a>リソース

**[リソース]** タブには、テーマに既に存在する色リソースの一覧が表示されます。

[![参考](material-design-features-images/vs/08-resources-w158-sml.png)](material-design-features-images/vs/08-resources-w158.png#lightbox)

**[リソース]** タブを使用すると、選択した色の一覧が制限されます。 テーマの別の部分に既に割り当てられている色リソースを選択すると、UI の隣接する2つの要素が "同時に実行" される可能性があることに注意してください (色が同じであるため)。ユーザーが区別するのが困難になります。

### <a name="material-palette"></a>素材パレット

**[素材パレット]** タブでは、**素材デザインのカラーパレット**が開きます。 このパレットから色の値を選択すると、色の選択が制限され、マテリアルデザインガイドラインとの一貫性が確保されます。

[![素材パレット](material-design-features-images/vs/09-material-palette-w158-sml.png)](material-design-features-images/vs/09-material-palette-w158.png#lightbox)

カラーパレットの上部には、主要な素材デザインの色が表示されます。パレットの下部には、選択した原色の色の範囲が表示されます。 たとえば、 **[indigo]** を選択すると、ダイアログの下部に**indigo**の色合いのコレクションが表示されます。
色合いを選択すると、プロパティの色が、選択した色合いに変わります。 次の例では、 `Background Tint`ボタンのが*Indigo 500*に変更されています。

![[Indigo 500] を選択します。](material-design-features-images/vs/10-indigo-w158.png)

`Background Tint`は*Indigo 500* (`#ff3f51b5`) の色コードに設定され、デザイナーはこの変更を反映するように背景色を更新します。

[![背景の濃淡が変更されました](material-design-features-images/vs/11-background-tint-w158-sml.png)](material-design-features-images/vs/11-background-tint-w158.png#lightbox)

マテリアルデザインカラーパレットの詳細については、「マテリアルデザインの[カラーパレットガイド](https://material.io/design/color/)」を参照してください。

### <a name="creating-a-new-theme"></a>新しいテーマの作成

次の例では、マテリアルパレットを使用して、新しいカスタムテーマを作成します。 まず、**背景**色を*青 900*に変更します。

![背景を青900に変更します](material-design-features-images/vs/12-change-background-to-blue-w158.png)

色リソースを変更すると、メッセージが表示され、*現在のテーマの変更は保存*されていません。

[![未保存の変更の警告](material-design-features-images/vs/13-unsaved-changes-w158-sml.png)](material-design-features-images/vs/13-unsaved-changes-w158.png#lightbox)

デザイナーの**背景**色が新しい色の選択に変わりましたが、この変更はまだ保存されていません。 この時点で、次のいずれかの操作を実行できます。

- **[変更の破棄]** をクリックして、新しい色の選択 (または選択) を破棄し、テーマを元の状態に戻します。

- <kbd>CTRL + S</kbd>キーを押して、現在のテーマへの変更を保存します。

次の例では、変更が**Apptheme**に保存されるように<kbd>CTRL + S キー</kbd>を押しました。

[![AppTheme に保存された変更](material-design-features-images/vs/14-custom-theme-w158-sml.png)](material-design-features-images/vs/14-custom-theme-w158.png#lightbox)

## <a name="summary"></a>まとめ

このトピックでは、Android Designer で使用できるマテリアルデザイン機能について説明します。 ここでは、[マテリアルデザイン] グリッドを有効にして構成する方法について説明しました。また、テーマエディターを使用して、マテリアルデザインガイドラインに準拠する新しいカスタムテーマを作成する方法についても説明しました。
Xamarin の詳細については、マテリアル設計の詳細については、「[マテリアルのテーマ](~/android/user-interface/material-theme.md)」を参照してください。




# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

このガイドでは、デザイナーの次の機能について説明します。

- *素材のデザイングリッド*&ndash;素材のデザインガイドラインに従ってレイアウトウィジェットを配置する際に役立つグリッド、スペース、および keylines を表示するデザインサーフェイスのオーバーレイ。

- *素材デザインの色パレット*&ndash; [プロパティパッド] ダイアログ。これは、公式のマテリアルデザインパレットから色を選択するのに役立ちます。

- *大*文字と小文字の調整テキストフィールドの`textAppearance`プロパティに対する素材デザインに準拠した設定を提供するプロパティパッドダイアログ。 &ndash;

- *テーマエディター*&ndash;テーマのサブセットの色情報を設定できる、小さいカラーリソースエディター。 たとえば、 `colorPrimary`、 `colorPrimaryDark`、 `colorAccent`などの素材の色をプレビューおよび変更できます。

これらの各機能について説明し、その使用方法の例を示します。

## <a name="material-design-grid"></a>素材のデザイングリッド

[マテリアルデザイン] グリッドメニューは、デザイナーの上部にあるツールバーから使用できます。

[![素材のデザイングリッド](material-design-features-images/xs/01-material-design-grid-sml.png)](material-design-features-images/xs/01-material-design-grid.png#lightbox)

[素材デザイン] グリッドアイコンをクリックすると、次の要素を含むデザインサーフェイスにオーバーレイが表示されます。

- Keylines (オレンジ色の線)

- 間隔 (緑の領域)

- グリッド (青い線)

これらの要素は、次のスクリーンショットで確認できます。

[![Keyline、スペーシング、および grid](material-design-features-images/xs/02-grid-and-keylines-sml.png)](material-design-features-images/xs/02-grid-and-keylines.png#lightbox)

これらの各オーバーレイ項目は構成可能です。 [マテリアルデザイン] グリッドメニュー&hellip;の横にある省略記号 () をクリックすると、ダイアログ segue が開き、グリッドを無効/有効にしたり、キー行の配置を構成したり、spacings を設定したりすることができます。 すべての値は (密度に`dp`依存しないピクセル) で表されることに注意してください。

[![Grid、keyline、および spacing の構成](material-design-features-images/xs/03-grid-configuration-sml.png)](material-design-features-images/xs/03-grid-configuration.png#lightbox)

新しい keyline を追加するには、 **[オフセット]** ボックスに新しいオフセット値を入力し、場所 (**left**、 **top**、 **right**、または**bottom**) を選択し、[+] アイコン (値を入力したときに右側に表示されます) をクリックして、新しい keyline を追加します。 同様に、新しいスペースを追加するには、サイズ ボックスと **オフセット**ボックスにそれぞれサイズとオフセット (dp) を入力します。 場所 (**左**、**上**、**右**、**下**) を選択し、[+] アイコンをクリックして新しい間隔を追加します。

これらの構成値を変更すると、レイアウト XML ファイルに保存され、再度レイアウトを開いたときに再利用されます。

## <a name="material-design-color-palette"></a>素材デザインの色パレット

色を受け入れるすべてのプロパティパネル項目には、次のスクリーンショットに示すように、[パレット] アイコンが追加されます。このアイコンを使用して、素材デザインのカラーパレットを開くことができます。

[![色アイコン](material-design-features-images/xs/04-new-color-icon-sml.png)](material-design-features-images/xs/04-new-color-icon.png#lightbox)

このアイコンをクリックすると、ダイアログ segue が開きます。このダイアログボックスでは、素材デザインのカラーパレットから、そのプロパティの色を構成することができます。

[![素材デザインの色パレット](material-design-features-images/xs/05-material-palette-sml.png)](material-design-features-images/xs/05-material-palette.png#lightbox)

カラーパレットの上部には、主要な素材デザインの色が表示されます。パレットの下部には、選択した原色の色の範囲が表示されます。 たとえば、 **[indigo]** を選択すると、ダイアログの下部に**indigo**の色合いのコレクションが表示されます。
色合いを選択すると、プロパティの色が、選択した色合いに変わります。 次の例では、 `Background Tint`ボタンのが*Indigo 500*に変更されています。

[![Indigo 500 を選択する](material-design-features-images/xs/06-indigo-sml.png)](material-design-features-images/xs/06-indigo.png#lightbox)

`Background Tint`は*Indigo 500* (`#ff3f51b5`) の色コードに設定され、デザイナーは、この変更を反映するようにボタンの背景色を更新します。

[![背景の濃淡の変更](material-design-features-images/xs/07-background-tint-sml.png)](material-design-features-images/xs/07-background-tint.png#lightbox)

マテリアルデザインカラーパレットの詳細については、「マテリアルデザインの[カラーパレットガイド](https://material.io/design/color/)」を参照してください。

## <a name="typographic-scale"></a>大文字と小文字の調整

[**プロパティ**パッド**スタイル**] タブの **[テキストの表示]** セクションには、マテリアルデザイン仕様`TextAppearance`に準拠するスタイルを選択できるアイコンがあります。

[![[スタイル] タブ](material-design-features-images/xs/08-typo-scale-icon-sml.png)](material-design-features-images/xs/08-typo-scale-icon.png#lightbox)

このアイコンをクリックすると、 **[タイポグラフィスケール]** ダイアログボックスが開き、事前に構成されたテキストスタイルの一覧が表示されます。これには、次の中から選択できます。

[![テキストスタイルの選択](material-design-features-images/xs/09-text-appearance-sml.png)](material-design-features-images/xs/09-text-appearance.png#lightbox)

次の例では、 **[表示 1]** をクリックすると、ボタンのテキストが、**表示 1**の大きなフォントに変わります。

[![1スタイルを表示](material-design-features-images/xs/10-display-1-sml.png)](material-design-features-images/xs/10-display-1.png#lightbox)

[文字**幅の調整**] ダイアログボックスのテキストスタイルは、**テーマ**の設定に従います。 たとえば、**ライト**テーマがデザイナーで選択されている場合、使用可能なテキストスタイルの一覧によって**明るい**テーマが反映されます。

[![ライトテーマ](material-design-features-images/xs/11-light-theme-sml.png)](material-design-features-images/xs/11-light-theme.png#lightbox)

## <a name="theme-editor"></a>テーマエディター

**テーマエディター**では、テーマ属性のサブセットの色情報をカスタマイズできます。 **テーマエディター**を開くには、ツールバーの [ペイントブラシ] アイコンをクリックします。

[![テーマエディターのアイコン](material-design-features-images/xs/12a-theme-editor-icon-sml.png)](material-design-features-images/xs/12a-theme-editor-icon.png#lightbox)

**テーマエディター**には、対象となるすべての Android バージョンと api レベルのツールバーからアクセスできますが、ターゲット api レベルが API 21 (Android 5.0 ロリポップ) よりも前の場合は、以下で説明する機能のサブセットのみを使用できます。

**テーマエディター**の左側のパネルには、現在選択されているテーマを構成する色の一覧が表示されます (この例`Default Theme`ではを使用しています)。

[![テーマエディター](material-design-features-images/xs/12b-theme-editor-sml.png)](material-design-features-images/xs/12b-theme-editor.png#lightbox)

左側の色を選択すると、右側のパネルには、その色を編集するための次のタブが表示されます。

- **継承**&ndash;選択した色のスタイルの継承ダイアグラムを表示し、そのテーマの色に割り当てられている解決済みの色と色のコードを表示します。

- **カラーピッカー**&ndash;選択した色を任意の値に変更できます。

- **素材パレット**&ndash;選択した色を、マテリアルデザインに準拠した値に変更できます。

- **リソース**&ndash;選択した色を、テーマ内の他の既存の色リソースの1つに変更できます。

これらのタブのそれぞれを詳しく見てみましょう。

### <a name="inherit-tab"></a>[継承] タブ

次の例に示すように、 **[継承]** タブには、**既定のテーマ**の**背景**色のスタイル継承が表示されます。

[![[継承] タブ](material-design-features-images/xs/13-inherit-sml.png)](material-design-features-images/xs/13-inherit.png#lightbox)

この例では、**既定のテーマ**はを使用`@color/background_material_dark`するスタイルを継承します`color/material_grey_850`が、色の`#ff303030`コード値がであるでオーバーライドします。
スタイルの継承の詳細については、「[スタイルとテーマ](https://developer.android.com/guide/topics/ui/themes.html#Inheritance)」を参照してください。

### <a name="color-picker"></a>カラー ピッカー

次のスクリーンショットは、**カラーピッカー**を示しています。

[![カラーピッカー](material-design-features-images/xs/14-color-picker-sml.png)](material-design-features-images/xs/14-color-picker.png#lightbox)


この例では、さまざまな方法で**背景**色を任意の値に変更できます。

- 色を直接クリックします。
- 色合い、鮮やかさ、および明るさの値を入力します。
- RGB (赤、緑、青) の値を10進数で入力します。
- 選択した色のアルファ (不透明度) を設定します。
- 16進数のカラーコードを直接入力します。

カラーピッカーで選択した色は、素材のデザインガイドラインや使用可能な色リソースのセットに限定され*ません*。

### <a name="resources"></a>リソース

**[リソース]** タブには、テーマに既に存在する色リソースの一覧が表示されます。

[![参考](material-design-features-images/xs/15-resources-sml.png)](material-design-features-images/xs/15-resources.png#lightbox)

**[リソース]** タブを使用すると、選択した色の一覧が制限されます。 テーマの別の部分に既に割り当てられている色リソースを選択すると、UI の隣接する2つの要素が "同時に実行" される可能性があることに注意してください (色が同じであるため)。ユーザーが区別するのが困難になります。

### <a name="material-palette"></a>素材パレット

**[素材パレット]** タブには、[前](#material-design-color-palette)に説明した [**マテリアルのデザイン] カラーパレット**が表示されます。 このパレットから色の値を選択すると、マテリアルのデザインガイドラインと一貫性があるように色の選択が制限されます。

[![素材パレット](material-design-features-images/xs/16-material-palette-sml.png)](material-design-features-images/xs/16-material-palette.png#lightbox)

### <a name="creating-a-new-theme"></a>新しいテーマの作成

次の例では、マテリアルパレットを使用して、新しいカスタムテーマを作成します。 まず、**背景**色を*青 900*に変更します。

[![背景を青900に変更します](material-design-features-images/xs/17-change-background-to-blue-sml.png)](material-design-features-images/xs/17-change-background-to-blue.png#lightbox)

色リソースを変更すると、メッセージが表示され、*現在のテーマの変更は保存*されていません。

[![未保存の変更の警告](material-design-features-images/xs/18-unsaved-changes-sml.png)](material-design-features-images/xs/18-unsaved-changes.png#lightbox)

デザイナーでの色の変更が行われましたが、この変更はまだ保存されていません。 この時点で、次のいずれかの操作を実行できます。

- **[変更の破棄]** をクリックして、新しい色の選択 (または選択) を破棄し、テーマを元の状態に戻します。

- **&#8984; + S**キーを押して、**カスタム**という名前の新しいテーマへの変更を保存します。


## <a name="summary"></a>Summary

このトピックでは、Android Designer で使用できるマテリアルデザイン機能について説明します。 ここでは、マテリアルデザイングリッドを有効にして構成する方法、素材デザインカラーパレットを使用して色のプロパティを編集する方法、およびタイポグラフィスケールセレクターを使用してテキストプロパティを構成する方法について説明しました。 また、テーマエディターを使用して、マテリアルデザインガイドラインに準拠する新しいカスタムテーマを作成する方法についても説明します。 Xamarin の詳細については、マテリアル設計の詳細については、「[マテリアルのテーマ](~/android/user-interface/material-theme.md)」を参照してください。

-----


## <a name="related-links"></a>関連リンク

- [素材のテーマ](~/android/user-interface/material-theme.md)
- [素材の設計の概要](https://material.io/design/introduction)
