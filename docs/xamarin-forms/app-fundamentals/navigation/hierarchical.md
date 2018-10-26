---
title: 階層ナビゲーション
description: この記事では、NavigationPage クラスを使用して、後入れ先出し (LIFO) のページのスタックでナビゲーションを実行する方法を示します。
ms.prod: xamarin
ms.assetid: C8A5EEFF-5A3B-4163-838A-147EE3939FAA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/14/2018
ms.openlocfilehash: a0a58cf05c97221a73cd0784b7859bb9c84cef86
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/24/2018
ms.locfileid: "38994677"
---
# <a name="hierarchical-navigation"></a>階層ナビゲーション

_NavigationPage クラスでは、ユーザーが forwards と backwards、必要に応じて、ページを移動できる階層ナビゲーション エクスペリエンスを提供します。クラスは、ページのオブジェクトの後入れ先出し (LIFO) スタックとしてナビゲーションを実装します。この記事では、NavigationPage クラスを使用して、ページのスタックでナビゲーションを実行する方法を示します。_

別に 1 つのページから移動するに、アプリケーションはそこでなるアクティブなページで、次の図に示すようをナビゲーション スタックに新しいページにプッシュします。

![](hierarchical-images/pushing.png "ページをナビゲーション スタックにプッシュ")

前のページに戻るに、アプリケーションは、ナビゲーション スタックから現在のページをポップし、次の図に示すように、新しい最上位のページがアクティブ ページ。

![](hierarchical-images/popping.png "ページをナビゲーション スタックからポップ")

ナビゲーション メソッドは、によって公開される、 [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation)任意プロパティ[ `Page` ](xref:Xamarin.Forms.Page)派生型。 これらのメソッドは、ナビゲーション スタックにページをナビゲーション スタックからポップのページにプッシュし、スタック操作を実行する機能を提供します。

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>ナビゲーションを実行します。

