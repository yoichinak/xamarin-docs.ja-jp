---
title: "階層ナビゲーション"
description: "NavigationPage クラスでは、ここで、ユーザーは forwards と backwards、必要に応じて、ページ間を移動することが階層的なナビゲーション エクスペリエンスを提供します。 クラスでは、ページのオブジェクトの後入れ先出し (LIFO) スタックとしてナビゲーションを実装します。 この記事では、NavigationPage クラスを使用して、ページの履歴内のナビゲーションを実行する方法を示します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: C8A5EEFF-5A3B-4163-838A-147EE3939FAA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2017
ms.openlocfilehash: 95fc958beb71cba8f4d575eaa96d0612aa458966
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="hierarchical-navigation"></a>階層ナビゲーション

_NavigationPage クラスでは、ここで、ユーザーは forwards と backwards、必要に応じて、ページ間を移動することが階層的なナビゲーション エクスペリエンスを提供します。クラスでは、ページのオブジェクトの後入れ先出し (LIFO) スタックとしてナビゲーションを実装します。この記事では、NavigationPage クラスを使用して、ページの履歴内のナビゲーションを実行する方法を示します。_

この記事では、次のトピックについて説明します。

- [ナビゲーションを実行する](#Performing_Navigation)– ルート ページを作成する、ページをナビゲーション スタックにプッシュ、ページ ナビゲーション スタックからポップおよびページは、遷移のアニメーション化します。
- [移動するときにデータを渡す](#Passing_Data_when_Navigating)– を使用してページのコンス トラクターでは、データを渡して、`BindingContext`です。
- [ナビゲーション スタックを操作する](#Manipulating_the_Navigation_Stack)– 挿入または削除するページで、スタックを操作します。

## <a name="overview"></a>概要

1 つのページから移動する、アプリケーションは場所には作業中のページでは、次の図に示すようをナビゲーション スタック上に新しいページにプッシュします。

![](hierarchical-images/pushing.png "ページをナビゲーション スタックにプッシュします。")

戻る、前のページに戻り、アプリケーションのナビゲーション スタックから現在のページが表示になり、新しい最上位のページ作業中のページでは、次の図に示すように。

![](hierarchical-images/popping.png "ページ ナビゲーション スタックからポップ")

ナビゲーション メソッドは、によって公開される、 [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/)いずれかのプロパティ[ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)派生型です。 これらのメソッドは、ナビゲーション スタックでは、ページをナビゲーション スタックからポップのページにプッシュし、スタック操作を実行する機能を提供します。

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>ナビゲーションを実行します。

階層ナビゲーションでは、[`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) クラスは [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) オブジェクトのスタック間をナビゲートするために使用されます。 次のスクリーン ショットの主要なコンポーネントを表示する、`NavigationPage`プラットフォームごとに。

![](hierarchical-images/navigationpage-components.png "NavigationPage コンポーネント")

レイアウト、 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)はプラットフォームに依存します。

- ナビゲーション バーは、タイトルを表示して、あるページの上部にある、ios の場合、*戻る*を前のページに返すボタンをクリックします。
- Android でナビゲーション バーは、タイトル、アイコンに表示されるページの上部にあると*戻る*を前のページに返すボタンをクリックします。 アイコンがで定義されている、`[Activity]`を装飾する属性、 `MainActivity` Android プラットフォームに固有のプロジェクト内のクラスです。
- Windows Phone のナビゲーション バーは、タイトルを表示するページの上部に存在します。 Windows Phone がない、*戻る*ため、ナビゲーション バーのボタンで、画面に表示される*戻る* ボタンが画面の下部に表示します。

すべてのプラットフォームの値で、 [ `Page.Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Title/)プロパティは、ページ タイトルとして表示されます。

> [!NOTE]
> お勧めする、`NavigationPage`を代入する`ContentPage`インスタンスのみです。

### <a name="creating-the-root-page"></a>ルート ページを作成します。

ナビゲーション スタックに追加された最初のページは、アプリケーションの*ルート* ページとなります。次のコード例に、これを実現する方法を示しています。

```csharp
public App ()
{
  MainPage = new NavigationPage (new Page1Xaml ());
}
```

これにより、 `Page1Xaml` [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)になったアクティブなページと、アプリケーションのルート ページ ナビゲーション スタックにプッシュされるインスタンスです。 これは、次のスクリーン ショットで示されます。

![](hierarchical-images/mainpage.png "ナビゲーション スタックのルート ページ")

> [!NOTE]
> [ `RootPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.RootPage/)のプロパティ、 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)インスタンスが移動スタック内の最初のページへのアクセスを提供します。

### <a name="pushing-pages-to-the-navigation-stack"></a>ページをナビゲーション スタックにプッシュします。

移動する`Page2Xaml`を呼び出す必要がある、 [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/)メソッドを[ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/)次のコード例に示すよう、現在のページのプロパティ。

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new Page2Xaml ());
}
```

これにより、`Page2Xaml` インスタンスがナビゲーション スタックにプッシュされるようになり、そこがアクティブ ページとなります。 これは、次のスクリーン ショットで示されます。

![](hierarchical-images/secondpage.png "ページがナビゲーションのスタックにプッシュされます。")

ときに、 [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/)メソッドが呼び出され、次のイベントが発生します。

- ページの呼び出し元の`PushAsync`がその[ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/)オーバーライドが呼び出されます。
- 移動先ページがその[ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/)オーバーライドが呼び出されます。
- `PushAsync`タスクが完了します。

ただし、これらのイベントが発生する正確な順序は、プラットフォームに依存します。 詳細については、次を参照してください。 [24 章](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)Charles Petzold Xamarin.Forms 書籍のです。

> [!NOTE]
> 呼び出し、 [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/)と[ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/)上書きは、ページ ナビゲーションの保証がないかとして扱うことはできません。 たとえば、iOS、上、`OnDisappearing`アプリケーションの終了時に、アクティブなページ オーバーライドが呼び出されます。

### <a name="popping-pages-from-the-navigation-stack"></a>ナビゲーション スタックからポップ ページ

アクティブ ページは、これが物理的なボタンであるか画面上のボタンであるかどうかにかかわらず、デバイスの *[戻る]* ボタンを押すことによってナビゲーション スタックからポップすることができます。

元のページにプログラムを使用して戻るには、`Page2Xaml` インスタンスが、次のコード例のとおり [`PopAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) メソッドを起動する必要があります。

```csharp
async void OnPreviousPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopAsync ();
}
```

これにより、ナビゲーション スタックから `Page2Xaml` インスタンスが削除され、新しい最上位のページがアクティブ ページとなります。 ときに、 [ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/)メソッドが呼び出され、次のイベントが発生します。

- ページの呼び出し元の`PopAsync`がその[ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/)オーバーライドが呼び出されます。
- 返されるページがその[ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/)オーバーライドが呼び出されます。
- `PopAsync`タスクを返します。

ただし、これらのイベントが発生する正確な順序は、プラットフォームに依存します。 詳細については、次を参照してください。 [24 章](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)Charles Petzold Xamarin.Forms 書籍のです。

だけでなく[ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/)と[ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) 、メソッド、 [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/)各ページのプロパティも用意されています、 [ `PopToRootAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopToRootAsync()/)メソッドは、次のコード例に示されています。

```csharp
async void OnRootPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopToRootAsync ();
}
```

このメソッドを除くすべてのルート ポップ[ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)ナビゲーション スタックからポップ アクティブ ページ、アプリケーションのルート ページにそのため、します。

### <a name="animating-page-transitions"></a>ページは、遷移のアニメーション化

[ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/)各ページのプロパティは、オーバーライドされたプッシュおよびポップなどのメソッドにも提供、`boolean`を次のコードに示すように、ナビゲーション中にページのアニメーションを表示するかどうかを制御するパラメーター例:

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

設定、`boolean`パラメーターを`false`パラメーターを設定中に、ページ遷移アニメーションを無効に`true`基になるプラットフォームでサポートされているページ遷移アニメーションを有効にします。 ただし、このパラメーターが不足している、プッシュおよびポップ メソッドでは、既定では、アニメーションが有効にします。

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>移動するときのデータの受け渡し

ページの移動中に別のページにデータを渡すために必要な場合があります。 これを実現する 2 つの方法はデータ ページのコンス トラクター、および新しいページの設定を渡している[ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)データにします。 各は今すぐするを順番に説明します。

### <a name="passing-data-through-a-page-constructor"></a>ページのコンス トラクターでのデータの受け渡し

ナビゲーション中に別のページにデータを渡すための最も単純な手法は、次のコード例に示されているページ コンス トラクターのパラメーターでは。

```csharp
public App ()
{
  MainPage = new NavigationPage (new MainPage (DateTime.Now.ToString ("u")));
}
```

このコードを作成、`MainPage`インスタンスにラップされて、現在の日付と時刻 ISO8601 の形式で渡すこと、 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)インスタンス。

`MainPage`インスタンスが次のコード例に示すように、コンス トラクター パラメーターを通じてデータを受信します。

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

設定して、ページで、データが表示されます、 [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/)プロパティ、次のスクリーン ショットに示すようにします。

![](hierarchical-images/passing-data-constructor.png "ページのコンス トラクターで渡されるデータ")

### <a name="passing-data-through-a-bindingcontext"></a>BindingContext を通じたデータの受け渡し

ナビゲーション中に別のページにデータを渡すための別のアプローチは、新しいページを設定して、 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)データの次のコード例に示すようにします。

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

このコードを設定、 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)の`SecondPage`インスタンスを`Contact`インスタンス、およびに移動し、`SecondPage`です。

