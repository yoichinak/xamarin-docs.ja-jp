---
title: Viewcell のカスタマイズ
description: Xamarin.Forms ViewCell は、ListView、開発者が定義したビューを含むテーブルを追加できるセルです。 この記事では、Xamarin.Forms の ListView コントロールの内部でホストされている ViewCell のカスタム レンダラーを作成する方法を示します。
ms.prod: xamarin
ms.assetid: 61F378C9-6DEF-436B-ACC3-2324B25D404E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
ms.openlocfilehash: 3cb4d7f152e0f9540275f12f0ade568cd0552784
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935577"
---
# <a name="customizing-a-viewcell"></a>Viewcell のカスタマイズ

_Xamarin.Forms ViewCell は、ListView、開発者が定義したビューを含むテーブルを追加できるセルです。この記事では、Xamarin.Forms の ListView コントロールの内部でホストされている ViewCell のカスタム レンダラーを作成する方法を示します。これを停止します Xamarin.Forms のレイアウト計算が ListView のスクロール中に繰り返し呼び出されます。_

Xamarin.Forms のすべてのセルには、ネイティブ コントロールのインスタンスを作成する各プラットフォーム用の付随するレンダラーがあります。 ときに、 [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) iOS での Xamarin.Forms アプリケーションによって表示される、`ViewCellRenderer`クラスをインスタンス化がさらにインスタンス化をネイティブ`UITableViewCell`コントロール。 Android のプラットフォームで、`ViewCellRenderer`クラスのインスタンスを作成、ネイティブ`View`コントロール。 ユニバーサル Windows プラットフォーム (UWP) で、`ViewCellRenderer`クラスのインスタンスを作成、ネイティブ`DataTemplate`します。 レンダラーと Xamarin.Forms コントロールにマップするネイティブ コントロール クラスの詳細については、次を参照してください。[レンダラーの基本クラスおよびネイティブ コントロール](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)します。

次の図の間のリレーションシップを示しています、 [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)およびそれを実装するネイティブ コントロールの対応します。

![](viewcell-images/viewcell-classes.png "ViewCell コントロールと実装のネイティブ コントロール間のリレーションシップ")

レンダリング プロセスに実行できる活用用のカスタム レンダラーを作成してプラットフォーム固有のカスタマイズを実装する[ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)各プラットフォームで。 これを行うためのプロセスは次のとおりです。

