---
title: "ListView のカスタマイズ"
description: "Xamarin.Forms ListView は、垂直方向の一覧としてデータのコレクションを表示するビューです。 この記事では、カスタム レンダラーを作成のプラットフォーム固有のリスト コントロールとネイティブのセルのレイアウトをカプセル化するネイティブのリスト コントロールのパフォーマンスをより細かく制御できるようにする方法を示します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2FBCB8C8-4F32-45E7-954F-63AD29D5F1B5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: eb4cf0285585351db5c45dc34a382236e6805c99
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="customizing-a-listview"></a>ListView のカスタマイズ

_Xamarin.Forms ListView は、垂直方向の一覧としてデータのコレクションを表示するビューです。この記事では、カスタム レンダラーを作成のプラットフォーム固有のリスト コントロールとネイティブのセルのレイアウトをカプセル化するネイティブのリスト コントロールのパフォーマンスをより細かく制御できるようにする方法を示します。_

各 Xamarin.Forms ビューには、ネイティブなコントロールのインスタンスを作成する各プラットフォームの付属のレンダラーがあります。 ときに、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) iOS 内の Xamarin.Forms アプリケーションによって表示される、`ListViewRenderer`クラスをインスタンス化、これがインスタンス化ネイティブ`UITableView`コントロール。 Android のプラットフォームでは、`ListViewRenderer`クラスのインスタンスを作成、ネイティブな`ListView`コントロール。 Windows Phone でユニバーサル Windows プラットフォーム (UWP)、`ListViewRenderer`クラスのインスタンスを作成、ネイティブな`ListView`コントロール。 レンダラーと Xamarin.Forms のコントロールにマップするネイティブ コントロール クラスの詳細については、次を参照してください。[レンダラー基底クラスとネイティブ コントロール](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)です。

次の図の間のリレーションシップを示しています、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)コントロールおよびそれを実装する対応するネイティブ コントロール。

![](listview-images/listview-classes.png "ListView コントロールと実装のネイティブ コントロール間のリレーションシップ")

表示処理に実行できるの利点のカスタム レンダラーを作成することで、プラットフォーム固有のカスタマイズ設定を実装する、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)プラットフォームごとにします。 これを行うためのプロセスは次のとおりです。

