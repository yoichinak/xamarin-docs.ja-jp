---
title: Android プラットフォーム仕様
description: この記事では Xamarin.Forms に組み込まれている Android のプラットフォーム仕様の使い方を説明します。 プラットフォーム仕様は、カスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。
ms.prod: xamarin
ms.assetid: C5D4AA65-9BAA-4008-8A1E-36CDB78A435D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/19/2018
ms.openlocfilehash: 5de5899b01965a33025c8af0c1ae6c09ac60dc9b
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2018
ms.locfileid: "52171288"
---
# <a name="android-platform-specifics"></a>Android プラットフォーム仕様

_この記事では Xamarin.Forms に組み込まれている Android のプラットフォーム仕様の使い方を説明します。プラットフォーム仕様は、カスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。_

## <a name="visualelements"></a>VisualElements

Android では、次のプラットフォーム固有の機能は、Xamarin.Forms のビュー、ページ、およびレイアウトの提供されます。

- 描画の順番を決定するための視覚要素の Z オーダーの制御。 詳細については、[視覚要素の昇格の制御(#elevation)](#elevation)を参照してください。
- サポートされている従来のカラー モードを無効にする[ `VisualElement`](xref:Xamarin.Forms.VisualElement)します。 詳細については、次を参照してください。[レガシ カラー モードを無効にすると](#legacy-color-mode)します。

<a name="elevation" />

### <a name="controlling-the-elevation-of-visual-elements"></a>視覚要素の昇格の制御

このプラットフォーム仕様は、ターゲットが API 21 以上のアプリケーションで視覚要素の昇格または Z オーダーを制御するために使われます。 視覚要素の昇格は、高い Z の値を持つ視覚要素が低い Z の値をもつ視覚要素を塞ぐように、自身の描画順を決定します。 これは XAML で `VisualElement.Elevation` 添付プロパティに`boolean`値を設定して使用します。

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             Title="Elevation">
    <StackLayout>
        <Grid>
            <Button Text="Button Beneath BoxView" />
            <BoxView Color="Red" Opacity="0.2" HeightRequest="50" />
        </Grid>        
        <Grid Margin="0,20,0,0">
            <Button Text="Button Above BoxView - Click Me" android:VisualElement.Elevation="10"/>
            <BoxView Color="Red" Opacity="0.2" HeightRequest="50" />
        </Grid>
    </StackLayout>
</ContentPage>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

public class AndroidElevationPageCS : ContentPage
{
    public AndroidElevationPageCS()
    {
        ...
        var aboveButton = new Button { Text = "Button Above BoxView - Click Me" };
        aboveButton.On<Android>().SetElevation(10);

        Content = new StackLayout
        {
            Children =
            {
                new Grid
                {
                    Children =
                    {
                        new Button { Text = "Button Beneath BoxView" },
                        new BoxView { Color = Color.Red, Opacity = 0.2, HeightRequest = 50 }
                    }
                },
                new Grid
                {
                    Margin = new Thickness(0,20,0,0),
                    Children =
                    {
                        aboveButton,
                        new BoxView { Color = Color.Red, Opacity = 0.2, HeightRequest = 50 }
                    }
                }
            }
        };
    }
}
```

`Button.On<Android>`メソッドは、このプラットフォーム仕様が Android 上でのみ動作することを指定します。 `VisualElement.SetElevation`メソッドは、[`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)名前空間に存在し、視覚要素の昇格に null 許容型の `float` 値を設定するために使われます。 さらに、`VisualElement.GetElevation`メソッドは、視覚要素の昇格値を取得するために使用できます。

その結果、視覚要素の昇格は、高いZ値の視覚要素が低いZ値の視覚要素を塞ぐように制御されます。 それによって、この例では2番目の[`Button`](xref:Xamarin.Forms.Button)は、より高い昇格値を持つため、[`BoxView`](xref:Xamarin.Forms.BoxView)の上に表示されます。

![](android-images/elevation.png)

<a name="legacy-color-mode" />

### <a name="disabling-legacy-color-mode"></a>レガシ カラー モードを無効にします。

Xamarin.Forms のビューのいくつかの機能の色をレガシ モード。 このモードでは、ときに、 [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled)ビューのプロパティに設定されて`false`ビューが既定のネイティブ色無効の状態を使用してユーザー設定の色をオーバーライドします。 旧バージョンと互換性のため、この色のレガシ モードでは、サポートされているビューの既定の動作は残ります。

このプラットフォームに固有では、ビューが無効になっている場合でも、ユーザーがビューの設定の色が維持されるようにこの色のレガシ モードが無効にします。 XAML で設定して使用される、 [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.IsLegacyColorModeEnabledProperty)添付プロパティを`false`:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button Text="Button"
                TextColor="Blue"
                BackgroundColor="Bisque"
                android:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

_legacyColorModeDisabledButton.On<Android>().SetIsLegacyColorModeEnabled(false);
```

`VisualElement.On<Android>`メソッドはこのプラットフォーム仕様が Android 上でのみ動作することを指定します。 [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement},System.Boolean))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)名前空間を使用してレガシのカラー モードが無効になっているかどうか。 さらに、 [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement}))を従来のカラー モードが無効になっているかどうかを返すメソッドを使用できます。

結果は、ことは、従来のカラー モードを無効にできます、ように、ユーザーがビューの設定の色もビューが無効になっています。

![](android-images/legacy-color-mode-disabled.png "従来のカラー モードを無効になっています")

> [!NOTE]
> 設定するときに、 [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup)レガシ カラー モードが完全に無視されます、ビューにします。 表示状態の詳細については、次を参照してください。 [、Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)します。

## <a name="views"></a>Views

Xamarin.Forms のビューは、次のプラットフォーム固有の機能が android では、用意されています。

- 既定のパディングと Android のボタンの影の値を使用します。 詳細については、次を参照してください。 [Android ボタンを使用して](#button-padding-shadow)します。
- 入力方式のソフト キーボードのエディター オプションの設定、 [ `Entry`](xref:Xamarin.Forms.Entry)します。 詳細については、次を参照してください。[設定エントリ入力方式エディター オプション](#entry-imeoptions)します。
- ドロップ シャドウを有効にすると、`ImageButton`します。 詳細については、次を参照してください。 [、ImageButton にドロップ シャドウを有効にする](#imagebutton-drop-shadow)します。
- [`ListView`](xref:Xamarin.Forms.ListView)での高速スクロールの有効化。詳細については、[ListView での高速スクロールの有効化(#fastscroll)](#fastscroll)を参照してください。
- 制御するかどうかを[ `WebView` ](xref:Xamarin.Forms.WebView)混合コンテンツを表示できます。 詳細については、次を参照してください。[混合コンテンツ、WebView 内で有効にする](#webview-mixed-content)します。

<a name="button-padding-shadow" />

### <a name="using-android-buttons"></a>Android のボタンを使用します。

このプラットフォームに固有では、Xamarin.Forms のボタンを使用して、既定のパディングと Android のボタンの影の値かどうかを制御します。 XAML で設定して使用される、 [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPaddingProperty)と[ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadowProperty)添付プロパティを`boolean`値。

```xaml
<ContentPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button ...
                android:Button.UseDefaultPadding="true"
                android:Button.UseDefaultShadow="true" />         
    </StackLayout>
</ContentPage>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

button.On<Android>().SetUseDefaultPadding(true).SetUseDefaultShadow(true);
```

`Button.On<Android>`メソッドはこのプラットフォーム仕様が Android 上でのみ動作することを指定します。 [ `Button.SetUseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean))と[`Button.SetUseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)名前空間には、Xamarin.Forms のボタンが既定値を使用するかどうか制御に使用されますパディングと Android のボタンの影の値。 さらに、 [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button}))と[ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button}))ボタンが既定の値と既定シャドウの値をそれぞれ埋め込みを使用するかどうかを返すメソッドを使用できます。

結果は、既定の埋め込みと Android のボタンの影の値は、Xamarin.Forms のボタンが使用できます。

![](android-images/button-padding-and-shadow.png "従来のカラー モードを無効になっています")

各上のスクリーン ショットで[ `Button` ](xref:Xamarin.Forms.Button)点を除いて同一の定義が含まれて、右側`Button`既定のパディングと Android のボタンの影の値を使用します。

<a name="entry-imeoptions" />

### <a name="setting-entry-input-method-editor-options"></a>設定エントリ入力方式エディターのオプション

このプラットフォーム固有設定入力方式エディター (IME) オプションのソフト キーボードの[ `Entry`](xref:Xamarin.Forms.Entry)します。 これは、ソフト キーボード、およびとの対話の下隅にあるユーザーのアクション ボタンの設定が含まれています、`Entry`します。 これは、 XAML で[`Entry.ImeOptions`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.ImeOptionsProperty)添付プロパティを[`ImeFlags`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags)列挙型の値に設定して使用します。

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Entry ... android:Entry.ImeOptions="Send" />
        ...
    </StackLayout>
</ContentPage>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

entry.On<Android>().SetImeOptions(ImeFlags.Send);
```

`Entry.On<Android>`メソッドはこのプラットフォーム仕様が Android 上でのみ動作することを指定します。 [ `Entry.SetImeOptions` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.SetImeOptions(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Entry},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)のソフト キーボードの入力メソッドのアクションのオプションを設定するため、名前空間、 [ `Entry` ](xref:Xamarin.Forms.Entry)、[ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags)次の値を提供する列挙体。

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Default) – 特定のアクションのキーは必要ありませんし、可能な基になるコントロールが、独自の場合に生成されることを示します。 これになりますか`Next`または`Done`します。
- [`None`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.None) – アクション キーがない使用できることを示します。
- [`Go`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Go) – アクション キーは、"go"操作を実行は、入力テキストのターゲットにしてユーザーを導くことを示します。
- [`Search`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Search) – [検索] 操作を実行するアクション キー、入力したテキストの検索の結果にユーザーを導くことを示します。
- [`Send`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Send) – アクション キーがターゲットにテキストを提供する「送信」操作を実行することを示します。
- [`Next`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Next) – アクション キーがテキストを受け入れる次のフィールドにユーザーを作成、[次へ] の操作を実行することを示します。
- [`Done`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Done) – アクション キーがソフト キーボードを閉じる、"done"操作を実行することを示します。
- [`Previous`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Previous) – アクション キーがテキストを受け取る前のフィールドに、ユーザーがかかっています「前」の操作を実行することを示します。
- [`ImeMaskAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.ImeMaskAction) – アクションのオプションを選択するマスク。
- [`NoPersonalizedLearning`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoPersonalizedLearning) – こと、スペル チェックによっては、ユーザーから学ぶでも、ユーザーが以前入力した内容に基づいて修正の提案を示します。
- [`NoFullscreen`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoFullscreen) – UI が全画面表示にならないことを示します。
- [`NoExtractUi`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoExtractUi) – 抽出されたテキストの UI が表示されませんを示します。
- [`NoAccessoryAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoAccessoryAction) – カスタム アクションの UI が表示しないことを示します。

