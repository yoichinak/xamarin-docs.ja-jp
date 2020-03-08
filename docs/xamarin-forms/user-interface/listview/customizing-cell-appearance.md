---
title: ListView セルの外観をカスタマイズします。
description: この記事では、ListView コントロールの利便性を活用しながら、Xamarin.Forms アプリケーションでのデータを表示するためのオプションについて説明します。
ms.prod: xamarin
ms.assetid: FD45CB91-1A8F-46FB-B432-6BC20492E456
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/12/2019
ms.openlocfilehash: ab54b54c9f2f7d6d7748137ea079439b7c3ddfca
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/07/2020
ms.locfileid: "78917017"
---
# <a name="customizing-listview-cell-appearance"></a>ListView セルの外観をカスタマイズします。

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)

Xamarin [`ListView`](xref:Xamarin.Forms.ListView)クラスは、スクロール可能なリストを表示するために使用されます。この一覧は、`ViewCell` 要素を使用してカスタマイズできます。 `ViewCell` 要素は、テキストとイメージの表示、true/false 状態の指定、およびユーザー入力の受信を行うことができます。

## <a name="built-in-cells"></a>セルの構築
Xamarin. Forms には、多くのアプリケーションで機能する組み込みセルが付属しています。

- [`TextCell`](#textcell)コントロールは、詳細テキストの省略可能な2行目でテキストを表示するために使用されます。
- [`ImageCell`](#imagecell)コントロールは `TextCell`に似ていますが、テキストの左側に画像が含まれています。
- `SwitchCell` コントロールを使用して、オン/オフまたは true/false の状態を表示およびキャプチャします。
- `EntryCell` コントロールは、ユーザーが編集できるテキストデータを表示するために使用されます。

[`SwitchCell`](~/xamarin-forms/user-interface/tableview.md#switchcell)コントロールと[`EntryCell`](~/xamarin-forms/user-interface/tableview.md#entrycell)コントロールは、 [`TableView`](~/xamarin-forms/user-interface/tableview.md)のコンテキストでよく使用されます。

### <a name="textcell"></a>TextCell

[`TextCell`](xref:Xamarin.Forms.TextCell)は、テキストを表示するためのセルです。オプションで、詳細テキストとして2行目を使用できます。 次のスクリーンショットは、iOS と Android の `TextCell` 項目を示しています。

![](customizing-cell-appearance-images/text-cell-default.png "Default TextCell Example")

TextCells は実行時にネイティブコントロールとしてレンダリングされるため、カスタム `ViewCell`と比較してパフォーマンスが非常に優れています。 TextCells はカスタマイズ可能であり、次のプロパティを設定できます。

- 最初の行に表示されるテキストを大きいフォントで &ndash; `Text` ます。
- 最初の行の下に表示されるテキストを小さいフォントで &ndash; `Detail` ます。
- テキストの色を &ndash; `TextColor`。
- 詳細テキストの色を &ndash; `DetailColor`

次のスクリーンショットは、カスタマイズされた色プロパティを持つ `TextCell` 項目を示しています。

![](customizing-cell-appearance-images/text-cell-custom.png "Custom TextCell Example")

### <a name="imagecell"></a>ImageCell

[`ImageCell`](xref:Xamarin.Forms.ImageCell)は、`TextCell`のように、テキストとセカンダリの詳細テキストを表示するために使用できます。また、各プラットフォームのネイティブコントロールを使用して優れたパフォーマンスを提供します。 `ImageCell` は、テキストの左側に画像が表示されるという点で `TextCell` とは異なります。

次のスクリーンショットは、iOS と Android の `ImageCell` 項目を示しています: !["Default ImageCell Example"](customizing-cell-appearance-images/image-cell-default.png "既定の ImageCell の例")

`ImageCell` は、連絡先や映画の一覧など、視覚的な側面を持つデータの一覧を表示する必要がある場合に便利です。 `ImageCell`はカスタマイズ可能であり、次の設定を行うことができます。

- 最初の行に表示されるテキストを大きいフォントで &ndash; `Text` します。
- 最初の行の下に表示されるテキストを、小さいフォントで &ndash; `Detail` します。
- テキストの色を &ndash; `TextColor`
- 詳細テキストの色を &ndash; `DetailColor`
- テキストの横に表示するイメージを `ImageSource` &ndash;

次のスクリーンショットは、カスタマイズされた色プロパティを持つ `ImageCell` 項目を示しています。 "カスタマイズされた![ImageCell の例"](customizing-cell-appearance-images/image-cell-custom.png "カスタマイズされた ImageCell の例")

## <a name="custom-cells"></a>カスタムのセル
カスタムセルを使用すると、組み込みセルでサポートされていないセルレイアウトを作成できます。 たとえば、2 つのラベルを持つ同じ重み付けをセルに表示したい場合があります。 `TextCell` には小さいラベルが1つあるため、`TextCell` は不十分です。 ほとんどのセルのカスタマイズは、(追加ラベル、画像やその他の情報を表示) 読み取り専用データを追加します。

すべてのカスタムセルは[`ViewCell`](xref:Xamarin.Forms.ViewCell)から派生する必要があります。これは、組み込みのすべてのセル型が使用するのと同じ基本クラスです。

Xamarin. Forms は `ListView` コントロールの[キャッシュ動作](~/xamarin-forms/user-interface/listview/performance.md#caching-strategy)を提供します。これにより、一部の種類のカスタムセルのスクロールパフォーマンスが向上します。

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

- カスタムセルは、`ListView.ItemTemplate`内の `DataTemplate`内に入れ子になっています。 これは、組み込みのセルを使用するプロセスと同じです。
- `ViewCell` は、カスタムセルの種類です。 `DataTemplate` 要素の子は、`ViewCell` クラスであるか、またはその派生クラスである必要があります。
- `ViewCell`内では、すべての Xamarin. フォームレイアウトでレイアウトを管理できます。 この例では、レイアウトは `StackLayout`によって管理されます。これにより、背景色をカスタマイズできます。

> [!NOTE]
> バインド可能な `StackLayout` のすべてのプロパティは、カスタムセル内でバインドできます。 ただし、この機能は XAML の例には記載されていません。

### <a name="code"></a>コード

カスタムセルは、コードで作成することもできます。 まず、`ViewCell` から派生するカスタムクラスを作成する必要があります。

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

ページコンストラクターで、ListView の `ItemTemplate` プロパティは、指定された `CustomCell` 型の `DataTemplate` に設定されます。

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

### <a name="binding-context-changes"></a>バインド コンテキストの変更

カスタムセル型の[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)インスタンスにバインドする場合、`BindableProperty` 値を表示する UI コントロールでは、次のコード例に示すように、セルコンストラクターではなく、各セルに表示されるデータを設定するために[`OnBindingContextChanged`](xref:Xamarin.Forms.Cell.OnBindingContextChanged)オーバーライドを使用する必要があります。

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

[`OnBindingContextChanged`](xref:Xamarin.Forms.Cell.OnBindingContextChanged)オーバーライドは、 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)プロパティの値の変更に応じて、 [`BindingContextChanged`](xref:Xamarin.Forms.BindableObject.BindingContextChanged)イベントが発生したときに呼び出されます。 したがって、`BindingContext` が変更された場合、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)値を表示する UI コントロールでデータを設定する必要があります。 `BindingContext` は `null` 値をチェックする必要があることに注意してください。これは、ガベージコレクションのために Xamarin によって設定できます。これにより、`OnBindingContextChanged` のオーバーライドが呼び出されます。

または、UI コントロールを[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)インスタンスにバインドして値を表示することもできます。これにより、`OnBindingContextChanged` メソッドをオーバーライドする必要がなくなります。

> [!NOTE]
> `OnBindingContextChanged`をオーバーライドする場合は、登録されているデリゲートが `BindingContextChanged` イベントを受け取るように、基本クラスの `OnBindingContextChanged` メソッドが呼び出されていることを確認します。

XAML では、カスタムのセルの種類のデータへのバインドを実現する次のコード例に示すように。

```xaml
<ListView x:Name="listView">
    <ListView.ItemTemplate>
        <DataTemplate>
            <local:CustomCell Name="{Binding Name}" Age="{Binding Age}" Location="{Binding Location}" />
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

これにより、`CustomCell` インスタンス内の `Name`、`Age`、および `Location` バインド可能なプロパティが、基になるコレクション内の各オブジェクトの `Name`、`Age`、および `Location` の各プロパティにバインドされます。

C# の等価なバインドは、次のコード例に示されます。

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

IOS と Android で[`ListView`](xref:Xamarin.Forms.ListView)がリサイクル要素であり、カスタムセルでカスタムレンダラーが使用されている場合、カスタムレンダラーはプロパティ変更通知を正しく実装する必要があります。 セルが再利用されると、プロパティ値は、`PropertyChanged` イベントが発生している使用可能なセルのバインドコンテキストに更新されると変更されます。 詳細については、「 [ViewCell のカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)」を参照してください。 セルのリサイクルの詳細については、「[キャッシュ方法](~/xamarin-forms/user-interface/listview/performance.md#caching-strategy)」を参照してください。

## <a name="related-links"></a>関連リンク

- [組み込みセル (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-builtincells)
- [カスタムセル (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)
- [バインドコンテキストの変更 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-bindingcontextchanged)
