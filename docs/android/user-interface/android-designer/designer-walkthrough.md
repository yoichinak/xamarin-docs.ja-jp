---
title: Android Designer の使用
description: この記事は、Android Designer のチュートリアルです。 これは、小さいカラーブラウザーアプリのユーザーインターフェイスを作成する方法を示しています。このユーザーインターフェイスは、デザイナーで完全に作成されます。
ms.prod: xamarin
ms.assetid: 70FF2F9A-71BD-317E-C881-A44D82DF1BD8
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/25/2018
ms.openlocfilehash: df83bdfcc847b07754a349060c9be1613efd0b08
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029524"
---
# <a name="using-the-xamarinandroid-designer"></a>Android Designer の使用

_この記事は、Android Designer のチュートリアルです。これは、小さいカラーブラウザーアプリのユーザーインターフェイスを作成する方法を示しています。このユーザーインターフェイスは、デザイナーで完全に作成されます。_

## <a name="overview"></a>概要

Android ユーザーインターフェイスは、XML ファイルを使用するか、コードを記述することによって、宣言によって作成できます。 Android Designer を使用すると、開発者は XML ファイルを手動で編集する必要なく、宣言型のレイアウトを視覚的に作成および変更できます。 また、デザイナーでは、アプリケーションをデバイスまたはエミュレーターに再配置しなくても、UI の変更を評価できるリアルタイムのフィードバックが提供されます。 これらのデザイナー機能を使用すると、Android UI の開発を大幅に高速化できます。
この記事では、Android Designer を使用してユーザーインターフェイスを視覚的に作成する方法について説明します。

> [!TIP]
> 新しいリリースの Visual Studio では、Android Designer 内で .xml ファイルを開くことができます。
>
> Android Designer では、.axml ファイルと .xml ファイルの両方がサポートされています。

## <a name="walkthrough"></a>チュートリアル

このチュートリアルの目的は、Android Designer を使用して、サンプルのカラーブラウザーアプリ用のユーザーインターフェイスを作成することです。 カラーブラウザーアプリでは、色、名前、および RGB 値の一覧が表示されます。 **デザインサーフェイス**にウィジェットを追加する方法と、これらのウィジェットを視覚的にレイアウトする方法についても説明します。 その後、**デザインサーフェイス**で対話的にウィジェットを変更する方法、またはデザイナーの **[プロパティ]** ペインを使用してウィジェットを変更する方法を学習します。 最後に、デバイスまたはエミュレーターでアプリを実行すると、デザインがどのように見えるかを確認できます。

<!-- markdownlint-disable MD001 -->

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

### <a name="creating-a-new-project"></a>新しいプロジェクトの作成

