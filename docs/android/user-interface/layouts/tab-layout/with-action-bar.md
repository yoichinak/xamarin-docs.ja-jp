---
title: ActionBar を使用したタブ付きレイアウト
description: このガイドでは、ActionBar Api を使用して、Xamarin Android アプリケーションでタブ付きユーザーインターフェイスを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: B7E60AAF-BDA5-4305-9000-675F0438734D
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: b1e6fadb52f6009e28d18143ac24c5180dae037c
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457043"
---
# <a name="tabbed-layouts-with-the-actionbar"></a>ActionBar を使用したタブ付きレイアウト

_このガイドでは、ActionBar Api を使用して、Xamarin Android アプリケーションでタブ付きユーザーインターフェイスを作成する方法について説明します。_

## <a name="overview"></a>概要

アクションバーは、タブ、アプリケーション id、メニュー、検索などの主要な機能に対して一貫したユーザーインターフェイスを提供するために使用される Android UI パターンです。 Android 3.0 (API レベル 11) では、Google によって、ActionBar Api が Android プラットフォームに導入されました。 ActionBar Api は UI テーマを導入して、タブ付きユーザーインターフェイスを可能にする一貫したルックアンドフィールとクラスを提供します。 このガイドでは、操作バータブを Xamarin Android アプリケーションに追加する方法について説明します。 また、android サポートライブラリ v7 を使用して、android 2.1 を対象とするバックポート ActionBar タブを android 2.3 にする方法についても説明します。 

は、 `Toolbar` の代わりに使用する必要がある、より新しい一般化されたアクションバーコンポーネントであることに注意してください `ActionBar` (を `Toolbar` 置き換えるように設計されてい `ActionBar` ます)。 詳細については、「 [ツールバー](~/android/user-interface/controls/tool-bar/index.md)」を参照してください。 

## <a name="requirements"></a>必要条件

API レベル 11 (Android 3.0) 以降を対象とするすべての Xamarin Android アプリケーションは、ネイティブ Android Api の一部として ActionBar Api にアクセスできます。 

