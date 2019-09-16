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
ms.sourcegitcommit: a5ef4497db04dfa016865bc7454b3de6ff088554
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/15/2019
ms.locfileid: "70998112"
---
# <a name="customizing-listview-cell-appearance"></a>ListView セルの外観をカスタマイズします。

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)

Xamarin [`ListView`](xref:Xamarin.Forms.ListView)クラスは、スクロール可能なリストを表示するために使用されます。これは`ViewCell` 、要素を使用してカスタマイズできます。 要素`ViewCell`は、テキストとイメージの表示、true/false 状態の指定、およびユーザー入力の受信を行うことができます。

## <a name="built-in-cells"></a>セルの構築
Xamarin. Forms には、多くのアプリケーションで機能する組み込みセルが付属しています。

- [`TextCell`](#textcell)コントロールは、詳細テキストの省略可能な2行目でテキストを表示するために使用されます。
- [`ImageCell`](#imagecell)コントロールは s に`TextCell`似ていますが、テキストの左側に画像が含まれています。
- `SwitchCell`コントロールを使用して、オン/オフまたは true/false の状態の表示とキャプチャを行います。
- `EntryCell`コントロールは、ユーザーが編集できるテキストデータを表示するために使用されます。

コントロール[`SwitchCell`](~/xamarin-forms/user-interface/tableview.md#switchcell) [`TableView`](~/xamarin-forms/user-interface/tableview.md)と[`EntryCell`](~/xamarin-forms/user-interface/tableview.md#entrycell)コントロールは、のコンテキストでよく使用されます。

### <a name="textcell"></a>TextCell

[`TextCell`](xref:Xamarin.Forms.TextCell) 詳細なテキストとして 2 番目の行で必要に応じて、テキストを表示するためのセルです。 次のスクリーンショット`TextCell`は、iOS と Android の項目を示しています。

![](customizing-cell-appearance-images/text-cell-default.png "TextCell の既定の例")

パフォーマンスは、カスタムと比べると非常に良好なので、実行時にネイティブ コントロールとして TextCells がレンダリングされます`ViewCell`します。 TextCells はカスタマイズ可能であり、次のプロパティを設定できます。

- `Text` &ndash; 大きいフォントで、最初の行に表示されるテキスト。
- `Detail` &ndash; 小さいフォントで、最初の行の下に表示されるテキスト。
- `TextColor` &ndash; テキストの色。
- `DetailColor` &ndash; 詳細なテキストの色

次のスクリーンショット`TextCell`は、カスタマイズされた色プロパティを持つ項目を示しています。

![](customizing-cell-appearance-images/text-cell-custom.png "カスタムの TextCell の例")

### <a name="imagecell"></a>ImageCell

[`ImageCell`](xref:Xamarin.Forms.ImageCell)、のような`TextCell`、テキストおよびセカンダリの詳細なテキストを表示するために使用でき、各プラットフォームのネイティブ コントロールを使用して優れたパフォーマンスを提供します。 `ImageCell` 異なる`TextCell`イメージ、テキストの左側に表示します。

次のスクリーンショット`ImageCell`は、iOS と Android の項目を示しています。!["Default ImageCell の例"](customizing-cell-appearance-images/image-cell-default.png "既定の ImageCell の例")

`ImageCell` 連絡先またはムービーの一覧などの視覚的要素を使用してデータの一覧を表示する必要がある場合に役立ちます。 `ImageCell`はカスタマイズ可能であり、次の設定を行うことができます。

- `Text` &ndash; 大きいフォントで、最初の行に表示されるテキスト
- `Detail` &ndash; テキストのフォント サイズを小さく、最初の行の下に表示されます。
- `TextColor` &ndash; テキストの色
- `DetailColor` &ndash; 詳細なテキストの色
- `ImageSource` &ndash; テキストの横に表示するイメージ

次のスクリーンショット`ImageCell`は、カスタマイズされた色プロパティを持つ項目を示しています。!["カスタマイズ]された ImageCell の例"(customizing-cell-appearance-images/image-cell-custom.png "カスタマイズ")された ImageCell の例

## <a name="custom-cells"></a>カスタムのセル
カスタムセルを使用すると、組み込みセルでサポートされていないセルレイアウトを作成できます。 たとえば、2 つのラベルを持つ同じ重み付けをセルに表示したい場合があります。 A`TextCell`ための十分なできなくなるため、`TextCell`が小さく、1 つのラベル。 ほとんどのセルのカスタマイズは、(追加ラベル、画像やその他の情報を表示) 読み取り専用データを追加します。

全てのカスタムセルは、すべての組み込みのセルが使用する基本クラスである[ `ViewCell` ](xref:Xamarin.Forms.ViewCell)から派生する必要があります。

Xamarin では、コントロールの[キャッシュ動作](~/xamarin-forms/user-interface/listview/performance.md#caching-strategy)が`ListView`提供されるため、一部の種類のカスタムセルのスクロールパフォーマンスが向上します。

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

- カスタムのセルが入れ子になっている、 `DataTemplate`、内である`ListView.ItemTemplate`します。 これは、組み込みのセルを使用するプロセスと同じです。
- `ViewCell` カスタムのセルの種類です。 `DataTemplate`要素の子は、 `ViewCell`クラスであるか、またはクラスから派生している必要があります。
- 内では、すべての Xamarin. フォームレイアウトでレイアウトを管理できます。 `ViewCell` この例では、レイアウトはによっ`StackLayout`て管理されます。これにより、背景色をカスタマイズできます。

> [!NOTE]
> バインド可能な`StackLayout`のすべてのプロパティは、カスタムセル内でバインドできます。 ただし、この機能は XAML の例には記載されていません。

### <a name="code"></a>コード

カスタムセルは、コードで作成することもできます。 最初に、から`ViewCell`派生するカスタムクラスを作成する必要があります。

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

ページコンストラクターでは、ListView の`ItemTemplate`プロパティは、指定された`CustomCell`型を`DataTemplate`持つに設定されます。

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

セルのカスタム型のバインドする場合に[ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)のインスタンスを表示する UI コントロール、`BindableProperty`値を使用する必要があります、 [ `OnBindingContextChanged` ](xref:Xamarin.Forms.Cell.OnBindingContextChanged)に表示されるデータを設定する上書き各セルはセル コンストラクターの次のコード例で示したなく:

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

[ `OnBindingContextChanged` ](xref:Xamarin.Forms.Cell.OnBindingContextChanged)ときに上書きを呼び出すが、 [ `BindingContextChanged` ](xref:Xamarin.Forms.BindableObject.BindingContextChanged)の値への応答でのイベントの起動、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)プロパティを変更します。 そのため、ときに、`BindingContext`変更されると、表示する UI コントロール、 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)値は、データを設定する必要があります。 なお、`BindingContext`をチェックする必要があります、`null`値がさらにガベージ コレクションの Xamarin.Forms で設定できます、`OnBindingContextChanged`オーバーライドが呼び出されます。

またに UI コントロールをバインドできます、 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)をオーバーライドする必要がなくなりますが、その値を表示するインスタンス、`OnBindingContextChanged`メソッド。

> [!NOTE]
> オーバーライドするときに`OnBindingContextChanged`、基本クラスのことを確認`OnBindingContextChanged`メソッドが呼び出されると、登録済みのデリゲートを受け取る、`BindingContextChanged`イベント。

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

これには、バインド、 `Name`、`Age`と`Location`でバインド可能なプロパティ、`CustomCell`インスタンスに、 `Name`、`Age`と`Location`基になるコレクション内の各オブジェクトのプロパティ。

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

IOS と Android での場合、 [ `ListView` ](xref:Xamarin.Forms.ListView)要素がリサイクルされると、カスタムのセルは、カスタム レンダラーを使用して、カスタム レンダラーがプロパティの変更通知を正しく実装する必要があります。 セルが再利用されると、`PropertyChanged` イベントが発生し、それらセルのプロパティの値は、binding context が有効なセルに更新される時を変更します。 より詳しい情報は [Customizing a ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md) を参照してください。 セルのリサイクルの詳細については、[キャッシュ戦略](~/xamarin-forms/user-interface/listview/performance.md#caching-strategy)を参照してください。

## <a name="related-links"></a>関連リンク

- [組み込みのセル (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-builtincells)
- [カスタムのセル (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)
- [バインド コンテキストを変更 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-bindingcontextchanged)
