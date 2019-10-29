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
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020264"
---
# <a name="fragments-walkthrough-ndash-landscape"></a>フラグメントのチュートリアル &ndash; 横

[フラグメントのチュートリアル &ndash; パート 1](./walkthrough.md)では、スマートフォンの小さい画面を対象とする、Android アプリでフラグメントを作成および使用する方法を示しました。 このチュートリアルの次の手順では、アプリケーションを変更して &ndash; タブレット上の余分な水平領域を活用します。1つのアクティビティが常に再生の一覧になり (`TitlesFragment`)、`PlayQuoteFragment` は r のアクティビティに動的に追加されます。esponse は、ユーザーが選択した内容を示します。

[タブレットで実行されている![アプリ](./walkthrough-landscape-images/01-tablet-screenshot-sml.png)](./walkthrough-landscape-images/01-tablet-screenshot.png#lightbox)

横モードで実行されている電話では、この拡張機能のメリットも得られます。

[ランドスケープモードで Android フォンで実行されている![アプリ](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

## <a name="updating-the-app-to-handle-landscape-orientation"></a>ランドスケープの向きを処理するようにアプリを更新しています

次の変更は、「[チュートリアル-電話](./walkthrough.md)」で実行した作業に基づいて作成されます。

1. 代替レイアウトを作成して、`TitlesFragment` と `PlayQuoteFragment`の両方を表示します。
1. `TitlesFragment` を更新して、デバイスに両方のフラグメントが同時に表示されているかどうかを検出し、それに応じて動作を変更します
1. デバイスが横モードのときに `PlayQuoteActivity` 更新して閉じる。

## <a name="1-create-an-alternate-layout"></a>1. 代替レイアウトを作成する

Android デバイスでメインアクティビティを作成すると、デバイスの向きに基づいて読み込むレイアウトが Android によって決定されます。 既定では、Android は**Resources/layout/activity_main**レイアウトファイルを提供します。 ランドスケープモードで読み込まれるデバイスの場合、Android は**Resources/layout-land/activity_main**レイアウトファイルを提供します。 Android[リソース](/xamarin/android/app-fundamentals/resources-in-android)のガイドでは、アプリケーション用に読み込むリソースファイルを android が決定する方法の詳細について説明します。

[代替](/xamarin/android/user-interface/android-designer/alternative-layout-views)レイアウトガイドで説明されている手順に従って、**横向き**のレイアウトを作成します。 これにより、新しいレイアウトリソースファイルが**Resources/layout/activity_main**というプロジェクトに追加されます。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[ソリューションエクスプローラーでの代替レイアウトの![](./walkthrough-landscape-images/02-alternate-layout.w157-sml.png)](./walkthrough-landscape-images/02-alternate-layout.w157.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[Solution Pad での代替レイアウトの![](./walkthrough-landscape-images/02-alternate-layout.m743-sml.png)](./walkthrough-landscape-images/02-alternate-layout.m743.png#lightbox)

-----

代替レイアウトを作成した後、次の XML に一致するように、file **Resources/layout-land/activity_main**のソースを編集します。

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

アクティビティのルートビューには `two_fragments_layout` リソース ID が割り当てられ、`fragment` と `FrameLayout`の2つのサブビューがあります。 `fragment` は静的に読み込まれますが、`FrameLayout` は、実行時に `PlayQuoteFragment`によって置き換えられる "プレースホルダー" として機能します。 `TitlesFragment`で新しい play が選択されるたびに、`playquote_container` が `PlayQuoteFragment`の新しいインスタンスで更新されます。

各サブビューでは、親の全体の高さが占有されます。 各サブビューの幅は、`android:layout_weight` 属性と `android:layout_width` 属性によって制御されます。 この例では、各サブビューは、親によって提供される幅の50% を占有します。 _レイアウトの重み_の詳細について[は、LinearLayout に関する Google のドキュメント](https://developer.android.com/guide/topics/ui/layout/linear.html)を参照してください。

## <a name="2-changes-to-titlesfragment"></a>2. TitlesFragment の変更

代替レイアウトが作成されたら、`TitlesFragment`を更新する必要があります。 アプリで1つのアクティビティに2つのフラグメントが表示されている場合、`TitlesFragment` は親アクティビティに `PlayQuoteFragment` を読み込む必要があります。 それ以外の場合、`TitlesFragment` は `PlayQuoteFragment`をホストする `PlayQuoteActivity` を起動する必要があります。 ブール型のフラグは、使用する必要がある動作を `TitlesFragment` 判断するのに役立ちます。 このフラグは `OnActivityCreated` メソッドで初期化されます。

最初に、`TitlesFragment` クラスの先頭にインスタンス変数を追加します。

```csharp
bool showingTwoFragments;
```

次に、変数を初期化するために、次のコードスニペットを `OnActivityCreated` に追加します。 

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

デバイスが横モードで実行されている場合は、リソース ID `playquote_container` の `FrameLayout` が画面に表示されるので、`showingTwoFragments` は `true`に初期化されます。 デバイスが縦モードで実行されている場合、`playquote_container` は画面に表示されないため、`showingTwoFragments` は `false`されます。

`ShowPlayQuote` メソッドは、フラグメントに引用符 &ndash; を表示する方法を変更するか、新しいアクティビティを起動する必要があります。  2つのフラグメントが表示されたときにフラグメントを読み込むように `ShowPlayQuote` メソッドを更新します。それ以外の場合は、アクティビティを起動する必要があります。

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

`PlayQuoteFragment`に現在表示されているものとは異なる再生をユーザーが選択した場合、新しい `PlayQuoteFragment` が作成され、`FragmentTransaction`のコンテキスト内で `playquote_container` の内容が置き換えられます。

### <a name="complete-code-for-titlesfragment"></a>TitlesFragment の完全なコード

`TitlesFragment`に対する以前の変更をすべて完了した後、完全なクラスは次のコードと一致する必要があります。

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

## <a name="3-changes-to-playquoteactivity"></a>3. PlayQuoteActivity の変更

最後の1つの詳細について説明します。デバイスが横モードの場合、`PlayQuoteActivity` は必要ありません。 デバイスが横モードの場合、`PlayQuoteActivity` は表示されません。 `PlayQuoteActivity` の `OnCreate` メソッドを更新して、それ自体が閉じられるようにします。 このコードは `PlayQuoteActivity.OnCreate`の最終バージョンです。

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

この変更により、デバイスの向きのチェックが追加されます。 横モードの場合、`PlayQuoteActivity` はそれ自体を閉じます。

## <a name="4-run-the-application"></a>4. アプリケーションを実行する

これらの変更が完了したら、アプリを実行し、デバイスを横向きモード (必要な場合) に回転させて、再生を選択します。 この見積もりは、再生の一覧と同じ画面に表示されます。

[ランドスケープモードで Android フォンで実行されている![アプリ](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

[Android タブレットで実行されている![アプリ](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)