最初の手順では、新しい Xamarin. Android プロジェクトを作成します。 Visual Studio を起動し、 **[新しいプロジェクト...]** をクリックし、[ **visual C\# > Android > Android アプリ (Xamarin)** ] テンプレートを選択します。
新しいアプリデザイナの**チュートリアル**に名前を指定し、[ **OK]** をクリックします。

[Android の空のアプリを![](designer-walkthrough-images/vs/01-android-app-w158-sml.png)](designer-walkthrough-images/vs/01-android-app-w158.png#lightbox)

**[新しい Android アプリ]** ダイアログで、 **[空のアプリ]** を選択し、 **[OK]** をクリックします。

[Android の空のアプリテンプレートを選択![](designer-walkthrough-images/vs/02-blank-app-w158-sml.png)](designer-walkthrough-images/vs/02-blank-app-w158.png#lightbox)

### <a name="adding-a-layout"></a>レイアウトの追加

次の手順では、ユーザーインターフェイス要素を保持する**LinearLayout**を作成します。 **ソリューションエクスプローラー**で **[リソース/レイアウト]** を右クリックし、 **[新しい項目の追加 >]** を選択します。 **[新しい項目の追加]** ダイアログで、 **[Android レイアウト]** を選択します。 ファイルに**list_item**という名前を指定し、 **[追加]** をクリックします。

[新しいレイアウトの![](designer-walkthrough-images/vs/03-new-layout-w158-sml.png)](designer-walkthrough-images/vs/03-new-layout-w158.png#lightbox)

新しい**list_item**レイアウトがデザイナーに表示されます。 **List_item**の*デザインサーフェイス*が左側のウィンドウに表示され、右側のウィンドウに XML ソースが表示されている &ndash;、2つのペインが表示されていることに注意してください。 次の2つのウィンドウの間にある **[スワップウィンドウ]** アイコンをクリックすると、**デザインサーフェイス**ペインと**ソース**ペインの位置を入れ替えることができます。

[![デザイナービュー](designer-walkthrough-images/vs/04-designer-view-w158-sml.png)](designer-walkthrough-images/vs/04-designer-view-w158.png#lightbox)

**[表示]** メニューの **[その他の Windows > ドキュメントアウトライン]** をクリックして、**ドキュメントアウトライン**を開きます。 **ドキュメントアウトライン**には、現在、レイアウトに1つの**LinearLayout**ウィジェットが含まれていることが示されています。

[![ドキュメントアウトライン](designer-walkthrough-images/vs/06-document-outline-w158-sml.png)](designer-walkthrough-images/vs/06-document-outline-w158.png#lightbox)

次の手順では、この `LinearLayout`内でカラーブラウザーアプリのユーザーインターフェイスを作成します。

### <a name="creating-the-list-item-user-interface"></a>リスト項目のユーザーインターフェイスを作成する

**ツールボックス**ウィンドウが表示されていない場合は、左側の **[ツールボックス]** タブをクリックします。 **[ツールボックス]** で、 **[Images &]** セクションまで下にスクロールし、`ImageView`を見つけるまで下にスクロールします。

[ImageView を検索![には](designer-walkthrough-images/vs/07-locate-imageview-w158-sml.png)](designer-walkthrough-images/vs/07-locate-imageview-w158.png#lightbox)

または、[検索] バーに「 *Imageview* 」と入力して、`ImageView`を検索することもできます。

[ImageView 検索の![](designer-walkthrough-images/vs/08-imageview-search-w158-sml.png)](designer-walkthrough-images/vs/08-imageview-search-w158.png#lightbox)

この `ImageView` をデザインサーフェイスにドラッグします (この `ImageView` は、カラーブラウザーアプリで色の見本を表示するために使用されます)。

[キャンバス上の![ImageView](designer-walkthrough-images/vs/09-imageview-on-canvas-w158-sml.png)](designer-walkthrough-images/vs/09-imageview-on-canvas-w158.png#lightbox)

次に、 **[ツールボックス]** から `LinearLayout (Vertical)` ウィジェットをデザイナーにドラッグします。 青いアウトラインが、追加された `LinearLayout`の境界を示すことに注意してください。 **ドキュメントアウトライン**では、[`imageView1 (ImageView)`] の下にある `LinearLayout`の子であることが示されています。

[![青のアウトライン](designer-walkthrough-images/vs/10-blue-outline-w158-sml.png)](designer-walkthrough-images/vs/10-blue-outline-w158.png#lightbox)

デザイナーで `ImageView` を選択すると、青い輪郭が `ImageView`を囲むように移動します。 さらに、選択項目は**ドキュメントアウトライン**の `imageView1 (ImageView)` に移動します。

[ImageView を選択![](designer-walkthrough-images/vs/11-select-imageview-w158-sml.png)](designer-walkthrough-images/vs/11-select-imageview-w158.png#lightbox)

次に、`Text (Large)` ウィジェットを **[ツールボックス]** から新しく追加した `LinearLayout`にドラッグします。 デザイナーでは、緑色の強調表示を使用して、新しいウィジェットを挿入する場所を指定します。

[![の緑色の強調表示](designer-walkthrough-images/vs/12-green-highlight-w158-sml.png)](designer-walkthrough-images/vs/12-green-highlight-w158.png#lightbox)

次に、`Text (Large)` ウィジェットの下に `Text (Small)` ウィジェットを追加します。

[小さいテキストウィジェットを追加![には](designer-walkthrough-images/vs/13-add-small-text-w158-sml.png)](designer-walkthrough-images/vs/13-add-small-text-w158.png#lightbox)

この時点で、デザイナー画面は次のスクリーンショットのようになります。

[![デザイナーのレイアウト](designer-walkthrough-images/vs/14-raw-layout-w158-sml.png)](designer-walkthrough-images/vs/14-raw-layout-w158.png#lightbox)

2つの `textView` ウィジェットが `linearLayout1`内にない場合は、それらを**ドキュメントアウトライン**の `linearLayout1` にドラッグして、前のスクリーンショットのように表示することができます (`linearLayout1`の下にインデントされます)。

### <a name="arranging-the-user-interface"></a>ユーザーインターフェイスの配置

次の手順では、UI を変更して左側に `ImageView` を表示し、2つの `TextView` ウィジェットを `ImageView`の右側に積み重ねます。

1. `ImageView`を選択します。

2. **プロパティウィンドウ**の 検索 ボックスに「 *width* 」と入力し、**レイアウトの幅** を検索します。

3. **[レイアウトの幅]** 設定を `wrap_content`に変更します。

![コンテンツの折り返しの設定](designer-walkthrough-images/vs/15-wrap-content-w158.png)

`Width` 設定を変更するもう1つの方法は、ウィジェットの右側にある三角形をクリックして、[幅] の設定を `wrap_content`に切り替えることです。

![ドラッグして幅を設定](designer-walkthrough-images/vs/15b-width-arrow-w158.png)

もう一度三角形をクリックすると、`Width` 設定が `match_parent`に戻ります。 次に、 **[ドキュメントアウトライン]** ウィンドウにアクセスして、ルート `LinearLayout`を選択します。

[![ルート LinearLayout の選択](designer-walkthrough-images/vs/16-root-linearlayout-w158-sml.png)](designer-walkthrough-images/vs/16-root-linearlayout-w158.png#lightbox)

ルート `LinearLayout` を選択した状態で、**プロパティ** ペインに戻り、検索 ボックスに「 *orientation* 」と入力して、**印刷の向き** 設定を探します。 **向き**を `horizontal`に変更します。

![水平方向の選択](designer-walkthrough-images/vs/17-horizontal-orientation-w158.png)

この時点で、デザイナー画面は次のスクリーンショットのようになります。
`TextView` ウィジェットが `ImageView`の右側に移動したことに注意してください。

[![デザイナーのレイアウト](designer-walkthrough-images/vs/18-designer-layout-w158-sml.png)](designer-walkthrough-images/vs/18-designer-layout-w158.png#lightbox)

### <a name="modifying-the-spacing"></a>間隔の変更

次の手順では、UI の埋め込みと余白の設定を変更して、ウィジェットの間により多くの領域を確保します。 デザイン画面で `ImageView` を選択します。 **プロパティ** ペインで、検索 ボックスに「`min`」と入力します。 最小の**高さ**と `50dp` の**最小幅**の `70dp` を入力してください:

[高さと幅を設定![](designer-walkthrough-images/vs/18b-set-height-width-sml.png)](designer-walkthrough-images/vs/18b-set-height-width.png#lightbox)

**プロパティ** ペインで、検索 ボックスに「`padding`」と入力し、**余白**として「`10dp`」と入力します。 これらの `minHeight`、`minWidth`、および `padding` の設定では、`ImageView` のすべての辺の周囲にパディングが追加され、垂直方向に elongate されます。 次の値を入力すると、レイアウト XML が変更されます。

[パディングを設定![には](designer-walkthrough-images/vs/19-padding-widths-w158-sml.png)](designer-walkthrough-images/vs/19-padding-widths-w158.png#lightbox)

下、左、右、および上にある余白の設定は、**埋め込み** の下に値を入力することによって個別に設定できます。また、埋め込み、**右** **へ**、埋め込み**の各フィールド**にそれぞれ設定できます。
たとえば、 **[左余白]** フィールドを `5dp` に、 **[埋め]** 込み の下に [余白 **]、[** 余白]、 **[余白]** を `10dp`に設定します。

[カスタムパディング設定の![](designer-walkthrough-images/vs/20-custom-padding-w158-sml.png)](designer-walkthrough-images/vs/20-custom-padding-w158.png#lightbox)

次に、2つの `TextView` ウィジェットを含む `LinearLayout` ウィジェットの位置を調整します。 **ドキュメントアウトライン**で、[`linearLayout1`] を選択します。 **プロパティ** ウィンドウで、検索 ボックスに「`margin`」と入力します。 **レイアウトの余白の下部**、レイアウトの余白 (**左**)、およびレイアウトの余白を **[上]** の [`5dp`] に設定します。 **レイアウトの余白**を `0dp`に設定します。

[余白の設定![](designer-walkthrough-images/vs/21-margins-w158-sml.png)](designer-walkthrough-images/vs/21-margins-w158.png#lightbox)

### <a name="removing-the-default-image"></a>既定のイメージの削除

`ImageView` はイメージではなく色を表示するために使用されるため、次の手順では、テンプレートによって追加された既定のイメージソースを削除します。

1. **デザイナー画面**で `ImageView` を選択します。

2. **[プロパティ]** で、検索ボックスに「 *src* 」と入力します。

3. **Src**プロパティ設定の右側にある小さな四角形をクリックし、 **[リセット]** を選択します。

[ImageView src 設定をクリア![には](designer-walkthrough-images/vs/22-clear-img-src-w158-sml.png)](designer-walkthrough-images/vs/22-clear-img-src-w158.png#lightbox)

これにより、その `ImageView`のソース XML から `android:src="@android:drawable/ic_menu_gallery"` が削除されます。

### <a name="adding-a-listview-container"></a>ListView コンテナーの追加

**List_item**レイアウトが定義されたので、次の手順として、メインレイアウトに `ListView` を追加します。 この `ListView` には、 **list_item**の一覧が含まれます。 

**ソリューションエクスプローラー**で、 **Resources/layout/activity_main**を開きます。 **[ツールボックス]** で `ListView` ウィジェットを見つけて、**デザインサーフェイス**にドラッグします。 デザイナー内の `ListView` は空白になります。ただし、選択されている場合は、境界線を輪郭する青い線が使用されます。 **ドキュメントアウトライン**を表示して、 **ListView**が正しく追加されたことを確認できます。

[新しい ListView の![](designer-walkthrough-images/vs/23-new-listview-w158-sml.png)](designer-walkthrough-images/vs/23-new-listview-w158.png#lightbox)

既定では、`ListView` には `@+id/listView1`の `Id` 値が割り当てられます。
`listView1` が**ドキュメントアウトライン**でまだ選択されている状態で、**プロパティ**ペインを開き、[並べ替え] をクリックし**て**、 **[カテゴリ]** を選択します。
**Main**を開き、 **Id**プロパティを見つけて、その値を `@+id/myListView`に変更します。

[id の名前を myListView に変更![](designer-walkthrough-images/vs/24-change-id-w158-sml.png)](designer-walkthrough-images/vs/24-change-id-w158.png#lightbox)

この時点で、ユーザーインターフェイスを使用する準備が整いました。

### <a name="running-the-application"></a>アプリケーションの実行

**MainActivity.cs**を開き、そのコードを次のコードに置き換えます。

```csharp
using Android.App;
using Android.Widget;
using Android.Views;
using Android.OS;
using Android.Support.V7.App;
using System.Collections.Generic;

namespace DesignerWalkthrough
{
    [Activity(Label = "@string/app_name", Theme = "@style/AppTheme", MainLauncher = true)]
    public class MainActivity : AppCompatActivity
    {
        List<ColorItem> colorItems = new List<ColorItem>();
        ListView listView;

        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(savedInstanceState);

            // Set our view from the "main" layout resource
            SetContentView(Resource.Layout.activity_main);
            listView = FindViewById<ListView>(Resource.Id.myListView);

            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.DarkRed,
                ColorName = "Dark Red",
                Code = "8B0000"
            });
            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.SlateBlue,
                ColorName = "Slate Blue",
                Code = "6A5ACD"
            });
            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.ForestGreen,
                ColorName = "Forest Green",
                Code = "228B22"
            });

            listView.Adapter = new ColorAdapter(this, colorItems);
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
        public override View GetView(int position, View convertView, ViewGroup parent)
        {
            var item = items[position];

            View view = convertView;
            if (view == null) // no view to re-use, create new
                view = context.LayoutInflater.Inflate(Resource.Layout.list_item, null);
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

このコードでは、カスタムの `ListView` アダプターを使用して、色情報を読み込み、このデータを作成したばかりの UI に表示します。 この例を短くしておくために、色情報はリストにハードコーディングされていますが、データソースからカラー情報を抽出したり、その場で計算したりするようにアダプターを変更することもできます。 `ListView` アダプターの詳細については、「 [ListView](~/android/user-interface/layouts/list-view/index.md)」を参照してください。

アプリケーションをビルドして実行します。 次のスクリーンショットは、デバイスで実行しているときにアプリがどのように表示されるかを示しています。

[![最終的なスクリーンショット](designer-walkthrough-images/vs/25-final-screenshot-sml.png)](designer-walkthrough-images/vs/25-final-screenshot.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

### <a name="creating-a-new-project"></a>新しいプロジェクトの作成

最初の手順では、新しい Xamarin. Android プロジェクトを作成します。

Visual Studio for Mac を起動し、 **[新しいプロジェクト]** をクリックします。**Android アプリ**テンプレートを選択し、 **[次へ]** をクリックします。

[Android の空のアプリを![](designer-walkthrough-images/xs/01-android-app-m75-sml.png)](designer-walkthrough-images/xs/01-android-app-m75.png#lightbox)

新しいアプリデザイナの**チュートリアル**に名前を指定します。 **[ターゲットプラットフォーム]** で、[**最新] と [最大**] を選択し、 **[次へ]** をクリックします。

[![名アプリ](designer-walkthrough-images/xs/02-designer-walkthrough-m75-sml.png)](designer-walkthrough-images/xs/02-designer-walkthrough-m75.png#lightbox)

次のダイアログ画面で、 **[作成]** をクリックします。

### <a name="adding-a-layout"></a>レイアウトの追加

次の手順では、ユーザーインターフェイス要素を保持する**LinearLayout**を作成します。

Visual Studio for Mac で、**ソリューション**パッドの **[リソース/レイアウト]** を右クリックし、 **[> 新しいファイルの追加]** を選択します。 **[新しいファイル]** ダイアログで、 **[Android > レイアウト]** を選択します。 ファイルに**list_item**という名前を指定し、 **[新規]** をクリックします。

[新しいレイアウトの![](designer-walkthrough-images/xs/03-new-layout-m75-sml.png)](designer-walkthrough-images/xs/03-new-layout-m75.png#lightbox)

このファイルを追加すると、**デザインサーフェイス**に新しい**list_item**レイアウトが表示されます (メッセージが表示された場合、*このプロジェクトには正常にコンパイルされていないリソースが含まれており、レンダリングが影響を受ける可能性があり*ます。 [ビルド] をクリックし **>[すべてビルド**]: プロジェクトをビルドします):

[![デザイナービュー](designer-walkthrough-images/xs/04-designer-view-m75-sml.png)](designer-walkthrough-images/xs/04-designer-view-m75.png#lightbox)

デザイナーの下部にある **[ソース]** タブをクリックすると、このレイアウトの XML ソースが表示されます。 右側の **[ドキュメントアウトライン]** タブをクリックすると、現在レイアウトに1つの**LinearLayout**ウィジェットが含まれていることが示されます。

[![デザイナー XML](designer-walkthrough-images/xs/05-designer-xml-m75-sml.png)](designer-walkthrough-images/xs/05-designer-xml-m75.png#lightbox)

次の手順では、カラーブラウザーアプリのユーザーインターフェイスを作成します。

### <a name="creating-the-list-item-user-interface"></a>リスト項目のユーザーインターフェイスを作成する

画面の下部にある **[デザイナー]** タブをクリックして、**デザイナー画面**に戻ります。 右側の **[ツールボックス]** ウィンドウで、下にスクロールして **[Images & Media]** セクションに移動し、`ImageView`を見つけます。

[ImageView を検索![には](designer-walkthrough-images/xs/06-locate-imageview-m75-sml.png)](designer-walkthrough-images/xs/06-locate-imageview-m75.png#lightbox)

または、[検索] バーに「 *Imageview* 」と入力して、`ImageView`を検索することもできます。

[ImageView 検索の![](designer-walkthrough-images/xs/07-imageview-search-m75-sml.png)](designer-walkthrough-images/xs/07-imageview-search-m75.png#lightbox)

この `ImageView` を**デザインサーフェイス**にドラッグします (この `ImageView` は、カラーブラウザーアプリで色の見本を表示するために使用されます)。

[キャンバス上の![ImageView](designer-walkthrough-images/xs/08-imageview-on-canvas-m75-sml.png)](designer-walkthrough-images/xs/08-imageview-on-canvas-m75.png#lightbox)

次に、 **[ツールボックス]** から `LinearLayout (Vertical)` ウィジェットを**デザインサーフェイス**にドラッグします。 青いアウトラインが、追加された `LinearLayout`の境界を示すことに注意してください。 **ドキュメントアウトライン**には、`imageView1 (ImageView)`の下にある `LinearLayout`の子であることが示されています。

[![青のアウトライン](designer-walkthrough-images/xs/10-blue-outline-m75-sml.png)](designer-walkthrough-images/xs/10-blue-outline-m75.png#lightbox)

デザイナーで `ImageView` を選択すると、青い輪郭が `ImageView`を囲むように移動します。 さらに、選択項目は**ドキュメントアウトライン**の `imageView1 (ImageView)` に移動します。

[ImageView を選択![](designer-walkthrough-images/xs/11-select-imageview-m75-sml.png)](designer-walkthrough-images/xs/11-select-imageview-m75.png#lightbox)

次に、`Text (Large)` ウィジェットを **[ツールボックス]** から新しく追加した `LinearLayout`にドラッグします。 **デザインサーフェイス**にマウスをドラッグすると、新しいウィジェットが挿入される場所が強調表示されることに注意してください。
`Text (Large)` ウィジェットは、次に示すように `linearLayout1` 内に配置する必要があります。

[![ラージ テキスト ウィジェットを追加する](designer-walkthrough-images/xs/12-green-highlight-m75-sml.png)](designer-walkthrough-images/xs/12-green-highlight-m75.png#lightbox)

次に、`Text (Large)` ウィジェットの下に `Text (Small)` ウィジェットを追加します。 この時点で、**デザインサーフェイス**は次のスクリーンショットのようになります。

[小さいテキストウィジェットを追加![には](designer-walkthrough-images/xs/13-add-small-text-m75-sml.png)](designer-walkthrough-images/xs/13-add-small-text-m75.png#lightbox)

2つの `textView` ウィジェットが `linearLayout1`内にない場合は、それらを**ドキュメントアウトライン**内の `linearLayout1` にドラッグして、前のスクリーンショットに示すように配置できます (`linearLayout1`の下にインデントされます)。

### <a name="arranging-the-user-interface"></a>ユーザーインターフェイスの配置

次の手順では、UI を変更して左側に `ImageView` を表示し、2つの `TextView` ウィジェットを `ImageView`の右側に積み重ねます。

1. `ImageView` 選択した状態で、 **[プロパティ]** タブをクリックします。

2. **[プロパティ]** タブのすぐ下にある **[レイアウト]** をクリックします。

3. **ViewGroup**まで下にスクロールし、`Width` 設定を `wrap_content`に変更します。

[コンテンツの折り返しを設定![には](designer-walkthrough-images/xs/15-wrap-content-m75-sml.png)](designer-walkthrough-images/xs/15-wrap-content-m75.png#lightbox)

`Width` 設定を変更するもう1つの方法は、ウィジェットの右側にある三角形をクリックして、[幅] の設定を `wrap_content`に切り替えることです。

[ドラッグして幅を設定![](designer-walkthrough-images/xs/16-width-arrow-m75-sml.png)](designer-walkthrough-images/xs/16-width-arrow-m75.png#lightbox)

もう一度三角形をクリックすると、`Width` 設定が `match_parent`に戻ります。 次に、 **[ドキュメントアウトライン]** ウィンドウにアクセスして、ルート `LinearLayout`を選択します。

[![ルート LinearLayout の選択](designer-walkthrough-images/xs/17-root-linearlayout-m75-sml.png)](designer-walkthrough-images/xs/17-root-linearlayout-m75.png#lightbox)

ルート `LinearLayout` を選択した状態で、 **[プロパティ]** タブに戻り、 **[ウィジェット]** をクリックします。 次に示すように、`Orientation` 設定を `horizontal` に変更します。 この時点で、**デザインサーフェイス**は次のスクリーンショットのようになります。 `TextView` ウィジェットが `ImageView`の右側に移動したことに注意してください。

[水平方向の選択![](designer-walkthrough-images/xs/18-horizontal-orientation-m75-sml.png)](designer-walkthrough-images/xs/18-horizontal-orientation-m75.png#lightbox)

### <a name="modifying-the-spacing"></a>間隔の変更

次の手順では、UI の埋め込みと余白の設定を変更して、ウィジェットの間により多くの領域を確保します。 `ImageView` を選択し、 **[プロパティ]** の下の **[レイアウト]** タブをクリックします。 `Min Width` を `50dp`に、`Min Height` を `70dp`に、`Padding` を `10dp`に変更します。
これにより、`ImageView` のすべての辺の周りに埋め込みが適用され、垂直方向に elongates ます。

[パディングを設定![には](designer-walkthrough-images/xs/20-padding-widths-m75-sml.png)](designer-walkthrough-images/xs/20-padding-widths-m75.png#lightbox)

[上]、[右]、[下]、[左余白] の各設定は、それぞれ `Top`、`Right`、`Bottom`、および `Left` padding の各フィールドに値を入力することによって個別に設定できます。 たとえば、`Left` padding 値を `5dp` に設定し、`Top`、`Right`、および `Bottom` パディングの値を `10dp`に設定します。 `Padding` 設定は、次の値のコンマ区切りリストに変更されることに注意してください。

[カスタムパディング設定の![](designer-walkthrough-images/xs/21-custom-padding-m75-sml.png)](designer-walkthrough-images/xs/21-custom-padding-m75.png#lightbox)

次に、2つの `TextView` ウィジェットを含む `LinearLayout` ウィジェットの位置を調整します。 **ドキュメントアウトライン**で、[`linearLayout1`] を選択します。 **[プロパティ]** ペインで、 **[レイアウト]** タブを選択します。下へスクロールして **[ViewGroup]** セクションを表示し、`Left`、`Top`、`Right`、および `Bottom` の余白をそれぞれ `5dp`、`5dp`、`0dp`、`5dp` に設定します。:

[余白の設定![](designer-walkthrough-images/xs/22-margins-m75-sml.png)](designer-walkthrough-images/xs/22-margins-m75.png#lightbox)

### <a name="removing-the-default-image"></a>既定のイメージの削除

`ImageView` はイメージではなく色を表示するために使用されるため、次の手順では、テンプレートによって追加された既定のイメージソースを削除します。

1. `ImageView`を選択します。

2. **[プロパティ]** の下にある **[ウィジェット]** タブをクリックします。

3. `Src` 設定をオフにして、空白にします。

[ImageView src 設定をクリア![には](designer-walkthrough-images/xs/23-clear-src-m75-sml.png)](designer-walkthrough-images/xs/23-clear-src-m75.png#lightbox)

これにより、その `ImageView`のソース XML から `android:src="@android:drawable/ic_menu_gallery"` が削除されます。

### <a name="adding-a-listview-container"></a>ListView コンテナーの追加

**List_item**レイアウトが定義されたので、次の手順として、メインレイアウトに `ListView` を追加します。 この `ListView` には、 **list_item**の一覧が含まれます。 

**ソリューションエクスプローラー**で、 **Resources/layout/Main. axml**を開きます。
`Button` ウィジェット (存在する場合) をクリックして削除します。 **[ツールボックス]** で `ListView` ウィジェットを見つけて、**デザインサーフェイス**にドラッグします。
デザイナー内の `ListView` は空白になります。ただし、選択されている場合は、境界線を輪郭する青い線が使用されます。 **ドキュメントアウトライン**を表示して、 **ListView**が正しく追加されたことを確認できます。

[新しい ListView の![](designer-walkthrough-images/xs/24-new-listview-m75-sml.png)](designer-walkthrough-images/xs/24-new-listview-m75.png#lightbox)

既定では、`ListView` には `@+id/listView1`の `Id` 値が割り当てられます。
`listView1` が**ドキュメントアウトライン**でまだ選択されている状態で、**プロパティ**ペインを開き、[並べ替え] をクリックし**て**、 **[カテゴリ]** を選択します。
**Main**を開き、 **Id**プロパティを見つけて、その値を `@+id/myListView`に変更します。

[id の名前を myListView に変更![](designer-walkthrough-images/xs/25-change-id-m75-sml.png)](designer-walkthrough-images/xs/25-change-id-m75.png#lightbox)

この時点で、ユーザーインターフェイスを使用する準備が整いました。

### <a name="running-the-application"></a>アプリケーションの実行

**MainActivity.cs**を開き、そのコードを次のコードに置き換えます。

```csharp
using Android.App;
using Android.Widget;
using Android.Views;
using Android.OS;
using System.Collections.Generic;

namespace DesignerWalkthrough
{
    [Activity(Label = "@string/app_name", MainLauncher = true)]
    public class MainActivity : Activity
    {
        List<ColorItem> colorItems = new List<ColorItem>();
        ListView listView;

        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(savedInstanceState);

            // Set our view from the "main" layout resource
            SetContentView(Resource.Layout.Main);
            listView = FindViewById<ListView>(Resource.Id.myListView);

            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.DarkRed,
                ColorName = "Dark Red",
                Code = "8B0000"
            });
            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.SlateBlue,
                ColorName = "Slate Blue",
                Code = "6A5ACD"
            });
            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.ForestGreen,
                ColorName = "Forest Green",
                Code = "228B22"
            });

            listView.Adapter = new ColorAdapter(this, colorItems);
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
        public override View GetView(int position, View convertView, ViewGroup parent)
        {
            var item = items[position];

            View view = convertView;
            if (view == null) // no view to re-use, create new
                view = context.LayoutInflater.Inflate(Resource.Layout.list_item, null);
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

このコードでは、カスタムの `ListView` アダプターを使用して、色情報を読み込み、このデータを作成したばかりの UI に表示します。 この例を短くしておくために、色情報はリストにハードコーディングされていますが、データソースからカラー情報を抽出したり、その場で計算したりするようにアダプターを変更することもできます。 `ListView` アダプターの詳細については、「 [ListView](~/android/user-interface/layouts/list-view/index.md)」を参照してください。

アプリケーションをビルドして実行します。 次のスクリーンショットは、デバイスで実行しているときにアプリがどのように表示されるかを示しています。

[![最終的なスクリーンショット](designer-walkthrough-images/xs/26-final-screenshot-sml.png)](designer-walkthrough-images/xs/26-final-screenshot.png#lightbox)

-----

## <a name="summary"></a>まとめ

この記事では、Visual Studio の Android Designer を使用して、基本的なアプリのユーザーインターフェイスを作成するプロセスについて説明します。
ここでは、リスト内の1つの項目のインターフェイスを作成する方法を説明し、ウィジェットを追加して視覚的にレイアウトする方法を示します。
また、リソースを割り当て、それらのウィジェットのさまざまなプロパティを設定する方法についても説明しました。
