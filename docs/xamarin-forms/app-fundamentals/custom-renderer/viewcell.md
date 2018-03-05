---
title: "ViewCell をカスタマイズします。"
description: "Xamarin.Forms ViewCell は、ListView または開発者が定義したビューが含まれているテーブルに追加できるセルです。 この記事では、Xamarin.Forms ListView コントロールの内部でホストされている ViewCell 用のカスタム レンダラーを作成する方法を示します。 これによってされない Xamarin.Forms レイアウトの計算で停止 ListView スクロール中に繰り返し呼び出されます。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 61F378C9-6DEF-436B-ACC3-2324B25D404E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
ms.openlocfilehash: 2c65bce7ae468ef07c6d898e3f532aa95580f2ba
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="customizing-a-viewcell"></a>ViewCell をカスタマイズします。

_Xamarin.Forms ViewCell は、ListView または開発者が定義したビューが含まれているテーブルに追加できるセルです。この記事では、Xamarin.Forms ListView コントロールの内部でホストされている ViewCell 用のカスタム レンダラーを作成する方法を示します。これによってされない Xamarin.Forms レイアウトの計算で停止 ListView スクロール中に繰り返し呼び出されます。_

Xamarin.Forms のすべてのセルは、ネイティブ コントロールのインスタンスを作成する各プラットフォームの付属のレンダラーがします。 ときに、 [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) iOS 内の Xamarin.Forms アプリケーションによって表示される、`ViewCellRenderer`クラスをインスタンス化、これがインスタンス化ネイティブ`UITableViewCell`コントロール。 Android のプラットフォームでは、`ViewCellRenderer`クラスのインスタンスを作成、ネイティブな`View`コントロール。 Windows Phone でユニバーサル Windows プラットフォーム (UWP)、`ViewCellRenderer`クラスのインスタンスを作成、ネイティブな`DataTemplate`します。 レンダラーと Xamarin.Forms のコントロールにマップするネイティブ コントロール クラスの詳細については、次を参照してください。[レンダラー基底クラスとネイティブ コントロール](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)です。

次の図の間のリレーションシップを示しています、 [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)とそれを実装する、対応するネイティブ コントロール。

![](viewcell-images/viewcell-classes.png "ViewCell コントロールと実装のネイティブ コントロール間のリレーションシップ")

表示処理に実行できるの利点のカスタム レンダラーを作成することで、プラットフォーム固有のカスタマイズ設定を実装する、 [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)プラットフォームごとにします。 これを行うためのプロセスは次のとおりです。

