---
title: ListView のカスタマイズ
description: Xamarin.Forms の ListView は、データのコレクションを縦方向の一覧として表示するビューです。 この記事では、プラットフォーム固有のリスト コントロールとネイティブのセルのレイアウトをカプセル化するカスタム レンダラーを作成し、ネイティブ リスト コントロールのパフォーマンスをより厳密に制御する方法を示します。
ms.prod: xamarin
ms.assetid: 2FBCB8C8-4F32-45E7-954F-63AD29D5F1B5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 39ba281f036b9c57f85629390f5ba76377c99dd8
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53054212"
---
# <a name="customizing-a-listview"></a>ListView のカスタマイズ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/listview/)

_Xamarin.Forms の ListView は、データのコレクションを縦方向の一覧として表示するビューです。この記事では、プラットフォーム固有のリスト コントロールとネイティブのセルのレイアウトをカプセル化するカスタム レンダラーを作成し、ネイティブ リスト コントロールのパフォーマンスをより厳密に制御する方法を示します。_

すべての Xamarin.Forms ビューに、ネイティブ コントロールのインスタンスを作成する各プラットフォーム用のレンダラーが付属しています。 iOS で Xamarin.Forms アプリケーションによって [`ListView`](xref:Xamarin.Forms.ListView) がレンダリングされると、`ListViewRenderer` クラスがインスタンス化され、それによってネイティブの `UITableView` コントロールもインスタンス化されます。 Android プラットフォーム上では、`ListViewRenderer` クラスによってネイティブの `ListView` コントロールがインスタンス化されます。 ユニバーサル Windows プラットフォーム (UWP) 上では、`ListViewRenderer` クラスによってネイティブの `ListView` コントロールがインスタンス化されます。 Xamarin.Forms コントロールによってマップされるレンダラーとネイティブ コントロール クラスの詳細については、「[Renderer Base Classes and Native Controls](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)」(レンダラーの基本クラスおよびネイティブ コントロール) を参照してください。

次の図は、[`ListView`](xref:Xamarin.Forms.ListView) コントロールと、それを実装する、対応するネイティブ コントロールの関係を示しています。

![](listview-images/listview-classes.png "ListView コントロールと実装するネイティブ コントロールの関係")

レンダリング プロセスを活用して各プラットフォーム上で [`ListView`](xref:Xamarin.Forms.ListView) 用のカスタム レンダラーを作成することで、プラットフォーム固有のカスタマイズを実装できます。 これを行うための実行プロセスは次のとおりです。