結果は、指定されたを[ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags)のソフト キーボードを適用する値、 [ `Entry` ](xref:Xamarin.Forms.Entry)、入力方式エディターのオプションを設定します。

[![エントリは、エディターのプラットフォームに固有のメソッドを入力](android-images/entry-imeoptions.png "エントリ エディターのプラットフォームに固有のメソッドの入力")](android-images/entry-imeoptions-large.png#lightbox "エントリ エディターのプラットフォームに固有のメソッドの入力")

<a name="imagebutton-drop-shadow" />

### <a name="enabling-a-drop-shadow-on-a-imagebutton"></a>ドロップ シャドウ、ImageButton で有効にします。

このプラットフォームに固有の使用をドロップ シャドウを有効にする、`ImageButton`します。 XAML で設定して使用される、`ImageButton.IsShadowEnabled`バインド可能なプロパティを`true`、と共にさまざまなドロップ シャドウを制御する追加の省略可能なバインド可能なプロパティ。

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
       <ImageButton ...
                    Source="XamarinLogo.png"
                    BackgroundColor="GhostWhite"
                    android:ImageButton.IsShadowEnabled="true"
                    android:ImageButton.ShadowColor="Gray"
                    android:ImageButton.ShadowRadius="12">
            <android:ImageButton.ShadowOffset>
                <Size>
                    <x:Arguments>
                        <x:Double>10</x:Double>
                        <x:Double>10</x:Double>
                    </x:Arguments>
                </Size>
            </android:ImageButton.ShadowOffset>
        </ImageButton>
        ...
    </StackLayout>
</ContentPage>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

var imageButton = new Xamarin.Forms.ImageButton { Source = "XamarinLogo.png", BackgroundColor = Color.GhostWhite, ... };
imageButton.On<Android>()
           .SetIsShadowEnabled(true)
           .SetShadowColor(Color.Gray)
           .SetShadowOffset(new Size(10, 10))
           .SetShadowRadius(12);
```

> [!IMPORTANT]
> 一部としてドロップ シャドウを描画、`ImageButton`場合にバック グラウンド、および背景が描画されるのみ、`BackgroundColor`プロパティを設定します。 そのため、ドロップ シャドウがいない描画される場合、`ImageButton.BackgroundColor`プロパティが設定されていません。

`ImageButton.On<Android>`メソッドはこのプラットフォーム仕様が Android 上でのみ動作することを指定します。 `ImageButton.SetIsShadowEnabled`メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)にドロップ シャドウが有効になっているかどうか、名前空間を使用して、`ImageButton`します。 さらに、ドロップ シャドウを制御する、次のメソッドを呼び出すことができます。

- `SetShadowColor` – ドロップ シャドウの色を設定します。 既定の色は[ `Color.Default`](xref:Xamarin.Forms.Color.Default*)します。
- `SetShadowOffset` –、影のオフセットを設定します。 影がキャストされますとしてを指定して方向を変更して、オフセット、 [ `Size` ](xref:Xamarin.Forms.Size)値。 `Size`までの距離 (負の値) の左または右 (正の値)、最初の値と上記の距離をされている 2 番目の値 (負の値) または (正の値) の下、構造体の値は、デバイスに依存しない単位で表されます. このプロパティの既定値は (0.0, 0.0) のすべての面にキャスト、影がその結果、`ImageButton`します。
- `SetShadowRadius`– ドロップ シャドウを表示するために使用するぼかしの半径を設定します。 既定の半径の値には 10.0 です。

> [!NOTE]
> ドロップ シャドウの状態は呼び出すことによってクエリを実行できる、 `GetIsShadowEnabled`、 `GetShadowColor`、 `GetShadowOffset`、および`GetShadowRadius`メソッド。

結果は、ドロップ シャドウの有効になっている、 `ImageButton`:

![](android-images/imagebutton-drop-shadow.png "ドロップ シャドウを伴って ImageButton")

<a name="fastscroll" />

### <a name="enabling-fast-scrolling-in-a-listview"></a>ListView での高速スクロールの有効化

このプラットフォーム仕様は[`ListView`](xref:Xamarin.Forms.ListView)のデータ間での高速なスクロールを有効にするために使われます。 これはXAMLで`ListView.IsFastScrollEnabled`添付プロパティに`boolean`値を設定することで使用します。

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        ...
        <ListView ItemsSource="{Binding GroupedEmployees}"
                  GroupDisplayBinding="{Binding Key}"
                  IsGroupingEnabled="true"
                  android:ListView.IsFastScrollEnabled="true">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

var listView = new Xamarin.Forms.ListView { IsGroupingEnabled = true, ... };
listView.SetBinding(ItemsView<Cell>.ItemsSourceProperty, "GroupedEmployees");
listView.GroupDisplayBinding = new Binding("Key");
listView.On<Android>().SetIsFastScrollEnabled(true);
```

`ListView.On<Android>`メソッドはこのプラットフォーム仕様が Android上でのみ動作することを指定します。 `ListView.SetIsFastScrollEnabled`メソッドは、[`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)名前空間に存在し、アプリケーションがバックグラウンドに切り替わった時に[`ListView`](xref:Xamarin.Forms.ListView)ぺージイベントの発生を有効化または無効化するために使用されます。 `SetIsFastScrollEnabled`メソッドは、アプリケーションがバックグラウンドから復帰した時に`IsFastScrollEnabled`ページイベントの発生を有効化または無効化するために使用されます。

```csharp
listView.On<Android>().SetIsFastScrollEnabled(!listView.On<Android>().IsFastScrollEnabled());
```

メソッドは、一時停止時にソフトキーボードの操作方式に[`ListView`](xref:Xamarin.Forms.ListView)が設定されていて、それが表示されている場合に、再開時にそれを表示するかどうかを制御するために使用されます。

[![](android-images/fastscroll.png "ListView FastScroll プラットフォーム固有")](android-images/fastscroll-large.png#lightbox "ListView FastScroll プラットフォームに固有")

<a name="webview-mixed-content" />

### <a name="enabling-mixed-content-in-a-webview"></a>Web ビューで混合コンテンツの有効化

このプラットフォームに固有のコントロールかどうかを[ `WebView` ](xref:Xamarin.Forms.WebView)できます混在した表示アプリケーション内のコンテンツを対象とする API 21 以降。 混合コンテンツは、HTTP 接続経由で (イメージ、オーディオ、ビデオ、スタイル シート、スクリプト) などのリソースを読み込みます HTTPS 接続経由で最初に読み込まれるコンテンツです。 これは、 XAML で[`WebView.MixedContentMode`](x:ref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.MixedContentModeProperty)添付プロパティを[`MixedContentHandling`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling)列挙型の値に設定して使用します。

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView ... android:WebView.MixedContentMode="AlwaysAllow" />
</ContentPage>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>().SetMixedContentMode(MixedContentHandling.AlwaysAllow);
```

`WebView.On<Android>`メソッドはこのプラットフォーム仕様が Android 上でのみ動作することを指定します。 [ `WebView.SetMixedContentMode` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.SetMixedContentMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.WebView},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)名前空間を使用して混合コンテンツを表示できるかどうかを[ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling)3 つの値を提供する列挙体。

- [`AlwaysAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.AlwaysAllow) – ことを示します、 [ `WebView` ](xref:Xamarin.Forms.WebView) HTTP の配信元からコンテンツの読み込みに HTTPS origin できるようになります。
- [`NeverAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.NeverAllow) – ことを示します、 [ `WebView` ](xref:Xamarin.Forms.WebView) HTTPS origin HTTP の配信元からコンテンツを読み込むには許可されません。
- [`CompatibilityMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.CompatibilityMode) – ことを示します、 [ `WebView` ](xref:Xamarin.Forms.WebView)アプローチでは最新のデバイスの web ブラウザーの互換性があることを試みます。 一部の HTTP コンテンツが HTTPS の配信元で読み込むことができるし、他の種類のコンテンツはブロックされます。 ブロックまたは許可するコンテンツの種類は、各オペレーティング システムのリリースで変更できます。

結果はするが、指定された[ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling)値に適用されます、 [ `WebView` ](xref:Xamarin.Forms.WebView)、混合コンテンツを表示できるかどうかを制御します。

[![WebView 混合コンテンツの処理プラットフォーム固有](android-images/webview-mixedcontent.png "WebView 混合コンテンツの処理プラットフォーム固有")](android-images/webview-mixedcontent-large.png#lightbox "WebView 混合プラットフォームに固有のコンテンツの処理")

## <a name="pages"></a>ページ数

Xamarin.Forms のページは、次のプラットフォーム固有の機能が android では、用意されています。

- 上のナビゲーション バーの高さを設定、 [ `NavigationPage`](xref:Xamarin.Forms.NavigationPage)します。 詳細については、次を参照してください。 [NavigationPage のナビゲーション バーの高さを設定](#navigationpage-barheight)します。
- 内のページ間を移動するときに、遷移アニメーションを無効にすると、 [ `TabbedPage`](xref:Xamarin.Forms.TabbedPage)します。 詳細については、次を参照してください。[で、TabbedPage ページ遷移アニメーションを無効にすると](#tabbedpage-transition-animations)します。
- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)でのページ間のスワイプ操作の有効化。 詳細については、[TabbedPage でのページ間のスワイプ操作の有効化](#enable_swipe_paging)を参照してください。
- ツールバーの配置と色を設定、 [ `TabbedPage`](xref:Xamarin.Forms.TabbedPage)します。 詳細については、次を参照してください。 [TabbedPage ツールバーの配置の設定と色](#tabbedpage-toolbar)します。

<a name="navigationpage-barheight" />

### <a name="setting-the-navigation-bar-height-on-a-navigationpage"></a>ナビゲーション バーの設定、NavigationPage の高さ

このプラットフォームに固有のナビゲーション バーの高さを設定する、 [ `NavigationPage`](xref:Xamarin.Forms.NavigationPage)します。 XAML で設定して使用される、 [ `NavigationPage.BarHeight` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.BarHeightProperty)整数値にバインド可能なプロパティ。

```xaml
<NavigationPage ...
                xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;assembly=Xamarin.Forms.Core"
                android:NavigationPage.BarHeight="450">
    ...
</NavigationPage>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;
...

public class AndroidNavigationPageCS : Xamarin.Forms.NavigationPage
{
    public AndroidNavigationPageCS()
    {
        On<Android>().SetBarHeight(450);
    }
}
```

`NavigationPage.On<Android>`メソッドは、このプラットフォームに固有を Android アプリケーションの互換性でのみ実行されるを指定します。 [ `NavigationPage.SetBarHeight` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.SetBarHeight(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.NavigationPage},System.Int32))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)名前空間がのナビゲーション バーの高さを設定するため、 [ `NavigationPage`](xref:Xamarin.Forms.NavigationPage)します。 さらに、 [ `NavigationPage.GetBarHeight` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.GetBarHeight(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.NavigationPage}))メソッドを使用して、ナビゲーション バーの高さが返される、`NavigationPage`します。

その結果、ナビゲーション バーの高さを[ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)設定できます。

![](android-images/navigationpage-barheight.png "NavigationPage のナビゲーション バーの高さ")

<a name="tabbedpage-transition-animations" />

### <a name="disabling-page-transition-animations-in-a-tabbedpage"></a>TabbedPage ページ遷移アニメーションを無効にします。

このプラットフォームに固有の使用の移動時にページを使用するかプログラムで、または、タブ バーを使用する場合は、遷移アニメーションを無効にする、 [ `TabbedPage`](xref:Xamarin.Forms.TabbedPage)します。 XAML で設定して使用される、`TabbedPage.IsSmoothScrollEnabled`バインド可能なプロパティを`false`:

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.IsSmoothScrollEnabled="false">
    ...
</TabbedPage>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetIsSmoothScrollEnabled(false);
```

`TabbedPage.On<Android>`メソッドはこのプラットフォーム仕様が Android 上でのみ動作することを指定します。 `TabbedPage.SetIsSmoothScrollEnabled`メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)でページ間を移動するときに、遷移のアニメーションが表示されるかどうか、名前空間を使用して、 [ `TabbedPage`](xref:Xamarin.Forms.TabbedPage)します。 さらに、`TabbedPage`クラス、`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`名前空間は、次の方法もあります。

- `IsSmoothScrollEnabled`、ページ間を移動するときに、遷移のアニメーションが表示されるかどうかを取得に使用される、`TabbedPage`します。
- `EnableSmoothScroll`、ページ間で移動すると、遷移アニメーションを有効にするに使用される、`TabbedPage`します。
- `DisableSmoothScroll`、でページ間を移動する場合は、遷移アニメーションを無効にするため、`TabbedPage`します。

<a name="enable_swipe_paging" />

### <a name="enabling-swiping-between-pages-in-a-tabbedpage"></a>TabbedPage でのページ間のスワイプ操作の有効化

このプラットフォーム仕様は[`TabbedPage`](xref:Xamarin.Forms.TabbedPage)のぺージ間の水平ジェスチャーによるスワイプ移動を有効にするために使われます。 これは XAML で[`TabbedPage.IsSwipePagingEnabled`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.IsSwipePagingEnabledProperty)添付プロパティに`boolean` 値を設定することで使用されます。

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.OffscreenPageLimit="2"
            android:TabbedPage.IsSwipePagingEnabled="true">
    ...
</TabbedPage>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetOffscreenPageLimit(2)
             .SetIsSwipePagingEnabled(true);
```

`TabbedPage.On<Android>`メソッドはこのプラットフォーム仕様が Android 上でのみ動作することを指定します。 [`TabbedPage.SetIsSwipePagingEnabled`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetIsSwipePagingEnabled(Xamarin.Forms.BindableObject,System.Boolean))メソッドは、[`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)名前空間に存在し、[`TabbedPage`](xref:Xamarin.Forms.TabbedPage)のぺージ間のスワイプ移動を有効にするために使われます。 `TabbedPage`名前空間の`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`クラスには、このプラットフォーム仕様を有効にする[`EnableSwipePaging`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.EnableSwipePaging(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage}))メソッドと無効にする[`DisableSwipePaging`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.DisableSwipePaging(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage}))メソッドもあります。 [`TabbedPage.OffscreenPageLimit`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.OffscreenPageLimitProperty)添付プロパティと[`SetOffscreenPageLimit`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetOffscreenPageLimit(Xamarin.Forms.BindableObject,System.Int32))メソッドは現在のぺージの両側でアイドル状態のままで保持すべきぺージ数を設定するために使用します。

