---
title: Xamarin.Forms のマスター/詳細 ページ
description: Xamarin.Forms MasterDetailPage は、ページ、項目を表示するマスター ページ、およびマスター ページの項目に関する詳細情報を表示する詳細ページ、情報の 2 つの関連するページを管理します。 この記事では、その情報のページ間を移動して、MasterDetailPage を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 119945E3-58B8-4630-A3D2-8B561529D53B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 46fa32fc8203b32378f4a4fbe07cb8c9f8dbb854
ms.sourcegitcommit: 7a89735aed9ddf89c855fd33928915d72da40c2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/19/2018
ms.locfileid: "36209207"
---
# <a name="xamarinforms-master-detail-page"></a>Xamarin.Forms のマスター/詳細 ページ

_Xamarin.Forms MasterDetailPage は、ページ、項目を表示するマスター ページ、およびマスター ページの項目に関する詳細情報を表示する詳細ページ、情報の 2 つの関連するページを管理します。この記事では、その情報のページ間を移動して、MasterDetailPage を使用する方法について説明します。_

## <a name="overview"></a>概要

通常、次のスクリーン ショットに示すように、同様に、マスター ページで、項目のリストが表示されます。

[![](master-detail-page-images/masterpage-components.png "マスター ページ コンポーネント")](master-detail-page-images/masterpage-components-large.png#lightbox "マスター ページのコンポーネント")

項目のリストの場所は各プラットフォームのと同じを選択すると、項目のいずれかを対応する詳細ページに移動します。 さらに、マスター ページでは、アクティブな詳細ページへの移動に使用できるボタンを含むナビゲーション バーを機能も。

- Ios の場合は、ナビゲーション バーが、ページの上部に存在し、詳細ページに移動するボタンがあります。 さらに、アクティブな詳細ページは、マスター ページの左にスワイプしてに移動できます。
- Android では、ナビゲーション バーは、ページの上部に存在し、詳細ページにタイトル、アイコン、および移動するボタンを表示します。 アイコンがで定義されている、`[Activity]`を装飾する属性、 `MainActivity` Android プラットフォームに固有のプロジェクト内のクラスです。 さらに、アクティブな詳細ページに移動する、マスター ページを左にスワイプして、画面の右端にある詳細ページをタップして、タップして、*戻る*画面の下部にあるボタンをクリックします。
- ユニバーサル Windows プラットフォーム (UWP) のナビゲーション バーが、ページの上部に存在し、詳細ページに移動するボタンがあります。

ページで、マスターで選択した項目に対応する詳細ページ表示データと詳細ページの主要なコンポーネントは次のスクリーン ショットに表示されます。

![](master-detail-page-images/detailpage-components.png "詳細ページのコンポーネント")

詳細ページには、その内容はプラットフォームに依存する、ナビゲーション バーが含まれています。

- Ios の場合、ナビゲーション バーが存在、ページの上部にあると、タイトルを表示し、マスター ページに戻るボタンに詳細ページのインスタンスがラップされている、 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)インスタンス。 さらに、マスター ページは、右側に [詳細] ページをスワイプしてに返されますことができます。
- Android では、ナビゲーション バーは、ページの上部に存在し、タイトル、アイコン、およびマスター ページに戻るボタンを表示します。 アイコンがで定義されている、`[Activity]`を装飾する属性、 `MainActivity` Android プラットフォームに固有のプロジェクト内のクラスです。
- UWP、ナビゲーション バー、ページの上部にあると、タイトルを表示し、マスター ページに戻るボタンがあります。

### <a name="navigation-behavior"></a>ナビゲーションの動作

マスター/詳細のページ間のナビゲーションの操作の動作は、プラットフォームに依存します。

- Ios の場合、詳細ページ*スライド*マスター ページ スライドとして右、左と詳細の左側の部分からページが表示されます。
- 詳細およびマスター ページは、android で*オーバーレイ*互いに関連します。
- 詳細およびマスター ページは、UWP で*スワップ*です。

IOS および Android 上のマスター ページ幅がないのような縦向きモードのマスター ページとしてため詳細ページの詳細が表示されますが、同様の動作が横モードで実行されます。

