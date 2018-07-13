---
title: ListView のカスタマイズ
description: Xamarin.Forms の ListView は、データのコレクションを垂直方向の一覧として表示するビューです。 この記事では、カスタム レンダラーをプラットフォーム固有のリスト コントロールとネイティブのセルのレイアウトをカプセル化するネイティブ リスト コントロールのパフォーマンスをさらに制御できるように作成する方法を示します。
ms.prod: xamarin
ms.assetid: 2FBCB8C8-4F32-45E7-954F-63AD29D5F1B5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: b3b73d542faebdb8ab85c989d7812368f4f3ffac
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997490"
---
# <a name="customizing-a-listview"></a>ListView のカスタマイズ

_Xamarin.Forms の ListView は、データのコレクションを垂直方向の一覧として表示するビューです。この記事では、カスタム レンダラーをプラットフォーム固有のリスト コントロールとネイティブのセルのレイアウトをカプセル化するネイティブ リスト コントロールのパフォーマンスをさらに制御できるように作成する方法を示します。_

すべての Xamarin.Forms のビューには、ネイティブ コントロールのインスタンスを作成する各プラットフォームの付属のレンダラーがあります。 ときに、 [ `ListView` ](xref:Xamarin.Forms.ListView) iOS での Xamarin.Forms アプリケーションによって表示される、`ListViewRenderer`クラスをインスタンス化がさらにインスタンス化をネイティブ`UITableView`コントロール。 Android のプラットフォームで、`ListViewRenderer`クラスのインスタンスを作成、ネイティブ`ListView`コントロール。 ユニバーサル Windows プラットフォーム (UWP) で、`ListViewRenderer`クラスのインスタンスを作成、ネイティブ`ListView`コントロール。 レンダラーと Xamarin.Forms コントロールにマップするネイティブ コントロール クラスの詳細については、次を参照してください。[レンダラーの基本クラスおよびネイティブ コントロール](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)します。

次の図の間のリレーションシップを示しています、 [ `ListView` ](xref:Xamarin.Forms.ListView)コントロールとそれを実装するネイティブ コントロールの対応します。

![](listview-images/listview-classes.png "ListView コントロールと実装のネイティブ コントロール間のリレーションシップ")

レンダリング プロセスに実行できる活用用のカスタム レンダラーを作成してプラットフォーム固有のカスタマイズを実装する[ `ListView` ](xref:Xamarin.Forms.ListView)各プラットフォームで。 これを行うためのプロセスは次のとおりです。

