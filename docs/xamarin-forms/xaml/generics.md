---
title: XAML のジェネリック Xamarin.Forms
description: Xamarin.Forms XAML は、ジェネリック型の制約を型引数として指定することによって、ジェネリック CLR 型を使用するためのサポートを提供します。
ms.prod: xamarin
ms.assetid: 97B73048-4F90-41AD-AB48-8EB804C4998B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/28/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 05d0917ac748ca27350f43ae225f1cb1d1aad84f
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93375383"
---
# <a name="generics-in-xamarinforms-xaml"></a>XAML のジェネリック Xamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/xaml-generics/)

Xamarin.Forms XAML は、ジェネリック型の制約を型引数として指定することによって、ジェネリック CLR 型を使用するためのサポートを提供します。 このサポートはディレクティブによって提供され `x:TypeArguments` ます。このディレクティブは、ジェネリックの制約型引数をジェネリック型のコンストラクターに渡します。

> [!IMPORTANT]
> ディレクティブを使用した XAML でのジェネリッククラスの定義 Xamarin.Forms `x:TypeArguments` はサポートされていません。

型引数は文字列として指定され、通常はやのようにプレフィックスが付けられ `sys:String` `sys:Int32` ます。 既定の型の名前空間にマップされていないライブラリから CLR ジェネリック制約の一般的な型が取得されるため、プレフィックスを付ける必要が Xamarin.Forms あります。 ただし、XAML 2009 の組み込み型 (やなど) は、 `x:String` `x:Int32` 型引数として指定することもできます。ここで、 `x` は XAML 2009 の xaml 言語名前空間です。 XAML 2009 組み込み型の詳細については、「 [xaml 2009 言語プリミティブ](/dotnet/desktop-wpf/xaml-services/types-for-primitives#xaml-2009-language-primitives)」を参照してください。

コンマ区切り記号を使用して、複数の型引数を指定できます。 また、ジェネリック制約でジェネリック型を使用する場合は、入れ子になった制約型の引数をかっこで囲む必要があります。

> [!NOTE]
> `x:Type`マークアップ拡張機能は、ジェネリック型の CLR 型参照を提供し、C# の演算子と同様の関数を備えてい `typeof` ます。 詳細については、「 [x:Type markup extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#xtype-markup-extension)」を参照してください。

## <a name="single-primitive-type-argument"></a>1つのプリミティブ型の引数

ディレクティブを使用して、1つのプリミティブ型の引数をプレフィックス付きの文字列引数として指定でき `x:TypeArguments` ます。

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

この例で `System.Collections.Generic` は、が XAML 名前空間として定義されて `scg` います。 `CollectionView.ItemsSource`プロパティは、 `List<T>` `string` XAML 2009 組み込み型を使用して、型引数を使用してインスタンス化されたに設定され `x:String` ます。 `List<string>`コレクションが複数の項目で初期化されて `string` います。

また、同様に、 `List<T>` CLR 型を使用してコレクションをインスタンス化することもでき `String` ます。

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

ディレクティブを使用して、1つのオブジェクト型引数をプレフィックス付き文字列引数として指定でき `x:TypeArguments` ます。

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

この例では、が xaml 名前空間として定義され、 `GenericsDemo.Models` `models` `System.Collections.Generic` が xaml 名前空間として定義されてい `scg` ます。 `CollectionView.ItemsSource`プロパティは、 `List<T>` 型引数を使用してインスタンス化されたに設定され `Monkey` ます。 `List<Monkey>`コレクションは複数の項目で初期化され、 `Monkey` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 各オブジェクトの外観を定義するは `Monkey` 、のとして設定され `ItemTemplate` [`CollectionView`](xref:Xamarin.Forms.CollectionView) ます。

## <a name="multiple-type-arguments"></a>複数の型引数

ディレクティブを使用して、複数の型引数をプレフィックス付き文字列引数としてコンマで区切って指定でき `x:TypeArguments` ます。 ジェネリック制約でジェネリック型を使用する場合、入れ子になった制約型の引数はかっこ内に含まれます。

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

この例では、が xaml 名前空間として定義され、 `GenericsDemo.Models` `models` `System.Collections.Generic` が xaml 名前空間として定義されてい `scg` ます。 `CollectionView.ItemsSource`プロパティは、 `List<T>` 内部制約型の引数とを使用して、制約でインスタンス化されたに設定され `KeyValuePair<TKey, TValue>` `string` `Monkey` ます。 コレクションは、 `List<KeyValuePair<string,Monkey>>` 既定以外のコンストラクターを使用して複数の項目で初期化され `KeyValuePair` `KeyValuePair` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) ます。また、各オブジェクトの外観を定義するは `Monkey` 、のとして設定され `ItemTemplate` [`CollectionView`](xref:Xamarin.Forms.CollectionView) ます。 既定以外のコンストラクターに引数を渡す方法については、「 [コンストラクター引数の引き渡し](~/xamarin-forms/xaml/passing-arguments.md#passing-constructor-arguments)」を参照してください。

## <a name="related-links"></a>関連リンク

- [XAML のジェネリック (サンプル)](/samples/xamarin/xamarin-forms-samples/xaml-generics/)
- [XAML 2009 言語プリミティブ](/dotnet/desktop-wpf/xaml-services/types-for-primitives#xaml-2009-language-primitives)
- [x:Type マークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/consuming.md#xtype-markup-extension)
- [コンストラクター引数の引き渡し](~/xamarin-forms/xaml/passing-arguments.md#passing-constructor-arguments)