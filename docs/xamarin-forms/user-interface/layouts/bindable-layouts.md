---
title: Xamarin. Forms のバインド可能なレイアウト
description: バインド可能なレイアウトでは、レイアウトクラスが項目のコレクションにバインドすることによってコンテンツを生成できます。また、各項目の外観を System.windows.datatemplate> で設定することもできます。
ms.prod: xamarin
ms.assetid: 824C3319-20A0-42D0-8632-CDECD98349C3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/09/2020
ms.openlocfilehash: d084d910786c24a4b0333ecbc3e893cfe144d404
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517205"
---
# <a name="bindable-layouts-in-xamarinforms"></a>Xamarin. Forms のバインド可能なレイアウト

[![](~/media/shared/download.png)サンプルをダウンロードするサンプルをダウンロードする](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)

バインド可能なレイアウトでは、 [`Layout<T>`](xref:Xamarin.Forms.Layout`1)クラスから派生した任意のレイアウトクラスを使用して、項目のコレクションにバインドすることによってコンテンツを生成でき[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)ます。また、を使用して各項目の外観を設定することもできます。 バインド可能なレイアウトは、 `BindableLayout`クラスによって提供されます。このクラスは、次の添付プロパティを公開します。

- `ItemsSource`–レイアウトによって`IEnumerable`表示される項目のコレクションを指定します。
- `ItemTemplate`–レイアウトに[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)よって表示される項目のコレクション内の各項目に適用するを指定します。
- `ItemTemplateSelector`–実行時[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)に項目のを[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)選択するために使用されるを指定します。

> [!NOTE]
> プロパティ`ItemTemplate` `ItemTemplate`と`ItemTemplateSelector`プロパティの両方が設定されている場合は、プロパティが優先されます。

さらに、クラス`BindableLayout`は、次のバインド可能なプロパティを公開します。

- `EmptyView`– `string` `ItemsSource`プロパティが`null`の場合、または`ItemsSource`プロパティによって指定されたコレクションが`null`または空の場合に表示されるビューまたはビューを指定します。 既定値は `null` です。
- `EmptyViewTemplate`– [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) `ItemsSource`プロパティが`null`の場合、または`ItemsSource`プロパティによって指定されたコレクションが`null`または空の場合に表示されるを指定します。 既定値は `null` です。

> [!NOTE]
> プロパティ`EmptyViewTemplate` `EmptyView`と`EmptyViewTemplate`プロパティの両方が設定されている場合は、プロパティが優先されます。

これら[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)すべてのプロパティは、 [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) [`Grid`](xref:Xamarin.Forms.Grid) [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) [`StackLayout`](xref:Xamarin.Forms.StackLayout) [`Layout<T>`](xref:Xamarin.Forms.Layout`1)クラスから派生する、、、、およびの各クラスにアタッチできます。

クラス`Layout<T>`は、レイアウト[`Children`](xref:Xamarin.Forms.Layout`1.Children)の子要素が追加されるコレクションを公開します。 `BinableLayout.ItemsSource`プロパティが項目のコレクションに設定され、 [`Layout<T>`](xref:Xamarin.Forms.Layout`1)から派生したクラスに関連付けられている場合、コレクション内の各項目は、レイアウトによって表示されるように`Layout<T>.Children`コレクションに追加されます。 `Layout<T>`派生クラスは、基になるコレクションが変更されたときに、その子ビューを更新します。 Xamarin のレイアウトサイクルの詳細については、「[カスタムレイアウトを作成する](~/xamarin-forms/user-interface/layouts/custom.md)」を参照してください。

バインド可能なレイアウトは、表示される項目のコレクションが小さく、スクロールと選択が不要な場合にのみ使用してください。 では、バインド可能なレイアウトをラップすることに[`ScrollView`](xref:Xamarin.Forms.ScrollView)よってスクロールできますが、バインド可能なレイアウトに UI 仮想化が存在しない場合は推奨されません。 スクロールが必要な場合[`ListView`](xref:Xamarin.Forms.ListView)は、や[`CollectionView`](xref:Xamarin.Forms.CollectionView)などの UI 仮想化を含むスクロール可能なビューを使用する必要があります。 この推奨事項に従わないと、パフォーマンスの問題が発生する可能性があります。