その結果、[`TabbedPage`](xref:Xamarin.Forms.TabbedPage)によって表示されたぺージ間のスワイプ遷移が有効になります。

![](android-images/tabbedpage-swipe.png)

<a name="tabbedpage-toolbar" />

### <a name="setting-tabbedpage-toolbar-placement-and-color"></a>TabbedPage ツールバーの配置と色の設定

これらのプラットフォーム固有の配置と、ツールバーの色を設定に使用する[ `TabbedPage`](xref:Xamarin.Forms.TabbedPage)します。 設定して XAML で使用された、 [ `TabbedPage.ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.ToolbarPlacementProperty)添付プロパティの値を[ `ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement)列挙型、および[ `TabbedPage.BarItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.BarItemColorProperty)と[ `TabbedPage.BarSelectedItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.BarSelectedItemColorProperty)添付プロパティを[ `Color` ](xref:Xamarin.Forms.Color):

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.ToolbarPlacement="Bottom"
            android:TabbedPage.BarItemColor="Black"
            android:TabbedPage.BarSelectedItemColor="Red">
    ...
</TabbedPage>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetToolbarPlacement(ToolbarPlacement.Bottom)
             .SetBarItemColor(Color.Black)
             .SetBarSelectedItemColor(Color.Red);
```

`TabbedPage.On<Android>`メソッドは、Android でのこれらのプラットフォーム固有の実行はのみを指定します。 [ `TabbedPage.SetToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetToolbarPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)でツールバーの配置を設定するため、名前空間、 [ `TabbedPage`](xref:Xamarin.Forms.TabbedPage)で、 [`ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement)列挙体の次の値を指定します。

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Default) –、ページの既定の場所にツールバーを配置することを示します。 これは、デバイスは、ページの上部およびその他のデバイスの表現方法ではページの下部にあります。
- [`Top`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Top) ページの上部にあるツールバーを配置することを示します。
- [`Bottom`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Bottom) ツールバーがページの下部に配置されることを示します。

さらに、 [ `TabbedPage.SetBarItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetBarItemColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage},Xamarin.Forms.Color))と[ `TabbedPage.SetBarSelectedItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetBarSelectedItemColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage},Xamarin.Forms.Color))メソッドを使用してツールバー項目と、選択したツールバー項目の色をそれぞれ設定します。

> [!NOTE]
> [ `GetToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.GetToolbarPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage}))、 [ `GetBarItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.GetBarItemColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage}))、および[ `GetBarSelectedItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.GetBarSelectedItemColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage}))メソッドを使用して、配置との色を取得すること、 [ `TabbedPage`](xref:Xamarin.Forms.TabbedPage)ツールバー。

