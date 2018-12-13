---
title: ControlTemplate の作成
description: コントロール テンプレートは、アプリケーション レベルまたはページ レベルで定義できます。 この記事では、コントロール テンプレートを作成して使用する方法を示します。
ms.prod: xamarin
ms.assetid: A9AEB052-FBF5-4589-9BD4-6D6F62BED7F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: b83668f6836b1d5d98f67592bf3e2b01e7319edc
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998190"
---
# <a name="creating-a-controltemplate"></a>ControlTemplate の作成

"_コントロール テンプレートは、アプリケーション レベルまたはページ レベルで定義できます。この記事では、コントロール テンプレートを作成して使用する方法を示します。_"

## <a name="creating-a-controltemplate-in-xaml"></a>XAML を使用した ControlTemplate の作成

[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) をアプリケーション レベルで定義するには、[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) を `App` クラスに追加する必要があります。 既定では、テンプレートから作成したすべての Xamarin.Forms アプリケーションでは、**App** クラスを使用して [`Application`](xref:Xamarin.Forms.Application) サブクラスが実装されます。 `ControlTemplate` をアプリケーション レベルで宣言するには、次のコード例に示すように、XAML を使用するアプリケーションの `ResourceDictionary` 内で、既定の **App** クラスを XAML の **App** クラスとそれに関連する分離コードに置き換える必要があります。

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

再利用可能オブジェクトとして、[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 内に各 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) インスタンスが作成されます。  これは、`ResourceDictionary` 内に説明的なキーを提供する一意の `x:Key` 属性が各宣言に指定されることによって実現されます。

次のコード例で、関連する `App` 分離コードを示します。

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

[`MainPage`](xref:Xamarin.Forms.Application.MainPage) プロパティの設定だけでなく、分離コードで `InitializeComponent` メソッドを呼び出して、関連付けられている XAML を読み込んで解析する必要があります。

次のコード例で、`TealTemplate` を [`ContentView`](xref:Xamarin.Forms.ContentView) に適用する[`ContentPage`](xref:Xamarin.Forms.ContentPage) を示します。

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

`StaticResource` マークアップ拡張を使用して、`TealTemplate` が [`ContentView.ControlTemplate`](xref:Xamarin.Forms.TemplatedView.ControlTemplate) プロパティに割り当てられます。 [`ContentView.Content`[`ContentPage`](xref:Xamarin.Forms.ContentPage) に表示されるコンテンツを定義する [`StackLayout`](xref:Xamarin.Forms.StackLayout) に、](xref:Xamarin.Forms.ContentView.Content) プロパティが設定されます。 `TealTemplate` に含まれている [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter) によって、このコンテンツが表示されます。 この結果、次のスクリーンショットに示すような外観が表示されます。

![](creating-images/teal-theme.png "青緑コントロール テンプレート")

### <a name="re-theming-an-application-at-runtime"></a>実行時のアプリケーションのテーマ変更

**[テーマの変更]** ボタンをクリックすると、次のコード例に示す `OnButtonClicked` メソッドが実行されます。

```csharp
void OnButtonClicked (object sender, EventArgs e)
{
  originalTemplate = !originalTemplate;
  contentView.ControlTemplate = (originalTemplate) ? tealTemplate : aquaTemplate;
}
```

このメソッドで、アクティブな [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) インスタンスが別の `ControlTemplate` インスタンスに置き替えられ、次のスクリーンショットに示す結果になります。

![](creating-images/aqua-theme.png "水色コントロール テンプレート")

> [!NOTE]
> `ContentPage` に `Content` プロパティを割り当てることができ、さらに `ControlTemplate` プロパティも割り当てることができます。 これが発生するときに、`ControlTemplate` に `ContentPresenter` インスタンスが含まれている場合は、`ControlTemplate` 内の `ContentPresenter` によって `Content` プロパティに割り当てられたコンテンツが表示されます。

