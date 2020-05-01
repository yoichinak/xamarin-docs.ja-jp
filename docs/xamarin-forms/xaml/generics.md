---
title: Xamarin. Forms XAML のジェネリック
description: Xamarin XAML は、ジェネリック型の制約を型引数として指定することによって、ジェネリック CLR 型を使用するためのサポートを提供します。
ms.prod: xamarin
ms.assetid: 97B73048-4F90-41AD-AB48-8EB804C4998B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/28/2020
ms.openlocfilehash: 9cda08a3bab0e25db2315c9795721e25d47d2429
ms.sourcegitcommit: 154a3e7aec775327565bb54eda1a610976af1d6f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/01/2020
ms.locfileid: "82624710"
---
# <a name="generics-in-xamarinforms-xaml"></a>Xamarin. Forms XAML のジェネリック

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-generics/)

Xamarin XAML は、ジェネリック型の制約を型引数として指定することによって、ジェネリック CLR 型を使用するためのサポートを提供します。 このサポートは`x:TypeArguments`ディレクティブによって提供されます。このディレクティブは、ジェネリックの制約型引数をジェネリック型のコンストラクターに渡します。

> [!IMPORTANT]
> Xamarin でジェネリッククラスを定義する`x:TypeArguments` XAML は、ディレクティブを使用してサポートされていません。

