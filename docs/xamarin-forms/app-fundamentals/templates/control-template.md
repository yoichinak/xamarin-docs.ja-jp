---
title: Xamarin.Forms のコントロール テンプレート
description: Xamarin.Forms コントロール テンプレートには、ContentView 派生カスタム コントロールと ContentPage 派生ページのビジュアル構造が定義されています。
ms.prod: xamarin
ms.assetid: 8B8E2360-6531-44A3-A7C8-9A8808DE9B86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/13/2020
ms.openlocfilehash: 707e105b8535cbbb2819c5b8daeaa32bf6c5da37
ms.sourcegitcommit: 211fed94fb96127a3e158ae1ff5d7eb831a203d8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2020
ms.locfileid: "75956533"
---
# <a name="xamarinforms-control-templates"></a>Xamarin.Forms のコントロール テンプレート

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-controltemplatedemos)

Xamarin.Forms コントロール テンプレートを使うと、[`ContentView`](xref:Xamarin.Forms.ContentView) 派生カスタム コントロールと [`ContentPage`](xref:Xamarin.Forms.ContentPage) 派生ページのビジュアル構造を定義できます。 コントロール テンプレートを使うと、カスタム コントロールまたはページのユーザー インターフェイス (UI) を、コントロールまたはページを実装するロジックから分離できます。 また、事前に定義した場所にあるテンプレート化されたカスタム コントロールまたはテンプレート化されたページに追加のコンテンツを挿入することもできます。

たとえば、カスタム コントロールで提供された UI を再定義するコントロール テンプレートを作成できます。 コントロール テンプレートは、必要なカスタム コントロール インスタンスで使用できます。 または、アプリケーションの複数のページで使用される共通の UI を定義するコントロール テンプレートを作成できます。 コントロール テンプレートを複数のページに使用し、さらに各ページに固有のコンテンツを表示することができます。

## <a name="create-a-controltemplate"></a>ControlTemplate の作成

次の例は、`CardView` カスタム コントロールのコードを示しています。

```csharp
public class CardView : ContentView
{
    public static readonly BindableProperty CardTitleProperty = BindableProperty.Create(nameof(CardTitle), typeof(string), typeof(CardView), string.Empty);
    public static readonly BindableProperty CardDescriptionProperty = BindableProperty.Create(nameof(CardDescription), typeof(string), typeof(CardView), string.Empty);
    // ...

    public string CardTitle
    {
        get => (string)GetValue(CardTitleProperty);
        set => SetValue(CardTitleProperty, value);
    }

    public string CardDescription
    {
        get => (string)GetValue(CardDescriptionProperty);
        set => SetValue(CardDescriptionProperty, value);
    }
    // ...
}
```

[`ContentView`](xref:Xamarin.Forms.ContentView) クラスから派生した `CardView` クラスは、カードのようなレイアウトでデータを表示するカスタム コントロールを表します。 このクラスには、表示されるデータのプロパティが含まれており、バインド可能なプロパティに基づいています。 ただし、`CardView` クラスを使って UI を定義することはできません。 代わりに、コントロール テンプレートを使って UI を定義します。 `ContentView` 派生カスタム コントロールの作成の詳細については、「[Xamarin.Forms ContentView](~/xamarin-forms/user-interface/layouts/contentview.md)」を参照してください。

コントロール テンプレートは、[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) 型を使用して作成されます。 `ControlTemplate` を作成するときは、[`View`](xref:Xamarin.Forms.View) オブジェクトを組み合わせてカスタム コントロールまたはページの UI を構築します。 `ControlTemplate` は、ルート要素として `View` を 1 つだけ持つ必要があります。 ただし、通常、ルート要素には他の `View` オブジェクトが含まれます。 複数のオブジェクトを組み合わせて、コントロールの視覚的な構造を構成します。

[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) をインラインで定義することはできますが、リソース ディクショナリのリソースとして [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) を宣言する方法が一般的です。 コントロール テンプレートはリソースなので、すべてのリソースに適用されるものと同じスコープ規則に従います。 たとえば、アプリケーション定義 XAML ファイルのルート要素でコントロール テンプレートを宣言すると、そのテンプレートはアプリケーションのどこでも使用できます。 ページでテンプレートを定義すると、そのページでのみ、コントロール テンプレートを使用できます。 リソースの詳細については、[Xamarin.Forms のリソース ディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)に関する記事を参照してください。