1. [作成](#Creating_the_Custom_Cell)Xamarin.Forms カスタム セル。
1. [消費](#Consuming_the_Custom_Cell)Xamarin.Forms からカスタムのセル。
1. [作成](#Creating_the_Custom_Renderer_on_each_Platform)プラットフォームごとのセルのカスタム レンダラーです。

各項目を実装する順番に説明するようになりましたが、 `NativeCell` 、Xamarin.Forms 内でホストされる各セルのプラットフォーム固有のレイアウトを利用するレンダラー [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)コントロール。 これにより、停止、Xamarin.Forms レイアウトの計算中に繰り返し呼び出される`ListView`スクロールします。

<a name="Creating_the_Custom_Cell" />

## <a name="creating-the-custom-cell"></a>カスタムのセルを作成します。

サブクラス化して、カスタムのセル コントロールを作成することができます、 [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)クラスに、次のコード例に示すようにします。

```csharp
public class NativeCell : ViewCell
{
  public static readonly BindableProperty NameProperty =
    BindableProperty.Create ("Name", typeof(string), typeof(NativeCell), "");

  public string Name {
    get { return (string)GetValue (NameProperty); }
    set { SetValue (NameProperty, value); }
  }

  public static readonly BindableProperty CategoryProperty =
    BindableProperty.Create ("Category", typeof(string), typeof(NativeCell), "");

  public string Category {
    get { return (string)GetValue (CategoryProperty); }
    set { SetValue (CategoryProperty, value); }
  }

  public static readonly BindableProperty ImageFilenameProperty =
    BindableProperty.Create ("ImageFilename", typeof(string), typeof(NativeCell), "");

  public string ImageFilename {
    get { return (string)GetValue (ImageFilenameProperty); }
    set { SetValue (ImageFilenameProperty, value); }
  }
}
```
`NativeCell`クラスは、ポータブル クラス ライブラリ (PCL) プロジェクトが作成され、カスタムのセルの API を定義します。 カスタムのセルを公開`Name`、 `Category`、および`ImageFilename`データ バインディングによって表示可能なプロパティです。 データ バインディングの詳細については、「[Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)」 (データ バインディングの基礎) を参照してください。

<a name="Consuming_the_Custom_Cell" />

## <a name="consuming-the-custom-cell"></a>カスタムのセルの使用

`NativeCell`カスタム セルできますによって参照される Xaml で PCL プロジェクト内の場所の名前空間の宣言とカスタム セル要素に名前空間プレフィックスを使用します。 次のコード例に示す方法、 `NativeCell` XAML ページで使用できるカスタム セル。

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <ContentPage.Content>
        <StackLayout>
            <Label Text="Xamarin.Forms native cell" HorizontalTextAlignment="Center" />
            <ListView x:Name="listView" CachingStrategy="RecycleElement" ItemSelected="OnItemSelected">
                <ListView.ItemTemplate>
                    <DataTemplate>
                        <local:NativeCell Name="{Binding Name}" Category="{Binding Category}" ImageFilename="{Binding ImageFilename}" />
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

`local`何も名前空間プレフィックスを付けることができます。 ただし、`clr-namespace`と`assembly`値がカスタム コントロールの詳細情報と一致する必要があります。 名前空間が宣言されると、カスタムのセルを参照する、プレフィックスを使用します。

次のコード例に示す方法、`NativeCell`カスタム セルは、c# のページで使用できます。

```csharp
public class NativeCellPageCS : ContentPage
{
    ListView listView;

    public NativeCellPageCS()
    {
        listView = new ListView(ListViewCachingStrategy.RecycleElement)
        {
            ItemsSource = DataSource.GetList(),
            ItemTemplate = new DataTemplate(() =>
            {
                var nativeCell = new NativeCell();
                nativeCell.SetBinding(NativeCell.NameProperty, "Name");
                nativeCell.SetBinding(NativeCell.CategoryProperty, "Category");
                nativeCell.SetBinding(NativeCell.ImageFilenameProperty, "ImageFilename");

                return nativeCell;
            })
        };

        switch (Device.RuntimePlatform)
        {
            case Device.iOS:
                Padding = new Thickness(0, 20, 0, 0);
                break;
            case Device.Android:
            case Device.UWP:
                Padding = new Thickness(0);
                break;
        }

        Content = new StackLayout
        {
            Children = {
                new Label { Text = "Xamarin.Forms native cell", HorizontalTextAlignment = TextAlignment.Center },
                listView
            }
        };
        listView.ItemSelected += OnItemSelected;
    }
    ...
}
```

Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)を介して作成される、データの一覧を表示するコントロールが使用される、 [ `ItemSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%601.ItemsSource/)プロパティです。 [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/)キャッシング戦略を最小限にとどめる、`ListView`を一覧のセルを再利用して、メモリ使用量と実行が高速化します。 詳細については、次を参照してください。[キャッシュ戦略](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy)です。

リスト内の各行には、データの名前、カテゴリ、および画像のファイル名の 3 つの項目が含まれています。 リスト内の各行のレイアウトがによって定義された、`DataTemplate`を通じて参照される、 [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%601.ItemTemplate/)バインド可能プロパティです。 `DataTemplate`リスト内のデータの行ごとになることを定義、`NativeCell`を表示するその`Name`、 `Category`、および`ImageFilename`プロパティ データ バインディングを使用します。 詳細については、`ListView`を制御しを参照してください[ListView](~/xamarin-forms/user-interface/listview/index.md)です。

カスタム レンダラーは、各セルのプラットフォーム固有のレイアウトをカスタマイズするには、各アプリケーション プロジェクトを今すぐ追加できます。

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>各プラットフォームでは、カスタム レンダラーを作成します。

カスタム レンダラー クラスを作成するプロセスは次のとおりです。

1. サブクラスを作成、`ViewCellRenderer`カスタムのセルを表示するクラス。
1. カスタムのセルを表示するプラットフォーム固有メソッドをオーバーライドし、それをカスタマイズするロジックを記述します。
1. 追加、`ExportRenderer`属性をカスタム レンダラー クラス Xamarin.Forms カスタム セルを表示するために使用することを指定します。 この属性を使用して、Xamarin.Forms を使用したカスタム レンダラーを登録します。

> [!NOTE]
> **注**: Xamarin.Forms のほとんどの要素は、各プラットフォームのプロジェクトでのカスタム レンダラーを提供する省略可能です。 カスタム レンダラーが登録されていない場合は、コントロールの基底クラスの既定のレンダラーが使用されます。 ただし、カスタム レンダラーが必要に各プラットフォームのプロジェクトでレンダリングするときに、 [ViewCell](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)要素。

次の図は、両者間のリレーションシップと共に、サンプル アプリケーション内の各プロジェクトの役割を示しています。

![](viewcell-images/solution-structure.png "NativeCell カスタム レンダラーのプロジェクトの責任")

`NativeCell`カスタム セルがから派生するプラットフォーム固有のレンダラー クラスによって表示される、`ViewCellRenderer`各プラットフォームのクラスです。 これは、結果、各`NativeCell`次のスクリーン ショットに示すように、プラットフォーム固有のレイアウトとレンダリングされるカスタムのセル。

![](viewcell-images/screenshots.png "各プラットフォームで NativeCell")

`ViewCellRenderer`クラスは、カスタムのセルを表示するためのプラットフォーム固有のメソッドを公開します。 これは、`GetCell`プラットフォームでは、iOS、メソッド、`GetCellCore`プラットフォームでは、Android、メソッド、 `GetTemplate` Windows Phone のプラットフォームでのメソッドです。

各カスタム レンダラー クラスがで修飾された、 `ExportRenderer` Xamarin.Forms では、レンダラーを登録する属性。 属性は、–、表示される Xamarin.Forms セルの種類の名前とカスタム レンダラーの種類の名前の 2 つのパラメーターを受け取ります。 `assembly`属性にプレフィックスは、属性がアセンブリ全体に適用されることを指定します。

次のセクションでは、各プラットフォームに固有のカスタム レンダラー クラスの実装について説明します。

### <a name="creating-the-custom-renderer-on-ios"></a>IOS では、カスタム レンダラーを作成します。

次のコード例は、iOS プラットフォーム用のカスタム レンダラーを示しています。

```csharp
[assembly: ExportRenderer(typeof(NativeCell), typeof(NativeiOSCellRenderer))]
namespace CustomRenderer.iOS
{
    public class NativeiOSCellRenderer : ViewCellRenderer
    {
        NativeiOSCell cell;

        public override UITableViewCell GetCell(Cell item, UITableViewCell reusableCell, UITableView tv)
        {
            var nativeCell = (NativeCell)item;

            cell = reusableCell as NativeiOSCell;
            if (cell == null)
                cell = new NativeiOSCell(item.GetType().FullName, nativeCell);
            else
                cell.NativeCell.PropertyChanged -= OnNativeCellPropertyChanged;

            nativeCell.PropertyChanged += OnNativeCellPropertyChanged;
            cell.UpdateCell(nativeCell);
            return cell;
        }
        ...
    }
}
```

`GetCell`を表示するには、各セルを構築するメソッドが呼び出されます。 各セルは、`NativeiOSCell`インスタンスで、セルとそのデータのレイアウトを定義します。 操作、`GetCell`メソッドが依存、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)戦略をキャッシュします。

- ときに、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 、キャッシング戦略[ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/)、`GetCell`各セルのメソッドが呼び出されます。 A`NativeiOSCell`の各インスタンスが作成されます`NativeCell`画面に表示される最初のインスタンス。 ユーザーがスクロールとして、 `ListView`、`NativeiOSCell`インスタンスを再利用します。 IOS のセルを再利用の詳細については、次を参照してください。[セルが再利用](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)です。

  > [!NOTE]
  > このカスタム レンダラー コードは、いくつかのセルを再利用場合でも、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)セルの保持に設定されています。

  各によって表示されるデータ`NativeiOSCell`インスタンス、新しく作成または再利用、かどうかがデータで更新される、各から`NativeCell`をインスタンス、`UpdateCell`メソッドです。

  > [!NOTE]
  > `OnNativeCellPropertyChanged`メソッドでは決してときに呼び出されます、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)セルを保持する戦略のキャッシュを設定します。

- ときに、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 、キャッシング戦略[ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/)、`GetCell`初期画面に表示される各セルのメソッドが呼び出されます。 A`NativeiOSCell`の各インスタンスが作成されます`NativeCell`画面に表示される最初のインスタンス。 各によって表示されるデータ`NativeiOSCell`インスタンスからのデータで更新されます、`NativeCell`をインスタンス、`UpdateCell`メソッドです。 ただし、`GetCell`メソッドをユーザーがスクロールとして起動しない、`ListView`です。 代わりに、`NativeiOSCell`インスタンスが再使用されます。 `PropertyChanged` イベントが発生、`NativeCell`インスタンスのデータが変更されたときに、`OnNativeCellPropertyChanged`イベント ハンドラーが再使用されるそれぞれのデータを更新`NativeiOSCell`インスタンス。

次のコード例に示す、`OnNativeCellPropertyChanged`時点で呼び出されますがメソッド、`PropertyChanged`イベントが発生します。

```csharp
namespace CustomRenderer.iOS
{
    public class NativeiOSCellRenderer : ViewCellRenderer
    {
        ...

        void OnNativeCellPropertyChanged(object sender, PropertyChangedEventArgs e)
        {
            var nativeCell = (NativeCell)sender;
            if (e.PropertyName == NativeCell.NameProperty.PropertyName)
            {
                cell.HeadingLabel.Text = nativeCell.Name;
            }
            else if (e.PropertyName == NativeCell.CategoryProperty.PropertyName)
            {
                cell.SubheadingLabel.Text = nativeCell.Category;
            }
            else if (e.PropertyName == NativeCell.ImageFilenameProperty.PropertyName)
            {
                cell.CellImageView.Image = cell.GetImage(nativeCell.ImageFilename);
            }
        }
    }
}
```

このメソッドを更新して表示されるデータが再利用`NativeiOSCell`インスタンス。 メソッドは、複数回呼び出すことができるように変更されているプロパティのチェックが行われます。

`NativeiOSCell`クラスは、各セルのレイアウトを定義し、次のコード例に示します。

```csharp
internal class NativeiOSCell : UITableViewCell, INativeElementView
{
  public UILabel HeadingLabel { get; set; }
  public UILabel SubheadingLabel { get; set; }
  public UIImageView CellImageView { get; set; }

  public NativeCell NativeCell { get; private set; }
  public Element Element => NativeCell;

  public NativeiOSCell(string cellId, NativeCell cell) : base(UITableViewCellStyle.Default, cellId)
  {
    NativeCell = cell;

    SelectionStyle = UITableViewCellSelectionStyle.Gray;
    ContentView.BackgroundColor = UIColor.FromRGB(255, 255, 224);
    CellImageView = new UIImageView();

    HeadingLabel = new UILabel()
    {
      Font = UIFont.FromName("Cochin-BoldItalic", 22f),
      TextColor = UIColor.FromRGB(127, 51, 0),
      BackgroundColor = UIColor.Clear
    };

    SubheadingLabel = new UILabel()
    {
      Font = UIFont.FromName("AmericanTypewriter", 12f),
      TextColor = UIColor.FromRGB(38, 127, 0),
      TextAlignment = UITextAlignment.Center,
      BackgroundColor = UIColor.Clear
    };

    ContentView.Add(HeadingLabel);
    ContentView.Add(SubheadingLabel);
    ContentView.Add(CellImageView);
  }

  public void UpdateCell(NativeCell cell)
  {
    HeadingLabel.Text = cell.Name;
    SubheadingLabel.Text = cell.Category;
    CellImageView.Image = GetImage(cell.ImageFilename);
  }

  public UIImage GetImage(string filename)
  {
    return (!string.IsNullOrWhiteSpace(filename)) ? UIImage.FromFile("Images/" + filename + ".jpg") : null;
  }

  public override void LayoutSubviews()
  {
    base.LayoutSubviews();

    HeadingLabel.Frame = new CGRect(5, 4, ContentView.Bounds.Width - 63, 25);
    SubheadingLabel.Frame = new CGRect(100, 18, 100, 20);
    CellImageView.Frame = new CGRect(ContentView.Bounds.Width - 63, 5, 33, 33);
  }
}
```

このクラスは、セルの内容、およびそれらのレイアウトを表示するために使用されているコントロールを定義します。 クラスを実装、 [ `INativeElementView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INativeElementView/)必要になるときに、インターフェイス、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)を使用して、 [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/)戦略をキャッシュします。 このインターフェイスは、このクラスを実装する必要がありますを指定します、 [ `Element` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INativeElementView.Element/)プロパティで、再利用されるセルのセルのカスタム データを返す必要があります。

`NativeiOSCell`コンス トラクターの外観を初期化します、 `HeadingLabel`、 `SubheadingLabel`、および`CellImageView`プロパティです。 これらのプロパティに格納されているデータを表示する使用、`NativeCell`インスタンスで、`UpdateCell`の各プロパティの値の設定に呼び出されるメソッド。 さらに、ときに、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)を使用して、 [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/)戦略、によって表示されるデータをキャッシュ、 `HeadingLabel`、 `SubheadingLabel`、および`CellImageView`プロパティを指定できますによって更新、`OnNativeCellPropertyChanged`カスタム レンダラーのメソッドです。

