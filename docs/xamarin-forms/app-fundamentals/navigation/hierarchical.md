---
title: 階層ナビゲーション
description: この記事では、NavigationPage クラスを使用して後入れ先出し (LIFO) ページのスタックでナビゲーションを実行する方法について説明します。
ms.prod: xamarin
ms.assetid: C8A5EEFF-5A3B-4163-838A-147EE3939FAA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/14/2018
ms.openlocfilehash: 11ad1fb18d1263eb77ef037350a3633510934c42
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79303823"
---
# <a name="hierarchical-navigation"></a>階層ナビゲーション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-hierarchical)

_NavigationPage クラスは、ユーザーが前後を希望どおりにページを移動することができる階層ナビゲーション エクスペリエンスを提供します。このクラスは、Page オブジェクトの後入れ先出し (LIFO) スタックとしてナビゲーションを提供します。この記事では、NavigationPage クラスを使用してページのスタックでナビゲーションを実行する方法について説明します。_

1 つのページから別のページに移動するには、次の図に示すように、アプリケーションは新しいページを、そこでアクティブなページとなるナビゲーション スタックにプッシュします。

![](hierarchical-images/pushing.png "Pushing a Page to the Navigation Stack")

次の図に示すように、前のページに戻るには、アプリケーションは現在のページをナビゲーション スタックからポップします。そして新しい最上位のページがアクティブ ページになります。

![](hierarchical-images/popping.png "Popping a Page from the Navigation Stack")

ナビゲーション メソッドは、任意の [`Page`](xref:Xamarin.Forms.Page) 派生型の [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) プロパティによって公開されます。 これらのメソッドには、ページをナビゲーション スタックにプッシュし、ナビゲーション スタックからページをポップし、スタック操作を実行する機能があります。

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>ナビゲーションを実行する

