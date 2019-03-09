---
title: Actionbar タブ付きレイ アウト
description: このガイドでは、紹介し、ActionBar Api を使用して、Xamarin.Android アプリケーションで、タブ付きのユーザー インターフェイスを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: B7E60AAF-BDA5-4305-9000-675F0438734D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 6ce8099aa4230a11a12f4fe8aeffe850f9ef2ce9
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57671001"
---
# <a name="tabbed-layouts-with-the-actionbar"></a>Actionbar タブ付きレイ アウト

_このガイドでは、紹介し、ActionBar Api を使用して、Xamarin.Android アプリケーションで、タブ付きのユーザー インターフェイスを作成する方法について説明します。_


## <a name="overview"></a>概要

アクション バーでは、タブ、アプリケーション id、メニューのおよび検索といった主要機能の一貫性のあるユーザー インターフェイスを提供するために使用する Android の UI パターンです。 Android 3.0 (API レベル 11) で Google Android プラットフォームを ActionBar Api が導入されました。 ActionBar Api は、一貫したルック アンド フィールとタブ付きのユーザー インターフェイスを使用するクラスを提供する UI テーマを紹介します。 このガイドでは、Xamarin.Android アプリケーションを操作バーのタブを追加する方法について説明します。 Android 2.3 に Android 2.1 を対象とする Xamarin.Android アプリケーションにバックポート ActionBar タブに Android サポート ライブラリ v7 を使用する方法も説明します。 

なお`Toolbar`の代わりに使用する新しいより汎用的なアクション バー コンポーネント`ActionBar`(`Toolbar`を置き換える`ActionBar`)。 詳細については、次を参照してください。[ツールバー](~/android/user-interface/controls/tool-bar/index.md)します。 



## <a name="requirements"></a>必要条件

対象が API レベル 11 (Android 3.0) または高い ActionBar Api にアクセスするネイティブの Android Api の一部として Xamarin.Android アプリケーション。 

