---
title: Xamarin.Forms Basic バインド
description: この記事では、Xamarin.Forms データ バインディング、少なくとも 2 つのオブジェクト間のプロパティのペアへのリンクうちの 1 つは、通常はユーザー インターフェイス オブジェクトを使用する方法について説明します。 これら 2 つのオブジェクトは、ターゲットとソースと呼ばれます。
ms.prod: xamarin
ms.assetid: 96553DF7-12EA-4FB2-AE85-3D1D59382B40
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: e31cba5c61624b0bca03443262b95d7497564750
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675199"
---
# <a name="xamarinforms-basic-bindings"></a>Xamarin.Forms Basic バインド

Xamarin.Forms のデータ バインディングは、1 組のうち少なくとも 1 つはユーザー インターフェイス オブジェクトでは、通常は、2 つのオブジェクト間のプロパティをリンクします。 これら 2 つのオブジェクトが呼び出されます、*ターゲット*と*ソース*:

- *ターゲット*オブジェクト (およびのプロパティ) がデータ バインディングが設定されています。
- *ソース*はデータ バインドによって参照されるオブジェクト (およびプロパティ)。

この区別を少し混乱を招く場合があります。 最も簡単な場合は、データ フロー ソースから、ターゲットは、ソース プロパティの値からターゲット プロパティの値を設定することを意味します。 ただし、場合によっては、データまたはフローできますターゲットからソース、または双方向で。 混乱を回避するには、ターゲットは、データ バインディングが設定されている場合でも、これはデータを提供するではなくデータの受信オブジェクトでは常に注意してください。

## <a name="bindings-with-a-binding-context"></a>バインディング コンテキストを持つバインド

データ バインドの指定が全体を XAML では、通常は、コードでのデータ バインドを確認することができます。 **コードの基本的なバインディング**ページには XAML ファイルが含まれています、`Label`と`Slider`:

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

`Slider` 0 ~ 360 の範囲の設定です。 このプログラムの目的は、回転する、`Label`操作することによって、`Slider`します。

設定データ バインディング、なし、`ValueChanged`のイベント、`Slider`イベント ハンドラーにアクセスするに、`Value`のプロパティ、`Slider`し、その値の設定、`Rotation`のプロパティ、 `Label`。 データ バインディングは、そのジョブを自動化します。イベント ハンドラーとその中のコードが必要ではありません。

派生したクラスのインスタンスにバインドを設定する[ `BindableObject`](xref:Xamarin.Forms.BindableObject)が含まれています`Element`、 `VisualElement`、 `View`、および`View`派生クラス。  バインディングは、対象のオブジェクトで常に設定されます。 バインディングは、ソース オブジェクトを参照します。 データ バインディングを設定するには、次の 2 つのターゲット クラスのメンバーを使用します。

- [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)プロパティ ソース オブジェクトを指定します。
- [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase))メソッドは、ターゲット プロパティと基になるプロパティを指定します。

この例で、`Label`バインディングのターゲットと`Slider`バインド ソースであります。 変更、`Slider`ソースの回転に影響を与える、`Label`ターゲット。 データは、ソースからターゲットに送られます。

`SetBinding`メソッドによって定義された`BindableObject`型の引数を持つ[ `BindingBase` ](xref:Xamarin.Forms.BindingBase)元となる、 [ `Binding` ](xref:Xamarin.Forms.Binding)クラスが派生が、その他を使用する必要がある`SetBinding`メソッドによって定義された、 [ `BindableObjectExtensions` ](xref:Xamarin.Forms.BindableObjectExtensions)クラス。 分離コード ファイル、**コードの基本的なバインディング**サンプルを使用して、簡単な[ `SetBinding` ](xref:Xamarin.Forms.BindableObjectExtensions.SetBinding*)このクラスから拡張メソッド。

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

`Label`オブジェクトにはバインディング ターゲットがあるため、このプロパティを設定して、メソッドが呼び出されたオブジェクトであること。 `BindingContext`プロパティがあるバインディング ソースを示します、`Slider`します。

`SetBinding`メソッドがバインディングのターゲットで呼び出されますが、ターゲット プロパティと基になるプロパティの両方を指定します。 ターゲット プロパティとして指定する`BindableProperty`オブジェクト:`Label.RotationProperty`します。 ソース プロパティが文字列として指定され、ことを示します、`Value`プロパティの`Slider`します。

`SetBinding`メソッドでは、データ バインドの最も重要な規則のいずれかが表示されます。

*ターゲット プロパティは、バインド可能なプロパティでバックアップする必要があります。*

このルールは、ターゲット オブジェクトから派生したクラスのインスタンスである必要がありますを意味`BindableObject`します。 参照してください、 [**バインド可能なプロパティ**](~/xamarin-forms/xaml/bindable-properties.md)バインド可能なオブジェクトとバインド可能なプロパティの概要についての記事。

