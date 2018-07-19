---
title: ControlTemplate の作成
description: アプリケーション レベルまたはページ レベルでは、コントロール テンプレートを定義できます。 この記事では、作成およびコントロール テンプレートを使用する方法を示します。
ms.prod: xamarin
ms.assetid: A9AEB052-FBF5-4589-9BD4-6D6F62BED7F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: b83668f6836b1d5d98f67592bf3e2b01e7319edc
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998190"
---
# <a name="creating-a-controltemplate"></a>ControlTemplate の作成

_アプリケーション レベルまたはページ レベルでは、コントロール テンプレートを定義できます。この記事では、作成およびコントロール テンプレートを使用する方法を示します。_

## <a name="creating-a-controltemplate-in-xaml"></a>XAML の ControlTemplate の作成

定義する、 [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate)アプリケーション レベル、 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)に追加する必要があります、`App`クラス。 既定では、テンプレートから作成したすべての Xamarin.Forms アプリケーションを使用して、**アプリ**を実装するクラス、 [ `Application` ](xref:Xamarin.Forms.Application)サブクラスです。 宣言する、`ControlTemplate`アプリケーションのアプリケーション レベルで`ResourceDictionary`XAML、既定値を使用して**アプリ**クラスは、XAML で置き換える必要がある**アプリ**クラスと関連付けられているコード ビハインド、として次のコード例に示します。

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.App">
    <Application.Resources>
        <ResourceDictionary>
            <ControlTemplate x:Key="TealTemplate">
                <Grid>
                    ...
                    <BoxView ... />
                    <Label Text="Control Template Demo App"
                           TextColor="White"
                           VerticalOptions="Center" ... />
                    <ContentPresenter ... />
                    <BoxView Color="Teal" ... />
                    <Label Text="(c) Xamarin 2016"
                           TextColor="White"
                           VerticalOptions="Center" ... />
                </Grid>
            </ControlTemplate>
            <ControlTemplate x:Key="AquaTemplate">
                ...
            </ControlTemplate>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

各[ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate)で再利用可能なオブジェクトとしてインスタンスを作成、 [ `ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)します。  これは、一意の各宣言指定することにより実現されます`x:Key`、属性のわかりやすいキーで利用可能になる、`ResourceDictionary`します。

次のコード例を示しています、関連付けられている`App`分離コード。

```csharp
public partial class App : Application
{
  public App ()
  {
    InitializeComponent ();
    MainPage = new HomePage ();
  }
}
```

設定と、 [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage)プロパティ、分離コードを呼び出す必要がありますも、`InitializeComponent`読み込み、関連付けられている XAML を解析するメソッド。

次のコード例は、 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)適用、`TealTemplate`を[ `ContentView` ](xref:Xamarin.Forms.ContentView):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentView x:Name="contentView" Padding="0,20,0,0"
                 ControlTemplate="{StaticResource TealTemplate}">
        <StackLayout VerticalOptions="CenterAndExpand">
            <Label Text="Welcome to the app!" HorizontalOptions="Center" />
            <Button Text="Change Theme" Clicked="OnButtonClicked" />
        </StackLayout>
    </ContentView>
</ContentPage>
```

`TealTemplate`に割り当てられている、 [ `ContentView.ControlTemplate` ](xref:Xamarin.Forms.TemplatedView.ControlTemplate)プロパティを使用して、`StaticResource`マークアップ拡張機能。 [ `ContentView.Content` ](xref:Xamarin.Forms.ContentView.Content)プロパティに設定されて、 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)に表示するコンテンツを定義する、 [ `ContentPage`](xref:Xamarin.Forms.ContentPage)します。 このコンテンツによって表示される、 [ `ContentPresenter` ](xref:Xamarin.Forms.ContentPresenter)に含まれている、`TealTemplate`します。 次のスクリーン ショットに示すように外観が発生します。

![](creating-images/teal-theme.png "青緑コントロール テンプレート")

### <a name="re-theming-an-application-at-runtime"></a>実行時にアプリケーションを再テーマ

クリックすると、**テーマを変更する**ボタンの実行、`OnButtonClicked`メソッドは、次のコード例に示されています。

```csharp
void OnButtonClicked (object sender, EventArgs e)
{
  originalTemplate = !originalTemplate;
  contentView.ControlTemplate = (originalTemplate) ? tealTemplate : aquaTemplate;
}
```

このメソッドは、アクティブなを置き換える[ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) 、代替手段でインスタンス`ControlTemplate`インスタンス、その結果、次のスクリーン ショット。

![](creating-images/aqua-theme.png "水色のコントロール テンプレート")

> [!NOTE]
> `ContentPage`、`Content`プロパティを割り当てることができる、`ControlTemplate`プロパティを設定することもできます。 ときにこれが発生した場合、`ControlTemplate`が含まれています、`ContentPresenter`インスタンスに割り当てられているコンテンツ、`Content`によってプロパティが表示されます、`ContentPresenter`内、`ControlTemplate`します。

### <a name="setting-a-controltemplate-with-a-style"></a>ControlTemplate のスタイルを設定

