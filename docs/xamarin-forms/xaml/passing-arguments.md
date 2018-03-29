---
title: XAML で引数の受け渡し
description: この記事では、工場出荷時のメソッドを呼び出すと、汎用引数の型を指定する既定以外のコンス トラクターに引数を渡すために使用できる XAML 属性の使用方法を示します。
ms.topic: article
ms.prod: xamarin
ms.assetid: 8F3B267F-499E-4D79-9193-FCA99F199519
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2016
ms.openlocfilehash: 232f60bb7afca7acf73e63bd7e11e1b6ec47fbd2
ms.sourcegitcommit: 17a9cf246a4d33cfa232016992b308df540c8e4f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/29/2018
---
# <a name="passing-arguments-in-xaml"></a>XAML で引数の受け渡し

_この記事では、工場出荷時のメソッドを呼び出すと、汎用引数の型を指定する既定以外のコンス トラクターに引数を渡すために使用できる XAML 属性の使用方法を示します。_

## <a name="overview"></a>概要

引数を必要とするコンス トラクターまたは静的作成メソッドを呼び出してオブジェクトをインスタンス化する必要があります。 これを行う xaml を使用して、`x:Arguments`と`x:FactoryMethod`属性。

- `x:Arguments`属性は、既定以外のコンス トラクターまたはファクトリ メソッド オブジェクト宣言のコンス トラクター引数を指定するために使用します。 詳細については、次を参照してください。[コンス トラクターの引数を渡す](#constructor_arguments)です。
- `x:FactoryMethod`属性を使用してオブジェクトを初期化するために使用できるファクトリ メソッドを指定します。 詳細については、次を参照してください。[ファクトリ メソッドを呼び出して](#factory_methods)です。

さらに、`x:TypeArguments`ジェネリック型のコンス トラクターにジェネリック型引数を指定する属性を使用できます。 詳細については、次を参照してください。[ジェネリック型引数を指定する](#generic_type_arguments)です。

<a name="constructor_arguments" />

## <a name="passing-constructor-arguments"></a>コンス トラクター引数の受け渡し

既定以外のコンス トラクターを使用して、引数を渡すことができます、`x:Arguments`属性。 各コンス トラクターの引数は、引数の型を表す XML 要素内で区切る必要があります。 Xamarin.Forms では、基本的な型に対して、次の要素をサポートしています。

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

次のコード例では、使用方法を示します、`x:Arguments`を 3 つの属性[ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/)コンス トラクター。

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

内の要素の数、`x:Arguments`タグ、およびこれらの要素の型はのいずれかで一致する必要があります、 [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/)コンス トラクターです。 `Color` [コンス トラクター](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/)単一のパラメーターが 1 (白) に 0 (黒) からグレースケール値が必要です。 `Color` [コンス トラクター](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/) 3 つのパラメーターの 0 から 1 まで赤、緑、および青の値が必要です。 `Color` [コンス トラクター](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/System.Double/) 4 つのパラメーターを使用して、4 番目のパラメーターとしてアルファ チャネルを追加します。

次のスクリーン ショットそれぞれの呼び出しの結果を表示する[ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/)指定した引数の値を持つコンス トラクター。

![](passing-arguments-images/passing-arguments.png "X: 引数で指定された BoxView.Color")

<a name="factory_methods" />

## <a name="calling-factory-methods"></a>ファクトリ メソッドの呼び出し

メソッドの指定することによって XAML のファクトリ メソッドを呼び出すことが名前を使用して、`x:FactoryMethod`属性、およびその引数を使用して、`x:Arguments`属性。 工場出荷時のメソッドは、`public static`オブジェクトまたはクラスまたはメソッドを定義する構造体と同じ型の値を返すメソッド。

[ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/)構造は、さまざまなファクトリ メソッドを定義し、次のコード例は、それらの呼び出し元の 3 つを示します。

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

内の要素の数、`x:Arguments`タグ、およびこれらの要素の型が呼び出されるファクトリ メソッドの引数に一致する必要があります。 [ `FromRgba` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgba/p/System.Int32/System.Int32/System.Int32/System.Int32/)工場出荷時のメソッドでは、4 つが必要な[ `Int32` ](https://docs.microsoft.com/dotnet/api/system.int32)パラメーターで、0 から 255 までそれぞれ、赤、緑、青、およびアルファ値を表します。 [ `FromHsla` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHsla/p/System.Double/System.Double/System.Double/System.Double/)工場出荷時のメソッドでは、4 つが必要な[ `Double` ](https://docs.microsoft.com/dotnet/api/system.double)色合い、鮮やかさ、明るさ、および、0 ~ 1 それぞれのアルファ値を表すパラメーター。 [ `FromHex` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHex/p/System.String/)ファクトリ メソッドが必要な[ `String` ](https://docs.microsoft.com/dotnet/api/system.string)を表す 16 進数値 (A) RGB 色。

次のスクリーン ショットそれぞれの呼び出しの結果を表示する[ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/)指定した引数の値を持つファクトリ メソッド。

![](passing-arguments-images/factory-methods.png "X:factorymethod および x: 引数で指定された BoxView.Color")

<a name="generic_type_arguments" />

## <a name="specifying-a-generic-type-argument"></a>ジェネリック型引数を指定します。

使用してジェネリック型のコンス トラクターのジェネリック型引数を指定することができます、`x:TypeArguments`属性が次のコード例で示したようにします。

```xaml
<ContentPage ...>
  <StackLayout>
    <StackLayout.Margin>
      <OnPlatform x:TypeArguments="Thickness">
        <On Platform="iOS" Value="0,20,0,0" />
        <On Platform="Android" Value="5, 10" />
        <On Platform="WinPhone, Windows" Value="10" />
      </OnPlatform>
    </StackLayout.Margin>
  </StackLayout>
</ContentPage>
```

[ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/)クラスはジェネリック クラスであり、インスタンス化する必要があります、`x:TypeArguments`対象の型と一致する属性。 [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/)クラス、 [ `Platform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Platform/)属性は、1 つを受け入れることができます`string`値、またはコンマで区切られた複数`string`値。 この例では、 [ `StackLayout.Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/)プラットフォーム固有にプロパティが設定されている[ `Thickness`](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/)です。

## <a name="summary"></a>まとめ

この記事では、工場出荷時のメソッドを呼び出すと、汎用引数の型を指定する既定以外のコンス トラクターに引数を渡すために使用できる XAML 属性の使用を示します。


## <a name="related-links"></a>関連リンク

- [XAML 名前空間](~/xamarin-forms/xaml/namespaces.md)
- [コンス トラクター引数を渡す方法 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/xaml/passingconstructorarguments/)
- [ファクトリ メソッドの呼び出し (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/xaml/callingfactorymethods/)
