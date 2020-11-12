---
title: Xamarin.Forms の基本的なバインディング
description: この記事では、通常は少なくとも一方がユーザー インターフェイス オブジェクトである 2 つのオブジェクト間でプロパティのペアをリンクする、Xamarin.Forms データ バインディングの使用方法について説明します。 これら 2 つのオブジェクトは、ターゲットおよびソースと呼ばれます。
ms.prod: xamarin
ms.assetid: 96553DF7-12EA-4FB2-AE85-3D1D59382B40
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/22/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.custom: video
ms.openlocfilehash: 107916edee01171b8ff5d4871de3b1243c385dd5
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93366426"
---
# <a name="no-locxamarinforms-basic-bindings"></a>Xamarin.Forms の基本的なバインディング

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/databindingdemos)

Xamarin.Forms のデータ バインディングでは、通常は少なくとも一方がユーザー インターフェイス オブジェクトである 2 つのオブジェクト間において、プロパティのペアをリンクします。 これら 2 つのオブジェクトは、" *ターゲット* " および " *ソース* " と呼ばれます。

- " *ターゲット* " は、データ バインディングが設定されている方のオブジェクト (およびプロパティ) です。
- " *ソース* " は、データ バインディングによって参照されている方のオブジェクト (およびプロパティ) です。

この区別は少し混乱を招くことがあります。最も単純な場合は、データがソースからターゲットに流れます。これは、ソース プロパティの値からターゲット プロパティの値が設定されることを意味します。 ただし、場合によっては、データがターゲットからソースに、または双方向に流れることがあります。 混乱を避けるには、ターゲットは (データを受け取るのではなく提供する場合でも) 常に、データ バインディングが設定されている方のオブジェクトであることに留意します。

## <a name="bindings-with-a-binding-context"></a>バインディング コンテキストを使用したバインド

通常はデータ バインディングをすべて XAML で指定しますが、コードでのデータ バインディングを確認することは有益です。 **Basic Code Binding** (基本的なコード バインディング) ページの内容は、`Label` と `Slider` を使用した XAML ファイルです。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.BasicCodeBindingPage"
             Title="Basic Code Binding">
    <StackLayout Padding="10, 0">
        <Label x:Name="label"
               Text="TEXT"
               FontSize="48"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

`Slider` は、0 から 360 までの範囲に設定されています。 このプログラムは、`Slider` を操作することによって `Label` を回転させることを意図したものです。

データ バインディングを使用しない場合は、`Slider` の `ValueChanged` イベントを、`Slider` の `Value` プロパティにアクセスしてその値を `Label` の `Rotation` プロパティに設定するイベント ハンドラーに設定します。 データ バインディングによって、そのジョブが自動化されます。イベント ハンドラーおよびイベント ハンドラー内のコードは不要になります。

[`BindableObject`](xref:Xamarin.Forms.BindableObject) から派生した任意のクラスのインスタンスにバインディングを設定できます。これには、`Element`、`VisualElement`、`View`、`View` の派生クラスが含まれます。  バインディングは、常にターゲット オブジェクトに設定されます。 そのバインディングによって、ソース オブジェクトが参照されます。 データ バインディングを設定するには、次に示すターゲット クラスの 2 つのメンバーを使用します。

- [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) プロパティでは、ソース オブジェクトを指定します。
- [`SetBinding`](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) メソッドでは、ターゲット プロパティとソース プロパティを指定します。

この例では、`Label` がバインディング ターゲットで、`Slider` がバインディング ソースです。 `Slider` ソースにおける変更が、`Label` テンプレートの回転に影響を与えます。 データはソースからターゲットに流れます。

`BindableObject` によって定義された `SetBinding` メソッドには、[`Binding`](xref:Xamarin.Forms.Binding) クラスの派生元となる [`BindingBase`](xref:Xamarin.Forms.BindingBase) 型の引数が使用されていますが、[`BindableObjectExtensions`](xref:Xamarin.Forms.BindableObjectExtensions) クラスによって定義された他の `SetBinding` メソッドがあります。 **Basic Code Binding** (基本的なコード バインディング) サンプルの分離コード ファイルでは、よりシンプルな、このクラスの [`SetBinding`](xref:Xamarin.Forms.BindableObjectExtensions.SetBinding*) 拡張メソッドを使用しています。

```csharp
public partial class BasicCodeBindingPage : ContentPage
{
    public BasicCodeBindingPage()
    {
        InitializeComponent();

        label.BindingContext = slider;
        label.SetBinding(Label.RotationProperty, "Value");
    }
}
```

