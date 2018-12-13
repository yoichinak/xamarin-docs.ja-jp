---
title: ViewCell のカスタマイズ
description: Xamarin.Forms の ViewCell は、ListView または TableView に追加できるセルであり、開発者が定義したビューを含みます。 この記事では、Xamarin.Forms の ListView コントロールの内部でホストされる ViewCell 用のカスタム レンダラーを作成する方法を示します。
ms.prod: xamarin
ms.assetid: 61F378C9-6DEF-436B-ACC3-2324B25D404E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
ms.openlocfilehash: b1ebe2694ad5fa996b8b679cfb31a203588de05c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38999000"
---
# <a name="customizing-a-viewcell"></a>ViewCell のカスタマイズ

"_Xamarin.Forms の ViewCell は、ListView または TableView に追加できるセルであり、開発者が定義したビューを含みます。この記事では、Xamarin.Forms の ListView コントロールの内部でホストされる ViewCell 用のカスタム レンダラーを作成する方法を示します。これにより、ListView のスクロール中に Xamarin.Forms のレイアウトの計算が繰り返し呼び出されることが回避されます。_"

Xamarin.Forms のすべてのセルには、ネイティブ コントロールのインスタンスを作成する各プラットフォーム用のレンダラーが付属しています。 iOS で Xamarin.Forms アプリケーションによって [`ViewCell`](xref:Xamarin.Forms.ViewCell) がレンダリングされると、iOS では `ViewCellRenderer` クラスがインスタンス化され、それによってネイティブの `UITableViewCell` コントロールもインスタンス化されます。 Android プラットフォーム上では、`ViewCellRenderer` クラスによってネイティブの `View` コントロールがインスタンス化されます。 ユニバーサル Windows プラットフォーム (UWP) 上では、`ViewCellRenderer` クラスによってネイティブの `DataTemplate` がインスタンス化されます。 Xamarin.Forms コントロールによってマップされるレンダラーとネイティブ コントロール クラスの詳細については、「[Renderer Base Classes and Native Controls](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)」(レンダラーの基底クラスとネイティブ コントロール) を参照してください。

次の図に、[`ViewCell`](xref:Xamarin.Forms.ViewCell) と、それを実装する、対応するネイティブ コントロールの関係を示します。

![](viewcell-images/viewcell-classes.png "ViewCell コントロールとネイティブ コントロールの実装間の関係")

レンダリング プロセスを活用して各プラットフォーム上で [`ViewCell`](xref:Xamarin.Forms.ViewCell) 用のカスタム レンダラーを作成することで、プラットフォーム固有のカスタマイズを実装できます。 その実行プロセスは次のとおりです。