ツールバーの配置、ツールバーの項目の色および選択したツール バー アイテムの色に設定できることになります、 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage):

![](android-images/tabbedpage-toolbar-placement.png)

## <a name="application"></a>アプリケーション

Xamarin.Forms の android では、次のプラットフォーム固有の機能が提供されている[ `Application` ](xref:Xamarin.Forms.Application)クラス。

- ソフトキーボードの操作モードの設定。 詳細については、[ソフトキーボードの入力モードの設定](#soft_input_mode)を参照してください。
- AppCompat を使用するアプリケーションに対し、一時停止や再開時に [`Disappearing`](xref:Xamarin.Forms.Page.Appearing)や[`Appearing`](xref:Xamarin.Forms.Page.Appearing) といったぺージのライフサイクルのイベントをそれぞれ無効化する。 詳細については、[Disappearing と Appearing のぺージライフサイクルイベントの無効化](#disable_lifecycle_events)」を参照してください。

<a name="soft_input_mode" />

### <a name="setting-the-soft-keyboard-input-mode"></a>ソフトキーボードの入力モードの設定

このプラットフォーム仕様はソフトキーボードの入力エリアのための操作方式を設定するために使われ、Xaml で[`Application.WindowSoftInputModeAdjust`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.WindowSoftInputModeAdjustProperty)添付プロパティを[`WindowSoftInputModeAdjust`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust)列挙型の値に設定して使用します。

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize">
  ...
</Application>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

App.Current.On<Android>().UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize);
```

`Application.On<Android>`メソッドは、このプラットフォーム仕様が Android 上でのみ動作することを指定します。 [`Application.UseWindowSoftInputModeAdjust`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.UseWindowSoftInputModeAdjust(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust))メソッドは、[`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)名前空間に存在し、[`WindowSoftInputModeAdjust`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust)列挙型が提供する2つの値（[`Pan`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Pan) /[`Resize`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize)）を使って、ソフトキーボードの入力エリアの操作方式を設定するために使用します。 `Pan`は[`AdjustPan`](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustPan/)調整オプションを使用しますが、それは入力コントローラがフォーカスを持つ時にウィンドウをリサイズしません。 その代わり、ウィンドウのコンテンツはソフトキーボードによって現在のフォーカスが隠れないように移動します。 `Resize`は[`AdjustResize`](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustResize/)調整オプションを使用し、入力コントロールがフォーカスを持つ時にソフトキーボードのスペースを空けるためにリサイズします。

