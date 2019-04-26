---
title: Xamarin.Forms でのバインド可能なレイアウト
description: バインド可能なレイアウトは、DataTemplate で各項目の外観を設定することも、項目のコレクションにバインドして、コンテンツを生成するレイアウト クラスを有効にします。
ms.prod: xamarin
ms.assetid: 824C3319-20A0-42D0-8632-CDECD98349C3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/18/2018
ms.openlocfilehash: b0e2d5e3c7923e5c3cf2adcc1dd104a97b78e727
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61321574"
---
# <a name="bindable-layouts-in-xamarinforms"></a>Xamarin.Forms でのバインド可能なレイアウト

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindableLayouts/)

バインド可能なレイアウトから派生した任意のレイアウト クラスを有効にする、 [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1)で各項目の外観を設定することも、項目のコレクションにバインドするには、そのコンテンツを生成するクラス、 [ `DataTemplate`](xref:Xamarin.Forms.DataTemplate). バインド可能なレイアウトがによって提供される、`BindableLayout`クラスは、次の添付プロパティを公開します。

- `ItemsSource` – のコレクションを指定`IEnumerable`レイアウトで表示する項目。
- `ItemTemplate` – を指定します、 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)レイアウトで表示されるアイテムのコレクション内の各項目に適用します。
- `ItemTemplateSelector` – を指定します、 [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector)選択に使用される、 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)のランタイムにある項目。

これらのプロパティに接続できる、 [ `AbsoluteLayout` ](xref:Xamarin.Forms.AbsoluteLayout)、 [ `FlexLayout` ](xref:Xamarin.Forms.FlexLayout)、 [ `Grid` ](xref:Xamarin.Forms.Grid)、 [ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout)、および[ `StackLayout` ](xref:Xamarin.Forms.StackLayout)から派生するクラス、 [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1)クラス。

> [!NOTE]
> `ItemTemplate`プロパティが優先と両方、`ItemTemplate`と`ItemTemplateSelector`プロパティを設定します。

`Layout<T>`クラスでは、 [ `Children` ](xref:Xamarin.Forms.Layout`1.Children)レイアウトの子要素を追加するコレクション。 ときに、`BinableLayout.ItemsSource`プロパティが項目のコレクションに設定されに接続されている、 [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1)-クラスの派生、コレクション内の各項目を追加、`Layout<T>.Children`レイアウトで表示するためのコレクション。 `Layout<T>`の基になるコレクションが変更されたときに、派生クラスはその子ビューを更新し、されます。 Xamarin.Forms のレイアウト サイクルの詳細については、次を参照してください。[カスタム レイアウトを作成する](~/xamarin-forms/user-interface/layouts/custom.md)します。

> [!IMPORTANT]
> バインド可能なレイアウトは、表示する項目のコレクションが小さいと、スクロールと選択が必要ない場合にのみ使用する必要があります。 バインド可能なレイアウトの折り返しによって提供されることのスクロール中に、 [ `ScrollView` ](xref:Xamarin.Forms.ScrollView)、これはお勧めしませんバインド可能なレイアウトが UI の仮想化がないです。 スクロールが必要な場合、UI の仮想化にはなどが含まれているスクロール可能なビュー [ `ListView` ](xref:Xamarin.Forms.ListView)または`CollectionView`、使用する必要があります。 この推奨事項を確認するエラーは、パフォーマンスの問題につながります。

## <a name="populating-a-bindable-layout-with-data"></a>データ バインド可能なレイアウトの作成

バインド可能なレイアウトに設定してデータを設定、`ItemsSource`プロパティを実装するコレクションを`IEnumerable`、アタッチして、 [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1)-派生クラス。

```xaml
<Grid BindableLayout.ItemsSource="{Binding Items}" />
```

同等の C# コードに示します。

```csharp
IEnumerable<string> items = ...;
var grid = new Grid();
BindableLayout.SetItemsSource(grid, items);
```

ときに、`BindableLayout.ItemsSource`レイアウトに、添付プロパティが設定されていますが、`BindableLayout.ItemTemplate`添付プロパティで設定されていない、すべての項目、`IEnumerable`によってコレクションが表示されます、 [ `Label` ](xref:Xamarin.Forms.Label) によって作成され`BindableLayout`クラス。

## <a name="defining-item-appearance"></a>項目の外観を定義します。

設定してバインド可能なレイアウト内の各項目の外観を定義することができます、`BindableLayout.ItemTemplate`添付プロパティを[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate):

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

同等の C# コードに示します。

```csharp
DataTemplate circleImageTemplate = ...;
var stackLayout = new StackLayout();
BindableLayout.SetItemsSource(stackLayout, viewModel.User.TopFollowers);
BindableLayout.SetItemTemplate(stackLayout, circleImageTemplate);
```

この例では、すべての項目にでは、`TopFollowers`によってコレクションが表示されます、`CircleImage`ビューで定義されている、 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate):

![DataTemplate でバインド可能なレイアウト](bindable-layouts-images/top-followers.png "データ テンプレートを使用してバインド可能なレイアウト")

データ テンプレートについて詳しくは「[Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」(Xamarin.Forms のデータ テンプレート) をご覧ください。

## <a name="choosing-item-appearance-at-runtime"></a>実行時に項目の外観を選択します。

バインド可能なレイアウト内の各項目の外観を設定して、項目の値に基づいて、実行時に選択できる、`BindableLayout.ItemTemplateSelector`添付プロパティを[ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector):

```xaml
<FlexLayout BindableLayout.ItemsSource="{Binding User.FavoriteTech}"
            BindableLayout.ItemTemplateSelector="{StaticResource TechItemTemplateSelector}"
            ... />
```

同等の C# コードに示します。

```csharp
DataTemplateSelector dataTemplateSelector = new TechItemTemplateSelector { ... };
var flexLayout = new FlexLayout();
BindableLayout.SetItemsSource(flexLayout, viewModel.User.FavoriteTech);
BindableLayout.SetItemTemplateSelector(flexLayout, dataTemplateSelector);
```

[ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector)使用されるサンプル アプリケーションが次の例で示すようにします。

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

`TechItemTemplateSelector`クラス定義`DefaultTemplate`と`XamarinFormsTemplate` [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)さまざまなデータ テンプレートに設定されているプロパティ。 `OnSelectTemplate`メソッドが返す、`XamarinFormsTemplate`項目が"Xamarin.Forms"に等しい場合に、その横にある心臓部を濃い赤で項目が表示されます。 項目は、『 Xamarin.Forms"と等しくない場合、`OnSelectTemplate`メソッドを返します、`DefaultTemplate`の既定の色を使用して項目を表示する、 [ `Label` ](xref:Xamarin.Forms.Label):

![バインド可能なレイアウト、DataTemplateSelector](bindable-layouts-images/favorite-tech.png "データ テンプレート セレクターにバインド可能なレイアウト")

データ テンプレート セレクターの詳細については、次を参照してください。[作成 Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)します。

## <a name="related-links"></a>関連リンク

- [バインド可能なレイアウトのデモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindableLayouts/)
- [カスタム レイアウトの作成](~/xamarin-forms/user-interface/layouts/custom.md)
- [Xamarin.Forms データ テンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Xamarin.Forms DataTemplateSelector を作成します。](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
