---
title: Android デザイナーの使用
description: このトピックでは、Xamarin.Android デザイナーのチュートリアルです。 アプリについては、小さなカラー ブラウザー以外のユーザー インターフェイスを作成する方法を示しますこのユーザー インターフェイスは、デザイナーで完全作成されます。
ms.prod: xamarin
ms.assetid: 70FF2F9A-71BD-317E-C881-A44D82DF1BD8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/10/2018
ms.openlocfilehash: 8d1dc410d5336d9c2505a18720cc7f734e838c39
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
ms.locfileid: "33798633"
---
# <a name="using-the-android-designer"></a>Android デザイナーの使用

_このトピックでは、Xamarin.Android デザイナーのチュートリアルです。アプリについては、小さなカラー ブラウザー以外のユーザー インターフェイスを作成する方法を示しますこのユーザー インターフェイスは、デザイナーで完全作成されます。_


## <a name="overview"></a>概要

Android のユーザー インターフェイスは XML ファイルを使用するか、プログラムによってコードを記述して、宣言によって作成されることができます。 Xamarin.Android デザイナーでは、作成し、XML ファイルを手作業で編集の面倒に対処することがなく宣言型のレイアウトを視覚的に、変更をすることができます。 デザイナーには、リアルタイムのフィードバックは、開発者はでき、デバイスまたはエミュレーターにアプリケーションを再配置することがなく、UI の変更を評価も用意されています。 これは、時間を短縮できます Android UI 開発大幅。 この記事では、Xamarin.Android デザイナーを使用して、視覚的に、ユーザー インターフェイスを作成する方法を示すチュートリアルご紹介します。


## <a name="walkthrough"></a>チュートリアル

このチュートリアルでは、Android デザイナーを使用して、色、その名前、およびその RGB 値の一覧を表示する例色ブラウザー アプリ向けのユーザー インターフェイスを作成します。 デザイン画面にウィジェットを追加する方法と、これらのウィジェットを視覚的にレイアウトする方法に紹介します。 その後、または、デザイナーを使用して、デザイン画面上で対話的にウィジェットを変更する方法を用いて説明**プロパティ パッド**です。 最後に、アプリを実行すると、デザインがどのようおが表示されます。

開始しましょう!


### <a name="creating-a-new-project"></a>新規プロジェクトの作成

最初の手順では、新しい Xamarin.Android プロジェクトを作成します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio を起動し、をクリックして**新しいプロジェクト.** を選択し、 **Visual C\# > Android > Android アプリ (Xamarin)** テンプレート。