`SecondPage`データ バインディングを使用して、表示、 `Contact` XAML コードの例を次に示すように、データをインスタンス化します。

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

次のコード例は、c# でのデータ バインディングの実現方法を示しています。

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

データが、一連のページに表示し、 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)を制御する次のスクリーン ショットに示すようにします。

![](hierarchical-images/passing-data-bindingcontext.png "BindingContext を通じて渡されるデータ")

データ バインディングの詳細については、「[Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)」 (データ バインディングの基礎) を参照してください。

<a name="Manipulating_the_Navigation_Stack" />

## <a name="manipulating-the-navigation-stack"></a>ナビゲーション スタックを操作します。

[ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/)プロパティが公開する[ `NavigationStack` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.NavigationStack/)プロパティのナビゲーション スタック内のページを取得できます。 Xamarin.Forms ナビゲーション スタックへのアクセスを管理するときに、`Navigation`プロパティでは、 [ `InsertPageBefore` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page)/)と[ `RemovePage` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page)/)を挿入することによって、スタックを操作するメソッドページまたはそれらを削除します。

[ `InsertPageBefore` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page)/)メソッドは、次の図に示すように、既存の指定したページの前に、ナビゲーション スタックで指定したページを挿入します。

![](hierarchical-images/insert-page-before.png "移動スタック内のページを挿入します。")

