---
title: "セルの外観"
description: "ListView の利便性の利点を活かしながらデータを表すためのオプションを探索します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: FD45CB91-1A8F-46FB-B432-6BC20492E456
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
ms.openlocfilehash: 62ac3ab4b3114447f0c67d86c601a688bb8ff1a7
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="cell-appearance"></a>セルの外観

ListView の使用によりカスタマイズ可能なスクロール可能なリストを表示する`ViewCell`s。 `ViewCells` テキストとイメージを表示、true または false の状態を示す、およびユーザー入力を受け取るのために使用できます。

ListView のセルから、使用するファイルの場所を取得する 2 つの方法はあります。

- **[組み込みのセルのカスタマイズ](#Built_in_Cells)** &ndash;より簡単に実装し、カスタマイズ機能を犠牲にしてパフォーマンスを向上します。
- **[カスタムのセルの作成](#customcells)** &ndash;より制御の最終結果が正しく実装されていない場合は、パフォーマンスの問題の可能性があります。

<a name="Built_in_Cells" />

## <a name="built-in-cells"></a>セルに組み込まれています。
Xamarin.Forms は、多くの単純なアプリケーションで使用できる組み込みのセルに付属します。

- **TextCell** &ndash;テキストを表示します。
- **ImageCell** &ndash;テキストとイメージを表示します。

2 つの追加セル[ `SwitchCell` ](~/xamarin-forms/user-interface/tableview.md#switchcell)と[ `EntryCell` ](~/xamarin-forms/user-interface/tableview.md#entrycell)でよく使用されませんが、使用できるは`ListView`します。 参照してください[ `TableView` ](~/xamarin-forms/user-interface/tableview.md)これらのセルの詳細についてはします。

<a name="TextCell" />

### <a name="textcell"></a>TextCell

[`TextCell`](http://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) オプションで詳細テキストと 2 番目の行のテキストを表示するセルです。

パフォーマンスは、カスタムと比較して非常に良いように TextCells は実行時に、ネイティブなコントロールとしてレンダリングされます`ViewCell`です。 TextCells はカスタマイズ可能なを設定することができます。

- `Text` &ndash; 大きいフォントで、最初の行に表示されるテキストです。
- `Detail` &ndash; フォント サイズを小さくの最初の行の下に表示されるテキストです。
- `TextColor` &ndash; テキストの色。
- `DetailColor` &ndash; 詳細テキストの色

![](customizing-cell-appearance-images/text-cell-default.png "TextCell の既定の例")

![](customizing-cell-appearance-images/text-cell-custom.png "カスタマイズされた TextCell 例")

<a name="ImageCell" />

### <a name="imagecell"></a>ImageCell

[`ImageCell`](http://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/)、のような`TextCell`テキストとセカンダリ詳細テキストを表示するために使用できます、および各プラットフォームのネイティブ コントロールを使用して優れたパフォーマンスを提供します。 `ImageCell` 異なる`TextCell`テキストの左側にイメージが表示されます。

`ImageCell` 連絡先またはムービーの一覧などのビジュアルな部分で、データの一覧を表示する必要がある場合に役立ちます。 ImageCells はカスタマイズ可能なを設定することができます。

- `Text` &ndash; 大きいフォントで、最初の行に表示されるテキスト
- `Detail` &ndash; フォント サイズを小さくの最初の行の下に表示されるテキスト
- `TextColor` &ndash; テキストの色
- `DetailColor` &ndash; 詳細テキストの色
- `ImageSource` &ndash; テキストの横に表示するイメージ

Windows Phone 8.1 を対象とするときに注意してください`ImageCell`を既定ではイメージをスケールできません。 また、Windows Phone 8.1 がテキストが表示される詳細情報の唯一のプラットフォームである、同じ色とフォントとして既定では主要なテキストに注意してください。 Windows Phone 8.0 のレンダリング`ImageCell`以下に示すようにします。

![](customizing-cell-appearance-images/image-cell-default.png "ImageCell の既定の例")

![](customizing-cell-appearance-images/image-cell-custom.png "カスタマイズされた ImageCell 例")

<a name="customcells" />

## <a name="custom-cells"></a>カスタムのセル
組み込みのセルが、必要なレイアウトを提供しない場合に、カスタムのセルには、必要なレイアウトが実装されています。 たとえば、を同じ重みを持つ 2 つのラベルが付いたセルを表示することがあります。 A`LabelCell`十分となりますので、`LabelCell`小さいが 1 つのラベルがあります。

カスタムのすべてのセルから派生しなければなりません[ `ViewCell` ](http://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)、組み込みのセルのすべての型を使用している同じ基本クラスです。

セルのカスタマイズのほとんどは、(その他のラベル、イメージや他の情報を表示) など、追加の読み取り専用のデータを追加します。 ボタンまたはフォーカスすることはその他のコントロールを追加すると、セル自体は Android でクリック可能なできない可能性があります。 この制限を克服する方法を下を参照してください。

Xamarin.Forms 2 に導入された新しい[キャッシュの動作](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy)上、`ListView`コントロールいくつかの種類のカスタムのセルのスクロールのパフォーマンスを向上させるために設定することができます。

カスタムのセルの例を次に示します。

![](customizing-cell-appearance-images/custom-cell.png "カスタムのセルの例")

### <a name="xaml"></a>XAML
上記のレイアウトを作成する XAML 次に示します。

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

上記の XAML はずっと行っています。 分割してみましょう。

- カスタムのセルが入れ子になっている、 `DataTemplate`、内部にある`ListView.ItemTemplate`です。 これは、その他のセルを使用する場合と同じプロセスです。
- `ViewCell` カスタムのセルの種類です。 子、`DataTemplate`要素か必要がありますの型から派生する`ViewCell`です。
- その内部に注意してください、 `ViewCell`、レイアウトはによって管理されて、`StackLayout`です。 このレイアウトを使用して、背景色をカスタマイズすることができます。 注意の任意のプロパティ`StackLayout`されているバインド可能なことができますは、ここで示されていませんが、カスタムのセルの内部にバインドします。

### <a name="cnum"></a>C&num;

C# でカスタムのセルを指定することは、XAML の相当するよりも少し詳細なです。 では、始めましょう。

最初に、カスタム セル クラスを定義する`ViewCell`基底クラスとして。

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

使用してページのコンス トラクターで、 `ListView`、設定リスト ビューの`ItemTemplate`プロパティを新しい`DataTemplate`:

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

なお、コンス トラクターの`DataTemplate`型を受け取り、します。 Typeof 演算子の CLR 型を取得する`CustomCell`です。

<a name="binding-context-changes" />

### <a name="binding-context-changes"></a>バインド コンテキストの変更

セルのカスタム型にバインドするときに[ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)インスタンスを表示する UI コントロール、`BindableProperty`値を使用する必要があります、 [ `OnBindingContextChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.OnBindingContextChanged()/)に表示されるデータを設定する上書き各セルではなくセル コンス トラクターを次のコード例で示したようにします。

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

[ `OnBindingContextChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.OnBindingContextChanged()/)上書き呼び出されるときに、 [ `BindingContextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.BindableObject.BindingContextChanged/)の値への応答でのイベントの起動、 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)プロパティを変更します。 したがって、ときに、`BindingContext`変更されると、表示する UI コントロール、 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)値は、データを設定する必要があります。 注意してください、`BindingContext`をチェックする必要があります、`null`のガベージ コレクションは、さらに、Xamarin.Forms で設定できますように、値、`OnBindingContextChanged`呼び出されてオーバーライドします。

またを UI コントロールにバインドできる、 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)をオーバーライドする必要がなくなります値を表示するインスタンス、`OnBindingContextChanged`メソッドです。

> [!NOTE]
> オーバーライドする場合`OnBindingContextChanged`、基本クラスのことを確認`OnBindingContextChanged`メソッドが呼び出されると、登録されているデリゲートが表示される、`BindingContextChanged`イベント。

XAML では、カスタムのセルの種類をデータにバインド行うには次のコード例に示すように。

```xaml
<ListView x:Name="listView">
    <ListView.ItemTemplate>
        <DataTemplate>
            <local:CustomCell Name="{Binding Name}" Age="{Binding Age}" Location="{Binding Location}" />
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

バインド、 `Name`、 `Age`、および`Location`バインド可能なプロパティで、`CustomCell`インスタンスに、 `Name`、 `Age`、および`Location`基になるコレクション内の各オブジェクトのプロパティです。

C# の場合と同じバインディングは、次のコード例で表示されます。

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

IOS と Android の場合、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)要素がリサイクルしカスタムのセルは、カスタム レンダラーを使用して、カスタムのレンダラーがプロパティの変更通知を正しく実装する必要があります。 使用可能なセルのバインド コンテキストが更新されたときに、プロパティ値が変更されますセルが再利用される`PropertyChanged`イベントが発生します。 詳細については、次を参照してください。 [、ViewCell をカスタマイズする](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)です。 セルのリサイクルの詳細については、次を参照してください。[キャッシュ戦略](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy)です。

### <a name="enabling-row-selection-on-android"></a>Android での行の選択を有効にします。

含まれているセルの入力要素のために、行の選択を許可するボタン、単純な[ `custom renderer` ](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)が必要です。 サブクラスを作成、一般的なコードで`Button`されるため、プラットフォームのプロジェクトでカスタム レンダラーを追加することができます。

```csharp
public class ListButton : Button { }
```

Android 用レンダラーの実装で設定するだけ、`Focusable`ホスト クリック可能なボタンと選択可能にするのには、行を許可する属性。 このコードは、Android アプリケーション プロジェクトに追加されます。

```csharp
[assembly: ExportRenderer (typeof (ListButton), typeof (ListButtonRenderer))]
// ...
public class ListButtonRenderer : ButtonRenderer {
    protected override void OnElementChanged (ElementChangedEventArgs<ListButton> e) {
        base.OnElementChanged (e);
        Control.Focusable = false;
    }
}
```

前述のように、唯一の Android が必要です、`ButtonRenderer`実装します。 iOS および Windows Phone プラットフォームは、カスタム レンダラーを実装することがなくクリックするボタンを許可します。


## <a name="related-links"></a>関連リンク

- [組み込みのセル (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BuiltInCells)
- [カスタムのセル (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/CustomCells)
- [バインディング コンテキストを変更 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BindingContextChanged)