セルのレイアウトは、`LayoutSubviews`の座標を設定する、上書き`HeadingLabel`、 `SubheadingLabel`、および`CellImageView`セル内。

### <a name="creating-the-custom-renderer-on-android"></a>Android では、カスタム レンダラーを作成します。

次のコード例は、Android プラットフォーム用のカスタム レンダラーを示しています。

```csharp
[assembly: ExportRenderer(typeof(NativeCell), typeof(NativeAndroidCellRenderer))]
namespace CustomRenderer.Droid
{
    public class NativeAndroidCellRenderer : ViewCellRenderer
    {
        NativeAndroidCell cell;

        protected override Android.Views.View GetCellCore(Cell item, Android.Views.View convertView, ViewGroup parent, Context context)
        {
            var nativeCell = (NativeCell)item;
            Console.WriteLine("\t\t" + nativeCell.Name);

            cell = convertView as NativeAndroidCell;
            if (cell == null)
            {
                cell = new NativeAndroidCell(context, nativeCell);
            }
            else
            {
                cell.NativeCell.PropertyChanged -= OnNativeCellPropertyChanged;
            }

            nativeCell.PropertyChanged += OnNativeCellPropertyChanged;

            cell.UpdateCell(nativeCell);
            return cell;
        }
        ...
    }
}
```

