---
title: Xamarin.Forms モーダル ページ
description: Xamarin.Forms はモーダル ページをサポートしています。 モーダル ページは、そのタスクが完了するかキャンセルされるまで、他の操作ができない自己完結型のタスクを完了させるようユーザーに促します。 この記事では、モーダル ページに移動する方法について説明します。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4f6547049f2801e5d15115c0ae80af9a07034731
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137827"
---
# <a name="xamarinforms-modal-pages"></a>Xamarin.Forms モーダル ページ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-modal)

_Xamarin.Forms はモーダル ページをサポートしています。モーダル ページは、そのタスクが完了するかキャンセルされるまで、他の操作ができない自己完結型のタスクを完了させるようユーザーに促します。この記事では、モーダル ページに移動する方法について説明します。_

この記事では、次のトピックについて説明します。

- [ナビゲーションを実行する](#Performing_Navigation): モーダル スタックにページをプッシュし、モーダル スタックからページをポップし、戻るボタンを無効にし、ページ遷移をアニメーション化します。
- [ナビゲーション時にデータを渡す](#Passing_Data_when_Navigating): ページ コンストラクターと `BindingContext` を介してデータを渡します。

## <a name="overview"></a>概要

モーダル ページは、Xamarin.Forms でサポートされている任意の [Page](~/xamarin-forms/user-interface/controls/pages.md) の種類にすることができます。 次の図に示すように、モーダル ページを表示するために、アプリケーションからモーダル スタックにプッシュされ、アクティブ ページになります。

![](modal-images/pushing.png "Pushing a Page to the Modal Stack")

次の図に示すように、前のページに戻るために、アプリケーションでは現在のページがモーダル スタックからポップされ、新しい最上位のページがアクティブ ページになります。

![](modal-images/popping.png "Popping a Page from the Modal Stack")

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>ナビゲーションを実行する

モーダル ナビゲーション メソッドは、任意の [`Page`](xref:Xamarin.Forms.Page) 派生型の [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) プロパティによって公開されます。 これらのメソッドは、モーダル スタックに[モーダル ページをプッシュし](#Pushing_Pages_to_the_Modal_Stack)、モーダル スタックから[モーダル ページをポップする](#Popping_Pages_from_the_Modal_Stack)機能を提供します。

[`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) プロパティでは、モーダル スタック内のモーダル ページを取得する [`ModalStack`](xref:Xamarin.Forms.INavigation.ModalStack) プロパティも公開されています。 ただし、モーダル スタックの操作を実行したり、モーダル ナビゲーションで、ルート ページにポップしたりする概念はありません。 これは、これらの操作が基になるプラットフォームで一般にサポートされていないためです。

> [!NOTE]
> [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) インスタンスは、モーダル ページ ナビゲーションの実行には不要です。

<a name="Pushing_Pages_to_the_Modal_Stack" />

### <a name="pushing-pages-to-the-modal-stack"></a>モーダル スタックにページをプッシュする

`ModalPage` にナビゲートするには、次のコード例で示すように、現在のページの [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) プロパティで [`PushModalAsync`](xref:Xamarin.Forms.INavigation.PushModalAsync*) メソッドを起動する必要があります。

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

これにより、`MainPage` インスタンス上の [`ListView`](xref:Xamarin.Forms.ListView) で項目が選択されている場合は、`ModalPage` インスタンスがモーダル スタックにプッシュされ、アクティブ ページになります。 次のスクリーンショットに `ModalPage` インスタンスを示します。

![](modal-images/modalpage.png "Modal Page Example")

[`PushModalAsync`](xref:Xamarin.Forms.INavigation.PushModalAsync*) が呼び出されると、次のイベントが発生します。

- 基となるプラットフォームが Android ではない場合、`PushModalAsync` を呼び出すページでは、[`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) のオーバーライドが呼び出されます。
- ナビゲート先のページでは、[`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) のオーバーライドが呼び出されます。
- `PushAsync` タスクが完了します。

ただし、これらのイベントが発生する正確な順序はプラットフォームによって異なります。 詳細については、Charles Petzold 氏著作の Xamarin.Forms ブックの[第 24 章](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)を参照してください。

> [!NOTE]
> [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) および [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) のオーバーライドの呼び出しは、ページ ナビゲーションを示す保証として扱うことはできません。 たとえば、iOS では、アプリケーションの終了時にアクティブ ページで `OnDisappearing` のオーバーライドが呼び出されます。

<a name="Popping_Pages_from_the_Modal_Stack" />

### <a name="popping-pages-from-the-modal-stack"></a>モーダル スタックからページをポップする

アクティブ ページは、これが物理的なボタンであるか画面上のボタンであるかどうかにかかわらず、デバイスの *[戻る]* ボタンを押すことによってモーダル スタックからポップすることができます。

元のページにプログラムを使用して戻るには、`ModalPage` インスタンスが、次のコード例のとおり [`PopModalAsync`](xref:Xamarin.Forms.INavigation.PopModalAsync) メソッドを起動する必要があります。

```csharp
async void OnDismissButtonClicked (object sender, EventArgs args)
{
  await Navigation.PopModalAsync ();
}
```

これにより、モーダル スタックから `ModalPage` インスタンスが削除され、新しい最上位のページがアクティブ ページとなります。 [`PopModalAsync`](xref:Xamarin.Forms.INavigation.PopModalAsync) が呼び出されると、次のイベントが発生します。

- `PopModalAsync` を呼び出すページでは、[`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) のオーバーライドが呼び出されます。
- 基となるプラットフォームが Android ではない場合、戻り先のページでは、[`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) のオーバーライドが呼び出されます。
- `PopModalAsync` タスクが復帰します。

ただし、これらのイベントが発生する正確な順序はプラットフォームによって異なります。 詳細については、Charles Petzold 氏著作の Xamarin.Forms ブックの[第 24 章](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)を参照してください。

### <a name="disabling-the-back-button"></a>[戻る] ボタンを無効にする

Android では、デバイスの標準の *[戻る]* ボタンを押して、いつでも前のページに戻ることができます。 モーダル ページで、ユーザーがページから離れる前に自己完結型タスクを完了する必要がある場合は、アプリケーションで *[戻る]* ボタンを無効にする必要があります。 これを実現するには、モーダル ページの [`Page.OnBackButtonPressed`](xref:Xamarin.Forms.Page.OnBackButtonPressed) メソッドをオーバーライドします。 詳細については、Charles Petzold 氏著作の Xamarin.Forms ブックの[第 24 章](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)を参照してください。

### <a name="animating-page-transitions"></a>ページ遷移をアニメーション化する

各ページの [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) プロパティには、次のコード例に示すように、ナビゲーション中にページ アニメーションを表示するかどうかを制御する `boolean` パラメーターを含むオーバーライドされたプッシュおよびポップ メソッドも用意されています。

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

`boolean` パラメーターを `false` に設定すると、ページ遷移アニメーションが無効になります。また、パラメーターを `true` に設定すると、基となるプラットフォームでサポートされている場合はページ遷移アニメーションが有効になります。 ただし、プッシュとポップのメソッドでこのパラメーターが指定されていない場合は、既定でアニメーションが有効になります。

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>ナビゲーション時にデータを渡す

場合によっては、ナビゲーション中に、あるページから別のページにデータを渡す必要があります。 これを実現する 2 つの手法では、ページ コンストラクターを介してデータを渡し、新しいページの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) をデータに設定しています。 これから、それぞれについて順番に説明します。

### <a name="passing-data-through-a-page-constructor"></a>ページ コンストラクターを介してデータを渡す

ナビゲーション中に別のページにデータを渡す最も簡単な手法は、次のコード例で示すように、ページ コンストラクター パラメーターを使用することです。

```csharp
public App ()
{
  MainPage = new MainPage (DateTime.Now.ToString ("u")));
}
```

このコードでは、現在の日付と時刻を ISO8601 形式で渡す `MainPage` インスタンスを作成します。

`MainPage` インスタンスは、次のコード例に示すように、コンストラクター パラメーターを使用してデータを受け取ります。

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

データは、[`Label.Text`](xref:Xamarin.Forms.Label.Text) プロパティを設定することでページに表示されます。

### <a name="passing-data-through-a-bindingcontext"></a>BindingContext を介してデータを渡す

ナビゲーション中に別のページにデータを渡すもう 1 つの方法は、次のコード例に示すように、新しいページの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) をデータに設定することです。

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

このコードでは、`DetailPage` インスタンスの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) を `Contact` インスタンスに設定し、`DetailPage` にナビゲートします。

次の XAML コード例に示すように、`DetailPage` ではデータ バインディングを使用して `Contact` インスタンス データを表示します。

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

次のコード例は、C# でデータ バインディングを実行する方法を示しています。

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

データは、一連の [`Label`](xref:Xamarin.Forms.Label) コントロールによってページに表示されます。

データ バインディングの詳細については、「[Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/index.md)」 (データ バインディングの基礎) を参照してください。

## <a name="summary"></a>まとめ

この記事では、モーダル ページに移動する方法について説明しました。 モーダル ページは、そのタスクが完了するかキャンセルされるまで、他の操作ができない自己完結型のタスクを完了させるようユーザーに促します。

## <a name="related-links"></a>関連リンク

- [ページのナビゲーション](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [モーダル (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-modal)
- [PassingData (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-passingdata)