その結果、入力コントロールがフォーカスを持つ時のソフトキーボードの入力エリアの操作方式を設定することができます。

[![](android-images/pan-resize.png "動作モードのプラットフォームに固有のソフト キーボード")](android-images/pan-resize-large.png#lightbox "ソフト キーボードのモードのプラットフォームに固有の動作")

<a name="disable_lifecycle_events" />

### <a name="disabling-the-disappearing-and-appearing-page-lifecycle-events"></a>Disappearing と Appearing のぺージライフサイクルイベントの無効化

このプラットフォーム仕様は、AppCompat を使ったアプリケーションで、アプリケーションの一時停止と再開の[`Disappearing`](xref:Xamarin.Forms.Page.Appearing)と[`Appearing`](xref:Xamarin.Forms.Page.Appearing)のぺージイベントをそれぞれ無効にするために使用されます。 さらに、これには一時停止時にソフトキーボードの操作方式に[`WindowSoftInputModeAdjust.Resize`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize)が設定されていて、ソフトキーボードが表示されていた場合に、再開時にソフトキーボードを表示するかどうかを制御する機能も含まれます。

> [!NOTE]
> これらのイベントは、そのイベントに依存するアプリケーションの既存の動作を保持するためにデフォルトで有効であることに注意してください。 これらのイベントを無効にするとAppCompatのイベントサイクルを以前のAppCompatのイベントサイクルに合わせます。

このプラットフォーム仕様はXamlで[`Application.SendDisappearingEventOnPause`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPauseProperty)、[`Application.SendAppearingEventOnResume`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResumeProperty)、[`Application.ShouldPreserveKeyboardOnResume`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResumeProperty)添付プロパティに`boolean`値を設定して使用します。

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"             xmlns:androidAppCompat="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize"
             androidAppCompat:Application.SendDisappearingEventOnPause="false"
             androidAppCompat:Application.SendAppearingEventOnResume="false"
             androidAppCompat:Application.ShouldPreserveKeyboardOnResume="true">
  ...
</Application>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;
...

Xamarin.Forms.Application.Current.On<Android>()
     .UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize)
     .SendDisappearingEventOnPause(false)
     .SendAppearingEventOnResume(false)
     .ShouldPreserveKeyboardOnResume(true);