`GetCellCore`を表示するには、各セルを構築するメソッドが呼び出されます。 各セルは、`NativeAndroidCell`インスタンスで、セルとそのデータのレイアウトを定義します。 操作、`GetCellCore`メソッドが依存、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)戦略をキャッシュします。

- ときに、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 、キャッシング戦略[ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/)、`GetCellCore`各セルのメソッドが呼び出されます。 A`NativeAndroidCell`ごとに作成されます`NativeCell`画面に表示される最初のインスタンス。 ユーザーがスクロールとして、 `ListView`、`NativeAndroidCell`インスタンスを再利用します。 Android のセルが再利用の詳細については、次を参照してください。[行ビューを再利用](~/android/user-interface/layouts/list-view/populating.md)です。

  > [!NOTE]
  > このレンダラーがカスタム コードがいくつかのセルを再利用を実行することに注意してください場合でも、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)セルの保持に設定されています。

  各によって表示されるデータ`NativeAndroidCell`インスタンス、新しく作成または再利用、かどうかがデータで更新される、各から`NativeCell`をインスタンス、`UpdateCell`メソッドです。

  > [!NOTE]
  > 注意してください、`OnNativeCellPropertyChanged`メソッドになるときに呼び出されます、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)がセルを保持する設定は更新されません、`NativeAndroidCell`プロパティの値。