型引数は文字列として指定され、通常は`sys:String`や`sys:Int32`のようにプレフィックスが付けられます。 一般的な CLR ジェネリック制約の型は、既定の Xamarin. Forms 名前空間にマップされていないライブラリから取得されるため、プレフィックスを付ける必要があります。 ただし、XAML 2009 の組み込み型 ( `x:String`や`x:Int32`など) は、型引数として指定することも`x`できます。ここで、は xaml 2009 の xaml 言語名前空間です。 XAML 2009 組み込み型の詳細については、「 [xaml 2009 言語プリミティブ](/dotnet/desktop-wpf/xaml-services/types-for-primitives#xaml-2009-language-primitives)」を参照してください。

コンマ区切り記号を使用して、複数の型引数を指定できます。 また、ジェネリック制約でジェネリック型を使用する場合は、入れ子になった制約型の引数をかっこで囲む必要があります。

> [!NOTE]
> マーク`x:Type`アップ拡張機能は、ジェネリック型の CLR 型参照を提供し、C# の`typeof`演算子と同様の関数を備えています。 詳細については、「 [x:Type markup extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#type)」を参照してください。

## <a name="single-primitive-type-argument"></a>1つのプリミティブ型の引数

`x:TypeArguments`ディレクティブを使用して、1つのプリミティブ型の引数をプレフィックス付きの文字列引数として指定できます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:scg="clr-namespace:System.Collections.Generic;assembly=netstandard"
             ...>
    <CollectionView>
        <CollectionView.ItemsSource>
            <scg:List x:TypeArguments="x:String">
                <x:String>Baboon</x:String>
                <x:String>Capuchin Monkey</x:String>
                <x:String>Blue Monkey</x:String>
                <x:String>Squirrel Monkey</x:String>
                <x:String>Golden Lion Tamarin</x:String>
                <x:String>Howler Monkey</x:String>
                <x:String>Japanese Macaque</x:String>
            </scg:List>
        </CollectionView.ItemsSource>
    </CollectionView>
</ContentPage>
```

この例では`System.Collections.Generic` 、が`scg` XAML 名前空間として定義されています。 `CollectionView.ItemsSource`プロパティは、XAML 2009 組み込み`List<T>` `x:String`型を使用して`string` 、型引数を使用してインスタンス化されたに設定されます。 `List<string>`コレクションが複数`string`の項目で初期化されています。

また、同様に、CLR `List<T>` `String`型を使用してコレクションをインスタンス化することもできます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:scg="clr-namespace:System.Collections.Generic;assembly=netstandard"
             xmlns:sys="clr-namespace:System;assembly=netstandard"
             ...>
    <CollectionView>
        <CollectionView.ItemsSource>
            <scg:List x:TypeArguments="sys:String">
                <sys:String>Baboon</sys:String>
                <sys:String>Capuchin Monkey</sys:String>
                <sys:String>Blue Monkey</sys:String>
                <sys:String>Squirrel Monkey</sys:String>
                <sys:String>Golden Lion Tamarin</sys:String>
                <sys:String>Howler Monkey</sys:String>
                <sys:String>Japanese Macaque</sys:String>
            </scg:List>
        </CollectionView.ItemsSource>
    </CollectionView>
</ContentPage>
```

## <a name="single-object-type-argument"></a>単一オブジェクト型の引数

`x:TypeArguments`ディレクティブを使用して、1つのオブジェクト型引数をプレフィックス付き文字列引数として指定できます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:models="clr-namespace:GenericsDemo.Models"
             xmlns:scg="clr-namespace:System.Collections.Generic;assembly=netstandard"
             ...>
    <CollectionView>
        <CollectionView.ItemsSource>
            <scg:List x:TypeArguments="models:Monkey">
                <models:Monkey Name="Baboon"
                               Location="Africa and Asia"
                               ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg" />
                <models:Monkey Name="Capuchin Monkey"
                               Location="Central and South America"
                               ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/4/40/Capuchin_Costa_Rica.jpg/200px-Capuchin_Costa_Rica.jpg" />
                <models:Monkey Name="Blue Monkey"
                               Location="Central and East Africa"
                               ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/BlueMonkey.jpg/220px-BlueMonkey.jpg" />
            </scg:List>
        </CollectionView.ItemsSource>
        <CollectionView.ItemTemplate>
            <DataTemplate>
                <Grid Padding="10">
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="Auto" />
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto" />
                        <ColumnDefinition Width="Auto" />
                    </Grid.ColumnDefinitions>
                    <Image Grid.RowSpan="2"
                           Source="{Binding ImageUrl}"
                           Aspect="AspectFill"
                           HeightRequest="60"
                           WidthRequest="60" />
                    <Label Grid.Column="1"
                           Text="{Binding Name}"
                           FontAttributes="Bold" />
                    <Label Grid.Row="1"
                           Grid.Column="1"
                           Text="{Binding Location}"
                           FontAttributes="Italic"
                           VerticalOptions="End" />
                </Grid>
            </DataTemplate>
        </CollectionView.ItemTemplate>
    </CollectionView>
</ContentPage>
```

この例では`GenericsDemo.Models` 、が`models` xaml 名前空間として`System.Collections.Generic`定義され、 `scg`が xaml 名前空間として定義されています。 `CollectionView.ItemsSource`プロパティは、 `Monkey`型引数を使用してインスタンス化され`List<T>`たに設定されます。 `List<Monkey>` `Monkey`コレクションは複数の項目で初期化され、 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)各`Monkey`オブジェクトの外観を定義するは、 `ItemTemplate` [`CollectionView`](xref:Xamarin.Forms.CollectionView)のとして設定されます。

## <a name="multiple-type-arguments"></a>複数の型引数

`x:TypeArguments`ディレクティブを使用して、複数の型引数をプレフィックス付き文字列引数としてコンマで区切って指定できます。 ジェネリック制約でジェネリック型を使用する場合、入れ子になった制約型の引数はかっこ内に含まれます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:models="clr-namespace:GenericsDemo.Models"
             xmlns:scg="clr-namespace:System.Collections.Generic;assembly=netstandard"
             ...>
    <CollectionView>
        <CollectionView.ItemsSource>
            <scg:List x:TypeArguments="scg:KeyValuePair(x:String,models:Monkey)">
                <scg:KeyValuePair x:TypeArguments="x:String,models:Monkey">
                    <x:Arguments>
                        <x:String>Baboon</x:String>
                        <models:Monkey Location="Africa and Asia"
                                       ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg" />
                    </x:Arguments>
                </scg:KeyValuePair>
                <scg:KeyValuePair x:TypeArguments="x:String,models:Monkey">
                    <x:Arguments>
                        <x:String>Capuchin Monkey</x:String>
                        <models:Monkey Location="Central and South America"
                                       ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/4/40/Capuchin_Costa_Rica.jpg/200px-Capuchin_Costa_Rica.jpg" />   
                    </x:Arguments>
                </scg:KeyValuePair>
                <scg:KeyValuePair x:TypeArguments="x:String,models:Monkey">
                    <x:Arguments>
                        <x:String>Blue Monkey</x:String>
                        <models:Monkey Location="Central and East Africa"
                                       ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/BlueMonkey.jpg/220px-BlueMonkey.jpg" />
                    </x:Arguments>
                </scg:KeyValuePair>
            </scg:List>
        </CollectionView.ItemsSource>
        <CollectionView.ItemTemplate>
            <DataTemplate>
                <Grid Padding="10">
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="Auto" />
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto" />
                        <ColumnDefinition Width="Auto" />
                    </Grid.ColumnDefinitions>
                    <Image Grid.RowSpan="2"
                           Source="{Binding Value.ImageUrl}"
                           Aspect="AspectFill"
                           HeightRequest="60"
                           WidthRequest="60" />
                    <Label Grid.Column="1"
                           Text="{Binding Key}"
                           FontAttributes="Bold" />
                    <Label Grid.Row="1"
                           Grid.Column="1"
                           Text="{Binding Value.Location}"
                           FontAttributes="Italic"
                           VerticalOptions="End" />
                </Grid>
            </DataTemplate>
        </CollectionView.ItemTemplate>
    </CollectionView>
</ContentPage    
```

この例では`GenericsDemo.Models` 、が`models` xaml 名前空間として`System.Collections.Generic`定義され、 `scg`が xaml 名前空間として定義されています。 `CollectionView.ItemsSource`プロパティは、内部制約型`List<T>`の引数`string`と`Monkey`を使用`KeyValuePair<TKey, TValue>`して、制約でインスタンス化されたに設定されます。 `List<KeyValuePair<string,Monkey>>`コレクションは`KeyValuePair` 、既定`KeyValuePair`以外のコンストラクターを使用して複数の項目で初期化さ[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)れます。また、各`Monkey`オブジェクトの外観を定義`ItemTemplate`するは[`CollectionView`](xref:Xamarin.Forms.CollectionView)、のとして設定されます。 既定以外のコンストラクターに引数を渡す方法については、「[コンストラクター引数の引き渡し](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments)」を参照してください。

## <a name="related-links"></a>関連リンク

- [XAML のジェネリック (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-generics/)
- [XAML 2009 言語プリミティブ](/dotnet/desktop-wpf/xaml-services/types-for-primitives#xaml-2009-language-primitives)
- [x:Type のマークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/consuming.md#type)
- [コンストラクター引数の引き渡し](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments)
