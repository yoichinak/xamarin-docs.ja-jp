---
title: Android Designer の使用
description: この記事は、Android Designer のチュートリアルです。 これは、小さいカラーブラウザーアプリのユーザーインターフェイスを作成する方法を示しています。このユーザーインターフェイスは、デザイナーで完全に作成されます。
ms.prod: xamarin
ms.assetid: 70FF2F9A-71BD-317E-C881-A44D82DF1BD8
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/25/2018
ms.openlocfilehash: 1eed9691684bffbc9367c9b411a83876a52cf72f
ms.sourcegitcommit: 63029dd7ea4edb707a53ea936ddbee684a926204
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/20/2021
ms.locfileid: "98609774"
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

このチュートリアルの目的は、Android Designer を使用して、サンプルのカラーブラウザーアプリ用のユーザーインターフェイスを作成することです。 カラーブラウザーアプリでは、色、名前、および RGB 値の一覧が表示されます。 **デザインサーフェイス** にウィジェットを追加する方法と、これらのウィジェットを視覚的にレイアウトする方法についても説明します。 その後、 **デザインサーフェイス** で対話的にウィジェットを変更する方法、またはデザイナーの [ **プロパティ** ] ペインを使用してウィジェットを変更する方法を学習します。 最後に、デバイスまたはエミュレーターでアプリを実行すると、デザインがどのように見えるかを確認できます。

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

### <a name="creating-a-new-project"></a>新しいプロジェクトの作成

最初の手順では、新しい Xamarin. Android プロジェクトを作成します。 Visual Studio を起動し、[ **新しいプロジェクト...**] をクリックし、 **Visual C \# > android > android アプリ (Xamarin)** テンプレートを選択します。
新しいアプリデザイナの **チュートリアル** に名前を指定し、[ **OK]** をクリックします。

