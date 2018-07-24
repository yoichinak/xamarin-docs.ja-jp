---
title: 階層ナビゲーション
description: この記事では、NavigationPage クラスを使用して、後入れ先出し (LIFO) のページのスタックでナビゲーションを実行する方法を示します。
ms.prod: xamarin
ms.assetid: C8A5EEFF-5A3B-4163-838A-147EE3939FAA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2017
ms.openlocfilehash: f8f8f9b4e5755e8b1707178fef633321b64e4e94
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994677"
---
# <a name="hierarchical-navigation"></a>階層ナビゲーション

_NavigationPage クラスでは、ユーザーが forwards と backwards、必要に応じて、ページを移動できる階層ナビゲーション エクスペリエンスを提供します。クラスは、ページのオブジェクトの後入れ先出し (LIFO) スタックとしてナビゲーションを実装します。この記事では、NavigationPage クラスを使用して、ページのスタックでナビゲーションを実行する方法を示します。_

この記事では、次のトピックについて説明します。

- [ナビゲーションを実行する](#Performing_Navigation)– ルート ページの作成、ページをナビゲーション スタックにプッシュ、ページをナビゲーション スタックからポップおよびページの切り替え効果をアニメーション化します。
- [移動するときにデータを渡す](#Passing_Data_when_Navigating)– を使用してページのコンス トラクターでは、データを渡す、`BindingContext`します。
- [ナビゲーション スタックを操作する](#Manipulating_the_Navigation_Stack)– スタックを操作するには、挿入、または削除したりします。

## <a name="overview"></a>概要

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

[ `InsertPageBefore` ](xref:Xamarin.Forms.INavigation.InsertPageBefore*)メソッドは、次の図に示すように既存の指定ページの前に、ナビゲーション スタックで指定したページを挿入します。

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

## <a name="summary"></a>まとめ

この記事では、使用する方法を示しました、 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)ページ スタックのナビゲーションを実行するクラス。 このクラスは、ユーザーが forwards と backwards、必要に応じて、ページを移動できる階層ナビゲーション エクスペリエンスを提供します。 このクラスは、[`Page`](xref:Xamarin.Forms.Page) オブジェクトの後入れ先出し (LIFO) スタックとしてナビゲーションを提供します。


## <a name="related-links"></a>関連リンク

- [ページ ナビゲーション](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [階層 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/)
- [PassingData (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
- [LoginFlow (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)
- [Xamarin.Forms (Xamarin University のビデオ) サンプルでは画面のフローで、記号を作成する方法](http://xamarinuniversity.blob.core.windows.net/lightninglectures/CreateASignIn.zip)
- [Xamarin.Forms (Xamarin University のビデオ) でフローを画面で、記号を作成する方法](https://university.xamarin.com/lightninglectures/how-to-create-a-sign-in-screen-flow-in-xamarinforms)
- [NavigationPage](xref:Xamarin.Forms.NavigationPage)
