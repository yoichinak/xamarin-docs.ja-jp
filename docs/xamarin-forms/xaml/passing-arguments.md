---
title: XAML で引数の受け渡し
description: この記事では、工場出荷時のメソッドを呼び出すと、ジェネリック引数の型を指定する既定以外のコンス トラクターに引数を渡すに使用できる XAML 属性の使用を示します。
ms.prod: xamarin
ms.assetid: 8F3B267F-499E-4D79-9193-FCA99F199519
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2016
ms.openlocfilehash: 80f332e45d6c46ad49543923e85cbb2eceadb378
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306561"
---
# <a name="passing-arguments-in-xaml"></a>XAML で引数の受け渡し

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-passingconstructorarguments)

_この記事では、既定以外のコンストラクターに引数を渡すために使用できる XAML 属性の使用方法、ファクトリメソッドを呼び出す方法、およびジェネリック引数の型を指定する方法について説明します。_

## <a name="overview"></a>概要

引数を必要とするコンス トラクターまたは静的作成メソッドを呼び出してオブジェクトをインスタンス化する必要があります。 これは、`x:Arguments` 属性と `x:FactoryMethod` 属性を使用して XAML で実現できます。

- `x:Arguments` 属性は、既定以外のコンストラクターまたはファクトリメソッドオブジェクトの宣言にコンストラクター引数を指定するために使用されます。 詳細については、「[コンストラクター引数の引き渡し](#constructor_arguments)」を参照してください。
- `x:FactoryMethod` 属性は、オブジェクトの初期化に使用できるファクトリメソッドを指定するために使用されます。 詳細については、「[ファクトリメソッドの呼び出し](#factory_methods)」を参照してください。

また、`x:TypeArguments` 属性を使用して、ジェネリック型のコンストラクターに対するジェネリック型引数を指定することもできます。 詳細については、「[ジェネリック型引数の指定](#generic_type_arguments)」を参照してください。

<a name="constructor_arguments" />

## <a name="passing-constructor-arguments"></a>コンス トラクター引数の受け渡し

引数は、`x:Arguments` 属性を使用して、既定以外のコンストラクターに渡すことができます。 各コンス トラクターの引数は、引数の型を表す XML 要素内で区切る必要があります。 Xamarin.Forms は、基本的な型を次の要素をサポートします。

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

次のコード例では、3つの[`Color`](xref:Xamarin.Forms.Color)コンストラクターで `x:Arguments` 属性を使用する方法を示します。

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

`x:Arguments` タグ内の要素の数、およびこれらの要素の型は、 [`Color`](xref:Xamarin.Forms.Color)コンストラクターのいずれかと一致する必要があります。 1つのパラメーターを持つ `Color`[コンストラクター](xref:Xamarin.Forms.Color.%23ctor(System.Double))には、0 (黒) から 1 (白) までのグレースケール値を指定する必要があります。 3つのパラメーターを持つ `Color`[コンストラクター](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double))には、0 ~ 1 の範囲の赤、緑、および青の値が必要です。 4つのパラメーターを持つ `Color`[コンストラクター](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double,System.Double))は、4番目のパラメーターとしてアルファチャネルを追加します。

次のスクリーンショットは、指定された引数値を使用して各[`Color`](xref:Xamarin.Forms.Color)コンストラクターを呼び出した結果を示しています。

![BoxView。 x:Arguments で指定した色](passing-arguments-images/passing-arguments.png)

<a name="factory_methods" />

## <a name="calling-factory-methods"></a>ファクトリ メソッドを呼び出す

ファクトリメソッドは、`x:FactoryMethod` 属性を使用してメソッドの名前を指定し、その引数を `x:Arguments` 属性を使用して指定することで、XAML で呼び出すことができます。 ファクトリメソッドは、メソッドを定義するクラスまたは構造体と同じ型のオブジェクトまたは値を返す `public static` メソッドです。

[`Color`](xref:Xamarin.Forms.Color)構造体は、さまざまなファクトリメソッドを定義します。次のコード例では、そのうち3つを呼び出します。

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

`x:Arguments` タグ内の要素の数、およびこれらの要素の型は、呼び出されるファクトリメソッドの引数と一致する必要があります。 [`FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Int32,System.Int32,System.Int32,System.Int32))ファクトリメソッドには、それぞれ 0 ~ 255 の範囲の赤、緑、青、およびアルファ値を表す4つの[`Int32`](https://docs.microsoft.com/dotnet/api/system.int32)パラメーターが必要です。 [`FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double))ファクトリメソッドには4つの[`Double`](https://docs.microsoft.com/dotnet/api/system.double)パラメーターが必要です。これらは、それぞれ0から1までの色相、鮮やかさ、明度、アルファ値を表します。 [`FromHex`](xref:Xamarin.Forms.Color.FromHex(System.String))ファクトリメソッドには、16進数 (a) の RGB 色を表す[`String`](https://docs.microsoft.com/dotnet/api/system.string)が必要です。

次のスクリーンショットは、指定された引数値を使用して各[`Color`](xref:Xamarin.Forms.Color)ファクトリメソッドを呼び出した結果を示しています。

![BoxView。 x:FactoryMethod および x:Arguments で指定した色](passing-arguments-images/factory-methods.png)

<a name="generic_type_arguments" />

## <a name="specifying-a-generic-type-argument"></a>ジェネリック型引数を指定します。

ジェネリック型のコンストラクターのジェネリック型引数は、次のコード例に示すように、`x:TypeArguments` 属性を使用して指定できます。

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

[`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1)クラスはジェネリッククラスであり、ターゲットの型に一致する `x:TypeArguments` 属性を使用してインスタンス化する必要があります。 [`On`](xref:Xamarin.Forms.On)クラスでは、 [`Platform`](xref:Xamarin.Forms.On.Platform)属性は1つの `string` 値、または複数のコンマ区切り `string` 値を受け取ることができます。 この例では、 [`StackLayout.Margin`](xref:Xamarin.Forms.View.Margin)プロパティはプラットフォーム固有の[`Thickness`](xref:Xamarin.Forms.Thickness)に設定されています。

## <a name="summary"></a>要約

この記事では、工場出荷時のメソッドを呼び出すと、ジェネリック引数の型を指定する既定以外のコンス トラクターに引数を渡すに使用できる XAML 属性を使用して示されています。

## <a name="related-links"></a>関連リンク

- [XAML 名前空間](~/xamarin-forms/xaml/namespaces.md)
- [コンストラクターの引数を渡す (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-passingconstructorarguments)
- [ファクトリメソッドの呼び出し (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-callingfactorymethods)