[ `RemovePage` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page)/)メソッドは、次の図に示すように、ナビゲーション スタックから指定したページを削除します。

![](hierarchical-images/remove-page.png "ナビゲーション スタックからページを削除します。")

これらのメソッドは、新しいページで、次のログインに成功すると、ログイン ページを置換などのカスタム ナビゲーション エクスペリエンスを有効にします。 次のコード例では、このシナリオを示しています。

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

ユーザーの資格情報が正しいこと、`MainPage`インスタンスが現在のページの前に、ナビゲーション スタックに挿入します。 [ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/)メソッドを持つナビゲーション スタックから現在のページを削除、`MainPage`アクティブ ページになるインスタンスです。

## <a name="summary"></a>まとめ

この記事には、使用する方法が示されている、 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)をページの履歴内のナビゲーションを実行するクラス。 このクラスは、ここで、ユーザーは forwards と backwards、必要に応じて、ページ間を移動することが階層的なナビゲーション エクスペリエンスを提供します。 このクラスは、[`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) オブジェクトの後入れ先出し (LIFO) スタックとしてナビゲーションを提供します。


## <a name="related-links"></a>関連リンク

- [ページ ナビゲーション](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [階層 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/)
- [PassingData (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
- [LoginFlow (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)
- [Xamarin.Forms (Xamarin 大学ビデオ) サンプルでは画面のフローで、記号を作成する方法](http://xamarinuniversity.blob.core.windows.net/lightninglectures/CreateASignIn.zip)
- [Xamarin.Forms (Xamarin 大学ビデオ) で画面フローで、記号を作成する方法](https://university.xamarin.com/lightninglectures/how-to-create-a-sign-in-screen-flow-in-xamarinforms)
- [NavigationPage](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)
