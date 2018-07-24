---
title: Xamarin.Forms のモーダル ページ
description: Xamarin.Forms はモーダル ページをサポートしています。 モーダル ページは、そのタスクが完了するかキャンセルされるまで、他の操作ができない自己完結型のタスクを完了させるようユーザーに促します。 この記事では、モーダル ページに移動する方法を示します。
ms.prod: xamarin
ms.assetid: 486CB7FD-2B9A-4DE3-94BD-C8D904E5D3C6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 44aee8500c7de2ae56b59049368d6025ec49cc5e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994829"
---
# <a name="xamarinforms-modal-pages"></a>Xamarin.Forms のモーダル ページ

_Xamarin.Forms はモーダル ページのサポートを提供します。モーダル ページは、そのタスクが完了するかキャンセルされるまで、他の操作ができない自己完結型のタスクを完了させるようユーザーに促します。この記事では、モーダル ページに移動する方法を示します。_

この記事では、次のトピックについて説明します。

- [ナビゲーションを実行する](#Performing_Navigation)モーダル スタック、モーダル スタックからポップをそれぞれのページにページをプッシュ –、[戻る] ボタンを無効にして、ページの切り替え効果をアニメーション化します。
- [移動するときにデータを渡す](#Passing_Data_when_Navigating)– を使用してページのコンス トラクターでは、データを渡す、`BindingContext`します。

## <a name="overview"></a>概要

モーダル ページには、いずれかを指定できる、[ページ](~/xamarin-forms/user-interface/controls/pages.md)Xamarin.Forms でサポートされる型。 モーダル ページを表示するには、アプリケーションは、スタックにプッシュ モーダル、そこでなるアクティブなページで、次の図に示すようにします。

![](modal-images/pushing.png "ページのモーダル スタックにプッシュ")

返される前のページに、アプリケーションは、モーダル スタックから現在のページをポップし、新しい最上位のページがアクティブなページで、次の図に示すように。

![](modal-images/popping.png "ページをモーダル スタックからポップ")

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>ナビゲーションを実行します。

モーダル ナビゲーション メソッドは、任意の [`Page`](xref:Xamarin.Forms.Page) 派生型の [`Navigation`](xref:Xamarin.Forms.VisualElement.Navigation) プロパティによって公開されます。 これらのメソッドを行う[モーダル ページをプッシュ](#Pushing_Pages_to_the_Modal_Stack)モーダル スタックと[モーダル ページをポップ](#Popping_Pages_from_the_Modal_Stack)モーダル スタックから。

[ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation)プロパティも公開、 [ `ModalStack` ](xref:Xamarin.Forms.INavigation.ModalStack)プロパティは、モーダル スタックのモーダル ページを取得できます。 ただし、モーダル スタックの操作を実行したり、モーダル ナビゲーションで、ルート ページにポップしたりする概念はありません。 これは、これらの操作が基になるプラットフォームで一般にサポートされていないためです。

> [!NOTE]
> [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) インスタンスは、モーダル ページ ナビゲーションの実行には不要です。

<a name="Pushing_Pages_to_the_Modal_Stack" />

### <a name="pushing-pages-to-the-modal-stack"></a>モーダル スタックにプッシュ ページ

移動する、`ModalPage`を呼び出す必要がある、 [ `PushModalAsync` ](xref:Xamarin.Forms.INavigation.PushModalAsync*)メソッドを[ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation)のコード例を次のとおり、現在のページのプロパティ。

```csharp
async void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
{
  if (listView.SelectedItem != null) {
    var detailPage = new DetailPage ();
    ...
    await Navigation.PushModalAsync (detailPage);
  }
}
```

これにより、`ModalPage`で項目が選択されているアクティブなページが、モーダル スタックにプッシュされるインスタンスが提供される、 [ `ListView` ](xref:Xamarin.Forms.ListView)上、`MainPage`インスタンス。 `ModalPage`インスタンスは、次のスクリーン ショットで示されます。

![](modal-images/modalpage.png "モーダル ページの例")

ときに[ `PushModalAsync` ](xref:Xamarin.Forms.INavigation.PushModalAsync*)呼び出されると、次のイベントが発生します。

- 呼び出し元ページ`PushModalAsync`がその[ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing)オーバーライドされるとき、基になるプラットフォームのない Android 呼び出されます。
- 移動先ページがその[ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing)オーバーライドが呼び出されます。
- `PushAsync`タスクが完了します。

ただし、これらのイベントが発生する正確な順序は、プラットフォームに依存します。 詳細については、次を参照してください。[第 24 章](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)Charles Petzold の Xamarin.Forms book の。

> [!NOTE]
> 呼び出し、 [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing)と[ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing)上書きは、ページ ナビゲーションの保証がないとして扱うことはできません。 たとえば、iOS、上、`OnDisappearing`アプリケーションの終了時に、アクティブなページ オーバーライドが呼び出されます。

<a name="Popping_Pages_from_the_Modal_Stack" />

### <a name="popping-pages-from-the-modal-stack"></a>モーダル スタックからポップをそれぞれのページ

アクティブなページは、キーを押して、モーダル スタックからポップできます、*戻る*に関係なく、デバイスのボタンではこれは、デバイス上の物理ボタンであるかどうか、または画面上のボタンします。

元のページにプログラムを使用して戻るには、`ModalPage` インスタンスが、次のコード例のとおり [`PopModalAsync`](xref:Xamarin.Forms.INavigation.PopModalAsync) メソッドを起動する必要があります。

```csharp
async void OnDismissButtonClicked (object sender, EventArgs args)
{
  await Navigation.PopModalAsync ();
}
```

これにより、`ModalPage`アクティブ ページになる新しい最上位のページで、モーダル スタックから削除するインスタンス。 ときに[ `PopModalAsync` ](xref:Xamarin.Forms.INavigation.PopModalAsync)呼び出されると、次のイベントが発生します。

- 呼び出し元ページ`PopModalAsync`がその[ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing)オーバーライドが呼び出されます。
- 返されるページがその[ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing)オーバーライドされるとき、基になるプラットフォームのない Android 呼び出されます。
- `PopModalAsync`タスクを返します。

ただし、これらのイベントが発生する正確な順序は、プラットフォームに依存します。 詳細については、次を参照してください。[第 24 章](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)Charles Petzold の Xamarin.Forms book の。

### <a name="disabling-the-back-button"></a>[戻る] ボタンを無効にします。

Android では、ユーザー常に押して戻ることが前のページに、標準*戻る*デバイス上のボタンをクリックします。 モーダル ページは、ページを終了する前に、自己完結型のタスクを完了するユーザーを必要とする場合、アプリケーションを無効にする必要があります、*戻る*ボタンをクリックします。 これは、オーバーライドすることで実現できます、 [ `Page.OnBackButtonPressed` ](xref:Xamarin.Forms.Page.OnBackButtonPressed)モーダル ページ上のメソッド。 詳細については、次を参照してください。[第 24 章](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)Charles Petzold の Xamarin.Forms book の。

### <a name="animating-page-transitions"></a>ページの切り替え効果をアニメーション化

[ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation)各ページのプロパティは、オーバーライドされたプッシュおよびポップ メソッドが含まれているにも提供します、`boolean`に次のコードに示すように、ナビゲーション中にページのアニメーションを表示するかどうかを制御するパラメーター例:

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PushModalAsync (new DetailPage (), false);
}

async void OnDismissButtonClicked (object sender, EventArgs args)
{
  // Page appearance not animated
  await Navigation.PopModalAsync (false);
}
```

設定、`boolean`パラメーターを`false`パラメータを設定するときに、ページ遷移アニメーションを無効にします。`true`基になるプラットフォームでサポートされている、ページ遷移アニメーションを使用します。 ただし、このパラメーターを持たない push および pop メソッドは、既定では、アニメーションを有効にします。

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>移動するときにデータの受け渡し

ページの移動中に別のページにデータを渡すために必要な場合があります。 これを行うために 2 つの手法は、新しいページの設定とページのコンス トラクターでは、データを渡すことによって[ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)データにします。 さらに説明するようになりましたが。

### <a name="passing-data-through-a-page-constructor"></a>ページのコンス トラクターを通じてデータの受け渡し

別のページへの移動中にデータを渡すための最も単純な手法は、次のコード例に示されているページのコンス トラクターのパラメーターでは。

```csharp
public App ()
{
  MainPage = new MainPage (DateTime.Now.ToString ("u")));
}
```

このコードを作成、`MainPage`インスタンス、現在の日付と時刻 ISO8601 形式で渡します。

`MainPage`インスタンスが次のコード例に示すように、コンス トラクターのパラメーターを使用してデータを受信します。

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

データが設定ページに表示し、 [ `Label.Text` ](xref:Xamarin.Forms.Label.Text)プロパティ。

### <a name="passing-data-through-a-bindingcontext"></a>BindingContext を通じたデータの受け渡し

別のページへの移動中にデータを渡すための別のアプローチは、新しいページを設定して、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)データの次のコード例に示すようにします。

```csharp
async void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
{
  if (listView.SelectedItem != null) {
    var detailPage = new DetailPage ();
    detailPage.BindingContext = e.SelectedItem as Contact;
    listView.SelectedItem = null;
    await Navigation.PushModalAsync (detailPage);
  }
}
```

このコードを設定、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)の`DetailPage`インスタンスを`Contact`インスタンス、およびに移動し、`DetailPage`します。

`DetailPage`データ バインディングを使用して、表示、`Contact`の XAML コードの例を次に示すように、データをインスタンスします。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ModalNavigation.DetailPage">
    <ContentPage.Padding>
      <OnPlatform x:TypeArguments="Thickness">
        <On Platform="iOS" Value="0,40,0,0" />
      </OnPlatform>
    </ContentPage.Padding>
    <ContentPage.Content>
        <StackLayout HorizontalOptions="Center" VerticalOptions="Center">
            <StackLayout Orientation="Horizontal">
                <Label Text="Name:" FontSize="Medium" HorizontalOptions="FillAndExpand" />
                <Label Text="{Binding Name}" FontSize="Medium" FontAttributes="Bold" />
            </StackLayout>
              ...
            <Button x:Name="dismissButton" Text="Dismiss" Clicked="OnDismissButtonClicked" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

次のコード例では、データ バインディングを実現 (C#) する方法を示します。

```csharp
public class DetailPageCS : ContentPage
{
  public DetailPageCS ()
  {
    var nameLabel = new Label {
      FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
      FontAttributes = FontAttributes.Bold
    };
    nameLabel.SetBinding (Label.TextProperty, "Name");
    ...
    var dismissButton = new Button { Text = "Dismiss" };
    dismissButton.Clicked += OnDismissButtonClicked;

    Thickness padding;
    switch (Device.RuntimePlatform)
    {
        case Device.iOS:
            padding = new Thickness(0, 40, 0, 0);
            break;
        default:
            padding = new Thickness();
            break;
    }

    Padding = padding;
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
        dismissButton
      }
    };
  }

  async void OnDismissButtonClicked (object sender, EventArgs args)
  {
    await Navigation.PopModalAsync ();
  }
}
```

データが、一連のページに表示し、 [ `Label` ](xref:Xamarin.Forms.Label)コントロール。

データ バインディングの詳細については、「[Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/index.md)」 (データ バインディングの基礎) を参照してください。

## <a name="summary"></a>まとめ

この記事では、モーダル ページに移動する方法を示しました。 モーダル ページは、そのタスクが完了するかキャンセルされるまで、他の操作ができない自己完結型のタスクを完了させるようユーザーに促します。


## <a name="related-links"></a>関連リンク

- [ページ ナビゲーション](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [モーダル (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Modal/)
- [PassingData (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