A [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate)を使用して適用することも、 [ `Style` ](xref:Xamarin.Forms.Style)テーマ機能をさらに展開します。 作成してこれを実現する、*暗黙的な*または*明示的な*で対象のビューのスタイルを[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)、および設定、`ControlTemplate`ターゲットのプロパティ表示、 [ `Style` ](xref:Xamarin.Forms.Style)インスタンス。 次のコード例は、*暗黙的な*はアプリケーション レベルに追加されたスタイル[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary):

```xaml
<Style TargetType="ContentView">
    <Setter Property="ControlTemplate" Value="{StaticResource TealTemplate}" />
</Style>
```

[ `Style` ](xref:Xamarin.Forms.Style)インスタンスが*暗黙的な*がすべてに適用`ContentView`アプリケーション内のインスタンス。 そのため、設定する必要はなくなりました、 [ `ContentView.ControlTemplate` ](xref:Xamarin.Forms.TemplatedView.ControlTemplate)プロパティは、次のコード例で示した。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentView x:Name="contentView" Padding="0,20,0,0">
      ...
    </ContentView>
</ContentPage>
```

スタイルの詳細については、次を参照してください。[スタイル](~/xamarin-forms/user-interface/styles/index.md)します。

### <a name="creating-a-controltemplate-at-page-level"></a>ページ レベルで ControlTemplate の作成

作成するだけでなく[ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate)アプリケーション レベルのインスタンスが作成することも、ページ レベルで次のコード例に示すようにします。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentPage.Resources>
        <ResourceDictionary>
            <ControlTemplate x:Key="TealTemplate">
                ...
            </ControlTemplate>
            <ControlTemplate x:Key="AquaTemplate">
                ...
            </ControlTemplate>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentView ... ControlTemplate="{StaticResource TealTemplate}">
        ...
    </ContentView>
</ContentPage>
```

追加するときに、 [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate)ページ レベル、 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)に追加されます、 [ `ContentPage`](xref:Xamarin.Forms.ContentPage)をクリックし、`ControlTemplate`インスタンスが含まれています`ResourceDictionary`します。

## <a name="creating-a-controltemplate-in-c35"></a>C で ControlTemplate の作成&#35;

定義する、 [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate)アプリケーション レベル、`class`を表すを作成する必要があります、`ControlTemplate`します。 クラスを派生する必要があります、[レイアウト](~/xamarin-forms/user-interface/layouts/index.md)テンプレートについては、次のコード例で示すように使用されています。

```csharp
class TealTemplate : Grid
{
  public TealTemplate ()
  {
    ...
    var contentPresenter = new ContentPresenter ();
    Children.Add (contentPresenter, 0, 1);
    Grid.SetColumnSpan (contentPresenter, 2);
    ...
  }
}

class AquaTemplate : Grid
{
  ...
}
```

`AquaTemplate`クラスのと同じですが、`TealTemplate`クラスの異なる色が使用する点を除いて、 [ `BoxView.Color` ](xref:Xamarin.Forms.BoxView.Color)と[ `Label.TextColor` ](xref:Xamarin.Forms.Label.TextColor)プロパティ。

次のコード例は、 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)適用、`TealTemplate`を[ `ContentView` ](xref:Xamarin.Forms.ContentView):

```csharp
public class HomePageCS : ContentPage
{
  ...
  ControlTemplate tealTemplate = new ControlTemplate (typeof(TealTemplate));
  ControlTemplate aquaTemplate = new ControlTemplate (typeof(AquaTemplate));

  public HomePageCS ()
  {
    var button = new Button { Text = "Change Theme" };
    var contentView = new ContentView {
      Padding = new Thickness (0, 20, 0, 0),
      Content = new StackLayout {
        VerticalOptions = LayoutOptions.CenterAndExpand,
        Children = {
          new Label { Text = "Welcome to the app!", HorizontalOptions = LayoutOptions.Center },
          button
        }
      },
      ControlTemplate = tealTemplate
    };
    ...
    Content = contentView;
  }
}
```

[ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate)で、コントロール テンプレートを定義するクラスの種類を指定してインスタンスを作成、`ControlTemplate`コンス トラクター。

[ `ContentView.Content` ](xref:Xamarin.Forms.ContentView.Content)プロパティに設定されて、 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)に表示するコンテンツを定義する、 [ `ContentPage`](xref:Xamarin.Forms.ContentPage)します。 このコンテンツによって表示される、 [ `ContentPresenter` ](xref:Xamarin.Forms.ContentPresenter)に含まれている、`TealTemplate`します。 記載されている、同じメカニズムを使用する実行時にテーマを変更するして以前、`AquaTheme`します。

## <a name="summary"></a>まとめ

この記事では、作成し、コントロール テンプレートを使用する方法を示しました。 アプリケーション レベルまたはページ レベルでは、コントロール テンプレートを定義できます。


## <a name="related-links"></a>関連リンク

- [スタイル](~/xamarin-forms/user-interface/styles/index.md)
- [単純なテーマ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simpletheme/)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentPresenter](xref:Xamarin.Forms.ContentPresenter)
- [ContentView](xref:Xamarin.Forms.ContentView)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