階層ナビゲーションでは、[`NavigationPage`](xref:Xamarin.Forms.NavigationPage) クラスは [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトのスタック間をナビゲートするために使用されます。 次のスクリーンショットは、各プラットフォームでの `NavigationPage` のメイン コンポーネントを示します。

![](hierarchical-images/navigationpage-components.png "NavigationPage Components")

[`NavigationPage`](xref:Xamarin.Forms.NavigationPage) のレイアウトは、プラットフォームによって異なります。

- iOS では、ナビゲーション バーがページの上部にあり、タイトルが表示され、前のページに戻る *[戻る]* ボタンがあります。
- Android では、ナビゲーション バーがページの上部にあり、タイトル、アイコンと、前のページに戻る *[戻る]* ボタンが表示されています。 Android プラットフォーム固有プロジェクトでは、アイコンは `MainActivity` クラスを修飾する `[Activity]` 属性で定義されています。
- ユニバーサル Windows プラットフォームでは、ナビゲーション バーはページの上部にあり、タイトルが表示されています。

いずれのプラットフォームでも、[`Page.Title`](xref:Xamarin.Forms.Page.Title) プロパティの値がページ タイトルとして表示されます。

> [!NOTE]
> `NavigationPage` を `ContentPage` インスタンスのみで作成することをお勧めします。

### <a name="creating-the-root-page"></a>ルート ページを作成する

ナビゲーション スタックに追加された最初のページは、アプリケーションの*ルート* ページとなります。次のコード例に、これを実現する方法を示しています。

```csharp
public App ()
{
  MainPage = new NavigationPage (new Page1Xaml ());
}
```

これにより、`Page1Xaml` [`ContentPage`](xref:Xamarin.Forms.ContentPage) インスタンスがナビゲーション スタックにプッシュされるようになります。そこがアクティブ ページであり、アプリケーションのルート ページとなります。 これを次のスクリーンショットに示します。

![](hierarchical-images/mainpage.png "Root Page of Navigation Stack")

> [!NOTE]
> [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) インスタンスの [`RootPage`](xref:Xamarin.Forms.NavigationPage.RootPage) プロパティを使用すると、ナビゲーション スタック内の最初のページにアクセスできます。

### <a name="pushing-pages-to-the-navigation-stack"></a>ナビゲーション スタックにページをプッシュする

`Page2Xaml` にナビゲートするには、次のコード例で示すように、現在のページの [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) プロパティで [`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync*) メソッドを起動する必要があります。

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new Page2Xaml ());
}
```

これにより、`Page2Xaml` インスタンスがナビゲーション スタックにプッシュされるようになり、そこがアクティブ ページとなります。 これを次のスクリーンショットに示します。

![](hierarchical-images/secondpage.png "Page Pushed onto Navigation Stack")

[`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync*) メソッドが呼び出されると、次のイベントが発生します。

- `PushAsync` を呼び出すページでは、[`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) のオーバーライドが呼び出されます。
- ナビゲート先のページでは、[`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) のオーバーライドが呼び出されます。
- `PushAsync` タスクが完了します。

ただし、これらのイベントが発生する正確な順序はプラットフォームによって異なります。 詳細については、Charles Petzold 氏著作の Xamarin.Forms ブックの[第 24 章](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)を参照してください。

> [!NOTE]
> [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) および [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) のオーバーライドの呼び出しは、ページ ナビゲーションを示す保証として扱うことはできません。 たとえば、iOS では、アプリケーションの終了時にアクティブ ページで `OnDisappearing` のオーバーライドが呼び出されます。

### <a name="popping-pages-from-the-navigation-stack"></a>ナビゲーション スタックからページをポップする

アクティブ ページは、これが物理的なボタンであるか画面上のボタンであるかどうかにかかわらず、デバイスの *[戻る]* ボタンを押すことによってナビゲーション スタックからポップすることができます。

元のページにプログラムを使用して戻るには、`Page2Xaml` インスタンスが、次のコード例のとおり [`PopAsync`](xref:Xamarin.Forms.NavigationPage.PopAsync) メソッドを起動する必要があります。

```csharp
async void OnPreviousPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopAsync ();
}
```

これにより、ナビゲーション スタックから `Page2Xaml` インスタンスが削除され、新しい最上位のページがアクティブ ページとなります。 [`PopAsync`](xref:Xamarin.Forms.NavigationPage.PopAsync) メソッドが呼び出されると、次のイベントが発生します。

- `PopAsync` を呼び出すページでは、[`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) のオーバーライドが呼び出されます。
- 戻り先のページでは、[`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) のオーバーライドが呼び出されます。
- `PopAsync` タスクが復帰します。

ただし、これらのイベントが発生する正確な順序はプラットフォームによって異なります。 詳細については、Charles Petzold 氏著作の Xamarin.Forms ブックの[第 24 章](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)を参照してください。

各ページの [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) プロパティには、[`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync*) および [`PopAsync`](xref:Xamarin.Forms.NavigationPage.PopAsync) メソッドだけでなく、次のコード例に示すように、[`PopToRootAsync`](xref:Xamarin.Forms.NavigationPage.PopToRootAsync) メソッドも用意されています。

```csharp
async void OnRootPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopToRootAsync ();
}
```

このメソッドは、ルート [`Page`](xref:Xamarin.Forms.Page) 以外のすべてをナビゲーション スタックからポップします。そのため、アプリケーションのルート ページがアクティブ ページになります。

### <a name="animating-page-transitions"></a>ページ遷移をアニメーション化する

各ページの [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) プロパティには、次のコード例に示すように、ナビゲーション中にページ アニメーションを表示するかどうかを制御する `boolean` パラメーターを含むオーバーライドされたプッシュおよびポップ メソッドも用意されています。

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

`boolean` パラメーターを `false` に設定すると、ページ遷移アニメーションが無効になります。また、パラメーターを `true` に設定すると、基となるプラットフォームでサポートされている場合はページ遷移アニメーションが有効になります。 ただし、プッシュとポップのメソッドでこのパラメーターが指定されていない場合は、既定でアニメーションが有効になります。

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>ナビゲーション時にデータを渡す

場合によっては、ナビゲーション中に、あるページから別のページにデータを渡す必要があります。 これを実現する 2 つの手法では、ページ コンストラクターを介してデータを渡し、新しいページの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) をデータに設定しています。 これから、それぞれについて順番に説明します。

### <a name="passing-data-through-a-page-constructor"></a>ページ コンストラクターを介してデータを渡す

ナビゲーション中に別のページにデータを渡す最も簡単な手法は、次のコード例で示すように、ページ コンストラクター パラメーターを使用することです。

```csharp
public App ()
{
  MainPage = new NavigationPage (new MainPage (DateTime.Now.ToString ("u")));
}
```

このコードでは、現在の日付と時刻を ISO8601 形式で渡す `MainPage` インスタンスを作成します。これは [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) インスタンスでラップされています。

