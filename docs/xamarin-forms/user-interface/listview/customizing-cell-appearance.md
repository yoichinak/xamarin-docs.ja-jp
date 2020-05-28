---
title: ''
description: この記事では Xamarin.Forms 、ListView コントロールの利便性を活かしながら、アプリケーションにデータを表示するためのオプションについて説明します。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: cdede547e3ef7cf9f7b6d89751c7476a2ce66d3d
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84129013"
---
# <a name="customizing-listview-cell-appearance"></a>ListView セルの外観のカスタマイズ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)

クラスは、 Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView) 要素を使用してカスタマイズできるスクロール可能なリストを表示するために使用され `ViewCell` ます。 `ViewCell`要素は、テキストとイメージの表示、true/false 状態の指定、およびユーザー入力の受信を行うことができます。

## <a name="built-in-cells"></a>組み込みセル
Xamarin.Formsには、多くのアプリケーションで使用できる組み込みセルが用意されています。

- [`TextCell`](#textcell)コントロールは、詳細テキストの省略可能な2行目でテキストを表示するために使用されます。
- [`ImageCell`](#imagecell)コントロールは s に似 `TextCell` ていますが、テキストの左側に画像が含まれています。
- `SwitchCell`コントロールを使用して、オン/オフまたは true/false の状態の表示とキャプチャを行います。
- `EntryCell`コントロールは、ユーザーが編集できるテキストデータを表示するために使用されます。

[`SwitchCell`](~/xamarin-forms/user-interface/tableview.md#switchcell)コントロールと [`EntryCell`](~/xamarin-forms/user-interface/tableview.md#entrycell) コントロールは、のコンテキストでよく使用され [`TableView`](~/xamarin-forms/user-interface/tableview.md) ます。

### <a name="textcell"></a>TextCell

[`TextCell`](xref:Xamarin.Forms.TextCell)テキストを表示するためのセルです。オプションで、詳細テキストとして2行目を使用できます。 次のスクリーンショットは、 `TextCell` iOS と Android の項目を示しています。

![](customizing-cell-appearance-images/text-cell-default.png "Default TextCell Example")

TextCells は実行時にネイティブコントロールとしてレンダリングされるため、カスタムと比較してパフォーマンスが非常に優れてい `ViewCell` ます。 TextCells はカスタマイズ可能であり、次のプロパティを設定できます。

- `Text`&ndash;1 行目に大きいフォントで表示されるテキスト。
- `Detail`&ndash;最初の行の下に表示されるテキストで、小さいフォントで表示されます。
- `TextColor`&ndash;テキストの色。
- `DetailColor`&ndash;詳細テキストの色

次のスクリーンショットは、カスタマイズされ `TextCell` た色プロパティを持つ項目を示しています。

![](customizing-cell-appearance-images/text-cell-custom.png "Custom TextCell Example")

### <a name="imagecell"></a>ImageCell

[`ImageCell`](xref:Xamarin.Forms.ImageCell)と同様に、 `TextCell` テキストとセカンダリの詳細テキストを表示するために使用できます。また、各プラットフォームのネイティブコントロールを使用して優れたパフォーマンスを提供します。 `ImageCell``TextCell`は、テキストの左側に画像を表示するのとは異なります。

次のスクリーンショットは、 `ImageCell` iOS と Android の項目を示しています: !["Default ImageCell Example"](customizing-cell-appearance-images/image-cell-default.png "既定の ImageCell の例")

`ImageCell`は、連絡先や映画の一覧など、視覚的な側面を持つデータの一覧を表示する必要がある場合に便利です。 `ImageCell`はカスタマイズ可能であり、次の設定を行うことができます。

- `Text`&ndash;最初の行に表示されるテキスト (大きいフォント)
- `Detail`&ndash;最初の行の下に表示されるテキストで、小さいフォントで表示されます。
- `TextColor`&ndash;テキストの色
- `DetailColor`&ndash;詳細テキストの色
- `ImageSource`&ndash;テキストの横に表示する画像

次のスクリーンショットは、カスタマイズされ `ImageCell` た色プロパティを持つ項目を示しています。 "カスタマイズされた![ImageCell の例"](customizing-cell-appearance-images/image-cell-custom.png "カスタマイズされた ImageCell の例")

## <a name="custom-cells"></a>カスタムセル
カスタムセルを使用すると、組み込みセルでサポートされていないセルレイアウトを作成できます。 たとえば、重みが同じ2つのラベルを持つセルを表示する場合があります。 には、 `TextCell` 小さいラベルが1つあるため、十分ではあり `TextCell` ません。 ほとんどのセルのカスタマイズでは、追加のラベル、画像、その他の表示情報など、読み取り専用のデータが追加されます。

すべてのカスタムセルは [`ViewCell`](xref:Xamarin.Forms.ViewCell) 、組み込みのすべてのセル型が使用するのと同じ基本クラスから派生する必要があります。

Xamarin.Formsコントロールに[キャッシュ動作](~/xamarin-forms/user-interface/listview/performance.md#caching-strategy)を提供します `ListView` 。これにより、いくつかの種類のカスタムセルのスクロールパフォーマンスが向上します。

次のスクリーンショットは、カスタムセルの例を示しています。

!["カスタムセルの例"](customizing-cell-appearance-images/custom-cell.png "カスタムセルの例")

### <a name="xaml"></a>XAML
前のスクリーンショットに示されているカスタムセルは、次の XAML を使用して作成できます。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="demoListView.ImageCellPage">
    <ContentPage.Content>
        <ListView  x:Name="listView">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <StackLayout BackgroundColor="#eee"
                        Orientation="Vertical">
                            <StackLayout Orientation="Horizontal">
                                <Image Source="{Binding image}" />
                                <Label Text="{Binding title}"
                                TextColor="#f35e20" />
                                <Label Text="{Binding subtitle}"
                                HorizontalOptions="EndAndExpand"
                                TextColor="#503026" />
                            </StackLayout>
                        </StackLayout>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

XAML は次のように機能します。

- カスタムセルは内に入れ子になってい `DataTemplate` `ListView.ItemTemplate` ます。 これは、組み込みのセルを使用するプロセスと同じです。
- `ViewCell`カスタムセルの型です。 要素の子は、クラスである `DataTemplate` か、またはクラスから派生している必要があり `ViewCell` ます。
- 内では `ViewCell` 、レイアウトによってレイアウトを管理でき Xamarin.Forms ます。 この例では、レイアウトはによって管理されます。これにより、 `StackLayout` 背景色をカスタマイズできます。

> [!NOTE]
> バインド可能なのすべてのプロパティは `StackLayout` 、カスタムセル内でバインドできます。 ただし、この機能は XAML の例には記載されていません。

### <a name="code"></a>コード

カスタムセルは、コードで作成することもできます。 最初に、から派生するカスタムクラスを `ViewCell` 作成する必要があります。

```csharp
public class CustomCell : ViewCell
    {
        public CustomCell()
        {
            //instantiate each of our views
            var image = new Image ();
            StackLayout cellWrapper = new StackLayout ();
            StackLayout horizontalLayout = new StackLayout ();
            Label left = new Label ();
            Label right = new Label ();

            //set bindings
            left.SetBinding (Label.TextProperty, "title");
            right.SetBinding (Label.TextProperty, "subtitle");
            image.SetBinding (Image.SourceProperty, "image");

            //Set properties for desired design
            cellWrapper.BackgroundColor = Color.FromHex ("#eee");
            horizontalLayout.Orientation = StackOrientation.Horizontal;
            right.HorizontalOptions = LayoutOptions.EndAndExpand;
            left.TextColor = Color.FromHex ("#f35e20");
            right.TextColor = Color.FromHex ("503026");

            //add views to the view hierarchy
            horizontalLayout.Children.Add (image);
            horizontalLayout.Children.Add (left);
            horizontalLayout.Children.Add (right);
            cellWrapper.Children.Add (horizontalLayout);
            View = cellWrapper;
        }
    }
```

ページコンストラクターでは、ListView の `ItemTemplate` プロパティは、指定された型を持つに設定され `DataTemplate` `CustomCell` ます。

```csharp
public partial class ImageCellPage : ContentPage
    {
        public ImageCellPage ()
        {
            InitializeComponent ();
            listView.ItemTemplate = new DataTemplate (typeof(CustomCell));
        }
    }
```

### <a name="binding-context-changes"></a>バインドコンテキストの変更

カスタムセル型のインスタンスにバインドする場合 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 、値を表示する UI コントロールは、 `BindableProperty` 次の [`OnBindingContextChanged`](xref:Xamarin.Forms.Cell.OnBindingContextChanged) コード例に示すように、セルコンストラクターではなく、各セルに表示されるデータを設定するために、オーバーライドを使用する必要があります。

```csharp
public class CustomCell : ViewCell
{
    Label nameLabel, ageLabel, locationLabel;

    public static readonly BindableProperty NameProperty =
        BindableProperty.Create ("Name", typeof(string), typeof(CustomCell), "Name");
    public static readonly BindableProperty AgeProperty =
        BindableProperty.Create ("Age", typeof(int), typeof(CustomCell), 0);
    public static readonly BindableProperty LocationProperty =
        BindableProperty.Create ("Location", typeof(string), typeof(CustomCell), "Location");

    public string Name
    {
        get { return(string)GetValue (NameProperty); }
        set { SetValue (NameProperty, value); }
    }

    public int Age
    {
        get { return(int)GetValue (AgeProperty); }
        set { SetValue (AgeProperty, value); }
    }

    public string Location
    {
        get { return(string)GetValue (LocationProperty); }
        set { SetValue (LocationProperty, value); }
    }
    ...

    protected override void OnBindingContextChanged ()
    {
        base.OnBindingContextChanged ();

        if (BindingContext != null)
        {
            nameLabel.Text = Name;
            ageLabel.Text = Age.ToString ();
            locationLabel.Text = Location;
        }
    }
}
```

オーバーライドは、変更された [`OnBindingContextChanged`](xref:Xamarin.Forms.Cell.OnBindingContextChanged) [`BindingContextChanged`](xref:Xamarin.Forms.BindableObject.BindingContextChanged) プロパティの値に応じて、イベントが発生したときに呼び出され [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) ます。 そのため、が変更されたときに、 `BindingContext` 値を表示する UI コントロールで [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) データを設定する必要があります。 を値としてチェックする必要があることに注意して `BindingContext` ください `null` 。これは Xamarin.Forms ガベージコレクションに対して設定でき、その結果、 `OnBindingContextChanged` オーバーライドが呼び出されます。

または、UI コントロールでインスタンスにバインドして、その値を表示することもできます [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これにより、メソッドをオーバーライドする必要がなく `OnBindingContextChanged` なります。

> [!NOTE]
> オーバーライドする場合は、登録されている `OnBindingContextChanged` `OnBindingContextChanged` デリゲートがイベントを受け取るように、基本クラスのメソッドが呼び出されていることを確認し `BindingContextChanged` ます。

XAML では、次のコード例に示すように、カスタムセル型をデータにバインドできます。

```xaml
<ListView x:Name="listView">
    <ListView.ItemTemplate>
        <DataTemplate>
            <local:CustomCell Name="{Binding Name}" Age="{Binding Age}" Location="{Binding Location}" />
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

これにより `Name` 、 `Age` インスタンス内の、、およびのバインド可能なプロパティが、 `Location` `CustomCell` 基に `Name` `Age` `Location` なるコレクション内の各オブジェクトの、、およびの各プロパティにバインドされます。

C# での同等のバインドを次のコード例に示します。

```csharp
var customCell = new DataTemplate (typeof(CustomCell));
customCell.SetBinding (CustomCell.NameProperty, "Name");
customCell.SetBinding (CustomCell.AgeProperty, "Age");
customCell.SetBinding (CustomCell.LocationProperty, "Location");

var listView = new ListView
{
    ItemsSource = people,
    ItemTemplate = customCell
};
```

IOS および Android では、 [`ListView`](xref:Xamarin.Forms.ListView) がリサイクル要素であり、カスタムセルがカスタムレンダラーを使用する場合、カスタムレンダラーはプロパティ変更通知を正しく実装する必要があります。 セルが再利用されると、プロパティ値は、バインドコンテキストが、使用可能なセルのバインドコンテキストに更新されると変更され、 `PropertyChanged` イベントが発生します。 詳細については、「 [ViewCell のカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)」を参照してください。 セルのリサイクルの詳細については、「[キャッシュ方法](~/xamarin-forms/user-interface/listview/performance.md#caching-strategy)」を参照してください。

## <a name="related-links"></a>関連リンク

- [組み込みセル (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-builtincells)
- [カスタムセル (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)
- [バインドコンテキストの変更 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-bindingcontextchanged)