1. [作成](#Creating_the_Custom_Cell)Xamarin.Forms カスタム セル。
1. [消費](#Consuming_the_Custom_Cell)Xamarin.Forms からカスタム セル。
1. [作成](#Creating_the_Custom_Renderer_on_each_Platform)プラットフォームごとのセルのカスタム レンダラーです。

各項目が実装するためにさらに、説明するようになりましたが、`NativeCell`活用、Xamarin.Forms 内でホストされている各セルのプラットフォームに固有のレイアウト レンダラー [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)コントロール。 これが停止中に繰り返し呼び出されるから Xamarin.Forms のレイアウトの計算`ListView`スクロールします。

<a name="Creating_the_Custom_Cell" />

## <a name="creating-the-custom-cell"></a>カスタムのセルを作成します。

セルのカスタム コントロールをサブクラス化して作成できます、 [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)クラスに、次のコード例に示すようにします。

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
`NativeCell`クラスは、.NET Standard ライブラリ プロジェクトが作成され、カスタムのセルの API を定義します。 カスタムのセルを公開`Name`、 `Category`、および`ImageFilename`データ バインディングを介して表示できるプロパティです。 データ バインディングの詳細については、「[Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)」 (データ バインディングの基礎) を参照してください。

<a name="Consuming_the_Custom_Cell" />

## <a name="consuming-the-custom-cell"></a>カスタムのセルの使用

`NativeCell`カスタム セルできますによって参照される Xaml で .NET Standard ライブラリ プロジェクトの場所の名前空間の宣言とカスタム セル要素の名前空間プレフィックスを使用します。 次のコード例に示す方法、 `NativeCell` XAML ページで、カスタムのセルを使用できます。

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

`local`何も名前空間プレフィックスを付けることができます。 ただし、`clr-namespace`と`assembly`値は、カスタム コントロールの詳細と一致する必要があります。 名前空間が宣言されると、カスタムのセルを参照する、プレフィックスが使用されます。

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

Xamarin.Forms を[ `ListView` ](xref:Xamarin.Forms.ListView)を介して作成される、データの一覧を表示するコントロールが使用される、 [ `ItemSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%601.ItemsSource/)プロパティ。 [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement)を最小化しようとしてキャッシング戦略、`ListView`メモリ フット プリントと実行を一覧のセルのリサイクルによって高速化します。 詳細については、次を参照してください。[キャッシュ戦略](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy)します。

リスト内の各行には、名前、カテゴリ、およびイメージのファイル名 – データの 3 つの項目が含まれています。 一覧内の各行のレイアウトが定義されている、`DataTemplate`を通じて参照される、 [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%601.ItemTemplate/)バインド可能なプロパティ。 `DataTemplate`リスト内のデータの行ごとになることを定義、`NativeCell`を表示するその`Name`、`Category`と`ImageFilename`プロパティ データ バインディングを使用します。 詳細については、`ListView`コントロールを参照してください[ListView](~/xamarin-forms/user-interface/listview/index.md)します。

カスタム レンダラーは、各セルのプラットフォームに固有のレイアウトをカスタマイズするには、各アプリケーション プロジェクトを今すぐ追加できます。

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>各プラットフォームでのカスタム レンダラーの作成

カスタム レンダラー クラスを作成するプロセスは次のとおりです。

1. サブクラスを作成、`ViewCellRenderer`カスタム セルを描画するクラス。
1. カスタムのセルを表示するプラットフォーム固有メソッドをオーバーライドし、それをカスタマイズするロジックを記述します。
1. 追加、`ExportRenderer`属性をカスタム レンダラー クラスは、Xamarin.Forms カスタム セルを表示するために使用するように指定します。 この属性は、Xamarin.Forms でのカスタム レンダラーの登録に使用されます。

> [!NOTE]
> ほとんどの Xamarin.Forms 要素では、各プラットフォーム プロジェクトにカスタム レンダラーを提供する省略可能です。 カスタム レンダラーが登録されていない場合は、コントロールの基底クラスの既定のレンダラーが使用されます。 ただし、カスタム レンダラー必要は各プラットフォーム プロジェクトに表示するときに、 [ViewCell](xref:Xamarin.Forms.ViewCell)要素。

次の図は、サンプル アプリケーションとそれらの間のリレーションシップ内の各プロジェクトの役割を示します。

![](viewcell-images/solution-structure.png "NativeCell カスタム レンダラーのプロジェクトの責任")

`NativeCell`カスタム セルは、プラットフォーム固有のレンダラー クラスから派生するによって表示される、`ViewCellRenderer`各プラットフォームのクラス。 これは、結果、各`NativeCell`の次のスクリーン ショットに示すように、プラットフォーム固有のレイアウトとレンダリングされるカスタムのセル。

![](viewcell-images/screenshots.png "各プラットフォームで NativeCell")

`ViewCellRenderer`クラスは、カスタムのセルを表示するためのプラットフォーム固有のメソッドを公開します。 これは、`GetCell`メソッドは、iOS プラットフォームで、 `GetCellCore` Android のプラットフォームでは、メソッド、および`GetTemplate`UWP のメソッド。

各カスタム レンダラー クラスで修飾された、`ExportRenderer`レンダラーを Xamarin.Forms で登録される属性。 属性は、– 表示するには、Xamarin.Forms のセルの型名と、カスタム レンダラーの種類の名前の 2 つのパラメーターを受け取ります。 `assembly`属性にプレフィックスは、属性がアセンブリ全体に適用されることを指定します。

次のセクションでは、各プラットフォームに固有のカスタム レンダラー クラスの実装について説明します。

### <a name="creating-the-custom-renderer-on-ios"></a>IOS でのカスタム レンダラーの作成

次のコード例では、iOS プラットフォーム用のカスタム レンダラーを示します。

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

`GetCell`表示するには、各セルを構築するメソッドが呼び出されます。 各セルが、`NativeiOSCell`インスタンスで、セルとそのデータのレイアウトを定義します。 操作、`GetCell`メソッドが依存、 [ `ListView` ](xref:Xamarin.Forms.ListView)戦略をキャッシュします。

- ときに、 [ `ListView` ](xref:Xamarin.Forms.ListView)戦略のキャッシュは[ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement)、`GetCell`各セルのメソッドが呼び出されます。 A`NativeiOSCell`の各インスタンスが作成されます`NativeCell`画面に表示される最初のインスタンス。 を通じて、ユーザーがスクロールすると、 `ListView`、`NativeiOSCell`インスタンスを再利用になります。 IOS のセルを再使用の詳細については、次を参照してください。[セルが再利用](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)します。

  > [!NOTE]
  > このカスタム レンダラーのコードは、いくつかのセルを再利用場合でも、 [ `ListView` ](xref:Xamarin.Forms.ListView)セルを保持するよう設定されます。

  各によって表示されるデータ`NativeiOSCell`インスタンス、新しく作成または再利用、かどうかはデータで更新される、各から`NativeCell`インスタンスによって、`UpdateCell`メソッド。

  > [!NOTE]
  > `OnNativeCellPropertyChanged`メソッドはではありません。 されると呼び出されます、 [ `ListView` ](xref:Xamarin.Forms.ListView)セルを保持する戦略のキャッシュが設定されています。

- ときに、 [ `ListView` ](xref:Xamarin.Forms.ListView)戦略のキャッシュは[ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement)、`GetCell`初期画面に表示される各セルのメソッドが呼び出されます。 A`NativeiOSCell`の各インスタンスが作成されます`NativeCell`画面に表示される最初のインスタンス。 各によって表示されるデータ`NativeiOSCell`インスタンスからのデータで更新されます、`NativeCell`インスタンスによって、`UpdateCell`メソッド。 ただし、`GetCell`をユーザーがスクロール メソッドを呼び出されなくなって、`ListView`します。 代わりに、`NativeiOSCell`インスタンスを再利用になります。 `PropertyChanged` イベントを発生させる、`NativeCell`インスタンスのデータが変更されたときに、`OnNativeCellPropertyChanged`イベント ハンドラーでは、再使用される各データを更新します。`NativeiOSCell`インスタンス。

次のコード例は、`OnNativeCellPropertyChanged`がときに呼び出されてメソッド、`PropertyChanged`イベントが発生します。

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

このメソッドを更新して表示されているデータが再利用`NativeiOSCell`インスタンス。 変更されたプロパティのチェックが行われる、メソッドは、複数回呼び出すことができます。

`NativeiOSCell`クラスが、各セルのレイアウトを定義し、次のコード例に示した。

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

このクラスは、セルの内容、およびそれらのレイアウトを表示するために使用されているコントロールを定義します。 クラスの実装、 [ `INativeElementView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INativeElementView/)インターフェイスで、ときに必要です、 [ `ListView` ](xref:Xamarin.Forms.ListView)を使用して、 [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement)戦略をキャッシュします。 このインターフェイスは、クラスを実装する必要がありますを指定します、 [ `Element` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INativeElementView.Element/)プロパティで、再利用されるセルのセルのカスタム データを返す必要があります。

`NativeiOSCell`コンス トラクターの初期化の外観、 `HeadingLabel`、 `SubheadingLabel`、および`CellImageView`プロパティ。 これらのプロパティに格納されているデータを表示する使用、`NativeCell`インスタンスで、`UpdateCell`各プロパティの値を設定する呼び出されるメソッド。 さらに、ときに、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)を使用して、 [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement)戦略によって表示されるデータをキャッシュ、 `HeadingLabel`、 `SubheadingLabel`、および`CellImageView`プロパティを指定できますによって更新、`OnNativeCellPropertyChanged`カスタム レンダラーのメソッド。

セルのレイアウトは、`LayoutSubviews`の座標を設定する上書きします。 `HeadingLabel`、 `SubheadingLabel`、および`CellImageView`セル内で。

### <a name="creating-the-custom-renderer-on-android"></a>Android でのカスタム レンダラーの作成

次のコード例では、Android プラットフォーム用のカスタム レンダラーを示します。

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

`GetCellCore`表示するには、各セルを構築するメソッドが呼び出されます。 各セルが、`NativeAndroidCell`インスタンスで、セルとそのデータのレイアウトを定義します。 操作、`GetCellCore`メソッドが依存、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)戦略をキャッシュします。

- ときに、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)戦略のキャッシュは[ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement)、`GetCellCore`各セルのメソッドが呼び出されます。 A`NativeAndroidCell`ごとに作成`NativeCell`画面に表示される最初のインスタンス。 を通じて、ユーザーがスクロールすると、 `ListView`、`NativeAndroidCell`インスタンスを再利用になります。 Android のセルが再利用の詳細については、次を参照してください。[行ビューを再利用](~/android/user-interface/layouts/list-view/populating.md)します。

  > [!NOTE]
  > このカスタム レンダラーのコードがいくつかのセルを再利用することに注意してください場合でも、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)セルを保持するよう設定されます。

  各によって表示されるデータ`NativeAndroidCell`インスタンス、新しく作成または再利用、かどうかはデータで更新される、各から`NativeCell`インスタンスによって、`UpdateCell`メソッド。

  > [!NOTE]
  > 注意してください、`OnNativeCellPropertyChanged`メソッドになります呼び出されたときに、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)がセルを保持するよう設定は更新されません、`NativeAndroidCell`プロパティの値。

