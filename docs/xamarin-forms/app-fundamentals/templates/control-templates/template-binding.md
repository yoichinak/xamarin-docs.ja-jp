---
title: Xamarin.Forms ControlTemplate からバインド
description: テンプレート バインディングは、プロパティの値を簡単に変更するコントロール テンプレート内のコントロールを有効にすると、データにコントロール テンプレート内のコントロールのパブリック プロパティは、バインドを許可します。 テンプレート バインディングを使用して、コントロール テンプレートからのデータ バインディングを実行するかを説明します。
ms.prod: xamarin
ms.assetid: 794A663C-3A8D-438A-BD02-8E97C919B55F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 99d798ce2c74da0cf7fa0d497128db628a12ead5
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241582"
---
# <a name="binding-from-a-xamarinforms-controltemplate"></a>Xamarin.Forms ControlTemplate からバインド

_テンプレート バインディングは、プロパティの値を簡単に変更するコントロール テンプレート内のコントロールを有効にすると、データにコントロール テンプレート内のコントロールのパブリック プロパティは、バインドを許可します。テンプレート バインディングを使用して、コントロール テンプレートからのデータ バインディングを実行するかを説明します。_

A [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/)コントロール テンプレート内の親にバインド可能なプロパティに、コントロールのプロパティをバインドするために使用、*ターゲット*コントロール テンプレートを所有するビュー。 によって表示されるテキストを定義するのではなく、たとえば、 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)内インスタンス、 [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)、テンプレート バインディングを使用してバインドする可能性があります、 [ `Label.Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/)プロパティを表示するテキストを定義するバインド可能なプロパティです。

A [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/)を既存のような[ `Binding`](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/)する点を除いて、*ソース*の`TemplateBinding`の親に常に自動的に設定されている、*ターゲット*コントロール テンプレートを所有するビュー。 ただしを使用して、`TemplateBinding`の外側、 [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)はサポートされていません。

## <a name="creating-a-templatebinding-in-xaml"></a>XAML で、TemplateBinding を作成します。

XAML では、 [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/)を使用して作成されて、 [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TemplateBindingExtension/)マークアップ拡張機能で、次のコード例で示したようにします。

```xaml
<ControlTemplate x:Key="TealTemplate">
  <Grid>
    ...
    <Label Text="{TemplateBinding Parent.HeaderText}" ... />
    ...
    <Label Text="{TemplateBinding Parent.FooterText}" ... />
  </Grid>
</ControlTemplate>
```

設定ではなく、 [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/)プロパティを静的なテキストに、プロパティを使用できますテンプレート バインディングの親にバインド可能なプロパティにバインドする、*ターゲット*を所有するビュー、 [`ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/). ただし、テンプレート バインディングにバインドする`Parent.HeaderText`と`Parent.FooterText`、なく`HeaderText`と`FooterText`です。 これは、祖父のこの例では、バインド可能なプロパティが定義されているため、*ターゲット*表示、親ではなく、次のコード例に示すようにします。

```xaml
<ContentPage ...>
    <ContentView ... ControlTemplate="{StaticResource TealTemplate}">
          ...
    </ContentView>
</ContentPage>
```

*ソース*テンプレートのバインドが常に自動的の親を設定、*ターゲット*ここでは、コントロール テンプレートを所有するビュー、 [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)インスタンス。 バインディングが使用するテンプレート、 [ `Parent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Parent/)の親要素を取得するプロパティ、`ContentView`インスタンスの場合、 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)インスタンス。 したがってを使用して、 [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/)で、 [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)にバインドする`Parent.HeaderText`と`Parent.FooterText`で定義されているバインド可能なプロパティを検索し、 `ContentPage`、として次のコード例で説明します。

```csharp
public static readonly BindableProperty HeaderTextProperty =
  BindableProperty.Create ("HeaderText", typeof(string), typeof(HomePage), "Control Template Demo App");
public static readonly BindableProperty FooterTextProperty =
  BindableProperty.Create ("FooterText", typeof(string), typeof(HomePage), "(c) Xamarin 2016");

public string HeaderText {
  get { return (string)GetValue (HeaderTextProperty); }
}

public string FooterText {
  get { return (string)GetValue (FooterTextProperty); }
}
```

これは、結果、次のスクリーン ショットに示すように表示されます。

![](template-binding-images/teal-theme.png "テンプレート バインディングを使用して青緑コントロール テンプレート")

## <a name="creating-a-templatebinding-in-c35"></a>C では、TemplateBinding を作成します。&#35;

C# の場合は、 [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/)を使用して作成された、`TemplateBinding`コンス トラクターは、次のコード例で示したようにします。

```csharp
class TealTemplate : Grid
{
  public TealTemplate ()
  {
    ...
    var topLabel = new Label { TextColor = Color.White, VerticalOptions = LayoutOptions.Center };
    topLabel.SetBinding (Label.TextProperty, new TemplateBinding ("Parent.HeaderText"));
    ...
    var bottomLabel = new Label { TextColor = Color.White, VerticalOptions = LayoutOptions.Center };
    bottomLabel.SetBinding (Label.TextProperty, new TemplateBinding ("Parent.FooterText"));
    ...
  }
}
```

設定ではなく、 [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/)プロパティを静的なテキストに、プロパティを使用できますテンプレート バインディングの親にバインド可能なプロパティにバインドする、*ターゲット*を所有するビュー、 [`ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/). 使用してテンプレート バインディングを作成、 [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/)メソッドを指定する、 [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) 2 番目のパラメーターとインスタンス。 テンプレート バインディングをバインドすることに注意してください`Parent.HeaderText`と`Parent.FooterText`の祖父のバインド可能なプロパティが定義されているため、*ターゲット*表示、親ではなく、次のコード例に示すようにします。

