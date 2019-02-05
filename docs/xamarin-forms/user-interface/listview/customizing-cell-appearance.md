---
title: ListView セルの外観をカスタマイズします。
description: この記事では、ListView コントロールの利便性を活用しながら、Xamarin.Forms アプリケーションでのデータを表示するためのオプションについて説明します。
ms.prod: xamarin
ms.assetid: FD45CB91-1A8F-46FB-B432-6BC20492E456
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
ms.openlocfilehash: 067ff4758ca78f7d706c7be96ffecd10e4e57965
ms.sourcegitcommit: d8edb1b9e7fd61979014d5f5f091ee135ab70e34
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/04/2019
ms.locfileid: "55712073"
---
# <a name="customizing-listview-cell-appearance"></a>ListView セルの外観をカスタマイズします。

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/CustomCells)

ListView はスクロール可能なリストは、使用してカスタマイズできる`ViewCell`秒。 `ViewCells` テキストとイメージを表示、true または false の状態を示すおよびユーザー入力を受け取るのために使用できます。

ListView セルから希望の外観を取得する 2 つの方法はあります。

- **[組み込みのセルのカスタマイズ](#Built_in_Cells)** &ndash;より簡単に実装し、カスタマイズ性を犠牲のパフォーマンスが向上します。
- **[カスタムのセルを作成する](#customcells)** &ndash;より制御の結果が正しく実装されていない場合のパフォーマンスの問題の可能性があります。

<a name="Built_in_Cells" />

## <a name="built-in-cells"></a>セルの構築
多くの単純なアプリケーションに最適な組み込みのセルには Xamarin.Forms できます。

- **TextCell** &ndash;テキストを表示します。
- **ImageCell** &ndash;テキストとイメージを表示します。

2 つの追加セル[ `SwitchCell` ](~/xamarin-forms/user-interface/tableview.md#switchcell)と[ `EntryCell` ](~/xamarin-forms/user-interface/tableview.md#entrycell)でよく使用されていないが、使用できるは`ListView`します。 参照してください[ `TableView` ](~/xamarin-forms/user-interface/tableview.md)これらのセルの詳細についてはします。

<a name="TextCell" />

### <a name="textcell"></a>TextCell

[`TextCell`](xref:Xamarin.Forms.TextCell) 詳細なテキストとして 2 番目の行で必要に応じて、テキストを表示するためのセルです。

パフォーマンスは、カスタムと比べると非常に良好なので、実行時にネイティブ コントロールとして TextCells がレンダリングされます`ViewCell`します。 TextCells に設定することができます、カスタマイズ可能な場合は。

- `Text` &ndash; 大きいフォントで、最初の行に表示されるテキスト。
- `Detail` &ndash; 小さいフォントで、最初の行の下に表示されるテキスト。
- `TextColor` &ndash; テキストの色。
- `DetailColor` &ndash; 詳細なテキストの色

![](customizing-cell-appearance-images/text-cell-default.png "TextCell の既定の例")

![](customizing-cell-appearance-images/text-cell-custom.png "カスタマイズされた TextCell 例")

<a name="ImageCell" />

### <a name="imagecell"></a>ImageCell

[`ImageCell`](xref:Xamarin.Forms.ImageCell)、のような`TextCell`、テキストおよびセカンダリの詳細なテキストを表示するために使用でき、各プラットフォームのネイティブ コントロールを使用して優れたパフォーマンスを提供します。 `ImageCell` 異なる`TextCell`イメージ、テキストの左側に表示します。

`ImageCell` 連絡先またはムービーの一覧などの視覚的要素を使用してデータの一覧を表示する必要がある場合に役立ちます。 ImageCells に設定することができます、カスタマイズ可能な場合は。

- `Text` &ndash; 大きいフォントで、最初の行に表示されるテキスト
- `Detail` &ndash; テキストのフォント サイズを小さく、最初の行の下に表示されます。
- `TextColor` &ndash; テキストの色
- `DetailColor` &ndash; 詳細なテキストの色
- `ImageSource` &ndash; テキストの横に表示するイメージ

![](customizing-cell-appearance-images/image-cell-default.png "ImageCell の既定の例")

![](customizing-cell-appearance-images/image-cell-custom.png "カスタマイズされた ImageCell 例")

<a name="customcells" />

## <a name="custom-cells"></a>カスタムのセル
組み込みのセルが、必要なレイアウトを指定しなかった場合、カスタム セルは、必要なレイアウトを実装します。 たとえば、2 つのラベルを持つ同じ重み付けをセルに表示したい場合があります。 A`TextCell`ための十分なできなくなるため、`TextCell`が小さく、1 つのラベル。 ほとんどのセルのカスタマイズは、(追加ラベル、画像やその他の情報を表示) 読み取り専用データを追加します。

全てのカスタムセルは、すべての組み込みのセルが使用する基本クラスである[ `ViewCell` ](xref:Xamarin.Forms.ViewCell)から派生する必要があります。

Xamarin.Forms 2 に導入された新しい[キャッシュ動作](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy)上、`ListView`コントロール カスタム セルの種類によってスクロールのパフォーマンスを向上させるために設定することができます。

これは、カスタムのセルの例を示します。

![](customizing-cell-appearance-images/custom-cell.png "カスタムのセルの例")

### <a name="xaml"></a>XAML
上記のレイアウトを作成する XAML を次に示します。

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

上記の XAML は多くのことを行っています。 分割してみましょう。

- カスタムのセルが入れ子になっている、 `DataTemplate`、内である`ListView.ItemTemplate`します。 これは、その他のセルを使用する場合と同じプロセスです。
- `ViewCell` カスタムのセルの種類です。 子、`DataTemplate`要素でまたは型から派生する必要があります`ViewCell`します。
- その内部に注意してください、 `ViewCell`、レイアウトは、によって管理される、`StackLayout`します。 このレイアウトでは、背景色をカスタマイズできます。 注意してくださいの任意のプロパティ`StackLayout`はバインド可能なことができますが、ここで示されていませんが、カスタムのセルの内部にバインドします。
- 内で、 `ViewCell`Xamarin.Forms のレイアウトでレイアウトを管理することができます。 

### <a name="cnum"></a>C&num;

C# でカスタムのセルを指定することは、XAML 相当するものよりもやや冗長です。 では、始めましょう。

最初に、カスタム セル クラスを定義で`ViewCell`基底クラスとして。

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

使用してページのコンス トラクターに、 `ListView`、ListView の設定`ItemTemplate`プロパティを新しい`DataTemplate`:

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

注意のコンス トラクター`DataTemplate`は、型を受け取ります。 Typeof 演算子の CLR 型を取得する`CustomCell`します。

<a name="binding-context-changes" />

### <a name="binding-context-changes"></a>バインド コンテキストの変更

セルのカスタム型のバインドする場合に[ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)のインスタンスを表示する UI コントロール、`BindableProperty`値を使用する必要があります、 [ `OnBindingContextChanged` ](xref:Xamarin.Forms.Cell.OnBindingContextChanged)に表示されるデータを設定する上書き各セルはセル コンス トラクターの次のコード例で示したなく:

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

    public string Name {
        get { return(string)GetValue (NameProperty); }
        set { SetValue (NameProperty, value); }
    }

    public int Age {
        get { return(int)GetValue (AgeProperty); }
        set { SetValue (AgeProperty, value); }
    }

    public string Location {
        get { return(string)GetValue (LocationProperty); }
        set { SetValue (LocationProperty, value); }
    }
    ...

    protected override void OnBindingContextChanged ()
    {
        base.OnBindingContextChanged ();

        if (BindingContext != null) {
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

var listView = new ListView {
    ItemsSource = people,
    ItemTemplate = customCell
};
```

IOS と Android での場合、 [ `ListView` ](xref:Xamarin.Forms.ListView)要素がリサイクルされると、カスタムのセルは、カスタム レンダラーを使用して、カスタム レンダラーがプロパティの変更通知を正しく実装する必要があります。 セルが再利用されると、`PropertyChanged` イベントが発生し、それらセルのプロパティの値は、binding context が有効なセルに更新される時を変更します。 より詳しい情報は [Customizing a ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md) を参照してください。 セルのリサイクルの詳細については、次を参照してください。[キャッシュ戦略](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy)します。

## <a name="related-links"></a>関連リンク

- [組み込みのセル (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BuiltInCells)
- [カスタムのセル (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/CustomCells)
- [バインド コンテキストを変更 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BindingContextChanged)