文字列として指定されたソース プロパティのようなルールはありません。 内部的には、リフレクションを使用して、実際のプロパティにアクセスします。 このケースで、ただし、`Value`プロパティがバインド可能なプロパティによってもサポートします。

コードは、若干簡素化:`RotationProperty`によってバインド可能なプロパティが定義されている`VisualElement`、によって継承されて`Label`と`ContentPage`内にクラス名は必要ためにも、`SetBinding`呼び出し。

```csharp
label.SetBinding(RotationProperty, "Value");
```

ただし、クラス名を含むは、ターゲット オブジェクトの適切なリマインダーです。

操作するとき、 `Slider`、`Label`適宜回転します。

[![基本的なコードのバインディング](basic-bindings-images/basiccodebinding-small.png "基本的なコードのバインディング")](basic-bindings-images/basiccodebinding-large.png#lightbox "基本的なコードのバインディング")

**基本的な Xaml バインディング**ページと同じ**コードの基本的なバインディング**XAML 内のすべてのデータ バインディングを定義する点が異なります。

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

これはターゲット オブジェクトのデータ バインディングを設定、コードと同じように、`Label`します。 2 つの XAML マークアップ拡張機能があります。 中かっこ区切り記号がすぐに認識できるものがあります。

- `x:Reference`となるソース オブジェクトを参照するマークアップ拡張機能が必要です、`Slider`という`slider`します。
- `Binding`マークアップ拡張機能へのリンク、`Rotation`のプロパティ、`Label`を`Value`のプロパティ、`Slider`します。

記事をご覧ください[XAML マークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/index.md)XAML マークアップ拡張機能の詳細についてはします。 `x:Reference`によってマークアップ拡張機能がサポートされている、 [ `ReferenceExtension` ](xref:Xamarin.Forms.Xaml.ReferenceExtension)クラスです。`Binding`でサポートされて、 [ `BindingExtension` ](xref:Xamarin.Forms.Xaml.BindingExtension)クラス。 Xml 名前空間プレフィックスを示す、 `x:Reference` XAML 2009 の仕様の一部である中`Binding`は Xamarin.Forms の一部です。 中かっこ内に引用符が表示されないことに注意してください。

これは忘れやすい、`x:Reference`マークアップ拡張機能を設定するとき、`BindingContext`します。 このようなバインディング ソースの名前に直接プロパティを誤って設定一般的には。

```xaml
BindingContext="slider"
```

適切でないです。 そのマークアップは、`BindingContext`プロパティを`string`"slider"のスペル文字のオブジェクト。

ソースのプロパティを指定した通知、 [ `Path` ](xref:Xamarin.Forms.Xaml.BindingExtension.Path)プロパティの`BindingExtension`に対応する、 [ `Path` ](xref:Xamarin.Forms.Binding.Path)のプロパティ、 [ `Binding`](xref:Xamarin.Forms.Binding)クラス。

表示されるマークアップ、**基本的な XAML バインディング**ページを簡略化できます: などの XAML マークアップ拡張機能`x:Reference`と`Binding`が*プロパティのコンテンツ*属性が定義する XAML のマークアップ拡張機能では、プロパティ名が表示される必要があることを意味します。 `Name`プロパティは、コンテンツのプロパティの`x:Reference`、および`Path`プロパティは、コンテンツのプロパティの`Binding`、つまり、式から取り除くことができます。

```xaml
<Label Text="TEXT"
       FontSize="80"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       BindingContext="{x:Reference slider}"
       Rotation="{Binding Value}" />
```

## <a name="bindings-without-a-binding-context"></a>バインド コンテキストなしのバインド

`BindingContext`プロパティは、データ バインディングの重要なコンポーネントが常に必要はありません。 ソース オブジェクトは、の代わりに指定できます、`SetBinding`呼び出しまたは`Binding`マークアップ拡張機能。

これは、方法については、**代わりにコードのバインディング**サンプル。 XAML ファイルと似ています、**コードの基本的なバインディング**サンプル点を除いて、`Slider`コントロールに定義されている場合は、`Scale`のプロパティ、 `Label`。 そのため、`Slider`の範囲の設定は&ndash;2 ~ 2。

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

分離コード ファイルでバインディングの設定、 [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase))メソッドによって定義された`BindableObject`します。 引数が、[コンス トラクター](xref:Xamarin.Forms.Binding.%23ctor(System.String,Xamarin.Forms.BindingMode,Xamarin.Forms.IValueConverter,System.Object,System.String,System.Object))の[ `Binding` ](xref:Xamarin.Forms.Binding)クラス。

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

`Binding`コンス トラクターは 6 つのパラメーターを持つため、`source`名前付き引数を含むパラメーターが指定されています。 引数が、`slider`オブジェクト。

このプログラムを実行するには、少し驚く可能性があります。