```

`Application.Current.On<Android>`メソッドは、このプラットフォーム仕様が Android 上でのみ動作することを指定します。 [ `Application.SendDisappearingEventOnPause` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPause(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application},System.Boolean))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)名前空間の使用を有効または、起動処理を無効にする、 [ `Disappearing` ](xref:Xamarin.Forms.Page.Appearing)ページ イベントときに、アプリケーションバック グラウンドを入力します。 [ `Application.SendAppearingEventOnResume` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResume(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application},System.Boolean))メソッドの使用を有効または、起動処理を無効にする、 [ `Appearing` ](xref:Xamarin.Forms.Page.Appearing)ページ イベントがバック グラウンドからアプリケーションを再開します。 [ `Application.ShouldPreserveKeyboardOnResume` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResume(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application},System.Boolean))メソッドを使用して制御で一時停止、表示された場合、再開にソフト キーボードが表示されるかどうかは、ソフト キーボードの動作モードに設定されている提供[ `WindowSoftInputModeAdjust.Resize` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize).

その結果、[`Disappearing`](xref:Xamarin.Forms.Page.Appearing)と[`Appearing`](xref:Xamarin.Forms.Page.Appearing)ぺージイベントはそれぞれアプリケーションの一時停止時と再開時に発生しなくなり、アプリケーションの一時停止時にソフトキーボードが表示されていた場合、再開時にもそれが表示されるようになります。

[![](android-images/keyboard-on-resume.png "ライフサイクル イベントのプラットフォーム仕様")](android-images/keyboard-on-resume-large.png#lightbox "ライフサイクル イベントのプラットフォーム仕様")

## <a name="summary"></a>まとめ

プラットフォーム仕様は、カスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では Xamarin.Forms に組み込まれている Android のプラットフォーム仕様の使い方を説明します。

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/creating.md)
- [プラットフォーム仕様 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [AndroidSpecific](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
