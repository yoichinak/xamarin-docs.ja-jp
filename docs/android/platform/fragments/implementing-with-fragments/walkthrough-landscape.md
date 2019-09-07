---
title: フラグメントのチュートリアル - パート 2
ms.prod: xamarin
ms.topic: tutorial
ms.assetid: 444A894D-5197-4726-934F-79BA80A71CB0
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/26/2018
ms.openlocfilehash: 0363213d76d9a67b559614741edf37d296848075
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761653"
---
# <a name="fragments-walkthrough-ndash-landscape"></a>フラグメントの&ndash;チュートリアルランドスケープ

この[ &ndash;チュートリアルのパート 1](./walkthrough.md)では、スマートフォンの小さな画面を対象とする、Android アプリでフラグメントを作成して使用する方法を説明しました。 このチュートリアルの次の手順では、アプリケーションを変更してタブレット&ndash;上の余分な水平領域を活用します`TitlesFragment`。1つのアクティビティ () が常に再生され`PlayQuoteFragment` 、動的に追加されます。次のように、ユーザーが選択した内容に応じてアクティビティに対して実行します。

[![タブレットで実行されているアプリ](./walkthrough-landscape-images/01-tablet-screenshot-sml.png)](./walkthrough-landscape-images/01-tablet-screenshot.png#lightbox)

横モードで実行されている電話では、この拡張機能のメリットも得られます。

[![ランドスケープモードの Android フォンで実行されているアプリ](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

## <a name="updating-the-app-to-handle-landscape-orientation"></a>ランドスケープの向きを処理するようにアプリを更新しています

次の変更は、「[チュートリアル-電話](./walkthrough.md)」で実行した作業に基づいて作成されます。

1. `TitlesFragment` と`PlayQuoteFragment`の両方を表示する代替レイアウトを作成します。
1. デバイスで両方のフラグメントが同時に表示されているかどうかを検出し、それに応じて動作を変更します。`TitlesFragment`
1. デバイス`PlayQuoteActivity`が横モードのときに更新して閉じます。

## <a name="1-create-an-alternate-layout"></a>1. 代替レイアウトを作成する

Android デバイスでメインアクティビティを作成すると、デバイスの向きに基づいて読み込むレイアウトが Android によって決定されます。 既定では、Android は**Resources/layout/activity_main**レイアウトファイルを提供します。 ランドスケープモードで読み込まれるデバイスの場合、Android は**Resources/layout-land/activity_main**レイアウトファイルを提供します。 Android[リソース](/xamarin/android/app-fundamentals/resources-in-android)のガイドでは、アプリケーション用に読み込むリソースファイルを android が決定する方法の詳細について説明します。

[代替](/xamarin/android/user-interface/android-designer/alternative-layout-views)レイアウトガイドで説明されている手順に従って、**横向き**のレイアウトを作成します。 これにより、新しいレイアウトリソースファイルが**Resources/layout/activity_main**というプロジェクトに追加されます。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![ソリューションエクスプローラーの代替レイアウト](./walkthrough-landscape-images/02-alternate-layout.w157-sml.png)](./walkthrough-landscape-images/02-alternate-layout.w157.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![Solution Pad の代替レイアウト](./walkthrough-landscape-images/02-alternate-layout.m743-sml.png)](./walkthrough-landscape-images/02-alternate-layout.m743.png#lightbox)

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

アクティビティのルートビューにはリソース ID `two_fragments_layout`が割り当てられ、 `fragment`と`FrameLayout`という2つのサブビューがあります。 が静的に読み込まれている`FrameLayout`間、は、によって`PlayQuoteFragment`実行時に置換される "プレースホルダー" として機能します。 `fragment` で`TitlesFragment`新しいplay`PlayQuoteFragment`を選択するたびに、の新しいインスタンスでが更新されます。`playquote_container`

各サブビューでは、親の全体の高さが占有されます。 各サブビューの幅は、属性`android:layout_weight`と`android:layout_width`属性によって制御されます。 この例では、各サブビューは、親によって提供される幅の 50% を占有します。 _レイアウトの重み_の詳細について[は、LinearLayout に関する Google のドキュメント](https://developer.android.com/guide/topics/ui/layout/linear.html)を参照してください。

## <a name="2-changes-to-titlesfragment"></a>2. TitlesFragment への変更

代替レイアウトを作成したら、を更新`TitlesFragment`する必要があります。 アプリで2つのフラグメントが1つのアクティビティに表示`TitlesFragment`されて`PlayQuoteFragment`いる場合、はを親アクティビティに読み込む必要があります。 それ以外`TitlesFragment`の場合は`PlayQuoteActivity` 、をホスト`PlayQuoteFragment`するを起動します。 ブール型のフラグは`TitlesFragment` 、どの動作を使用する必要があるかを判断するのに役立ちます。 このフラグは、 `OnActivityCreated`メソッドで初期化されます。

まず、 `TitlesFragment`クラスの先頭にインスタンス変数を追加します。

```csharp
bool showingTwoFragments;
```

次に、次のコードスニペットを`OnActivityCreated`に追加して、変数を初期化します。 

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

デバイスが横モードで実行されている場合`FrameLayout`は、リソース ID `playquote_container`を持つが画面に表示され`showingTwoFragments`ます。そのため`true`、はに初期化されます。 デバイスが縦モードで実行されている`playquote_container`場合、は画面に表示され`showingTwoFragments`ないため`false`、はになります。

メソッド`ShowPlayQuote`は、フラグメントに引用符&ndash;を表示する方法、または新しいアクティビティを起動する方法を変更する必要があります。  2つ`ShowPlayQuote`のフラグメントが表示されたときにフラグメントを読み込むようにメソッドを更新します。それ以外の場合は、アクティビティを起動します。

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

ユーザーがで`PlayQuoteFragment`現在表示されている play とは異なる再生を選択した場合は、新しい`PlayQuoteFragment`が作成され、のコンテキスト`playquote_container` `FragmentTransaction`内でのコンテンツが置き換えられます。

### <a name="complete-code-for-titlesfragment"></a>TitlesFragment の完全なコード

に`TitlesFragment`対する以前の変更をすべて完了した後、完全なクラスは次のコードと一致する必要があります。

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

最後の1つの詳細情報があります`PlayQuoteActivity` 。デバイスが横モードの場合は必要ありません。 デバイスが横モードの場合、は`PlayQuoteActivity`表示されません。 `OnCreate` の`PlayQuoteActivity`メソッドを更新して、それ自体が閉じられるようにします。 このコードは、の`PlayQuoteActivity.OnCreate`最終バージョンです。

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

この変更により、デバイスの向きのチェックが追加されます。 横モードの場合、 `PlayQuoteActivity`はそれ自体を閉じます。

## <a name="4-run-the-application"></a>4.アプリケーションの実行

これらの変更が完了したら、アプリを実行し、デバイスを横向きモード (必要な場合) に回転させて、再生を選択します。 この見積もりは、再生の一覧と同じ画面に表示されます。

[![ランドスケープモードの Android フォンで実行されているアプリ](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

[![Android タブレットで実行されているアプリ](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)