- ときに、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)戦略のキャッシュは[ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement)、`GetCellCore`初期画面に表示される各セルのメソッドが呼び出されます。 A`NativeAndroidCell`の各インスタンスが作成されます`NativeCell`画面に表示される最初のインスタンス。 各によって表示されるデータ`NativeAndroidCell`インスタンスからのデータで更新されます、`NativeCell`インスタンスによって、`UpdateCell`メソッド。 ただし、`GetCellCore`をユーザーがスクロール メソッドを呼び出されなくなって、`ListView`します。 代わりに、`NativeAndroidCell`インスタンスを再利用になります。  `PropertyChanged` イベントを発生させる、`NativeCell`インスタンスのデータが変更されたときに、`OnNativeCellPropertyChanged`イベント ハンドラーでは、再使用される各データを更新します。`NativeAndroidCell`インスタンス。

次のコード例は、`OnNativeCellPropertyChanged`がときに呼び出されてメソッド、`PropertyChanged`イベントが発生します。

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

このメソッドを更新して表示されているデータが再利用`NativeAndroidCell`インスタンス。 変更されたプロパティのチェックが行われる、メソッドは、複数回呼び出すことができます。

`NativeAndroidCell`クラスが、各セルのレイアウトを定義し、次のコード例に示した。

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

