---
title: 「バインド Xamarin.Forms 可能なレイアウト」の説明: 「バインド可能なレイアウト」では、項目のコレクションにバインドすることによって、レイアウトクラスでコンテンツを生成できます。また、各項目の外観を system.windows.datatemplate> で設定することもできます。
ms. 製品: xamarin ms. assetid: 824C3319-20A0-42D0-8632-CDECD98349C3: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 03/09/2020 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="bindable-layouts-in-xamarinforms"></a>バインド可能なレイアウトXamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)

バインド可能なレイアウトでは、クラスから派生した任意のレイアウトクラスを使用し [`Layout<T>`](xref:Xamarin.Forms.Layout`1) て、項目のコレクションにバインドすることによってコンテンツを生成できます。また、を使用して各項目の外観を設定することも [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) できます。 バインド可能なレイアウトは、クラスによって提供され `BindableLayout` ます。このクラスは、次の添付プロパティを公開します。

- `ItemsSource`– `IEnumerable` レイアウトによって表示される項目のコレクションを指定します。
- `ItemTemplate`– [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) レイアウトによって表示される項目のコレクション内の各項目に適用するを指定します。
- `ItemTemplateSelector`– [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 実行時に項目のを選択するために使用されるを指定します。

> [!NOTE]
> `ItemTemplate`プロパティ `ItemTemplate` とプロパティの両方が設定されている場合は、プロパティが優先 `ItemTemplateSelector` されます。

さらに、クラスは、 `BindableLayout` 次のバインド可能なプロパティを公開します。

- `EmptyView`– `string` プロパティがの場合、または `ItemsSource` `null` プロパティによって指定されたコレクション `ItemsSource` が `null` または空の場合に表示されるビューまたはビューを指定します。 既定値は `null` です。
- `EmptyViewTemplate`– [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) `ItemsSource` プロパティがの場合 `null` 、またはプロパティによって指定されたコレクション `ItemsSource` が `null` または空の場合に表示されるを指定します。 既定値は `null` です。

> [!NOTE]
> `EmptyViewTemplate`プロパティ `EmptyView` とプロパティの両方が設定されている場合は、プロパティが優先 `EmptyViewTemplate` されます。

これらすべてのプロパティは [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) 、 [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) [`Grid`](xref:Xamarin.Forms.Grid) [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) [`StackLayout`](xref:Xamarin.Forms.StackLayout) クラスから派生する、、、、およびの各クラスにアタッチでき [`Layout<T>`](xref:Xamarin.Forms.Layout`1) ます。

クラスは、 `Layout<T>` [`Children`](xref:Xamarin.Forms.Layout`1.Children) レイアウトの子要素が追加されるコレクションを公開します。 `BinableLayout.ItemsSource`プロパティが項目のコレクションに設定され、から派生したクラスに関連付けられている場合 [`Layout<T>`](xref:Xamarin.Forms.Layout`1) 、コレクション内の各項目は、 `Layout<T>.Children` レイアウトによって表示されるようにコレクションに追加されます。 `Layout<T>`派生クラスは、基になるコレクションが変更されたときに、その子ビューを更新します。 レイアウトサイクルの詳細については Xamarin.Forms 、「[カスタムレイアウトの作成](~/xamarin-forms/user-interface/layouts/custom.md)」を参照してください。

バインド可能なレイアウトは、表示される項目のコレクションが小さく、スクロールと選択が不要な場合にのみ使用してください。 では、バインド可能なレイアウトをラップすることによってスクロールでき [`ScrollView`](xref:Xamarin.Forms.ScrollView) ますが、バインド可能なレイアウトに UI 仮想化が存在しない場合は推奨されません。 スクロールが必要な場合は、やなどの UI 仮想化を含むスクロール可能なビューを [`ListView`](xref:Xamarin.Forms.ListView) [`CollectionView`](xref:Xamarin.Forms.CollectionView) 使用する必要があります。 この推奨事項に従わないと、パフォーマンスの問題が発生する可能性があります。

> [!IMPORTANT]
> クラスから派生した任意のレイアウトクラスにバインド可能なレイアウトをアタッチすることは技術的に可能ですが、特に、、およびの各 [`Layout<T>`](xref:Xamarin.Forms.Layout`1) クラスでは、これを行うことは実用的ではありません [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) [`Grid`](xref:Xamarin.Forms.Grid) [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) 。 たとえば、バインド可能なレイアウトを使用してのデータのコレクションを表示するシナリオを考えてみ [`Grid`](xref:Xamarin.Forms.Grid) ます。コレクション内の各項目は、複数のプロパティを含むオブジェクトです。 の各行には、 `Grid` コレクションのオブジェクトが表示されます。各列には、 `Grid` オブジェクトのプロパティの1つが表示されます。 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)バインド可能なレイアウトのには1つのオブジェクトのみを含めることができるため、そのオブジェクトは、それぞれが特定の列にあるオブジェクトのプロパティの1つを表示する複数のビューを含むレイアウトクラスである必要があり `Grid` ます。 このシナリオはバインド可能なレイアウトと realised ことができますが、 `Grid` バインドされ `Grid` たコレクション内の各項目の子を含む親になります。これは、レイアウトの非常に非効率的で問題のある使用です `Grid` 。

## <a name="populate-a-bindable-layout-with-data"></a>バインド可能なレイアウトにデータを設定する

バインド可能なレイアウトには、プロパティをが実装されて `ItemsSource` いる任意のコレクションに設定 `IEnumerable` し、それを派生クラスにアタッチすることによって、データを設定し [`Layout<T>`](xref:Xamarin.Forms.Layout`1) ます。