### <a name="setting-a-controltemplate-with-a-style"></a>スタイルを使用する ControlTemplate の設定

テーマの能力をさらに拡張するために、[`Style`](xref:Xamarin.Forms.Style) を使用して [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) を適用することもできます。 これは、[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 内にターゲット ビュー用の "*暗黙的*" または "*明示的*" なスタイルを作成し、ターゲット ビューの `ControlTemplate` プロパティを [`Style`](xref:Xamarin.Forms.Style) インスタンス内に設定することで実現できます。 次のコード例で、アプリケーション レベルの [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) に追加されている "*暗黙的な*" スタイルを示します。

```xaml
<Style TargetType="ContentView">
    <Setter Property="ControlTemplate" Value="{StaticResource TealTemplate}" />
</Style>
```

[`Style`](xref:Xamarin.Forms.Style) インスタンスが "*暗黙的*" であるため、それはアプリケーション内のすべての `ContentView` インスタンスに適用されます。 そのため、次のコード例に示すように、[`ContentView.ControlTemplate`](xref:Xamarin.Forms.TemplatedView.ControlTemplate) プロパティを設定する必要はなくなりました。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentView x:Name="contentView" Padding="0,20,0,0">
      ...
    </ContentView>
</ContentPage>
```

スタイルについて詳しくは、[スタイル](~/xamarin-forms/user-interface/styles/index.md)に関する記事をご覧ください。

### <a name="creating-a-controltemplate-at-page-level"></a>ページ レベルでの ControlTemplate の作成

アプリケーション レベルの [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) インスタンスの作成に加え、次のコード例に示すように、ページ レベルでもそれらを作成できます。

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

[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) をページ レベルで追加すると、[`ContentPage`](xref:Xamarin.Forms.ContentPage) に [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) が 追加され、`ResourceDictionary` 内に `ControlTemplate` インスタンスが含まれます。

## <a name="creating-a-controltemplate-in-c35"></a>C&#35; を使用した ControlTemplate の作成

[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) をアプリケーション レベルで定義するには、`ControlTemplate` を表わす `class` クラスを作成する必要があります。 次のコード例に示すように、テンプレート用に使用される[レイアウト](~/xamarin-forms/user-interface/layouts/index.md)からこのクラスを派生させる必要があります。

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

`AquaTemplate` クラスと `TealTemplate` クラスは、[`BoxView.Color`](xref:Xamarin.Forms.BoxView.Color) プロパティと [`Label.TextColor`](xref:Xamarin.Forms.Label.TextColor) プロパティで使用されている色が異なっていること以外は同一です。

次のコード例で、`TealTemplate` を [`ContentView`](xref:Xamarin.Forms.ContentView) に適用する[`ContentPage`](xref:Xamarin.Forms.ContentPage) を示します。

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

コントロール テンプレートを定義するクラスの種類を `ControlTemplate` コンストラクター内に指定することで、[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) インスタンスを作成します。

[`ContentView.Content`[`ContentPage`](xref:Xamarin.Forms.ContentPage) に表示されるコンテンツを定義する [`StackLayout`](xref:Xamarin.Forms.StackLayout) に、](xref:Xamarin.Forms.ContentView.Content) プロパティが設定されます。 `TealTemplate` に含まれている [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter) によって、このコンテンツが表示されます。 前に説明したのと同じメカニズムを使用して、実行時にテーマを `AquaTheme` に変更します。

## <a name="summary"></a>まとめ

この記事では、コントロール テンプレートを作成して使用する方法を示しました。 コントロール テンプレートは、アプリケーション レベルまたはページ レベルで定義できます。


## <a name="related-links"></a>関連リンク

- [スタイル](~/xamarin-forms/user-interface/styles/index.md)
- [単純なテーマ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simpletheme/)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentPresenter](xref:Xamarin.Forms.ContentPresenter)
- [ContentView](xref:Xamarin.Forms.ContentView)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