`MainPage` インスタンスは、次のコード例に示すように、コンストラクター パラメーターを使用してデータを受け取ります。

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

次のスクリーンショットに示すように、[`Label.Text`](xref:Xamarin.Forms.Label.Text) プロパティを設定することでデータがページに表示されます。

![](hierarchical-images/passing-data-constructor.png "Data Passed Through a Page Constructor")

### <a name="passing-data-through-a-bindingcontext"></a>BindingContext を介してデータを渡す

ナビゲーション中に別のページにデータを渡すもう 1 つの方法は、次のコード例に示すように、新しいページの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) をデータに設定することです。

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

このコードでは、`SecondPage` インスタンスの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) を `Contact` インスタンスに設定し、`SecondPage` にナビゲートします。

次の XAML コード例に示すように、`SecondPage` ではデータ バインディングを使用して `Contact` インスタンス データを表示します。

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

次のコード例は、C# でデータ バインディングを実行する方法を示しています。

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

次のスクリーンショットに示すように、一連の [`Label`](xref:Xamarin.Forms.Label) コントロールによってデータがページに表示されます。

![](hierarchical-images/passing-data-bindingcontext.png "Data Passed Through a BindingContext")

データ バインディングの詳細については、「[Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)」 (データ バインディングの基礎) を参照してください。

<a name="Manipulating_the_Navigation_Stack" />

## <a name="manipulating-the-navigation-stack"></a>ナビゲーション スタックの操作