このクラスは、セルの内容、およびそれらのレイアウトを表示するために使用されているコントロールを定義します。 クラスの実装、 [ `INativeElementView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INativeElementView/)インターフェイスで、ときに必要です、 [ `ListView` ](xref:Xamarin.Forms.ListView)を使用して、 [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement)戦略をキャッシュします。 このインターフェイスは、クラスを実装する必要がありますを指定します、 [ `Element` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INativeElementView.Element/)プロパティで、再利用されるセルのセルのカスタム データを返す必要があります。

`NativeAndroidCell`コンス トラクターの拡張、`NativeAndroidCell`レイアウト、および初期化、 `HeadingTextView`、 `SubheadingTextView`、および`ImageView`を高めのレイアウト コントロールのプロパティ。 これらのプロパティに格納されているデータを表示する使用、`NativeCell`インスタンスで、`UpdateCell`各プロパティの値を設定する呼び出されるメソッド。 さらに、ときに、 [ `ListView` ](xref:Xamarin.Forms.ListView)を使用して、 [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement)戦略によって表示されるデータをキャッシュ、 `HeadingTextView`、 `SubheadingTextView`、および`ImageView`プロパティを指定できますによって更新、`OnNativeCellPropertyChanged`カスタム レンダラーのメソッド。

次のコード例のレイアウトの定義を示しています、`NativeAndroidCell.axml`レイアウト ファイル。

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

このレイアウトは、その 2 つを指定`TextView`コントロールと`ImageView`コントロールがセルの内容を表示するために使用します。 2 つ`TextView`コントロールは垂直方向に内で、`LinearLayout`内に含まれているすべてのコントロールのコントロール、`RelativeLayout`します。

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP のカスタム レンダラーを作成します。

次のコード例では、UWP 用のカスタム レンダラーを示します。

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

`GetTemplate`リスト内のデータの行ごとに表示するセルを取得するメソッドが呼び出されます。 作成されます、`DataTemplate`ごと`NativeCell`インスタンスで、画面に表示される、`DataTemplate`セルの内容と外観を定義します。

`DataTemplate`は、アプリケーション レベルのリソース ディクショナリに格納され、次のコード例に示した。

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

`DataTemplate`セル、およびレイアウトと外観の内容を表示するために使用するコントロールを指定します。 2 つ`TextBlock`コントロールと`Image`コントロールがデータ バインドによって、セルの内容を表示するために使用します。 さらのインスタンス、`ConcatImageExtensionConverter`連結するために使用、`.jpg`ファイルに各イメージのファイル名拡張子。 これにより、`Image`コントロールの読み込みし、なったときに、イメージのレンダリング`Source`プロパティを設定します。

## <a name="summary"></a>まとめ

この記事では、用のカスタム レンダラーを作成する方法を示しましたが、 [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) 、Xamarin.Forms 内でホストされている[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)コントロール。 これが停止中に繰り返し呼び出されるから Xamarin.Forms のレイアウトの計算`ListView`スクロールします。


## <a name="related-links"></a>関連リンク

- [ListView のパフォーマンス](~/xamarin-forms/user-interface/listview/performance.md)
- [CustomRendererViewCell (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/viewcell/)
