---
title: Xamarin.Android デザイナー素材のデザイン機能
description: このトピックでは、素材の設計に準拠していませんレイアウトを作成する開発者にとってより簡単にするデザイナーの機能について説明します。 このセクションでは、紹介し、マテリアル グリッド、マテリアルの色パレット、印刷用のスケール、およびテーマ エディターを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: AC55E1B2-C239-4019-B0C3-A16F6CF0D6E0
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
ms.openlocfilehash: 5e6d7b4bdfdf7ea48d26537cb41c763656b050e0
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57669654"
---
# <a name="xamarinandroid-designer-material-design-features"></a>Xamarin.Android デザイナー マテリアル デザイン機能

_このトピックでは、素材の設計に準拠していませんレイアウトを作成する開発者にとってより簡単にするデザイナーの機能について説明します。このセクションでは、紹介し、マテリアル グリッド、マテリアルの色パレット、印刷用のスケール、およびテーマ エディターを使用する方法について説明します。_

> [!Video https://youtube.com/embed/E3_ZjIOzVzY]

**Evolve 2016:すべてのユーザーには、マテリアル デザイン美しいアプリを作成できます。**

## <a name="overview"></a>概要

Xamarin.Android のデザイナーには、マテリアル デザイン準拠のレイアウトを作成するためのより簡単にする機能が含まれています。 マテリアル デザインに慣れていない場合は、次を参照してください。、[マテリアル デザイン概要](https://material.io/design/introduction)します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

このガイドでは、次のデザイナーの機能を見てなります。

-   *素材グリッド*&ndash;グリッド、間隔、およびマテリアル デザイン ガイドラインに従ってレイアウト ウィジェットを配置するための主線を表示するデザイン サーフェイス上のオーバーレイします。

-   *テーマ エディター* &ndash;できるようにする小さなカラー リソース エディター テーマのサブセットの色の情報を設定します。 たとえば、プレビューおよびなどマテリアルの色を変更できます`colorPrimary`、 `colorPrimaryDark`、および`colorAccent`します。

これらの各機能確認がされその使用方法の例を示します。

## <a name="material-design-grid"></a>素材のデザイン グリッド

マテリアル デザイン グリッド] メニューの [デザイナーの上部にあるツールバーから提供されています。

[![素材のデザイン グリッド](material-design-features-images/vs/01-material-design-grid-w158-sml.png)](material-design-features-images/vs/01-material-design-grid-w158.png#lightbox)

マテリアル デザイン グリッド アイコンをクリックすると、デザイナーが、次の要素を含むデザイン画面にオーバーレイが表示されます。

-   主線 (オレンジ色の線)

-   間隔 (緑の領域)

-   グリッド (青い線)

これらの要素は、前のスクリーン ショットで確認できます。 オーバーレイの以下の内容は、構成できます。 マテリアル デザイン グリッド メニューの横にある省略記号をクリックするとダイアログ ポップ オーバーでは、グリッドを有効/無効にするの主線、配置を構成および間隔を設定することが表示されます。 すべての値で表現される注`dp`(密度に依存しないピクセル単位)。

[![グリッド、断裁、および間隔の構成](material-design-features-images/vs/03-grid-configuration-w158-sml.png)](material-design-features-images/vs/03-grid-configuration-w158.png#lightbox)

新しい断裁を追加するには、新しいオフセット値を入力します、**オフセット**ボックスに、場所を選択します (**左**、**上部**、**右**、または **。下部にある**) をクリックし、+ アイコンを新しい断裁を追加します。 同様に、新しい間隔を追加するを入力 (dp) 内のオフセットとサイズ、**サイズ**と**オフセット**ボックスに、それぞれします。 場所を選択します (**左**、**上部**、**右**、または**下部にある**) をクリックし、+ 新しい間隔を追加する アイコン。

これらの構成値を変更するときに、レイアウトの XML ファイルに保存されていて、レイアウトをもう一度開くときに再利用します。


## <a name="theme-editor"></a>テーマ エディター

**テーマ エディター**テーマの属性のサブセットの色の情報をカスタマイズすることができます。 開くには、**テーマ エディター**ツールバーで、ペイント ブラシ アイコンをクリックします。

[![テーマ エディター アイコン](material-design-features-images/vs/04-theme-editor-icon-w158-sml.png)](material-design-features-images/vs/04-theme-editor-icon-w158.png#lightbox)

ただし、**テーマ エディター**は、ツールバーからアクセスできるすべてのターゲット Android バージョンと API レベルでは、以下に示す機能のサブセットが使用できるは、ターゲット API レベルが API 21 (Android 5.0 より前の場合のみロリポップ)。

左側のパネル、**テーマ エディター**現在選択されているテーマを構成する色の一覧が表示されます (この例では使用、 `Default Theme`)。

[![テーマ エディター](material-design-features-images/vs/05-theme-editor-w158-sml.png)](material-design-features-images/vs/05-theme-editor-w158.png#lightbox)

左上の色を選択すると、右側のパネルは、その色を編集するために、次のタブを提供します。

-   **継承**&ndash;選択した色のスタイル継承ダイアグラムを表示し、解決済みの色とそのテーマの色に割り当てられているカラー コードを一覧表示します。

-   **カラー ピッカー** &ndash;任意の値を選択した色を変更することができます。

-   **素材パレット**&ndash;マテリアル デザインに準拠した値を選択した色を変更することができます。

-   **リソース**&ndash;テーマで、その他の既存の色リソースのいずれかに、選択した色を変更することができます。

これらのタブの詳細のいずれかを見てみましょう。

### <a name="inherit-tab"></a>タブを継承します。

次の例のように、**継承**のスタイルの継承がタブに一覧表示されます、**バック グラウンド**の色、**既定のテーマ**:

[![タブを継承します。](material-design-features-images/vs/06-inherit-tab-w158-sml.png)](material-design-features-images/vs/06-inherit-tab-w158.png#lightbox)

この例で、**既定のテーマ**を使用するスタイルを継承`@color/background_material_light`でよりも優先されますが、`color/material_grey_50`のカラー コード値を持つ`#fffafafa`します。
スタイルの継承の詳細については、次を参照してください。[スタイルとテーマ](https://developer.android.com/guide/topics/ui/themes.html#Inheritance)します。

### <a name="color-picker"></a>カラー ピッカー

次のスクリーン ショットを示しています、**カラー ピッカー**:

[![カラー ピッカー](material-design-features-images/vs/07-color-picker-w158-sml.png)](material-design-features-images/vs/07-color-picker-w158.png#lightbox)

この例で、**バック グラウンド**さまざまな方法で任意の値に色を変更することができます。

-   色を直接クリックするとします。
-   色合い、鮮やかさ、および明るさの値を入力します。
-   10 進数の RGB (赤、緑、青) 値を入力します。
-   選択した色のアルファ (透明度) を設定します。
-   16 進数カラー コードを直接入力します。

カラー ピッカーで選択した色は*いない*マテリアル デザイン ガイドラインに、または使用可能な色リソースのセットを制限します。

### <a name="resources"></a>リソース

**リソース** タブが存在するテーマの色リソースの一覧を提供しています。

[![リソース](material-design-features-images/vs/08-resources-w158-sml.png)](material-design-features-images/vs/08-resources-w158.png#lightbox)

使用して、**リソース** タブは、この色のリストにユーザーの選択を制限します。 テーマの別の部分に既に割り当てられている色リソースを選択すると、UI の 2 つの隣接する要素発生可能性があります"化"(顔色と一致している) ために注意してくださいと区別するためにユーザーを困難になります。

### <a name="material-palette"></a>素材パレット

**素材パレット**タブが表示されます、**マテリアル デザインのカラー パレット**します。 一貫性のあるマテリアル デザイン ガイドラインにあるように、色の選択を制約するこのパレットから色の値を選択します。

[![素材パレット](material-design-features-images/vs/09-material-palette-w158-sml.png)](material-design-features-images/vs/09-material-palette-w158.png#lightbox)

色パレットの上部では、パレットの下部には、選択した色の色合いの範囲が表示されます。 中に、プライマリのマテリアル デザイン色が表示されます。 たとえば、 **Indigo**、一連の**Indigo**色相がダイアログの下部に表示されます。
Hue を選択すると、プロパティの色が選択されている hue に変更されます。 次の例では、`Background Tint`ボタンの変更は*Indigo 500*:

![Indigo 500 を選択します。](material-design-features-images/vs/10-indigo-w158.png)

`Background Tint` カラー コードに設定されている*Indigo 500* (`#ff3f51b5`)、し、デザイナーは、この変更を反映するように背景色を更新します。

[![背景の濃淡の変更](material-design-features-images/vs/11-background-tint-w158-sml.png)](material-design-features-images/vs/11-background-tint-w158.png#lightbox)

マテリアル デザインのカラー パレットの詳細については、マテリアル デザインを参照してください。[色パレット ガイド](https://material.io/design/color/)します。

### <a name="creating-a-new-theme"></a>新しいテーマの作成

次の例では、素材パレット新しいカスタム テーマの作成に使用します。 最初に、変更します、**バック グラウンド**する色*青い 900*:

![背景を青 900 に変更します。](material-design-features-images/vs/12-change-background-to-blue-w158.png)

色リソースが変更されたときにポップアップ メッセージがメッセージと共に *、現在のテーマの変更が保存されて*:

[![警告未保存の変更](material-design-features-images/vs/13-unsaved-changes-w158-sml.png)](material-design-features-images/vs/13-unsaved-changes-w158.png#lightbox)

**バック グラウンド**デザイナーでの色を新しい色の選択を変更したが、この変更が保存されていません。 この時点で、次のいずれかを行うことができます。

-   クリックして**変更の破棄**を新しい色の選択 (または選択) を破棄し、テーマを元の状態を元に戻します。 

-   キーを押して<kbd>CTRL + S</kbd>に変更を保存する、現在のテーマ。 

次の例では、 <kbd>CTRL + S</kbd>に変更が保存されたようにが押された**AppTheme**:

[![AppTheme に保存された変更](material-design-features-images/vs/14-custom-theme-w158-sml.png)](material-design-features-images/vs/14-custom-theme-w158.png#lightbox)

## <a name="summary"></a>まとめ

このトピックでは、Xamarin.Android デザイナーで使用可能なマテリアル デザイン機能について説明します。 有効にし、素材のデザイン グリッドを構成する方法を説明し、テーマ エディターを使用して、マテリアル デザイン ガイドラインに準拠した新しいカスタム テーマを作成する方法について説明します。
マテリアル デザインの Xamarin.Android のサポートの詳細については、次を参照してください。[マテリアル テーマ](~/android/user-interface/material-theme.md)します。




# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

このガイドでは、次のデザイナー機能を見て。

-   *素材のデザイン グリッド*&ndash;グリッド、間隔、およびマテリアル デザイン ガイドラインに従ってレイアウト ウィジェットを配置するための主線を表示するデザイン サーフェイス上のオーバーレイします。

-   *素材のデザインのカラー パレット*&ndash;公式のマテリアル デザイン パレットから色を選択するのには、使用するプロパティ パッド ダイアログ。

-   *印刷用のスケール* &ndash; A プロパティ パッド ダイアログ用の素材のデザイン準拠設定の選択肢を提供する、`textAppearance`テキスト フィールドのプロパティ。

-   *テーマ エディター* &ndash;できるようにする小さなカラー リソース エディター テーマのサブセットの色の情報を設定します。 たとえば、プレビューおよびなどマテリアルの色を変更できます`colorPrimary`、 `colorPrimaryDark`、および`colorAccent`します。

これらの各機能確認がされその使用方法の例を示します。

## <a name="material-design-grid"></a>素材のデザイン グリッド

マテリアル デザイン グリッド] メニューの [デザイナーの上部にあるツールバーから提供されています。

[![素材のデザイン グリッド](material-design-features-images/xs/01-material-design-grid-sml.png)](material-design-features-images/xs/01-material-design-grid.png#lightbox)

マテリアル デザイン グリッド アイコンをクリックすると、デザイナーが、次の要素を含むデザイン画面にオーバーレイが表示されます。

-   主線 (オレンジ色の線)

-   間隔 (緑の領域)

-   グリッド (青い線)

これらの要素は、次のスクリーン ショットに表示されることができます。

[![断裁、間隔、およびグリッド](material-design-features-images/xs/02-grid-and-keylines-sml.png)](material-design-features-images/xs/02-grid-and-keylines.png#lightbox)

オーバーレイの以下の内容は、構成できます。 省略記号をクリックすると (&hellip;) マテリアル デザイン グリッド メニューの横にあるダイアログ ポップ オーバー グリッドを有効/無効にするの主線、配置を構成して、間隔を設定するためが開きます。 すべての値で表現される注`dp`(密度に依存しないピクセル単位)。

[![グリッド、断裁、および間隔の構成](material-design-features-images/xs/03-grid-configuration-sml.png)](material-design-features-images/xs/03-grid-configuration.png#lightbox)

新しい断裁を追加するには、新しいオフセット値を入力します、**オフセット**ボックスに、場所を選択します (**左**、**上部**、**右**、または **。下部にある**) をクリックし、+ アイコン (値を入力すると、右側に表示されます) を新しい断裁を追加します。 同様に、新しい間隔を追加するを入力 (dp) 内のオフセットとサイズ、**サイズ**と**オフセット**ボックスに、それぞれします。 場所を選択します (**左**、**上部**、**右**、または**下部にある**) をクリックし、+ 新しい間隔を追加する アイコン。

これらの構成値を変更するときに、レイアウトの XML ファイルに保存されていて、レイアウトをもう一度開くときに再利用します。

## <a name="material-design-color-palette"></a>素材のデザインのカラー パレット

すべてのプロパティ パネル項目を色を受け入れるようになりましたが、このスクリーン ショットで示すようにマテリアル デザインのカラー パレットを開きを使用する追加パレット アイコン。

[![色のアイコン](material-design-features-images/xs/04-new-color-icon-sml.png)](material-design-features-images/xs/04-new-color-icon.png#lightbox)

このアイコンをクリックすると、ダイアログのポップ オーバーはマテリアル デザインのカラー パレットからそのプロパティの色を構成するためにできるようにするを開きます。

[![素材のデザインのカラー パレット](material-design-features-images/xs/05-material-palette-sml.png)](material-design-features-images/xs/05-material-palette.png#lightbox)

色パレットの上部では、パレットの下部には、選択した色の色合いの範囲が表示されます。 中に、プライマリのマテリアル デザイン色が表示されます。 たとえば、 **Indigo**、一連の**Indigo**色相がダイアログの下部に表示されます。
Hue を選択すると、プロパティの色が選択されている hue に変更されます。 次の例では、`Background Tint`ボタンの変更は*Indigo 500*:

[![Indigo 500 を選択します。](material-design-features-images/xs/06-indigo-sml.png)](material-design-features-images/xs/06-indigo.png#lightbox)

`Background Tint` カラー コードに設定されている*Indigo 500* (`#ff3f51b5`)、し、デザイナーは、この変更を反映するように、ボタンの背景色を更新します。

[![背景の濃淡の変化](material-design-features-images/xs/07-background-tint-sml.png)](material-design-features-images/xs/07-background-tint.png#lightbox)

マテリアル デザインのカラー パレットの詳細については、マテリアル デザインを参照してください。[色パレット ガイド](https://material.io/design/color/)します。

## <a name="typographic-scale"></a>印刷用のスケール

**テキストの外観**のセクション、**プロパティ**パッド**スタイル**タブにすることができます アイコンを選択します、`TextAppearance`マテリアル デザインに準拠しているスタイル仕様:

[![[スタイル] タブ](material-design-features-images/xs/08-typo-scale-icon-sml.png)](material-design-features-images/xs/08-typo-scale-icon.png#lightbox)

このアイコンをクリックすると開きます、**文字体裁スケール**ダイアログ ポップ オーバーから選択できる構成済みのテキスト スタイルの一覧が表示されます。

[![テキスト スタイルの選択](material-design-features-images/xs/09-text-appearance-sml.png)](material-design-features-images/xs/09-text-appearance.png#lightbox)

クリックすると、次の例で**ディスプレイ 1**の大きいフォントにボタンのテキストを変更**ディスプレイ 1**:

[![1 のスタイルを表示します。](material-design-features-images/xs/10-display-1-sml.png)](material-design-features-images/xs/10-display-1.png#lightbox)

テキストのスタイル、**文字体裁スケール**に従って、**テーマ**設定します。 たとえば場合、 **Light**デザイナー、利用可能なテキストのスタイルのミラーの一覧では、テーマを選択してください。、 **Light**テーマ。

[![ライト テーマ](material-design-features-images/xs/11-light-theme-sml.png)](material-design-features-images/xs/11-light-theme.png#lightbox)

## <a name="theme-editor"></a>テーマ エディター

**テーマ エディター**テーマの属性のサブセットの色の情報をカスタマイズすることができます。 開くには、**テーマ エディター**ツールバーで、ペイント ブラシ アイコンをクリックします。

[![テーマ エディター アイコン](material-design-features-images/xs/12a-theme-editor-icon-sml.png)](material-design-features-images/xs/12a-theme-editor-icon.png#lightbox)

ただし、**テーマ エディター**は、ツールバーからアクセスできるすべてのターゲット Android バージョンと API レベルでは、以下に示す機能のサブセットが使用できるは、ターゲット API レベルが API 21 (Android 5.0 より前の場合のみロリポップ)。

左側のパネル、**テーマ エディター**現在選択されているテーマを構成する色の一覧が表示されます (この例では使用、 `Default Theme`)。

[![テーマ エディター](material-design-features-images/xs/12b-theme-editor-sml.png)](material-design-features-images/xs/12b-theme-editor.png#lightbox)

左上の色を選択すると、右側のパネルは、その色を編集するために、次のタブを提供します。

-   **継承**&ndash;選択した色のスタイル継承ダイアグラムを表示し、解決済みの色とそのテーマの色に割り当てられているカラー コードを一覧表示します。

-   **カラー ピッカー** &ndash;任意の値を選択した色を変更することができます。

-   **素材パレット**&ndash;マテリアル デザインに準拠した値を選択した色を変更することができます。

-   **リソース**&ndash;テーマで、その他の既存の色リソースのいずれかに、選択した色を変更することができます。

これらのタブの詳細のいずれかを見てみましょう。

### <a name="inherit-tab"></a>タブを継承します。

次の例のように、**継承**のスタイルの継承がタブに一覧表示されます、**バック グラウンド**の色、**既定のテーマ**:

[![タブを継承します。](material-design-features-images/xs/13-inherit-sml.png)](material-design-features-images/xs/13-inherit.png#lightbox)

この例で、**既定のテーマ**を使用するスタイルを継承`@color/background_material_dark`でよりも優先されますが、`color/material_grey_850`のカラー コード値を持つ`#ff303030`します。
スタイルの継承の詳細については、次を参照してください。[スタイルとテーマ](https://developer.android.com/guide/topics/ui/themes.html#Inheritance)します。

### <a name="color-picker"></a>カラー ピッカー

次のスクリーン ショットを示しています、**カラー ピッカー**:

[![カラー ピッカー](material-design-features-images/xs/14-color-picker-sml.png)](material-design-features-images/xs/14-color-picker.png#lightbox)


この例で、**バック グラウンド**さまざまな方法で任意の値に色を変更することができます。

-   色を直接クリックするとします。
-   色合い、鮮やかさ、および明るさの値を入力します。
-   10 進数の RGB (赤、緑、青) 値を入力します。
-   選択した色のアルファ (透明度) を設定します。
-   16 進数カラー コードを直接入力します。

カラー ピッカーで選択した色は*いない*マテリアル デザイン ガイドラインに、または使用可能な色リソースのセットを制限します。

### <a name="resources"></a>リソース

**リソース** タブが存在するテーマの色リソースの一覧を提供しています。

[![リソース](material-design-features-images/xs/15-resources-sml.png)](material-design-features-images/xs/15-resources.png#lightbox)

使用して、**リソース** タブは、この色のリストにユーザーの選択を制限します。 テーマの別の部分に既に割り当てられている色リソースを選択すると、UI の 2 つの隣接する要素発生可能性があります"化"(顔色と一致している) ために注意してくださいと区別するためにユーザーを困難になります。

### <a name="material-palette"></a>素材パレット

**素材パレット**タブが表示されます、**マテリアル デザインのカラー パレット**説明[以前](#material_palette)します。 このパレットから色の値を選択すると、マテリアル デザイン ガイドラインに矛盾があるように、色の選択が制限されます。

[![素材パレット](material-design-features-images/xs/16-material-palette-sml.png)](material-design-features-images/xs/16-material-palette.png#lightbox)

### <a name="creating-a-new-theme"></a>新しいテーマの作成

次の例では、素材パレット新しいカスタム テーマの作成に使用します。 最初に、変更します、**バック グラウンド**する色*青い 900*:

[![背景を青 900 に変更します。](material-design-features-images/xs/17-change-background-to-blue-sml.png)](material-design-features-images/xs/17-change-background-to-blue.png#lightbox)

色リソースが変更されたときにポップアップ メッセージがメッセージと共に *、現在のテーマの変更が保存されて*:

[![警告未保存の変更](material-design-features-images/xs/18-unsaved-changes-sml.png)](material-design-features-images/xs/18-unsaved-changes.png#lightbox)

デザイナーでの色の変更を行ったが、この変更が保存されていません。 この時点で、次のいずれかを行うことができます。

-   クリックして**変更の破棄**を新しい色の選択 (または選択) を破棄し、テーマを元の状態を元に戻します。 

-   キーを押して **&#8984; + S**という名前の新しいテーマに変更を保存する**カスタム**します。 


## <a name="summary"></a>まとめ

このトピックでは、Xamarin.Android デザイナーで使用可能なマテリアル デザイン機能について説明します。 これは、有効にし、素材のデザイン グリッドを構成する方法、マテリアル デザインのカラー パレットを使用して、色のプロパティを編集する方法、および文字体裁スケール セレクターを使用して、テキストのプロパティを構成する方法について説明します。 テーマ エディターを使用して、マテリアル デザイン ガイドラインに準拠した新しいカスタム テーマを作成する方法も示されています。 マテリアル デザインの Xamarin.Android のサポートの詳細については、次を参照してください。[マテリアル テーマ](~/android/user-interface/material-theme.md)します。

-----


## <a name="related-links"></a>関連リンク

- [素材のテーマ](~/android/user-interface/material-theme.md)
- [素材の設計の概要](https://material.io/design/introduction)
