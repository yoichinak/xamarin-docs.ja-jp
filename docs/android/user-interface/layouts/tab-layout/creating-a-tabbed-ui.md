---
title: チュートリアル - TabHost をタブの UI を作成します。
description: この記事は TabHost API を使用して Xamarin.Android にタブ付き UI の作成について説明します。
ms.prod: xamarin
ms.assetid: AD6E2173-974E-477C-940F-0CAB5E53326D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: ca9a3f3d31707205cdcd4e0d8e74fa303ccba047
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough---creating-a-tabbed-ui-with-tabhost"></a>チュートリアル - TabHost をタブの UI を作成します。

_この記事は TabHost API を使用して Xamarin.Android にタブ付き UI の作成について説明します。_

> [!NOTE]
> `TabHost` Google によっては廃止されている古い API です。 使用してタブ付きのアプリケーションを構築する開発者は、[アクションバー](~/android/user-interface/controls/action-bar.md)です。 `ActionBar`は Android のすべてのバージョンで使用できます。 Android 3.0 (API レベル 11) で初めて導入されました、Android 2.2 (API レベル 8) および Android 2.3 (API レベル 10) に移植された戻る、 [V7 AppCompat ライブラリ](http://developer.android.com/tools/support-library/features.html#v7-appcompat)、を介して Xamarin.Android に利用できる、 [XamarinAndroid のサポート ライブラリ - V7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)パッケージです。

この記事は Xamarin.Android でを使用してタブの UI を作成する手順について説明、 `TabHost` API です。 これは、Android のすべてのバージョンで使用可能な古い API です。 この例では、各タブがアクティビティでカプセル化されたロジックを持つ、3 つのタブで、アプリケーションを作成します。
次のスクリーン ショットは、作成したアプリケーションの例を示します。

![複数のタブを使用してアプリのサンプルのスクリーン ショット](creating-a-tabbed-ui-images/image02.png)


## <a name="creating-the-application"></a>アプリケーションの作成

ダウンロードして解凍[TabHostWalkthrough](https://developer.xamarin.com/samples/monodroid/UserInterface/TabHostWalkthrough/)です。
このプロジェクトは、アプリケーションの開始点として機能し、一部の画像が含まれています。 このプロジェクトを確認する場合、ドロウアブル タブのアイコンのリソースを作成した既にしたということが表示されます。

最初のレイアウト ファイルを更新してみましょう**Resources/Layout/Main.axml**タブをホストします。 編集**Resources/Layout/Main.axml**し、次の XML を挿入します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<TabHost xmlns:android="http://schemas.android.com/apk/res/android"
         android:id="@android:id/tabhost"
         android:layout_width="fill_parent"
         android:layout_height="fill_parent">
    <LinearLayout
            android:orientation="vertical"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent"
            android:padding="5dp">
        <TabWidget
                android:id="@android:id/tabs"
                android:layout_width="fill_parent"
                android:layout_height="wrap_content" />
        <FrameLayout
                android:id="@android:id/tabcontent"
                android:layout_width="fill_parent"
                android:layout_height="fill_parent"
                android:padding="5dp" />
    </LinearLayout>
</TabHost>
```

次のスクリーン ショットは、Xamarin デザイナー レイアウトを示しています。

[![Xamarin デザイナーで TabHost レイアウトのスクリーン ショット](creating-a-tabbed-ui-images/image04-sml.png)](creating-a-tabbed-ui-images/image04.png#lightbox)

TabHost がその内部の 2 つの子ビューを持つ必要があります。`TabWidget`と`FrameLayout`です。 位置に、`TabWidget`と`FrameLayout`垂直方向に内側、 `TabHost`、`LinearLayout`を使用します。 FrameLayout はどこのコンテンツの各タブを移動、空であるため、`TabHost`は自動的に実行時に、各アクティビティを埋め込みます。 これにはタブ付きのユーザー インターフェイスのレイアウトを作成する際に従う必要がありますをいくつかのルールがあります。

-   `TabHost` Id を持つ必要があります`@android:id/tabhost`です。

-   `TabWidget` Id を持つ必要があります`@android:id/tabs`です。

-   `FrameLayout` Id を持つ必要があります`@android:id/tabcontent`です。

-   `TabHost` 継承する、管理している任意のアクティビティを必要と`TabActivity`です。 したがって、することが重要サブクラス`TabActivity`ここ&ndash;通常の活動は機能しません。

ファイルを編集して**MainActivity.cs**ようにクラス`MainActivity`サブクラス`TabActivity`次のコード スニペットで示すようにします。

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/ic_launcher")]
public class MainActivity : TabActivity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.Main);
    }
}
```

プロジェクトに次の 4 つの別々 のアクティビティ クラスを作成します。 `MyScheduleActivity`、 `SessionsActivity`、 `SpeakersActivity`、および`WhatsOnActivity`です。 各アクティビティは、タブの UI を構成します。これらのアクティビティが表示するスタブをするようになりました、`TextView`単純なメッセージを使用します。 各活動を含む、次のコードを編集`OnCreate`実装します。

```csharp
[Activity]
public class MyScheduleActivity : Activity
{
    protected override void OnCreate (Bundle savedInstanceState)
    {
        base.OnCreate (savedInstanceState);
        TextView textview = new TextView (this);
        textview.Text = "This is the My Schedule tab";
        SetContentView (textview);
    }
}
```

上記のコードがレイアウト ファイルを使用しないことに注意してください。 だけを作成、`TextView`いくつかのテキストを設定して`TextView`コンテンツ ビューとして。 残りの 3 つのアクティビティの列ごとには、これを複製します。

次をアイコンに各タブに割り当てます。各タブには、2 つのアイコンが必要です&ndash;選択された状態と、選択されていない状態のいずれかのいずれか。 これら 2 つの異なるアイコンの例は、(このアプリケーションのために必要なアイコンが既に追加されて、サンプル プロジェクトを次の 2 つのイメージに表示されることができます。

![選択した状態、および選択されていない状態アイコンのスクリーン ショット](creating-a-tabbed-ui-images/tab-icons.png)


割り当てますドロウアブル リソース アイコンのタブに定義することによって、*状態リスト ディスプレイ*です。 状態リスト ドロウアブルは、XML で定義され、そのアイテムの状態に固有のさまざまなイメージを指定することを特別なドロウアブル リソースです。 この例では、タブが選択されているときに使用される 1 つのイメージと、タブが選択されていないときに使用されます。 時間を節約必要状態リスト ドロウアブルがプロジェクトに追加された、します。 次の一覧は、ファイルを示し、各 XML が含まれています。

-   `ic_tab_my_schedule.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_whats_on_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_whats_on_unselected"/>
    </selector>
    ```

-   `ic_tab_sessions.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_sessions_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_sessions_unselected"/>
    </selector>
    ```

-   `ic_tab_speakers.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_speakers_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_speakers_unselected"/>
    </selector>
    ```

-   `ic_tab_whats_on.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_whats_on_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_whats_on_unselected"/>
    </selector>
    ```

タブを追加、`TabHost`非常に繰り返しタスクはプログラムでは、します。 これを支援するためには、クラスに次のメソッドを追加`MainActivity`:

```csharp
private void CreateTab(Type activityType, string tag, string label, int drawableId )
{
    var intent = new Intent(this, activityType);
    intent.AddFlags(ActivityFlags.NewTask);

    var spec = TabHost.NewTabSpec(tag);
    var drawableIcon = Resources.GetDrawable(drawableId);
    spec.SetIndicator(label, drawableIcon);
    spec.SetContent(intent);

    TabHost.AddTab(spec);
}
```

各タブに、`TabHost`のインスタンスで表されるの`TabHost.TabSpec`クラスです。 このインスタンスには、メタデータが含まれています。 具体的には、[] タブを表示するために必要な。

-   **テキストとアイコン**&ndash;に表示される、`TabWidget`です。

-   **タブのコンテンツは**&ndash;いずれかの可能性があります、`Activity`または`View`タブが選択されていると表示されます。

-   **一意のタグ**&ndash;各タブに割り当てられている一意のタグを持つ必要があります。

追加する必要があります、`TabHost.TabSpec`の各タブに、アプリケーションのインスタンス。 次の手順で行うことができます。

メソッドを更新して`OnCreate`で`MainActivity`次のコードのようにします。

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    CreateTab(typeof(WhatsOnActivity), "whats_on", "What's On", Resource.Drawable.ic_tab_whats_on);
    CreateTab(typeof(SpeakersActivity), "speakers", "Speakers", Resource.Drawable.ic_tab_speakers);
    CreateTab(typeof(SessionsActivity), "sessions", "Sessions", Resource.Drawable.ic_tab_sessions);
    CreateTab(typeof(MyScheduleActivity), "my_schedule", "My Schedule", Resource.Drawable.ic_tab_my_schedule);
}
```

アプリケーションを実行します。 アプリケーションは、このチュートリアルの冒頭に示したスクリーン ショットのようになります。

これで完了です。 アプリケーションのさまざまな部分に簡単に移動するユーザーに与えますタブ付きアプリケーションを作成しました。



## <a name="summary"></a>まとめ

この章では、タブ付きレイアウトを説明し、タブ付きのアプリケーションの作成プロセスを案内します。 チュートリアルを使用する方法を示しました、`TabActivity`そのホストが、レイアウトを拡大するファイル、`TabHost`と`TabWidget`です。 `TabHost`のコレクションが、設定されている`TabHost.TabSpec`オブジェクトによって使用される、`TabHost`実行時に、各タブで使用されるアクティビティのインスタンスを作成します。



## <a name="related-links"></a>関連リンク

- [TabHostWalkthrough (サンプル)](https://developer.xamarin.com/samples/monodroid/UserInterface/TabHostWalkthrough/)
- [TabHost](https://developer.xamarin.com/api/type/Android.Widget.TabHost/)
- [TabWidget](https://developer.xamarin.com/api/type/Android.Widget.TabWidget/)
- [TabActivity](https://developer.xamarin.com/api/type/Android.App.TabActivity/)
- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Android のサポート ライブラリ v7 AppCompat NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [v7 appcompat ライブラリ](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