> [!IMPORTANT]
> [`Layout<T>`](xref:Xamarin.Forms.Layout`1)クラスから派生した任意のレイアウトクラスにバインド可能なレイアウトをアタッチすることは技術的に可能ですが、特に[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)、 [`Grid`](xref:Xamarin.Forms.Grid)、および[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)の各クラスでは、これを行うことは実用的ではありません。 たとえば、バインド可能なレイアウトを[`Grid`](xref:Xamarin.Forms.Grid)使用してのデータのコレクションを表示するシナリオを考えてみます。コレクション内の各項目は、複数のプロパティを含むオブジェクトです。 `Grid`の各行には、コレクションのオブジェクトが表示されます。各列に`Grid`は、オブジェクトのプロパティの1つが表示されます。 バインド可能[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)なレイアウトのには1つのオブジェクトのみを含めることができるため、そのオブジェクトは、それぞれが特定`Grid`の列にあるオブジェクトのプロパティの1つを表示する複数のビューを含むレイアウトクラスである必要があります。 このシナリオはバインド可能なレイアウトと realised ことができますが、 `Grid`バインドされ`Grid`たコレクション内の各項目の子を含む親になります。これは、 `Grid`レイアウトの非常に非効率的で問題のある使用です。

## <a name="populate-a-bindable-layout-with-data"></a>バインド可能なレイアウトにデータを設定する

バインド可能なレイアウトには、 `ItemsSource`プロパティをが実装`IEnumerable`されている任意のコレクションに設定し、 [`Layout<T>`](xref:Xamarin.Forms.Layout`1)それを派生クラスにアタッチすることによって、データを設定します。

```xaml
<Grid BindableLayout.ItemsSource="{Binding Items}" />
```

該当の C# コードを次に示します。

```csharp
IEnumerable<string> items = ...;
var grid = new Grid();
BindableLayout.SetItemsSource(grid, items);
```

`BindableLayout.ItemsSource`添付プロパティがレイアウトに設定されていても`BindableLayout.ItemTemplate` 、添付プロパティが設定されてい`IEnumerable`ない場合、コレクション内の[`Label`](xref:Xamarin.Forms.Label)すべての項目は、 `BindableLayout`クラスによって作成されたによって表示されます。

## <a name="define-item-appearance"></a>項目の外観を定義する

バインド可能な`BindableLayout.ItemTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)レイアウトの各項目の外観は、添付プロパティをに設定することによって定義できます。

```xaml
<StackLayout BindableLayout.ItemsSource="{Binding User.TopFollowers}"
             Orientation="Horizontal"
             ...>
    <BindableLayout.ItemTemplate>
        <DataTemplate>
            <controls:CircleImage Source="{Binding}"
                                  Aspect="AspectFill"
                                  WidthRequest="44"
                                  HeightRequest="44"
                                  ... />
        </DataTemplate>
    </BindableLayout.ItemTemplate>
</StackLayout>
```

該当の C# コードを次に示します。

```csharp
DataTemplate circleImageTemplate = ...;
var stackLayout = new StackLayout();
BindableLayout.SetItemsSource(stackLayout, viewModel.User.TopFollowers);
BindableLayout.SetItemTemplate(stackLayout, circleImageTemplate);
```

この例では、 `TopFollowers`コレクション内のすべての項目が、 `CircleImage` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)で定義されているビューによって表示されます。

![System.windows.datatemplate> を使用したバインド可能なレイアウト](bindable-layouts-images/top-followers.png "データテンプレートを使用したバインド可能なレイアウト")

データ テンプレートについて詳しくは「[Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」(Xamarin.Forms のデータ テンプレート) をご覧ください。

## <a name="choose-item-appearance-at-runtime"></a>実行時に項目の外観を選択する

バインド可能な`BindableLayout.ItemTemplateSelector` [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)レイアウトの各項目の外観は、添付プロパティをに設定することによって、項目の値に基づいて実行時に選択できます。

```xaml
<FlexLayout BindableLayout.ItemsSource="{Binding User.FavoriteTech}"
            BindableLayout.ItemTemplateSelector="{StaticResource TechItemTemplateSelector}"
            ... />
