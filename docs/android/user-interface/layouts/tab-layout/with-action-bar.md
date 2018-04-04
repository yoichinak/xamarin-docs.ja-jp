---
title: アクションバーを持つタブ付きレイアウト
description: このガイドでは、紹介し、アクションバー Api を使用して Xamarin.Android アプリケーションで、タブ付きのユーザー インターフェイスを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: B7E60AAF-BDA5-4305-9000-675F0438734D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 5956cd13708f4e7e73926fc01e6142d9cf4a8edb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="tabbed-layouts-with-the-actionbar"></a>アクションバーを持つタブ付きレイアウト

_このガイドでは、紹介し、アクションバー Api を使用して Xamarin.Android アプリケーションで、タブ付きのユーザー インターフェイスを作成する方法について説明します。_


## <a name="overview"></a>概要

操作バーは、タブ、アプリケーション id、メニューのおよび検索などの主要機能の一貫性のあるユーザー インターフェイスを提供するために使用する Android UI パターンです。 Android 3.0 (API レベル 11) では、Google Android プラットフォームをアクションバー Api が導入されました。 アクションバー Api は、一貫したルック アンド フィールとタブ付きのユーザー インターフェイスを使用できるクラスを提供する UI のテーマを紹介します。 このガイドでは、Xamarin.Android アプリケーションを操作バー タブを追加する方法について説明します。 Android のサポート ライブラリ v7 を Android 2.3 に Android 2.1 を対象とする Xamarin.Android アプリケーションを移植アクションバー タブを使用する方法についても説明します。 

なお`Toolbar`の代わりに使用する新しいおよびより汎用的なアクション バー コンポーネント`ActionBar`(`Toolbar`を交換できるように設計されました`ActionBar`)。 詳細については、次を参照してください。[ツールバー](~/android/user-interface/controls/tool-bar/index.md)です。 



## <a name="requirements"></a>要件

ターゲット API レベル 11 (Android 3.0) または高いネイティブの Android Api の一部としてアクションバー Api へのアクセスを持つ Xamarin.Android アプリケーションです。 