次の XAML の例は、`CardView` オブジェクトの [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) を示しています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             ...>
    <ContentPage.Resources>
      <ControlTemplate x:Key="CardViewControlTemplate">
          <Frame BindingContext="{Binding Source={RelativeSource TemplatedParent}}"
                 BackgroundColor="{Binding CardColor}"
                 BorderColor="{Binding BorderColor}"
                 CornerRadius="5"
                 HasShadow="True"
                 Padding="8"
                 HorizontalOptions="Center"
                 VerticalOptions="Center">
              <Grid>
                  <Grid.RowDefinitions>
                      <RowDefinition Height="75" />
                      <RowDefinition Height="4" />
                      <RowDefinition Height="Auto" />
                  </Grid.RowDefinitions>
                  <Grid.ColumnDefinitions>
                      <ColumnDefinition Width="75" />
                      <ColumnDefinition Width="200" />
                  </Grid.ColumnDefinitions>
                  <Frame IsClippedToBounds="True"
                         BorderColor="{Binding BorderColor}"
                         BackgroundColor="{Binding IconBackgroundColor}"
                         CornerRadius="38"
                         HeightRequest="60"
                         WidthRequest="60"
                         HorizontalOptions="Center"
                         VerticalOptions="Center">
                      <Image Source="{Binding IconImageSource}"
                             Margin="-20"
                             WidthRequest="100"
                             HeightRequest="100"
                             Aspect="AspectFill" />
                  </Frame>
                  <Label Grid.Column="1"
                         Text="{Binding CardTitle}"
                         FontAttributes="Bold"
                         FontSize="Large"
                         VerticalTextAlignment="Center"
                         HorizontalTextAlignment="Start" />
                  <BoxView Grid.Row="1"
                           Grid.ColumnSpan="2"
                           BackgroundColor="{Binding BorderColor}"
                           HeightRequest="2"
                           HorizontalOptions="Fill" />
                  <Label Grid.Row="2"
                         Grid.ColumnSpan="2"
                         Text="{Binding CardDescription}"
                         VerticalTextAlignment="Start"
                         VerticalOptions="Fill"
                         HorizontalOptions="Fill" />
              </Grid>
          </Frame>
      </ControlTemplate>
    </ContentPage.Resources>
    ...
</ContentPage>
```

[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) がリソースとして宣言される場合、リソース ディクショナリで識別できるように、`x:Key` 属性を使用して指定したキーが必要です。 この例では、`CardViewControlTemplate` のルート要素は [`Frame`](xref:Xamarin.Forms.Frame) オブジェクトです。 `Frame` オブジェクトでは、`RelativeSource` マークアップ拡張機能を使用して、その `BindingContext` をテンプレートが適用されるランタイム オブジェクト インスタンスに設定します。これは、"*テンプレート化された親*" と呼ばれます。 `CardView` オブジェクトのビジュアル構造を定義するために、`Frame` オブジェクトには [`Grid`](xref:Xamarin.Forms.Grid)、`Frame`、[`Image`](xref:Xamarin.Forms.Image)、[`Label`](xref:Xamarin.Forms.Label)、および [`BoxView`](xref:Xamarin.Forms.BoxView) の組み合わせが使用されます。 このようなオブジェクトのバインド式を使うと、ルート `Frame` エレメントから `BindingContext` が継承されるため、`CardView` プロパティに対して解決されます。 `RelativeSource` マークアップ拡張機能の詳細については、「[Xamarin.Forms の相対バインド](~/xamarin-forms/app-fundamentals/data-binding/relative-bindings.md)」を参照してください。

## <a name="consume-a-controltemplate"></a>ControlTemplate を使用する

[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) を [`ContentView`](xref:Xamarin.Forms.ContentView) 派生カスタム コントロールに適用するには、その [`ControlTemplate`](xref:Xamarin.Forms.TemplatedView.ControlTemplate) プロパティをコントロール テンプレート オブジェクトに設定します。 同様に、[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) を [`ContentPage`](xref:Xamarin.Forms.ContentPage) 派生ページに適用するには、その [`ControlTemplate`](xref:Xamarin.Forms.TemplatedPage.ControlTemplate) プロパティをコントロール テンプレート オブジェクトに設定します。 実行時に [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) が適用されると、`ControlTemplate` で定義されたすべてのコントロールがテンプレート化されたカスタム コントロールまたはテンプレート化されたページのビジュアル ツリーに追加されます。

次の例は、各 `CardView` オブジェクトの [`ControlTemplate`](xref:Xamarin.Forms.TemplatedView.ControlTemplate) プロパティに割り当てられた `CardViewControlTemplate` を示します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:controls="clr-namespace:ControlTemplateDemos.Controls"
             ...>
    <StackLayout Margin="30">
        <controls:CardView BorderColor="DarkGray"
                           CardTitle="John Doe"
                           CardDescription="Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla elit dolor, convallis non interdum."
                           IconBackgroundColor="SlateGray"
                           IconImageSource="user.png"
                           ControlTemplate="{StaticResource CardViewControlTemplate}" />
        <controls:CardView BorderColor="DarkGray"
                           CardTitle="Jane Doe"
                           CardDescription="Phasellus eu convallis mi. In tempus augue eu dignissim fermentum. Morbi ut lacus vitae eros lacinia."
                           IconBackgroundColor="SlateGray"
                           IconImageSource="user.png"
                           ControlTemplate="{StaticResource CardViewControlTemplate}" />
        <controls:CardView BorderColor="DarkGray"
                           CardTitle="Xamarin Monkey"
                           CardDescription="Aliquam sagittis, odio lacinia fermentum dictum, mi erat scelerisque erat, quis aliquet arcu."
                           IconBackgroundColor="SlateGray"
                           IconImageSource="user.png"
                           ControlTemplate="{StaticResource CardViewControlTemplate}" />
    </StackLayout>
</ContentPage>
```

