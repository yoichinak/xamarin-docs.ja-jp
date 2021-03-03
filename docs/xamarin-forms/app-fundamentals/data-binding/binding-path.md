---
title: Xamarin.Forms のバインド パス
description: この記事では、Xamarin.Forms のデータ バインディングを使用して、Binding クラスの Path プロパティでサブ プロパティおよびコレクション メンバーにアクセスする方法を説明します。
ms.prod: xamarin
ms.assetid: 3CF721A5-E157-468B-AD3A-DA0A45E58E8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4fd8c91ccf18e72c4e5881261637b7f41b2f3c79
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373615"
---
# <a name="xamarinforms-binding-path"></a>Xamarin.Forms のバインド パス

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/databindingdemos)

これまでのすべてのデータ バインディング例では、`Binding` クラスの [`Path`](xref:Xamarin.Forms.Binding.Path) プロパティ (または `Binding` マークアップ拡張の [`Path`](xref:Xamarin.Forms.Xaml.BindingExtension.Path) プロパティ) が単一のプロパティに設定されていました。 実際には、`Path` を *サブ プロパティ* (プロパティのプロパティ) またはコレクションのメンバーに設定することができます。

たとえば、ページに `TimePicker` が含まれるとします。

```xaml
<TimePicker x:Name="timePicker">
```

`TimePicker` の `Time` プロパティは `TimeSpan` 型ですが、その `TimeSpan` 値の `TotalSeconds` プロパティを参照するデータ バインディングを作成することが必要になる可能性があります。 次に、そのデータ バインディングを示します。

```xaml
{Binding Source={x:Reference timePicker},
         Path=Time.TotalSeconds}
```

`Time` プロパティは `TimeSpan` 型で、`TotalSeconds` プロパティがあります。 `Time` プロパティと `TotalSeconds` プロパティは、ピリオドで連結されます。 `Path` 文字列内の項目は常にプロパティを参照し、これらのプロパティの型を参照しません。

**Path Variations** ページでは、その例といくつかのその他の例が示されます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:globe="clr-namespace:System.Globalization;assembly=netstandard"
             x:Class="DataBindingDemos.PathVariationsPage"
             Title="Path Variations"
             x:Name="page">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="HorizontalTextAlignment" Value="Center" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10, 0">
        <TimePicker x:Name="timePicker" />

        <Label Text="{Binding Source={x:Reference timePicker},
                              Path=Time.TotalSeconds,
                              StringFormat='{0} total seconds'}" />

        <Label Text="{Binding Source={x:Reference page},
                              Path=Content.Children.Count,
                              StringFormat='There are {0} children in this StackLayout'}" />

        <Label Text="{Binding Source={x:Static globe:CultureInfo.CurrentCulture},
                              Path=DateTimeFormat.DayNames[3],
                              StringFormat='The middle day of the week is {0}'}" />

        <Label>
            <Label.Text>
                <Binding Path="DateTimeFormat.DayNames[3]"
                         StringFormat="The middle day of the week in France is {0}">
                    <Binding.Source>
                        <globe:CultureInfo>
                            <x:Arguments>
                                <x:String>fr-FR</x:String>
                            </x:Arguments>
                        </globe:CultureInfo>
                    </Binding.Source>
                </Binding>
            </Label.Text>
        </Label>

        <Label Text="{Binding Source={x:Reference page},
                              Path=Content.Children[1].Text.Length,
                              StringFormat='The second Label has {0} characters'}" />
    </StackLayout>
</ContentPage>
```

2 番目の `Label` では、ページそのものがバインディング ソースです。 `Content` プロパティは `StackLayout` 型で、`IList<View>` 型の `Children` プロパティがあり、このプロパティには、子の個数を示す `Count` プロパティがあります。

## <a name="paths-with-indexers"></a>インデクサーを含むパス

**Path Variations** ページの 3 番目の `Label` 内のバインドは、`System.Globalization` 名前空間内の [`CultureInfo`](xref:System.Globalization.CultureInfo) クラスを参照します。

```xaml
<Label Text="{Binding Source={x:Static globe:CultureInfo.CurrentCulture},
                      Path=DateTimeFormat.DayNames[3],
                      StringFormat='The middle day of the week is {0}'}" />