- ときに、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 、キャッシング戦略[ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/)、`GetCellCore`初期画面に表示される各セルのメソッドが呼び出されます。 A`NativeAndroidCell`の各インスタンスが作成されます`NativeCell`画面に表示される最初のインスタンス。 各によって表示されるデータ`NativeAndroidCell`インスタンスからのデータで更新されます、`NativeCell`をインスタンス、`UpdateCell`メソッドです。 ただし、`GetCellCore`メソッドをユーザーがスクロールとして起動しない、`ListView`です。 代わりに、`NativeAndroidCell`インスタンスが再使用されます。  `PropertyChanged` イベントが発生、`NativeCell`インスタンスのデータが変更されたときに、`OnNativeCellPropertyChanged`イベント ハンドラーが再使用されるそれぞれのデータを更新`NativeAndroidCell`インスタンス。

次のコード例に示す、`OnNativeCellPropertyChanged`時点で呼び出されますがメソッド、`PropertyChanged`イベントが発生します。

```csharp
namespace CustomRenderer.Droid
{
    public class NativeAndroidCellRenderer : ViewCellRenderer
    {
        ...

        void OnNativeCellPropertyChanged(object sender, PropertyChangedEventArgs e)
        {
            var nativeCell = (NativeCell)sender;
            if (e.PropertyName == NativeCell.NameProperty.PropertyName)
            {
                cell.HeadingTextView.Text = nativeCell.Name;
            }
            else if (e.PropertyName == NativeCell.CategoryProperty.PropertyName)
            {
                cell.SubheadingTextView.Text = nativeCell.Category;
            }
            else if (e.PropertyName == NativeCell.ImageFilenameProperty.PropertyName)
            {
                cell.SetImage(nativeCell.ImageFilename);
            }
        }
    }
}
```