1. Xamarin.Forms カスタム コントロールを[作成](#Creating_the_Custom_ListView_Control)します。
1. Xamarin.Forms からカスタム コントロールを[使用](#Consuming_the_Custom_Control)します。
1. 各プラットフォーム上でコントロールのカスタム レンダラーを[作成](#Creating_the_Custom_Renderer_on_each_Platform)します。

プラットフォーム固有のリスト コントロールとネイティブのセルのレイアウトを利用する `NativeListView` レンダラーを実装するために、各項目について順番に確認しましょう。 このシナリオは、再利用可能なリストとセルのコードを含む既存のネイティブ アプリを移行する場合に便利です。 さらに、データの視覚化など、パフォーマンスに影響を与える可能性があるリスト コントロール機能の詳細なカスタマイズが可能になります。

<a name="Creating_the_Custom_ListView_Control" />

## <a name="creating-the-custom-listview-control"></a>カスタム ListView コントロールの作成

カスタム [`ListView`](xref:Xamarin.Forms.ListView) コントロールは、次のコード例のように、`ListView` クラスをサブクラス化することで作成できます。

```csharp
public class NativeListView : ListView
{
  public static readonly BindableProperty ItemsProperty =
    BindableProperty.Create ("Items", typeof(IEnumerable<DataSource>), typeof(NativeListView), new List<DataSource> ());

  public IEnumerable<DataSource> Items {
    get { return (IEnumerable<DataSource>)GetValue (ItemsProperty); }
    set { SetValue (ItemsProperty, value); }
  }

  public event EventHandler<SelectedItemChangedEventArgs> ItemSelected;

  public void NotifyItemSelected (object item)
  {
    if (ItemSelected != null) {
      ItemSelected (this, new SelectedItemChangedEventArgs (item));
    }
  }
}
```

`NativeListView` は、.NET 標準ライブラリ プロジェクトで作成され、カスタム コントロールの API を定義します。 このコントロールは、`ListView` にデータにデータを設定するために使用される`Items` プロパティを公開しており、表示目的のために、このプロパティをデータ バインディングすることが可能です。 また、このコントロールでは、プラットフォーム固有のネイティブ リスト コントロールで項目が選択されるたびに起動される `ItemSelected` イベントも公開しています。 データ バインディングの詳細については、「[Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)」 (データ バインディングの基礎) を参照してください。

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>カスタム コントロールの使用

`NativeListView` カスタム コントロールは、その場所の名前空間を宣言し、コントロール上で名前空間プレフィックスを使用することで、.NET 標準ライブラリ プロジェクトの XAML で参照することができます。 次のコード例は、XAML ページがどのように `NativeListView` カスタム コントロールを使用できるかを示しています。

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <ContentPage.Content>
          <Grid>
            <Grid.RowDefinitions>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="*" />
            </Grid.RowDefinitions>
          <Label Text="{x:Static local:App.Description}" HorizontalTextAlignment="Center" />
            <local:NativeListView Grid.Row="1" x:Name="nativeListView" ItemSelected="OnItemSelected" VerticalOptions="FillAndExpand" />
          </Grid>
      </ContentPage.Content>
</ContentPage>
```

`local` 名前空間プレフィックスには任意の名前を付けることができます。 ただし、`clr-namespace` と `assembly` の値は、カスタム コントロールの詳細と一致する必要があります。 名前空間が宣言されると、プレフィックスを使用してカスタム コントロールが参照されます。

次のコード例は、C# ページがどのように `NativeListView` カスタム コントロールを使用できるかを示しています。

```csharp
public class MainPageCS : ContentPage
{
    NativeListView nativeListView;

    public MainPageCS()
    {
        nativeListView = new NativeListView
        {
            Items = DataSource.GetList(),
            VerticalOptions = LayoutOptions.FillAndExpand
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

        Content = new Grid
        {
            RowDefinitions = {
                new RowDefinition { Height = GridLength.Auto },
                new RowDefinition { Height = new GridLength (1, GridUnitType.Star) }
            },
            Children = {
                new Label { Text = App.Description, HorizontalTextAlignment = TextAlignment.Center },
                nativeListView
            }
        };
        nativeListView.ItemSelected += OnItemSelected;
    }
    ...
}
```

`NativeListView` カスタム コントロールでは、プラットフォーム固有のカスタム レンダラーを使用して、`Items` プロパティ経由で設定されたデータのリストを表示します。 リストの各行には、名前、カテゴリ、イメージ ファイル名という 3 つの項目が含まれます。 リストの各行のレイアウトは、プラットフォーム固有のカスタム レンダラーによって定義されています。

> [!NOTE]
> `NativeListView` カスタム コントロールは、スクロール機能を含むプラットフォーム固有のリスト コントロールを使ってレンダリングされるため、カスタム コントロールは [`ScrollView`](xref:Xamarin.Forms.ScrollView) などのスクロール可能なレイアウト コントロール内でホストしてはいけません。

プラットフォーム固有のリスト コントロールとネイティブのセルのレイアウトを作成するために、カスタム レンダラーは、各アプリケーション プロジェクトに追加できるようになりました。

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>各プラットフォーム上でのカスタム レンダラーの作成

カスタム レンダラー クラスを作成するプロセスは次のとおりです。

1. カスタム コントロールをレンダリングする `ListViewRenderer` クラスのサブクラスを作成します。
1. カスタム コントロールをレンダリングする `OnElementChanged` メソッドをオーバーライドして、ロジックを書き込んでカスタマイズします。 対応する Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView) が作成されると、このメソッドが呼び出されます。
1. `ExportRenderer` 属性をカスタム レンダラー クラスに追加して、Xamarin.Forms カスタム コントロールのレンダリングに使用されるように指定します。 この属性は、Xamarin.Forms にカスタム レンダラーを登録するために使用されます。

> [!NOTE]
> プラットフォーム プロジェクトごとにカスタム レンダラーを指定するかどうかは任意です。 カスタム レンダラーが登録されていない場合は、セルの基底クラスの既定のレンダラーが使用されます。

次の図に、サンプル アプリケーション内の各プロジェクトの役割と、それらの関係を示します。

![](listview-images/solution-structure.png "NativeListView カスタム レンダラーのプロジェクトの役割")

`NativeListView` カスタム コントロールはプラットフォーム固有のレンダラー クラスによってレンダリングされます。このクラスはすべて各プラットフォームの `ListViewRenderer` クラスから派生しています。 この結果、次のスクリーンショットに示すように、プラットフォーム固有のリスト コントロールとネイティブ のセルのレイアウトを使用してそれぞれの `NativeListView` カスタム コントロールがレンダリングされます。

![](listview-images/screenshots.png "各プラットフォーム上の NativeListView")

`ListViewRenderer` クラスは `OnElementChanged` メソッドを公開します。このメソッドは、該当するネイティブ コントロールをレンダリングするために、Xamarin.Forms カスタム コントロールの作成時に呼び出されます。 このメソッドでは、`OldElement` および `NewElement` プロパティを含む `ElementChangedEventArgs` パラメーターを受け取ります。 これらのプロパティは、レンダラーがアタッチされて*いた* Xamarin.Forms 要素と、レンダラーが現在アタッチされて*いる* Xamarin.Forms 要素をそれぞれ表しています。 サンプル アプリケーションでは、`OldElement` プロパティが `null` になり、`NewElement` プロパティに `NativeListView` インスタンスへの参照が含まれます。

各プラットフォーム固有のレンダラー クラス内の、オーバーライドされたバージョンの `OnElementChanged` メソッドは、ネイティブ コントロールのカスタマイズを行う場所です。 プラットフォーム上で使用されているネイティブ コントロールへの型指定された参照には、`Control` プロパティを使用してアクセスすることができます。 さらに、レンダリングされている Xamarin.Forms コントロールへの参照は、`Element` プロパティを使用して取得することができます。

次のコード例に示すように、`OnElementChanged` メソッドでイベント ハンドラーをサブスクライブする場合は注意が必要です。

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<Xamarin.Forms.ListView> e)
{
  base.OnElementChanged (e);

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the native control and subscribe to event handlers
  }
}
```

カスタム レンダラーが新しい Xamarin.Forms 要素に関連付けられるときにのみ、ネイティブ コントロールを設定し、イベント ハンドラーを登録する必要があります。 同様に、レンダラーがアタッチされている要素が変わるときにのみ、サブスクライブしていたイベント ハンドラーのサブスクライブをすべて解除する必要があります。 この手法を採用することは、メモリ リークが発生しないカスタム レンダラーの作成に役立ちます。

各プラットフォーム固有のレンダラー クラス内でオーバーライドされたバージョンの `OnElementPropertyChanged` メソッドは、Xamarin.Forms カスタム コントロール上でバインド可能なプロパティの変更に応答する場所です。 このオーバーライドは何度も呼び出されることがあるため、変更になったプロパティのチェックは常に行われる必要があります。

各カスタム レンダラー クラスは、レンダラーを Xamarin.Forms に登録する `ExportRenderer` 属性で修飾されます。 この属性は、レンダリングされている Xamarin.Forms カスタム コントロールの種類名と、カスタム レンダラーの種類名という 2 つのパラメーターを受け取ります。 属性の `assembly` プレフィックスでは、属性がアセンブリ全体に適用されることを指定します。

次のセクションで、各プラットフォーム固有のカスタム レンダラー クラスの実装について説明します。

### <a name="creating-the-custom-renderer-on-ios"></a>iOS 上でのカスタム レンダラーの作成

次のコード例は、iOS プラットフォーム用のカスタム レンダラーを示しています。

```csharp
[assembly: ExportRenderer (typeof(NativeListView), typeof(NativeiOSListViewRenderer))]
namespace CustomRenderer.iOS
{
    public class NativeiOSListViewRenderer : ListViewRenderer
    {
        protected override void OnElementChanged (ElementChangedEventArgs<Xamarin.Forms.ListView> e)
        {
            base.OnElementChanged (e);

            if (e.OldElement != null) {
                // Unsubscribe
            }

            if (e.NewElement != null) {
                Control.Source = new NativeiOSListViewSource (e.NewElement as NativeListView);
            }
        }
    }
}
```

カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされている場合、`NativeiOSListViewSource` クラスのインスタンスを作成することによって、`UITableView` コントロールが構成されます。 このクラスは、`UITableViewSource` クラスから `RowsInSection` および `GetCell` メソッドをオーバーライドし、表示されるデータのリストを含む `Items` プロパティを公開することで、`UITableView` コントロールにデータを提供します。 また、クラスでは、`NativeListView` カスタム コントロールによって提供された `ItemSelected` イベントを呼び出す `RowSelected` メソッドのオーバーライドも提供します。 メソッドのオーバーライドの詳細については、「[Subclassing UITableViewSource](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)」(UITableViewSource のサブクラス化) を参照してください。 `GetCell` メソッドは、次のコード例に示すように、リスト内の各行にデータが設定された `UITableCellView` を返します。

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
  // request a recycled cell to save memory
  NativeiOSListViewCell cell = tableView.DequeueReusableCell (cellIdentifier) as NativeiOSListViewCell;

  // if there are no cells to reuse, create a new one
  if (cell == null) {
    cell = new NativeiOSListViewCell (cellIdentifier);
  }

  if (String.IsNullOrWhiteSpace (tableItems [indexPath.Row].ImageFilename)) {
    cell.UpdateCell (tableItems [indexPath.Row].Name
      , tableItems [indexPath.Row].Category
      , null);
  } else {
    cell.UpdateCell (tableItems [indexPath.Row].Name
      , tableItems [indexPath.Row].Category
      , UIImage.FromFile ("Images/" + tableItems [indexPath.Row].ImageFilename + ".jpg"));
  }

  return cell;
}
```

このメソッドは、画面上に表示されるデータの各行に対して `NativeiOSListViewCell` インスタンスを作成します。 `NativeiOSCell` インスタンスでは、各セルのレイアウトとそのセルのデータを定義します。 スクロールによってセルが画面に表示されなくなると、そのセルは再利用可能になります。 これにより、リスト内のすべてのデータではなく、画面上に表示されているデータに対する `NativeiOSCell` インスタンスだけがある状態を保証することで、メモリの無駄遣いを防止しています。 セルの再利用に関する詳細については、「[セルの再利用](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)」を参照してください。 また、行のデータ (名前、カテゴリ、およびイメージ) で `NativeiOSListViewCell` インスタンスを更新する前に、`ImageFilename` プロパティが存在し、イメージを読み取って `UIImage` インスタンスとして格納している場合、`GetCell` メソッドは、データの各行のこのプロパティを読み取ります。

`NativeiOSListViewCell` クラスでは、次のコード例に示すように、各セルのレイアウトを定義します。

```csharp
public class NativeiOSListViewCell : UITableViewCell
{
  UILabel headingLabel, subheadingLabel;
  UIImageView imageView;

  public NativeiOSListViewCell (NSString cellId) : base (UITableViewCellStyle.Default, cellId)
  {
    SelectionStyle = UITableViewCellSelectionStyle.Gray;

    ContentView.BackgroundColor = UIColor.FromRGB (218, 255, 127);

    imageView = new UIImageView ();

    headingLabel = new UILabel () {
      Font = UIFont.FromName ("Cochin-BoldItalic", 22f),
      TextColor = UIColor.FromRGB (127, 51, 0),
      BackgroundColor = UIColor.Clear
    };

    subheadingLabel = new UILabel () {
      Font = UIFont.FromName ("AmericanTypewriter", 12f),
      TextColor = UIColor.FromRGB (38, 127, 0),
      TextAlignment = UITextAlignment.Center,
      BackgroundColor = UIColor.Clear
    };

    ContentView.Add (headingLabel);
    ContentView.Add (subheadingLabel);
    ContentView.Add (imageView);
  }

  public void UpdateCell (string caption, string subtitle, UIImage image)
  {
    headingLabel.Text = caption;
    subheadingLabel.Text = subtitle;
    imageView.Image = image;
  }

  public override void LayoutSubviews ()
  {
    base.LayoutSubviews ();

    headingLabel.Frame = new CoreGraphics.CGRect (5, 4, ContentView.Bounds.Width - 63, 25);
    subheadingLabel.Frame = new CoreGraphics.CGRect (100, 18, 100, 20);
    imageView.Frame = new CoreGraphics.CGRect (ContentView.Bounds.Width - 63, 5, 33, 33);
  }
}
```

このクラスでは、セルのコンテンツとレイアウトのレンダリングに使用されるコントロールを定義します。 `NativeiOSListViewCell` コンストラクターは、`UILabel` および `UIImageView` コントロールのインスタンスを作成し、その外観を初期化します。 これらのコントロールは各行のデータを表示するために利用され、`UpdateCell` メソッドを使用して `UILabel` および `UIImageView` インスタンス上にこのデータを設定します。 これらのインスタンスの場所は、オーバーライドされた `LayoutSubviews` メソッドによって、セル内での座標を指定することで設定されます。

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>カスタム コントロールでのプロパティ変更への応答

リストに対して項目が追加または削除されたために `NativeListView.Items` プロパティが変更された場合、カスタム レンダラーは変更を表示して応答する必要があります。 次のコード例に示すように、これは `OnElementPropertyChanged` メソッドをオーバーライドすることによって実現できます。

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Source = new NativeiOSListViewSource (Element as NativeListView);
  }
}
```

バインド可能な `NativeListView.Items` プロパティが変更された場合、メソッドでは、データを `UITableView` コントロールに提供する `NativeiOSListViewSource` クラスの新しいインスタンスを作成します。

### <a name="creating-the-custom-renderer-on-android"></a>Android 上でのカスタム レンダラーの作成

次のコード例は、Android プラットフォーム用のカスタム レンダラーを示しています。

```csharp
[assembly: ExportRenderer(typeof(NativeListView), typeof(NativeAndroidListViewRenderer))]
namespace CustomRenderer.Droid
{
    public class NativeAndroidListViewRenderer : ListViewRenderer
    {
        Context _context;

        public NativeAndroidListViewRenderer(Context context) : base(context)
        {
            _context = context;
        }

        protected override void OnElementChanged(ElementChangedEventArgs<Xamarin.Forms.ListView> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // unsubscribe
                Control.ItemClick -= OnItemClick;
            }

            if (e.NewElement != null)
            {
                // subscribe
                Control.Adapter = new NativeAndroidListViewAdapter(_context as Android.App.Activity, e.NewElement as NativeListView);
                Control.ItemClick += OnItemClick;
            }
        }
        ...

        void OnItemClick(object sender, Android.Widget.AdapterView.ItemClickEventArgs e)
        {
            ((NativeListView)Element).NotifyItemSelected(((NativeListView)Element).Items.ToList()[e.Position - 1]);
        }
    }
}
```

カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされている場合、ネイティブ `ListView` コントロールが構成されます。 この構成には、データをネイティブ `ListView` コントロールに提供する `NativeAndroidListViewAdapter` クラスのインスタンスを作成し、`ItemClick` イベントを処理するベント ハンドラーを登録する手順を伴います。 その後、このハンドラーは、`NativeListView` カスタム コントロールによって提供される `ItemSelected` イベントを呼び出します。 レンダラーがアタッチされている Xamarin.Forms 要素が変更された場合に、`ItemClick` イベントのサブスクライブが解除されます。

`NativeAndroidListViewAdapter` は `BaseAdapter` クラスから派生し、`Count`、`GetView`、`GetItemId`、および `this[int]` メソッドをオーバーライドするだけでなく、表示されるデータのリストを含む `Items` プロパティを公開します。 これらのメソッドのオーバーライドの詳細については、「[Implementing a ListAdapter](~/android/user-interface/layouts/list-view/populating.md)」(ListAdapter の実装) に関するページを参照してください。 `GetView` メソッドは、次のコード例に示すように、データが設定された各行のビューを返します。

```csharp
public override View GetView (int position, View convertView, ViewGroup parent)
{
  var item = tableItems [position];

  var view = convertView;
  if (view == null) {
    // no view to re-use, create new
    view = context.LayoutInflater.Inflate (Resource.Layout.NativeAndroidListViewCell, null);
  }
  view.FindViewById<TextView> (Resource.Id.Text1).Text = item.Name;
  view.FindViewById<TextView> (Resource.Id.Text2).Text = item.Category;

  // grab the old image and dispose of it
  if (view.FindViewById<ImageView> (Resource.Id.Image).Drawable != null) {
    using (var image = view.FindViewById<ImageView> (Resource.Id.Image).Drawable as BitmapDrawable) {
      if (image != null) {
        if (image.Bitmap != null) {
          //image.Bitmap.Recycle ();
          image.Bitmap.Dispose ();
        }
      }
    }
  }

  // If a new image is required, display it
  if (!String.IsNullOrWhiteSpace (item.ImageFilename)) {
    context.Resources.GetBitmapAsync (item.ImageFilename).ContinueWith ((t) => {
      var bitmap = t.Result;
      if (bitmap != null) {
        view.FindViewById<ImageView> (Resource.Id.Image).SetImageBitmap (bitmap);
        bitmap.Dispose ();
      }
    }, TaskScheduler.FromCurrentSynchronizationContext ());
  } else {
    // clear the image
    view.FindViewById<ImageView> (Resource.Id.Image).SetImageBitmap (null);
  }

  return view;
}
```

`GetView` メソッドは、リスト内のデータの各行に対応する、レンダリングされるセルを `View` として返すために、呼び出されます。 レイアウト ファイルに定義された `View` インスタンスの外観を利用して、画面上に表示される各行のデータの `View` インスタンスを作成します。 スクロールによってセルが画面に表示されなくなると、そのセルは再利用可能になります。 これにより、リスト内のすべてのデータではなく、画面上に表示されているデータに対する `View` インスタンスだけがある状態を保証することで、メモリの無駄遣いを防止しています。 ビューの再利用の詳細については、「[Row View Re-use](~/android/user-interface/layouts/list-view/populating.md)」(行ビューの再利用) を参照してください。

また、`GetView` メソッドは、`ImageFilename` プロパティに指定されたファイル名からイメージ データを読み取るなど、`View` インスタンスにデータを設定します。

ネイティブな `ListView` によって表示された各セルのレイアウトは、`LayoutInflater.Inflate` メソッドによって拡張される `NativeAndroidListViewCell.axml` レイアウト ファイル内に定義されています。 レイアウト定義を次のコード サンプルに示します。

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
            android:id="@+id/Text1"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textColor="#FF7F3300"
            android:textSize="20dip"
            android:textStyle="italic" />
        <TextView
            android:id="@+id/Text2"
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

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>カスタム コントロールでのプロパティ変更への応答

リストに対して項目が追加または削除されたために `NativeListView.Items` プロパティが変更された場合、カスタム レンダラーは変更を表示して応答する必要があります。 次のコード例に示すように、これは `OnElementPropertyChanged` メソッドをオーバーライドすることによって実現できます。

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Adapter = new NativeAndroidListViewAdapter (_context as Android.App.Activity, Element as NativeListView);
  }
}
```

このメソッドでは、バインド可能な `NativeListView.Items` プロパティが変更された場合、メソッドでは、データをネイティブ `ListView` コントロールに提供する `NativeAndroidListViewAdapter` クラスの新しいインスタンスを作成します。

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP 上でのカスタム レンダラーの作成

次のコード例は、UWP 用のカスタム レンダラーを示します。

```csharp
[assembly: ExportRenderer(typeof(NativeListView), typeof(NativeUWPListViewRenderer))]
namespace CustomRenderer.UWP
{
    public class NativeUWPListViewRenderer : ListViewRenderer
    {
        ListView listView;

        protected override void OnElementChanged(ElementChangedEventArgs<Xamarin.Forms.ListView> e)
        {
            base.OnElementChanged(e);

            listView = Control as ListView;

            if (e.OldElement != null)
            {
                // Unsubscribe
                listView.SelectionChanged -= OnSelectedItemChanged;
            }

            if (e.NewElement != null)
            {
                listView.SelectionMode = ListViewSelectionMode.Single;
                listView.IsItemClickEnabled = false;
                listView.ItemsSource = ((NativeListView)e.NewElement).Items;             
                listView.ItemTemplate = App.Current.Resources["ListViewItemTemplate"] as Windows.UI.Xaml.DataTemplate;
                // Subscribe
                listView.SelectionChanged += OnSelectedItemChanged;
            }  
        }

        void OnSelectedItemChanged(object sender, SelectionChangedEventArgs e)
        {
            ((NativeListView)Element).NotifyItemSelected(listView.SelectedItem);
        }
    }
}
```

カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされている場合、ネイティブ `ListView` コントロールが構成されます。 この構成では、コントロールで表示されるデータを入力し、各セルの外観とコンテンツを定義し、`SelectionChanged` イベントを処理するイベント ハンドラーを登録して、選択されている項目にネイティブ `ListView` コントロールが応答するための設定を行います。 その後、このハンドラーは、`NativeListView` カスタム コントロールによって提供される `ItemSelected` イベントを呼び出します。 レンダラーがアタッチされている Xamarin.Forms 要素が変更された場合に、`SelectionChanged` イベントのサブスクライブが解除されます。

各ネイティブ `ListView` セルの外観とコンテンツは、`ListViewItemTemplate` という名前の `DataTemplate` によって定義されています。 この `DataTemplate` は、次のコード例に示すように、アプリケーション レベルのリソース ディクショナリに格納されています。

```xaml
<DataTemplate x:Key="ListViewItemTemplate">
    <Grid Background="#DAFF7F">
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

`DataTemplate` では、レイアウトと外観と共に、セルのコンテンツの表示に使用されるコントロールを指定します。 データ バインディングを利用してセルのコンテンツを表示するために、2 つの `TextBlock` コントロールと `Image` コントロールが使用されます。 さらに、`.jpg` ファイルの拡張子を各イメージ ファイル名に付与するために、`ConcatImageExtensionConverter` のインスタンスが使用されます。 これにより、`Source` プロパティが設定された場合、`Image` コントロールでは、イメージの読み込みとレンダリングを確実に行うことができます。

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>カスタム コントロールでのプロパティ変更への応答

リストに対して項目が追加または削除されたために `NativeListView.Items` プロパティが変更された場合、カスタム レンダラーは変更を表示して応答する必要があります。 次のコード例に示すように、これは `OnElementPropertyChanged` メソッドをオーバーライドすることによって実現できます。

```csharp
protected override void OnElementPropertyChanged(object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
    base.OnElementPropertyChanged(sender, e);

    if (e.PropertyName == NativeListView.ItemsProperty.PropertyName)
    {
        listView.ItemsSource = ((NativeListView)Element).Items;
    }
}
```

バインド可能な `NativeListView.Items` プロパティが変更された場合、このメソッドでは、変更後のデータを使ってネイティブ `ListView` コントロールを再設定します。

## <a name="summary"></a>まとめ

この記事では、プラットフォーム固有のリスト コントロールとネイティブのセルのレイアウトをカプセル化するカスタム レンダラーを作成し、ネイティブ リスト コントロールのパフォーマンスをより厳密に制御する方法を示しました。


## <a name="related-links"></a>関連リンク

- [CustomRendererListView (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/listview/)
