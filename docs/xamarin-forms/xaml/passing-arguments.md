---
title: XAML での引数の受け渡し
description: この記事では、既定以外のコンストラクターに引数を渡すために使用できる XAML 属性の使用方法、ファクトリメソッドを呼び出す方法、およびジェネリック引数の型を指定する方法について説明します。
ms.prod: xamarin
ms.assetid: 8F3B267F-499E-4D79-9193-FCA99F199519
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: dcf09adc690aee5487107630eb74bb8c4e9599cb
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91562172"
---
# <a name="passing-arguments-in-xaml"></a>XAML での引数の受け渡し

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-passingconstructorarguments)

_この記事では、既定以外のコンストラクターに引数を渡すために使用できる XAML 属性の使用方法、ファクトリメソッドを呼び出す方法、およびジェネリック引数の型を指定する方法について説明します。_

## <a name="overview"></a>概要

多くの場合、引数を必要とするコンストラクターを使用するか、静的作成メソッドを呼び出すことによって、オブジェクトをインスタンス化する必要があります。 これは、XAML で属性と属性を使用して実現でき `x:Arguments` `x:FactoryMethod` ます。

- 属性は、 `x:Arguments` 既定以外のコンストラクターまたはファクトリメソッドオブジェクトの宣言にコンストラクター引数を指定するために使用されます。 詳細については、「 [コンストラクター引数の引き渡し](#passing-constructor-arguments)」を参照してください。
- `x:FactoryMethod`属性は、オブジェクトの初期化に使用できるファクトリメソッドを指定するために使用されます。 詳細については、「 [ファクトリメソッドの呼び出し](#calling-factory-methods)」を参照してください。

また、属性を使用して、ジェネリック型 `x:TypeArguments` のコンストラクターに対するジェネリック型引数を指定することもできます。 詳細については、「 [ジェネリック型引数の指定](#specifying-a-generic-type-argument)」を参照してください。

## <a name="passing-constructor-arguments"></a>コンストラクター引数の引き渡し

属性を使用して、既定以外のコンストラクターに引数を渡すことができ `x:Arguments` ます。 各コンストラクター引数は、引数の型を表す XML 要素内で区切る必要があります。 Xamarin.Forms では、基本型に対して次の要素がサポートされています。

- `x:Array`
- `x:Boolean`
- `x:Byte`
- `x:Char`
- `x:DateTime`
- `x:Decimal`
- `x:Double`
- `x:Int16`
- `x:Int32`
- `x:Int64`
- `x:Object`
- `x:Single`
- `x:String`
- `x:TimeSpan`

次のコード例では、 `x:Arguments` 3 つのコンストラクターを使用して属性を使用する方法を示し [`Color`](xref:Xamarin.Forms.Color) ます。

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

タグ内の要素の数、 `x:Arguments` およびこれらの要素の型は、コンストラクターのいずれかに一致する必要があり [`Color`](xref:Xamarin.Forms.Color) ます。 `Color`1 つのパラメーターを持つ[コンストラクター](xref:Xamarin.Forms.Color.%23ctor(System.Double))には、0 (黒) から 1 (白) までのグレースケール値を指定する必要があります。 `Color`3 つのパラメーターを持つ[コンストラクター](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double))には、0 ~ 1 の範囲の赤、緑、および青の値が必要です。 `Color`4 つのパラメーターを持つ[コンストラクター](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double,System.Double))は、4番目のパラメーターとしてアルファチャネルを追加します。

次のスクリーンショットは、 [`Color`](xref:Xamarin.Forms.Color) 指定された引数値を使用して各コンストラクターを呼び出した結果を示しています。

![BoxView。 x:Arguments で指定した色](passing-arguments-images/passing-arguments.png)

## <a name="calling-factory-methods"></a>ファクトリメソッドの呼び出し

ファクトリメソッドは、属性を使用してメソッドの名前を指定し、属性を使用してその引数を指定することで、XAML で呼び出すことができ `x:FactoryMethod` `x:Arguments` ます。 ファクトリメソッドは、メソッドを `public static` 定義するクラスまたは構造体と同じ型のオブジェクトまたは値を返すメソッドです。

[`Color`](xref:Xamarin.Forms.Color)構造体は、多数のファクトリメソッドを定義します。次のコード例では、そのうち3つを呼び出します。

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

タグ内の要素の数、 `x:Arguments` およびこれらの要素の型は、呼び出されるファクトリメソッドの引数と一致する必要があります。 [`FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Int32,System.Int32,System.Int32,System.Int32))ファクトリメソッドには、 [`Int32`](/dotnet/api/system.int32) 赤、緑、青、およびアルファ値を表す4つのパラメーターが必要です。これは、それぞれ 0 ~ 255 の範囲内にあります。 [`FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double))ファクトリメソッドには、 [`Double`](/dotnet/api/system.double) 色合い、鮮やかさ、明るさ、およびアルファ値を表す4つのパラメーターが必要です。これは、それぞれ0から1までの範囲です。 [`FromHex`](xref:Xamarin.Forms.Color.FromHex(System.String))ファクトリメソッドには、 [`String`](/dotnet/api/system.string) 16 進数 (a) の RGB 色を表すが必要です。

次のスクリーンショットは、 [`Color`](xref:Xamarin.Forms.Color) 指定された引数値を使用して各ファクトリメソッドを呼び出した結果を示しています。

![BoxView。 x:FactoryMethod および x:Arguments で指定した色](passing-arguments-images/factory-methods.png)

## <a name="specifying-a-generic-type-argument"></a>ジェネリック型引数の指定

ジェネリック型のコンストラクターのジェネリック型引数は、 `x:TypeArguments` 次のコード例に示すように、属性を使用して指定できます。

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

[`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1)クラスはジェネリッククラスであり、 `x:TypeArguments` ターゲットの型に一致する属性を使用してインスタンス化する必要があります。 クラスでは、 [`On`](xref:Xamarin.Forms.On) [`Platform`](xref:Xamarin.Forms.On.Platform) 属性は単一の `string` 値または複数のコンマ区切り値を受け取ることができ `string` ます。 この例では、 [`StackLayout.Margin`](xref:Xamarin.Forms.View.Margin) プロパティはプラットフォーム固有のに設定されて [`Thickness`](xref:Xamarin.Forms.Thickness) います。

ジェネリック型引数の詳細については、「 [ Xamarin.Forms XAML のジェネリック](generics.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [コンストラクターの引数を渡す (サンプル)](/samples/xamarin/xamarin-forms-samples/xaml-passingconstructorarguments)
- [ファクトリメソッドの呼び出し (サンプル)](/samples/xamarin/xamarin-forms-samples/xaml-callingfactorymethods)
- [XAML 名前空間](~/xamarin-forms/xaml/namespaces.md)
- [XAML のジェネリック Xamarin.Forms](generics.md)