`Label` オブジェクトはバインディング ターゲットです。したがって、このプロパティが設定されるオブジェクトであり、メソッドが呼び出される対象のオブジェクトです。 `BindingContext` プロパティでは、バインディング ソースである `Slider` が示されています。

`SetBinding` メソッドはバインディング ターゲットに対して呼び出されますが、ターゲット プロパティとソース プロパティの両方を指定しています。 ターゲット プロパティである `Label.RotationProperty` は、`BindableProperty` オブジェクトとして指定されています。 ソース プロパティは文字列として指定され、`Slider` の `Value` プロパティを示しています。

`SetBinding` メソッドにより、データ バインディングの最も重要なルールの 1 つが明らかになります。

*ターゲット プロパティは、バインド可能なプロパティによってサポートされている必要があります。*

このルールは、ターゲット オブジェクトが、`BindableObject` から派生したクラスのインスタンスである必要があることを示しています。 バインド可能なオブジェクトとバインド可能なプロパティの概要については、「 [**バインド可能なプロパティ**](~/xamarin-forms/xaml/bindable-properties.md)」の記事を参照してください。

ソース プロパティにこのようなルールはなく、文字列として指定されます。 内部的には、実際のプロパティにアクセスするためにリフレクションが使用されます。 ただし、この特定のケースでは、`Value` プロパティはバインド可能なプロパティによってもサポートされています。

コードはいくらか簡素化することができます。バインド可能なプロパティである `RotationProperty` は、`VisualElement` によって定義され、`Label` と `ContentPage` によって継承されるため、`SetBinding` 呼び出し内にクラス名は必要なくなります。

```csharp
label.SetBinding(RotationProperty, "Value");
```

ただし、クラス名を含めておくと、ターゲット オブジェクトのよいリマインダーになります。

`Slider` を操作すると、それに応じて `Label` が回転します。