[`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) プロパティは、ナビゲーション スタックのページを取得する [`NavigationStack`](xref:Xamarin.Forms.INavigation.NavigationStack) プロパティを公開します。 Xamarin.Forms はナビゲーション スタックへのアクセスを維持していますが、`Navigation` プロパティには、ページを挿入または削除することでスタックを操作するための [`InsertPageBefore`](xref:Xamarin.Forms.INavigation.InsertPageBefore*) および [`RemovePage`](xref:Xamarin.Forms.INavigation.RemovePage*) メソッドが用意されています。

次の図に示すように、[`InsertPageBefore`](xref:Xamarin.Forms.INavigation.InsertPageBefore*) メソッドによって、ナビゲーション スタック内の指定されたページが既存の指定されたページの前に挿入されます。

![](hierarchical-images/insert-page-before.png "Inserting a Page in the Navigation Stack")

次の図に示すように、[`RemovePage`](xref:Xamarin.Forms.INavigation.RemovePage*) メソッドによって、指定されたページがナビゲーション スタックから削除されます。

![](hierarchical-images/remove-page.png "Removing a Page from the Navigation Stack")

これらのメソッドを使用すると、ログインに成功した後に、ログイン ページを新しいページに置き換えるなど、カスタムのナビゲーション エクスペリエンスを実現できます。 次のコード例はこのシナリオを示しています。

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

ユーザーの資格情報が正しい場合、`MainPage` インスタンスは、ナビゲーション スタック内の現在のページの前に挿入されます。 次に [`PopAsync`](xref:Xamarin.Forms.NavigationPage.PopAsync) メソッドによってナビゲーション スタックから現在のページが削除され、`MainPage` インスタンスがアクティブ ページになります。

## <a name="displaying-views-in-the-navigation-bar"></a>ナビゲーション バーにビューを表示する

すべての Xamarin.Forms [`View`](xref:Xamarin.Forms.View) は、[`NavigationPage`](xref:Xamarin.Forms.NavigationPage) のナビゲーション バーに表示できます。 これを実現するには、[`NavigationPage.TitleView`](xref:Xamarin.Forms.NavigationPage.TitleViewProperty) 添付プロパティを `View` に設定します。 この添付プロパティは任意の [`Page`](xref:Xamarin.Forms.Page) に設定できます。また、`Page` が `NavigationPage` にプッシュされると、`NavigationPage` ではプロパティの値が反映されます。

[タイトル ビュー サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-titleview)から取り上げた次の例は、[`NavigationPage.TitleView`](xref:Xamarin.Forms.NavigationPage.TitleViewProperty) 添付プロパティを XAML から設定する方法を示しています。

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

C# での同等のコードを次に示します。

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

この結果、[`NavigationPage`](xref:Xamarin.Forms.NavigationPage) でナビゲーション バーに [`Slider`](xref:Xamarin.Forms.Slider) が表示されます:

[![スライダーの TitleView](hierarchical-images/titleview-small.png "スライダーの TitleView")](hierarchical-images/titleview-large.png#lightbox "スライダーの TitleView")

> [!IMPORTANT]
> ビューのサイズが [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) および[`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) のプロパティで指定されていない場合、多くのビューはナビゲーション バーに表示されません。 または、[`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) および [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) プロパティを適切な値に設定してビューを [`StackLayout`](xref:Xamarin.Forms.StackLayout) にラップすることができます。

[`Layout`](xref:Xamarin.Forms.Layout) クラスは [`View`](xref:Xamarin.Forms.View) クラスから派生しているため、複数のビューを含むレイアウト クラスを表示するように [`TitleView`](xref:Xamarin.Forms.NavigationPage.TitleViewProperty) 添付プロパティを設定することができます。 iOS およびユニバーサル Windows プラットフォーム (UWP) では、ナビゲーション バーの高さを変更できないため、ナビゲーション バーに表示されるビューがナビゲーション バーの既定サイズより大きい場合、クリップが発生します。 一方 Android では、[`NavigationPage.BarHeight`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.BarHeightProperty) バインディング可能プロパティを新しい高さを表す `double` に設定することで、ナビゲーション バーの高さを変更できます。 詳細については、「[Setting the Navigation Bar Height on a NavigationPage](~/xamarin-forms/platform/android/navigationpage-bar-height.md)」(NavigationPage でナビゲーション バーの高さを設定する) を参照してください。

また、ナビゲーション バーにコンテンツの一部を配置し、ナビゲーション バーと色を合わせたビューの一部をページ コンテンツの上部に配置して、拡張ナビゲーション バーを提案することもできます。 さらに、iOS では、[`NavigationPage.HideNavigationBarSeparator`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.HideNavigationBarSeparatorProperty) バインド可能プロパティを `true` に設定することで、ナビゲーション バーの下部にあるセパレーターと影を削除できます。 詳細については、「[Hiding the Navigation Bar Separator on a NavigationPage](~/xamarin-forms/platform/ios/navigation-bar-separator.md)」(NavigationPage でナビゲーション バーのセパレーターを非表示にする) を参照してください。

> [!NOTE]
> [`BackButtonTitle`](xref:Xamarin.Forms.NavigationPage.BackButtonTitleProperty)、[`Title`](xref:Xamarin.Forms.Page.Title)、[`TitleIcon`](xref:Xamarin.Forms.NavigationPage.TitleIconProperty)、[`TitleView`](xref:Xamarin.Forms.NavigationPage.TitleViewProperty) のプロパティのいずれでも、ナビゲーション バー上の領域を占める値を定義できます。 ナビゲーション バーのサイズはプラットフォームや画面サイズによって変わりますが、これらのプロパティをすべて設定すると、領域が限られているために競合が発生します。 これらのプロパティの組み合わせを使用するのではなく、`TitleView` プロパティのみ設定して目的のナビゲーション バーのデザインを改善することをお勧めします。

### <a name="limitations"></a>制限事項

[`NavigationPage`](xref:Xamarin.Forms.NavigationPage) のナビゲーション バーに [`View`](xref:Xamarin.Forms.View) を表示するときに注意が必要な制限事項がいくつかあります。

- iOS では、`NavigationPage` のナビゲーション バーに配置されるビューは、大きなタイトルが有効かどうかによって表示される位置が変わります。 大きなタイトルを有効にする方法については、「[Displaying Large Titles](~/xamarin-forms/platform/ios/page-large-title.md)」(大きなタイトルの表示) を参照してください。
- Android では、`NavigationPage` のナビゲーション バーにビューを配置することは、app-compat を使用するアプリでのみ実現できます。
- [`ListView`](xref:Xamarin.Forms.ListView) や [`TableView`](xref:Xamarin.Forms.TableView) などの大きくて複雑なビューを `NavigationPage` のナビゲーション バーに配置することはお勧めしません。

## <a name="related-links"></a>関連リンク

- [ページのナビゲーション](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [階層 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-hierarchical)
- [PassingData (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-passingdata)
- [LoginFlow (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-loginflow)
- [TitleView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-titleview)
- [Xamarin.Forms でサインイン画面のフローを作成する方法のビデオ](https://www.youtube.com/watch?v=qKQ7pyyG1fo)
- [NavigationPage](xref:Xamarin.Forms.NavigationPage)