```

該当の C# コードを次に示します。

```csharp
DataTemplateSelector dataTemplateSelector = new TechItemTemplateSelector { ... };
var flexLayout = new FlexLayout();
BindableLayout.SetItemsSource(flexLayout, viewModel.User.FavoriteTech);
BindableLayout.SetItemTemplateSelector(flexLayout, dataTemplateSelector);
```

サンプル[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)アプリケーションで使用されているは、次の例のようになります。

```csharp
public class TechItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate DefaultTemplate { get; set; }
    public DataTemplate XamarinFormsTemplate { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return (string)item == "Xamarin.Forms" ? XamarinFormsTemplate : DefaultTemplate;
    }
}
```

クラス`TechItemTemplateSelector`は、 `DefaultTemplate`さまざま`XamarinFormsTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)なデータテンプレートに設定されるプロパティとプロパティを定義します。 メソッド`OnSelectTemplate`はを返し`XamarinFormsTemplate`ます。これにより、項目が "Xamarin. Forms" に等しい場合は、その横にハートが付いた濃い赤の項目が表示されます。 項目が "Xamarin. Forms" と等しくない場合、メソッド`OnSelectTemplate`はを返し`DefaultTemplate` [`Label`](xref:Xamarin.Forms.Label)ます。これは、の既定の色を使用して項目を表示します。

![DataTemplateSelector を使用したバインド可能なレイアウト](bindable-layouts-images/favorite-tech.png "データテンプレートセレクターを使用したバインド可能なレイアウト")

データテンプレートセレクターの詳細については、「 [DataTemplateSelector の作成](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)」を参照してください。

## <a name="display-a-string-when-data-is-unavailable"></a>データが使用できないときに文字列を表示する

`EmptyView`プロパティは、プロパティ[`Label`](xref:Xamarin.Forms.Label)が`ItemsSource` `null`のときはによって表示される文字列に設定できます。また、 `ItemsSource`プロパティによって指定され`null`たコレクションがまたは空の場合にも表示されます。 次の XAML は、このシナリオの例を示しています。

```xaml
<StackLayout BindableLayout.ItemsSource="{Binding UserWithoutAchievements.Achievements}"
             BindableLayout.EmptyView="No achievements">
    ...
</StackLayout>
```

その結果、データバインドされたコレクションが`null`の場合、 `EmptyView`プロパティ値として設定された文字列が表示されます。