いくつかの ActionBar Api は、API レベル 7 (Android 2.1) に移植されており、 [V7 AppCompat ライブラリ](https://developer.android.com/tools/support-library/features.html#v7-appcompat)を通じて入手できます。このライブラリは、Xamarin [android Support library-V7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) パッケージを介して xamarin android アプリで使用できます。

## <a name="introducing-tabs-in-the-actionbar"></a>ActionBar でのタブの概要

操作バーでは、すべてのタブが同時に表示され、すべてのタブのサイズは、最も幅の広いタブラベルの幅に基づいて設定されます。 これを次のスクリーンショットに示します。 

![同じサイズのタブがすべて表示されている ActionBar のスクリーンショットの例](with-action-bar-images/image1.png)

ActionBar は、すべてのタブを表示できない場合、水平方向にスクロール可能なビューでタブを設定します。 ユーザーは、左または右にスワイプして残りのタブを表示することができます。 次のスクリーンショットは、この例を Google Play 示しています。 

![水平方向にスクロール可能なビューのタブのスクリーンショットの例](with-action-bar-images/image2.png)

アクションバーの各タブは、 [*フラグメント*](~/android/platform/fragments/index.md)に関連付けられている必要があります。 ユーザーがタブを選択すると、アプリケーションには、タブに関連付けられているフラグメントが表示されます。ActionBar は、ユーザーに適切なフラグメントを表示する役割がありません。 代わりに、ActionBar は、ActionBar. ITabListener インターフェイスを実装するクラスを使用して、タブの状態の変化についてアプリケーションに通知します。 このインターフェイスには、タブの状態が変化したときに Android によって起動される3つのコールバックメソッドが用意されています。 

- **Ontabselected** -このメソッドは、ユーザーがタブを選択したときに呼び出されます。フラグメントが表示されます。

- **Ontabreselected** 実行-このメソッドは、タブが既に選択されているが、ユーザーによって再度選択された場合に呼び出されます。 通常、このコールバックは、表示されているフラグメントを更新/更新するために使用されます。

- **Ontabunselected** -このメソッドは、ユーザーが別のタブを選択したときに呼び出されます。このコールバックは、表示されなくなったフラグメントの状態を保存するために使用されます。

Xamarin は、クラスの `ActionBar.ITabListener` イベントを使用してをラップします `ActionBar.Tab` 。 アプリケーションでは、イベントハンドラーをこれらのイベントの1つ以上に割り当てることができます。 [アクションバー] タブでは、次の3つのイベント (のメソッドごとに1つ `ActionBar.ITabListener` ) が発生します。 

- 選択されたタブ
- タブの前にある
- タブの選択解除

### <a name="adding-tabs-to-the-actionbar"></a>ActionBar へのタブの追加

ActionBar は Android 3.0 (API レベル 11) 以上にネイティブであり、この API を最小として対象とするすべての Xamarin Android アプリケーションで使用できます。 

次の手順では、ActionBar タブを Android アクティビティに追加する方法について説明します。 

1. `OnCreate` &ndash; *UI ウィジェットを初期化する前*のアクティビティのメソッドでは &ndash; `NavigationMode` 、 `ActionBar` 次の `ActionBar.NavigationModeTabs` コードスニペットに示すように、アプリケーションでをに設定する必要があります。

   ```csharp
   ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
   SetContentView(Resource.Layout.Main);
   ```

2. を使用して新しいタブを作成し `ActionBar.NewTab()` ます。

3. イベントハンドラーを割り当てるか、または `ActionBar.ITabListener` ユーザーが ActionBar タブと対話したときに発生するイベントに応答するカスタム実装を提供します。

4. 前の手順で作成したタブをに追加し `ActionBar` ます。

次のコードは、イベントハンドラーを使用して状態の変更に応答するアプリケーションにタブを追加するために、これらの手順を使用する例の1つです。 

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

#### <a name="event-handlers-vs-actionbaritablistener"></a>イベントハンドラーと ActionBar. ITabListener

アプリケーションでは、さまざまなシナリオでイベントハンドラーとを使用する必要があり `ActionBar.ITabListener` ます。 イベントハンドラーでは、一定量の構文上の利便性を提供します。これにより、クラスを作成してを実装する必要がなく `ActionBar.ITabListener` なります。 このような利便性は Xamarin に &ndash; あります。 Android では、この変換を実行して、1つのクラスを作成し、を実装 `ActionBar.ITabListener` します。 これは、アプリケーションのタブの数が限られている場合には問題ありません。 

多くのタブを処理する場合、または ActionBar タブ間で共通の機能を共有する場合、メモリとパフォーマンスの観点から、 `ActionBar.ITabListener` クラスの1つのインスタンスを実装し、共有するカスタムクラスを作成する方が効率的な場合があります。 これにより、Xamarin Android アプリケーションで使用されている複素数を減らすことができます。 

### <a name="backwards-compatibility-for-older-devices"></a>古いデバイスの旧バージョンとの互換性

[Android サポートライブラリ](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)によって、v7 のポートの actionbar タブが android 2.1 (API レベル 7) に戻されます。 このコンポーネントがプロジェクトに追加されると、Xamarin Android アプリケーションでタブにアクセスできるようになります。

ActionBar を使用するには、次の `ActionBarActivity` コードスニペットに示すように、アクティビティが AppCompat テーマをサブクラス化して使用する必要があります。

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/Theme.AppCompat", MainLauncher = true, Icon = "@drawable/ic_launcher")]
public class MainActivity: ActionBarActivity
```

アクティビティは、プロパティから ActionBar への参照を取得でき `ActionBarActivity.SupportingActionBar` ます。 次のコードスニペットは、アクティビティの ActionBar を設定する例を示しています。

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

このガイドでは、ActionBar を使用して Xamarin Android でタブ付きユーザーインターフェイスを作成する方法について説明しました。 ここでは、ActionBar にタブを追加する方法と、インターフェイスを使用してアクティビティが tab イベントと対話する方法について説明し `ActionBar.ITabListener` ます。 また、Android サポートライブラリの v7 AppCompat パッケージ backports が、以前のバージョンの Android に ActionBar タブを戻す方法についても説明しました。 

## <a name="related-links"></a>関連リンク

- [ActionBarTabs (サンプル)](/samples/xamarin/monodroid-samples/userinterface-actionbartabs)
- [ツール バー](~/android/user-interface/controls/tool-bar/index.md)
- [フラグメント](~/android/platform/fragments/index.md)
- [ActionBar](https://developer.android.com/guide/topics/ui/actionbar.html)
- [ActionBarActivity](https://developer.android.com/reference/android/support/v7/app/ActionBarActivity.html)
- [操作バーパターン](https://developer.android.com/design/patterns/actionbar.html)
- [Android v7 AppCompat](https://developer.android.com/tools/support-library/features.html#v7-appcompat)
- [Xamarin Android サポートライブラリ v7 AppCompat NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)