```xaml
<Grid BindableLayout.ItemsSource="{Binding Items}" />
```

これに相当する C# コードを次に示します。

```csharp
IEnumerable<string> items = ...;
var grid = new Grid();
BindableLayout.SetItemsSource(grid, items);
```

`BindableLayout.ItemsSource`添付プロパティがレイアウトに設定されていても、添付プロパティが設定されて `BindableLayout.ItemTemplate` いない場合、コレクション内のすべての項目 `IEnumerable` は、 [`Label`](xref:Xamarin.Forms.Label) クラスによって作成されたによって表示され `BindableLayout` ます。

## <a name="define-item-appearance"></a>項目の外観を定義する

バインド可能なレイアウトの各項目の外観は、添付プロパティをに設定することによって定義でき `BindableLayout.ItemTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) ます。

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

これに相当する C# コードを次に示します。

```csharp
DataTemplate circleImageTemplate = ...;
var stackLayout = new StackLayout();
BindableLayout.SetItemsSource(stackLayout, viewModel.User.TopFollowers);
BindableLayout.SetItemTemplate(stackLayout, circleImageTemplate);
```

この例では、コレクション内のすべての項目 `TopFollowers` が `CircleImage` 、で定義されているビューによって表示され [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) ます。

![System.windows.datatemplate> を使用したバインド可能なレイアウト](bindable-layouts-images/top-followers.png "データテンプレートを使用したバインド可能なレイアウト")

データ テンプレートの詳細については、「[Xamarin.Forms のデータ テンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」を参照してください。

## <a name="choose-item-appearance-at-runtime"></a>実行時に項目の外観を選択する

バインド可能なレイアウトの各項目の外観は、添付プロパティをに設定することによって、項目の値に基づいて実行時に選択でき `BindableLayout.ItemTemplateSelector` [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) ます。

```xaml
<FlexLayout BindableLayout.ItemsSource="{Binding User.FavoriteTech}"
            BindableLayout.ItemTemplateSelector="{StaticResource TechItemTemplateSelector}"
            ... />
```

これに相当する C# コードを次に示します。

```csharp
DataTemplateSelector dataTemplateSelector = new TechItemTemplateSelector { ... };
var flexLayout = new FlexLayout();
BindableLayout.SetItemsSource(flexLayout, viewModel.User.FavoriteTech);
BindableLayout.SetItemTemplateSelector(flexLayout, dataTemplateSelector);
```

サンプルアプリケーションで使用されているは、次の例のようになり [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) ます。

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

`TechItemTemplateSelector`クラスは、 `DefaultTemplate` `XamarinFormsTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) さまざまなデータテンプレートに設定されるプロパティとプロパティを定義します。 メソッドはを返します。これにより、項目 `OnSelectTemplate` `XamarinFormsTemplate` が "" に等しい場合に、その横にハートが付いた濃い赤の項目が表示され Xamarin.Forms ます。 項目が "" と等しくない場合 Xamarin.Forms 、メソッドはを `OnSelectTemplate` 返し `DefaultTemplate` ます。これは、の既定の色を使用して項目を表示し [`Label`](xref:Xamarin.Forms.Label) ます。

![DataTemplateSelector を使用したバインド可能なレイアウト](bindable-layouts-images/favorite-tech.png "データテンプレートセレクターを使用したバインド可能なレイアウト")

データテンプレートセレクターの詳細については、「 [ Xamarin.Forms DataTemplateSelector の作成](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)」を参照してください。

## <a name="display-a-string-when-data-is-unavailable"></a>データが使用できないときに文字列を表示する

プロパティは、プロパティ `EmptyView` がのときはによって表示される文字列に設定でき [`Label`](xref:Xamarin.Forms.Label) `ItemsSource` `null` ます。また、プロパティによって指定されたコレクション `ItemsSource` が `null` または空の場合にも表示されます。 次の XAML は、このシナリオの例を示しています。

```xaml
<StackLayout BindableLayout.ItemsSource="{Binding UserWithoutAchievements.Achievements}"
             BindableLayout.EmptyView="No achievements">
    ...
</StackLayout>
```