1. [作成](#Creating_the_Custom_ListView_Control)Xamarin.Forms カスタム コントロール。
1. [消費](#Consuming_the_Custom_Control)Xamarin.Forms からカスタム コントロール。
1. [作成](#Creating_the_Custom_Renderer_on_each_Platform)各プラットフォームでコントロールのカスタム レンダラーです。

各項目が実装するためにさらに、説明するようになりましたが、`NativeListView`プラットフォームに固有のリスト コントロールとネイティブのセルのレイアウトを活用したレンダラーです。 このシナリオは、リストを含む既存のネイティブ アプリと再利用できるセルのコードを移植するときに便利です。 さらに、データ仮想化などのパフォーマンスに影響を与えるリスト コントロールの機能の詳細なカスタマイズができます。

<a name="Creating_the_Custom_ListView_Control" />

## <a name="creating-the-custom-listview-control"></a>カスタム ListView コントロールを作成します。

カスタム[ `ListView` ](xref:Xamarin.Forms.ListView)をサブクラス化してコントロールを作成することができます、`ListView`クラスに、次のコード例に示すようにします。

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

`NativeListView`は .NET Standard ライブラリ プロジェクトで作成され、カスタム コントロールの API を定義します。 このコントロールは、公開、`Items`プロパティを設定するために使用される、 `ListView` for にバインドされたデータの表示目的で、データとなることができます。 また、公開、`ItemSelected`プラットフォームに固有のネイティブ リスト コントロールの項目が選択されるたびに起動されるイベント。 データ バインディングの詳細については、「[Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)」 (データ バインディングの基礎) を参照してください。

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>カスタム コントロールの使用

`NativeListView`カスタム コントロールを参照できます Xaml で .NET Standard ライブラリ プロジェクトの場所の名前空間を宣言して、コントロールの名前空間プレフィックスを使用します。 次のコード例に示す方法、 `NativeListView` XAML ページで、カスタム コントロールを使用できます。

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

`local`何も名前空間プレフィックスを付けることができます。 ただし、`clr-namespace`と`assembly`値は、カスタム コントロールの詳細と一致する必要があります。 名前空間が宣言されると、プレフィックスを使用して、カスタム コントロールを参照します。

次のコード例に示す方法、 `NativeListView` (C#) ページで、カスタム コントロールを使用できます。

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

`NativeListView`カスタム コントロールを介して作成される、データの一覧を表示するプラットフォーム固有のカスタム レンダラーを使用して、`Items`プロパティ。 リスト内の各行には、名前、カテゴリ、およびイメージのファイル名 – データの 3 つの項目が含まれています。 リスト内の各行のレイアウトは、プラットフォーム固有のカスタム レンダラーによって定義されます。

> [!NOTE]
> `NativeListView`スクロール機能を含むプラットフォーム固有のリスト コントロールを使用してカスタム コントロールをレンダリングする、カスタム コントロールをなど、スクロール可能なレイアウト コントロールでホストするされません、 [ `ScrollView`](xref:Xamarin.Forms.ScrollView)します。

カスタム レンダラーは、プラットフォーム固有のリスト コントロールとネイティブのセルのレイアウトを作成するには、各アプリケーション プロジェクトを今すぐ追加できます。

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>各プラットフォームでのカスタム レンダラーの作成

カスタム レンダラー クラスを作成するプロセスは次のとおりです。

1. サブクラスを作成、`ListViewRenderer`カスタム コントロールをレンダリングするクラス。
1. 上書き、`OnElementChanged`それをカスタマイズするカスタム コントロールと書き込みロジックをレンダリングするメソッド。 このメソッドが呼び出されます、対応する Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView)が作成されます。
1. 追加、`ExportRenderer`属性をカスタム レンダラー クラスは、Xamarin.Forms カスタム コントロールを表示するために使用するように指定します。 この属性は、Xamarin.Forms でのカスタム レンダラーの登録に使用されます。

> [!NOTE]
> 各プラットフォーム プロジェクトにカスタム レンダラーを提供する省略可能になります。 カスタム レンダラーが登録されていない場合は、セルの基本クラスの既定のレンダラーが使用されます。

次の図は、サンプル アプリケーションとそれらの間のリレーションシップ内の各プロジェクトの役割を示します。

![](listview-images/solution-structure.png "NativeListView カスタム レンダラーのプロジェクトの責任")

`NativeListView`でプラットフォーム固有のレンダラー クラスから派生するカスタム コントロールが表示される、`ListViewRenderer`各プラットフォームのクラス。 これは、結果、各`NativeListView`の次のスクリーン ショットに示すようにプラットフォーム固有のリスト コントロールとネイティブのセルのレイアウト、レンダリングされるカスタム コントロール。

![](listview-images/screenshots.png "各プラットフォームで NativeListView")

`ListViewRenderer`クラスでは、`OnElementChanged`メソッドで、Xamarin.Forms カスタム コントロールが、対応するネイティブ コントロールを表示するために作成されたときに呼び出されます。 このメソッドは、`ElementChangedEventArgs`を含むパラメーター`OldElement`と`NewElement`プロパティ。 これらのプロパティは、Xamarin.Forms 要素を表すをレンダラー*が*に接続されていると Xamarin.Forms 要素をレンダラー*は*に、それぞれに接続されています。 サンプル アプリケーションで、`OldElement`プロパティになります`null`と`NewElement`プロパティへの参照には、`NativeListView`インスタンス。

オーバーライドされたバージョン、`OnElementChanged`各プラットフォームに固有のレンダラー クラスのメソッドはネイティブ コントロールのカスタマイズを実行する場所です。 を介してアクセスできる、プラットフォームで使用されているネイティブ コントロールへの参照を型指定された、`Control`プロパティ。 さらでレンダリングされている Xamarin.Forms コントロールへの参照を取得できます、`Element`プロパティ。

内のイベント ハンドラーをサブスクライブしているときは注意する必要がある、`OnElementChanged`メソッドは、次のコード例で示した。

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

ネイティブ コントロールで構成するようにし、カスタム レンダラーが新しい Xamarin.Forms 要素に関連付けられている場合は、イベント ハンドラーをサブスクライブします。 同様に、サブスクライブされたイベント ハンドラーがサブスクリプションを解除する要素は、レンダラーが変更に関連付けられている場合にのみです。 このアプローチを採用すると、メモリ リークが発生しませんが、カスタム レンダラーを作成するのに役立ちます。

オーバーライドされたバージョン、`OnElementPropertyChanged`各プラットフォームに固有のレンダラー クラスのメソッドは、Xamarin.Forms カスタム コントロールにバインド可能なプロパティの変更に対応するためです。 この上書きは、何度も呼び出すことが、変更されているプロパティのチェックを行った常にする必要があります。

各カスタム レンダラー クラスで修飾された、`ExportRenderer`レンダラーを Xamarin.Forms で登録される属性。 属性は、– 表示するには、Xamarin.Forms カスタム コントロールの型名と、カスタム レンダラーの種類の名前の 2 つのパラメーターを受け取ります。 `assembly`属性にプレフィックスは、属性がアセンブリ全体に適用されることを指定します。

次のセクションでは、各プラットフォームに固有のカスタム レンダラー クラスの実装について説明します。

### <a name="creating-the-custom-renderer-on-ios"></a>IOS でのカスタム レンダラーの作成

次のコード例では、iOS プラットフォーム用のカスタム レンダラーを示します。

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

`UITableView`コントロールがのインスタンスを作成して構成されている、`NativeiOSListViewSource`クラス、カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされていること。 このクラスにデータを提供する、`UITableView`オーバーライドすることでコントロール、`RowsInSection`と`GetCell`メソッドを`UITableViewSource`クラス、および公開することによって、`Items`プロパティを表示するデータの一覧が含まれています。 クラスも用意されています。、`RowSelected`メソッドのオーバーライドを呼び出す、`ItemSelected`によって提供されるイベント、`NativeListView`カスタム コントロール。 メソッドの詳細を上書きするを参照してください。[サブクラス化 UITableViewSource](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)します。 `GetCell`メソッドを返します。 を`UITableCellView`次のコード例に示したを一覧で、各行のデータが表示されます。

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

このメソッドを作成、`NativeiOSListViewCell`画面に表示されるデータの各行のインスタンス。 `NativeiOSCell`インスタンスは、各セルとセルのデータのレイアウトを定義します。 セルは、スクロールのための画面からが消える、ときに、セルになります再利用可能です。 のみがあることを確認してメモリは無駄使いを回避できます`NativeiOSCell`リスト内のデータのすべてではなく、画面に表示されているデータのインスタンス。 セルの再利用の詳細については、次を参照してください。[セルが再利用](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)します。 `GetCell`メソッドは読み取りも、`ImageFilename`をそれが存在して、画像を読み込みますおよび保存として提供されているデータの各行のプロパティを`UIImage`インスタンス、更新する前に、 `NativeiOSListViewCell` (名前、カテゴリ、およびイメージ) のデータのインスタンス。行。

`NativeiOSListViewCell`クラスが、各セルのレイアウトを定義し、次のコード例に示した。

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

このクラスは、セルの内容、およびそれらのレイアウトを表示するために使用されているコントロールを定義します。 `NativeiOSListViewCell`コンス トラクターのインスタンスを作成する`UILabel`と`UIImageView`コントロール、およびそれらの外観を初期化します。 これらのコントロールを使用すると、すべての行のデータを表示、`UpdateCell`メソッドでこのデータを設定するために使用される、`UILabel`と`UIImageView`インスタンス。 これらのインスタンスの場所を設定して、オーバーライドされた`LayoutSubviews`セル内の座標を指定することで、メソッド。

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>カスタム コントロールのプロパティの変更に応答してください。

場合、`NativeListView.Items`カスタム レンダラーは、変更内容を表示して応答する必要があります、プロパティが項目に追加されるため、変更、または一覧から削除します。 これは、オーバーライドすることで実現できます、`OnElementPropertyChanged`メソッドは、次のコード例に示されています。

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Source = new NativeiOSListViewSource (Element as NativeListView);
  }
}
```

メソッドの新しいインスタンスを作成し、`NativeiOSListViewSource`データを提供するクラス、`UITableView`コントロールを提供する、バインド可能な`NativeListView.Items`プロパティが変更されました。

### <a name="creating-the-custom-renderer-on-android"></a>Android でのカスタム レンダラーの作成

次のコード例では、Android プラットフォーム用のカスタム レンダラーを示します。

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

ネイティブ`ListView`カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされているコントロールが構成されます。 この構成では、インスタンスを作成する必要があります、`NativeAndroidListViewAdapter`データをネイティブに提供するクラス`ListView`制御、および処理するイベント ハンドラーを登録、`ItemClick`イベント。 このハンドラーを呼び出しますが、さらに、`ItemSelected`によって提供されるイベント、`NativeListView`カスタム コントロール。 `ItemClick` Xamarin.Forms 要素は、レンダラーが変更にアタッチされている場合、イベントを購読解除します。

`NativeAndroidListViewAdapter`から派生した、`BaseAdapter`クラスと公開、`Items`プロパティを表示するデータの一覧を含むようオーバーライド、 `Count`、 `GetView`、 `GetItemId`、および`this[int]`メソッド。 これらのメソッド オーバーライドの詳細については、次を参照してください。 [、ListAdapter を実装する](~/android/user-interface/layouts/list-view/populating.md)します。 `GetView`メソッドは、行ごとにデータを含むビューを返し、次のコード例に示します。

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

`GetView`をレンダリングするセルを返すメソッドが呼び出されるとして、`View`リスト内のデータの行ごとにします。 作成、`View`の外観を備えた、画面に表示されるデータの各行のインスタンス、`View`レイアウト ファイルで定義されているインスタンス。 セルは、スクロールのための画面からが消える、ときに、セルになります再利用可能です。 のみがあることを確認してメモリは無駄使いを回避できます`View`リスト内のデータのすべてではなく、画面に表示されているデータのインスタンス。 ビューの再利用の詳細については、次を参照してください。[行ビューを再利用](~/android/user-interface/layouts/list-view/populating.md)します。

`GetView`メソッドも作成されます、`View`で指定されたファイル名から画像データの読み込みなどのデータ インスタンス、`ImageFilename`プロパティ。

ネイティブでは、各セル dispayed のレイアウト`ListView`で定義されている、`NativeAndroidListViewCell.axml`膨らませるレイアウト ファイル、`LayoutInflater.Inflate`メソッド。 次のコード例では、レイアウト定義を示します。

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

このレイアウトは、その 2 つを指定`TextView`コントロールと`ImageView`コントロールがセルの内容を表示するために使用します。 2 つ`TextView`コントロールは垂直方向に内で、`LinearLayout`内に含まれているすべてのコントロールのコントロール、`RelativeLayout`します。

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>カスタム コントロールのプロパティの変更に応答してください。

場合、`NativeListView.Items`カスタム レンダラーは、変更内容を表示して応答する必要があります、プロパティが項目に追加されるため、変更、または一覧から削除します。 これは、オーバーライドすることで実現できます、`OnElementPropertyChanged`メソッドは、次のコード例に示されています。

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Adapter = new NativeAndroidListViewAdapter (_context as Android.App.Activity, Element as NativeListView);
  }
}
```

メソッドの新しいインスタンスを作成し、`NativeAndroidListViewAdapter`データをネイティブに提供するクラス`ListView`コントロールを提供する、バインド可能な`NativeListView.Items`プロパティが変更されました。

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP のカスタム レンダラーを作成します。

次のコード例では、UWP 用のカスタム レンダラーを示します。

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

ネイティブ`ListView`カスタム レンダラーが新しい Xamarin.Forms 要素にアタッチされているコントロールが構成されます。 この構成設定は、どのネイティブ`ListView`コントロールが選択されている項目に応答が、各セルの内容と外観を定義して、を処理するイベントハンドラーを登録、コントロールによって表示されるデータの設定`SelectionChanged`イベント。 このハンドラーを呼び出しますが、さらに、`ItemSelected`によって提供されるイベント、`NativeListView`カスタム コントロール。 `SelectionChanged` Xamarin.Forms 要素は、レンダラーが変更にアタッチされている場合、イベントを購読解除します。

各ネイティブの内容と外観`ListView`セルがによって定義されている、`DataTemplate`という`ListViewItemTemplate`。 これは、`DataTemplate`は、アプリケーション レベルのリソース ディクショナリに格納され、次のコード例に示した。

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

`DataTemplate`セル、およびレイアウトと外観の内容を表示するために使用するコントロールを指定します。 2 つ`TextBlock`コントロールと`Image`コントロールがデータ バインドによって、セルの内容を表示するために使用します。 さらのインスタンス、`ConcatImageExtensionConverter`連結するために使用、`.jpg`ファイルに各イメージのファイル名拡張子。 これにより、`Image`コントロールの読み込みし、なったときに、イメージのレンダリング`Source`プロパティを設定します。

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>カスタム コントロールのプロパティの変更に応答してください。

場合、`NativeListView.Items`カスタム レンダラーは、変更内容を表示して応答する必要があります、プロパティが項目に追加されるため、変更、または一覧から削除します。 これは、オーバーライドすることで実現できます、`OnElementPropertyChanged`メソッドは、次のコード例に示されています。

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

メソッドは、ネイティブを再設定`ListView`コントロール、変更されたデータを提供する、バインド可能な`NativeListView.Items`プロパティが変更されました。

## <a name="summary"></a>まとめ

この記事では、カスタム レンダラーをプラットフォーム固有のリスト コントロールとネイティブのセルのレイアウトをカプセル化するネイティブ リスト コントロールのパフォーマンスをさらに制御できるように作成する方法について説明しました。


## <a name="related-links"></a>関連リンク

- [CustomRendererListView (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/listview/)
