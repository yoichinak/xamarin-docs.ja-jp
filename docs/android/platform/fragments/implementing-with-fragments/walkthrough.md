---
title: チュートリアル
ms.prod: xamarin
ms.assetid: ED368FA9-A34E-DC39-D535-5C34C32B9761
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: cc5c05fce9b9c3dd974afe027cc7cbb60085c621
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough"></a>チュートリアル

次の手順で、基本的なアプリにフラグメントが作成されます。 最初の手順では、Android プロジェクトに対して新しい Xamarin.Android を作成します。

## <a name="1-create-a-project"></a>1.プロジェクトの作成

いう新しい Xamarin.Android プロジェクトの作成**FragmentSample**です。 **最低限の Android**バージョン必要があります設定する Android 3.1 以降に、次の図に示すようにします。

[![最低限の Android バージョンを設定](walkthrough-images/00.png)](walkthrough-images/00.png#lightbox)


## <a name="2-create-the-mainactivity"></a>2.MainActivity を作成します。

次に、作成する必要があります、`MainActivity`クラスです。 これは、アプリケーションのスタートアップ アクティビティです。 このアクティビティは、画面のサイズに応じて、1 つまたは両方のフラグメントをホストします。 `MainActivity` これは実行、デバイスに適切なレイアウト ファイルを読み込んでいます。

```csharp
[Activity(Label = "Fragments Walkthrough", MainLauncher = true, Icon = "@drawable/launcher")]
public class MainActivity : Activity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       SetContentView(Resource.Layout.activity_main);
   }
}
```

> [!NOTE]
> `Note:` フラグメントのサブクラスでは、コンス トラクターの引数がない、パブリックの既定値が必要です。

## <a name="3-create-the-layout-files"></a>3.レイアウト ファイルを作成します。

2 つの異なる画面サイズでは、2 つの異なるレイアウト ファイルが必要です。 新しいフォルダーを作成しましょう**リソース/レイアウト大きな**と呼ばれる新しいレイアウトを作成および**activity_main.axml**です。 として既定のレイアウト ファイルの名前を変更も**Resources/Layout/activity_main.axml**です。 これらの変更後にレイアウト フォルダーは、次のスクリーン ショットのようになります。

[![IDE でレイアウト フォルダーのスクリーン ショット](walkthrough-images/01.png)](walkthrough-images/01.png#lightbox)


すべてのデバイスは読み込まれてレイアウト ファイルを使用して**リソース/レイアウト**です。
だけを表示する非常に単純なレイアウトは、 `TitlesFragment`:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:orientation="horizontal"
       android:layout_width="fill_parent"
       android:layout_height="fill_parent">
 <fragment class="com.xamarin.sample.fragments.supportlib.TitlesFragment"
           android:id="@+id/titles_fragment"
           android:layout_width="fill_parent"
           android:layout_height="fill_parent" />
</LinearLayout>
```

Android デバイスより大きな画面がある場合でレイアウト ファイルの読み込みは**リソース/レイアウト大きな**です。 タブレットのレイアウトの内容は次のとおりです。

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:orientation="horizontal"
       android:layout_width="fill_parent"
       android:layout_height="fill_parent">
 <fragment class="com.xamarin.sample.fragments.supportlib.TitlesFragment"
           android:id="@+id/titles_fragment"
           android:layout_weight="1"
           android:layout_width="0px"
           android:layout_height="match_parent" />
 <FrameLayout android:id="@+id/details"
              android:layout_weight="1"
              android:layout_width="0px"
              android:layout_height="match_parent" />
</LinearLayout>
```

大きな画面のレイアウト ファイルは若干異なります。 あるだけでなく、`TitlesFragment`このレイアウト ファイルに表示されますが、`FrameLayout`フラグメントの横に表示されます。 大きな画面で、`DetailsFragment`にプログラムで追加`MainActivity`ユーザーが、再生を選択するとします。 後で、この方法で詳しく説明します。

Android 3.2 では、画面のレイアウトを指定する新しい方法が導入されました。 このような新しい修飾子は、画面のサイズではなく、レイアウトが必要な領域の量を指定します。 このアプリケーションが Android 3.2 でのみ、またはそれ以降を実行することを意図した場合、を作成し、**リソース/レイアウト-sw600dp**フォルダー (フォルダーではなく**リソース/レイアウト大きな**) レイアウト ファイルの**activity_main.axml**です。 このリソース ファイルを 600 密度に依存しないピクセル単位の最小の画面の幅を持つすべてのデバイスによって読み込まれるとします。 ただし、このアプリケーションが Android 3.1 のターゲットを設定または以降では、古いリソース修飾子を使用します。

## <a name="4-create-the-titlesfragment"></a>4.TitlesFragment を作成します。

`TitlesFragment` さまざまなアプローチのタイトルを表示、プロジェクトに新しいフラグメントと呼ばれてでは追加`TitlesFragment`:

[![TitlesFragment プロジェクトに、新しいフラグメントを追加します。](walkthrough-images/02.png)](walkthrough-images/02.png#lightbox)

後に`TitlesFragment`追加されて、クラスから継承するように変更する必要があります`Android.App.ListFragment`です。 `ListFragment` 機能の一覧を含む特殊なフラグメント型です。
`TitlesFragment` また、無効`OnActivityCreated`(別のフラグメントのライフ サイクル メソッド) を提供し、`Adapter`を`ListFragment`リストの生成に使用されます。

```csharp
public override void OnActivityCreated(Bundle savedInstanceState)
{
   base.OnActivityCreated(savedInstanceState);
   var adapter = new ArrayAdapter<String>(Activity, Android.Resource.Layout.SimpleListItemChecked, Shakespeare.Titles);
   ListAdapter = adapter;
   if (savedInstanceState != null)
   {
       _currentPlayId = savedInstanceState.GetInt("current_play_id", 0);
   }
   var detailsFrame = Activity.FindViewById<View>(Resource.Id.details);
   _isDualPane = detailsFrame != null && detailsFrame.Visibility == ViewStates.Visible;
   if (_isDualPane)
   {
       ListView.ChoiceMode = (int) ChoiceMode.Single;
       ShowDetails(_currentPlayId);
   }
}
```

前述のように、アプリケーションが 2 つのレイアウトを`MainActivity`です。 内のコード`OnActivityCreated`の存在を検出、`FrameLayout`しレイアウト ファイルが読み込まれたを決定します。 場合、`FrameLayout`レイアウトでは、存在する、`_isDualPane`にフラグが設定されている`true`です。 `_isDualPane`フラグがによって明示的に使用、アクティビティで、他の場所、`ShowDetails`メソッドです。 `ShowDetails`メソッドはで詳しく取り上げます。

`TitlesFragment` 一覧は、ユーザーの選択リスト内に応答する必要があります。 これを行う`TitlesFragment`メソッドをオーバーライド`OnListItemClick`です。 内部`OnListItemClick`、新しい`DetailsFragment`が作成されに表示される、 `FrameLayout`、プログラムです。 内の関連コード`TitlesFragment`は。

```csharp
public override void OnListItemClick(ListView l, View v, int position, long id)
{
   ShowDetails(position);
}
private void ShowDetails(int playId)
{
   _currentPlayId = playId;
   if (_isDualPane)
   {
       // We can display everything in place with fragments.
       // Have the list highlight this item and show the data.
       ListView.SetItemChecked(playId, true);
       // Check what fragment is shown, replace if needed.
       var details = FragmentManager.FindFragmentById(Resource.Id.details) as DetailsFragment;
       if (details == null || details.ShownPlayId != playId)
       {
           // Make new fragment to show this selection.
           details = DetailsFragment.NewInstance(playId);
           // Execute a transaction, replacing any existing
           // fragment with this one inside the frame.
           var ft = FragmentManager.BeginTransaction();
           ft.Replace(Resource.Id.details, details);
           ft.SetTransition(FragmentTransaction.TransitFragmentFade);
           ft.Commit();
       }
   }
   else
   {
       // Otherwise we need to launch a new Activity to display
       // the dialog fragment with selected text.
       var intent = new Intent();
       intent.SetClass(Activity, typeof (DetailsActivity));
       intent.PutExtra("current_play_id", playId);
       StartActivity(intent);
   }
}
```

コードは、デバイスから書式設定して、選択した再生から見積もりを表示する方法を決定します。 場合は、タブレット、`_isDualPane`フラグが設定されます`true`、見積もりが横に表示されるようにし、`TitlesFragment`です。 場合、選択した再生`id`まだ表示されていない、し、新しい`DetailsFragment`が作成され、しに読み込まれる、`FrameLayout`アクティビティにします。 他のデバイス、大きなディスプレイがない&ndash;フォン、たとえば&ndash;`isDualPane`に設定されます`false`ように新しい`DetailsActivity`が開始されます。


## <a name="5-create-the-detailsactivity"></a>5.DetailsActivity を作成します。

`DetailsActivity`が表示されます、`DetailsFragment`小さいデバイス。 これを見るには、まず追加、新しいアクティビティという名前のプロジェクトに`DetailsActivity`です。 `DetailsActivity` 非常に単純なアクティビティです。 作成され、新しいホスト`DetailsFragment`再生`id`が送信します。

```csharp
[Activity(Label = "Details Activity")]
public class DetailsActivity : Activity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       var index = Intent.Extras.GetInt("current_play_id", 0);

       var details = DetailsFragment.NewInstance(index); // DetailsFragment.NewInstance is a factory method to create a Details Fragment
       var fragmentTransaction = FragmentManager.BeginTransaction();
       fragmentTransaction.Add(Android.Resource.Id.Content, details);
       fragmentTransaction.Commit();
   }
}
```

レイアウト ファイルが読み込まれていないことを確認`DetailsActivity`です。 代わりに、`DetailsFragment`はアクティビティのルート ビューに読み込まれます。 このルート ビューは、特別な ID を持つ`Android.Resource.Id.Content`します。 新しい`DetailFragment`が作成され、このルート内のビューに追加し、`FragmentTransaction`によってアクティビティの作成`FragmentManager`です。


## <a name="6-create-the-detailsfragment"></a>6.DetailsFragment を作成します。

ここで、別のフラグメントをという名前のアプリケーションに追加してみましょう`DetailsFragment`です。 このフラグメントでは、選択されている再生の見積もりを表示します。 次のコードは、完全な`DetailsFragment`:

```csharp
internal class DetailsFragment : Fragment
{
   public static DetailsFragment NewInstance(int playId)
   {
       var detailsFrag = new DetailsFragment {Arguments = new Bundle()};
       detailsFrag.Arguments.PutInt("current_play_id", playId);
       return detailsFrag;
   }
   public int ShownPlayId
   {
       get { return Arguments.GetInt("current_play_id", 0); }
   }
   public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
   {
       if (container == null)
       {
           // Currently in a layout without a container, so no reason to create our view.
           return null;
       }
       var scroller = new ScrollView(Activity);
       var text = new TextView(Activity);
       var padding = Convert.ToInt32(TypedValue.ApplyDimension(ComplexUnitType.Dip, 4, Activity.Resources.DisplayMetrics));
       text.SetPadding(padding, padding, padding, padding);
       text.TextSize = 24;
       text.Text = Shakespeare.Dialogue[ShownPlayId];
       scroller.AddView(text);
       return scroller;
   }
}
```

順序で`DetailsFragment`正常に機能することが必要で選択されている再生のインデックス、`TitlesFragment`です。 この値を指定する方法はたくさんあります`DetailsFragment`以外の場合はこの例では、再生`Id`バンドルに配置バンドルは、引数のプロパティのインスタンスに格納されていると、`DetailsFragment`です。 プロパティ`ShownPlayId`便利&ndash;のインスタンスによって使用されます`DetailsFragment`バンドルからその値を取得します。

`OnCreateView` フラグメントは、そのユーザー インターフェイスを描画する必要があるし、返す必要があるときに呼び出される、`Android.Views.View`オブジェクト。 これは、ほとんどの場合、`View`既存のレイアウト ファイルから値が大ききます。 上の例の場合、フラグメントが表示に使用されるビュー プログラムで作成されます。

おめでとうございます!  開発を簡略化のフォーム ファクターでフラグメントを使用するアプリケーションが作成されました。

[次のセクション](supporting-pre-honeycomb.md)、ここで事前 Android 3.0 デバイスで動作するように、このアプリケーションを拡張します。