1. Xamarin.Forms のカスタム セルを[作成](#Creating_the_Custom_Cell)します。
1. Xamarin.Forms からカスタム セルを[使用](#Consuming_the_Custom_Cell)します。
1. 各プラットフォーム上でセル用のカスタム レンダラーを[作成](#Creating_the_Custom_Renderer_on_each_Platform)します。

各項目を順に確認して、Xamarin.Forms の [`ListView`](xref:Xamarin.Forms.ListView) コントロールの内部でホストされる、各セル用のプラットフォーム固有のレイアウトを利用する `NativeCell` レンダラーを実装します。 これにより、`ListView` のスクロール中に Xamarin.Forms のレイアウトの計算が繰り返し呼び出されることが回避されます。

<a name="Creating_the_Custom_Cell" />

## <a name="creating-the-custom-cell"></a>カスタム セルの作成

次のコード例のように、[`ViewCell`](xref:Xamarin.Forms.ViewCell) クラスをサブクラス化することで、カスタム セル コントロールを作成できます。

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
`NativeCell` クラスが .NET Standard ライブラリ プロジェクト内に作成され、カスタム セル用の API が定義されます。 カスタム セルによって、データ バインディングを利用して表示できる `Name`、`Category`、および `ImageFilename` の各プロパティが公開されます。 データ バインディングの詳細については、「[Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)」 (データ バインディングの基礎) を参照してください。

<a name="Consuming_the_Custom_Cell" />

## <a name="consuming-the-custom-cell"></a>カスタム セルの使用

`NativeCell` カスタム セル用の場所の名前空間を宣言し、カスタム セル要素の名前空間プレフィックスを使用することで、XAML で .NET Standard ライブラリ プロジェクト内のカスタム セルを参照できます。 次のコード例で、XAML ページで `NativeCell` カスタム セルをどのように使用できるかを示します。

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

`local` 名前空間プレフィックスには任意の名前を付けることができます。 ただし、`clr-namespace` と `assembly` の値は、カスタム コントロールの詳細と一致させる必要があります。 名前空間が宣言されると、プレフィックスを使用してカスタム セルが参照されます。

次のコード例で、C# ページで `NativeCell` カスタム セルをどのように使用できるかを示します。

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

Xamarin.Forms の [`ListView`](xref:Xamarin.Forms.ListView) コントロールを使用して、データの一覧が表示されます。一覧は [`ItemSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) プロパティを利用して設定されます。 [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) キャッシング戦略では、一覧のセルをリサイクルすることで、`ListView` のメモリ占有領域を最小化し、実行速度を短縮しようとします。 詳細については、「[Caching Data](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy)」(キャッシング戦略) を参照してください。

一覧の各行には、名前、カテゴリ、および画像ファイルの名前という 3 つの項目が含まれます。 [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) バインド可能プロパティを利用して参照される `DataTemplate` によって、一覧の各行のレイアウトが定義されます。 `DataTemplate` によって、一覧の各データ行が、データ バインディングを利用して `Name`、`Category` および `ImageFilename` の各プロパティを表示する `NativeCell` であることが定義されます。 `ListView` コントロールの詳細については、「[ListView](~/xamarin-forms/user-interface/listview/index.md)」を参照してください。

これで、カスタム レンダラーを各アプリケーション プロジェクトに追加して、各セルのプラットフォーム固有のレイアウトをカスタマイズできます。

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>各プラットフォームでのカスタム レンダラーの作成

カスタム レンダラー クラスを作成するプロセスは次のとおりです。

1. カスタム セルをレンダリングする `ViewCellRenderer` クラスのサブクラスを作成します。
1. カスタム セルをレンダリングするプラットフォーム固有のメソッドをオーバーライドし、それをカスタマイズするロジックを記述します。
1. `ExportRenderer` 属性をカスタム レンダラー クラスに追加し、それを使用して Xamarin.Forms のカスタム セルがレンダリングされることを指定します。 この属性は、Xamarin.Forms にカスタム レンダラーを登録するために使用されます。

> [!NOTE]
> ほとんどの Xamarin.Forms 要素では、各プラットフォーム プロジェクトにカスタム レンダラーを指定するかどうかは任意です。 カスタム レンダラーが登録されていない場合は、コントロールの基底クラス用の既定のレンダラーが使用されます。 ただし、[ViewCell](xref:Xamarin.Forms.ViewCell) 要素をレンダリングするときは、各プラットフォーム プロジェクト内にカスタム レンダラーが必要です。

次の図に、サンプル アプリケーション内の各プロジェクトの役割とそれらの関係を示します。

![](viewcell-images/solution-structure.png "NativeCell カスタム レンダラーのプロジェクトの役割")

`NativeCell` カスタム セルはプラットフォーム固有のレンダラー クラスによってレンダリングされます。このクラスはすべて各プラットフォームの `ViewCellRenderer` クラスから派生します。 この結果、次のスクリーンショットに示すように、プラットフォーム固有のレイアウトを使用して、それぞれの `NativeCell` カスタム セルがレンダリングされます。

![](viewcell-images/screenshots.png "各プラットフォーム上の NativeCell")

`ViewCellRenderer` クラスによって、カスタム セルを表示するためのプラットフォーム固有のメソッドが公開されます。 これは、iOS プラットフォームでは `GetCell` メソッド、Android プラットフォームでは `GetCellCore` メソッド、および UWP では `GetTemplate` メソッドになります。

レンダラーを Xamarin.Forms に登録する `ExportRenderer` 属性によって各カスタム レンダラー クラスが修飾されます。 この属性では、レンダリングされる Xamarin.Forms セルの型名とカスタム レンダラーの型名という 2 つのパラメーターが使用されます。 属性の `assembly` プレフィックスにより、属性がアセンブリ全体に適用されることが指定されます。

次のセクションで、各プラットフォーム固有のカスタム レンダラー クラスの実装について説明します。

### <a name="creating-the-custom-renderer-on-ios"></a>iOS 上でのカスタム レンダラーの作成

次のコード例で、iOS プラットフォーム用のカスタム レンダラーを示します。

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

`GetCell` メソッドが呼び出され、表示される各セルが作成されます。 各セルは `NativeiOSCell` インスタンスであり、セルとそのデータのレイアウトを定義します。 `GetCell` メソッドの操作は、[`ListView`](xref:Xamarin.Forms.ListView) のキャッシュ戦略によって異なります。

- [`ListView`](xref:Xamarin.Forms.ListView) のキャッシュ戦略が [`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) の場合、各セルに対して `GetCell` メソッドが呼び出されます。 画面に最初に表示される各 `NativeCell` インスタンスに対して、`NativeiOSCell` インスタンスが作成されます。 ユーザーが `ListView` をスクロールすると、`NativeiOSCell` インスタンスが再利用されます。 iOS でのセルの再利用の詳細については、[セルの再利用](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)に関する記事を参照してください。

  > [!NOTE]
  > このカスタム レンダラーのコードでは、[`ListView`](xref:Xamarin.Forms.ListView) がセルを保持するように設定されている場合でも、多少のセルの再利用が実行されます。

  各 `NativeiOSCell` インスタンスによって表示されるデータは、新規作成されたものでも再利用されたものでも、`UpdateCell` メソッドによって各 `NativeCell` インスタンスからのデータに更新されます。

  > [!NOTE]
  > [`ListView`](xref:Xamarin.Forms.ListView) のキャッシング戦略がセルを保持するように設定されている場合は、`OnNativeCellPropertyChanged` メソッドが呼び出されることはありません。

- [`ListView`](xref:Xamarin.Forms.ListView) のキャッシュ戦略が [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) の場合、画面に最初に表示される各セルに対して `GetCell` メソッドが呼び出されます。 画面に最初に表示される各 `NativeCell` インスタンスに対して、`NativeiOSCell` インスタンスが作成されます。 各 `NativeiOSCell` インスタンスによって表示されるデータは、`UpdateCell` メソッドによって各 `NativeCell` インスタンスからのデータに更新されます。 ただし、ユーザーが `ListView` をスクロールしても、`GetCell` メソッドは呼び出されなくなります。 代わりに、`NativeiOSCell` インスタンスが再利用されます。 `NativeCell` インスタンスのデータが変更されると `PropertyChanged` イベントが発生し、再利用される各 `NativeiOSCell` インスタンスのデータが `OnNativeCellPropertyChanged` イベント ハンドラーによって更新されます。

次のコード例で、`PropertyChanged` イベントが発生したときに呼び出される`OnNativeCellPropertyChanged` メソッドを示します。

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

このメソッドによって、表示されるデータが、再利用される `NativeiOSCell` インスタンスによって更新されます。 メソッドを複数回呼び出すことができるため、変更されたプロパティのチェックが行われます。

次のコード例に示すように、`NativeiOSCell` クラスは、各セルのレイアウトを定義します。

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

このクラスは、セルのコンテンツとレイアウトのレンダリングに使用されるコントロールを定義します。 このクラスでは、[`INativeElementView`](xref:Xamarin.Forms.INativeElementView) インターフェイスを実装します。これは [`ListView`](xref:Xamarin.Forms.ListView) で [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) キャッシュ戦略を使用する場合に必要です。 このインターフェイスは、このクラスでは再利用されるセルのカスタム セル データを返す [`Element`](xref:Xamarin.Forms.INativeElementView.Element) プロパティを実装する必要があることを指定します。

`NativeiOSCell` コンストラクターによって、`HeadingLabel`、`SubheadingLabel`、および `CellImageView` の各プロパティの外観が初期化されます。 これらのプロパティを使用して、`NativeCell` インスタンスに格納されているデータが、各プロパティの値を設定する `UpdateCell` メソッドを呼び出して表示されます。 さらに、[`ListView`](xref:Xamarin.Forms.ListView) で [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) キャッシュ戦略を使用している場合は、`HeadingLabel`、`SubheadingLabel`、および `CellImageView` の各プロパティによって表示されるデータを、カスタム レンダラー内の `OnNativeCellPropertyChanged` メソッドによって更新できます。

セルのレイアウトは、`LayoutSubviews` のオーバーライドによって実行され、セル内の `HeadingLabel`、`SubheadingLabel`、および `CellImageView` の座標が設定されます。

### <a name="creating-the-custom-renderer-on-android"></a>Android 上でのカスタム レンダラーの作成

次のコード例で、Android プラットフォーム用のカスタム レンダラーを示します。

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

`GetCellCore` メソッドが呼び出され、表示される各セルが作成されます。 各セルは `NativeAndroidCell` インスタンスであり、セルとそのデータのレイアウトを定義します。 `GetCellCore` メソッドの操作は、[`ListView`](xref:Xamarin.Forms.ListView) のキャッシュ戦略によって異なります。

- [`ListView`](xref:Xamarin.Forms.ListView) のキャッシュ戦略が [`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) の場合、各セルに対して `GetCellCore` メソッドが呼び出されます。 画面に最初に表示される各 `NativeCell` インスタンスに対して、`NativeAndroidCell` が作成されます。 ユーザーが `ListView` をスクロールすると、`NativeAndroidCell` インスタンスが再利用されます。 ビューの再利用の詳細については、[行ビューの再利用](~/android/user-interface/layouts/list-view/populating.md)に関するページを参照してください。

  > [!NOTE]
  > このカスタム レンダラーのコードでは、[`ListView`](xref:Xamarin.Forms.ListView) がセルを保持するように設定されている場合でも、多少のセルの再利用が実行されることに注意してください。

  各 `NativeAndroidCell` インスタンスによって表示されるデータは、新規作成されたものでも再利用されたものでも、`UpdateCell` メソッドによって各 `NativeCell` インスタンスからのデータに更新されます。

  > [!NOTE]
  > [`ListView`](xref:Xamarin.Forms.ListView) がセルを保持するように設定されている場合は `OnNativeCellPropertyChanged` メソッドが呼び出されますが、`NativeAndroidCell` プロパティの値は更新されないことに注意してください。

- [`ListView`](xref:Xamarin.Forms.ListView) のキャッシュ戦略が [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) の場合、画面に最初に表示される各セルに対して `GetCellCore` メソッドが呼び出されます。 画面に最初に表示される各 `NativeCell` インスタンスに対して、`NativeAndroidCell` インスタンスが作成されます。 各 `NativeAndroidCell` インスタンスによって表示されるデータは、`UpdateCell` メソッドによって各 `NativeCell` インスタンスからのデータに更新されます。 ただし、ユーザーが `ListView` をスクロールしても、`GetCellCore` メソッドは呼び出されなくなります。 代わりに、`NativeAndroidCell` インスタンスが再利用されます。  `NativeCell` インスタンスのデータが変更されると `PropertyChanged` イベントが発生し、再利用される各 `NativeAndroidCell` インスタンスのデータが `OnNativeCellPropertyChanged` イベント ハンドラーによって更新されます。

次のコード例で、`PropertyChanged` イベントが発生したときに呼び出される`OnNativeCellPropertyChanged` メソッドを示します。

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

このメソッドによって、表示されるデータが、再利用される `NativeAndroidCell` インスタンスによって更新されます。 メソッドを複数回呼び出すことができるため、変更されたプロパティのチェックが行われます。

次のコード例に示すように、`NativeAndroidCell` クラスは、各セルのレイアウトを定義します。

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

このクラスは、セルのコンテンツとレイアウトのレンダリングに使用されるコントロールを定義します。 このクラスでは、[`INativeElementView`](xref:Xamarin.Forms.INativeElementView) インターフェイスを実装します。これは [`ListView`](xref:Xamarin.Forms.ListView) で [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) キャッシュ戦略を使用する場合に必要です。 このインターフェイスは、このクラスでは再利用されるセルのカスタム セル データを返す [`Element`](xref:Xamarin.Forms.INativeElementView.Element) プロパティを実装する必要があることを指定します。

`NativeAndroidCell` コンストラクターにより `NativeAndroidCell` レイアウトが拡張され、拡張されたレイアウト内のコントロールに対して `HeadingTextView`、`SubheadingTextView`、および `ImageView` の各プロパティが初期化されます。 これらのプロパティを使用して、`NativeCell` インスタンスに格納されているデータが、各プロパティの値を設定する `UpdateCell` メソッドを呼び出して表示されます。 さらに、[`ListView`](xref:Xamarin.Forms.ListView) で [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) キャッシュ戦略を使用している場合は、`HeadingTextView`、`SubheadingTextView`、および `ImageView` の各プロパティによって表示されるデータを、カスタム レンダラー内の `OnNativeCellPropertyChanged` メソッドによって更新できます。

次のコード例で、`NativeAndroidCell.axml` レイアウト ファイルのレイアウト定義を示します。

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

このレイアウトでは、セルのコンテンツの表示に使用される 2 つの `TextView` コントロールと `ImageView` コントロールを指定しています。 2 つの `TextView` コントロールは `LinearLayout` コントロール内で垂直方向に配置され、`RelativeLayout` 内にすべてのコントロールが含まれます。

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP 上でのカスタム レンダラーの作成

次のコード例で、UWP 用のカスタム レンダラーを示します。

```csharp
[assembly: ExportRenderer(typeof(NativeCell), typeof(NativeUWPCellRenderer))]
namespace CustomRenderer.UWP
{
    public class NativeUWPCellRenderer : ViewCellRenderer
    {
        public override Windows.UI.Xaml.DataTemplate GetTemplate(Cell cell)
        {
            return App.Current.Resources["ListViewItemTemplate"] as Windows.UI.Xaml.DataTemplate;
        }
    }
}
```

`GetTemplate` メソッドが呼び出され、リスト内の各行のデータがレンダリングされるセルが返されます。 画面に表示される各 `NativeCell` インスタンス用の `DataTemplate` が作成され、`DataTemplate` にセルのセルの外観とコンテンツが定義されます。

次のコード例に示すように、この `DataTemplate` は、アプリケーション レベルのリソース ディクショナリに格納されます。

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

`DataTemplate` では、レイアウトと外観と共に、セルのコンテンツの表示に使用されるコントロールを指定します。 データ バインディングを利用してセルのコンテンツを表示するために、2 つの `TextBlock` コントロールと `Image` コントロールが使用されます。 さらに、`.jpg` ファイルの拡張子を各イメージ ファイル名に付与するために、`ConcatImageExtensionConverter` のインスタンスが使用されます。 これにより、`Source` プロパティが設定された場合、`Image` コントロールでイメージの読み込みとレンダリングを確実に行うことができます。

## <a name="summary"></a>まとめ

この記事では、Xamarin.Forms の [`ListView`](xref:Xamarin.Forms.ListView) コントロールの内部でホストされる [`ViewCell`](xref:Xamarin.Forms.ViewCell) 用のカスタム レンダラーを作成する方法を示しました。 これにより、`ListView` のスクロール中に Xamarin.Forms のレイアウトの計算が繰り返し呼び出されることが回避されます。


## <a name="related-links"></a>関連リンク

- [ListView のパフォーマンス](~/xamarin-forms/user-interface/listview/performance.md)
- [CustomRendererViewCell (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/viewcell/)