API レベル 7 (Android 2.1) に戻る移植された ActionBar Api の一部と、経由で使用できるは、 [V7 AppCompat ライブラリ](https://developer.android.com/tools/support-library/features.html#v7-appcompat)、これを使用して Xamarin.Android アプリを使用できる、 [Xamarin Android サポート ライブラリ - v7 の詳細](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)パッケージ。



## <a name="introducing-tabs-in-the-actionbar"></a>ActionBar 内のタブの概要

操作バーは、同時にそのタブのすべてを表示し、すべてのタブを最も幅の広いタブ ラベルの幅に基づくサイズに等しいことを試みます。 これは、次のスクリーン ショットで示されています。 

![すべて同じサイズのタブに表示 ActionBar の例のスクリーン ショット](with-action-bar-images/image1.png)

ActionBar には、すべてのタブを表示できません、水平方向にスクロール可能なビュー内のタブが設定されます。 ユーザーは、左または右に残りのタブを参照してくださいにスワイプします。 Google Play からこのスクリーン ショットは、この例を示しています。 

![水平方向にスクロール可能なビュー内のタブのスクリーン ショットの例](with-action-bar-images/image2.png)

操作バーの各タブに関連付けられた、 [*フラグメント*](~/android/platform/fragments/index.md)します。 ユーザーは、タブを選択するときにアプリケーションには、タブに関連付けられているフラグメントが表示されます。ActionBar は、ユーザーに適切なフラグメントを表示するための責任ではありません。 代わりに、ActionBar ActionBar.ITabListener インターフェイスを実装するクラスを使用 タブの状態の変化についてアプリケーションに通知されます。 このインターフェイスは、タブの状態が変更されたときに Android で起動される 3 つのコールバック メソッドを提供します。 

-  **OnTabSelected** -、タブを選択すると、このメソッドが呼び出されます。フラグメントが表示されます。

-  **OnTabReselected** -、タブが既に選択されているが、ユーザーがもう一度選択されているときに、このメソッドが呼び出されます。 このコールバックは、通常、表示されているフラグメントの更新/更新に使用されます。

-  **OnTabUnselected** -ユーザーが別のタブを選択すると、このメソッドが呼び出されます。このコールバックを使用して、前に表示されているフラグメントの状態を保存します。

Xamarin.Android のラップ、`ActionBar.ITabListener`のイベントに、`ActionBar.Tab`クラス。 アプリケーションは、1 つ以上のこれらのイベントにイベント ハンドラーを割り当てます。 3 つのイベント (内の各メソッドの 1 つ`ActionBar.ITabListener`) を発生させるアクションのバー タブ。 

-  TabSelected
-  TabReselected
-  TabUnselected



### <a name="adding-tabs-to-the-actionbar"></a>ActionBar をタブの追加

ActionBar ネイティブ Android 3.0 (API レベル 11) 以降、最低でもこの API を対象とするすべての Xamarin.Android アプリケーションに使用できます。 

次の手順では、Android の Activity を ActionBar タブを追加する方法を説明します。 

1. `OnCreate`アクティビティのメソッド&ndash; *UI ウィジェットを初期化する前に*&ndash;アプリケーションを設定する必要があります、`NavigationMode`上、`ActionBar`に`ActionBar.NavigationModeTabs`このコードで示すようにスニペット:

   ```csharp
   ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
   SetContentView(Resource.Layout.Main);
   ```

2. 新しいタブを使用して、作成`ActionBar.NewTab()`です。

3. イベント ハンドラーを割り当てるか、または任意`ActionBar.ITabListener`ActionBar タブと、ユーザーが対話するときに発生するイベントに応答する実装。

4. 前の手順で作成したタブの追加、`ActionBar`します。


次のコードでは、次の手順を使用して、状態の変更に応答するイベント ハンドラーを使用するアプリケーションにタブを追加する 1 つの例を示します。 

```csharp
protected override void OnCreate(Bundle bundle)
{
    ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
    SetContentView(Resource.Layout.Main);

    ActionBar.Tab tab = ActionBar.NewTab();
    tab.SetText(Resources.GetString(Resource.String.tab1_text));
    tab.SetIcon(Resource.Drawable.tab1_icon);
    tab.TabSelected += (sender, args) => {
                           // Do something when tab is selected
                       };
    ActionBar.AddTab(tab);

    tab = ActionBar.NewTab();
    tab.SetText(Resources.GetString(Resource.String.tab2_text));
    tab.SetIcon(Resource.Drawable.tab2_icon);
    tab.TabSelected += (sender, args) => {
                           // Do something when tab is selected
                       };
    ActionBar.AddTab(tab);
}
```


#### <a name="event-handlers-vs-actionbaritablistener"></a>イベント ハンドラーの vs ActionBar.ITabListener

アプリケーションは、イベント ハンドラーを使用する必要がありますと`ActionBar.ITabListener`さまざまなシナリオです。 イベント ハンドラーでは、構文上便利な; 量を提供します。クラスを作成し、実装することから保存した`ActionBar.ITabListener`します。 この便利な機能、コストがかかる&ndash;Xamarin.Android では、この変換を実行する 1 つのクラスの作成と実装の`ActionBar.ITabListener`できます。 これは、タブの数に制限がある問題ありません。 

多くのタブを扱うときまたは ActionBar タブ間で共通の機能を共有するには、メモリおよび実装するカスタム クラスを作成するパフォーマンスの観点より効率的な`ActionBar.ITabListener`、およびクラスの 1 つのインスタンスを共有します。 Xamarin.Android アプリケーションを使用して GREF の数を減らすこのされます。 



### <a name="backwards-compatibility-for-older-devices"></a>旧バージョンと古いデバイスの互換性

[Android サポート ライブラリ v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)バックエンド ポート ActionBar タブ Android 2.1 (API レベル 7)。 タブは、このコンポーネントがプロジェクトに追加されると、Xamarin.Android アプリケーションでアクセスできます。

アクティビティを ActionBar を使用するには、サブクラス化する必要があります`ActionBarActivity`AppCompat テーマを使用して、次のコード スニペットで示すようとします。

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/Theme.AppCompat", MainLauncher = true, Icon = "@drawable/ic_launcher")]
public class MainActivity: ActionBarActivity
```

アクティビティがその ActionBar からへの参照を取得する、`ActionBarActivity.SupportingActionBar`プロパティ。 次のコード スニペットは、アクティビティで ActionBar を設定する例を示しています。

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/Theme.AppCompat", MainLauncher = true, Icon = "@drawable/ic_launcher")]
public class MainActivity : ActionBarActivity, ActionBar.ITabListener
{
    static readonly string Tag = "ActionBarTabsSupport";

    public void OnTabReselected(ActionBar.Tab tab, FragmentTransaction ft)
    {
        // Optionally refresh/update the displayed tab.
        Log.Debug(Tag, "The tab {0} was re-selected.", tab.Text);
    }

    public void OnTabSelected(ActionBar.Tab tab, FragmentTransaction ft)
    {
        // Display the fragment the user should see
        Log.Debug(Tag, "The tab {0} has been selected.", tab.Text);
    }

    public void OnTabUnselected(ActionBar.Tab tab, FragmentTransaction ft)
    {
        // Save any state in the displayed fragment.
        Log.Debug(Tag, "The tab {0} as been unselected.", tab.Text);
    }

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SupportActionBar.NavigationMode = ActionBar.NavigationModeTabs;
        SetContentView(Resource.Layout.Main);
    }

    void AddTabToActionBar(int labelResourceId, int iconResourceId)
    {
        ActionBar.Tab tab = SupportActionBar.NewTab()
                                            .SetText(labelResourceId)
                                            .SetIcon(iconResourceId)
                                            .SetTabListener(this);
        SupportActionBar.AddTab(tab);
    }
}
```


## <a name="summary"></a>まとめ

このガイドでは、タブ付きのユーザー インターフェイスを ActionBar を使用して Xamarin.Android で作成する方法について説明します。 ActionBar にタブを追加する方法と経由でイベントをタブに、アクティビティの対話方法を説明した、`ActionBar.ITabListener`インターフェイス。 Android サポート ライブラリ v7 AppCompat パッケージ backports ActionBar と以前のバージョンの Android のタブをも確認しました。 


## <a name="related-links"></a>関連リンク

- [ActionBarTabs (サンプル)](https://developer.xamarin.com/samples/monodroid/UserInterface/ActionBarTabs/)
- [ツール バー](~/android/user-interface/controls/tool-bar/index.md)
- [フラグメント](~/android/platform/fragments/index.md)
- [ActionBar](https://developer.android.com/guide/topics/ui/actionbar.html)
- [ActionBarActivity](https://developer.android.com/reference/android/support/v7/app/ActionBarActivity.html)
- [アクション バーのパターン](https://developer.android.com/design/patterns/actionbar.html)
- [Android v7 AppCompat](https://developer.android.com/tools/support-library/features.html#v7-appcompat)
- [Xamarin.Android サポート ライブラリ v7 AppCompat NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