その結果、データバインドされたコレクションがの場合 `null` 、プロパティ値として設定された文字列 `EmptyView` が表示されます。

[![IOS と Android のバインド可能なレイアウト文字列の空のビューのスクリーンショット](bindable-layouts-images/emptyview-string.png "バインド可能なレイアウト文字列の空の表示")](bindable-layouts-images/emptyview-string-large.png#lightbox "バインド可能なレイアウト文字列の空の表示")

## <a name="display-views-when-data-is-unavailable"></a>データが使用できないときにビューを表示する

プロパティは、プロパティ `EmptyView` がの場合 `ItemsSource` 、またはプロパティ `null` で指定されたコレクション `ItemsSource` がまたは空の場合に表示されるビューに設定でき `null` ます。 1つのビュー、または複数の子ビューを含むビューを指定できます。 次の XAML の例は、 `EmptyView` 複数の子ビューを含むビューに設定されたプロパティを示しています。

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

その結果、データバインドコレクションがの場合、 `null` [`StackLayout`](xref:Xamarin.Forms.StackLayout) とその子ビューが表示されます。

[![IOS と Android での複数のビューを含む、バインド可能なレイアウトの空のビューのスクリーンショット](bindable-layouts-images/emptyview-views.png "バインド可能なレイアウトの空のビュー")](bindable-layouts-images/emptyview-views-large.png#lightbox "バインド可能なレイアウトの空のビュー")

同様に、を `EmptyViewTemplate` に設定することもできます [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 。これは、 `ItemsSource` プロパティがの場合 `null` 、またはプロパティによって指定されたコレクション `ItemsSource` が `null` または空の場合に表示されます。 には、 `DataTemplate` 1 つのビュー、または複数の子ビューを含むビューを含めることができます。 また、のは、の `BindingContext` `EmptyViewTemplate` から継承され `BindingContext` `BindableLayout` ます。 次の XAML の例は、 `EmptyViewTemplate` 1 つのビューを含むに設定されたプロパティを示して `DataTemplate` います。

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

その結果、データバインドされたコレクションがの場合、内のが `null` [`Label`](xref:Xamarin.Forms.Label) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 表示されます。

[![IOS と Android のバインド可能なレイアウトの空のビューテンプレートのスクリーンショット](bindable-layouts-images/emptyviewtemplate.png "バインド可能なレイアウト空のビューテンプレート")](bindable-layouts-images/emptyviewtemplate-large.png#lightbox "バインド可能なレイアウト空のビューテンプレート")

> [!NOTE]
> `EmptyViewTemplate`プロパティは、を使用して設定することはできません [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) 。

## <a name="choose-an-emptyview-at-runtime"></a>実行時に EmptyView を選択する

データが使用できないときにとして表示されるビュー `EmptyView` は、 [`ContentView`](xref:Xamarin.Forms.ContentView) 内のオブジェクトとして定義でき [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ます。 この `EmptyView` プロパティは、 `ContentView` 実行時に何らかのビジネスロジックに基づいて、特定のに設定できます。 次の XAML は、このシナリオの例を示しています。

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

XAML は、ページレベルで2つのオブジェクトを定義し [`ContentView`](xref:Xamarin.Forms.ContentView) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) [`Switch`](xref:Xamarin.Forms.Switch) ます。オブジェクトは、 `ContentView` プロパティ値として設定するオブジェクトを制御し `EmptyView` ます。 `Switch`が切り替えられると、 `OnEmptyViewSwitchToggled` イベントハンドラーによってメソッドが実行され `ToggleEmptyView` ます。

```csharp
void ToggleEmptyView(bool isToggled)
{
    object view = isToggled ? Resources["BasicEmptyView"] : Resources["AdvancedEmptyView"];
    BindableLayout.SetEmptyView(stackLayout, view);
}
```

メソッドは、 `ToggleEmptyView` `EmptyView` `stackLayout` プロパティの値に基づいて、オブジェクトのプロパティを [`ContentView`](xref:Xamarin.Forms.ContentView) 、に格納されている2つのオブジェクトのいずれかに設定し [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) [`Switch.IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) ます。 次に、データバインドコレクションがの場合、 `null` `ContentView` プロパティとして設定されたオブジェクト `EmptyView` が表示されます。

[![実行時に選択した空のビューのスクリーンショット (iOS と Android)](bindable-layouts-images/emptyview-runtime.png "バインド可能なレイアウト空のビューランタイムの選択")](bindable-layouts-images/emptyview-runtime-large.png#lightbox "バインド可能なレイアウト空のビューランタイムの選択")

## <a name="related-links"></a>関連リンク

- [バインド可能なレイアウトのデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [カスタム レイアウトの作成](~/xamarin-forms/user-interface/layouts/custom.md)
- [Xamarin.Formsデータテンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [DataTemplateSelector の作成 Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