```csharp
public class HomePageCS : ContentPage
{
  ...
  public HomePageCS ()
  {
    Content = new ContentView {
      ControlTemplate = tealTemplate,
      Content = new StackLayout {
        ...
      },
      ...
    };
    ...
  }
}
```

バインド可能なプロパティが定義されている、`ContentPage`先に触れたとおり、します。

### <a name="binding-a-bindableproperty-to-a-viewmodel-property"></a>ViewModel プロパティへの BindableProperty のバインド

前述のよう、 [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/)バインド可能なプロパティの親にコントロール テンプレートのコントロールのプロパティをバインド、*ターゲット*コントロール テンプレートを所有するビュー。 さらに、これらのバインド可能なプロパティは、ViewModels のプロパティにバインドできます。

次のコード例は、ViewModel で 2 つのプロパティを定義します。

```csharp
public class HomePageViewModel
{
  public string HeaderText { get { return "Control Template Demo App"; } }
  public string FooterText { get { return "(c) Xamarin 2016"; } }
}
```

`HeaderText`と`FooterText`ViewModel プロパティを次の XAML コードの例に示すようにバインドすることができます。

```xaml
<ContentPage xmlns:local="clr-namespace:SimpleTheme;assembly=SimpleTheme"
             HeaderText="{Binding HeaderText}" FooterText="{Binding FooterText}" ...>
    <ContentPage.BindingContext>
        <local:HomePageViewModel />
    </ContentPage.BindingContext>
    <ContentView ControlTemplate="{StaticResource TealTemplate}" ...>
    ...
    </ContentView>
</ContentPage>
```

`HeaderText`と`FooterText`バインド可能なプロパティにバインドされます、`HomePageViewModel.HeaderText`と`HomePageViewModel.FooterText`の設定のため、プロパティ、 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)のインスタンスに、`HomePageViewModel`クラスです。 コントロールのプロパティで全体的に見て、この結果、 [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)にバインドされている[ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)インスタンス、 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)、順番にバインドします。ViewModel プロパティ。

これと同じ C# コードの例は次のとおりです。

```csharp
public class HomePageCS : ContentPage
{
  ...
  public HomePageCS ()
  {
    BindingContext = new HomePageViewModel ();
    this.SetBinding (HeaderTextProperty, "HeaderText");
    this.SetBinding (FooterTextProperty, "FooterText");
    ...
  }
}
```

ViewModels へのデータ バインディングの詳細については、次を参照してください。 [MVVM へのデータのバインドから](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)です。

## <a name="summary"></a>まとめ

この記事では、テンプレート バインディングを使用して、コントロール テンプレートからのデータ バインディングを実行するかを説明します。 テンプレート バインディングは、プロパティの値を簡単に変更するコントロール テンプレート内のコントロールを有効にすると、データにコントロール テンプレート内のコントロールのパブリック プロパティは、バインドを許可します。



## <a name="related-links"></a>関連リンク

- [データのバインドの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [MVVM へのデータ バインディング](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
- [テンプレート バインディング (サンプル) を含む単純なテーマ](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebinding/)
- [テンプレート バインディングと ViewModel (サンプル) を含む単純なテーマ](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebindingandviewmodel/)
- [TemplateBinding](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/)
- [ControlTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)
- [ContentView](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)