[![Android の空のアプリ](designer-walkthrough-images/vs/01-android-app-w158-sml.png)](designer-walkthrough-images/vs/01-android-app-w158.png#lightbox)

[ **新しい Android アプリ** ] ダイアログで、[ **空のアプリ** ] を選択し、[ **OK**] をクリックします。

[![Android の空のアプリテンプレートを選択する](designer-walkthrough-images/vs/02-blank-app-w158-sml.png)](designer-walkthrough-images/vs/02-blank-app-w158.png#lightbox)

### <a name="adding-a-layout"></a>レイアウトの追加

次の手順では、ユーザーインターフェイス要素を保持する **LinearLayout** を作成します。 **ソリューションエクスプローラー** で [**リソース/レイアウト**] を右クリックし、[**新しい項目の追加 >**] を選択します。[**新しい項目の追加**] ダイアログで、[ **Android レイアウト**] を選択します。 ファイルに **list_item** 名前を指定し、[ **追加**] をクリックします。

[![新しいレイアウト](designer-walkthrough-images/vs/03-new-layout-w158-sml.png)](designer-walkthrough-images/vs/03-new-layout-w158.png#lightbox)

新しい **list_item** レイアウトがデザイナーに表示されます。 2つのペインが表示されていることに注意して &ndash; ください。 **list_item** の *デザインサーフェイス* が左側のウィンドウに表示され、その XML ソースが右側のウィンドウに表示されます。 次の2つのウィンドウの間にある [**スワップウィンドウ**] アイコンをクリックすると、**デザインサーフェイス** ペインと **ソース** ペインの位置を入れ替えることができます。

[![デザイナービュー](designer-walkthrough-images/vs/04-designer-view-w158-sml.png)](designer-walkthrough-images/vs/04-designer-view-w158.png#lightbox)

[ **表示** ] メニューの [ **その他の Windows > ドキュメントアウトライン** ] をクリックして、 **ドキュメントアウトライン** を開きます。 **ドキュメントアウトライン** には、現在、レイアウトに1つの **LinearLayout** ウィジェットが含まれていることが示されています。

[![ドキュメントアウトライン](designer-walkthrough-images/vs/06-document-outline-w158-sml.png)](designer-walkthrough-images/vs/06-document-outline-w158.png#lightbox)

次の手順では、この内でカラーブラウザーアプリのユーザーインターフェイスを作成し `LinearLayout` ます。

### <a name="creating-the-list-item-user-interface"></a>リスト項目のユーザーインターフェイスを作成する

**ツールボックス** ウィンドウが表示されていない場合は、左側の [**ツールボックス**] タブをクリックします。 [ **ツールボックス**] で、[ **Images &** ] セクションまで下にスクロールし、次の場所まで下にスクロールし `ImageView` ます。

[![ImageView の検索](designer-walkthrough-images/vs/07-locate-imageview-w158-sml.png)](designer-walkthrough-images/vs/07-locate-imageview-w158.png#lightbox)

または、検索バーに *Imageview* を入力して、を見つけることもでき `ImageView` ます。

[![ImageView 検索](designer-walkthrough-images/vs/08-imageview-search-w158-sml.png)](designer-walkthrough-images/vs/08-imageview-search-w158.png#lightbox)

これをデザインサーフェイスにドラッグし `ImageView` ます (これは、カラー `ImageView` ブラウザーアプリで色の見本を表示するために使用されます)。

[![キャンバス上の ImageView](designer-walkthrough-images/vs/09-imageview-on-canvas-w158-sml.png)](designer-walkthrough-images/vs/09-imageview-on-canvas-w158.png#lightbox)

次に、 `LinearLayout (Vertical)` ウィジェットを **ツールボックス** からデザイナーにドラッグします。 青色の枠線が、追加されたの境界を示していることに注意して `LinearLayout` ください。 **ドキュメントアウトライン** には、次の場所にあるの子であることが示されてい `LinearLayout` `imageView1 (ImageView)` ます。

[![青いアウトライン](designer-walkthrough-images/vs/10-blue-outline-w158-sml.png)](designer-walkthrough-images/vs/10-blue-outline-w158.png#lightbox)

デザイナーでを選択すると、 `ImageView` 青い輪郭がを囲むように移動し `ImageView` ます。 さらに、選択範囲は `imageView1 (ImageView)` **ドキュメントアウトライン** 内のに移動します。

[![ImageView を選択する](designer-walkthrough-images/vs/11-select-imageview-w158-sml.png)](designer-walkthrough-images/vs/11-select-imageview-w158.png#lightbox)

次に、 `Text (Large)` ウィジェットを **ツールボックス** から、新しく追加したにドラッグし `LinearLayout` ます。 デザイナーでは、緑色の強調表示を使用して、新しいウィジェットを挿入する場所を指定します。

[![緑のハイライト](designer-walkthrough-images/vs/12-green-highlight-w158-sml.png)](designer-walkthrough-images/vs/12-green-highlight-w158.png#lightbox)

次に、 `Text (Small)` ウィジェットの下にウィジェットを追加し `Text (Large)` ます。

[![小さいテキストウィジェットの追加](designer-walkthrough-images/vs/13-add-small-text-w158-sml.png)](designer-walkthrough-images/vs/13-add-small-text-w158.png#lightbox)

この時点で、デザイナー画面は次のスクリーンショットのようになります。

[![スクリーンショットには、ツールボックス、ドキュメントアウトライン、および小さいテキストが選択されたレイアウト領域を含むデザイナー画面が表示されます。](designer-walkthrough-images/vs/14-raw-layout-w158-sml.png)](designer-walkthrough-images/vs/14-raw-layout-w158.png#lightbox)

2つの `textView` ウィジェットが含まれていない場合は `linearLayout1` 、 `linearLayout1` **ドキュメントアウトライン** 内のにドラッグして、前のスクリーンショットのように表示されるように配置できます (下にインデントが付き `linearLayout1` ます)。

### <a name="arranging-the-user-interface"></a>ユーザーインターフェイスの配置

次の手順では、を左側に表示するように UI を変更し `ImageView` `TextView` ます。2つのウィジェットがの右側に積み重ねられて `ImageView` います。

1. `ImageView`を選択します。

2. [ **プロパティウィンドウ** の [検索] ボックスに「 *width* 」と入力し、[ **レイアウトの幅**] を検索します。

3. [ **レイアウトの幅** ] の設定を次のように変更し `wrap_content` ます。

![コンテンツの折り返しの設定](designer-walkthrough-images/vs/15-wrap-content-w158.png)

設定を変更するもう1つの方法 `Width` は、ウィジェットの右側にある三角形をクリックして、幅の設定を次のように切り替えることです `wrap_content` 。

![ドラッグして幅を設定](designer-walkthrough-images/vs/15b-width-arrow-w158.png)

もう一度三角形をクリックすると、 `Width` 設定がに戻り `match_parent` ます。 次に、[ **ドキュメントアウトライン** ] ウィンドウにアクセスして、ルートを選択し `LinearLayout` ます。

[![ルート LinearLayout の選択](designer-walkthrough-images/vs/16-root-linearlayout-w158-sml.png)](designer-walkthrough-images/vs/16-root-linearlayout-w158.png#lightbox)

ルートを選択した状態で、 `LinearLayout` [ **プロパティ** ] ペインに戻り、[検索] ボックスに「 *orientation* 」と入力して、[ **印刷の向き** ] 設定を探します。 **方向** を次のように変更し `horizontal` ます。

![水平方向の選択](designer-walkthrough-images/vs/17-horizontal-orientation-w158.png)

この時点で、デザイナー画面は次のスクリーンショットのようになります。
`TextView`ウィジェットがの右側に移動したことに注意して `ImageView` ください。

[![スクリーンショットには、ツールボックス、ドキュメントアウトライン、およびレイアウト領域を含むデザイナー画面が表示されています。](designer-walkthrough-images/vs/18-designer-layout-w158-sml.png)](designer-walkthrough-images/vs/18-designer-layout-w158.png#lightbox)

### <a name="modifying-the-spacing"></a>間隔の変更

次の手順では、UI の埋め込みと余白の設定を変更して、ウィジェットの間により多くの領域を確保します。 `ImageView`デザイン画面でを選択します。 [ **プロパティ** ] ペインで、[検索] ボックスに「」と入力し `min` ます。 `70dp`[**最小の高さ**] と [最小の幅] に「」と入力し `50dp` ます。

[![高さと幅を設定する](designer-walkthrough-images/vs/18b-set-height-width-sml.png)](designer-walkthrough-images/vs/18b-set-height-width.png#lightbox)

[**プロパティ**] ペインの [ `padding` 検索] ボックスに「」と入力し、「Padding」と入力し `10dp` ます。  これら `minHeight` の `minWidth` 設定により `padding` 、のすべての辺の周囲にパディングが追加さ `ImageView` れ、垂直方向に elongate されます。 次の値を入力すると、レイアウト XML が変更されます。

[![パディングの設定](designer-walkthrough-images/vs/19-padding-widths-w158-sml.png)](designer-walkthrough-images/vs/19-padding-widths-w158.png#lightbox)

下、左、右、および上にある余白の設定は、[**埋め込み**] の下に値を入力することによって個別に設定できます。また、[埋め込み]、[**右****へ**]、[埋め込み]**の各フィールド** にそれぞれ設定できます。
たとえば、[**左余白**] フィールドをに設定し、[余白]、[右余白]、 `5dp` [**余白**] の各フィールドをに設定し  `10dp` ます。

[![カスタム埋め込みの設定](designer-walkthrough-images/vs/20-custom-padding-w158-sml.png)](designer-walkthrough-images/vs/20-custom-padding-w158.png#lightbox)

次に、 `LinearLayout` 2 つのウィジェットを含むウィジェットの位置を調整し `TextView` ます。 [ **ドキュメントアウトライン**] で、[] を選択し `linearLayout1` ます。 [ **プロパティ** ] ウィンドウで、検索ボックスに「」と入力し `margin` ます。 **レイアウトの余白の下部**、レイアウトの余白の **左**、およびレイアウトの **余白を上** に設定し `5dp` ます。 **レイアウトの余白** を次のように設定し `0dp` ます。

[![余白の設定](designer-walkthrough-images/vs/21-margins-w158-sml.png)](designer-walkthrough-images/vs/21-margins-w158.png#lightbox)

### <a name="removing-the-default-image"></a>既定のイメージの削除

は `ImageView` イメージではなく色を表示するために使用されるため、次の手順では、テンプレートによって追加された既定のイメージソースを削除します。

1. `ImageView`**デザイナー画面** でを選択します。

2. [ **プロパティ**] で、検索ボックスに「 *src* 」と入力します。

3. **Src** プロパティ設定の右側にある小さな四角形をクリックし、[**リセット**] を選択します。

[![ImageView src 設定をクリアします。](designer-walkthrough-images/vs/22-clear-img-src-w158-sml.png)](designer-walkthrough-images/vs/22-clear-img-src-w158.png#lightbox)

これ `android:src="@android:drawable/ic_menu_gallery"` により、ソース XML からが削除さ `ImageView` れます。

### <a name="adding-a-listview-container"></a>ListView コンテナーの追加

**List_item** レイアウトが定義されたので、次の手順ではを `ListView` メインレイアウトに追加します。 これ `ListView` には **list_item** の一覧が含まれます。 

**ソリューションエクスプローラー** で、 **Resources/layout/activity_main を開きます。** **ツールボックス** で、 `ListView` ウィジェットを見つけて **デザインサーフェイス** にドラッグします。 デザイナーのは、選択され `ListView` たときに境界線を輪郭する青い線を除き、空白になります。 **ドキュメントアウトライン** を表示して、 **ListView** が正しく追加されたことを確認できます。

[![新しい ListView](designer-walkthrough-images/vs/23-new-listview-w158-sml.png)](designer-walkthrough-images/vs/23-new-listview-w158.png#lightbox)

既定では、の値はに設定され `ListView` `Id` `@+id/listView1` ます。
`listView1`**ドキュメントアウトライン** でを選択したまま、**プロパティ** ペインを開き、[**並べ替え**] をクリックして、[**カテゴリ**] を選択します。
**Main** を開き、 **Id** プロパティを見つけて、その値をに変更し `@+id/myListView` ます。

[![Id 名を myListView に変更](designer-walkthrough-images/vs/24-change-id-w158-sml.png)](designer-walkthrough-images/vs/24-change-id-w158.png#lightbox)

この時点で、ユーザーインターフェイスを使用する準備が整いました。

### <a name="running-the-application"></a>アプリケーションの実行

**MainActivity.cs** を開き、そのコードを次のコードに置き換えます。

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

このコードでは、カスタムアダプターを使用し `ListView` てカラー情報を読み込み、このデータを作成したばかりの UI に表示します。 この例を短くしておくために、色情報はリストにハードコーディングされていますが、データソースからカラー情報を抽出したり、その場で計算したりするようにアダプターを変更することもできます。 アダプターの詳細について `ListView` は、「 [ListView](~/android/user-interface/layouts/list-view/index.md)」を参照してください。

アプリケーションをビルドして実行します。 次のスクリーンショットは、デバイスで実行しているときにアプリがどのように表示されるかを示しています。

[![最終的なスクリーンショット](designer-walkthrough-images/vs/25-final-screenshot-sml.png)](designer-walkthrough-images/vs/25-final-screenshot.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

### <a name="creating-a-new-project"></a>新しいプロジェクトの作成

最初の手順では、新しい Xamarin. Android プロジェクトを作成します。

Visual Studio for Mac を起動し、[ **新しいプロジェクト**] をクリックします。 **Android アプリ** テンプレートを選択し、[ **次へ**] をクリックします。

[![Android の空のアプリ](designer-walkthrough-images/xs/01-android-app-m75-sml.png)](designer-walkthrough-images/xs/01-android-app-m75.png#lightbox)

新しいアプリデザイナの **チュートリアル** に名前を指定します。 [ **ターゲットプラットフォーム**] で、[ **最新] と [最大** ] を選択し、[ **次へ**] をクリックします。

[![アプリの名前](designer-walkthrough-images/xs/02-designer-walkthrough-m75-sml.png)](designer-walkthrough-images/xs/02-designer-walkthrough-m75.png#lightbox)

次のダイアログ画面で、[ **作成**] をクリックします。

### <a name="adding-a-layout"></a>レイアウトの追加

次の手順では、ユーザーインターフェイス要素を保持する **LinearLayout** を作成します。

Visual Studio for Mac で、**ソリューション** パッドの [**リソース/レイアウト**] を右クリックし、[ **> 新しいファイルの追加**] を選択します。[**新しいファイル**] ダイアログで、[ **Android > レイアウト**] を選択します。 ファイルに **list_item** という名前を指定し、[ **新規**] をクリックします。

[![新しいレイアウト](designer-walkthrough-images/xs/03-new-layout-m75-sml.png)](designer-walkthrough-images/xs/03-new-layout-m75.png#lightbox)

このファイルが追加されると、**デザインサーフェイス** に新しい **list_item** レイアウトが表示されます (メッセージが表示された場合、*このプロジェクトには正常にコンパイルされなかったリソースが含まれているため、レンダリングが影響を受ける可能性があり* ます。 [ビルド **> ビルド**] をクリックしてプロジェクトをビルドします)。

[![デザイナービュー](designer-walkthrough-images/xs/04-designer-view-m75-sml.png)](designer-walkthrough-images/xs/04-designer-view-m75.png#lightbox)

デザイナーの下部にある [ **ソース** ] タブをクリックすると、このレイアウトの XML ソースが表示されます。 右側の [ **ドキュメントアウトライン** ] タブをクリックすると、現在レイアウトに1つの **LinearLayout** ウィジェットが含まれていることが示されます。

[![デザイナー XML](designer-walkthrough-images/xs/05-designer-xml-m75-sml.png)](designer-walkthrough-images/xs/05-designer-xml-m75.png#lightbox)

次の手順では、カラーブラウザーアプリのユーザーインターフェイスを作成します。

### <a name="creating-the-list-item-user-interface"></a>リスト項目のユーザーインターフェイスを作成する

画面の下部にある [ **デザイナー** ] タブをクリックして、 **デザイナー画面** に戻ります。 右側の [ **ツールボックス** ] ウィンドウで、下にスクロールして [ **Images & Media** ] セクションに移動し、次の場所に移動し `ImageView` ます。

[![ImageView の検索](designer-walkthrough-images/xs/06-locate-imageview-m75-sml.png)](designer-walkthrough-images/xs/06-locate-imageview-m75.png#lightbox)

または、検索バーに *Imageview* を入力して、を見つけることもでき `ImageView` ます。

[![ImageView 検索](designer-walkthrough-images/xs/07-imageview-search-m75-sml.png)](designer-walkthrough-images/xs/07-imageview-search-m75.png#lightbox)

これを `ImageView` **デザインサーフェイス** にドラッグします (これは、カラー `ImageView` ブラウザーアプリで色の見本を表示するために使用されます)。

[![キャンバス上の ImageView](designer-walkthrough-images/xs/08-imageview-on-canvas-m75-sml.png)](designer-walkthrough-images/xs/08-imageview-on-canvas-m75.png#lightbox)

次に、 `LinearLayout (Vertical)` **ツールボックス** からウィジェットを **デザインサーフェイス** にドラッグします。 青色の枠線が、追加されたの境界を示していることに注意して `LinearLayout` ください。 **ドキュメントアウトライン** には、次に示すの子であることが示されてい `LinearLayout` `imageView1 (ImageView)` ます。

[![青いアウトライン](designer-walkthrough-images/xs/10-blue-outline-m75-sml.png)](designer-walkthrough-images/xs/10-blue-outline-m75.png#lightbox)

デザイナーでを選択すると、 `ImageView` 青い輪郭がを囲むように移動し `ImageView` ます。 さらに、選択範囲は `imageView1 (ImageView)` **ドキュメントアウトライン** 内のに移動します。

[![ImageView を選択する](designer-walkthrough-images/xs/11-select-imageview-m75-sml.png)](designer-walkthrough-images/xs/11-select-imageview-m75.png#lightbox)

次に、 `Text (Large)` ウィジェットを **ツールボックス** から、新しく追加したにドラッグし `LinearLayout` ます。 **デザインサーフェイス** にマウスをドラッグすると、新しいウィジェットが挿入される場所が強調表示されることに注意してください。
ウィジェットは、次に `Text (Large)` 示すように、内に配置する必要があり `linearLayout1` ます。

[![大きいテキストウィジェットの追加](designer-walkthrough-images/xs/12-green-highlight-m75-sml.png)](designer-walkthrough-images/xs/12-green-highlight-m75.png#lightbox)

次に、ウィジェット `Text (Small)` の下にウィジェットを追加し `Text (Large)` ます。 この時点で、 **デザインサーフェイス** は次のスクリーンショットのようになります。

[![小さいテキストウィジェットの追加](designer-walkthrough-images/xs/13-add-small-text-m75-sml.png)](designer-walkthrough-images/xs/13-add-small-text-m75.png#lightbox)

2つの `textView` ウィジェットが含まれていない場合は `linearLayout1` 、 `linearLayout1` **ドキュメントアウトライン** 内のにドラッグして、前のスクリーンショット (下にインデント) で表示されるように配置でき `linearLayout1` ます。

### <a name="arranging-the-user-interface"></a>ユーザーインターフェイスの配置

次の手順では、を左側に表示するように UI を変更し `ImageView` `TextView` ます。2つのウィジェットがの右側に積み重ねられて `ImageView` います。

1. を選択した状態で、 `ImageView` [ **プロパティ** ] タブをクリックします。

2. [ **プロパティ** ] タブのすぐ下にある [ **レイアウト**] をクリックします。

3. **ViewGroup** まで下にスクロールし、 `Width` 設定を次のように変更し `wrap_content` ます。

[![コンテンツの折り返しの設定](designer-walkthrough-images/xs/15-wrap-content-m75-sml.png)](designer-walkthrough-images/xs/15-wrap-content-m75.png#lightbox)

設定を変更するもう1つの方法 `Width` は、ウィジェットの右側にある三角形をクリックして、幅の設定を次のように切り替えることです `wrap_content` 。

[![ドラッグして幅を設定](designer-walkthrough-images/xs/16-width-arrow-m75-sml.png)](designer-walkthrough-images/xs/16-width-arrow-m75.png#lightbox)

もう一度三角形をクリックすると、 `Width` 設定がに戻り `match_parent` ます。 次に、[ **ドキュメントアウトライン** ] ウィンドウにアクセスして、ルートを選択し `LinearLayout` ます。

[![ルート LinearLayout の選択](designer-walkthrough-images/xs/17-root-linearlayout-m75-sml.png)](designer-walkthrough-images/xs/17-root-linearlayout-m75.png#lightbox)

ルートを選択した状態で、 `LinearLayout` [ **プロパティ** ] タブに戻り、[ **ウィジェット**] をクリックします。 次に `Orientation` 示すように、設定をに変更し `horizontal` ます。 この時点で、 **デザインサーフェイス** は次のスクリーンショットのようになります。 `TextView`ウィジェットがの右側に移動したことに注意して `ImageView` ください。

[![水平方向の選択](designer-walkthrough-images/xs/18-horizontal-orientation-m75-sml.png)](designer-walkthrough-images/xs/18-horizontal-orientation-m75.png#lightbox)

### <a name="modifying-the-spacing"></a>間隔の変更

次の手順では、UI の埋め込みと余白の設定を変更して、ウィジェットの間により多くの領域を確保します。 を選択 `ImageView` し、[**プロパティ**] の下の [**レイアウト**] タブをクリックします。 をに、をに、をに変更し `Min Width` `50dp` `Min Height` `70dp` `Padding` `10dp` ます。
これにより、のすべての辺の周囲にパディングが適用され、 `ImageView` 垂直方向に elongates ます。

[![パディングの設定](designer-walkthrough-images/xs/20-padding-widths-m75-sml.png)](designer-walkthrough-images/xs/20-padding-widths-m75.png#lightbox)

[上]、[右]、[下]、および [左] の各余白の設定は `Top` 、それぞれ、、、 `Right` `Bottom` およびパディングの各フィールドに値を入力することによって個別に設定でき `Left` ます。 たとえば、パディングの値をに設定し、、、およびの各パディングの値をに設定し `Left` `5dp` `Top` `Right` `Bottom` `10dp` ます。 `Padding`この設定は、次の値のコンマ区切りのリストに変更されることに注意してください。

[![カスタム埋め込みの設定](designer-walkthrough-images/xs/21-custom-padding-m75-sml.png)](designer-walkthrough-images/xs/21-custom-padding-m75.png#lightbox)

次に、 `LinearLayout` 2 つのウィジェットを含むウィジェットの位置を調整し `TextView` ます。 [ **ドキュメントアウトライン**] で、[] を選択し `linearLayout1` ます。 **プロパティ** ペインで、[**レイアウト**] タブを選択します。[ **ViewGroup** ] セクションまで下にスクロールし、、、 `Left` `Top` `Right` 、およびの `Bottom` 余白を `5dp` それぞれ、、 `5dp` `0dp` 、、に設定し `5dp` ます。

[![余白の設定](designer-walkthrough-images/xs/22-margins-m75-sml.png)](designer-walkthrough-images/xs/22-margins-m75.png#lightbox)

### <a name="removing-the-default-image"></a>既定のイメージの削除

は `ImageView` イメージではなく色を表示するために使用されるため、次の手順では、テンプレートによって追加された既定のイメージソースを削除します。

1. `ImageView`を選択します。

2. [**プロパティ**] の下にある [**ウィジェット**] タブをクリックします。

3. 設定をオフにして、空白にし `Src` ます。

[![ImageView src 設定をクリアします。](designer-walkthrough-images/xs/23-clear-src-m75-sml.png)](designer-walkthrough-images/xs/23-clear-src-m75.png#lightbox)

これ `android:src="@android:drawable/ic_menu_gallery"` により、ソース XML からが削除さ `ImageView` れます。

### <a name="adding-a-listview-container"></a>ListView コンテナーの追加

**List_item** レイアウトが定義されたので、次の手順ではを `ListView` メインレイアウトに追加します。 これ `ListView` には **list_item** の一覧が含まれます。 

**ソリューションエクスプローラー** で、 **Resources/layout/Main. axml** を開きます。
`Button`ウィジェット (存在する場合) をクリックして削除します。 **ツールボックス** で、 `ListView` ウィジェットを見つけて **デザインサーフェイス** にドラッグします。
デザイナーのは、選択され `ListView` たときに境界線を輪郭する青い線を除き、空白になります。 **ドキュメントアウトライン** を表示して、 **ListView** が正しく追加されたことを確認できます。

[![新しい ListView](designer-walkthrough-images/xs/24-new-listview-m75-sml.png)](designer-walkthrough-images/xs/24-new-listview-m75.png#lightbox)

既定では、の値はに設定され `ListView` `Id` `@+id/listView1` ます。
`listView1`**ドキュメントアウトライン** でを選択したまま、**プロパティ** ペインを開き、[**並べ替え**] をクリックして、[**カテゴリ**] を選択します。
**Main** を開き、 **Id** プロパティを見つけて、その値をに変更し `@+id/myListView` ます。

[![Id 名を myListView に変更](designer-walkthrough-images/xs/25-change-id-m75-sml.png)](designer-walkthrough-images/xs/25-change-id-m75.png#lightbox)

この時点で、ユーザーインターフェイスを使用する準備が整いました。

### <a name="running-the-application"></a>アプリケーションの実行

**MainActivity.cs** を開き、そのコードを次のコードに置き換えます。

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

このコードでは、カスタムアダプターを使用し `ListView` てカラー情報を読み込み、このデータを作成したばかりの UI に表示します。 この例を短くしておくために、色情報はリストにハードコーディングされていますが、データソースからカラー情報を抽出したり、その場で計算したりするようにアダプターを変更することもできます。 アダプターの詳細について `ListView` は、「 [ListView](~/android/user-interface/layouts/list-view/index.md)」を参照してください。

アプリケーションをビルドして実行します。 次のスクリーンショットは、デバイスで実行しているときにアプリがどのように表示されるかを示しています。

[![最終的なスクリーンショット](designer-walkthrough-images/xs/26-final-screenshot-sml.png)](designer-walkthrough-images/xs/26-final-screenshot.png#lightbox)

-----

## <a name="summary"></a>まとめ

この記事では、Visual Studio の Android Designer を使用して、基本的なアプリのユーザーインターフェイスを作成するプロセスについて説明します。
ここでは、リスト内の1つの項目のインターフェイスを作成する方法を説明し、ウィジェットを追加して視覚的にレイアウトする方法を示します。
また、リソースを割り当て、それらのウィジェットのさまざまなプロパティを設定する方法についても説明しました。