[![Android の空のアプリケーション](designer-walkthrough-images/vs/01-android-app-sml.w157.png)](designer-walkthrough-images/vs/01-android-app.w157.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac とクリックの Visual Studio を起動して**新しいソリューションをしています.**.選択、 **Android アプリ**テンプレートとクリック**次**:

[![Android の空のアプリケーション](designer-walkthrough-images/xs/01-android-app-sml.png)](designer-walkthrough-images/xs/01-android-app.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

アプリの新しい名前**DesignerWalkthrough**  をクリック**OK**です。

[![名前のアプリ](designer-walkthrough-images/vs/02-name-app-sml.png)](designer-walkthrough-images/vs/02-name-app.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

アプリの新しい名前**DesignerWalkthrough**です。 **ターゲット プラットフォーム****最新かつ最大** をクリック**次へ**:

[![名前のアプリ](designer-walkthrough-images/xs/02-designer-walkthrough-sml.png)](designer-walkthrough-images/xs/02-designer-walkthrough.png#lightbox)

次のダイアログの画面でクリックして**作成**です。

-----



### <a name="adding-a-layout"></a>レイアウトを追加します。

作成してみましょう、 **LinearLayout**をユーザー インターフェイス要素を保持するために使用されます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio を右クリックして**リソース/レイアウト**で、**ソリューション エクスプ ローラー**選択**追加 > 新しい項目の追加しています.**.**新しい項目の追加**ダイアログで、 **Android レイアウト**です。 ファイルの名前を付けます**ListItem.axml**  をクリック**追加**:

[![新しいレイアウト](designer-walkthrough-images/vs/03-new-layout-sml.w157.png)](designer-walkthrough-images/vs/03-new-layout.w157.png#lightbox)

新しい**ListItem**レイアウトは、デザイナーに表示されます。

[![デザイナー ビュー](designer-walkthrough-images/vs/04-designer-view-sml.png)](designer-walkthrough-images/vs/04-designer-view.png#lightbox)

クリックして、**ソース**このレイアウトの XML ソースを表示するデザイナーの下部にあるタブ。

[![デザイナーの XML](designer-walkthrough-images/vs/05-designer-xml-sml.png)](designer-walkthrough-images/vs/05-designer-xml.png#lightbox)

**ビュー**  メニューのをクリックして**その他のウィンドウ > ドキュメント アウトライン**を開くには、 **ドキュメント アウトライン**です。 **ドキュメント アウトライン**レイアウト現在 1 つに含まれる表示**LinearLayout**ウィジェット。

[![ドキュメント アウトライン](designer-walkthrough-images/vs/06-document-outline-sml.png)](designer-walkthrough-images/vs/06-document-outline.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac 用 Visual Studio で、右クリック**リソース/レイアウト**で、**ソリューション**を埋め込むし、選択**追加 > 新しいファイル.**.**新しいファイル**ダイアログで、 **Android > レイアウト**です。 ファイルの名前を付けます**ListItem**  をクリック**新規**:

[![新しいレイアウト](designer-walkthrough-images/xs/03-new-layout-sml.png)](designer-walkthrough-images/xs/03-new-layout.png#lightbox)

新しい**ListItem**レイアウトは、デザイナーに表示されます。

[![デザイナー ビュー](designer-walkthrough-images/xs/04-designer-view-sml.png)](designer-walkthrough-images/xs/04-designer-view.png#lightbox)

クリックして、**ソース**このレイアウトの XML ソースを表示するデザイナーの下部にあるタブ。 クリックすると、 **[ドキュメント アウトライン**] タブ、右側には、レイアウトが現在、1 つを含むことを示します**LinearLayout**ウィジェット。

[![デザイナーの XML](designer-walkthrough-images/xs/05-designer-xml-sml.png)](designer-walkthrough-images/xs/05-designer-xml.png#lightbox)

-----



### <a name="creating-the-list-item-user-interface"></a>リスト項目のユーザー インターフェイスを作成します。

次に、行う色ブラウザー アプリ向けのユーザー インターフェイスを作成します。
クリックして、**デザイナー**  タブのデザイン画面に戻ります。
**ツールボックス**、下方向にスクロール、**イメージ & メディア**セクションし、各項目を熟読が見つかるまで、 *ImageView*:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![ImageView を見つける](designer-walkthrough-images/vs/07-locate-imageview-sml.png)](designer-walkthrough-images/vs/07-locate-imageview.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![ImageView を見つける](designer-walkthrough-images/xs/06-locate-imageview-sml.png)](designer-walkthrough-images/xs/06-locate-imageview.png#lightbox)

-----

入力する代わりに、 *ImageView*を検索する検索バーに、`ImageView`ウィジェット。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![ImageView 検索](designer-walkthrough-images/vs/08-imageview-search-sml.png)](designer-walkthrough-images/vs/08-imageview-search.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![ImageView 検索](designer-walkthrough-images/xs/07-imageview-search-sml.png)](designer-walkthrough-images/xs/07-imageview-search.png#lightbox)

-----

このドラッグ`ImageView`デザイン サーフェイスに。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![キャンバス上 ImageView](designer-walkthrough-images/vs/09-imageview-on-canvas-sml.png)](designer-walkthrough-images/vs/09-imageview-on-canvas.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![キャンバス上 ImageView](designer-walkthrough-images/xs/08-imageview-on-canvas-sml.png)](designer-walkthrough-images/xs/08-imageview-on-canvas.png#lightbox)

-----

これは、`ImageView`色のブラウザー アプリケーション内での色の見本の表示に使用されます。

次に、ドラッグ、`LinearLayout (Vertical)`からウィジェット、**ツールボックス**デザイナーにします。 青のアウトラインが追加される境界を示す注`LinearLayout`、および**ドキュメント アウトライン**のすぐ下に配置されていることを示しています`imageView1 (ImageView)`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![青のアウトライン](designer-walkthrough-images/vs/10-blue-outline-sml.png)](designer-walkthrough-images/vs/10-blue-outline.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![青のアウトライン](designer-walkthrough-images/xs/10-blue-outline-sml.png)](designer-walkthrough-images/xs/10-blue-outline.png#lightbox)

-----

選択した場合、`ImageView`を囲む青のアウトラインに移動、デザイナーで、 `ImageView`; で、 **ドキュメント アウトライン**に移動`imageView1 (ImageView)`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![ImageView を選択します。](designer-walkthrough-images/vs/11-select-imageview-sml.png)](designer-walkthrough-images/vs/11-select-imageview.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![ImageView を選択します。](designer-walkthrough-images/xs/11-select-imageview-sml.png)](designer-walkthrough-images/xs/11-select-imageview.png#lightbox)

-----

次に、ドラッグ、`Text (Large)`からウィジェット、**ツールボックス**、新しく追加されたに`LinearLayout`です。 新しいウィジェットを挿入する位置を示すためにデザイナーが緑を使用して通知を示しています。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![緑色の強調表示](designer-walkthrough-images/vs/12-green-highlight-sml.png)](designer-walkthrough-images/vs/12-green-highlight.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![緑色の強調表示](designer-walkthrough-images/xs/12-green-highlight-sml.png)](designer-walkthrough-images/xs/12-green-highlight.png#lightbox)

-----

次に、追加、`Text (Small)`ウィジェット下、`Text (Large)`ウィジェット。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![小さなテキスト ウィジェットを追加します。](designer-walkthrough-images/vs/13-add-small-text-sml.png)](designer-walkthrough-images/vs/13-add-small-text.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![小さなテキスト ウィジェットを追加します。](designer-walkthrough-images/xs/13-add-small-text-sml.png)](designer-walkthrough-images/xs/13-add-small-text.png#lightbox)

-----

この時点では、デザイナーには、次のスクリーン ショットがようになります。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![デザイナーのレイアウト](designer-walkthrough-images/vs/14-raw-layout-sml.png)](designer-walkthrough-images/vs/14-raw-layout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![デザイナーのレイアウト](designer-walkthrough-images/xs/14-raw-layout-sml.png)](designer-walkthrough-images/xs/14-raw-layout.png#lightbox)

-----

場合、2 つ`textView`ウィジェットを inside されない`linearLayout1`、ドラッグすることを`linearLayout1`で、 **ドキュメント アウトライン**の前のスクリーン ショットに示すように表示されるように配置して (の下にインデント`linearLayout1`)。



### <a name="arranging-the-user-interface"></a>ユーザー インターフェイスを配置します。

表示する UI を変更してみましょう、`ImageView`左側の 2 つに`TextView`ウィジェットが積み上げの右側に、`ImageView`です。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  `ImageView` を選択します。

2.  **プロパティ ウィンドウ**、 をクリックして、**項目別**アイコンをクリックして下にスクロールを並べ替え、**レイアウト - ビュー**セクション。

3.  変更、`layout_width`設定`wrap_content`:

![ラップ内容設定](designer-walkthrough-images/vs/15-wrap-content.png "ラップ コンテンツ")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  `ImageView`選択するをクリックして、**プロパティ**タブです。

2.  すぐ下、**プロパティ** タブで、をクリックして**レイアウト**です。

3.  下方向にスクロール**ビュー**を変更して、`Width`設定`wrap_content`:

[![ラップ コンテンツ](designer-walkthrough-images/xs/15-wrap-content-sml.png)](designer-walkthrough-images/xs/15-wrap-content.png#lightbox)

-----

別の方法を変更する、`Width`設定では幅設定を切り替えるウィジェットの右側にある三角形をクリックして、 `wrap_content`:

[![幅を設定するドラッグ](designer-walkthrough-images/xs/16-width-arrow-sml.png)](designer-walkthrough-images/xs/16-width-arrow.png#lightbox)

返しますの三角形を再度クリックすると、`Width`設定`match_parent`です。

次に切り替え、 **ドキュメント アウトライン**のルートを選択して`LinearLayout`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![LinearLayout のルートを選択します。](designer-walkthrough-images/vs/16-root-linearlayout-sml.png)](designer-walkthrough-images/vs/16-root-linearlayout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![LinearLayout のルートを選択します。](designer-walkthrough-images/xs/17-root-linearlayout-sml.png)](designer-walkthrough-images/xs/17-root-linearlayout.png#lightbox)

-----
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

ルートと`LinearLayout`に戻り、選択されている、**プロパティ**ウィンドウで、をクリックして、**アルファベット順**が見つかるまでスクロールしてアイコンを並べ替える`orientation`です。 変更、`orientation`設定`horizontal`:

![水平方向の向きを選択して](designer-walkthrough-images/vs/17-horizontal-orientation.png "水平方向の選択")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

ルートと`LinearLayout`に戻り、選択されている、**プロパティ** タブでをクリックし、**ウィジェット**です。 変更、`Orientation`設定`horizontal`:

[![水平方向を選択します。](designer-walkthrough-images/xs/18-horizontal-orientation-sml.png)](designer-walkthrough-images/xs/18-horizontal-orientation.png#lightbox)

-----

この時点では、デザイナーには、次のスクリーン ショットがようになります。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![デザイナーのレイアウト](designer-walkthrough-images/vs/18-designer-layout-sml.png)](designer-walkthrough-images/vs/18-designer-layout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![デザイナーのレイアウト](designer-walkthrough-images/xs/19-designer-layout-sml.png)](designer-walkthrough-images/xs/19-designer-layout.png#lightbox)

-----


### <a name="modifying-the-spacing"></a>間隔を変更します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

次に、空き領域を増やすのウィジェットの間で UI のパディングとマージンの設定を変更します。 選択、 `ImageView`、 をクリックして、 **Categorized**にある検索アイコン、**プロパティ**下にスクロールしてウィンドウ、**レイアウト**セクションです。 変更、`Min Height`に`70dp`、`Min Width`に`50dp`、および`padding`に`10dp`です。 これには、すべての側面の周辺のスペースが適用されます、`ImageView`垂直方向に elongates とします。

[![余白を設定します。](designer-walkthrough-images/vs/19-padding-widths-sml.png)](designer-walkthrough-images/vs/19-padding-widths.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

次に、空き領域を増やすのウィジェットの間で UI のパディングとマージンの設定を変更します。 選択、 `ImageView`  をクリックし、**レイアウト** タブの下にある**プロパティ**です。 変更、`Padding`に`10dp`、`Min Width`に`50dp`、および`Min Height`に`70dp`です。 これには、すべての側面の周辺のスペースが適用されます、`ImageView`垂直方向に elongates とします。

[![余白を設定します。](designer-walkthrough-images/xs/20-padding-widths-sml.png)](designer-walkthrough-images/xs/20-padding-widths.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

下、左、右、および上の設定の余白の設定されません個別に値を入力して、 `paddingBottom`、 `paddingLeft`、 `paddingRight`、および`paddingTop`フィールドに、それぞれします。
たとえば、設定、`paddingLeft`フィールドを`5dp`と`paddingBottom`、 `paddingRight`、および`paddingTop`フィールドを`10dp`:

[![カスタムの余白設定](designer-walkthrough-images/vs/20-custom-padding-sml.png)](designer-walkthrough-images/vs/20-custom-padding.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

上、右、下、および左の余白の設定をする個別に設定に値を入力して、 `Top`、 `Right`、 `Bottom`、および`Left`フィールドをそれぞれパディングです。 たとえば、設定、`Left`パディング値に`5dp`と`Top`、 `Right`、および`Bottom`余白の値を`10dp`です。 なお、`Padding`設定をこれらの値のコンマ区切りのリストに変更します。

[![カスタムの余白設定](designer-walkthrough-images/xs/21-custom-padding-sml.png)](designer-walkthrough-images/xs/21-custom-padding.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

位置を次に、調整、`LinearLayout`ウィジェットを含む 2 つ`TextView`ウィジェット。 **ドキュメント アウトライン**`linearLayout1`です。 **プロパティ**ウィンドウで、**レイアウト - ビュー**セクションです。 設定`layout_marginBottom`、 `layout_marginLeft`、 `layout_marginRight`、および`layout_marginTop`に`5dp`、 `5dp`、 `0dp`、および`5dp`それぞれ。

[![余白を設定します。](designer-walkthrough-images/vs/21-margins-sml.png)](designer-walkthrough-images/vs/21-margins.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

位置を次に、調整、`LinearLayout`ウィジェットを含む 2 つ`TextView`ウィジェット。 **ドキュメント アウトライン**`linearLayout1`です。 **プロパティ**ペインで、**レイアウト**タブです。下方向にスクロール、**ビュー**セクションし、設定、 `Left`、 `Top`、 `Right`、および`Bottom`に余白`5dp`、 `5dp`、 `0dp`、および`5dp`それぞれ。

[![余白を設定します。](designer-walkthrough-images/xs/22-margins-sml.png)](designer-walkthrough-images/xs/22-margins.png#lightbox)

-----



### <a name="removing-the-default-image"></a>既定のイメージを削除します。

使用しているため、 `ImageView` (イメージではなく) の色の表示へのテンプレートによって追加された既定のイメージ ソースを削除します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  `ImageView` を選択します。

2.  **プロパティ**、検索、`src`フィールドです。

3.  クリア、`src`を空白に設定します。

![ImageView src 設定をオフに](designer-walkthrough-images/vs/22-clear-img-src.png "ImageView src の設定をクリア")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  `ImageView` を選択します。

2.  クリックして、**ウィジェット** タブの下にある**プロパティ**です。

3.  クリア、`Src`を空白に設定します。

[![ImageView src の設定をクリアします。](designer-walkthrough-images/xs/23-clear-src-sml.png)](designer-walkthrough-images/xs/23-clear-src.png#lightbox)

-----


### <a name="adding-a-listview-container"></a>ListView のコンテナーを追加します。

これで、 **ListItem**レイアウトを定義すると、追加、`ListView`をメインのレイアウトにします。 これは、`ListView`の一覧を含む**Listitem**です。
**ツールボックス**、検索、`ListView`ウィジェットし、デザイン画面にドラッグします。 `ListView`デザイナーでは空白になりますが選択されているときの枠線が示されている青色の行を除く。 表示することができます、 **ドキュメント アウトライン**ことを確認する、 **ListView**が正しく追加されました。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![新しい ListView](designer-walkthrough-images/vs/23-new-listview.png "新しい ListView")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![新しい ListView](designer-walkthrough-images/xs/24-new-listview-sml.png)](designer-walkthrough-images/xs/24-new-listview.png#lightbox)

-----

既定では、`ListView`が与えられます、`Id`値`@+id/listView1`です。
開く、**ウィジェット** タブの下にある**プロパティ**を変更して、`Id`に`@+id/myListView`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![MyListView に id の名前を変更](designer-walkthrough-images/vs/24-change-id.png "myListView する id を名前の変更")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![MyListView に id の名前を変更します。](designer-walkthrough-images/xs/25-change-id-sml.png)](designer-walkthrough-images/xs/25-change-id.png#lightbox)

-----

この時点では、ユーザー インターフェイスが使用できるようにします。



### <a name="running-the-application"></a>アプリケーションの実行


開いている**MainActivity.cs**を次のコードに置き換えます。

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;
using System.Collections.Generic;

namespace DesignerWalkthrough
{
    [Activity (Label = "DesignerWalkthrough", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        List<ColorItem> colorItems = new List<ColorItem> ();
        ListView listView;

        protected override void OnCreate (Bundle savedInstanceState)
        {
            base.OnCreate (savedInstanceState);

            SetContentView (Resource.Layout.Main);
            listView = FindViewById<ListView> (Resource.Id.myListView);

            colorItems.Add (new ColorItem () { Color = Android.Graphics.Color.DarkRed,
                                               ColorName = "Dark Red", Code = "8B0000" });
            colorItems.Add (new ColorItem () { Color = Android.Graphics.Color.SlateBlue,
                                               ColorName = "Slate Blue", Code = "6A5ACD" });
            colorItems.Add (new ColorItem () { Color = Android.Graphics.Color.ForestGreen,
                                               ColorName = "Forest Green", Code = "228B22" });

            listView.Adapter = new ColorAdapter (this, colorItems);
        }
    }
    public class ColorAdapter : BaseAdapter<ColorItem>
    {
        List<ColorItem> items;
        Activity context;
        public ColorAdapter(Activity context, List<ColorItem> items)
            : base()
        {
            this.context = context;
            this.items = items;
        }
        public override long GetItemId(int position)
        {
            return position;
        }
        public override ColorItem this[int position]
        {
            get { return items[position]; }
        }
        public override int Count
        {
            get { return items.Count; }
        }
        public override View GetView (int position, View convertView, ViewGroup parent)
        {
            var item = items[position];

            View view = convertView;
            if (view == null) // no view to re-use, create new
                view = context.LayoutInflater.Inflate(Resource.Layout.ListItem, null);
            view.FindViewById<TextView>(Resource.Id.textView1).Text = item.ColorName;
            view.FindViewById<TextView>(Resource.Id.textView2).Text = item.Code;
            view.FindViewById<ImageView>(Resource.Id.imageView1).SetBackgroundColor(item.Color);

            return view;
        }
    }

    public class ColorItem
    {
        public string ColorName { get; set; }
        public string Code { get; set; }
        public Android.Graphics.Color Color { get; set; }
    }
}

```

この例では、カスタム`ListView`を色の情報を読み込んで先ほど作成した UI にこのデータを表示するアダプター。 ためにこの例の短い、色の情報を一覧で、ハードコーディングしますが、データ ソースから色の情報を抽出するかを即座に計算するアダプターを変更できませんできます。 詳細については`ListView`アダプター, を参照してください[の Listview およびアダプター](~/android/user-interface/layouts/list-view/index.md)です。

アプリケーションをビルドして実行します。 次のスクリーン ショットは、デバイスで実行されているときのアプリの外観の例を示します。

[![最終的なスクリーン ショット](designer-walkthrough-images/xs/26-final-screenshot-sml.png)](designer-walkthrough-images/xs/26-final-screenshot.png#lightbox)



## <a name="summary"></a>まとめ

この記事ではとおし、Visual Studio for Mac で Xamarin.Android デザイナーを使用して、ユーザー インターフェイスを作成する方法です。 次に、一覧で 1 つの項目のインターフェイスを作成する方法を説明します。
その過程では、ウィジェットを追加して、視覚的に、レイアウトにすると、リソースの割り当て、それらのウィジェットのさまざまなプロパティを設定する方法を説明しました。 結論として、アプリケーションの例で、デザイナーで作成したインターフェイスの実行方法示されています。
