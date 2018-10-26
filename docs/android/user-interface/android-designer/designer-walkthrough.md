---
title: Xamarin.Android のデザイナーを使用してください。
description: この記事では、Xamarin.Android デザイナーのチュートリアルです。 小さなカラーのブラウザー アプリのユーザー インターフェイスを作成する方法を示しますこのユーザー インターフェイスが完全に、デザイナーで作成されます。
ms.prod: xamarin
ms.assetid: 70FF2F9A-71BD-317E-C881-A44D82DF1BD8
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
ms.openlocfilehash: 1174fe5cb417d4977fd6519086e6c4942e74c10b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120275"
---
# <a name="using-the-xamarinandroid-designer"></a>Xamarin.Android のデザイナーを使用してください。

_この記事では、Xamarin.Android デザイナーのチュートリアルです。小さなカラーのブラウザー アプリのユーザー インターフェイスを作成する方法を示しますこのユーザー インターフェイスが完全に、デザイナーで作成されます。_


## <a name="overview"></a>概要

Android ユーザー インターフェイスは、XML ファイルを使用して、またはコードを記述してプログラムで宣言によって作成できます。 Xamarin.Android のデザイナーを作成し、XML ファイルの手動編集することがなく宣言型のレイアウトを視覚的に、変更ができます。 デザイナーには、リアルタイム フィードバックできるように、デバイスとエミュレーターにアプリケーションを再デプロイしなくても、UI の変更を評価する開発者も提供します。 これらのデザイナー機能は、Android の UI 開発を大幅短縮できます。
この記事では、Xamarin.Android のデザイナーを使用して、視覚的にユーザー インターフェイスを作成する方法を示します。

## <a name="walkthrough"></a>チュートリアル

このチュートリアルでは、Android デザイナーを使用して色ブラウザー アプリの例のユーザー インターフェイスを作成します。 色のブラウザー アプリには、色、その名前、および RGB の値の一覧が表示されます。 ウィジェットを追加する方法について説明します、**デザイン サーフェイス**これらのウィジェットを視覚的にレイアウトする方法についてもします。 その後、対話形式でのウィジェットを変更する方法を学習、**デザイン サーフェイス**またはデザイナーを使用して**プロパティ**ウィンドウ。 最後に、デバイスまたはエミュレーターで実行されると、アプリのデザインの外観を確認します。


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

### <a name="creating-a-new-project"></a>新しいプロジェクトを作成します。

最初の手順では、新しい Xamarin.Android プロジェクトを作成します。 Visual Studio を起動して、をクリックして**新しいプロジェクト.** を選択し、 **Visual C\# > Android > Android アプリ (Xamarin)** テンプレート。
新しいアプリの名前を付けます**DesignerWalkthrough**  をクリック**OK**します。

