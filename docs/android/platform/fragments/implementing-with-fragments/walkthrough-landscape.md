---
title: フラグメントのチュートリアル - パート 2
ms.prod: xamarin
ms.topic: tutorial
ms.assetid: 444A894D-5197-4726-934F-79BA80A71CB0
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/26/2018
ms.openlocfilehash: 7ec8ad6ce428107d2255dd07c7e69c9e77780c09
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61026313"
---
# <a name="fragments-walkthrough-ndash-landscape"></a>フラグメントのチュートリアル&ndash;ランドス ケープ

[フラグメント チュートリアル&ndash;第 1 部](./walkthrough.md)を作成し、小さい画面は、スマート フォンを対象とする Android アプリでフラグメントを使用する方法を示しました。 このチュートリアルでは、次の手順がタブレットの横に余分な空き領域を活用するためにアプリケーションを変更するには&ndash;アプローチの一覧では常に 1 つのアクティビティがあります (、 `TitlesFragment`) と`PlayQuoteFragment`が動的に追加されますユーザーによる選択への応答内のアクティビティ。

[![タブレットで実行されているアプリ](./walkthrough-landscape-images/01-tablet-screenshot-sml.png)](./walkthrough-landscape-images/01-tablet-screenshot.png#lightbox)

横モードで実行されているスマート フォンは、この機能強化からも活用できます。

[![Android フォンで横モードで実行されているアプリ](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

## <a name="updating-the-app-to-handle-landscape-orientation"></a>横向きを処理するためにアプリを更新します。

次の変更で行った作業の上に構築、[フラグメントのチュートリアル - 電話](./walkthrough.md)

1. 両方を表示する代替レイアウトを作成、`TitlesFragment`と`PlayQuoteFragment`します。
1. Update`TitlesFragment`をデバイスが両方のフラグメントを同時に表示するかどうかを検出し、それに応じて動作を変更します。
1. Update`PlayQuoteActivity`をデバイスが横モードを閉じます。

## <a name="1-create-an-alternate-layout"></a>1.代替のレイアウトを作成します。

Android デバイスでメイン アクティビティが作成されると、Android は、デバイスの向きに基づいてをロードするレイアウトを決定します。 既定では、Android の提供、 **Resources/layout/activity_main.axml**レイアウト ファイルです。 Android デバイスを横モードで読み込むが提供する、 **Resources/layout-land/activity_main.axml**レイアウト ファイルです。 ガイド[Android リソース](/xamarin/android/app-fundamentals/resources-in-android)Android がアプリケーション用に読み込むファイルのどのリソースが決定する方法の詳細が含まれています。

対象とする別のレイアウトを作成**ランドス ケープ**次で説明されている手順に従って、向き、[代替レイアウト](/xamarin/android/user-interface/android-designer/alternative-layout-views)ガイド。 これには、プロジェクトに新しいレイアウト リソース ファイルを追加する必要があります**Resources/layout/activity_main.axml**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![ソリューション エクスプ ローラーで代替レイアウト](./walkthrough-landscape-images/02-alternate-layout.w157-sml.png)](./walkthrough-landscape-images/02-alternate-layout.w157.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![Solution Pad の代替レイアウト](./walkthrough-landscape-images/02-alternate-layout.m743-sml.png)](./walkthrough-landscape-images/02-alternate-layout.m743.png#lightbox)

-----

代替レイアウトを作成した後、ファイルのソースを編集**Resources/layout-land/activity_main.axml**この XML に一致します。

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

アクティビティのルート ビューには、リソース ID が与えられます`two_fragments_layout`2 つのサブ ビューであり、`fragment`と`FrameLayout`します。 中に、`fragment`が静的に読み込まれて、`FrameLayout`によって実行時に置き換えられる"placeholder"として機能、`PlayQuoteFragment`します。 新しいプレイを選択するたびに、 `TitlesFragment`、`playquote_container`の新しいインスタンスに反映されます、`PlayQuoteFragment`します。

各サブ ビューは、親の全体の高さを占有します。 各サブビューの幅がによって制御される、`android:layout_weight`と`android:layout_width`属性。 この例では、各サブビューが幅の 50% を占める親によって提供されます。 参照してください[Google のドキュメントで、LinearLayout](https://developer.android.com/guide/topics/ui/layout/linear.html)の詳細について_レイアウト重み_します。

## <a name="2-changes-to-titlesfragment"></a>2.TitlesFragment への変更

更新する必要は代替のレイアウトが作成されたら、`TitlesFragment`します。 ときにアプリが表示されて、2 つのフラグメント 1 つのアクティビティし`TitlesFragment`読み込む必要があります、`PlayQuoteFragment`アクティビティの親にします。 それ以外の場合、`TitlesFragment`起動する必要があります、`PlayQuoteActivity`ホスト、`PlayQuoteFragment`します。 ブール型のフラグが役立つ`TitlesFragment`を使用するどの動作を決定します。 このフラグはで初期化する、`OnActivityCreated`メソッド。

最初に、上部にあるインスタンス変数を追加、`TitlesFragment`クラス。

```csharp
bool showingTwoFragments;
```

次のコード スニペットを追加し、`OnActivityCreated`変数を初期化します。 

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

デバイスが、横向きモードで実行されている場合、`FrameLayout`リソース ID を持つ`playquote_container`が画面に表示されるように`showingTwoFragments`に初期化されます`true`します。 デバイスが縦モードの場合で、実行されているかどうかは`playquote_container`されません画面で、そのため`showingTwoFragments`なります`false`します。

`ShowPlayQuote`メソッドは、見積もりを表示する方法を変更する必要があります&ndash;フラグメントまたは新しいアクティビティを起動します。  更新プログラム、`ShowPlayQuote`メソッドを 2 つのフラグメントを表示するときに、フラグメントを読み込むアクティビティを起動する必要がありますそれ以外の場合。

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

ユーザーに現在表示されているとは異なるアプローチを選択した場合`PlayQuoteFragment`、し、新しい`PlayQuoteFragment`が作成されの内容を置き換える、`playquote_container`のコンテキスト内で、`FragmentTransaction`します。

### <a name="complete-code-for-titlesfragment"></a>TitlesFragment の完全なコード

以前のすべての変更の完了後`TitlesFragment`、完全なクラスがこのコードと一致する必要があります。

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

ように注意する 1 つの最終的な詳細がある:`PlayQuoteActivity`とき、デバイスが横モードでは必要ありません。 デバイスが横モードである場合、`PlayQuoteActivity`は表示されません。 更新プログラム、`OnCreate`メソッドの`PlayQuoteActivity`自体に閉じることができるようにします。 このコードの最終バージョンは、 `PlayQuoteActivity.OnCreate`:

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

この変更は、デバイスの向きのチェックを追加します。 横向きモードである場合`PlayQuoteActivity`自体を閉じます。

## <a name="4-run-the-application"></a>4.アプリケーションの実行

これらの変更が完了し、アプリの実行し、は横長表示モード (必要な) 場合、デバイスの回転を再生 を選択します。 見積もりは、再生リストとして同じ画面に表示する必要があります。

[![Android フォンで横モードで実行されているアプリ](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

[![Android タブレットで実行されているアプリ](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)