このメソッドを更新して表示されるデータが再利用`NativeAndroidCell`インスタンス。 メソッドは、複数回呼び出すことができるように変更されているプロパティのチェックが行われます。

`NativeAndroidCell`クラスは、各セルのレイアウトを定義し、次のコード例に示します。

```csharp
internal class NativeAndroidCell : LinearLayout, INativeElementView
{
  public TextView HeadingTextView { get; set; }
  public TextView SubheadingTextView { get; set; }
  public ImageView ImageView { get; set; }

  public NativeCell NativeCell { get; private set; }
  public Element Element => NativeCell;

  public NativeAndroidCell(Context context, NativeCell cell) : base(context)
  {
    NativeCell = cell;

    var view = (context as Activity).LayoutInflater.Inflate(Resource.Layout.NativeAndroidCell, null);
    HeadingTextView = view.FindViewById<TextView>(Resource.Id.HeadingText);
    SubheadingTextView = view.FindViewById<TextView>(Resource.Id.SubheadingText);
    ImageView = view.FindViewById<ImageView>(Resource.Id.Image);

    AddView(view);
  }

  public void UpdateCell(NativeCell cell)
  {
    HeadingTextView.Text = cell.Name;
    SubheadingTextView.Text = cell.Category;

    // Dispose of the old image
    if (ImageView.Drawable != null)
    {
      using (var image = ImageView.Drawable as BitmapDrawable)
      {
        if (image != null)
        {
          if (image.Bitmap != null)
          {
            image.Bitmap.Dispose();
          }
        }
      }
    }

    SetImage(cell.ImageFilename);
  }

  public void SetImage(string filename)
  {
    if (!string.IsNullOrWhiteSpace(filename))
    {
      // Display new image
      Context.Resources.GetBitmapAsync(filename).ContinueWith((t) =>
      {
        var bitmap = t.Result;
        if (bitmap != null)
        {
          ImageView.SetImageBitmap(bitmap);
          bitmap.Dispose();
        }
      }, TaskScheduler.FromCurrentSynchronizationContext());
    }
    else
    {
      // Clear the image
      ImageView.SetImageBitmap(null);
    }
  }
}
```