```

ソースは、静的な `CultureInfo.CurrentCulture` プロパティに設定されます。これは、`CultureInfo` 型のオブジェクトです。 そのクラスは `DateTimeFormat` という名前の [`DateTimeFormatInfo`](xref:System.Globalization.DateTimeFormatInfo) 型で、`DayNames` コレクションが含まれます。 インデックスは 4 番目の項目を選択します。

4 番目の `Label` も同様のことを実行しますが、フランスに関連付けられたカルチャ用です。 バインドの `Source` プロパティは、コンストラクターを持つ `CultureInfo` オブジェクトに設定されます。

```xaml
<Label>
    <Label.Text>
        <Binding Path="DateTimeFormat.DayNames[3]"
                 StringFormat="The middle day of the week in France is {0}">
            <Binding.Source>
                <globe:CultureInfo>
                    <x:Arguments>
                        <x:String>fr-FR</x:String>
                    </x:Arguments>
                </globe:CultureInfo>
            </Binding.Source>
        </Binding>
    </Label.Text>
</Label>
```

XAML でコンストラクター引数を指定する方法の詳細については、「[コンストラクター引数の受け渡し](~/xamarin-forms/xaml/passing-arguments.md#passing-constructor-arguments)」を参照してください。

最後に、最後の例は 2 番目の例とよく似ています。ただし、この例では、`StackLayout` の子の 1 つを参照します。

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content.Children[1].Text.Length,
                      StringFormat='The first Label has {0} characters'}" />
```

その子は `Label` で、`String` 型の `Text` プロパティがあり、そのプロパティには `Length` プロパティがあります。 最初の `Label` は `TimePicker` 内の `TimeSpan` 設定を報告するので、テキストが変更されると、最後の `Label` も変更されます。

実行中のプログラムを次に示します。

[![パスのバリエーション](binding-path-images/pathvariations-small.png "パスのバリエーション")](binding-path-images/pathvariations-large.png#lightbox "パスのバリエーション")

## <a name="debugging-complex-paths"></a>複雑なパスのデバッグ

複雑なパスの定義は、作成が困難になる場合があります。つまり、次のサブ プロパティを正しく追加するには、各サブ プロパティの型またはコレクション内の項目の型を知る必要がありますが、型自体はパスに表示されません。 1 つの適切な手法は、パスを 1 つずつ増分して構築しながら、その間の結果を観察することです。 この最後の例では、`Path` 定義のないパスから開始します。

```xaml
<Label Text="{Binding Source={x:Reference page},
                      StringFormat='{0}'}" />
```

これは、バインド ソース型または `DataBindingDemos.PathVariationsPage` を表示します。 `PathVariationsPage`は `ContentPage` から派生することがわかっているので、これには `Content` プロパティがあります。

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content,
                      StringFormat='{0}'}" />
```

これで、`Content` プロパティの型は `Xamarin.Forms.StackLayout` であることが明らかになります。 `Children`プロパティを `Path` に追加すると、型は `Xamarin.Forms.ElementCollection'1[Xamarin.Forms.View]` です。これは、Xamarin.Forms 内部のクラスですが、明らかにコレクション型です。 これにインデックスを追加すると、型は `Xamarin.Forms.Label` です。 この方法で続行します。

Xamarin.Forms は、バインド パスを処理するので、`INotifyPropertyChanged` インターフェイスを実装するパス内のいずれかのオブジェクトに `PropertyChanged` ハンドラーをインストールします。 たとえば、`Text` プロパティが変更されるため、最後のバインドは最初の `Label` の変更に対応します。

バインド パス内のプロパティが `INotifyPropertyChanged` を実装しない場合、そのプロパティに対する変更は無視されます。 一部の変更はバインド パスを完全に無効にするため、プロパティおよびサブ プロパティの文字列が絶対に無効にならない場合にのみ、この手法を使用する必要があります。

## <a name="related-links"></a>関連リンク

- [データ バインディングのデモ (サンプル)](/samples/xamarin/xamarin-forms-samples/databindingdemos)
- [Xamarin.Forms ブックのデータ バインディングに関する章](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)