[![代替のコードのバインディング](basic-bindings-images/alternativecodebinding-small.png "代替コードのバインディング")](basic-bindings-images/alternativecodebinding-large.png#lightbox "代替コードのバインディング")

左側の [iOS] 画面では、ページが表示される最初の画面の外観を示します。 場所は、`Label`でしょうか。

問題なは、`Slider`が初期値は 0。 これにより、`Scale`のプロパティ、`Label`も 1 の場合は、その既定値をオーバーライドする 0 に設定します。 これは、結果、`Label`最初は表示されません。 操作できるように Android およびユニバーサル Windows プラットフォーム (UWP) のスクリーン ショットに示すため、`Slider`させる、 `Label` 、もう一度表示されますが、その初期消滅の混乱を招く。

わかる、[次の記事](binding-mode.md)初期化することによってこの問題を回避する方法、`Slider`の既定値から、`Scale`プロパティ。

> [!NOTE]
> [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)クラスも定義[ `ScaleX` ](xref:Xamarin.Forms.VisualElement.ScaleX)と[ `ScaleY` ](xref:Xamarin.Forms.VisualElement.ScaleY)プロパティ、拡張することが、`VisualElement`異なる方法で、水平および垂直方向。

**代わりに XAML バインディング**ページ全体を XAML で等価なバインドが表示されます。

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

これで、`Binding`マークアップ拡張機能は 2 つのプロパティを設定すると、`Source`と`Path`コンマで区切られました。 場合は、同じ行に表示されることができます。

```xaml
Scale="{Binding Source={x:Reference slider}, Path=Value}" />
```

`Source`プロパティが設定される埋め込み`x:Reference`をそれ以外の場合の設定と同じ構文を持つマークアップ拡張機能、`BindingContext`します。 される 2 つのプロパティをコンマで区切られる必要があります、中かっこ内に引用符が表示されないことに注意してください。

コンテンツのプロパティ、`Binding`マークアップ拡張機能は`Path`が、`Path=`マークアップ拡張機能の一部は、式の最初のプロパティである場合にのみ排除できます。 排除するために、`Path=`一部、2 つのプロパティをスワップする必要があります。

```xaml
Scale="{Binding Value, Source={x:Reference slider}}" />
```

XAML マークアップ拡張機能は、通常は、中かっこで区切られた、オブジェクト要素として、表現できます。

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

これで、`Source`と`Path`プロパティは、通常 XAML 属性: 引用符で囲まれた値が表示され、属性は、コンマで区切られていません。 `x:Reference`マークアップ拡張機能は、オブジェクト要素にもなります。

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

この構文は一般的ではありませんが必要な複雑なオブジェクトが関係する場合があります。

これまでに示した例では、設定、`BindingContext`プロパティおよび`Source`プロパティの`Binding`を`x:Reference`ページ上の別のビューを参照するマークアップ拡張機能。 これら 2 つのプロパティが型`Object`ソースのバインドに適しているプロパティを含む任意のオブジェクトに設定することができます。

今後の記事で設定できることを検出します、`BindingContext`または`Source`プロパティを`x:Static`静的プロパティまたはフィールドの値を参照するマークアップ拡張機能または`StaticResource`に格納されているオブジェクトを参照するマークアップ拡張機能、リソース ディクショナリ、または直接のオブジェクトには通常 (が常にではありません)、ViewModel のインスタンス。

`BindingContext`プロパティを設定することも、`Binding`オブジェクトように、`Source`と`Path`のプロパティ`Binding`バインディング コンテキストを定義します。

## <a name="binding-context-inheritance"></a>バインド コンテキストの継承

この記事では、ソース オブジェクトを使用して指定できることを説明しました、`BindingContext`プロパティまたは`Source`のプロパティ、`Binding`オブジェクト。 両方が設定されている場合、`Source`のプロパティ、`Binding`よりも優先、`BindingContext`します。

`BindingContext`プロパティが非常に重要な特性。

*設定、`BindingContext`プロパティは、ビジュアル ツリーから継承されます。*

後ほど説明すると、場合によっては、バインド式を簡略化するために非常に便利なできます&mdash;モデル-ビュー-ビューモデル (MVVM) のシナリオで特に&mdash;ことが重要です。

**バインド コンテキストの継承**サンプルは、バインディング コンテキストの継承の簡単なデモ。

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

`BindingContext`のプロパティ、`StackLayout`に設定されている、`slider`オブジェクト。 このバインディング コンテキストが両方によって継承される、`Label`と`BoxView`の両方をされて、`Rotation`プロパティに設定、`Value`のプロパティ、 `Slider`:

[![バインド コンテキストの継承](basic-bindings-images/bindingcontextinheritance-small.png "バインド コンテキストの継承")](basic-bindings-images/bindingcontextinheritance-large.png#lightbox "バインド コンテキストの継承")

[次の記事](binding-mode.md)が表示されますが、どのように*バインド モード*ターゲットとソースのオブジェクト間のデータ フローを変更することができます。


## <a name="related-links"></a>関連リンク

- [データ バインディング デモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms book からデータ バインド」の章](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