このクラスは、セルの内容、およびそれらのレイアウトを表示するために使用されているコントロールを定義します。 クラスを実装、 [ `INativeElementView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INativeElementView/)必要になるときに、インターフェイス、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)を使用して、 [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/)戦略をキャッシュします。 このインターフェイスは、このクラスを実装する必要がありますを指定します、 [ `Element` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INativeElementView.Element/)プロパティで、再利用されるセルのセルのカスタム データを返す必要があります。

`NativeAndroidCell`コンス トラクターだけ、`NativeAndroidCell`レイアウト、および初期化、 `HeadingTextView`、 `SubheadingTextView`、および`ImageView`高めのレイアウトでのコントロールのプロパティです。 これらのプロパティに格納されているデータを表示する使用、`NativeCell`インスタンスで、`UpdateCell`の各プロパティの値の設定に呼び出されるメソッド。 さらに、ときに、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)を使用して、 [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/)戦略、によって表示されるデータをキャッシュ、 `HeadingTextView`、 `SubheadingTextView`、および`ImageView`プロパティを指定できますによって更新、`OnNativeCellPropertyChanged`カスタム レンダラーのメソッドです。

次のコード例は、レイアウトの定義を示しています、`NativeAndroidCell.axml`レイアウト ファイル。

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:padding="8dp"
    android:background="@drawable/CustomSelector">
    <LinearLayout
        android:id="@+id/Text"
        android:orientation="vertical"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:paddingLeft="10dip">
        <TextView
            android:id="@+id/HeadingText"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textColor="#FF7F3300"
            android:textSize="20dip"
            android:textStyle="italic" />
        <TextView
            android:id="@+id/SubheadingText"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textSize="14dip"
            android:textColor="#FF267F00"
            android:paddingLeft="100dip" />
    </LinearLayout>
    <ImageView
        android:id="@+id/Image"
        android:layout_width="48dp"
        android:layout_height="48dp"
        android:padding="5dp"
        android:src="@drawable/icon"
        android:layout_alignParentRight="true" />
</RelativeLayout>
```

このレイアウトを指定する 2 つ`TextView`コントロールと`ImageView`コントロールがセルの内容を表示するために使用します。 2 つ`TextView`コントロールは垂直方向に配置内で、`LinearLayout`中に含まれるすべてのコントロールのコントロール、`RelativeLayout`です。

### <a name="creating-the-custom-renderer-on-windows-phone-and-uwp"></a>Windows Phone のカスタム レンダラーを作成し、UWP

次のコード例では、Windows Phone と UWP のカスタム レンダラーを示しています。

```csharp
[assembly: ExportRenderer(typeof(NativeCell), typeof(NativeWinPhoneCellRenderer))]
namespace CustomRenderer.WinPhone81
{
    public class NativeWinPhoneCellRenderer : ViewCellRenderer
    {
        public override Windows.UI.Xaml.DataTemplate GetTemplate(Cell cell)
        {
            return App.Current.Resources["ListViewItemTemplate"] as Windows.UI.Xaml.DataTemplate;
        }
    }
}
```

`GetTemplate`リスト内のデータの行ごとに描画するセルを取得するメソッドが呼び出されます。 作成、`DataTemplate`ごと`NativeCell`インスタンスで、画面に表示される、`DataTemplate`セルの内容と外観を定義します。

`DataTemplate`はアプリケーション レベル リソース ディクショナリに格納され、次のコード例に示します。

```xaml
<DataTemplate x:Key="ListViewItemTemplate">
    <Grid Background="LightYellow">
        <Grid.Resources>
            <local:ConcatImageExtensionConverter x:Name="ConcatImageExtensionConverter" />
        </Grid.Resources>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.40*" />
            <ColumnDefinition Width="0.40*"/>
            <ColumnDefinition Width="0.20*" />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.ColumnSpan="2" Foreground="#7F3300" FontStyle="Italic" FontSize="22" VerticalAlignment="Top" Text="{Binding Name}" />
        <TextBlock Grid.RowSpan="2" Grid.Column="1" Foreground="#267F00" FontWeight="Bold" FontSize="12" VerticalAlignment="Bottom" Text="{Binding Category}" />
        <Image Grid.RowSpan="2" Grid.Column="2" HorizontalAlignment="Left" VerticalAlignment="Center" Source="{Binding ImageFilename, Converter={StaticResource ConcatImageExtensionConverter}}" Width="50" Height="50" />
        <Line Grid.Row="1" Grid.ColumnSpan="3" X1="0" X2="1" Margin="30,20,0,0" StrokeThickness="1" Stroke="LightGray" Stretch="Fill" VerticalAlignment="Bottom" />
    </Grid>
</DataTemplate>
```

`DataTemplate`セル、およびレイアウトと外観の内容を表示するために使用するコントロールを指定します。 2 つ`TextBlock`コントロールと`Image`コントロールがデータ バインディングによってセルの内容を表示するために使用します。 さらのインスタンス、`ConcatImageExtensionConverter`連結するために使用、`.jpg`ファイルを各イメージ ファイル名拡張子。 これにより、`Image`コントロールが読み込むし、ある場合に、イメージをレンダリング`Source`プロパティを設定します。

## <a name="summary"></a>まとめ

この記事は、用のカスタム レンダラーを作成する方法を示しましたが、 [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) 、Xamarin.Forms 内でホストされている[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)コントロール。 これにより、停止、Xamarin.Forms レイアウトの計算中に繰り返し呼び出される`ListView`スクロールします。


## <a name="related-links"></a>関連リンク

- [ListView のパフォーマンス](~/xamarin-forms/user-interface/listview/performance.md)
- [CustomRendererViewCell (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/viewcell/)
