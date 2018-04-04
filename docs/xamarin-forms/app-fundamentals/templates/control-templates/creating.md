---
title: ControlTemplate の作成
description: アプリケーション レベルまたはページ レベルでは、コントロール テンプレートを定義できます。 この記事では、作成し、コントロール テンプレートを使用する方法を示します。
ms.prod: xamarin
ms.assetid: A9AEB052-FBF5-4589-9BD4-6D6F62BED7F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: e8a0695969609a4b0bbeb38896adae9a7c16ed07
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="creating-a-controltemplate"></a>ControlTemplate の作成

_アプリケーション レベルまたはページ レベルでは、コントロール テンプレートを定義できます。この記事では、作成し、コントロール テンプレートを使用する方法を示します。_

## <a name="creating-a-controltemplate-in-xaml"></a>XAML で、ControlTemplate の作成

定義する、 [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)アプリケーション レベルで、 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)に追加する必要があります、`App`クラスです。 既定では、テンプレートから作成されたすべての Xamarin.Forms アプリケーションを使用して、**アプリ**を実装するクラス、 [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/)サブクラスです。 宣言する、`ControlTemplate`アプリケーションのアプリケーション レベルで`ResourceDictionary`XAML、既定値を使用して**アプリ**クラスを XAML で置き換える必要があります**アプリ**クラスと関連付けられているコード ビハインド、として次のコード例に示します。

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

各[ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)インスタンスが再利用可能なオブジェクトとして作成、 [ `ResourceDictionary`](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)です。  これは、各宣言を一意に付けることで実現`x:Key`のわかりやすいキーで利用可能になる属性、`ResourceDictionary`です。

次のコード例を示しています、関連する`App`分離コード。

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

設定だけでなく、 [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/)プロパティ、分離コードを呼び出す必要がありますも、`InitializeComponent`を読み込みし、関連する XAML を解析します。

次のコード例は、 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)を適用する、`TealTemplate`を[ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

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

`TealTemplate`に割り当てられている、 [ `ContentView.ControlTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TemplatedView.ControlTemplate/)プロパティを使用して、`StaticResource`マークアップ拡張機能です。 [ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/)プロパティに設定されている、 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)に表示されるコンテンツを定義する、 [ `ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)です。 このコンテンツによって表示される、 [ `ContentPresenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/)に含まれている、`TealTemplate`です。 これは、結果、次のスクリーン ショットに示すように表示されます。

![](creating-images/teal-theme.png "青緑コントロール テンプレート")

### <a name="re-theming-an-application-at-runtime"></a>実行時にアプリケーションを再テーマ

クリックすると、**テーマを変更**ボタン実行、`OnButtonClicked`メソッドは、次のコード例に示されています。

```csharp
void OnButtonClicked (object sender, EventArgs e)
{
  originalTemplate = !originalTemplate;
  contentView.ControlTemplate = (originalTemplate) ? tealTemplate : aquaTemplate;
}
```

このメソッドは、アクティブに置き換えて[ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)代替手段を持つインスタンス`ControlTemplate`インスタンス、次のスクリーン ショットで結果として得られます。

![](creating-images/aqua-theme.png "水色コントロール テンプレート")

> [!NOTE]
> `ContentPage`では、`Content`プロパティを割り当てることができますと`ControlTemplate`プロパティを設定することもできます。 この場合する場合、`ControlTemplate`が含まれています、`ContentPresenter`に割り当てられるコンテンツのインスタンス、`Content`によってプロパティが表示されます、`ContentPresenter`内で、`ControlTemplate`です。

### <a name="setting-a-controltemplate-with-a-style"></a>スタイルと ControlTemplate の設定

A [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)を介して適用することも、 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)をさらにテーマの機能を展開します。 作成することでこれを行う、*暗黙的な*または*明示的な*で対象のビューのスタイル、 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)、および設定、`ControlTemplate`ターゲットのプロパティ表示、 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)インスタンス。 次のコード例は、*暗黙的な*アプリケーション レベルに追加されているスタイル[ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/):

```xaml
<Style TargetType="ContentView">
    <Setter Property="ControlTemplate" Value="{StaticResource TealTemplate}" />
</Style>
```

[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)インスタンスが*暗黙的な*、すべてに適用されます`ContentView`アプリケーション内のインスタンス。 そのため、設定する必要はなくなりました、 [ `ContentView.ControlTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TemplatedView.ControlTemplate/)プロパティ、次のコード例で示したようにします。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentView x:Name="contentView" Padding="0,20,0,0">
      ...
    </ContentView>
</ContentPage>
```

スタイルの詳細については、次を参照してください。[スタイル](~/xamarin-forms/user-interface/styles/index.md)です。

### <a name="creating-a-controltemplate-at-page-level"></a>ページ レベルで、ControlTemplate の作成

作成するだけでなく[ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)アプリケーション レベルのインスタンスをできますも作成する必要がページ レベルでは、次のコード例に示すようにします。

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

追加するときに、 [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)ページ レベルで、 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)に追加、 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)、し、`ControlTemplate`インスタンスが含まれます`ResourceDictionary`です。

## <a name="creating-a-controltemplate-in-c35"></a>C では、ControlTemplate の作成&#35;

定義する、 [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)アプリケーション レベルで、`class`を表すを作成する必要があります、`ControlTemplate`です。 クラスの派生元の[レイアウト](~/xamarin-forms/user-interface/layouts/index.md)の次のコード例に示すように、テンプレートの使用されています。

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

`AquaTemplate`クラスと同じ、`TealTemplate`クラスの異なる色が使用する点を除いて、 [ `BoxView.Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/)と[ `Label.TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/)プロパティです。

次のコード例は、 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)を適用する、`TealTemplate`を[ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

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

[ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)でコントロール テンプレートを定義するクラスの種類を指定してインスタンスを作成、`ControlTemplate`コンス トラクターです。

[ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/)プロパティに設定されている、 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)に表示されるコンテンツを定義する、 [ `ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)です。 このコンテンツによって表示される、 [ `ContentPresenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/)に含まれている、`TealTemplate`です。 記載されているのと同じメカニズムは、実行時にテーマを変更する使用は以前、`AquaTheme`です。

## <a name="summary"></a>まとめ

この記事では、作成、およびコントロール テンプレートを使用する方法を示しました。 アプリケーション レベルまたはページ レベルでは、コントロール テンプレートを定義できます。


## <a name="related-links"></a>関連リンク

- [スタイル](~/xamarin-forms/user-interface/styles/index.md)
- [単純なテーマ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simpletheme/)
- [ControlTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)
- [ContentPresenter](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/)
- [ContentView](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
