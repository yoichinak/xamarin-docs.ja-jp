---
title: Xamarin.Forms の相対バインド
description: この記事では、RelativeSource マークアップ拡張機能を使用して、バインディング ソースをバインディング ターゲットの位置に対して設定することにより、相対バインドを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: CC64BB1D-8303-46B1-94B6-4EF2F20317A8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/30/2019
ms.openlocfilehash: 24879b1ffcac97acdba27c32a22e43bfb6e80459
ms.sourcegitcommit: e71474f91639bb43159b22f5d534325c3270ba93
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/22/2019
ms.locfileid: "72749781"
---
# <a name="xamarinforms-relative-bindings"></a>Xamarin.Forms の相対バインド

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)

相対バインドには、バインド ターゲットの位置を基準としてバインドソースを設定する機能があります。 これらは `RelativeSource` マークアップ拡張機能で作成され、バインド式の `Source` プロパティとして設定されます。

`RelativeSource` マークアップ拡張機能は、次のプロパティを定義する `RelativeSourceExtension` クラスによってサポートされています。

- `RelativeBindingSourceMode` 型の `Mode`。バインディング ターゲットの位置を基準とする相対的なバインド ソースの位置を記述します。
- `int` 型の `AncestorLevel`。`Mode` プロパティが `FindAncestor` の場合、検索するオプションの先祖レベル。
- `Type` 型の `AncestorType`。`Mode` プロパティが `FindAncestor` の場合、検索する先祖の種類。

> [!NOTE]
> XAML パーサーでは、`RelativeSourceExtension` クラスを `RelativeSource` に短縮できます。

`Mode` プロパティは、`RelativeBindingSourceMode` 列挙型メンバーのいずれかに設定する必要があります。