API レベル (Android 2.1) を 7 に戻る移植されたアクションバー Api の一部であり経由で入手できますが、 [V7 AppCompat ライブラリ](http://developer.android.com/tools/support-library/features.html#v7-appcompat)が可能な Xamarin.Android アプリを使用して、 [Xamarin Android サポート ライブラリ - V7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)パッケージです。



## <a name="introducing-tabs-in-the-actionbar"></a>アクションバーのタブの概要

操作バーしようと同時にすべてのタブの表示し、同じサイズの最も幅の広いタブ ラベルの幅に基づくすべてのタブを作成します。 次のスクリーン ショットでこの例を示します。 

![示されるすべてのサイズが等しいかどうかのタブとアクションバーの例のスクリーン ショット](with-action-bar-images/image1.png)

アクションバーには、すべてのタブを表示できない場合、は、水平方向にスクロール可能なビュー内のタブを設定します。 ユーザーは、左または右に残りのタブを参照してくださいスワイプ可能性があります。 Google Play からこのスクリーン ショットは、この例を示しています。 

![水平方向にスクロール可能なビュー内のタブの例のスクリーン ショット](with-action-bar-images/image2.png)

操作バーの各タブが関連付けられる必要があります、 [*フラグメント*](~/android/platform/fragments/index.md)です。 ユーザーがタブを選択すると、アプリケーションには、タブに関連付けられているフラグメントが表示されます。アクションバーが適切なフラグメントをユーザーに表示するの責任ではありません。 代わりに、アクションバー ActionBar.ITabListener インターフェイスを実装するクラスを使用して、タブで状態の変更をアプリケーションに通知されます。 このインターフェイスは、タブの状態が変更されたときに Android で起動される 3 つのコールバック メソッドを提供します。 

-  **OnTabSelected** -ユーザーは、タブを選択したときに、このメソッドが呼び出されます。フラグメントが表示されます。

-  **OnTabReselected**のタブが既に選択されているが、ユーザーがもう一度選択されている場合に、このメソッドが呼び出されます。 通常、このコールバックは、表示されているフラグメントの更新/更新に使用されます。

-  **OnTabUnselected** -ユーザーが別のタブを選択したときに、このメソッドが呼び出されます。このコールバックは表示されなくなります前に、表示されているフラグメントの状態を保存するために使用します。

Xamarin.Android ラップ、`ActionBar.ITabListener`のイベントに、`ActionBar.Tab`クラスです。 アプリケーションは、これらのイベントの 1 つ以上をイベント ハンドラーを割り当てることがあります。 3 つのイベント (内の各メソッドのいずれかの`ActionBar.ITabListener`)、[操作] バー タブを発生させる。 

-  TabSelected
-  TabReselected
-  TabUnselected



### <a name="adding-tabs-to-the-actionbar"></a>アクションバーへのタブの追加

アクションバー ネイティブ Android 3.0 (API レベル 11) 以降、最低でもこの API を対象とする任意の Xamarin.Android アプリケーションに対して使用できます。 

次の手順では、Android のアクティビティにアクションバー タブを追加する方法を示しています。 

1. `OnCreate`アクティビティのメソッド&ndash;*任意 UI ウィジェットを初期化する前に*&ndash;アプリケーションを設定する必要があります、`NavigationMode`上、`ActionBar`に`ActionBar.NavigationModeTabs`次のコードに示すようにスニペット:

   ```csharp
   ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
   SetContentView(Resource.Layout.Main);
   ```

2. 新しいタブを使用して、作成`ActionBar.NewTab()`です。

3. イベント ハンドラーを割り当てるか、または任意`ActionBar.ITabListener`アクションバー タブを持つユーザーが操作するときに発生するイベントに応答する実装。

4. 前の手順で作成されたタブの追加、`ActionBar`です。


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
                       }
    ActionBar.AddTab(tab);

    tab = ActionBar.NewTab();
    tab.SetText(Resources.GetString(Resource.String.tab2_text));
    tab.SetIcon(Resource.Drawable.tab2_icon);
    tab.TabSelected += (sender, args) => {
                           // Do something when tab is selected
                       }
    ActionBar.AddTab(tab);
}
```


#### <a name="event-handlers-vs-actionbaritablistener"></a>イベント ハンドラーと ActionBar.ITabListener

アプリケーションは、イベント ハンドラーを使用する必要がありますと`ActionBar.ITabListener`さまざまなシナリオです。 イベント ハンドラーでは、構文上の便利な; 量を用意します。保存することが、クラスを作成して実装する必要がなくなります`ActionBar.ITabListener`です。 この機能はコストで得られる&ndash;Xamarin.Android では、この変換を実行する、1 つのクラスの作成と実装の`ActionBar.ITabListener`します。 これは、タブの数に制限がある問題ありません。 

多くのタブを処理する場合またはアクションバー タブの間で共通の機能を共有するには、メモリおよびを実装するカスタム クラスを作成するパフォーマンスの観点から効率`ActionBar.ITabListener`、およびクラスの 1 つのインスタンスを共有します。 これにより、Xamarin.Android アプリケーションを使用して GREF の数が減ります。 



### <a name="backwards-compatibility-for-older-devices"></a>旧バージョンと古いデバイスの互換性

[Android のサポート ライブラリ v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)背面ポート アクションバー タブ Android 2.1 (API レベル 7)。 タブは、このコンポーネントがプロジェクトに追加される Xamarin.Android アプリケーションにアクセスできます。

使用するには、アクションバー サブクラス アクティビティする必要があります。`ActionBarActivity`し、次のコード スニペットに示すように、AppCompat テーマを使用します。

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/Theme.AppCompat", MainLauncher = true, Icon = "@drawable/ic_launcher")]
public class MainActivity: ActionBarActivity
```

アクティビティがそのアクションバーからへの参照を取得する、`ActionBarActivity.SupportingActionBar`プロパティです。 次のコード スニペットは、アクティビティでアクションバーを設定する例を示しています。

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

このガイドでは、タブ付きのユーザー インターフェイスを使用して、アクションバー Xamarin.Android で作成する方法について説明します。 アクションバーにタブを追加する方法とアクティビティを使用してタブ イベントと対話方法を説明した、`ActionBar.ITabListener`インターフェイスです。 古いバージョンの Android サポート ライブラリを Android v7 AppCompat パッケージ backports、アクションバーのタブをも説明しました。 


## <a name="related-links"></a>関連リンク

- [ActionBarTabs (サンプル)](https://developer.xamarin.com/samples/monodroid/UserInterface/ActionBarTabs/)
- [ツール バー](~/android/user-interface/controls/tool-bar/index.md)
- [フラグメント](~/android/platform/fragments/index.md)
- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [ActionBarActivity](http://developer.android.com/reference/android/support/v7/app/ActionBarActivity.html)
- [操作バー パターン](http://developer.android.com/design/patterns/actionbar.html)
- [Android v7 AppCompat](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
- [Xamarin.Android サポート ライブラリ v7 AppCompat NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