[![空の android アプリ](designer-walkthrough-images/vs/01-android-app-w158-sml.png)](designer-walkthrough-images/vs/01-android-app-w158.png#lightbox)

**新しい Android アプリ**ダイアログ ボックスで、選択**空のアプリ** をクリック**OK**:

[![空の Android アプリ テンプレートを選択します。](designer-walkthrough-images/vs/02-blank-app-w158-sml.png)](designer-walkthrough-images/vs/02-blank-app-w158.png#lightbox)


### <a name="adding-a-layout"></a>レイアウトを追加します。

次の手順が作成するには、 **LinearLayout**ユーザー インターフェイス要素を保持します。 右クリック**リソース/レイアウト**で、**ソリューション エクスプ ローラー**選択と**追加 > 新しい項目.**.**新しい項目の追加**ダイアログ ボックスで、 **Android レイアウト**します。 ファイルに名前を**list_item**クリック**追加**:

[![新しいレイアウト](designer-walkthrough-images/vs/03-new-layout-w158-sml.png)](designer-walkthrough-images/vs/03-new-layout-w158.png#lightbox)

新しい**list_item**デザイナーでレイアウトが表示されます。 2 つのペインが表示されることに注意してください。 &ndash; 、*デザイン サーフェイス*の、 **list_item**は左側のウィンドウに表示される、XML ソースは、右ペインに表示されます。 位置を入れ替えることができます、**デザイン サーフェイス**と**ソース**ペインをクリックして、**スワップ ペイン**アイコンが 2 つのペインの間にあります。

[![デザイナー ビュー](designer-walkthrough-images/vs/04-designer-view-w158-sml.png)](designer-walkthrough-images/vs/04-designer-view-w158.png#lightbox)

**ビュー** ] メニューのをクリックして**その他の Windows > [ドキュメント アウトライン**を開く、**ドキュメント アウトライン**します。 **ドキュメント アウトライン**レイアウトで現在、1 つが含まれる表示**LinearLayout**ウィジェット。

[![ドキュメント アウトライン](designer-walkthrough-images/vs/06-document-outline-w158-sml.png)](designer-walkthrough-images/vs/06-document-outline-w158.png#lightbox)

次の手順がこの色のブラウザー アプリのユーザー インターフェイスを作成するには`LinearLayout`します。

### <a name="creating-the-list-item-user-interface"></a>リスト項目のユーザー インターフェイスを作成します。

場合、**ツールボックス**ウィンドウが表示されない、クリックして、**ツールボックス**左側のタブ。 **ツールボックス**、下へスクロールして、**イメージとメディア**セクションし、が見つかるまでさらにスクロールする`ImageView`:

[![ImageView を検索します。](designer-walkthrough-images/vs/07-locate-imageview-w158-sml.png)](designer-walkthrough-images/vs/07-locate-imageview-w158.png#lightbox)

入力する代わりに、 *ImageView*を検索する検索バーに、 `ImageView`:

[![ImageView 検索](designer-walkthrough-images/vs/08-imageview-search-w158-sml.png)](designer-walkthrough-images/vs/08-imageview-search-w158.png#lightbox)

これをドラッグして`ImageView`デザイン サーフェイスに (この`ImageView`色 browser アプリでの色の見本を表示するために使用されます)。

[![キャンバス上の ImageView](designer-walkthrough-images/vs/09-imageview-on-canvas-w158-sml.png)](designer-walkthrough-images/vs/09-imageview-on-canvas-w158.png#lightbox)

次に、ドラッグ、`LinearLayout (Vertical)`からウィジェット、**ツールボックス**デザイナーにします。 青色の輪郭を追加の境界を示すことに注意してください`LinearLayout`します。 **ドキュメント アウトライン**の子があることを示します`LinearLayout`の下にある`imageView1 (ImageView)`:

[![青のアウトライン](designer-walkthrough-images/vs/10-blue-outline-w158-sml.png)](designer-walkthrough-images/vs/10-blue-outline-w158.png#lightbox)

選択すると、`ImageView`デザイナーで、青色の輪郭の移動を囲む、`ImageView`します。 さらに、選択範囲に移動`imageView1 (ImageView)`で、**ドキュメント アウトライン**:

[![ImageView を選択します。](designer-walkthrough-images/vs/11-select-imageview-w158-sml.png)](designer-walkthrough-images/vs/11-select-imageview-w158.png#lightbox)

次に、ドラッグ、`Text (Large)`からウィジェット、**ツールボックス**に新たに追加した`LinearLayout`します。 新しいウィジェットを挿入する位置を示すためにデザイナーが、緑を使用して通知を示しています。

[![緑色の強調表示](designer-walkthrough-images/vs/12-green-highlight-w158-sml.png)](designer-walkthrough-images/vs/12-green-highlight-w158.png#lightbox)

次に、追加、`Text (Small)`ウィジェットの下、`Text (Large)`ウィジェット。

[![小さなテキスト ウィジェットを追加します。](designer-walkthrough-images/vs/13-add-small-text-w158-sml.png)](designer-walkthrough-images/vs/13-add-small-text-w158.png#lightbox)

この時点では、デザイナー画面には、次のスクリーン ショットがようになります。

[![デザイナーのレイアウト](designer-walkthrough-images/vs/14-raw-layout-w158-sml.png)](designer-walkthrough-images/vs/14-raw-layout-w158.png#lightbox)

場合、2 つ`textView`ウィジェットが内部ではない`linearLayout1`にドラッグすることができます`linearLayout1`で、**ドキュメント アウトライン**の前のスクリーン ショットに示すように表示されるように配置 (下にインデント`linearLayout1`)。


### <a name="arranging-the-user-interface"></a>ユーザー インターフェイスを配置します。

次の手順が表示する UI を変更するには、`ImageView`左側の 2 つ`TextView`の右側に積み上げ横ウィジェット、`ImageView`します。

1.  `ImageView`を選択します。

2.  **プロパティ ウィンドウ**、入力*幅*、検索ボックス**レイアウト**。

3.  変更、**レイアウト**設定`wrap_content`:

![ラップ コンテンツ](designer-walkthrough-images/vs/15-wrap-content-w158.png)

別の方法を変更する、`Width`設定では、その幅の設定を切り替えるウィジェットの右側にある三角形をクリックして`wrap_content`:

![幅を設定するドラッグ](designer-walkthrough-images/vs/15b-width-arrow-w158.png)

返します、三角形をもう一度クリックすると、`Width`設定`match_parent`。 次に移動、**ドキュメント アウトライン**ウィンドウとルートを選択`LinearLayout`:

[![LinearLayout ルートを選択します。](designer-walkthrough-images/vs/16-root-linearlayout-w158-sml.png)](designer-walkthrough-images/vs/16-root-linearlayout-w158.png#lightbox)

ルート`LinearLayout`に戻り、選択されている、**プロパティ**ウィンドウで、入力*向き*検索にボックス、**向き**設定。 変更**向き**に`horizontal`:

![水平方向の向きを選択します。](designer-walkthrough-images/vs/17-horizontal-orientation-w158.png)

この時点では、デザイナー画面には、次のスクリーン ショットがようになります。
なお、`TextView`ウィジェットの右側に移動された、 `ImageView`:

[![デザイナーのレイアウト](designer-walkthrough-images/vs/18-designer-layout-w158-sml.png)](designer-walkthrough-images/vs/18-designer-layout-w158.png#lightbox)

### <a name="modifying-the-spacing"></a>間隔を変更します。

次の手順では、ウィジェットの間でより多くの領域を提供する UI のパディングとマージンの設定を変更します。 選択、`ImageView`デザイン サーフェイス。 **プロパティ**ウィンドウで、入力`min`検索ボックスにします。 入力`70dp`の**高さの最小値**と`50dp`の**幅の最小値**:

[![セットの高さと幅](designer-walkthrough-images/vs/18b-set-height-width-sml.png)](designer-walkthrough-images/vs/18b-set-height-width.png#lightbox)

**プロパティ**ウィンドウで、入力`padding`検索を入力します`10dp`の**Padding**します。 これら`minHeight`、`minWidth`と`padding`設定がすべての辺の周囲にパディングを追加、`ImageView`図形の垂直方向に長くします。 これらの値を入力すると、XML レイアウトの変更されたことに注意してください。

[![パディングを設定します。](designer-walkthrough-images/vs/19-padding-widths-w158-sml.png)](designer-walkthrough-images/vs/19-padding-widths-w158.png#lightbox)

下、左、右、および上の余白設定できますが個別に設定に値を入力して、**下部のパディング**、**左側のパディング**、**右のパディング**、および**上の余白**フィールドに、それぞれします。
たとえば、設定、**左側のパディング**フィールドを`5dp`と**下部のパディング**、**右の余白**と**上部のパディング**フィールド`10dp`:

[![カスタムの埋め込みの設定](designer-walkthrough-images/vs/20-custom-padding-w158-sml.png)](designer-walkthrough-images/vs/20-custom-padding-w158.png#lightbox)

位置を次に、調整、`LinearLayout`ウィジェットを含む 2 つ`TextView`ウィジェット。 **ドキュメント アウトライン**、`linearLayout1`します。 **プロパティ**ウィンドウで、入力`margin`検索ボックスにします。 設定**レイアウトの余白の下部**、**レイアウトの左余白**、および**レイアウトの余白の上部**に`5dp`します。 設定**レイアウトの余白右**に`0dp`:

[![余白を設定します。](designer-walkthrough-images/vs/21-margins-w158-sml.png)](designer-walkthrough-images/vs/21-margins-w158.png#lightbox)

### <a name="removing-the-default-image"></a>既定のイメージを削除します。

`ImageView`色の表示に使用されている (イメージ) ではなく、次の手順は、テンプレートによって追加された既定のイメージ ソースを削除します。

1.  選択、`ImageView`上、**デザイナー画面**します。

2.  **プロパティ**、入力*src*検索ボックスにします。

3.  右側に小さな四角形をクリックして、 **Src**プロパティを設定および **リセット**:

[![ImageView src の設定をクリアします。](designer-walkthrough-images/vs/22-clear-img-src-w158-sml.png)](designer-walkthrough-images/vs/22-clear-img-src-w158.png#lightbox)

これを削除します`android:src="@android:drawable/ic_menu_gallery"`XML ソースからその`ImageView`します。

### <a name="adding-a-listview-container"></a>ListView のコンテナーを追加します。

これで、 **list_item**レイアウトが定義されている、次の手順が追加するには、`ListView`メイン レイアウトにします。 これは、`ListView`の一覧を含む**list_item**します。 

**ソリューション エクスプ ローラー**オープン**Resources/layout/activity_main.axml**します。 **ツールボックス**、検索、`ListView`ウィジェット上にドラッグし、**デザイン サーフェイス**します。 `ListView`デザイナーでは青い線が選択されているときに、枠線の輪郭を除いて空白になります。 表示することができます、**ドキュメント アウトライン**ことを確認する、 **ListView**が正しく追加されました。

[![新しい ListView](designer-walkthrough-images/vs/23-new-listview-w158-sml.png)](designer-walkthrough-images/vs/23-new-listview-w158.png#lightbox)

既定で、`ListView`指定、`Id`の値`@+id/listView1`します。
中に`listView1`でが選択されている、**ドキュメント アウトライン**、オープン、**プロパティ**ウィンドウで、をクリックして**並べ替え**を選択し、**カテゴリ**.
開いている**Main**、検索、 **Id**プロパティ、およびその値を変更`@+id/myListView`:

[![MyListView に id の名前を変更します。](designer-walkthrough-images/vs/24-change-id-w158-sml.png)](designer-walkthrough-images/vs/24-change-id-w158.png#lightbox)

この時点では、ユーザー インターフェイスが使用できる状態にします。

### <a name="running-the-application"></a>アプリケーションの実行

開いている**MainActivity.cs**次のコードを置き換えます。

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

このコードは、カスタム`ListView`アダプター色の情報を読み込むと、作成した UI にこのデータを表示します。 この例は短いを保持する、色情報一覧では、ハード コードされたが、色の情報をデータ ソースから抽出する、またはその場で計算することをアダプターに変更できること。 詳細については`ListView`アダプターを参照してください[ListView](~/android/user-interface/layouts/list-view/index.md)します。

アプリケーションをビルドして実行します。 次のスクリーン ショットでは、デバイスで実行されているときのアプリの外観の例を示します。

[![最終的なスクリーン ショット](designer-walkthrough-images/vs/25-final-screenshot-sml.png)](designer-walkthrough-images/vs/25-final-screenshot.png#lightbox)



# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

### <a name="creating-a-new-project"></a>新しいプロジェクトを作成します。

最初の手順では、新しい Xamarin.Android プロジェクトを作成します。

Visual Studio for Mac とクリック起動**新しいプロジェクト.**.選択、 **Android アプリ**テンプレートとクリック**次**:

[![空の android アプリ](designer-walkthrough-images/xs/01-android-app-m75-sml.png)](designer-walkthrough-images/xs/01-android-app-m75.png#lightbox)

新しいアプリの名前を付けます**DesignerWalkthrough**します。 [**ターゲット プラットフォーム**を選択します**最新および最大**] をクリック**次**:

[![名前のアプリ](designer-walkthrough-images/xs/02-designer-walkthrough-m75-sml.png)](designer-walkthrough-images/xs/02-designer-walkthrough-m75.png#lightbox)

次のダイアログ画面でクリックして**作成**です。

### <a name="adding-a-layout"></a>レイアウトを追加します。

次の手順が作成するには、 **LinearLayout**ユーザー インターフェイス要素を保持します。

Visual Studio for Mac では、右クリックして**リソース/レイアウト**で、**ソリューション**を埋め込むし、選択**追加 > 新しいファイル.**.**新しいファイル**ダイアログ ボックスで、 **Android > レイアウト**します。 ファイルに名前を**list_item**クリック**新規**:

[![新しいレイアウト](designer-walkthrough-images/xs/03-new-layout-m75-sml.png)](designer-walkthrough-images/xs/03-new-layout-m75.png#lightbox)

このファイルが追加された後、新しい**list_item**でレイアウトが表示されます、**デザイン サーフェイス**(、メッセージが表示された場合*このプロジェクトには、正常にコンパイルされていないリソースが含まれています。レンダリングの影響を受ける可能性があります*、 をクリックして**ビルド > すべてビルド**プロジェクトをビルドする)。

[![デザイナー ビュー](designer-walkthrough-images/xs/04-designer-view-m75-sml.png)](designer-walkthrough-images/xs/04-designer-view-m75.png#lightbox)

をクリックして、**ソース**このレイアウトの XML ソースを表示するデザイナーの下部にあるタブ。 クリックすると、**ドキュメント アウトライン** タブの右側には、レイアウトに現在、1 つが含まれているを示します**LinearLayout**ウィジェット。

[![XML デザイナー](designer-walkthrough-images/xs/05-designer-xml-m75-sml.png)](designer-walkthrough-images/xs/05-designer-xml-m75.png#lightbox)

次の手順では、色のブラウザー アプリのユーザー インターフェイスを作成します。

### <a name="creating-the-list-item-user-interface"></a>リスト項目のユーザー インターフェイスを作成します。

をクリックして、**デザイナー**  タブに戻り、画面の下部にある、**デザイナー サーフェス**します。 **ツールボックス**ウィンドウ、右側にスクロールして、**イメージとメディア**セクション探し`ImageView`:

[![ImageView を検索します。](designer-walkthrough-images/xs/06-locate-imageview-m75-sml.png)](designer-walkthrough-images/xs/06-locate-imageview-m75.png#lightbox)

入力する代わりに、 *ImageView*を検索する検索バーに、 `ImageView`:

[![ImageView 検索](designer-walkthrough-images/xs/07-imageview-search-m75-sml.png)](designer-walkthrough-images/xs/07-imageview-search-m75.png#lightbox)

これをドラッグして`ImageView`上に、**デザイン サーフェイス**(この`ImageView`色 browser アプリでの色の見本を表示するために使用されます)。

[![キャンバス上の ImageView](designer-walkthrough-images/xs/08-imageview-on-canvas-m75-sml.png)](designer-walkthrough-images/xs/08-imageview-on-canvas-m75.png#lightbox)

次に、ドラッグ、`LinearLayout (Vertical)`からウィジェット、**ツールボックス**に、**デザイン サーフェイス**します。 青色の輪郭を追加の境界を示すことに注意してください`LinearLayout`します。 **ドキュメント アウトライン**の子があることを示します`LinearLayout`の下にある`imageView1 (ImageView)`:

[![青のアウトライン](designer-walkthrough-images/xs/10-blue-outline-m75-sml.png)](designer-walkthrough-images/xs/10-blue-outline-m75.png#lightbox)

選択すると、`ImageView`デザイナーで、青色の輪郭の移動を囲む、`ImageView`します。 さらに、選択範囲に移動`imageView1 (ImageView)`で、**ドキュメント アウトライン**:

[![ImageView を選択します。](designer-walkthrough-images/xs/11-select-imageview-m75-sml.png)](designer-walkthrough-images/xs/11-select-imageview-m75.png#lightbox)

次に、ドラッグ、`Text (Large)`からウィジェット、**ツールボックス**に新たに追加した`LinearLayout`します。 上にマウスをドラッグすると、注意、**デザイン サーフェイス**、新しいウィジェットを挿入する位置が強調表示されます。
`Text (Large)`内でウィジェットを配置する必要があります`linearLayout1`ご覧のとおり。

[![ラージ テキスト ウィジェットを追加する](designer-walkthrough-images/xs/12-green-highlight-m75-sml.png)](designer-walkthrough-images/xs/12-green-highlight-m75.png#lightbox)

次に、追加、`Text (Small)`ウィジェットの下、`Text (Large)`ウィジェット。 この時点で、**デザイン サーフェイス**次のスクリーン ショットのようになります。

[![小さなテキスト ウィジェットを追加します。](designer-walkthrough-images/xs/13-add-small-text-m75-sml.png)](designer-walkthrough-images/xs/13-add-small-text-m75.png#lightbox)

場合、2 つ`textView`ウィジェットが内部ではない`linearLayout1`にドラッグすることができます`linearLayout1`で、**ドキュメント アウトライン**配置の前のスクリーン ショットに示すように表示されるようにする (下でインデントされた`linearLayout1`).


### <a name="arranging-the-user-interface"></a>ユーザー インターフェイスを配置します。

次の手順が表示する UI を変更するには、`ImageView`左側の 2 つ`TextView`の右側に積み上げ横ウィジェット、`ImageView`します。

1.  `ImageView`選択すると、をクリックして、**プロパティ**タブ。

2.  すぐ下、**プロパティ**] タブで [**レイアウト**します。

3.  下へスクロールして**ViewGroup**を変更して、`Width`設定`wrap_content`:

[![ラップ コンテンツ](designer-walkthrough-images/xs/15-wrap-content-m75-sml.png)](designer-walkthrough-images/xs/15-wrap-content-m75.png#lightbox)

別の方法を変更する、`Width`設定では、その幅の設定を切り替えるウィジェットの右側にある三角形をクリックして`wrap_content`:

[![幅を設定するドラッグ](designer-walkthrough-images/xs/16-width-arrow-m75-sml.png)](designer-walkthrough-images/xs/16-width-arrow-m75.png#lightbox)

返します、三角形をもう一度クリックすると、`Width`設定`match_parent`。 次に移動、**ドキュメント アウトライン**ウィンドウとルートを選択`LinearLayout`:

[![LinearLayout ルートを選択します。](designer-walkthrough-images/xs/17-root-linearlayout-m75-sml.png)](designer-walkthrough-images/xs/17-root-linearlayout-m75.png#lightbox)

ルート`LinearLayout`に戻り、選択されている、**プロパティ** タブでをクリックし、**ウィジェット**します。 変更、`Orientation`設定`horizontal`次のようです。 この時点で、**デザイン サーフェイス**次のスクリーン ショットのようになります。 なお、`TextView`ウィジェットの右側に移動された、 `ImageView`:

[![水平方向の向きを選択します。](designer-walkthrough-images/xs/18-horizontal-orientation-m75-sml.png)](designer-walkthrough-images/xs/18-horizontal-orientation-m75.png#lightbox)


### <a name="modifying-the-spacing"></a>間隔を変更します。

次の手順では、ウィジェットの間でより多くの領域を提供する UI のパディングとマージンの設定を変更します。 選択、 `ImageView`  をクリックし、**レイアウト**タブ**プロパティ**します。 変更、`Min Width`に`50dp`、`Min Height`に`70dp`、および`Padding`に`10dp`します。
これには、すべての辺の周囲にパディングが適用されます、`ImageView`垂直方向に elongates と。

[![パディングを設定します。](designer-walkthrough-images/xs/20-padding-widths-m75-sml.png)](designer-walkthrough-images/xs/20-padding-widths-m75.png#lightbox)

上、右、下、および左の余白設定できますが個別に設定に値を入力して、 `Top`、 `Right`、 `Bottom`、および`Left`それぞれフィールド内のスペースします。 たとえば、設定、`Left`パディング値に`5dp`と`Top`、`Right`と`Bottom`余白の値を`10dp`します。 なお、`Padding`設定はこれらの値のコンマ区切りの一覧に変わります。

[![カスタムの埋め込みの設定](designer-walkthrough-images/xs/21-custom-padding-m75-sml.png)](designer-walkthrough-images/xs/21-custom-padding-m75.png#lightbox)

位置を次に、調整、`LinearLayout`ウィジェットを含む 2 つ`TextView`ウィジェット。 **ドキュメント アウトライン**、`linearLayout1`します。 **プロパティ**ペインで、**レイアウト**タブ。下へスクロールして、 **ViewGroup**セクションし、設定、 `Left`、 `Top`、 `Right`、および`Bottom`に余白`5dp`、 `5dp`、 `0dp`、および`5dp`それぞれ。

[![余白を設定します。](designer-walkthrough-images/xs/22-margins-m75-sml.png)](designer-walkthrough-images/xs/22-margins-m75.png#lightbox)

### <a name="removing-the-default-image"></a>既定のイメージを削除します。

`ImageView`色の表示に使用されている (イメージ) ではなく、次の手順は、テンプレートによって追加された既定のイメージ ソースを削除します。

1.  `ImageView`を選択します。

2.  をクリックして、**ウィジェット**タブ**プロパティ**します。

3.  クリア、`Src`を空白に設定します。

[![ImageView src の設定をクリアします。](designer-walkthrough-images/xs/23-clear-src-m75-sml.png)](designer-walkthrough-images/xs/23-clear-src-m75.png#lightbox)

これを削除します`android:src="@android:drawable/ic_menu_gallery"`XML ソースからその`ImageView`します。

### <a name="adding-a-listview-container"></a>ListView のコンテナーを追加します。

これで、 **list_item**レイアウトが定義されている、次の手順が追加するには、`ListView`メイン レイアウトにします。 これは、`ListView`の一覧を含む**list_item**します。 

**ソリューション エクスプ ローラー**オープン**Resources/layout/Main.axml**します。
をクリックして、`Button`ウィジェット (ある場合) 削除します。 **ツールボックス**、検索、`ListView`ウィジェット上にドラッグし、**デザイン サーフェイス**します。
`ListView`デザイナーでは青い線が選択されているときに、枠線の輪郭を除いて空白になります。 表示することができます、**ドキュメント アウトライン**ことを確認する、 **ListView**が正しく追加されました。

[![新しい ListView](designer-walkthrough-images/xs/24-new-listview-m75-sml.png)](designer-walkthrough-images/xs/24-new-listview-m75.png#lightbox)

既定で、`ListView`指定、`Id`の値`@+id/listView1`します。
中に`listView1`でが選択されている、**ドキュメント アウトライン**、オープン、**プロパティ**ウィンドウで、をクリックして**並べ替え**を選択し、**カテゴリ**.
開いている**Main**、検索、 **Id**プロパティ、およびその値を変更`@+id/myListView`:

[![MyListView に id の名前を変更します。](designer-walkthrough-images/xs/25-change-id-m75-sml.png)](designer-walkthrough-images/xs/25-change-id-m75.png#lightbox)

この時点では、ユーザー インターフェイスが使用できる状態にします。

### <a name="running-the-application"></a>アプリケーションの実行

開いている**MainActivity.cs**次のコードを置き換えます。

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

このコードは、カスタム`ListView`アダプター色の情報を読み込むと、作成した UI にこのデータを表示します。 この例は短いを保持する、色情報一覧では、ハード コードされたが、色の情報をデータ ソースから抽出する、またはその場で計算することをアダプターに変更できること。 詳細については`ListView`アダプターを参照してください[ListView](~/android/user-interface/layouts/list-view/index.md)します。

アプリケーションをビルドして実行します。 次のスクリーン ショットでは、デバイスで実行されているときのアプリの外観の例を示します。

[![最終的なスクリーン ショット](designer-walkthrough-images/xs/26-final-screenshot-sml.png)](designer-walkthrough-images/xs/26-final-screenshot.png#lightbox)

-----


## <a name="summary"></a>まとめ

この記事では、Visual Studio で Xamarin.Android デザイナーを使用して、基本的なアプリのユーザー インターフェイスを作成するプロセスを説明しました。
一覧では、1 つの項目のインターフェイスを作成する方法を説明し、ウィジェットを追加し、それらを視覚的にレイアウトする方法を説明します。
リソースを割り当てるし、これらのウィジェットでさまざまなプロパティを設定する方法についても説明します。