階層ナビゲーションでは、[`NavigationPage`](xref:Xamarin.Forms.NavigationPage) クラスは [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトのスタック間をナビゲートするために使用されます。 次のスクリーン ショットの主要なコンポーネント、`NavigationPage`各プラットフォームで。

![](hierarchical-images/navigationpage-components.png "NavigationPage コンポーネント")

レイアウトを[ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)はプラットフォームに依存します。

- Ios では、ナビゲーション バーは、タイトルを表示しを持つページの上部にある存在、*戻る*を返す前のページにボタンをクリックします。
- Android では、ナビゲーション バーは、アイコン、タイトルを表示するページの上部にあると*戻る*を返す前のページにボタンをクリックします。 アイコンが定義されている、`[Activity]`を装飾する属性、 `MainActivity` Android のプラットフォーム固有プロジェクト内のクラス。
- ユニバーサル Windows プラットフォームでは、ナビゲーション バーは、タイトルを表示するページの上部にある存在します。

すべてのプラットフォームの値で、 [ `Page.Title` ](xref:Xamarin.Forms.Page.Title)プロパティがページのタイトルとして表示されます。

> [!NOTE]
> お勧めしますが、`NavigationPage`を代入するか`ContentPage`インスタンスのみです。

### <a name="creating-the-root-page"></a>ルート ページの作成

ナビゲーション スタックに追加された最初のページは、アプリケーションの*ルート* ページとなります。次のコード例に、これを実現する方法を示しています。

```csharp
public App ()
{
  MainPage = new NavigationPage (new Page1Xaml ());
}
```

これにより、 `Page1Xaml` [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)がアクティブ ページおよびアプリケーションのルート ページ、ナビゲーション スタックにプッシュされるインスタンス。 これは、次のスクリーン ショットに示されます。

![](hierarchical-images/mainpage.png "ナビゲーション スタックのルート ページ")

> [!NOTE]
> [ `RootPage` ](xref:Xamarin.Forms.NavigationPage.RootPage)のプロパティを[ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)インスタンスがナビゲーション スタックの最初のページへのアクセスを提供します。

### <a name="pushing-pages-to-the-navigation-stack"></a>ページをナビゲーション スタックにプッシュ

移動する`Page2Xaml`を呼び出す必要がある、 [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync*)メソッドを[ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation)のコード例を次のとおり、現在のページのプロパティ。

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new Page2Xaml ());
}
```

これにより、`Page2Xaml` インスタンスがナビゲーション スタックにプッシュされるようになり、そこがアクティブ ページとなります。 これは、次のスクリーン ショットに示されます。

![](hierarchical-images/secondpage.png "ページがナビゲーション スタックにプッシュされます。")

ときに、 [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync*)メソッドが呼び出される、次のイベントが発生します。

- 呼び出し元ページ`PushAsync`がその[ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing)オーバーライドが呼び出されます。
- 移動先ページがその[ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing)オーバーライドが呼び出されます。
- `PushAsync`タスクが完了します。

ただし、これらのイベントが発生する正確な順序は、プラットフォームに依存します。 詳細については、次を参照してください。[第 24 章](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)Charles Petzold の Xamarin.Forms book の。

> [!NOTE]
> 呼び出し、 [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing)と[ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing)上書きは、ページ ナビゲーションの保証がないとして扱うことはできません。 たとえば、iOS、上、`OnDisappearing`アプリケーションの終了時に、アクティブなページ オーバーライドが呼び出されます。

### <a name="popping-pages-from-the-navigation-stack"></a>ナビゲーション スタックからポップをそれぞれのページ

アクティブ ページは、これが物理的なボタンであるか画面上のボタンであるかどうかにかかわらず、デバイスの *[戻る]* ボタンを押すことによってナビゲーション スタックからポップすることができます。

元のページにプログラムを使用して戻るには、`Page2Xaml` インスタンスが、次のコード例のとおり [`PopAsync`](xref:Xamarin.Forms.NavigationPage.PopAsync) メソッドを起動する必要があります。

```csharp
async void OnPreviousPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopAsync ();
}
```

これにより、ナビゲーション スタックから `Page2Xaml` インスタンスが削除され、新しい最上位のページがアクティブ ページとなります。 ときに、 [ `PopAsync` ](xref:Xamarin.Forms.NavigationPage.PopAsync)メソッドが呼び出される、次のイベントが発生します。

- 呼び出し元ページ`PopAsync`がその[ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing)オーバーライドが呼び出されます。
- 返されるページがその[ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing)オーバーライドが呼び出されます。
- `PopAsync`タスクを返します。

ただし、これらのイベントが発生する正確な順序は、プラットフォームに依存します。 詳細については、次を参照してください。[第 24 章](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)Charles Petzold の Xamarin.Forms book の。

だけでなく[ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync*)と[ `PopAsync` ](xref:Xamarin.Forms.NavigationPage.PopAsync) 、メソッド、 [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation)各ページのプロパティも用意されています、 [ `PopToRootAsync` 。](xref:Xamarin.Forms.NavigationPage.PopToRootAsync)メソッドは、次のコード例に示されています。

```csharp
async void OnRootPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopToRootAsync ();
}
```

このメソッドは、ルートを除くすべてポップ[ `Page` ](xref:Xamarin.Forms.Page)ナビゲーション スタックからアプリケーションのアクティブなページのルート ページにそのため、します。

### <a name="animating-page-transitions"></a>ページの切り替え効果をアニメーション化

[ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation)各ページのプロパティは、オーバーライドされたプッシュおよびポップ メソッドが含まれているにも提供します、`boolean`に次のコードに示すように、ナビゲーション中にページのアニメーションを表示するかどうかを制御するパラメーター例:

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PushAsync (new Page2Xaml (), false);
}

async void OnPreviousPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PopAsync (false);
}

async void OnRootPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PopToRootAsync (false);
}
```

設定、`boolean`パラメーターを`false`パラメータを設定するときに、ページ遷移アニメーションを無効にします。`true`基になるプラットフォームでサポートされている、ページ遷移アニメーションを使用します。 ただし、このパラメーターを持たない push および pop メソッドは、既定では、アニメーションを有効にします。

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>移動するときにデータの受け渡し

