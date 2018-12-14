---
title: Xamarin.Forms ControlTemplate からのバインド
description: テンプレートのバインドを使うと、コントロール テンプレート内のコントロールをパブリック プロパティにデータ バインドでき、そのコントロール テンプレート内のコントロール上のプロパティ値が変更しやすくなります。 この記事では、テンプレートのバインドを使ってコントロール テンプレートからデータ バインディングを実行する方法を示します。
ms.prod: xamarin
ms.assetid: 794A663C-3A8D-438A-BD02-8E97C919B55F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 13730dce5d4698085abe10cb93da5ba50b87ab01
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106430"
---
# <a name="binding-from-a-xamarinforms-controltemplate"></a>Xamarin.Forms ControlTemplate からのバインド

_テンプレートのバインドを使うと、コントロール テンプレート内のコントロールをパブリック プロパティにデータ バインドでき、そのコントロール テンプレート内のコントロール上のプロパティ値が変更しやすくなります。この記事では、テンプレートのバインドを使ってコントロール テンプレートからデータ バインディングを実行する方法を示します。_

コントロール テンプレート内のコントロールのプロパティを、コントロール テンプレートを所有する *target* ビューの親においてバインド可能なプロパティにバインドするために、[`TemplateBinding`](xref:Xamarin.Forms.TemplateBinding) が使用されます。 たとえば、[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) 内の [`Label`](xref:Xamarin.Forms.Label) インスタンスによって表示されるテキストを定義するのではなく、テンプレートのバインドを使用して、表示されるテキストを定義するバインド可能なプロパティに [`Label.Text`](xref:Xamarin.Forms.Label.Text) プロパティをバインドすることができます。

`TemplateBinding` の *source* は、コントロール テンプレートを所有する *target* ビューの親に常に自動的に設定されるという点を除き、[`TemplateBinding`](xref:Xamarin.Forms.TemplateBinding) は既存の [`Binding`](xref:Xamarin.Forms.Binding) と似ています。 ただし、[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) 以外での `TemplateBinding` の使用はサポートされていません。

## <a name="creating-a-templatebinding-in-xaml"></a>XAML での TemplateBinding の作成

XAML では、次のコード例が示すように、[`TemplateBinding`](xref:Xamarin.Forms.Xaml.TemplateBindingExtension) マークアップ拡張を使用して [`TemplateBinding`](xref:Xamarin.Forms.TemplateBinding) が作成されます。

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

静的テキストに [`Label.Text`](xref:Xamarin.Forms.Label.Text) プロパティを設定するのではなく、テンプレートのバインドを使用して、[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) を所有する *target* ビューの親に関するバインド可能なプロパティにバインドできます。 ただし、テンプレートのバインドは、`HeaderText` と `FooterText` ではなく `Parent.HeaderText` と `Parent.FooterText` にバインドされる点に注意してください。 これは、次のコード例に示すように、この例では *target* ビューの親ではなく、親の親においてバインド可能なプロパティが定義されているためです。

```xaml
<ContentPage ...>
    <ContentView ... ControlTemplate="{StaticResource TealTemplate}">
          ...
    </ContentView>
</ContentPage>
```

テンプレートのバインドの *source* は、[`ContentView`](xref:Xamarin.Forms.ContentView) インスタンスであるコントロール テンプレートを所有する *target* ビューの親に常に自動的に設定されます。 テンプレートのバインドは、[`Parent`](xref:Xamarin.Forms.Element.Parent) プロパティを使用して、[`ContentPage`](xref:Xamarin.Forms.ContentPage) インスタンスである `ContentView` インスタンスの親要素を返します。 そのため、次のコード例に示すように、[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) の [`TemplateBinding`](xref:Xamarin.Forms.TemplateBinding) を使用して `Parent.HeaderText` にバインドし、`Parent.FooterText` は `ContentPage` に定義されているバインド可能なプロパティを特定します。

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

これで、次のスクリーンショットのような結果になります。

![](template-binding-images/teal-theme.png "テンプレートのバインドを使用する青緑色のコントロール テンプレート")

## <a name="creating-a-templatebinding-in-c35"></a>C&#35; での TemplateBinding の作成

C# では、次のコード例に示すように、`TemplateBinding` コンストラクターを使用して [`TemplateBinding`](xref:Xamarin.Forms.TemplateBinding) が作成されます。

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

静的テキストに [`Label.Text`](xref:Xamarin.Forms.Label.Text) プロパティを設定するのではなく、テンプレートのバインドを使用して、[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) を所有する *target* ビューの親においてバインド可能なプロパティにバインドできます。 テンプレートのバインドは、[`TemplateBinding`](xref:Xamarin.Forms.TemplateBinding) インスタンスを第 2 パラメーターとして指定し、[`SetBinding`](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) メソッドを使用して作成されます。 テンプレートのバインドは `Parent.HeaderText` と `Parent.FooterText` にバインドされる点に注意してください。これは、次のコード例に示すように、*target* ビューの親ではなく、親の親においてバインド可能なプロパティが定義されているためです。

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

バインド可能なプロパティは、前述のように `ContentPage` に関して定義されています。

### <a name="binding-a-bindableproperty-to-a-viewmodel-property"></a>BindableProperty を ViewModel プロパティにバインドする

前述のように、[`TemplateBinding`](xref:Xamarin.Forms.TemplateBinding) によって、コントロール テンプレート内のコントロールのプロパティは、コントロール テンプレートを所有する *target* ビューの親に関するバインド可能なプロパティにバインドされます。 次に、これらのバインド可能なプロパティを ViewModel のプロパティにバインドすることができます。

次のコード例では、ViewModel の 2 つのプロパティを定義しています。

```csharp
public class HomePageViewModel
{
  public string HeaderText { get { return "Control Template Demo App"; } }
  public string FooterText { get { return "(c) Xamarin 2016"; } }
}
```

次の XAML コード例に示すように、`HeaderText` および `FooterText` ViewModel プロパティをバインドできます。

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

[`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) を `HomePageViewModel` クラスのインスタンスに設定するため、`HeaderText` および `FooterText` のバインド可能なプロパティは `HomePageViewModel.HeaderText` および `HomePageViewModel.FooterText` プロパティにバインドされています。 全体的に見ると、[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) のコントロール プロパティが [`ContentPage`](xref:Xamarin.Forms.ContentPage) の [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) インスタンスにバインドされ、それが ViewModel プロパティにバインドされる結果になります。

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

また、view モデル プロパティに直接バインドすることもできます。これにより、コントロール テンプレートを Parent.BindingContext._PropertyName_ にバインドすることで、`ContentPage` に対する `HeaderText` と `FooterText` のために `BindableProperty` を宣言する必要がなくなります。

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

ViewModel へのデータ バインドの詳細については、[データ バインディングから MVVM ](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)に関するページを参照してください。

## <a name="summary"></a>まとめ

この記事では、テンプレートのバインドを使ってコントロール テンプレートからデータ バインディングを実行する方法について説明しました。 テンプレートのバインドを使うと、コントロール テンプレート内のコントロールをパブリック プロパティにデータ バインドでき、そのコントロール テンプレート内のコントロール上のプロパティ値が変更しやすくなります。

## <a name="related-links"></a>関連リンク

- [データ バインディングの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [データ バインディングから MVVM まで](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
- [テンプレートのバインドを含むシンプルなテーマ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebinding/)
- [テンプレートのバインドと ViewModel を含むシンプルなテーマ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebindingandviewmodel/)
- [TemplateBinding](xref:Xamarin.Forms.TemplateBinding)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentView](xref:Xamarin.Forms.ContentView)
