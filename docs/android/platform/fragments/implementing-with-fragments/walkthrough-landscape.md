---
title: チュートリアルの第 2 部をフラグメントします。
ms.prod: xamarin
ms.topic: tutorial
ms.assetid: 444A894D-5197-4726-934F-79BA80A71CB0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/26/2018
ms.openlocfilehash: 58291388d375a4fd9273c8e0cd46db3799966766
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
---
# <a name="fragments-walkthrough-ndash-landscape"></a>チュートリアルをフラグメント化&ndash;ランドス ケープ

[フラグメント チュートリアル&ndash;第 1 部](./walkthrough.md)作成し、電話で小さい画面を対象とする Android アプリでフラグメントを使用する方法を示しました。 このチュートリアルでは、次の手順は、タブレット上余分な水平方向のスペースを活用するためにアプリケーションを変更するのには&ndash;アプローチの一覧が必ず 1 つのアクティビティがあります (、 `TitlesFragment`) および`PlayQuoteFragment`動的に追加されますユーザーによる選択に応じてアクティビティ。

[![タブレットで実行されているアプリ](./walkthrough-landscape-images/01-tablet-screenshot-sml.png)](./walkthrough-landscape-images/01-tablet-screenshot.png#lightbox)

また、横モードで実行されている携帯電話はこの拡張機能から効率的になります。

[![Android フォンの横モードで実行されているアプリ](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

## <a name="updating-the-app-to-handle-landscape-orientation"></a>横方向を処理するアプリの更新

行われた作業時にビルドは、次の変更、[フラグメント チュートリアル - 電話](./walkthrough.md)

1. 両方を表示する代替のレイアウトを作成、`TitlesFragment`と`PlayQuoteFragment`です。
1. 更新`TitlesFragment`をデバイスが両方のフラグメントを同時に表示するかどうかを検出し、それに従って動作を変更します。
1. 更新`PlayQuoteActivity`デバイスは、横置きモードのときに終了します。

## <a name="1-create-an-alternate-layout"></a>1.代替のレイアウトを作成します。

Android デバイスでメインのアクティビティが作成されると、Android は、デバイスの向きに基づいてロードするレイアウトを決定します。 既定では、Android が提供する、 **Resources/layout/activity_main.axml**レイアウト ファイルです。 Android デバイスの横モードで読み込むことが提供する、 **Resources/layout-land/activity_main.axml**レイアウト ファイルです。 ガイド[Android リソース](/xamarin/android/app-fundamentals/resources-in-android)Android がアプリケーション用に読み込むファイルのどのリソースを決定する方法の詳細が含まれています。

対象とする別のレイアウトを作成する**ランドス ケープ**次で説明されている手順に従って、向き、[代替レイアウト](/xamarin/android/user-interface/android-designer/alternative-layout-views)ガイドです。 プロジェクトに新しいレイアウト リソース ファイルを追加この**Resources/layout/activity_main.axml**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![ソリューション エクスプ ローラーで代替のレイアウト](./walkthrough-landscape-images/02-alternate-layout.w157-sml.png)](./walkthrough-landscape-images/02-alternate-layout.w157.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![ソリューション パッドで代替のレイアウト](./walkthrough-landscape-images/02-alternate-layout.m743-sml.png)](./walkthrough-landscape-images/02-alternate-layout.m743.png#lightbox)

-----

代替のレイアウトを作成した後、ファイルのソースを編集**Resources/layout-land/activity_main.axml**この XML に一致します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/two_fragments_layout"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <fragment android:name="FragmentSample.TitlesFragment"
        android:id="@+id/titles"
        android:layout_weight="1"
        android:layout_width="0px"
        android:layout_height="match_parent" />

    <FrameLayout android:id="@+id/playquote_container"
            android:layout_weight="1"
            android:layout_width="0px"
            android:layout_height="match_parent"
             />
</LinearLayout>
```

アクティビティのルート ビューには、リソース ID が与えられます`two_fragments_layout`2 つのサブ ビューがあり、`fragment`と`FrameLayout`です。 中に、`fragment`が静的に読み込まれて、 `FrameLayout` "プレース ホルダーとして"で実行時に置き換えられる、`PlayQuoteFragment`です。 新しい再生を選択するたびに、 `TitlesFragment`、`playquote_container`での新しいインスタンスが更新される、`PlayQuoteFragment`です。

各サブ ビューは、親の最大の高さを占有します。 によって各サブビューの幅を制御、`android:layout_weight`と`android:layout_width`属性。 この例では各サブビューが幅の 50% を占める親によって提供されます。 参照してください[Google のドキュメントは、LinearLayout で](https://developer.android.com/guide/topics/ui/layout/linear.html)詳細については_レイアウト重み_です。

## <a name="2-changes-to-titlesfragment"></a>2.TitlesFragment への変更

更新する必要は代替のレイアウトが作成されたら、`TitlesFragment`です。 ときに、アプリで表示する 2 つのフラグメントを 1 つのアクティビティにし、`TitlesFragment`読み込む必要があります、`PlayQuoteFragment`アクティビティの親にします。 それ以外の場合、`TitlesFragment`起動する必要があります、`PlayQuoteActivity`のどのホスト、`PlayQuoteFragment`です。 ブール型のフラグに役立つ`TitlesFragment`が使用する動作を決定します。 このフラグはで初期化する、`OnActivityCreated`メソッドです。

最初の上部にあるインスタンス変数を追加、`TitlesFragment`クラス。

```csharp
bool showingTwoFragments;
```

次のコード スニペットを追加して、`OnActivityCreated`変数を初期化します。 

```csharp
var quoteContainer = Activity.FindViewById(Resource.Id.playquote_container);
showingTwoFragments = quoteContainer != null &&
                      quoteContainer.Visibility == ViewStates.Visible;
if (showingTwoFragments)
{
    ListView.ChoiceMode = ChoiceMode.Single;
    ShowPlayQuote(selectedPlayId);
}
```

デバイスが横モードで実行されている場合、`FrameLayout`リソース ID を持つ`playquote_container`を画面に表示するように`showingTwoFragments`に初期化されます`true`です。 デバイスが縦モードで、実行されているかどうか`playquote_container`されません画面で、そのため`showingTwoFragments`なります`false`です。

`ShowPlayQuote`メソッドは、引用符の表示方法を変更する必要があります&ndash;フラグメントまたは起動して、新しいアクティビティのいずれか。  更新プログラム、`ShowPlayQuote`を 2 つのフラグメントを示す際に、フラグメントを読み込むメソッドそれ以外の場合、アクティビティが起動します。

```csharp
void ShowPlayQuote(int playId)
{
    selectedPlayId = playId;
    if (showingTwoFragments)
    {
        ListView.SetItemChecked(selectedPlayId, true);

        var playQuoteFragment = FragmentManager.FindFragmentById(Resource.Id.playquote_container) as PlayQuoteFragment;

        if (playQuoteFragment == null || playQuoteFragment.PlayId != playId)
        {
            var container = Activity.FindViewById(Resource.Id.playquote_container);
            var quoteFrag = PlayQuoteFragment.NewInstance(selectedPlayId);

            FragmentTransaction ft = FragmentManager.BeginTransaction();
            ft.Replace(Resource.Id.playquote_container, quoteFrag);
            ft.Commit();
        }
    }
    else
    {
        var intent = new Intent(Activity, typeof(PlayQuoteActivity));
        intent.PutExtra("current_play_id", playId);
        StartActivity(intent);
    }
}
```

ユーザーに現在表示されているとは異なるアプローチを選択した場合`PlayQuoteFragment`、し、新しい`PlayQuoteFragment`が作成されの内容を置き換える、`playquote_container`のコンテキスト内で、`FragmentTransaction`です。

### <a name="complete-code-for-titlesfragment"></a>TitlesFragment の完全なコード

以前のすべての変更の完了後に`TitlesFragment`、完全なクラスはこのコードに一致する必要があります。

```csharp
public class TitlesFragment : ListFragment
{
    int selectedPlayId;
    bool showingTwoFragments;

    public override void OnActivityCreated(Bundle savedInstanceState)
    {
        base.OnActivityCreated(savedInstanceState);
        ListAdapter = new ArrayAdapter<string>(Activity, Android.Resource.Layout.SimpleListItemActivated1, Shakespeare.Titles);

        if (savedInstanceState != null)
        {
            selectedPlayId = savedInstanceState.GetInt("current_play_id", 0);
        }


        var quoteContainer = Activity.FindViewById(Resource.Id.playquote_container);
        showingTwoFragments = quoteContainer != null &&
                                quoteContainer.Visibility == ViewStates.Visible;
        if (showingTwoFragments)
        {
            ListView.ChoiceMode = ChoiceMode.Single;
            ShowPlayQuote(selectedPlayId);
        }
    }

    public override void OnSaveInstanceState(Bundle outState)
    {
        base.OnSaveInstanceState(outState);
        outState.PutInt("current_play_id", selectedPlayId);
    }

    public override void OnListItemClick(ListView l, View v, int position, long id)
    {
        ShowPlayQuote(position);
    }

    void ShowPlayQuote(int playId)
    {
        selectedPlayId = playId;
        if (showingTwoFragments)
        {
            ListView.SetItemChecked(selectedPlayId, true);

            var playQuoteFragment = FragmentManager.FindFragmentById(Resource.Id.playquote_container) as PlayQuoteFragment;

            if (playQuoteFragment == null || playQuoteFragment.PlayId != playId)
            {
                var container = Activity.FindViewById(Resource.Id.playquote_container);
                var quoteFrag = PlayQuoteFragment.NewInstance(selectedPlayId);

                FragmentTransaction ft = FragmentManager.BeginTransaction();
                ft.Replace(Resource.Id.playquote_container, quoteFrag);
                ft.AddToBackStack(null);
                ft.SetTransition(FragmentTransit.FragmentFade);
                ft.Commit();
            }
        }
        else
        {
            var intent = new Intent(Activity, typeof(PlayQuoteActivity));
            intent.PutExtra("current_play_id", playId);
            StartActivity(intent);
        }
    }
}
```

## <a name="3-changes-to-playquoteactivity"></a>3.PlayQuoteActivity への変更

処理する 1 つの最終的な詳細がある:`PlayQuoteActivity`デバイスが横モードにする必要はありません。 デバイスが横モードである場合、`PlayQuoteActivity`は表示されません。 更新プログラム、`OnCreate`メソッドの`PlayQuoteActivity`それ自体を閉じることができるようにします。 このコードは最終バージョンの`PlayQuoteActivity.OnCreate`:

```csharp
protected override void OnCreate(Bundle savedInstanceState)
{
    base.OnCreate(savedInstanceState);

    if (Resources.Configuration.Orientation == Android.Content.Res.Orientation.Landscape)
    {
        Finish();
    }

    var playId = Intent.Extras.GetInt("current_play_id", 0);
    var playQuoteFrag = PlayQuoteFragment.NewInstance(playId);
    FragmentManager.BeginTransaction()
                    .Add(Android.Resource.Id.Content, playQuoteFrag)
                    .Commit();
}
```

この変更は、デバイスの向きのチェックを追加します。 場合、横モードでは`PlayQuoteActivity`自体を閉じます。

## <a name="4-run-the-application"></a>4.アプリケーションの実行

これらの変更が完了したら、アプリを実行し、横モード (必要な場合)、デバイスを回転し、[再生] を選択します。 見積もりは、再生リストとして同じ画面に表示される必要があります。

[![Android フォンの横モードで実行されているアプリ](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

[![Android タブレットで実行されているアプリ](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)