この例では、`CardViewControlTemplate` のコントロールは各 `CardView` オブジェクトのビジュアルツリーの一部になります。 コントロール テンプレートのルート [`Frame`](xref:Xamarin.Forms.Frame) オブジェクトによって、その `BindingContext` はテンプレート化された親に設定されるため、`Frame` とその子では、各 `CardView` オブジェクトのプロパティに対するバインディング式は解決されます。

次のスクリーンショットは、3 つの `CardView` オブジェクトに適用された `CardViewControlTemplate` を示します。

[![iOS および Android 上のテンプレート化された CardView オブジェクトのスクリーンショット](control-template-images/relativesource-controltemplate.png "テンプレート化された CardView オブジェクト")](control-template-images/relativesource-controltemplate-large.png#lightbox "テンプレート化された CardView オブジェクト")

> [!IMPORTANT]
> [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) がコントロール インスタンスに適用された時点を検出するには、テンプレート化されたカスタム コントロールまたはテンプレート化されたページで `OnApplyTemplate` メソッドをオーバーライドします。 詳細については、「[テンプレートから名前付き要素を取得する](#get-a-named-element-from-a-template)」を参照してください。

## <a name="pass-parameters-with-templatebinding"></a>TemplateBinding を使用してパラメーターを渡す

`TemplateBinding` マークアップ拡張機能を使うと、[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) 内にある要素のプロパティを、テンプレート化されたカスタム コントロールまたはテンプレート化されたページによって定義されたパブリック プロパティにバインドできます。 `TemplateBinding` を使うと、コントロールのプロパティをテンプレートのパラメーターとして機能させることができます。 そのため、テンプレート化されたカスタム コントロールまたはテンプレート化されたページのプロパティを設定すると、`TemplateBinding` が指定された要素にその値が渡されます。

> [!IMPORTANT]
> `TemplateBinding` マークアップ拡張機能は、`RelativeSource` マークアップ拡張機能を使用してテンプレート内のルート要素の `BindingContext` をテンプレート化された親に設定するという [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) の作成の代替手段です。 `TemplateBinding` マークアップ拡張機能では、`RelativeSource` のバインドは排除され、`Binding` 式は `TemplateBinding` 式に置き換えられます。

`TemplateBinding` マークアップ拡張機能を使うと、次のプロパティを定義できます。

- `Path` (`string` 型)。プロパティのパス。
- `Mode` (`BindingMode` 型)。"*ソース*" と "*ターゲット*" の間で変更が反映される方向。
- `Converter` (`IValueConverter` 型)。バインディング値コンバーター。
- `ConverterParameter` (`object` 型)。バインディング値コンバーターへのパラメーター。
- `StringFormat` (`string` 型)。バインディングの文字列形式。

`TemplateBinding` マークアップ拡張機能の `ContentProperty` は `Path` です。 そのため、マークアップ拡張機能の "Path =" 部分は、そのパスが `TemplateBinding` 式の最初の項目である場合、省略できます。 バインディング式でこれらのプロパティを使用する方法の詳細については、[Xamarin.Forms のデータ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)に関する記事を参照してください。

> [!WARNING]
> `TemplateBinding` マークアップ拡張機能は、[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) 内でのみ使用することをお勧めします。 ただし、`ControlTemplate` の外部で `TemplateBinding` 式を使用しようとしても、ビルド エラーや例外はスローされません。

次の XAML の例は、`TemplateBinding` マークアップ拡張機能を使用する `CardView` オブジェクトの [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) を示します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             ...>
    <ContentPage.Resources>
        <ControlTemplate x:Key="CardViewControlTemplate">
            <Frame BackgroundColor="{TemplateBinding CardColor}"
                   BorderColor="{TemplateBinding BorderColor}"
                   CornerRadius="5"
                   HasShadow="True"
                   Padding="8"
                   HorizontalOptions="Center"
                   VerticalOptions="Center">
                <Grid>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="75" />
                        <RowDefinition Height="4" />
                        <RowDefinition Height="Auto" />
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="75" />
                        <ColumnDefinition Width="200" />
                    </Grid.ColumnDefinitions>
                    <Frame IsClippedToBounds="True"
                           BorderColor="{TemplateBinding BorderColor}"
                           BackgroundColor="{TemplateBinding IconBackgroundColor}"
                           CornerRadius="38"
                           HeightRequest="60"
                           WidthRequest="60"
                           HorizontalOptions="Center"
                           VerticalOptions="Center">
                        <Image Source="{TemplateBinding IconImageSource}"
                               Margin="-20"
                               WidthRequest="100"
                               HeightRequest="100"
                               Aspect="AspectFill" />
                    </Frame>
                    <Label Grid.Column="1"
                           Text="{TemplateBinding CardTitle}"
                           FontAttributes="Bold"
                           FontSize="Large"
                           VerticalTextAlignment="Center"
                           HorizontalTextAlignment="Start" />
                    <BoxView Grid.Row="1"
                             Grid.ColumnSpan="2"
                             BackgroundColor="{TemplateBinding BorderColor}"
                             HeightRequest="2"
                             HorizontalOptions="Fill" />
                    <Label Grid.Row="2"
                           Grid.ColumnSpan="2"
                           Text="{TemplateBinding CardDescription}"
                           VerticalTextAlignment="Start"
                           VerticalOptions="Fill"
                           HorizontalOptions="Fill" />
                </Grid>
            </Frame>
        </ControlTemplate>
    </ContentPage.Resources>
    ...
</ContentPage>
```

この例では、`TemplateBinding` マークアップ拡張機能によって、各 `CardView` オブジェクトのプロパティに対するバインド式が解決されます。 次のスクリーンショットは、3 つの `CardView` オブジェクトに適用された `CardViewControlTemplate` を示します。

[![iOS および Android 上のテンプレート化された CardView オブジェクトのスクリーンショット](control-template-images/templatebinding-controltemplate.png "テンプレート化された CardView オブジェクト")](control-template-images/templatebinding-controltemplate-large.png#lightbox "テンプレート化された CardView オブジェクト")

> [!IMPORTANT]
> `TemplateBinding` マークアップ拡張機能を使用することは、テンプレートのルート要素の `BindingContext` を `RelativeSource` マークアップ拡張機能を使用してテンプレート化された親に設定し、`Binding` マークアップ拡張機能を使用して子オブジェクトのバインドを解決することと同等です。 実際、`TemplateBinding` マークアップ拡張機能を使用すると、`Source` が `RelativeBindingSource.TemplatedParent` である `Binding` が作成されます。

## <a name="apply-a-controltemplate-with-a-style"></a>スタイルを使用して ControlTemplate を適用する

コントロール テンプレートは、スタイルを使用して適用することもできます。 これを実行するには、[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) を使用する "*暗黙的*" または "*明示的*" なスタイルを作成します。

次の XAML の例は、`CardViewControlTemplate` を使用する "*暗黙的*" なスタイルを示します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:controls="clr-namespace:ControlTemplateDemos.Controls"
             ...>
    <ContentPage.Resources>
        <ControlTemplate x:Key="CardViewControlTemplate">
            ...
        </ControlTemplate>

        <Style TargetType="controls:CardView">
            <Setter Property="ControlTemplate"
                    Value="{StaticResource CardViewControlTemplate}" />
        </Style>
    </ContentPage.Resources>
    <StackLayout Margin="30">
        <controls:CardView BorderColor="DarkGray"
                           CardTitle="John Doe"
                           CardDescription="Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla elit dolor, convallis non interdum."
                           IconBackgroundColor="SlateGray"
                           IconImageSource="user.png" />
        <controls:CardView BorderColor="DarkGray"
                           CardTitle="Jane Doe"
                           CardDescription="Phasellus eu convallis mi. In tempus augue eu dignissim fermentum. Morbi ut lacus vitae eros lacinia."
                           IconBackgroundColor="SlateGray"
                           IconImageSource="user.png"/>
        <controls:CardView BorderColor="DarkGray"
                           CardTitle="Xamarin Monkey"
                           CardDescription="Aliquam sagittis, odio lacinia fermentum dictum, mi erat scelerisque erat, quis aliquet arcu."
                           IconBackgroundColor="SlateGray"
                           IconImageSource="user.png" />
    </StackLayout>
</ContentPage>
```

この例では、"*暗黙的な*" [`Style`](xref:Xamarin.Forms.Style) は各 `CardView` オブジェクトに自動的に適用され、各 `CardView` の [`ControlTemplate`](xref:Xamarin.Forms.TemplatedView.ControlTemplate) プロパティは `CardViewControlTemplate` に設定されます。

スタイルの詳細については、[Xamarin.Forms のスタイル](~/xamarin-forms/user-interface/styles/index.md)に関する記事を参照してください。

## <a name="redefine-a-controls-ui"></a>コントロールの UI を再定義する

[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) がインスタンス化され、[`ContentView`](xref:Xamarin.Forms.ContentView) 派生カスタム コントロールまたは [`ContentPage`](xref:Xamarin.Forms.ContentPage) 派生ページの `ControlTemplate` プロパティに割り当てられると、カスタム コントロールまたはページに定義されたビジュアル構造は、`ControlTemplate` で定義されたビジュアル構造に置き換えられます。

たとえば、`CardViewUI` カスタム コントロールでは、次の XAML を使用してユーザー インターフェイスを定義します。

```xaml
<ContentView xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ControlTemplateDemos.Controls.CardViewUI"
             x:Name="this">
    <Frame BindingContext="{x:Reference this}"
           BackgroundColor="{Binding CardColor}"
           BorderColor="{Binding BorderColor}"
           CornerRadius="5"
           HasShadow="True"
           Padding="8"
           HorizontalOptions="Center"
           VerticalOptions="Center">
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="75" />
                <RowDefinition Height="4" />
                <RowDefinition Height="Auto" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="75" />
                <ColumnDefinition Width="200" />
            </Grid.ColumnDefinitions>
            <Frame IsClippedToBounds="True"
                   BorderColor="{Binding BorderColor, FallbackValue='Black'}"
                   BackgroundColor="{Binding IconBackgroundColor, FallbackValue='Gray'}"
                   CornerRadius="38"
                   HeightRequest="60"
                   WidthRequest="60"
                   HorizontalOptions="Center"
                   VerticalOptions="Center">
                <Image Source="{Binding IconImageSource}"
                       Margin="-20"
                       WidthRequest="100"
                       HeightRequest="100"
                       Aspect="AspectFill" />
            </Frame>
            <Label Grid.Column="1"
                   Text="{Binding CardTitle, FallbackValue='Card title'}"
                   FontAttributes="Bold"
                   FontSize="Large"
                   VerticalTextAlignment="Center"
                   HorizontalTextAlignment="Start" />
            <BoxView Grid.Row="1"
                     Grid.ColumnSpan="2"
                     BackgroundColor="{Binding BorderColor, FallbackValue='Black'}"
                     HeightRequest="2"
                     HorizontalOptions="Fill" />
            <Label Grid.Row="2"
                   Grid.ColumnSpan="2"
                   Text="{Binding CardDescription, FallbackValue='Card description'}"
                   VerticalTextAlignment="Start"
                   VerticalOptions="Fill"
                   HorizontalOptions="Fill" />
        </Grid>
    </Frame>
</ContentView>
```

ただし、この UI を構成するコントロールは、[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) で新しいビジュアル構造を定義し、それを `CardViewUI` オブジェクトの [`ControlTemplate`](xref:Xamarin.Forms.TemplatedView.ControlTemplate) プロパティに割り当てることで置き換えることができます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             ...>
    <ContentPage.Resources>
        <ControlTemplate x:Key="CardViewCompressed">
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="100" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="100" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Image Source="{TemplateBinding IconImageSource}"
                        BackgroundColor="{TemplateBinding IconBackgroundColor}"
                        WidthRequest="100"
                        HeightRequest="100"
                        Aspect="AspectFill"
                        HorizontalOptions="Center"
                        VerticalOptions="Center" />
                <StackLayout Grid.Column="1">
                    <Label Text="{TemplateBinding CardTitle}"
                           FontAttributes="Bold" />
                    <Label Text="{TemplateBinding CardDescription}" />
                </StackLayout>
            </Grid>
        </ControlTemplate>
    </ContentPage.Resources>
    <StackLayout Margin="30">
        <controls:CardViewUI BorderColor="DarkGray"
                             CardTitle="John Doe"
                             CardDescription="Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla elit dolor, convallis non interdum."
                             IconBackgroundColor="SlateGray"
                             IconImageSource="user.png"
                             ControlTemplate="{StaticResource CardViewCompressed}" />
        <controls:CardViewUI BorderColor="DarkGray"
                             CardTitle="Jane Doe"
                             CardDescription="Phasellus eu convallis mi. In tempus augue eu dignissim fermentum. Morbi ut lacus vitae eros lacinia."
                             IconBackgroundColor="SlateGray"
                             IconImageSource="user.png"
                             ControlTemplate="{StaticResource CardViewCompressed}" />
        <controls:CardViewUI BorderColor="DarkGray"
                             CardTitle="Xamarin Monkey"
                             CardDescription="Aliquam sagittis, odio lacinia fermentum dictum, mi erat scelerisque erat, quis aliquet arcu."
                             IconBackgroundColor="SlateGray"
                             IconImageSource="user.png"
                             ControlTemplate="{StaticResource CardViewCompressed}" />
    </StackLayout>
</ContentPage>
```

この例では、`CardViewUI` オブジェクトのビジュアル構造が [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) で再定義され、要約リストに適した、よりコンパクトなビジュアル構造になります。

[![iOS および Android 上のテンプレート化された CardViewUI オブジェクトのスクリーンショット](control-template-images/redefine-controltemplate.png "テンプレート化された CardViewUI オブジェクト")](control-template-images/redefine-controltemplate-large.png#lightbox "テンプレート化された CardViewUI オブジェクト")

## <a name="substitute-content-into-a-contentpresenter"></a>コンテンツを ContentPresenter に置き換える

[`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter) をコントロール テンプレートに配置して、テンプレート化されたカスタム コントロールまたはテンプレート化されたページに表示されるコンテンツが表示される場所をマークすることができます。 カスタム コントロールまたはコントロール テンプレートを使用するページを使うと、`ContentPresenter` によって表示されるコンテンツを定義できます。 次の図は、青い四角形でマークされた `ContentPresenter` を含め、多数のコントロールがあるページの `ControlTemplate` を示しています。

![ContentPage のコントロール テンプレート](control-template-images/control-template.png "ContentPage のコントロール テンプレート")

次の XAML は、ビジュアル構造に [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter) を含む `TealTemplate` というコントロール テンプレートを示します。

```xaml
<ControlTemplate x:Key="TealTemplate">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="0.1*" />
            <RowDefinition Height="0.8*" />
            <RowDefinition Height="0.1*" />
        </Grid.RowDefinitions>
        <BoxView Color="Teal" />
        <Label Margin="20,0,0,0"
               Text="{TemplateBinding HeaderText}"
               TextColor="White"
               FontSize="Title"
               VerticalOptions="Center" />
        <ContentPresenter Grid.Row="1" />
        <BoxView Grid.Row="2"
                 Color="Teal" />
        <Label x:Name="changeThemeLabel"
               Grid.Row="2"
               Margin="20,0,0,0"
               Text="Change Theme"
               TextColor="White"
               HorizontalOptions="Start"
               VerticalOptions="Center">
            <Label.GestureRecognizers>
                <TapGestureRecognizer Tapped="OnChangeThemeLabelTapped" />
            </Label.GestureRecognizers>
        </Label>
        <controls:HyperlinkLabel Grid.Row="2"
                                 Margin="0,0,20,0"
                                 Text="Help"
                                 TextColor="White"
                                 Url="https://docs.microsoft.com/xamarin/xamarin-forms/"
                                 HorizontalOptions="End"
                                 VerticalOptions="Center" />
    </Grid>
</ControlTemplate>
```

次の例は、[`ContentPage`](xref:Xamarin.Forms.ContentPage) 派生ページの [`ControlTemplate`](xref:Xamarin.Forms.TemplatedPage.ControlTemplate) プロパティに割り当てられた `TealTemplate` を示します。

```xaml
<controls:HeaderFooterPage xmlns="http://xamarin.com/schemas/2014/forms"
                           xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                           xmlns:controls="clr-namespace:ControlTemplateDemos.Controls"                           
                           ControlTemplate="{StaticResource TealTemplate}"
                           HeaderText="MyApp"
                           ...>
    <StackLayout Margin="10">
        <Entry Placeholder="Enter username" />
        <Entry Placeholder="Enter password"
               IsPassword="True" />
        <Button Text="Login" />
    </StackLayout>
</controls:HeaderFooterPage>
```

実行時に `TealTemplate` がページに適用されると、ページ コンテンツがコントロール テンプレートに定義されている [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter) に置き換えられます。

[![iOS および Android 上のテンプレート ページ オブジェクトのスクリーンショット](control-template-images/tealtemplate-contentpage.png "テンプレート化された ContentPage")](control-template-images/tealtemplate-contentpage-large.png#lightbox "テンプレート化された ContentPage")

## <a name="get-a-named-element-from-a-template"></a>テンプレートから名前付き要素を取得する

コントロール テンプレート内の名前付き要素は、テンプレート化されたカスタム コントロールまたはテンプレート化されたページから取得できます。 これは `GetTemplateChild` メソッドを使って実行できます。これを使うと、インスタンス化された [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) ビジュアル ツリーの名前付き要素が返されます (見つかった場合)。 それ以外の場合は、 `null`を返します。

コントロール テンプレートがインスタンス化された後、テンプレートの `OnApplyTemplate` メソッドが呼び出されます。 そのため、`GetTemplateChild` メソッドは、テンプレート コントロールまたはテンプレート ページの `OnApplyTemplate` オーバーライドから呼び出す必要があります。

> [!IMPORTANT]
> `GetTemplateChild` メソッドは、`OnApplyTemplate` メソッドを呼び出した後にのみ呼び出す必要があります。

次の XAML は、[`ContentPage`](xref:Xamarin.Forms.ContentPage) 派生ページに適用できる `TealTemplate` というコントロール テンプレートを示します。

```xaml
<ControlTemplate x:Key="TealTemplate">
    <Grid>
        ...
        <Label x:Name="changeThemeLabel"
               Grid.Row="2"
               Margin="20,0,0,0"
               Text="Change Theme"
               TextColor="White"
               HorizontalOptions="Start"
               VerticalOptions="Center">
            <Label.GestureRecognizers>
                <TapGestureRecognizer Tapped="OnChangeThemeLabelTapped" />
            </Label.GestureRecognizers>
        </Label>
        ...
    </Grid>
</ControlTemplate>
```

この例では、[`Label`](xref:Xamarin.Forms.Label) 要素には名前が付けられており、テンプレート化されたページのコードで取得できます。 これを実行するには、テンプレート化されたページの `OnApplyTemplate` オーバーライドから `GetTemplateChild` メソッドを呼び出します。

```csharp
public partial class AccessTemplateElementPage : HeaderFooterPage
{
    Label themeLabel;

    public AccessTemplateElementPage()
    {
        InitializeComponent();
    }

    protected override void OnApplyTemplate()
    {
        base.OnApplyTemplate();
        themeLabel = (Label)GetTemplateChild("changeThemeLabel");
        themeLabel.Text = OriginalTemplate ? "Aqua Theme" : "Teal Theme";
    }
}
```

この例では、`ControlTemplate` がインスタンス化されると、`changeThemeLabel` という [`Label`](xref:Xamarin.Forms.Label) オブジェクトが取得されます。 これで、`AccessTemplateElementPage` クラスで `changeThemeLabel` にアクセスしたり操作したりできるようになります。 次のスクリーンショットは、`Label` によって表示されるテキストが変更されたことを示します。

[![iOS および Android 上のテンプレート ページ オブジェクトのスクリーンショット](control-template-images/get-named-element.png "テンプレート化された ContentPage")](control-template-images/get-named-element-large.png#lightbox "テンプレート化された ContentPage")

## <a name="bind-to-a-viewmodel"></a>viewmodel にバインドする

`ControlTemplate` がテンプレート化された親 (テンプレートが適用されるランタイム オブジェクト インスタンス) にバインドされている場合でも、[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) を使うとデータを viewmodel にバインドできます。

次の XAML の例は、`PeopleViewModel` という viewmodel を使用するページを示します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ControlTemplateDemos"
             xmlns:controls="clr-namespace:ControlTemplateDemos.Controls"
             ...>
    <ContentPage.BindingContext>
        <local:PeopleViewModel />
    </ContentPage.BindingContext>

    <ContentPage.Resources>
        <DataTemplate x:Key="PersonTemplate">
            <controls:CardView BorderColor="DarkGray"
                               CardTitle="{Binding Name}"
                               CardDescription="{Binding Description}"
                               ControlTemplate="{StaticResource CardViewControlTemplate}" />
        </DataTemplate>
    </ContentPage.Resources>

    <StackLayout Margin="10"
                 BindableLayout.ItemsSource="{Binding People}"
                 BindableLayout.ItemTemplate="{StaticResource PersonTemplate}" />
</ContentPage>
```

この例では、ページの `BindingContext` が `PeopleViewModel` インスタンスに設定されています。 この viewmodel では、`People` コレクションと、`DeletePersonCommand` という名前の `ICommand` が公開されています。 ページ上の [`StackLayout`](xref:Xamarin.Forms.StackLayout) では、バインド可能なレイアウトを使用して `People` コレクションにデータをバインドします。また、バインド可能なレイアウトの `ItemTemplate` は `PersonTemplate` リソースに設定されます。 この [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) は、`People` コレクションの各項目が `CardView` オブジェクトを使用して表示されるように指定しています。 `CardView` オブジェクトのビジュアル構造は、`CardViewControlTemplate` という [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) を使用して定義されます。

```xaml
<ControlTemplate x:Key="CardViewControlTemplate">
    <Frame BindingContext="{Binding Source={RelativeSource TemplatedParent}}"
           BackgroundColor="{Binding CardColor}"
           BorderColor="{Binding BorderColor}"
           CornerRadius="5"
           HasShadow="True"
           Padding="8"
           HorizontalOptions="Center"
           VerticalOptions="Center">
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="75" />
                <RowDefinition Height="4" />
                <RowDefinition Height="Auto" />
            </Grid.RowDefinitions>
            <Label Text="{Binding CardTitle}"
                   FontAttributes="Bold"
                   FontSize="Large"
                   VerticalTextAlignment="Center"
                   HorizontalTextAlignment="Start" />
            <BoxView Grid.Row="1"
                     BackgroundColor="{Binding BorderColor}"
                     HeightRequest="2"
                     HorizontalOptions="Fill" />
            <Label Grid.Row="2"
                   Text="{Binding CardDescription}"
                   VerticalTextAlignment="Start"
                   VerticalOptions="Fill"
                   HorizontalOptions="Fill" />
            <Button Text="Delete"
                    Command="{Binding Source={RelativeSource AncestorType={x:Type local:PeopleViewModel}}, Path=DeletePersonCommand}"
                    CommandParameter="{Binding CardTitle}"
                    HorizontalOptions="End" />
        </Grid>
    </Frame>
</ControlTemplate>
```

この例では、[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) のルート要素は [`Frame`](xref:Xamarin.Forms.Frame) オブジェクトです。 `Frame` オブジェクトでは、`RelativeSource` マークアップ拡張機能を使用して、その `BindingContext` をテンプレート化された親に設定します。 `BindingContext` はルートの `Frame` 要素から継承されるため、`Frame` オブジェクトとその子のバインディング式は、`CardView` プロパティに対して解決されます。 次のスクリーンショットは、3 つの項目で構成される `People` コレクションを表示するページを示します。

[![iOS および Android 上のテンプレート化された CardView オブジェクトのスクリーンショット](control-template-images/viewmodel-controltemplate.png "テンプレート化された CardView オブジェクト")](control-template-images/viewmodel-controltemplate-large.png#lightbox "テンプレート化された CardView オブジェクト")

[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) のオブジェクトはテンプレート化された親のプロパティにバインドされますが、コントロール テンプレート内の [`Button`](xref:Xamarin.Forms.Button) は、テンプレート化された親と、viewmodel 内の `DeletePersonCommand` の両方にバインドされます。 これは、バインディング コンテキストの種類が `PeopleViewModel` ([`StackLayout`](xref:Xamarin.Forms.StackLayout)) である祖先のバインディング コンテキストになるように、`Button.Command` プロパティによってバインディング ソースが再定義されているためです。 そのため、バインディング式の `Path` 部分によって `DeletePersonCommand` プロパティを解決できます。 ただし、`Button.CommandParameter` プロパティによってバインディング ソースが変更されることはなく、代わりに [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) の親から継承されます。 そのため、`CommandParameter` プロパティは `CardView` の `CardTitle` プロパティにバインドされます。

[`Button`](xref:Xamarin.Forms.Button) のバインディングの全体的な効果として、`Button` がタップされると、`PeopleViewModel` クラスの `DeletePersonCommand` が実行され、`CardName` プロパティの値が `DeletePersonCommand` に渡されるようになります。 その結果、指定された `CardView` がバインド可能なレイアウトから削除されます。

[![iOS および Android 上のテンプレート化された CardView オブジェクトのスクリーンショット](control-template-images/viewmodel-itemdeleted.png "テンプレート化された CardView オブジェクト")](control-template-images/viewmodel-itemdeleted-large.png#lightbox "テンプレート化された CardView オブジェクト")

相対バインドの詳細については、「[Xamarin.Forms の相対バインド](~/xamarin-forms/app-fundamentals/data-binding/relative-bindings.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [ControlTemplateDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-controltemplatedemo)
- [Xamarin.Forms ContentView](~/xamarin-forms/user-interface/layouts/contentview.md)
- [Xamarin.Forms の相対バインド](~/xamarin-forms/app-fundamentals/data-binding/relative-bindings.md)
- [Xamarin.Forms のリソース ディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Xamarin.Forms のデータ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin.Forms のスタイル](~/xamarin-forms/user-interface/styles/index.md)