- `TemplatedParent` は、テンプレート (バインド要素が存在する) が適用される要素を示します。 詳細については、[テンプレート化された親へのバインド](#bind-to-a-templated-parent)のセクションを参照してください。
- `Self` は、バインドが設定されている要素を示し、その要素の 1 つのプロパティを同じ要素の別のプロパティにバインドできます。 詳細については、[自己へのバインド](#bind-to-self)のセクションを参照してください。
- `FindAncestor` は、バインドされた要素のビジュアル ツリーの先祖を示します。 `AncestorType` プロパティによって表される先祖コントロールにバインドするには、このモードを使用する必要があります。 詳細については、[先祖へのバインド](#bind-to-an-ancestor)のセクションを参照してください。
- `FindAncestorBindingContext` は、バインドされた要素のビジュアル ツリーの先祖の `BindingContext` を示します。 `AncestorType` プロパティによって表される先祖の `BindingContext` にバインドするには、このモードを使用する必要があります。 詳細については、[先祖へのバインド](#bind-to-an-ancestor)のセクションを参照してください。

`Mode` プロパティは、`RelativeSourceExtension` クラスのコンテンツ プロパティです。 そのため、中かっこで囲まれた XAML マークアップ式では、式の `Mode=` 部分を削除できます。

Xamarin.Forms マークアップ拡張機能の詳細については、「[XAML マークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/index.md)」を参照してください。

## <a name="bind-to-self"></a>自己にバインドする

`Self` の相対バインド モードは、要素のプロパティを同じ要素の別のプロパティにバインドするために使用されます。

```xaml
<BoxView Color="Red"
         WidthRequest="200"
         HeightRequest="{Binding Source={RelativeSource Self}, Path=WidthRequest}"
         HorizontalOptions="Center" />
```

この例では、[`BoxView`](xref:Xamarin.Forms.BoxView) で `WidthRequest` プロパティを固定サイズに設定し、`HeightRequest` プロパティを `WidthRequest` プロパティにバインドします。 したがって、両方のプロパティが等しいため、正方形が描画されます。

[![iOS および Android 上のセルフ モードの相対バインドのスクリーンショット](relative-bindings-images/self-relative-binding.png "自己相対バインド モード")](relative-bindings-images/self-relative-binding-large.png#lightbox "自己相対バインド モード")

> [!IMPORTANT]
> 要素のプロパティを同じ要素の別のプロパティにバインドする場合、プロパティは同じ型である必要があります。 または、バインドでコンバーターを指定して、値を変換することもできます。

このバインディング モードの一般的な用途は、オブジェクトの `BindingContext` をそのプロパティに設定することです。 この例を次のコードに示します。

```xaml
<ContentPage ...
             BindingContext="{Binding Source={RelativeSource Self}, Path=DefaultViewModel}">
    <StackLayout>
        <ListView ItemsSource="{Binding Employees}">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

この例では、ページの `BindingContext` はその `DefaultViewModel` プロパティに設定されています。 このプロパティは、ページの分離コードファイルで定義され、viewmodel インスタンスが用意されています。 [`ListView`](xref:Xamarin.Forms.ListView) は viewmodel の `Employees` プロパティにバインドされます。

## <a name="bind-to-an-ancestor"></a>先祖にバインドする

`FindAncestor` および `FindAncestorBindingContext` の相対バインド モードは、ビジュアル ツリー内の特定の型の親要素にバインドするために使用されます。 `FindAncestor` モードは、[`Element`](xref:Xamarin.Forms.Element) 型から派生した親要素にバインドするために使用されます。 `FindAncestorBindingContext` モードは、親要素の `BindingContext` にバインドするために使用されます。

> [!WARNING]
> `FindAncestor` および `FindAncestorBindingContext` の相対バインド モードを使用する場合は、`AncestorType` プロパティを `Type` に設定する必要があります。それ以外の場合は、`XamlParseException` がスローされます。

`Mode` プロパティが明示的に設定されていない場合、`AncestorType` プロパティを [`Element`](xref:Xamarin.Forms.Element) から派生した型に設定すると、`Mode` プロパティが暗黙的に `FindAncestor` に設定されます。 同様に、`AncestorType` プロパティを `Element` から派生していない型に設定すると、`Mode` プロパティは暗黙的に `FindAncestorBindingContext` に設定されます。

次の XAML は、`Mode` プロパティが暗黙的に `FindAncestorBindingContext` に設定される例を示しています。

```xaml
<ContentPage ...
             BindingContext="{Binding Source={RelativeSource Self}, Path=DefaultViewModel}">
    <StackLayout>
        <ListView ItemsSource="{Binding Employees}">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <StackLayout Orientation="Horizontal">
                            <Label Text="{Binding Fullname}"
                                   VerticalOptions="Center" />
                            <Button Text="Delete"
                                    Command="{Binding Source={RelativeSource AncestorType={x:Type local:PeopleViewModel}}, Path=DeleteEmployeeCommand}"
                                    CommandParameter="{Binding}"
                                    HorizontalOptions="EndAndExpand" />
                        </StackLayout>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

この例では、ページの `BindingContext` はその `DefaultViewModel` プロパティに設定されています。 このプロパティは、ページの分離コードファイルで定義され、viewmodel インスタンスが用意されています。 [`ListView`](xref:Xamarin.Forms.ListView) は viewmodel の `Employees` プロパティにバインドされます。 `ListView` の各項目の外観を定義する [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) には、[`Button`](xref:Xamarin.Forms.Button) が含まれています。 ボタンの `Command` プロパティは、親の viewmodel の `DeleteEmployeeCommand` にバインドされます。 `Button` をタップすると、従業員が削除されます。

[![iOS および Android 上の FindAncestor モードの相対バインドのスクリーンショット](relative-bindings-images/findancestor-relative-binding.png "FindAncestor 相対バインド モード")](relative-bindings-images/findancestor-relative-binding-large.png#lightbox "FindAncestor 相対バインド モード")

さらに、省略可能な `AncestorLevel` プロパティは、ビジュアル ツリーにその型の先祖が複数存在する可能性があるシナリオで先祖検索を明確にするために役立ちます。

```xaml
<Label Text="{Binding Source={RelativeSource AncestorType={x:Type Entry}, AncestorLevel=2}, Path=Text}" />
```

この例では、`Label.Text` プロパティは、バインディングのターゲット要素から開始して、上方向へのパスで遭遇する 2 番目の [`Entry`](xref:Xamarin.Forms.Entry) の`Text` プロパティにバインドします。

> [!NOTE]
> バインディング ターゲット要素に最も近い先祖を見つけるには、`AncestorLevel` プロパティを 1 に設定する必要があります。

## <a name="bind-to-a-templated-parent"></a>テンプレート化された親にバインドする

`TemplatedParent` 相対バインド モードは、コントロール テンプレート内から、テンプレートが適用されるランタイム オブジェクト インスタンス (テンプレート化された親と呼ばれます) にバインドするために使用されます。 このモードは、相対バインドがコントロール テンプレート内にある場合にのみ適用され、`TemplateBinding` の設定に似ています。

次の XAML は、`TemplatedParent` の相対バインド モードの例を示しています。

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ControlTemplate x:Key="CardViewControlTemplate">
            <Frame BindingContext="{Binding Source={RelativeSource TemplatedParent}}"
                   BackgroundColor="{Binding CardColor}"
                   BorderColor="{Binding BorderColor}"
                   ...>
                <Grid>
                    ...
                    <Label Text="{Binding CardTitle}"
                           ... />
                    <BoxView BackgroundColor="{Binding BorderColor}"
                             ... />
                    <Label Text="{Binding CardDescription}"
                           ... />
                </Grid>
            </Frame>
        </ControlTemplate>
    </ContentPage.Resources>
    <StackLayout>        
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

この例では、`ControlTemplate` のルート要素である [`Frame`](xref:Xamarin.Forms.Frame) の `BindingContext` には、テンプレートが適用されるランタイム オブジェクト インスタンスが設定されています。 そのため、`Frame` とその子によって、各 `CardView` オブジェクトのプロパティに対してバインディング式が解決されます。

[![iOS および Android 上の TemplatedParent モードの相対バインドのスクリーンショット](relative-bindings-images/templatedparent-relative-binding.png "TemplatedParent 相対バインド モード")](relative-bindings-images/templatedparent-relative-binding-large.png#lightbox "TemplatedParent 相対バインド モード")

コントロール テンプレートの詳細については、「[Xamarin.Forms のコントロール テンプレート](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [データ バインディングのデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)
- [XAML マークアップ拡張](~/xamarin-forms/xaml/markup-extensions/index.md)
- [Xamarin.Forms のコントロール テンプレート](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md)
