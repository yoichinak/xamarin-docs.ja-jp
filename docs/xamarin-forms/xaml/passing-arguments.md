---
title: XAML で引数の受け渡し
description: この記事では、工場出荷時のメソッドを呼び出すと、ジェネリック引数の型を指定する既定以外のコンス トラクターに引数を渡すに使用できる XAML 属性の使用を示します。
ms.prod: xamarin
ms.assetid: 8F3B267F-499E-4D79-9193-FCA99F199519
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2016
ms.openlocfilehash: 1baad2e2edfb661fff9f3ef0ccf52c9922e9f351
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53051825"
---
# <a name="passing-arguments-in-xaml"></a>XAML で引数の受け渡し

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/xaml/passingconstructorarguments/)

_この記事では、工場出荷時のメソッドを呼び出すと、ジェネリック引数の型を指定する既定以外のコンス トラクターに引数を渡すに使用できる XAML 属性の使用を示します。_

## <a name="overview"></a>概要

引数を必要とするコンス トラクターまたは静的作成メソッドを呼び出してオブジェクトをインスタンス化する必要があります。 これを使用して XAML で実現できる、`x:Arguments`と`x:FactoryMethod`属性。

- `x:Arguments`属性は、既定ではないコンス トラクターまたはファクトリ メソッドのオブジェクトの宣言にコンス トラクター引数を指定するために使用します。 詳細については、次を参照してください。[コンス トラクターの引数を渡す](#constructor_arguments)します。
- `x:FactoryMethod`オブジェクトを初期化するために使用できるファクトリ メソッドを指定する属性を使用します。 詳細については、次を参照してください。[ファクトリ メソッドを呼び出す](#factory_methods)します。

さらに、`x:TypeArguments`属性を使用して、ジェネリック型のコンス トラクターにジェネリック型引数を指定することができます。 詳細については、次を参照してください。[ジェネリック型引数を指定する](#generic_type_arguments)します。

<a name="constructor_arguments" />

## <a name="passing-constructor-arguments"></a>コンス トラクター引数の受け渡し

使用して、既定以外のコンス トラクターに引数を渡すことができます、`x:Arguments`属性。 各コンス トラクターの引数は、引数の型を表す XML 要素内で区切る必要があります。 Xamarin.Forms は、基本的な型を次の要素をサポートします。

- `x:Object`
- `x:Boolean`
- `x:Byte`
- `x:Int16`
- `x:Int32`
- `x:Int64`
- `x:Single`
- `x:Double`
- `x:Decimal`
- `x:Char`
- `x:String`
- `x:TimeSpan`
- `x:Array`
- `x:DateTime`

次のコード例に示しますを使用して、 `x:Arguments` 3 つの属性[ `Color` ](xref:Xamarin.Forms.Color)コンス トラクター。

```xaml
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.9</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.25</x:Double>
        <x:Double>0.5</x:Double>
        <x:Double>0.75</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.8</x:Double>
        <x:Double>0.5</x:Double>
        <x:Double>0.2</x:Double>
        <x:Double>0.5</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
```

内の要素の数、`x:Arguments`タグ、およびこれらの要素の型はのいずれかで一致する必要があります、 [ `Color` ](xref:Xamarin.Forms.Color)コンス トラクター。 `Color` [コンス トラクター](xref:Xamarin.Forms.Color.%23ctor(System.Double)) 1 つのパラメーターを 1 (白) に 0 (黒) からグレースケール値が必要です。 `Color` [コンス トラクター](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double)) 3 つのパラメーターの 0 から 1 まで赤、緑、および青の値が必要です。 `Color` [コンス トラクター](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double,System.Double)) 4 つのパラメーターを 4 番目のパラメーターとして、アルファ チャネルを追加します。

次のスクリーン ショットは、各呼び出しの結果を表示する[ `Color` ](xref:Xamarin.Forms.Color)指定された引数の値を持つコンス トラクター。

![](passing-arguments-images/passing-arguments.png "X: 引数で指定された BoxView.Color")

<a name="factory_methods" />

## <a name="calling-factory-methods"></a>ファクトリ メソッドを呼び出す

メソッドの指定することによって XAML のファクトリ メソッドを呼び出すことが名前を使用して、`x:FactoryMethod`属性、およびその引数を使用して、`x:Arguments`属性。 ファクトリ メソッドが、`public static`オブジェクトまたはクラスまたはメソッドを定義する構造体と同じ型の値を返すメソッド。

[ `Color` ](xref:Xamarin.Forms.Color)構造体のファクトリ メソッドを定義し、次のコード例は、呼び出し元の 3 つのことを示します。

```xaml
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromRgba">
      <x:Arguments>
        <x:Int32>192</x:Int32>
        <x:Int32>75</x:Int32>
        <x:Int32>150</x:Int32>                        
        <x:Int32>128</x:Int32>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromHsla">
      <x:Arguments>
        <x:Double>0.23</x:Double>
        <x:Double>0.42</x:Double>
        <x:Double>0.69</x:Double>
        <x:Double>0.7</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromHex">
      <x:Arguments>
        <x:String>#FF048B9A</x:String>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
```

内の要素の数、`x:Arguments`タグ、およびこれらの要素の型が呼び出されている、ファクトリ メソッドの引数に一致する必要があります。 [ `FromRgba` ](xref:Xamarin.Forms.Color.FromRgba(System.Int32,System.Int32,System.Int32,System.Int32))ファクトリ メソッドでは、4 つが必要な[ `Int32` ](https://docs.microsoft.com/dotnet/api/system.int32)パラメーターで、0 から 255 までそれぞれ赤、緑、青、およびアルファ値を表します。 [ `FromHsla` ](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double))ファクトリ メソッドでは、4 つが必要な[ `Double` ](https://docs.microsoft.com/dotnet/api/system.double)色合い、鮮やかさ、明るさ、およびそれぞれに 1 0 からまでのアルファ値を表すパラメーター。 [ `FromHex` ](xref:Xamarin.Forms.Color.FromHex(System.String))ファクトリ メソッドが必要です、 [ `String` ](https://docs.microsoft.com/dotnet/api/system.string)を表す、16 進 RGB 色は (A)。

次のスクリーン ショットは、各呼び出しの結果を表示する[ `Color` ](xref:Xamarin.Forms.Color)指定された引数の値を持つファクトリ メソッド。

![](passing-arguments-images/factory-methods.png "X:factorymethod x: 引数と共に指定 BoxView.Color")

<a name="generic_type_arguments" />

## <a name="specifying-a-generic-type-argument"></a>ジェネリック型引数を指定します。

使用してジェネリック型のコンス トラクターのジェネリック型引数を指定することができます、`x:TypeArguments`属性は、次のコード例で示した。

```xaml
<ContentPage ...>
  <StackLayout>
    <StackLayout.Margin>
      <OnPlatform x:TypeArguments="Thickness">
        <On Platform="iOS" Value="0,20,0,0" />
        <On Platform="Android" Value="5, 10" />
        <On Platform="UWP" Value="10" />
      </OnPlatform>
    </StackLayout.Margin>
  </StackLayout>
</ContentPage>
```

[ `OnPlatform` ](xref:Xamarin.Forms.OnPlatform`1)クラスはジェネリック クラスでありでインスタンス化する必要があります、`x:TypeArguments`ターゲット型と一致する属性。 [ `On` ](xref:Xamarin.Forms.On)クラス、 [ `Platform` ](xref:Xamarin.Forms.On.Platform)属性は、1 つを受け入れることができる`string`値、またはコンマで区切られた複数`string`値。 この例で、 [ `StackLayout.Margin` ](xref:Xamarin.Forms.View.Margin)プロパティがプラットフォーム固有[ `Thickness`](xref:Xamarin.Forms.Thickness)します。

## <a name="summary"></a>まとめ

この記事では、工場出荷時のメソッドを呼び出すと、ジェネリック引数の型を指定する既定以外のコンス トラクターに引数を渡すに使用できる XAML 属性を使用して示されています。


## <a name="related-links"></a>関連リンク

- [XAML 名前空間](~/xamarin-forms/xaml/namespaces.md)
- [コンス トラクター引数の受け渡し (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/xaml/passingconstructorarguments/)
- [ファクトリ メソッドを呼び出す (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/xaml/callingfactorymethods/)
