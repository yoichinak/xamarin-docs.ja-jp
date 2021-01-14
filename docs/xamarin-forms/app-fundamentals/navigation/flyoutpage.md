---
title: Xamarin.Forms FlyoutPage
description: Xamarin.Forms の FlyoutPage は、2 つの関連する情報ページ、つまり項目を表示するポップアップ ページと、ポップアップ ページ上の項目に関する詳細を表示する詳細ページを管理するページです。 この記事では、FlyoutPage を使用する方法と、その情報ページ間を移動する方法について説明します。
ms.prod: xamarin
ms.assetid: 119945E3-58B8-4630-A3D2-8B561529D53B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ea0be2136b354ef7a613904799481079bcae52ad
ms.sourcegitcommit: 1decf2c65dc4c36513f7dd459a5df01e170a036f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/12/2021
ms.locfileid: "98115237"
---
# <a name="no-locxamarinforms-flyoutpage"></a>Xamarin.Forms FlyoutPage

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/navigation-flyoutpage)

通常、ポップアップ ページには、次のスクリーンショットのように、項目の一覧が表示されます。

[![ポップアップ ページのコンポーネント](flyoutpage-images/flyoutpage-components.png)](flyoutpage-images/flyoutpage-components-large.png#lightbox "ポップアップ ページのコンポーネント")

項目の一覧の場所は各プラットフォームで同じであり、いずれかの項目を選択すると対応する詳細ページに移動します。 さらに、ポップアップ ページのナビゲーション バーに含まれるボタンを使用してアクティブな詳細ページに移動することもできます。

- iOS では、ナビゲーション バーはページの上部にあり、詳細ページに移動するボタンが含まれます。 さらに、マスター ページを左にスワイプすることで、アクティブな詳細ページに移動できます。
- Android では、ナビゲーション バーはページの上部にあり、タイトル、アイコン、詳細ページに移動するボタンが表示されます。 アイコンは、Android プラットフォーム固有プロジェクトにおいて `MainActivity` クラスを修飾する `[Activity]` 属性で定義されています。 さらに、ポップアップ ページを左にスワイプするか、画面右端の詳細ページをタップするか、画面の下部にある *[戻る]* ボタンをタップすることによって、アクティブな詳細ページに移動できます。
- ユニバーサル Windows プラットフォーム (UWP) では、ナビゲーション バーはページの上部にあり、詳細ページに移動するボタンが含まれます。

次のスクリーンショットのように、詳細ページには、ポップアップ ページで選択されている項目に対応するデータと、詳細ページの主要コンポーネントが表示されます。

![詳細ページのコンポーネント](flyoutpage-images/detailpage-components.png)

詳細ページにはナビゲーション バーが含まれ、その内容はプラットフォームによって異なります。

- iOS では、ナビゲーション バーはページの上部に存在し、タイトルが表示されています。また、詳細ページのインスタンスが [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) インスタンスにラップされている場合は、ポップアップ ページに戻るボタンがあります。 さらに、ポップアップ ページには、詳細ページを右にスワイプすることによって戻ることができます。
- Android では、ナビゲーション バーはページの上部にあり、タイトル、アイコン、ポップアップ ページに戻るボタンが表示されます。 Android プラットフォーム固有プロジェクトでは、アイコンは `MainActivity` クラスを修飾する `[Activity]` 属性で定義されています。
- UWP では、ナビゲーション バーはページの上部にあり、タイトルが表示され、ポップアップ ページに戻るボタンがあります。

## <a name="navigation-behavior"></a>ナビゲーションの動作

ポップアップ ページと詳細ページの間のナビゲーション エクスペリエンスの動作は、プラットフォームによって異なります。

- iOS では、ポップアップ ページが左から "*スライド*" してくると、詳細ページは右にスライドしますが、詳細ページの左の部分はまだ表示されています。
- Android では、詳細ページとポップアップ ページは相互に "*オーバーレイ*" されます。
- UWP では、ポップアップ ページが左からスライドして詳細ページの一部を覆います。ただし、[`FlyoutLayoutBehavior`](xref:Xamarin.Forms.FlyoutPage.FlyoutLayoutBehavior) プロパティが `Popover` に設定されている場合です。

横モードでの動作も同様ですが、iOS と Android のポップアップ ページは縦モードのポップアップ ページと同じ幅なので、表示される詳細ページの部分が多くなります。

ナビゲーション動作の制御の詳細については、「[詳細ページのレイアウト動作を制御する](#control-the-detail-page-layout-behavior)」をご覧ください。

## <a name="create-a-flyoutpage"></a>FlyoutPage を作成する

[`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) には [`Flyout`](xref:Xamarin.Forms.FlyoutPage.Flyout) プロパティと [`Detail`](xref:Xamarin.Forms.FlyoutPage.Detail) プロパティ (どちらも [`Page`](xref:Xamarin.Forms.Page) 型) が含まれ、それぞれ、ポップアップ ページと詳細ページの取得と設定に使用されます。

> [!IMPORTANT]
> [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) はルート ページとして設計されており、他のページの種類の子ページとして使用すると、予期されていない一貫性のない動作が発生する可能性があります。 さらに、`FlyoutPage` のポップアップ ページは常に [`ContentPage`](xref:Xamarin.Forms.ContentPage) インスタンスにすること、および詳細ページは [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)、[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)、`ContentPage` のインスタンスのみで設定することをお勧めします。 これは、すべてのプラットフォームで一貫したユーザー エクスペリエンスにするのに役立ちます。

次の XAML コード例では、[`Flyout`](xref:Xamarin.Forms.FlyoutPage.Flyout) プロパティと [`Detail`](xref:Xamarin.Forms.FlyoutPage.Detail) プロパティを設定する [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) を示します。

```xaml
<FlyoutPage xmlns="http://xamarin.com/schemas/2014/forms"
            xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
            xmlns:local="clr-namespace:FlyoutPageNavigation;assembly=FlyoutPageNavigation"
            x:Class="FlyoutPageNavigation.MainPage">
    <FlyoutPage.Flyout>
        <local:FlyoutMenuPage x:Name="flyoutPage" />
    </FlyoutPage.Flyout>
    <FlyoutPage.Detail>
        <NavigationPage>
            <x:Arguments>
                <local:ContactsPage />
            </x:Arguments>
        </NavigationPage>
    </FlyoutPage.Detail>
</FlyoutPage>
```

次のコード例は、同じ [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) を C# で作成します。

```csharp
public class MainPageCS : FlyoutPage
{
    FlyoutMenuPageCS flyoutPage;

    public MainPageCS()
    {
        flyoutPage = new FlyoutMenuPageCS();
        Flyout = flyoutPage;
        Detail = new NavigationPage(new ContactsPageCS());
        ...
    }
    ...
}    
```

[`Flyout`](xref:Xamarin.Forms.FlyoutPage.Flyout) プロパティには、[`ContentPage`](xref:Xamarin.Forms.ContentPage) インスタンスが設定されます。 [`Detail`](xref:Xamarin.Forms.FlyoutPage.Detail) プロパティには、`ContentPage` インスタンスを含む [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) が設定されます。

### <a name="create-the-flyout-page"></a>ポップアップ ページを作成する

次に示す XAML のコード例は、`FlyoutMenuPage` オブジェクトの宣言です。このオブジェクトは、[`Flyout`](xref:Xamarin.Forms.FlyoutPage.Flyout) プロパティを通して参照されます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="using:FlyoutPageNavigation"
             x:Class="FlyoutPageNavigation.FlyoutMenuPage"
             Padding="0,40,0,0"
             IconImageSource="hamburger.png"
             Title="Personal Organiser">
    <StackLayout>
        <ListView x:Name="listView" x:FieldModifier="public">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type local:FlyoutPageItem}">
                    <local:FlyoutPageItem Title="Contacts" IconSource="contacts.png" TargetType="{x:Type local:ContactsPage}" />
                    <local:FlyoutPageItem Title="TodoList" IconSource="todo.png" TargetType="{x:Type local:TodoListPage}" />
                    <local:FlyoutPageItem Title="Reminders" IconSource="reminders.png" TargetType="{x:Type local:ReminderPage}" />
                </x:Array>
            </ListView.ItemsSource>
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <Grid Padding="5,10">
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="30"/>
                                <ColumnDefinition Width="*" />
                            </Grid.ColumnDefinitions>
                            <Image Source="{Binding IconSource}" />
                            <Label Grid.Column="1" Text="{Binding Title}" />
                        </Grid>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

ページは、[`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) プロパティに `FlyoutPageItem` オブジェクトの配列を設定することによって XAML のデータを設定された [`ListView`](xref:Xamarin.Forms.ListView) で構成されます。 各 `FlyoutPageItem` では、`Title`、`IconSource`、および `TargetType` プロパティが定義されています。

各 `FlyoutPageItem` を表示するため、[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) が [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) プロパティに割り当てられます。 `DataTemplate` には、[`Image`](xref:Xamarin.Forms.Image) と [`Label`](xref:Xamarin.Forms.Label) で構成される [`ViewCell`](xref:Xamarin.Forms.ViewCell) が含まれます。 各 `FlyoutPageItem` について、[`Image`](xref:Xamarin.Forms.Image) では `IconSource` プロパティの値が表示され、[`Label`](xref:Xamarin.Forms.Label) では `Title` プロパティの値が表示されます。

ページでは、[`Title`](xref:Xamarin.Forms.Page.Title) プロパティと [`IconImageSource`](xref:Xamarin.Forms.Page.IconImageSource) プロパティが設定されます。 詳細ページにタイトル バーがある場合は、アイコンが詳細ページに表示されます。 iOS では、詳細ページのインスタンスを [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) インスタンスにラップすることによって、これを有効にする必要があります。

> [!NOTE]
> [`Flyout`](xref:Xamarin.Forms.FlyoutPage.Flyout) ページではその [`Title`](xref:Xamarin.Forms.Page.Title) プロパティを設定する必要があります。設定しないと例外が発生します。

次に示すのは、C# で作成された同等のページのコード例です。

```csharp
public class FlyoutMenuPageCS : ContentPage
{
    ListView listView;
    public ListView ListView { get { return listView; } }

    public FlyoutMenuPageCS()
    {
        var flyoutPageItems = new List<FlyoutPageItem>();
        flyoutPageItems.Add(new FlyoutPageItem
        {
            Title = "Contacts",
            IconSource = "contacts.png",
            TargetType = typeof(ContactsPageCS)
        });
        flyoutPageItems.Add(new FlyoutPageItem
        {
            Title = "TodoList",
            IconSource = "todo.png",
            TargetType = typeof(TodoListPageCS)
        });
        flyoutPageItems.Add(new FlyoutPageItem
        {
            Title = "Reminders",
            IconSource = "reminders.png",
            TargetType = typeof(ReminderPageCS)
        });

        listView = new ListView
        {
            ItemsSource = flyoutPageItems,
            ItemTemplate = new DataTemplate(() =>
            {
                var grid = new Grid { Padding = new Thickness(5, 10) };
                grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(30) });
                grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Star });

                var image = new Image();
                image.SetBinding(Image.SourceProperty, "IconSource");
                var label = new Label { VerticalOptions = LayoutOptions.FillAndExpand };
                label.SetBinding(Label.TextProperty, "Title");

                grid.Children.Add(image);
                grid.Children.Add(label, 1, 0);

                return new ViewCell { View = grid };
            }),
            SeparatorVisibility = SeparatorVisibility.None
        };

        IconImageSource = "hamburger.png";
        Title = "Personal Organiser";
        Padding = new Thickness(0, 40, 0, 0);
        Content = new StackLayout
        {
            Children = { listView }
        };
    }
}
```

次のスクリーンショットでは、各プラットフォームでのポップアップ ページを示します。

![マスター ページの例](flyoutpage-images/flyoutpage.png)

### <a name="create-and-display-the-detail-page"></a>詳細ページを作成して表示する

`FlyoutMenuPage` インスタンスに含まれる `ListView` プロパティによって公開されるその [`ListView`](xref:Xamarin.Forms.ListView) インスタンスにより、`MainPage` の [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) インスタンスは [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) イベントを処理するためのイベント ハンドラーを登録できます。 これにより、`MainPage` のインスタンスは、[`Detail`](xref:Xamarin.Forms.FlyoutPage.Detail) プロパティに、選択された `ListView` 項目を表示するページを設定できます。 次に示すのは、イベント ハンドラーのコード例です。

```csharp
public partial class MainPage : FlyoutPage
{
    public MainPage()
    {
        ...
        flyoutPage.listView.ItemSelected += OnItemSelected;
    }

    void OnItemSelected(object sender, SelectedItemChangedEventArgs e)
    {
        var item = e.SelectedItem as FlyoutPageItem;
        if (item != null)
        {
            Detail = new NavigationPage((Page)Activator.CreateInstance(item.TargetType));
            flyoutPage.listView.SelectedItem = null;
            IsPresented = false;
        }
    }
}
```

`OnItemSelected` メソッドでは、次の操作が実行されます。

- [`ListView`](xref:Xamarin.Forms.ListView) のインスタンスから [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) を取得し、それが `null` でない場合は、詳細ページに、`FlyoutPageItem` の `TargetType` プロパティに格納されているページ種類の新しいインスタンスを設定します。 `FlyoutMenuPage` の [`IconImageSource`](xref:Xamarin.Forms.Page.IconImageSource) プロパティを通して参照されているアイコンが iOS の詳細ページに表示されるように、ページの種類を [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) のインスタンス内にラップします。
- 次に `FlyoutMenuPage` が表示されるときにどの `ListView` 項目も選択されていないように、[`ListView`](xref:Xamarin.Forms.ListView) の選択済み項目を `null` に設定します。
- [`FlyoutPage.IsPresented`](xref:Xamarin.Forms.FlyoutPage.IsPresented) プロパティを `false` に設定することにより、ユーザーに詳細ページを表示します。 このプロパティによって、ポップアップまたは詳細ページのいずれを表示するかが制御されます。 ポップアップ ページを表示するには `true` に、詳細ページを表示するには `false` に、設定する必要があります。

次のスクリーンショットでは、`ContactPage` の詳細ページを示します。これは、ポップアップ ページで選択された後に表示されます。

![詳細ページの例](flyoutpage-images/detailpage.png)

### <a name="control-the-detail-page-layout-behavior"></a>詳細ページのレイアウト動作を制御する

[`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) によるポップアップと詳細の各ページの管理方法は、アプリケーションがスマートフォンまたはタブレットのどちらで実行されているか、デバイスの向き、および [`FlyoutLayoutBehavior`](xref:Xamarin.Forms.FlyoutPage.FlyoutLayoutBehavior) プロパティの値によって異なります。 このプロパティでは、詳細ページの表示方法が決まります。 次の値を指定できます。

- `Default` – ページは、プラットフォームの既定値を使用して表示されます。
- `Popover` – 詳細ページによってポップアップ ページがカバーまたは部分的にカバーされます。
- `Split` – ポップアップ ページが左側に、詳細ページが右側に表示されます。
- `SplitOnLandscape` – デバイスが横長の向きの場合は、分割画面が使用されます。
- `SplitOnPortrait` – デバイスが縦長の向きの場合は、分割画面が使用されます。

次の XAML コードの例では、[`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) で [`FlyoutLayoutBehavior`](xref:Xamarin.Forms.FlyoutPage.FlyoutLayoutBehavior) プロパティを設定する方法を示します。

```xaml
<FlyoutPage xmlns="http://xamarin.com/schemas/2014/forms"
            xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
            x:Class="FlyoutPageNavigation.MainPage"
            FlyoutLayoutBehavior="Popover">
  ...
</FlyoutPage>
```

次のコード例は、同じ [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) を C# で作成します。

```csharp
public class MainPageCS : FlyoutPage
{
    FlyoutMenuPageCS flyoutPage;

    public MainPageCS()
    {
        ...
        FlyoutLayoutBehavior = FlyoutLayoutBehavior.Popover;
    }
}
```

> [!IMPORTANT]
> [`FlyoutLayoutBehavior`](xref:Xamarin.Forms.FlyoutPage.FlyoutLayoutBehavior) プロパティの値の影響を受けるのは、タブレットまたはデスクトップで実行されているアプリケーションに限られます。 スマートフォンで実行されているアプリケーションは、常に `Popover` 動作になります。

## <a name="related-links"></a>関連リンク

- [ページの変数](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [FlyoutPage (サンプル)](/samples/xamarin/xamarin-forms-samples/navigation-flyoutpage)
- [FlyoutPage API](xref:Xamarin.Forms.FlyoutPage)