ナビゲーション動作を制御する方法の詳細については、次を参照してください。[詳細ページの表示動作を制御する](#Controlling_the_Detail_Page_Display_Behavior)です。

## <a name="creating-a-masterdetailpage"></a>MasterDetailPage を作成します。

A [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)含む[ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/)と[ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/)型の両方であるプロパティ[ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)を取得し、マスター/詳細ページをそれぞれ設定に使用されます。

> [!IMPORTANT]
> A [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)ルート ページに設計されていて、予期しないと一貫性のない動作が発生する他のページの種類の子ページを使用しています。 さらに、ことをお勧めするのマスター ページ、 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)は常に、 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)インスタンス、および詳細ページの代入のみか[ `TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)、 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)、および`ContentPage`インスタンス。 これは、すべてのプラットフォームで一貫性のあるユーザー エクスペリエンスを確保するのに役立ちます。

XAML コード例を次に、 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)が設定された、 [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/)と[ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/)プロパティ。

```xaml
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
                  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                  xmlns:local="clr-namespace:MasterDetailPageNavigation;assembly=MasterDetailPageNavigation"
                  x:Class="MasterDetailPageNavigation.MainPage">
    <MasterDetailPage.Master>
        <local:MasterPage x:Name="masterPage" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <NavigationPage>
            <x:Arguments>
                <local:ContactsPage />
            </x:Arguments>
        </NavigationPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>
```

次のコード例は、該当するショートカットを示しています。 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) c# で作成します。

```csharp
public class MainPageCS : MasterDetailPage
{
    MasterPageCS masterPage;

    public MainPageCS ()
    {
        masterPage = new MasterPageCS ();
        Master = masterPage;
        Detail = new NavigationPage (new ContactsPageCS ());
        ...
    }
    ...
}
```

[ `MasterDetailPage.Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/)プロパティに設定されている、 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)インスタンス。 [ `MasterDetailPage.Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/)プロパティに設定されている、 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)を含む、`ContentPage`インスタンス。

### <a name="creating-the-master-page"></a>マスター ページの作成

XAML コードの例を次の宣言を示しています、`MasterPage`を通じて参照されているオブジェクト、 [ `MasterDetailPage.Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/)プロパティ。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="using:MasterDetailPageNavigation"
             x:Class="MasterDetailPageNavigation.MasterPage"
             Padding="0,40,0,0"
             Icon="hamburger.png"
             Title="Personal Organiser">
    <StackLayout>
        <ListView x:Name="listView" x:FieldModifier="public">
           <ListView.ItemsSource>
                <x:Array Type="{x:Type local:MasterPageItem}">
                    <local:MasterPageItem Title="Contacts" IconSource="contacts.png" TargetType="{x:Type local:ContactsPage}" />
                    <local:MasterPageItem Title="TodoList" IconSource="todo.png" TargetType="{x:Type local:TodoListPage}" />
                    <local:MasterPageItem Title="Reminders" IconSource="reminders.png" TargetType="{x:Type local:ReminderPage}" />
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

ページから成る、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)するデータが入力された XAML で設定してその[ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/)プロパティの配列を`MasterPageItem`インスタンス。 各`MasterPageItem`定義`Title`、 `IconSource`、および`TargetType`プロパティです。

A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)に割り当てられている、 [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) 、表示するプロパティをそれぞれ`MasterPageItem`です。 `DataTemplate`が含まれています、 [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)で構成される、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)と[ `Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)です。 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)が表示されます、`IconSource`プロパティの値、および[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)が表示されます、`Title`ごとのプロパティ値`MasterPageItem`です。

このページはその[ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Title/)と[ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/)プロパティを設定します。 アイコンは、詳細ページがタイトル バーを持っていれば、詳細ページに表示されます。 これが可能で iOS で、詳細ページでインスタンスをラップすることによって、 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)インスタンス。

> [!NOTE]
> [ `MasterDetailPage.Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/)ページが必要、 [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Title/)プロパティ、または、例外が発生します。

次のコード例は、c# で作成された同等のページを示しています。

```csharp
public class MasterPageCS : ContentPage
{
  public ListView ListView { get { return listView; } }

  ListView listView;

  public MasterPageCS ()
  {
    var masterPageItems = new List<MasterPageItem> ();
    masterPageItems.Add (new MasterPageItem {
      Title = "Contacts",
      IconSource = "contacts.png",
      TargetType = typeof(ContactsPageCS)
    });
    masterPageItems.Add (new MasterPageItem {
      Title = "TodoList",
      IconSource = "todo.png",
      TargetType = typeof(TodoListPageCS)
    });
    masterPageItems.Add (new MasterPageItem {
      Title = "Reminders",
      IconSource = "reminders.png",
      TargetType = typeof(ReminderPageCS)
    });

    listView = new ListView {
      ItemsSource = masterPageItems,
      ItemTemplate = new DataTemplate (() => {
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

    Icon = "hamburger.png";
    Title = "Personal Organiser";
    Content = new StackLayout
    {
      Children = { listView }
    };
  }
}
```

