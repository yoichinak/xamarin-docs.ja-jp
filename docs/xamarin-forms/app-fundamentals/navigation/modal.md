---
title: モーダル ページ
description: Xamarin.Forms はモーダル ページをサポートしています。 モーダル ページは、そのタスクが完了するかキャンセルされるまで、他の操作ができない自己完結型のタスクを完了させるようユーザーに促します。 この記事では、モーダルのページに移動する方法を示します。
ms.prod: xamarin
ms.assetid: 486CB7FD-2B9A-4DE3-94BD-C8D904E5D3C6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 30d0371e0eaa31673561ae12c7a46b7a7819a647
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847420"
---
# <a name="modal-pages"></a>モーダル ページ

_Xamarin.Forms は、モーダルのページのサポートを提供します。モーダル ページは、そのタスクが完了するかキャンセルされるまで、他の操作ができない自己完結型のタスクを完了させるようユーザーに促します。この記事では、モーダルのページに移動する方法を示します。_

この記事では、次のトピックについて説明します。

- [ナビゲーションを実行する](#Performing_Navigation)モーダル スタック、モーダル スタックからポップ ページへのページのプッシュ – を [戻る] ボタンを無効にし、ページは、遷移のアニメーション化します。
- [移動するときにデータを渡す](#Passing_Data_when_Navigating)– を使用してページのコンス トラクターでは、データを渡して、`BindingContext`です。

## <a name="overview"></a>概要

モーダル ページには、いずれかを指定できる、[ページ](~/xamarin-forms/user-interface/controls/pages.md)Xamarin.Forms でサポートされる型。 モーダル ページを表示するには、アプリケーションは、スタックにプッシュ モーダル、場所には作業中のページでは、次の図に示すように。

![](modal-images/pushing.png "ページをモーダル スタックにプッシュします。")

返す前のページに、アプリケーションがモーダルのスタックから現在のページを表示になり、新しい最上位のページ作業中のページでは、次の図に示すように。

![](modal-images/popping.png "モーダル スタックからページをポップ")

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>ナビゲーションを実行します。

モーダル ナビゲーション メソッドは、任意の [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 派生型の [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) プロパティによって公開されます。 これらのメソッドを提供する機能[モーダル ページのプッシュ](#Pushing_Pages_to_the_Modal_Stack)モーダル スタック上と[モーダル ページをポップ](#Popping_Pages_from_the_Modal_Stack)モーダル スタックからです。

[ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/)プロパティも公開、 [ `ModalStack` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.ModalStack/)プロパティ モーダル スタックでモーダル ページを取得できます。 ただし、モーダル スタックの操作を実行したり、モーダル ナビゲーションで、ルート ページにポップしたりする概念はありません。 これは、これらの操作が基になるプラットフォームで一般にサポートされていないためです。

> [!NOTE]
> [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) インスタンスは、モーダル ページ ナビゲーションの実行には不要です。

<a name="Pushing_Pages_to_the_Modal_Stack" />

### <a name="pushing-pages-to-the-modal-stack"></a>ページをモーダル スタックにプッシュします。

移動する、`ModalPage`を呼び出す必要がある、 [ `PushModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page)/)メソッドを[ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/)次のコード例に示すよう、現在のページのプロパティ。

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

これにより、`ModalPage`で項目が選択されているアクティブなページが、モーダルのスタックにプッシュされるインスタンスが提供される、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)上、`MainPage`インスタンス。 `ModalPage`インスタンスは、次のスクリーン ショットで示されます。

![](modal-images/modalpage.png "モーダル ページの例")

ときに[ `PushModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page)/)呼び出されると、次のイベントが発生します。

- ページの呼び出し元の`PushModalAsync`がその[ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/)オーバーライドが呼び出されると、基になるプラットフォームに Android がされていないこと。
- 移動先ページがその[ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/)オーバーライドが呼び出されます。
- `PushAsync`タスクが完了します。

ただし、これらのイベントが発生する正確な順序は、プラットフォームに依存します。 詳細については、次を参照してください。 [24 章](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)Charles Petzold Xamarin.Forms 書籍のです。

> [!NOTE]
> 呼び出し、 [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/)と[ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/)上書きは、ページ ナビゲーションの保証がないかとして扱うことはできません。 たとえば、iOS、上、`OnDisappearing`アプリケーションの終了時に、アクティブなページ オーバーライドが呼び出されます。

<a name="Popping_Pages_from_the_Modal_Stack" />

### <a name="popping-pages-from-the-modal-stack"></a>モーダル スタックからポップ ページ

アクティブ ページは、キーを押してモーダル スタックからポップすることができます、*戻る*に関係なく、デバイスのボタンではこれは、デバイス上の物理ボタンであるかどうか、または画面に表示されるボタンをクリックします。

元のページにプログラムを使用して戻るには、`ModalPage` インスタンスが、次のコード例のとおり [`PopModalAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/) メソッドを起動する必要があります。

```csharp
async void OnDismissButtonClicked (object sender, EventArgs args)
{
  await Navigation.PopModalAsync ();
}
```

これにより、`ModalPage`アクティブ ページになる新しい最上位のページで、モーダル スタックから削除するインスタンス。 ときに[ `PopModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/)呼び出されると、次のイベントが発生します。

- ページの呼び出し元の`PopModalAsync`がその[ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/)オーバーライドが呼び出されます。
- 返されるページがその[ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/)呼び出されると、基になるプラットフォームのない Android をオーバーライドします。
- `PopModalAsync`タスクを返します。

ただし、これらのイベントが発生する正確な順序は、プラットフォームに依存します。 詳細については、次を参照してください。 [24 章](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)Charles Petzold Xamarin.Forms 書籍のです。

### <a name="disabling-the-back-button"></a>[戻る] ボタンを無効にします。

Android でユーザー常に押して戻ることが前のページに、標準*戻る*デバイス上のボタンをクリックします。 モーダル ページでは、ページを終了する前に、自己完結型のタスクを完了する必要がある場合、アプリケーションを無効にする必要があります、*戻る*ボタンをクリックします。 これには、オーバーライドすることで、 [ `Page.OnBackButtonPressed` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnBackButtonPressed/)モーダル ページのメソッドです。 詳細については、次を参照してください。 [24 章](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)Charles Petzold Xamarin.Forms 書籍のです。

### <a name="animating-page-transitions"></a>ページは、遷移のアニメーション化

[ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/)各ページのプロパティは、オーバーライドされたプッシュおよびポップなどのメソッドにも提供、`boolean`を次のコードに示すように、ナビゲーション中にページのアニメーションを表示するかどうかを制御するパラメーター例:

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

設定、`boolean`パラメーターを`false`パラメーターを設定中に、ページ遷移アニメーションを無効に`true`基になるプラットフォームでサポートされているページ遷移アニメーションを有効にします。 ただし、このパラメーターが不足している、プッシュおよびポップ メソッドでは、既定では、アニメーションが有効にします。

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>移動するときのデータの受け渡し

ページの移動中に別のページにデータを渡すために必要な場合があります。 これを実現する 2 つの方法は、ページのコンス トラクターでは、データを渡すことによって、新しいページを設定して[ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)データにします。 各は今すぐするを順番に説明します。

### <a name="passing-data-through-a-page-constructor"></a>ページのコンス トラクターでのデータの受け渡し

ナビゲーション中に別のページにデータを渡すための最も単純な手法は、次のコード例に示されているページ コンス トラクターのパラメーターでは。

```csharp
public App ()
{
  MainPage = new MainPage (DateTime.Now.ToString ("u")));
}
```

このコードを作成、`MainPage`インスタンス、現在の日付と時刻 ISO8601 の形式で渡すことです。

`MainPage`インスタンスが次のコード例に示すように、コンス トラクター パラメーターを通じてデータを受信します。

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

設定して、ページで、データが表示されます、 [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/)プロパティです。

### <a name="passing-data-through-a-bindingcontext"></a>BindingContext を通じたデータの受け渡し

ナビゲーション中に別のページにデータを渡すための別のアプローチは、新しいページを設定して、 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)データの次のコード例に示すようにします。

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

このコードを設定、 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)の`DetailPage`インスタンスを`Contact`インスタンス、およびに移動し、`DetailPage`です。

`DetailPage`データ バインディングを使用して、表示、 `Contact` XAML コードの例を次に示すように、データをインスタンス化します。

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

次のコード例は、c# でのデータ バインディングの実現方法を示しています。

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

データが、一連のページに表示し、 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)コントロール。

データ バインディングの詳細については、「[Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/index.md)」 (データ バインディングの基礎) を参照してください。

## <a name="summary"></a>まとめ

この記事では、モーダルのページに移動する方法を示しました。 モーダル ページは、そのタスクが完了するかキャンセルされるまで、他の操作ができない自己完結型のタスクを完了させるようユーザーに促します。


## <a name="related-links"></a>関連リンク

- [ページ ナビゲーション](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [モーダル (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Modal/)
- [PassingData (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