ページの移動中に別のページにデータを渡すために必要な場合があります。 これを行うために 2 つの手法は、ページのコンス トラクター、および、新しいページを設定してデータを渡している[ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)データにします。 さらに説明するようになりましたが。

### <a name="passing-data-through-a-page-constructor"></a>ページのコンス トラクターを通じてデータの受け渡し

別のページへの移動中にデータを渡すための最も単純な手法は、次のコード例に示されているページのコンス トラクターのパラメーターでは。

```csharp
public App ()
{
  MainPage = new NavigationPage (new MainPage (DateTime.Now.ToString ("u")));
}
```

このコードを作成、`MainPage`にラップされて、現在の日付と時刻 ISO8601 形式で渡すことで、 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)インスタンス。

`MainPage`インスタンスが次のコード例に示すように、コンス トラクターのパラメーターを使用してデータを受信します。

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

データが設定ページに表示し、 [ `Label.Text` ](xref:Xamarin.Forms.Label.Text)プロパティは、次のスクリーン ショットに示すようにします。

![](hierarchical-images/passing-data-constructor.png "ページのコンス トラクターを通じて渡されるデータ")

### <a name="passing-data-through-a-bindingcontext"></a>BindingContext を通じたデータの受け渡し

別のページへの移動中にデータを渡すための別のアプローチは、新しいページを設定して、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)データの次のコード例に示すようにします。

```csharp
async void OnNavigateButtonClicked (object sender, EventArgs e)
{
  var contact = new Contact {
    Name = "Jane Doe",
    Age = 30,
    Occupation = "Developer",
    Country = "USA"
  };

  var secondPage = new SecondPage ();
  secondPage.BindingContext = contact;
  await Navigation.PushAsync (secondPage);
}
```

このコードを設定、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)の`SecondPage`インスタンスを`Contact`インスタンス、およびに移動し、`SecondPage`します。

`SecondPage`データ バインディングを使用して、表示、`Contact`の XAML コードの例を次に示すように、データをインスタンスします。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="PassingData.SecondPage"
             Title="Second Page">
    <ContentPage.Content>
        <StackLayout HorizontalOptions="Center" VerticalOptions="Center">
            <StackLayout Orientation="Horizontal">
                <Label Text="Name:" HorizontalOptions="FillAndExpand" />
                <Label Text="{Binding Name}" FontSize="Medium" FontAttributes="Bold" />
            </StackLayout>
            ...
            <Button x:Name="navigateButton" Text="Previous Page" Clicked="OnNavigateButtonClicked" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