次のスクリーン ショットは、各プラットフォームで、マスター ページを表示します。

![](master-detail-page-images/masterpage.png "マスター ページの例")

### <a name="creating-and-displaying-the-detail-page"></a>作成して、詳細ページを表示します。

`MasterPage`インスタンスに含まれる、`ListView`プロパティを公開する、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)インスタンスできるように、 `MainPage` [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)インスタンスを登録できます、処理するイベント ハンドラー、 [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/)イベント。 これにより、`MainPage`を設定するインスタンス、 [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/)プロパティを表す、選択したページを`ListView`項目。 次のコード例では、イベント ハンドラーを示します。

```csharp
public partial class MainPage : MasterDetailPage
{
    public MainPage ()
    {
        ...
        masterPage.listView.ItemSelected += OnItemSelected;
    }

    void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
    {
        var item = e.SelectedItem as MasterPageItem;
        if (item != null) {
            Detail = new NavigationPage ((Page)Activator.CreateInstance (item.TargetType));
            masterPage.listView.SelectedItem = null;
            IsPresented = false;
        }
    }
}
```

`OnItemSelected`メソッドは、次の操作を実行します。

- 取得、 [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/)から、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)インスタンス、および提供されていないこと`null`、詳細ページを設定、に格納されているページの種類の新しいインスタンスを`TargetType`のプロパティ、`MasterPageItem`です。 ページの種類にラップされて、 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)アイコンから参照されていることを確認するインスタンス、 [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/)プロパティを`MasterPage`ios 詳細ページに表示されます。
- 内の選択項目、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)に設定されている`null`ことを確認するのいずれも、`ListView`項目が次の時間を選択する、`MasterPage`が表示されます。
- 詳細ページが設定して、ユーザーに表示される、 [ `MasterDetailPage.IsPresented` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsPresented/)プロパティを`false`です。 このプロパティは、マスターまたは詳細ページが表示されるかどうかを制御します。 設定する必要があります`true`、マスター ページを表示および`false`詳細ページを表示します。

次のスクリーン ショットに示さ、`ContactPage`詳細 ページで、マスター ページで選択したが示されています。

![](master-detail-page-images/detailpage.png "詳細ページの例")

<a name="Controlling_the_Detail_Page_Display_Behavior" />

### <a name="controlling-the-detail-page-display-behavior"></a>詳細ページの表示動作を制御します。

どのように[ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)マスター/詳細ページを管理する、アプリケーションが、電話やタブレット、デバイスの向きの値で実行されているかどうかによって異なります、 [ `MasterBehavior` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.MasterBehavior/)プロパティ。 このプロパティは、詳細ページの表示方法を決定します。 使用可能な値は次のとおりです。

- **既定**– プラットフォームの既定値を使用して、ページが表示されます。
- **重なって**– 詳細ページをカバーしているか、マスター ページが部分的にカバーします。
- **分割**: マスター ページが左に表示し、詳細ページは右端に表示します。
- **SplitOnLandscape** – 分割画面を使用すると、デバイスが横向き場合。
- **SplitOnPortrait** –、[分割] 画面を使用して、デバイスが縦向きにします。

次の XAML コードの例は、設定する方法を示します、 [ `MasterBehavior` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.MasterBehavior/)プロパティを[ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
                  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                  x:Class="MasterDetailPageNavigation.MainPage"
                  MasterBehavior="Popover">
  ...
</MasterDetailPage>
```

次のコード例は、該当するショートカットを示しています。 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) c# で作成します。

```csharp
public class MainPageCS : MasterDetailPage
{
    MasterPageCS masterPage;

    public MainPageCS ()
    {
        MasterBehavior = MasterBehavior.Popover;
        ...
    }
}
```

ただしの値、 [ `MasterBehavior` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.MasterBehavior/)プロパティでは、タブレットやデスクトップで実行されるアプリケーションのみに影響します。 携帯電話で常に実行されるアプリケーションが、*重なって*動作します。

## <a name="summary"></a>まとめ

この記事には、使用する方法が示されている、 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)し、情報のページ間を移動します。 Xamarin.Forms`MasterDetailPage`項目を表示するマスター ページ、およびマスター ページの項目に関する詳細情報を表示する詳細ページ 2 つのページの関連情報を管理するページです。


## <a name="related-links"></a>関連リンク

- [ページの種類](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [MasterDetailPage (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/MasterDetailPage/)
- [MasterDetailPage](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)