1. [作成](#Creating_the_Custom_ListView_Control)Xamarin.Forms のカスタム コントロールです。
1. [消費](#Consuming_the_Custom_Control)Xamarin.Forms からカスタム コントロールです。
1. [作成](#Creating_the_Custom_Renderer_on_each_Platform)各プラットフォームでコントロールのカスタム レンダラーです。

各項目を実装する順番に説明するようになりましたが、`NativeListView`レンダラーのプラットフォーム固有のリスト コントロールとネイティブのセルのレイアウトを利用します。 このシナリオは、一覧を含む既存のネイティブ アプリと再利用できるセルのコードを移植するときに便利です。 さらに、データの仮想化などのパフォーマンスに影響を与えることができますリスト コントロールの機能の詳細なカスタマイズができます。

<a name="Creating_the_Custom_ListView_Control" />

## <a name="creating-the-custom-listview-control"></a>カスタムの ListView コントロールの作成

カスタム[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)サブクラス化してコントロールを作成することができます、`ListView`クラスに、次のコード例に示すようにします。

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

`NativeListView`ポータブル クラス ライブラリ (PCL) プロジェクトが作成され、カスタム コントロールの API を定義します。 このコントロールは、公開、`Items`を設定するために使用されているプロパティ、`ListView`にバインドされているデータは表示目的、データとなることができます。 また、`ItemSelected`プラットフォーム固有のネイティブ リスト コントロールの項目が選択されるたびに発生するイベントです。 データ バインディングの詳細については、「[Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)」 (データ バインディングの基礎) を参照してください。

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>カスタム コントロールの使用

`NativeListView`カスタム コントロールで参照できます Xaml PCL プロジェクト内の場所の名前空間の宣言してコントロールに名前空間プレフィックスを使用します。 次のコード例に示す方法、`NativeListView`カスタム コントロールを XAML ページで利用できることができます。

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

`local`何も名前空間プレフィックスを付けることができます。 ただし、`clr-namespace`と`assembly`値がカスタム コントロールの詳細情報と一致する必要があります。 名前空間が宣言されると、カスタム コントロールを参照する、プレフィックスを使用します。

次のコード例に示す方法、`NativeListView`カスタム コントロールは、c# のページで利用できることができます。

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

`NativeListView`カスタム コントロールを介して作成される、データの一覧を表示するプラットフォーム固有のカスタム レンダラーを使用して、`Items`プロパティです。 リスト内の各行には、データの名前、カテゴリ、および画像のファイル名の 3 つの項目が含まれています。 リスト内の各行のレイアウトは、プラットフォーム固有のカスタム レンダラーによって定義されます。

> [!NOTE]
> `NativeListView`スクロール機能が含まれるプラットフォーム固有のリスト コントロールを使用してカスタム コントロールをレンダリングする、カスタム コントロール ホストしてはならないスクロール可能なレイアウト コントロールなど、 [ `ScrollView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/)です。

カスタム レンダラーは、プラットフォーム固有のリスト コントロールとネイティブのセルのレイアウトを作成するには、各アプリケーション プロジェクトを今すぐ追加できます。

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>各プラットフォームでは、カスタム レンダラーを作成します。

カスタム レンダラー クラスを作成するプロセスは次のとおりです。

1. サブクラスを作成、`ListViewRenderer`カスタム コントロールを表示するクラス。
1. 上書き、`OnElementChanged`をカスタマイズするカスタム コントロールと書き込みのロジックを表示するメソッド。 このメソッドが呼び出されます対応する Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)を作成します。
1. 追加、`ExportRenderer`属性をカスタム レンダラー クラス Xamarin.Forms のカスタム コントロールを表示するために使用することを指定します。 この属性を使用して、Xamarin.Forms を使用したカスタム レンダラーを登録します。

> [!NOTE]
> 各プラットフォームのプロジェクトでのカスタム レンダラーを提供する省略可能であります。 カスタム レンダラーが登録されていない場合は、セルの基本クラスの既定のレンダラーが使用されます。

次の図は、両者間のリレーションシップと共に、サンプル アプリケーション内の各プロジェクトの役割を示しています。

![](listview-images/solution-structure.png "NativeListView カスタム レンダラーのプロジェクトの責任")

`NativeListView`カスタム コントロールがから派生するプラットフォーム固有のレンダラー クラスによって表示される、`ListViewRenderer`各プラットフォームのクラスです。 これは、結果、各`NativeListView`カスタム コントロールが次のスクリーン ショットに示すように、プラットフォーム固有のリスト コントロールとネイティブのセルのレイアウト、レンダリングされます。

![](listview-images/screenshots.png "各プラットフォームで NativeListView")

`ListViewRenderer`クラスが公開、 `OnElementChanged` Xamarin.Forms のカスタム コントロールが、対応するネイティブ コントロールを表示するために作成されたときに呼び出されるメソッド。 このメソッドは、`ElementChangedEventArgs`を含むパラメーター`OldElement`と`NewElement`プロパティです。 これらのプロパティは、Xamarin.Forms 要素を表すをレンダラーでは、*が*に接続されていると Xamarin.Forms の要素をレンダラーでは、*は*に、それぞれをアタッチします。 サンプル アプリケーションで、`OldElement`プロパティ`null`と`NewElement`プロパティへの参照が格納されます、`NativeListView`インスタンス。

オーバーライドのバージョン、`OnElementChanged`メソッドは、各プラットフォームに固有のレンダラー クラスでは、ネイティブ コントロールのカスタマイズを実行する場所です。 型指定されたコントロールへの参照、ネイティブ プラットフォームで使用されている経由でアクセスできる、`Control`プロパティです。 さらに、を通じて表示される Xamarin.Forms コントロールへの参照を取得できます、`Element`プロパティです。

注意が必要でイベント ハンドラーをサブスクライブしているとき、`OnElementChanged`メソッドを次のコード例で示したようにします。

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

ネイティブ コントロールで構成するようにし、カスタム レンダラーが新しい Xamarin.Forms 要素に関連付けられている場合は、イベント ハンドラーをサブスクライブします。 同様に、イベント ハンドラーをサブスクライブしていたをすることはできませんのサブスクライブ レンダラーでは、要素が変更に関連付けられている場合にのみです。 このアプローチを採用することは、メモリ リークは発生しないこと、カスタム レンダラーを作成するのに役立ちます。

オーバーライドのバージョン、`OnElementPropertyChanged`メソッドは、各プラットフォームに固有のレンダラー クラスでは、Xamarin.Forms のカスタム コントロールにバインド可能なプロパティの変更に応答する場所です。 このオーバーライドは、何度も呼び出すことができるよう、変更されているプロパティのチェックを実行常にする必要があります。

各カスタム レンダラー クラスがで修飾された、 `ExportRenderer` Xamarin.Forms では、レンダラーを登録する属性。 属性は、–、表示される Xamarin.Forms カスタム コントロールの型名とカスタム レンダラーの種類の名前の 2 つのパラメーターを受け取ります。 `assembly`属性にプレフィックスは、属性がアセンブリ全体に適用されることを指定します。

次のセクションでは、各プラットフォームに固有のカスタム レンダラー クラスの実装について説明します。

### <a name="creating-the-custom-renderer-on-ios"></a>IOS では、カスタム レンダラーを作成します。

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

`UITableView`コントロールがのインスタンスを作成することで構成されている、`NativeiOSListViewSource`クラス、カスタムのレンダラーが新しい Xamarin.Forms 要素にアタッチされています。 このクラスにデータを提供する、`UITableView`オーバーライドすることでコントロール、`RowsInSection`と`GetCell`メソッドを`UITableViewSource`クラス、および公開することによって、`Items`プロパティを表示するデータの一覧を含むです。 クラスにも用意されています、`RowSelected`メソッドのオーバーライドを呼び出す、`ItemSelected`によって提供されるイベント、`NativeListView`カスタム コントロールです。 詳細については、メソッドをオーバーライドするを参照してください。[サブクラス化 UITableViewSource](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)です。 `GetCell`メソッドを返します、`UITableCellView`を一覧で、各行のデータが設定され、次のコード例に示します。

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

このメソッドを作成、`NativeiOSListViewCell`画面に表示されるデータの各行のインスタンス。 `NativeiOSCell`インスタンスは、各セルとセルのデータのレイアウトを定義します。 セルは、スクロールのための画面から消えます、ときに、セルになります再利用します。 これを回避できますのみが存在しているメモリを確保無駄使い`NativeiOSCell`リスト内のデータのすべてではなく、画面に表示されるデータ用のインスタンス。 セルの再利用の詳細については、次を参照してください。[セルが再利用](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)です。 `GetCell`メソッドも読み取ります、`ImageFilename`ことと、あることと、画像を読み込みますとして格納を指定されたデータの各行のプロパティ、`UIImage`更新する前に、インスタンス、`NativeiOSListViewCell`データ (名前、カテゴリ、およびイメージ) のインスタンス。行。

`NativeiOSListViewCell`クラスは、各セルのレイアウトを定義し、次のコード例に示します。

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

このクラスは、セルの内容、およびそれらのレイアウトを表示するために使用されているコントロールを定義します。 `NativeiOSListViewCell`コンス トラクターのインスタンスを作成する`UILabel`と`UIImageView`コントロール、およびその外観を初期化します。 これらのコントロールを使用すると、すべての行のデータを表示、`UpdateCell`このデータを設定するために使用されるメソッド、`UILabel`と`UIImageView`インスタンス。 これらのインスタンスの場所を設定して、オーバーライドされた`LayoutSubviews`セル内のそれぞれの座標を指定することによって、メソッド。

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>カスタム コントロールのプロパティの変更に応答してください。

場合、`NativeListView.Items`カスタム レンダラーは、変更内容を表示することによって応答する必要があります、プロパティが項目に追加されているため、変更、または一覧から削除します。 これには、オーバーライドすることで、`OnElementPropertyChanged`メソッドは、次のコード例に示されています。

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Source = new NativeiOSListViewSource (Element as NativeListView);
  }
}
```

メソッドの新しいインスタンスを作成する、`NativeiOSListViewSource`データを提供するクラス、`UITableView`コントロールを提供する、バインド可能な`NativeListView.Items`プロパティが変更されました。

### <a name="creating-the-custom-renderer-on-android"></a>Android では、カスタム レンダラーを作成します。

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

ネイティブ`ListView`カスタム レンダラーは、新しい Xamarin.Forms 要素にアタッチされているコントロールが構成されます。 この構成ではのインスタンスを作成する、`NativeAndroidListViewAdapter`ネイティブにデータを提供するクラス`ListView`制御、および処理するイベント ハンドラーを登録、`ItemClick`イベント。 さらに、このハンドラーを呼び出す、`ItemSelected`によって提供されるイベント、`NativeListView`カスタム コントロールです。 `ItemClick` Xamarin.Forms 要素は、レンダラーが変更に関連付けられている場合、イベントを購読解除します。

`NativeAndroidListViewAdapter`から派生した、`BaseAdapter`クラスを公開、`Items`をオーバーライドするとともに、表示されるデータの一覧を含むプロパティ、 `Count`、 `GetView`、 `GetItemId`、および`this[int]`メソッドです。 これらのメソッド オーバーライドの詳細については、次を参照してください。 [、ListAdapter を実装する](~/android/user-interface/layouts/list-view/populating.md)です。 `GetView`メソッドはデータを読み込んだ、各行のビューを返し、次のコード例に示します。

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

`GetView` 、描画するセルを取得するメソッドが呼び出されたとして、`View`リスト内のデータの行ごとにします。 作成、`View`の外観を備えた、画面に表示されるデータの各行のインスタンス、`View`レイアウト ファイルで定義されているインスタンス。 セルは、スクロールのための画面から消えます、ときに、セルになります再利用します。 これを回避できますのみが存在しているメモリを確保無駄使い`View`リスト内のデータのすべてではなく、画面に表示されるデータ用のインスタンス。 ビューを再利用の詳細については、次を参照してください。[行ビューを再利用](~/android/user-interface/layouts/list-view/populating.md)です。

`GetView`メソッドが読み込まれます、`View`で指定されたファイル名から画像データの読み込みなどのデータ インスタンス、`ImageFilename`プロパティです。

各セル dispayed ネイティブでのレイアウト`ListView`で定義されて、`NativeAndroidListViewCell.axml`レイアウト ファイルは、によって大きく、`LayoutInflater.Inflate`メソッドです。 次のコード例は、レイアウトの定義を示しています。

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

このレイアウトを指定する 2 つ`TextView`コントロールと`ImageView`コントロールがセルの内容を表示するために使用します。 2 つ`TextView`コントロールは垂直方向に配置内で、`LinearLayout`中に含まれるすべてのコントロールのコントロール、`RelativeLayout`です。

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>カスタム コントロールのプロパティの変更に応答してください。

場合、`NativeListView.Items`カスタム レンダラーは、変更内容を表示することによって応答する必要があります、プロパティが項目に追加されているため、変更、または一覧から削除します。 これには、オーバーライドすることで、`OnElementPropertyChanged`メソッドは、次のコード例に示されています。

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Adapter = new NativeAndroidListViewAdapter (_context as Android.App.Activity, Element as NativeListView);
  }
}
```

メソッドの新しいインスタンスを作成する、`NativeAndroidListViewAdapter`ネイティブにデータを提供するクラス`ListView`コントロールを提供する、バインド可能な`NativeListView.Items`プロパティが変更されました。

### <a name="creating-the-custom-renderer-on-windows-phone-and-uwp"></a>Windows Phone のカスタム レンダラーを作成し、UWP

次のコード例では、Windows Phone と UWP のカスタム レンダラーを示しています。

```csharp
[assembly: ExportRenderer (typeof(NativeListView), typeof(NativeWinPhoneListViewRenderer))]
namespace CustomRenderer.WinPhone81
{
    public class NativeWinPhoneListViewRenderer : ListViewRenderer
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

ネイティブ`ListView`カスタム レンダラーは、新しい Xamarin.Forms 要素にアタッチされているコントロールが構成されます。 この構成設定は、どのネイティブ`ListView`コントロールが選択されているアイテムに応答は、各セルの内容と外観を定義して、を処理するイベントハンドラーの登録は、コントロールによって表示されるデータを設定します。`SelectionChanged`イベント。 さらに、このハンドラーを呼び出す、`ItemSelected`によって提供されるイベント、`NativeListView`カスタム コントロールです。 `SelectionChanged` Xamarin.Forms 要素は、レンダラーが変更に関連付けられている場合、イベントを購読解除します。

各ネイティブの内容と外観`ListView`セルがによって定義された、`DataTemplate`という`ListViewItemTemplate`です。 これは、`DataTemplate`はアプリケーション レベル リソース ディクショナリに格納され、次のコード例に示します。

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

`DataTemplate`セル、およびレイアウトと外観の内容を表示するために使用するコントロールを指定します。 2 つ`TextBlock`コントロールと`Image`コントロールがデータ バインディングによってセルの内容を表示するために使用します。 さらのインスタンス、`ConcatImageExtensionConverter`連結するために使用、`.jpg`ファイルを各イメージ ファイル名拡張子。 これにより、`Image`コントロールが読み込むし、ある場合に、イメージをレンダリング`Source`プロパティを設定します。

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>カスタム コントロールのプロパティの変更に応答してください。

場合、`NativeListView.Items`カスタム レンダラーは、変更内容を表示することによって応答する必要があります、プロパティが項目に追加されているため、変更、または一覧から削除します。 これには、オーバーライドすることで、`OnElementPropertyChanged`メソッドは、次のコード例に示されています。

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

メソッドは、ネイティブを再設定`ListView`コントロールに変更されたデータを提供する、バインド可能な`NativeListView.Items`プロパティが変更されました。

## <a name="summary"></a>まとめ

この記事では、カスタム レンダラーのプラットフォーム固有のリスト コントロールとネイティブのセルのレイアウトをカプセル化するネイティブのリスト コントロールのパフォーマンスをより細かく制御できるようにを作成する方法を説明しました。


## <a name="related-links"></a>関連リンク

- [CustomRendererListView (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/listview/)