[![IOS と Android のバインド可能なレイアウト文字列の空のビューのスクリーンショット](bindable-layouts-images/emptyview-string.png "バインド可能なレイアウト文字列の空の表示")](bindable-layouts-images/emptyview-string-large.png#lightbox "バインド可能なレイアウト文字列の空の表示")

## <a name="display-views-when-data-is-unavailable"></a>データが使用できないときにビューを表示する

`EmptyView` `ItemsSource`プロパティは、プロパティが`null`の場合、またはプロパティで`ItemsSource`指定されたコレクションが`null`または空の場合に表示されるビューに設定できます。 1つのビュー、または複数の子ビューを含むビューを指定できます。 次の XAML の例は`EmptyView` 、複数の子ビューを含むビューに設定されたプロパティを示しています。

```xaml
<StackLayout BindableLayout.ItemsSource="{Binding UserWithoutAchievements.Achievements}">
    <BindableLayout.EmptyView>
        <StackLayout>
            <Label Text="None."
                   FontAttributes="Italic"
                   FontSize="{StaticResource smallTextSize}" />
            <Label Text="Try harder and return later?"
                   FontAttributes="Italic"
                   FontSize="{StaticResource smallTextSize}" />
        </StackLayout>
    </BindableLayout.EmptyView>
    ...
</StackLayout>
```

その結果、データバインドコレクションが`null`の場合、 [`StackLayout`](xref:Xamarin.Forms.StackLayout)とその子ビューが表示されます。

[![IOS と Android での複数のビューを含む、バインド可能なレイアウトの空のビューのスクリーンショット](bindable-layouts-images/emptyview-views.png "バインド可能なレイアウトの空のビュー")](bindable-layouts-images/emptyview-views-large.png#lightbox "バインド可能なレイアウトの空のビュー")

同様に、 `EmptyViewTemplate`をに[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)設定することもできます。これは、 `ItemsSource`プロパティが`null`の場合、または`ItemsSource`プロパティによって指定`null`されたコレクションがまたは空の場合に表示されます。 に`DataTemplate`は、1つのビュー、または複数の子ビューを含むビューを含めることができます。 また`BindingContext` 、のは、の`EmptyViewTemplate`から`BindingContext`継承され`BindableLayout`ます。 次の XAML の例は`EmptyViewTemplate` 、1つの`DataTemplate`ビューを含むに設定されたプロパティを示しています。

```xaml
<StackLayout BindableLayout.ItemsSource="{Binding UserWithoutAchievements.Achievements}">
    <BindableLayout.EmptyViewTemplate>
        <DataTemplate>
            <Label Text="{Binding Source={x:Reference usernameLabel}, Path=Text, StringFormat='{0} has no achievements.'}" />
        </DataTemplate>
    </BindableLayout.EmptyViewTemplate>
    ...
</StackLayout>
```

その結果、データバインドされたコレクションが`null`の場合[`Label`](xref:Xamarin.Forms.Label) 、内[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)のが表示されます。

[![IOS と Android のバインド可能なレイアウトの空のビューテンプレートのスクリーンショット](bindable-layouts-images/emptyviewtemplate.png "バインド可能なレイアウト空のビューテンプレート")](bindable-layouts-images/emptyviewtemplate-large.png#lightbox "バインド可能なレイアウト空のビューテンプレート")

> [!NOTE]
> プロパティ`EmptyViewTemplate`は、を[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)使用して設定することはできません。

## <a name="choose-an-emptyview-at-runtime"></a>実行時に EmptyView を選択する

データが使用できない`EmptyView`ときにとして表示されるビューは[`ContentView`](xref:Xamarin.Forms.ContentView) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)、内のオブジェクトとして定義できます。 この`EmptyView`プロパティは、実行時に何らか`ContentView`のビジネスロジックに基づいて、特定のに設定できます。 次の XAML は、このシナリオの例を示しています。

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        ...    
        <ContentView x:Key="BasicEmptyView">
            <StackLayout>
                <Label Text="No achievements."
                       FontSize="14" />
            </StackLayout>
        </ContentView>
        <ContentView x:Key="AdvancedEmptyView">
            <StackLayout>
                <Label Text="None."
                       FontAttributes="Italic"
                       FontSize="14" />
                <Label Text="Try harder and return later?"
                       FontAttributes="Italic"
                       FontSize="14" />
            </StackLayout>
        </ContentView>
    </ContentPage.Resources>

    <StackLayout>
        ...
        <Switch Toggled="OnEmptyViewSwitchToggled" />

        <StackLayout x:Name="stackLayout"
                     BindableLayout.ItemsSource="{Binding UserWithoutAchievements.Achievements}">
            ...
        </StackLayout>
    </StackLayout>
</ContentPage>
```

XAML は、ページ[`ContentView`](xref:Xamarin.Forms.ContentView)レベル[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)で2つのオブジェクトを定義し[`Switch`](xref:Xamarin.Forms.Switch)ます。オブジェクト`ContentView`は、 `EmptyView`プロパティ値として設定するオブジェクトを制御します。 `Switch`が切り替えられると、 `OnEmptyViewSwitchToggled`イベントハンドラーによっ`ToggleEmptyView`てメソッドが実行されます。

```csharp
void ToggleEmptyView(bool isToggled)
{
    object view = isToggled ? Resources["BasicEmptyView"] : Resources["AdvancedEmptyView"];
    BindableLayout.SetEmptyView(stackLayout, view);
}
```

メソッド`ToggleEmptyView` `EmptyView`は、 [`Switch.IsToggled`](xref:Xamarin.Forms.Switch.IsToggled)プロパティの値に`stackLayout`基づいて、オブジェクトのプロパティ[`ContentView`](xref:Xamarin.Forms.ContentView)を、に格納[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)されている2つのオブジェクトのいずれかに設定します。 次に、データバインドコレクションが`null`の場合、 `ContentView` `EmptyView`プロパティとして設定されたオブジェクトが表示されます。

[![実行時に選択した空のビューのスクリーンショット (iOS と Android)](bindable-layouts-images/emptyview-runtime.png "バインド可能なレイアウト空のビューランタイムの選択")](bindable-layouts-images/emptyview-runtime-large.png#lightbox "バインド可能なレイアウト空のビューランタイムの選択")

## <a name="related-links"></a>関連リンク

- [バインド可能なレイアウトのデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [カスタム レイアウトの作成](~/xamarin-forms/user-interface/layouts/custom.md)
- [Xamarin.Forms のデータ テンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Xamarin.Forms DataTemplateSelector の作成](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