次のコード例では、データ バインディングを実現 (C#) する方法を示します。

```csharp
public class SecondPageCS : ContentPage
{
  public SecondPageCS ()
  {
    var nameLabel = new Label {
      FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
      FontAttributes = FontAttributes.Bold
    };
    nameLabel.SetBinding (Label.TextProperty, "Name");
    ...
    var navigateButton = new Button { Text = "Previous Page" };
    navigateButton.Clicked += OnNavigateButtonClicked;

    Content = new StackLayout {
      HorizontalOptions = LayoutOptions.Center,
      VerticalOptions = LayoutOptions.Center,
      Children = {
        new StackLayout {
          Orientation = StackOrientation.Horizontal,
          Children = {
            new Label{ Text = "Name:", FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)), HorizontalOptions = LayoutOptions.FillAndExpand },
            nameLabel
          }
        },
        ...
        navigateButton
      }
    };
  }

  async void OnNavigateButtonClicked (object sender, EventArgs e)
  {
    await Navigation.PopAsync ();
  }
}
```

データが、一連のページに表示し、 [ `Label` ](xref:Xamarin.Forms.Label)コントロール、次のスクリーン ショットに示すようにします。

![](hierarchical-images/passing-data-bindingcontext.png "BindingContext を通じて渡されるデータ")

データ バインディングの詳細については、「[Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)」 (データ バインディングの基礎) を参照してください。

<a name="Manipulating_the_Navigation_Stack" />

## <a name="manipulating-the-navigation-stack"></a>ナビゲーション スタックを操作します。

[ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation)プロパティが公開、 [ `NavigationStack` ](xref:Xamarin.Forms.INavigation.NavigationStack)プロパティ、ナビゲーション スタック内のページを取得できます。 Xamarin.Forms の維持、ナビゲーション スタックへのアクセス中に、`Navigation`プロパティは、提供、 [ `InsertPageBefore` ](xref:Xamarin.Forms.INavigation.InsertPageBefore*)と[ `RemovePage` ](xref:Xamarin.Forms.INavigation.RemovePage*)を挿入することで、スタックを操作するメソッドページまたはそれらを削除します。

[ `InsertPageBefore` ](xref:Xamarin.Forms.INavigation.InsertPageBefore*)メソッドは、次の図に示すように既存の指定 ページの前に、ナビゲーション スタックで指定したページを挿入します。

![](hierarchical-images/insert-page-before.png "ナビゲーション スタックにページを挿入します。")

[ `RemovePage` ](xref:Xamarin.Forms.INavigation.RemovePage*)メソッドは、次の図に示すように、ナビゲーション スタックから指定したページを削除します。

![](hierarchical-images/remove-page.png "ページをナビゲーション スタックから削除します。")

これらのメソッドは、新しいページで、次のログインに成功すると、ログイン ページを置き換えるなどのカスタム ナビゲーション エクスペリエンスを有効にします。 次のコード例では、このシナリオを示しています。

```csharp
async void OnLoginButtonClicked (object sender, EventArgs e)
{
  ...
  var isValid = AreCredentialsCorrect (user);
  if (isValid) {
    App.IsUserLoggedIn = true;
    Navigation.InsertPageBefore (new MainPage (), this);
    await Navigation.PopAsync ();
  } else {
    // Login failed
  }
}

```

ユーザーの資格情報が正しいこと、`MainPage`インスタンスが現在のページの前に、ナビゲーション スタックに挿入されます。 [ `PopAsync` ](xref:Xamarin.Forms.NavigationPage.PopAsync)メソッドをナビゲーション スタックから現在のページを削除し、`MainPage`インスタンスがアクティブ ページになることです。

## <a name="displaying-views-in-the-navigation-bar"></a>ナビゲーション バーでビューを表示します。

すべての Xamarin.Forms [ `View` ](xref:Xamarin.Forms.View)のナビゲーション バーに表示されることができます、 [ `NavigationPage`](xref:Xamarin.Forms.NavigationPage)します。 これは、設定によって実現されます、 [ `NavigationPage.TitleView` ](xref:Xamarin.Forms.NavigationPage.TitleViewProperty)添付プロパティを`View`します。 いずれかでこの添付プロパティを設定できます[ `Page` ](xref:Xamarin.Forms.Page)、いつ、`Page`にプッシュされます、 `NavigationPage`、`NavigationPage`プロパティの値が反映されます。

取得した次の例で、[タイトル ビュー サンプル](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TitleView/)を設定する方法を示しています、 [ `NavigationPage.TitleView` ](xref:Xamarin.Forms.NavigationPage.TitleViewProperty)添付プロパティを XAML から。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="NavigationPageTitleView.TitleViewPage">
    <NavigationPage.TitleView>
        <Slider HeightRequest="44" WidthRequest="300" />
    </NavigationPage.TitleView>
    ...
</ContentPage>
```

ここでは、相当C#コード。

```csharp
public class TitleViewPage : ContentPage
{
    public TitleViewPage()
    {
        var titleView = new Slider { HeightRequest = 44, WidthRequest = 300 };
        NavigationPage.SetTitleView(this, titleView);
        ...
    }
}
```

これは、結果、 [ `Slider` ](xref:Xamarin.Forms.Slider)のナビゲーション バーに表示されている、 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage):

[![スライダー TitleView](hierarchical-images/titleview-small.png "スライダー TitleView")](hierarchical-images/titleview-large.png#lightbox "スライダー TitleView")

> [!IMPORTANT]
> ビューのサイズを指定しない場合、ナビゲーション バーに複数のビューは表示されません、 [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest)と[ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest)プロパティ。 ビューにラップする代わりに、 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)で、 [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions)と[ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions)適切な値に設定します。

に、注意してください、 [ `Layout` ](xref:Xamarin.Forms.Layout)クラスから派生、 [ `View` ](xref:Xamarin.Forms.View)クラス、 [ `TitleView` ](xref:Xamarin.Forms.NavigationPage.TitleViewProperty)レイアウトを表示する添付プロパティを設定することができます複数のビューを含むクラスです。 IOS、ユニバーサル Windows プラットフォーム (UWP) で、変更できないナビゲーション バーの高さとナビゲーション バーに表示されるビューがナビゲーション バーの既定のサイズより大きい場合にクリッピングが発生するようにします。 ただし、android では、ナビゲーション バーの高さは設定して、 [ `NavigationPage.BarHeight` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.BarHeightProperty)バインド可能なプロパティを`double`新しい高さを表します。 詳細については、次を参照してください。 [NavigationPage のナビゲーション バーの高さを設定](~/xamarin-forms/platform/platform-specifics/consuming/android.md#navigationpage-barheight)します。

または、上部のナビゲーション バーにする色に一致するページのコンテンツ ビューでのナビゲーション バーで、コンテンツの一部とを配置することで、拡張のナビゲーション バーを推奨します。 さらに、iOS で、区切り線とナビゲーション バーの下部にあるシャドウを取り外せるを設定して、 [ `NavigationPage.HideNavigationBarSeparator` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.HideNavigationBarSeparatorProperty)バインド可能なプロパティを`true`します。 詳細については、次を参照してください。 [NavigationPage のナビゲーション バーの区切り記号を非表示](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#navigationpage-hideseparatorbar)します。

> [!NOTE]
> [ `BackButtonTitle` ](xref:Xamarin.Forms.NavigationPage.BackButtonTitleProperty)、 [ `Title` ](xref:Xamarin.Forms.Page.Title)、 [ `TitleIcon` ](xref:Xamarin.Forms.NavigationPage.TitleIconProperty)、および[ `TitleView` ](xref:Xamarin.Forms.NavigationPage.TitleViewProperty)プロパティを定義できますナビゲーション バー上の空き領域の値。 ナビゲーション バーのサイズは、プラットフォームや画面サイズによって異なります、中で利用できる限られたスペースに起因する競合が発生これらすべてのプロパティを設定します。 これらのプロパティの組み合わせを使用しようとすると、代わりにのみ設定して、目的のナビゲーション バーのデザインをより実現できますを見つけることがあります、`TitleView`プロパティ。

### <a name="limitations"></a>制限事項

表示するときに注意すべき制限事項がいくつか、 [ `View` ](xref:Xamarin.Forms.View)のナビゲーション バーで、 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage):

- Ios では、ビューの配置のナビゲーション バーで、`NavigationPage`大きいタイトルが有効になっているかどうかによって別の位置に表示されます。 大規模なタイトルを有効にする方法の詳細については、次を参照してください。[大きいタイトルを表示する](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#large_title)します。
- Android でのナビゲーション バーでビューを配置する、`NavigationPage`アプリケーション互換性を使用するアプリでのみ実行できます。
- など、大規模で複雑なビューを配置することは推奨されません[ `ListView` ](xref:Xamarin.Forms.ListView)と[ `TableView`](xref:Xamarin.Forms.TableView)のナビゲーション バーで、`NavigationPage`します。

## <a name="related-links"></a>関連リンク

- [ページ ナビゲーション](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [階層 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/)
- [PassingData (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
- [LoginFlow (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)
- [TitleView (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TitleView/)
- [Xamarin.Forms (Xamarin University のビデオ) サンプルでは画面のフローで、記号を作成する方法](http://xamarinuniversity.blob.core.windows.net/lightninglectures/CreateASignIn.zip)
- [Xamarin.Forms (Xamarin University のビデオ) でフローを画面で、記号を作成する方法](https://university.xamarin.com/lightninglectures/how-to-create-a-sign-in-screen-flow-in-xamarinforms)
- [NavigationPage](xref:Xamarin.Forms.NavigationPage)
