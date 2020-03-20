---
title: フラグメントのチュートリアル - パート 2
ms.prod: xamarin
ms.topic: tutorial
ms.assetid: 444A894D-5197-4726-934F-79BA80A71CB0
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/26/2018
ms.openlocfilehash: 4d9ef88f39914f8fa5e578577ee9f6977c2bc88e
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "73020264"
---
# <a name="fragments-walkthrough-ndash-landscape"></a>フラグメントのチュートリアル &ndash; 横向き

「[フラグメントのチュートリアル &ndash; パート 1](./walkthrough.md)」では、スマートフォン上の小さい画面を対象とする Android アプリでフラグメントを作成および使用する方法を示しました。 このチュートリアルの次のステップでは、アプリケーションを変更し、タブレット上でより多くの水平領域を利用できるようにします。常に芝居の一覧である 1 つのアクティビティがあり (`TitlesFragment`)、ユーザーの選択に対応してアクティビティに `PlayQuoteFragment` が動的に追加されます。

[![タブレットで実行されているアプリ](./walkthrough-landscape-images/01-tablet-screenshot-sml.png)](./walkthrough-landscape-images/01-tablet-screenshot.png#lightbox)

横モードで実行されているスマートフォンでは、この拡張からのメリットも得られます。

[![横モードの Android フォンで実行されているアプリ](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

## <a name="updating-the-app-to-handle-landscape-orientation"></a>横向きを処理するようにアプリを更新する

次の変更は、「[フラグメントのチュートリアル - スマートフォン](./walkthrough.md)」で行った作業が基になっています。

1. `TitlesFragment` と `PlayQuoteFragment` の両方を表示する代替レイアウトを作成します。
1. デバイスで両方のフラグメントが同時に表示されているかどうかを検出し、それに応じて動作を変更するように、`TitlesFragment` を更新します。
1. デバイスが横モードのときは閉じるように、`PlayQuoteActivity` を更新します。

## <a name="1-create-an-alternate-layout"></a>1.代替レイアウトを作成する

Android デバイスでメイン アクティビティが作成されると、Android によってデバイスの向きに基づいて読み込むレイアウトが決定されます。 Android では、既定で **Resources/layout/activity_main.axml** レイアウト ファイルが提供されています。 横モードで読み込まれるデバイスの場合、Android によって **Resources/layout-land/activity_main.axml** レイアウト ファイルが提供されます。 [Android リソース](/xamarin/android/app-fundamentals/resources-in-android)に関するガイドでは、アプリケーション用に読み込むリソース ファイルを Android で決定する方法がさらに詳しく説明されています。

[代替レイアウト](/xamarin/android/user-interface/android-designer/alternative-layout-views)に関するガイドで説明されている手順に従って、**横**向きを対象とする代替レイアウトを作成します。 これにより、新しいレイアウト リソース ファイル **Resources/layout/activity_main.axml** がプロジェクトに追加されます。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![ソリューション エクスプローラーでの代替レイアウト](./walkthrough-landscape-images/02-alternate-layout.w157-sml.png)](./walkthrough-landscape-images/02-alternate-layout.w157.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

[![Solution Pad での代替レイアウト](./walkthrough-landscape-images/02-alternate-layout.m743-sml.png)](./walkthrough-landscape-images/02-alternate-layout.m743.png#lightbox)

-----

代替レイアウトを作成した後、次の XML に一致するように、ファイル **Resources/layout-land/activity_main.axml** のソースを編集します。

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

アクティビティのルート ビューには、リソース ID `two_fragments_layout` が割り当てられ、`fragment` と `FrameLayout` の 2 つのサブビューがあります。 `fragment` は静的に読み込まれますが、`FrameLayout` は、実行時に `PlayQuoteFragment` によって置き換えられる "プレースホルダー" として機能します。 `TitlesFragment` で新しい芝居が選択されるたびに、`playquote_container` は `PlayQuoteFragment` の新しいインスタンスで更新されます。

各サブビューは、親の全体の高さを占めます。 各サブビューの幅は、`android:layout_weight` 属性と `android:layout_width` 属性によって制御されます。 この例では、各サブビューは、親によって提供される幅の 50% を占めます。 "_レイアウトのウェイト_" の詳細については、[LinearLayout に関する Google のドキュメント](https://developer.android.com/guide/topics/ui/layout/linear.html)を参照してください。

## <a name="2-changes-to-titlesfragment"></a>2.TitlesFragment を変更する

代替レイアウトを作成したら、`TitlesFragment` を更新する必要があります。 アプリで 1 つのアクティビティに 2 つのフラグメントが表示されている場合、`TitlesFragment` では親アクティビティに `PlayQuoteFragment` を読み込む必要があります。 それ以外の場合は、`TitlesFragment` では `PlayQuoteFragment` をホストする `PlayQuoteActivity` を起動する必要があります。 ブール型のフラグが、`TitlesFragment` で使用する必要がある動作を決定するのに役立ちます。 このフラグは、`OnActivityCreated` メソッドで初期化されます。

最初に、`TitlesFragment` クラスの先頭にインスタンス変数を追加します。

```csharp
bool showingTwoFragments;
```

次に、次のコード スニペットを `OnActivityCreated` に追加して、変数を初期化します。 

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

デバイスが横モードで実行されている場合は、リソース ID `playquote_container` の `FrameLayout` が画面に表示されるので、`showingTwoFragments` は `true` に初期化されます。 デバイスが縦モードで実行されている場合は、`playquote_container` は画面に表示されないため、`showingTwoFragments` は `false` です。

`ShowPlayQuote` メソッドでは、引用を表示する方法を、フラグメント内または新しいアクティビティの起動のどちらかに変更する必要があります。  2 つのフラグメントが表示されているときはフラグメントを読み込み、それ以外の場合はアクティビティを起動するように、`ShowPlayQuote` メソッドを更新します。

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

`PlayQuoteFragment` に現在表示されているものとは異なる芝居をユーザーが選択した場合は、新しい `PlayQuoteFragment` が作成されて、`FragmentTransaction` のコンテキスト内で `playquote_container` の内容が置き換えられます。

### <a name="complete-code-for-titlesfragment"></a>TitlesFragment の完全なコード

`TitlesFragment` に対する前記の変更をすべて完了した後、完全なクラスは次のコードと一致する必要があります。

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

## <a name="3-changes-to-playquoteactivity"></a>3.PlayQuoteActivity を変更する

最後にもう 1 つ考慮する必要のある詳細があります。デバイスが横モードのときは、`PlayQuoteActivity` は必要ありません。 デバイスが横モードの場合は、`PlayQuoteActivity` が表示されないようにする必要があります。 `PlayQuoteActivity` の `OnCreate` メソッドを更新して、自動的に閉じるようにします。 次のコードは `PlayQuoteActivity.OnCreate` の最終バージョンです。

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

この変更では、デバイスの向きのチェックを追加します。 横モードの場合、`PlayQuoteActivity` は自動的に閉じます。

## <a name="4-run-the-application"></a>4.アプリケーションの実行

これらの変更が完了したら、アプリを実行し、デバイスを横モードに回転させて (必要な場合)、芝居を選択します。 引用が、芝居の一覧と同じ画面に表示されます。

[![横モードの Android フォンで実行されているアプリ](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

[![Android タブレットで実行されているアプリ](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)
