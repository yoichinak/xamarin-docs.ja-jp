---
title: Xamarin. Forms ContentView
description: この記事では、ContentView クラスを使用して、例 CardView などのカスタムコントロールを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: 638402E7-CA44-456B-863B-791F6B6B561D
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 08/14/2019
ms.openlocfilehash: e340b45148c7528eff1aa511ee9902a4ac2658c0
ms.sourcegitcommit: 9178e2e689f027212ea3e623b556b312985d79fe
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/21/2019
ms.locfileid: "69658159"
---
# <a name="xamarinforms-contentview"></a>Xamarin. Forms ContentView

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/en-us/samples/xamarin/xamarin-forms-samples/userinterface-cardview/)

Xamarin [`ContentView`](xref:Xamarin.Forms.ContentView)クラスは、単一の`Layout`子要素を格納する型であり、通常は、カスタムの再利用可能なコントロールを作成するために使用されます。 クラス`ContentView`は、から[`TemplatedView`](xref:Xamarin.Forms.TemplatedView)継承されます。 この記事および関連するサンプルでは、 `CardView` `ContentView`クラスに基づいてカスタムコントロールを作成する方法について説明します。

次のスクリーンショットは`CardView` 、 `ContentView`クラスから派生したコントロールを示しています。

[![CardView サンプルアプリケーションのスクリーンショット](contentview-images/cardview-list-cropped.png)](contentview-images/cardview-list.png#lightbox)

クラス`ContentView`は、1つのプロパティを定義します。

* [`Content`](xref:Xamarin.Forms.ContentView.Content)`View`はオブジェクトです。 このプロパティは、データバインディング[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)のターゲットとして使用できるように、オブジェクトによってサポートされます。

また`ContentView` 、は、 `TemplatedView`クラスからプロパティを継承します。

* [`ControlTemplate`](xref:Xamarin.Forms.TemplatedView.ControlTemplate)は、 `ControlTemplate`コントロールの外観を定義またはオーバーライドできるです。

プロパティの`ControlTemplate`詳細については、「 [ControlTemplate を使用して外観をカスタマイズする](#customize-appearance-with-a-controltemplate)」を参照してください。

## <a name="create-a-custom-control"></a>カスタムコントロールを作成する

クラス`ContentView`は、単独ではほとんど機能を提供しませんが、カスタムコントロールを作成するために使用できます。 サンプルプロジェクトでは、 `CardView`画像、タイトル、および説明をカードのようなレイアウトで表示する UI 要素を定義します。

カスタムコントロールを作成するには、次の手順を実行します。

1. Visual Studio 2019 でテンプレートを`ContentView`使用して、新しいクラスを作成します。
1. 新しいカスタムコントロールの分離コードファイルで、一意のプロパティまたはイベントを定義します。
1. カスタムコントロールの UI を作成します。

> [!NOTE]
> XAML ではなくコードで定義されたレイアウトを持つカスタムコントロールを作成することができます。 わかりやすくするために、サンプルアプリケーションでは`CardView` 、XAML レイアウトを持つ1つのクラスのみを定義します。 ただし、サンプルアプリケーションには、コード内でカスタムコントロールを使用するプロセスを示す**Cardviewcodepage**クラスが含まれています。

### <a name="create-code-behind-properties"></a>分離コードプロパティの作成

カスタム`CardView`コントロールは、次のプロパティを定義します。

* `CardTitle`: カードに表示されるタイトルを表すオブジェクト。`string`
* `CardDescription`: カードに表示される説明を表すオブジェクト。`string`
* `IconImageSource`: カードに表示されるイメージを表すオブジェクト。`ImageSource`
* `IconBackgroundColor`: カードに表示されるイメージの背景色を表すオブジェクト。`Color`
* `BorderColor`: カードの境界線、画像の境界線、および区分線の色を表すオブジェクト。`Color`
* `CardColor`: カードの背景色を表すオブジェクト。`Color`

> [!NOTE]
> プロパティ`BorderColor`は、デモンストレーションの目的で複数の項目に影響します。 このプロパティは、必要に応じて3つのプロパティに分けることができます。

各プロパティは、 `BindableProperty`インスタンスによってサポートされます。 バッキング`BindableProperty`では、MVVM パターンを使用して、各プロパティのスタイル設定とバインドを行うことができます。 詳細については、「 [MVVM を使用したデータのバインド](#bind-data-with-mvvm)」を参照してください。

次の例は、バッキング`BindableProperty`を作成する方法を示しています。

```csharp
public static readonly BindableProperty CardTitleProperty = BindableProperty.Create(
    "CardTitle",        // the name of the bindable property
    typeof(string),     // the bindable property type
    typeof(CardView),   // the parent object type
    string.Empty);      // the default value for the property
```

カスタムプロパティは、メソッド`GetValue`と`SetValue`メソッドを使用して、 `BindableProperty`オブジェクトの値を取得および設定します。

```csharp
public string CardTitle
{
    get => (string)GetValue(CardView.CardTitleProperty);
    set => SetValue(CardView.CardTitleProperty, value);
}
```

`BindableProperty`オブジェクトの詳細については、「バインド可能な[プロパティ](~/xamarin-forms/xaml/bindable-properties.md)」を参照してください。

### <a name="define-ui"></a>UI の定義

カスタムコントロール UI では、 `ContentView` `CardView`コントロールのルート要素としてを使用します。 XAML の`CardView`例を次に示します。

```XAML
<ContentView ...
             x:Name="this"
             x:Class="CardViewDemo.Controls.CardView">
    <Frame BindingContext="{x:Reference this}"
            BackgroundColor="{Binding CardColor}"
            BorderColor="{Binding BorderColor}"
            ...>
        <Grid>
            ...
            <Frame BorderColor="{Binding BorderColor}"
                   BackgroundColor="{Binding IconBackgroundColor}"
                   ...>
                <Image Source="{Binding IconImageSource}"
                       .. />
            </Frame>
            <Label Text="{Binding CardTitle}"
                   ... />
            <BoxView BackgroundColor="{Binding BorderColor}"
                     ... />
            <Label Text="{Binding CardDescription}"
                   ... />
        </Grid>
    </Frame>
</ContentView>
```

要素`ContentView`は`x:Name` 、プロパティをこのに設定します。**これ**を使用して、 `CardView`インスタンスにバインドされたオブジェクトにアクセスできます。 プロパティのバインドセットの要素は、バインドされたオブジェクトで定義されている値に設定されます。

データ バインディングの詳細については、「[Xamarin.Forms Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md)」 (Xamarin.Forms のデータ バインディング) を参照してください。

## <a name="instantiate-a-custom-control"></a>カスタムコントロールのインスタンス化

カスタムコントロールの名前空間への参照は、カスタムコントロールをインスタンス化するページに追加する必要があります。 次の例は、XAML の`ContentPage`インスタンスに追加された**コントロール**と呼ばれる名前空間参照を示しています。

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CardViewDemo.Controls" >
```

参照が追加されると、 `CardView`は XAML でインスタンス化でき、そのプロパティは次のように定義されます。

```xaml
<controls:CardView BorderColor="DarkGray"
                   CardTitle="Slavko Vlasic"
                   CardDescription="Lorem ipsum dolor sit..."
                   IconBackgroundColor="SlateGray"
                   IconImageSource="user.png"/>
```

は`CardView` 、コードでインスタンス化することもできます。

```csharp
CardView card = new CardView
{
    BorderColor = Color.DarkGray,
    CardTitle = "Slavko Vlasic",
    CardDescription = "Lorem ipsum dolor sit...",
    IconBackgroundColor = Color.SlateGray,
    IconImageSource = ImageSource.FromFile("user.png")
};
```

### <a name="bind-data-with-mvvm"></a>MVVM を使用してデータをバインドする

`CardView`クラス`BindableProperty`のオブジェクトは、モデルビュービューモデル (MVVM) スタイルのバインドを許可します。 サンプルアプリケーションには、 `PersonCollectionViewModel`単一のコレクションプロパティを定義するクラスが含まれています。

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{
    ...
    public List<PersonViewModel> Items
    {
        get
        {
            return items;
        }
        set
        {
            items = value;
            NotifyPropertyChanged();
        }
    }
    ...
}
```

クラス`PersonViewModel`は、個人プロファイルを表します。

```csharp
public class PersonViewModel : INotifyPropertyChanged
{
    ...

    string photo;
    public string Photo
    {
        get
        {
            return photo;
        }
        set
        {
            photo = value;
            NotifyPropertyChanged();
        }
    }

    string name;
    public string Name
    {
        get
        {
            return name;
        }
        set
        {
            name = value;
            NotifyPropertyChanged();
        }
    }

    string bio;
    public string Bio
    {
        get
        {
            return bio;
        }
        set
        {
            bio = value;
            NotifyPropertyChanged();
        }
    }
    ...
}
```

は`CardView` 、オブジェクトの`PersonViewModel`コレクションをカードの一覧として表示するために使用できます。 次の例は、 `PersonViewCollection`インスタンスを XAML の`StackLayout`インスタンスにバインドする方法を示しています。

```xaml
<StackLayout HorizontalOptions="Fill"
             VerticalOptions="Fill"
             BindableLayout.ItemsSource="{Binding Items}">
    <BindableLayout.ItemTemplate>
        <DataTemplate>
            <controls:CardView Margin="4"
                               BorderColor="DarkGray"
                               IconBackgroundColor="SlateGray"
                               BindingContext="{Binding .}"
                               CardTitle="{Binding Name}"
                               CardDescription="{Binding Bio}"
                               IconImageSource="{Binding Photo}"/>
        </DataTemplate>
    </BindableLayout.ItemTemplate>
</StackLayout>
```

`PersonViewCollection`インスタンスの`StackLayout`プロパティは、バインド可能なレイアウトを使用してにバインドされます。 `Items` は`DataTemplate` 、各`CardView`オブジェクトの外観と、のプロパティに`PersonViewModel`バインドするデータを定義します。 を設定`PersonView` `Items`すると、コレクション内のオブジェクトごとにオブジェクトが作成されます。`CardView` `BindingContext` は`BindingContext` 、次の例に示すように設定されます。

```csharp
public partial class CardViewMvvmPage : ContentPage
{
    public CardViewMvvmPage()
    {
        InitializeComponent();
        BindingContext = DataService.GetPersonCollection();
    }
}
```

データバインディングの詳細については、「 [Xamarin. Forms Data binding](~/xamarin-forms/app-fundamentals/data-binding/index.md)」を参照してください。 `BindableProperty`オブジェクトの詳細については、「バインド可能な[プロパティ](~/xamarin-forms/xaml/bindable-properties.md)」を参照してください。

## <a name="customize-appearance-with-a-controltemplate"></a>ControlTemplate を使用して外観をカスタマイズする

`ContentView`クラスから派生したカスタムコントロールは、XAML やコードを使用して外観を定義したり、外観をまったく定義したりすることはできません。 外観がどのように定義され`ControlTemplate`ているかに関係なく、オブジェクトはカスタムレイアウトで外観をオーバーライドできます。

レイアウト`CardView`によっては、一部のユースケースに対して多くの領域が占有される場合があります。 で`ControlTemplate`は、レイアウト`CardView`をオーバーライドして、よりコンパクトなビューを提供できます。これは、要約されたリストに適しています。

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <ControlTemplate x:Key="CardViewCompressed">
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="100" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="100" />
                    <ColumnDefinition Width="100*" />
                </Grid.ColumnDefinitions>
                <Image Grid.Row="0"
                       Grid.Column="0"
                       Source="{TemplateBinding IconImageSource}"
                       BackgroundColor="{TemplateBinding IconBackgroundColor}"
                       WidthRequest="100"
                       HeightRequest="100"
                       Aspect="AspectFill"
                       HorizontalOptions="Center"
                       VerticalOptions="Center"/>
                <StackLayout Grid.Row="0"
                             Grid.Column="1">
                    <Label Text="{TemplateBinding CardTitle}"
                           FontAttributes="Bold" />
                    <Label Text="{TemplateBinding CardDescription}" />
                </StackLayout>
            </Grid>
        </ControlTemplate>
    </ResourceDictionary>
</ContentPage.Resources>
```

のデータバインディングで`ControlTemplate`は、 `TemplateBinding`マークアップ拡張機能を使用してバインディングを指定します。 その`ControlTemplate`後、プロパティの`x:Key`値を使用して、定義された ControlTemplate オブジェクトにプロパティを設定できます。 次の例は、 `ControlTemplate` `CardView`インスタンスに設定されたプロパティを示しています。

```xaml
<controls:CardView ControlTemplate="{StaticResource CardViewCompressed}"/>
```

次のスクリーンショットは、 `CardView` `ControlTemplate`がオーバーライド`CardView`された標準のインスタンスを示しています。

[![CardView ControlTemplate スクリーンショット](contentview-images/cardview-controltemplates-cropped.png)](contentview-images/cardview-controltemplates.png#lightbox)

コントロールテンプレートの詳細については、「 [Xamarin. フォームコントロールテンプレート](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md)」を参照してください。

## <a name="related-links"></a>関連リンク

* [CardView サンプルアプリケーション](https://docs.microsoft.com/en-us/samples/xamarin/xamarin-forms-samples/userinterface-cardview/)
* [Xamarin. フォームデータバインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)
* [バインド](~/xamarin-forms/xaml/bindable-properties.md)可能なプロパティ。
* [Xamarin. フォームコントロールテンプレート](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md)
