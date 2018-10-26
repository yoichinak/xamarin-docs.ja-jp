---
title: Xamarin.Forms の ControlTemplate からバインド
description: テンプレート バインディングは、簡単に変更するコントロール テンプレート内のコントロールのプロパティの値を有効にすると、データをコントロール テンプレート内のコントロールは、パブリック プロパティにバインドを許可します。 この記事では、コントロール テンプレートからのデータ バインディングを実行するテンプレート バインディングを使用してを示します。
ms.prod: xamarin
ms.assetid: 794A663C-3A8D-438A-BD02-8E97C919B55F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 13730dce5d4698085abe10cb93da5ba50b87ab01
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106430"
---
# <a name="binding-from-a-xamarinforms-controltemplate"></a>Xamarin.Forms の ControlTemplate からバインド

_テンプレート バインディングは、簡単に変更するコントロール テンプレート内のコントロールのプロパティの値を有効にすると、データをコントロール テンプレート内のコントロールは、パブリック プロパティにバインドを許可します。この記事では、コントロール テンプレートからのデータ バインディングを実行するテンプレート バインディングを使用してを示します。_

A [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding)コントロールのプロパティをコントロール テンプレート内の親でバインド可能なプロパティにバインドするために使用、*ターゲット*コントロール テンプレートを所有するビューです。 によって表示されるテキストを定義するのではなく、たとえば、 [ `Label` ](xref:Xamarin.Forms.Label)インスタンス内で、 [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate)、テンプレートのバインドを使用してバインドすることが、 [ `Label.Text`](xref:Xamarin.Forms.Label.Text)プロパティを表示するテキストを定義するバインド可能なプロパティ。

A [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding)を既存のような[ `Binding`](xref:Xamarin.Forms.Binding)ことを除いて、*ソース*の`TemplateBinding`の親に常に自動的に設定されますが、*ターゲット*コントロール テンプレートを所有するビューです。 ただしを使用して、`TemplateBinding`の外部、 [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate)はサポートされていません。

## <a name="creating-a-templatebinding-in-xaml"></a>TemplateBinding を XAML で作成します。

XAML、 [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding)を使用して作成、 [ `TemplateBinding` ](xref:Xamarin.Forms.Xaml.TemplateBindingExtension)マークアップ拡張機能で、次のコード例で示した。

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

設定ではなく、 [ `Label.Text` ](xref:Xamarin.Forms.Label.Text)静的テキストのプロパティ、プロパティ テンプレート バインディング バインドに使用できるバインド可能なプロパティの親に、*ターゲット*ビューを所有する、 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate). ただし、テンプレート バインディングにバインドする`Parent.HeaderText`と`Parent.FooterText`、なく`HeaderText`と`FooterText`します。 これは、祖父母でこの例では、バインド可能なプロパティが定義されているため、*ターゲット*を表示、親ではなく、次のコード例で示した。

```xaml
<ContentPage ...>
    <ContentView ... ControlTemplate="{StaticResource TealTemplate}">
          ...
    </ContentView>
</ContentPage>
```

*ソース*テンプレートのバインドは常に自動的の親に設定、*ターゲット*コントロール テンプレートでは、ここでは、これを所有するビュー、 [ `ContentView` ](xref:Xamarin.Forms.ContentView)インスタンス。 バインディングが使用するテンプレート、 [ `Parent` ](xref:Xamarin.Forms.Element.Parent)プロパティの親要素を返す、`ContentView`インスタンスの場合、 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)インスタンス。 したがってを使用して、 [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding)で、 [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate)にバインドする`Parent.HeaderText`と`Parent.FooterText`で定義されているバインド可能なプロパティを検索、 `ContentPage`、として次のコード例で示されています。

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

次のスクリーン ショットに示すように外観が発生します。

![](template-binding-images/teal-theme.png "青緑のコントロール テンプレートがテンプレート バインディングの使用")

## <a name="creating-a-templatebinding-in-c35"></a>C で、TemplateBinding を作成します。&#35;

C# で、 [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding)を使用して作成されて、`TemplateBinding`コンス トラクターは、次のコード例で示した。

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

設定ではなく、 [ `Label.Text` ](xref:Xamarin.Forms.Label.Text)静的テキストのプロパティ、プロパティ テンプレート バインディング バインドに使用できるバインド可能なプロパティの親に、*ターゲット*ビューを所有する、 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate). 使用して、テンプレートのバインドが作成された、 [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase))メソッドを指定する、 [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding)インスタンスが 2 番目のパラメーターとして。 テンプレート バインディングをバインド注`Parent.HeaderText`と`Parent.FooterText`の祖父母でバインド可能なプロパティが定義されているため、*ターゲット*を表示、親ではなく、次のコード例で示した。

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

バインド可能なプロパティが定義されている、 `ContentPage`、先ほど説明したようです。

### <a name="binding-a-bindableproperty-to-a-viewmodel-property"></a>ViewModel のプロパティへの BindableProperty のバインド

前述のようを[ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding)コントロールのプロパティをコントロール テンプレート内の親でバインド可能なプロパティにバインド、*ターゲット*コントロール テンプレートを所有するビューです。 さらに、これらのバインド可能なプロパティは、Viewmodel のプロパティにバインドできます。

次のコード例では、ViewModel で 2 つのプロパティを定義します。

```csharp
public class HomePageViewModel
{
  public string HeaderText { get { return "Control Template Demo App"; } }
  public string FooterText { get { return "(c) Xamarin 2016"; } }
}
```

`HeaderText`と`FooterText`ビューモデルのプロパティを次の XAML コード例に示すようにバインドできます。

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

`HeaderText`と`FooterText`バインド可能なプロパティをバインドする、`HomePageViewModel.HeaderText`と`HomePageViewModel.FooterText`設定により、プロパティ、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)のインスタンスに、`HomePageViewModel`クラス。 コントロールのプロパティで全体的に見て、この結果、 [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate)にバインドされている[ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)インスタンス、 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)、順番にバインドします。ViewModel のプロパティです。

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

バインドすることも、ビュー モデルのプロパティを直接宣言する必要があるように`BindableProperty`の`HeaderText`と`FooterText`上、 `ContentPage`、コントロール テンプレートにバインドする Parent.BindingContext によって _。PropertyName_など。

```xaml
<ControlTemplate x:Key="TealTemplate">
  <Grid>
    ...
    <Label Text="{TemplateBinding Parent.BindingContext.HeaderText}" ... />
    ...
    <Label Text="{TemplateBinding Parent.BindingContext.FooterText}" ... />
  </Grid>
</ControlTemplate>
```

ViewModels へのデータ バインディングの詳細については、次を参照してください。[データ バインディングから mvvm まで](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)します。

## <a name="summary"></a>まとめ

この記事では、テンプレート バインディングを使用して、コントロール テンプレートからのデータ バインディングを実行するかを説明します。 テンプレート バインディングは、簡単に変更するコントロール テンプレート内のコントロールのプロパティの値を有効にすると、データをコントロール テンプレート内のコントロールは、パブリック プロパティにバインドを許可します。

## <a name="related-links"></a>関連リンク

- [データ バインディングの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [MVVM へのデータ バインディングから](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
- [テンプレートのバインド (サンプル) を含む単純なテーマ](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebinding/)
- [テンプレートのバインドと ViewModel (サンプル) を含む単純なテーマ](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebindingandviewmodel/)
- [TemplateBinding](xref:Xamarin.Forms.TemplateBinding)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentView](xref:Xamarin.Forms.ContentView)