[![Basic Code Binding](basic-bindings-images/basiccodebinding-small.png "Basic Code Binding")](basic-bindings-images/basiccodebinding-large.png#lightbox "Basic Code Binding")

**Basic Xaml Binding** (基本的な XAML バインディング) ページは、すべてのデータ バインディングを XAML で定義している点を除き、 **Basic Code Binding** (基本的なコード バインディング) と同じです。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.BasicXamlBindingPage"
             Title="Basic XAML Binding">
    <StackLayout Padding="10, 0">
        <Label Text="TEXT"
               FontSize="80"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand"
               BindingContext="{x:Reference Name=slider}"
               Rotation="{Binding Path=Value}" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

コードの場合と同様、ターゲット オブジェクトである `Label` にデータ バインディングを設定しています。 2 つの XAML マークアップ拡張が使用されています。 それらは、中かっこの区切り記号によって、すぐに見分けがつきます。

- `x:Reference` マークアップ拡張は、`slider` という名前の `Slider` であるソース オブジェクトを参照するために必要です。
- `Binding` マークアップ拡張によって、`Label` の `Rotation` プロパティが、`Slider` の `Value` プロパティにリンクされます。

XAML マークアップ拡張の詳細については、「[XAML マークアップ拡張](~/xamarin-forms/xaml/markup-extensions/index.md)」の記事を参照してください。 `x:Reference` マークアップ拡張は、[`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension) クラスによってサポートされています。`Binding` は、[`BindingExtension`](xref:Xamarin.Forms.Xaml.BindingExtension) クラスによってサポートされています。 XML 名前空間のプレフィックスが示すように、`x:Reference` は XAML 2009 仕様の一部です。一方、`Binding` は Xamarin.Forms の一部です。 中かっこ内に引用符が使用されていないことに注目してください。

`BindingContext` を設定するとき、`x:Reference` マークアップ拡張を忘れがちです。 次のように、誤ってプロパティをバインディング ソースの名前にじかに設定することがよくあります。

```xaml
BindingContext="slider"
```

しかし、これは正しくありません。 そのマークアップでは、`BindingContext` プロパティが、"slider" というスペルの英字の `string` オブジェクトに設定されます。

ソース プロパティが `BindingExtension` の [`Path`](xref:Xamarin.Forms.Xaml.BindingExtension.Path) プロパティを使用して指定されていることに注意してください。これは、[`Binding`](xref:Xamarin.Forms.Binding) クラスの [`Path`](xref:Xamarin.Forms.Binding.Path) プロパティに対応します。

**Basic XAML Binding** (基本的な XAML バインディング) ページに示されているマークアップは、簡素化することができます。`x:Reference` や `Binding` などの XAML マークアップ拡張には、 *コンテンツ プロパティ* 属性を定義することができます。これは、XAML マークアップ拡張に対してプロパティ名を使用する必要がないことを意味します。 `Name` プロパティは `x:Reference` のコンテンツ プロパティであり、`Path` プロパティは `Binding` のコンテンツ プロパティです。つまり、これらは式から省略できます。

```xaml
<Label Text="TEXT"
       FontSize="80"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       BindingContext="{x:Reference slider}"
       Rotation="{Binding Value}" />
```

## <a name="bindings-without-a-binding-context"></a>バインディング コンテキストを使用しないバインド

`BindingContext` プロパティは、データ バインディングの重要なコンポーネントですが、常に必要なわけではありません。 代わりに `SetBinding` 呼び出しまたは `Binding` マークアップ拡張においてソース オブジェクトを指定できます。

これは、 **Alternative Code Binding** サンプルに示されています。 XAML ファイルは、`Label` の `Scale` プロパティを制御するための `Slider` が定義されている点を除き、 **Basic Code Binding** (基本的なコード バインディング) サンプルと同様です。 その理由のため、`Slider` は &ndash;2 から 2 までの範囲に設定されています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.AlternativeCodeBindingPage"
             Title="Alternative Code Binding">
    <StackLayout Padding="10, 0">
        <Label x:Name="label"
               Text="TEXT"
               FontSize="40"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Minimum="-2"
                Maximum="2"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

分離コード ファイルでは、`BindableObject` によって定義された [`SetBinding`](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) メソッドを使用してバインディングを設定します。 引数は、[`Binding`](xref:Xamarin.Forms.Binding) クラスの [constructor](xref:Xamarin.Forms.Binding.%23ctor(System.String,Xamarin.Forms.BindingMode,Xamarin.Forms.IValueConverter,System.Object,System.String,System.Object)) です。

```csharp
public partial class AlternativeCodeBindingPage : ContentPage
{
    public AlternativeCodeBindingPage()
    {
        InitializeComponent();

        label.SetBinding(Label.ScaleProperty, new Binding("Value", source: slider));
    }
}
```

`Binding` コンストラクターには 6 つのパラメーターがあるため、名前付き引数を使用して `source` パラメーターが指定されています。 その引数は `slider` オブジェクトです。

このプログラムを実行すると、少々驚くかもしれません。

[![Alternative Code Binding](basic-bindings-images/alternativecodebinding-small.png "Alternative Code Binding")](basic-bindings-images/alternativecodebinding-large.png#lightbox "Alternative Code Binding")

左側の iOS 画面は、ページが最初に表示されたときに画面がどのように見えるかを示しています。 `Label` はどこでしょうか。

問題は、`Slider` が初期値の 0 になっていることです。 このため、`Label` の `Scale` プロパティも 0 に設定され、既定値の 1 がオーバーライドされています。 この結果、最初は `Label` が表示されません。 Android のスクリーンショットが示すように、`Slider` を操作して `Label` が再び表示されるようにすることができますが、最初に表示されないのは不安を与えます。

[次の記事](binding-mode.md)では、`Scale` プロパティの既定値から `Slider` を初期化することによる、この問題の回避方法について説明しています。

> [!NOTE]
> [`VisualElement`](xref:Xamarin.Forms.VisualElement) クラスでは、[`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX) プロパティと [`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY) プロパティも定義しています。これにより、`VisualElement` を縦方向と横方向に別々にスケールできます。

**Alternative XAML Binding** (代替 XAML バインディング) ページには、同等のバインディングがすべて XAML で示されます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.AlternativeXamlBindingPage"
             Title="Alternative XAML Binding">
    <StackLayout Padding="10, 0">
        <Label Text="TEXT"
               FontSize="40"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand"
               Scale="{Binding Source={x:Reference slider},
                               Path=Value}" />

        <Slider x:Name="slider"
                Minimum="-2"
                Maximum="2"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

これで、`Binding` マークアップ拡張には、コンマで区切った `Source` と `Path` という 2 つのプロパティが設定されました。 好みにより、これらは同じ行で使用することもできます。

```xaml
Scale="{Binding Source={x:Reference slider}, Path=Value}" />
```

`Source` プロパティは、埋め込み `x:Reference` マークアップ拡張に設定されています。それ以外の場合、`BindingContext` を設定するのと同じ構文になります。 中かっこ内に引用符が使用されていないこと、および 2 つのプロパティをコンマで区切る必要があることに注意してください。

`Binding` マークアップ拡張のコンテンツ プロパティは `Path` ですが、そのマークアップ拡張の `Path=` の部分は、式の中で最初のプロパティである場合にのみ省略できます。 `Path=` の部分を省略するには、2 つのプロパティを入れ替える必要があります。

```xaml
Scale="{Binding Value, Source={x:Reference slider}}" />
```

XAML マークアップ拡張は、通常は中かっこで区切りますが、オブジェクト要素として表すこともできます。

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand">
    <Label.Scale>
        <Binding Source="{x:Reference slider}"
                 Path="Value" />
    </Label.Scale>
</Label>
```

これで、`Source` プロパティと `Path` プロパティは、標準の XAML 属性になりました。値は引用符で囲んで使用し、属性はコンマで区切りません。 `x:Reference` マークアップ拡張は、オブジェクト要素にすることもできます。

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand">
    <Label.Scale>
        <Binding Path="Value">
            <Binding.Source>
                <x:Reference Name="slider" />
            </Binding.Source>
        </Binding>
    </Label.Scale>
</Label>
```

この構文は一般的ではありませんが、複合オブジェクトを使用する場合に必要になることがあります。

これまでに示した例では、`Binding` の `BindingContext` プロパティと `Source` プロパティを `x:Reference` マークアップ拡張に設定して、ページ上の別のビューを参照しています。 これら 2 つのプロパティは、`Object` 型であり、ソースのバインドに適したプロパティを含む任意のオブジェクトに設定することができます。

この後の記事では、`BindingContext` または `Source` プロパティを `x:Static` マークアップ拡張に設定して静的プロパティまたはフィールドの値を参照したり、`StaticResource` マークアップ拡張に設定して、リソース ディクショナリに格納されているオブジェクトを参照したりできること、あるいは、一般的には ViewModel のインスタンス (しかし常にそうとは限らない) であるオブジェクトに直接設定できることがわかります。

`Binding` の `Source` プロパティと `Path` プロパティによってバインディング コンテキストが定義されるように `BindingContext` プロパティを `Binding` オブジェクトに設定することもできます。

## <a name="binding-context-inheritance"></a>バインディング コンテキストの継承

この記事では、`Binding` オブジェクトの `BindingContext` プロパティまたは `Source` プロパティを使用してソース オブジェクトを指定できることを説明しました。 両方が設定されている場合、`Binding` の `Source` プロパティが `BindingContext` よりも優先されます。

`BindingContext` プロパティには非常に重要な特性があります。

*`BindingContext` プロパティの設定は、ビジュアル ツリーを介して継承されます。*

これから説明するように、これはバインディング式を簡素化するために非常に便利であり、場合によっては (特に Model-View-ViewModel (MVVM) のシナリオにおいては)、不可欠です。

**Binding Context Inheritance** (バインディング コンテキストの継承) サンプルは、バインディング コンテキストの継承を示す簡単なデモです。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.BindingContextInheritancePage"
             Title="BindingContext Inheritance">
    <StackLayout Padding="10">

        <StackLayout VerticalOptions="FillAndExpand"
                     BindingContext="{x:Reference slider}">

            <Label Text="TEXT"
                   FontSize="80"
                   HorizontalOptions="Center"
                   VerticalOptions="EndAndExpand"
                   Rotation="{Binding Value}" />

            <BoxView Color="#800000FF"
                     WidthRequest="180"
                     HeightRequest="40"
                     HorizontalOptions="Center"
                     VerticalOptions="StartAndExpand"
                     Rotation="{Binding Value}" />
        </StackLayout>

        <Slider x:Name="slider"
                Maximum="360" />

    </StackLayout>
</ContentPage>
```

`StackLayout` の `BindingContext` プロパティは、`slider` オブジェクトに設定されています。 このバインディング コンテキストは、`Label` と `BoxView` の両方によって継承され、これらは両方とも、`Rotation` プロパティが `Slider` の `Value` に設定されています。

[![バインディング コンテキストの継承](basic-bindings-images/bindingcontextinheritance-small.png "バインディング コンテキストの継承")](basic-bindings-images/bindingcontextinheritance-large.png#lightbox "バインディング コンテキストの継承")

[次の記事](binding-mode.md)では、ターゲット オブジェクトとソース オブジェクトの間のデータ フローが *バインディング モード* によってどのように変わるかについて説明します。

## <a name="related-links"></a>関連リンク

- [データ バインディングのデモ (サンプル)](/samples/xamarin/xamarin-forms-samples/databindingdemos)
- [Xamarin.Forms ブックのデータ バインディングに関する章](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/XamarinForms-101-Data-Binding/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]