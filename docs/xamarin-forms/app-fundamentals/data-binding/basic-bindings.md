---
title: "基本的なバインディング"
description: "データ バインディング、ソース、およびバインディング コンテキスト"
ms.topic: article
ms.prod: xamarin
ms.assetid: 96553DF7-12EA-4FB2-AE85-3D1D59382B40
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: b82c0471985306962133c3bf7b084b49d5588bb6
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="basic-bindings"></a>基本的なバインディング

Xamarin.Forms データ バインディングは、通常、ユーザー インターフェイス オブジェクトを少なくとも 1 つは、2 つのオブジェクト間でのプロパティのペアをリンクします。 これら 2 つのオブジェクトが呼び出されます、*ターゲット*と*ソース*:

- *ターゲット*オブジェクト (およびプロパティ) は、データ バインディングが設定されています。
- *ソース*はデータ バインディングによって参照されるオブジェクト (およびプロパティ)。

この区別が、少しわかりにくいことがあります。 最も単純な場合は、データ フローからソースをターゲットにターゲット プロパティの値が基になるプロパティの値から設定されていることを意味します。 ただし、場合によっては、データまたはフローできますターゲットからソース、または双方向で。 混乱を避けるためには、ターゲットは、データ バインディングが設定されている場合でも、これはデータを提供するではなくデータを受信するオブジェクトでは常に注意してください。

## <a name="bindings-with-a-binding-context"></a>バインディング、バインディング コンテキストで

データ バインディングの指定が XAML では、通常、コードでデータ バインドを表示することができます。 **コードの基本的なバインディング**ページには、XAML ファイルが含まれています、`Label`と`Slider`:

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

`Slider` 0 ~ 360 の範囲を設定します。 このプログラムの目的は、回転する、`Label`操作することによって、`Slider`です。

設定とデータ バインディング、せず、`ValueChanged`のイベント、`Slider`にアクセスするイベント ハンドラーに、`Value`のプロパティ、`Slider`その値を設定し、`Rotation`のプロパティ、`Label`です。 データ バインディングがそのジョブを自動化します。イベント ハンドラーとその中のコードでは、必要なくなりました。 

派生したクラスのインスタンス上のバインドを設定することができます[ `BindableObject`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)が含まれている`Element`、 `VisualElement`、 `View`、および`View`派生します。  バインディングは、対象のオブジェクトで常に設定されます。 バインディングは、ソース オブジェクトを参照します。 データ バインディングを設定するには、ターゲット クラスの次の 2 つのメンバーを使用します。

- [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)プロパティは、ソース オブジェクトを指定します。
- [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/)メソッドは、ターゲット プロパティと基になるプロパティを指定します。

この例では、`Label`バインディング ターゲットは、および`Slider`バインド ソースであります。 変更内容、`Slider`ソースの回転の影響、`Label`ターゲットです。 データは、ソースからターゲットに送られます。

`SetBinding`メソッドによって定義された`BindableObject`型の引数を持つ[ `BindingBase` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingBase/)元となる、 [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/)クラスの派生があるその他の`SetBinding`メソッドによって定義された、 [ `BindableObjectExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObjectExtensions/)クラスです。 分離コード ファイルで、**コードの基本的なバインディング**サンプルを使用して、シンプル[ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObjectExtensions.SetBinding/p/Xamarin.Forms.BindableObject/Xamarin.Forms.BindableProperty/System.String/)このクラスから拡張メソッド。

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

`Label`オブジェクトは、このプロパティが設定されていると、メソッドが呼び出されるときに、オブジェクトになるようにバインディング ターゲットがします。 `BindingContext`プロパティがあるバインディング ソースを示す、`Slider`です。

`SetBinding`メソッドは、バインディング ターゲットで呼び出されますが、ターゲット プロパティと基になるプロパティの両方を指定します。 ターゲット プロパティとして指定する`BindableProperty`オブジェクト:`Label.RotationProperty`です。 基になるプロパティを文字列として指定されたことを示し、`Value`プロパティ`Slider`です。 

`SetBinding`メソッドでは、データ バインドの最も重要な規則のいずれかのことがわかります。

*ターゲット プロパティは、バインド可能なプロパティでバックアップする必要があります。*

このルールは、ターゲット オブジェクトはから派生したクラスのインスタンスである必要がありますを意味`BindableObject`です。 参照してください、 [**バインド可能なプロパティ**](~/xamarin-forms/xaml/bindable-properties.md)バインド可能なオブジェクトとバインド可能なプロパティの概要については資料です。

文字列として指定されたソース プロパティのようなルールはありません。 内部的には、リフレクションを使用して、実際のプロパティにアクセスできます。 この特定のケースで、ただし、`Value`プロパティがバインド可能なプロパティによってもサポートします。

コードを指定できますやや簡素化されます。`RotationProperty`によって連結可能なプロパティが定義されている`VisualElement`、および継承される`Label`と`ContentPage`同様に、そのため、クラス名ため必要はありませんで、`SetBinding`呼び出し。

```csharp
label.SetBinding(RotationProperty, "Value");
```

ただし、クラス名を含むターゲット オブジェクトの適切なアラームです。

操作するとき、 `Slider`、`Label`適宜回転させます。

[![バインド Basice コード](basic-bindings-images/basiccodebinding-small.png "基本的なコード バインディング")](basic-bindings-images/basiccodebinding-large.png#lightbox "基本的なコードのバインディング")

**基本的な Xaml バインディング**ページと同じ**コードの基本的なバインディング**XAML で全体のデータ バインディングを定義する点を除いて。

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

対象のオブジェクトでのデータ バインディングを設定、コードのように、`Label`です。 2 つの XAML マークアップ拡張機能が関与します。 次は、中かっこの区切り記号で即座に認識できるものがあります。

- `x:Reference`はソース オブジェクトを参照するマークアップ拡張機能が必要な`Slider`という`slider`です。
- `Binding`マークアップ拡張機能へのリンク、`Rotation`のプロパティ、`Label`を`Value`のプロパティ、`Slider`です。 

記事を参照して[XAML マークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/index.md)XAML マークアップ拡張機能の詳細についてはします。 `x:Reference`によってマークアップ拡張機能がサポートされている、 [ `ReferenceExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/)クラスです。`Binding`でサポートされて、 [ `BindingExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/)クラスです。 Xml 名前空間プレフィックスを示す、 `x:Reference` XAML 2009 の仕様の一部であるときに`Binding`Xamarin.Forms の一部であります。 中かっこ内に引用符が表示されないことに注意してください。

ことを忘れがち、`x:Reference`マークアップ拡張機能を設定する場合、`BindingContext`です。 これは、誤ってプロパティを設定する、次のように、バインディング ソースの名前を直接共通します。

```xaml
BindingContext="slider"
```

正しいことはありません。 そのマークアップ、`BindingContext`プロパティを`string`文字が"slider"のスペル オブジェクトです。

通知を基になるプロパティが指定されている、 [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.Path/)プロパティの`BindingExtension`に対応する、 [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/)のプロパティ、 [ `Binding`](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/)クラスです。

表示されるマークアップ、**基本的な XAML バインド**ページを簡略化できます: などの XAML マークアップ拡張機能`x:Reference`と`Binding`を持てる*コンテンツ プロパティ*属性が定義する XAML のマークアップ拡張機能では、プロパティ名が表示される必要があることを意味します。 `Name`プロパティのコンテンツ プロパティは、 `x:Reference`、および`Path`プロパティのコンテンツ プロパティは、 `Binding`、つまり、式から除外することができます。

```xaml
<Label Text="TEXT"
       FontSize="80"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       BindingContext="{x:Reference slider}"
       Rotation="{Binding Value}" />
```

## <a name="bindings-without-a-binding-context"></a>バインド コンテキストなしのバインド

`BindingContext`プロパティは、データ バインドの重要なコンポーネントが常に必要はありません。 ソース オブジェクトはの代わりに指定する、`SetBinding`呼び出しまたは`Binding`マークアップ拡張機能です。

これに示されている、**代替コード バインド**サンプルです。 XAML ファイルがに似ていますが、**コードの基本的なバインディング**サンプルの点を除いて、`Slider`コントロールに定義された、`Scale`のプロパティ、`Label`です。 そのため、`Slider`の範囲の設定は&ndash;2 ~ 2。

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

分離コード ファイルでバインディングの設定、 [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/)によって定義されたメソッド`BindableObject`です。 引数が、[コンス トラクター](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Binding.Binding/p/System.String/Xamarin.Forms.BindingMode/Xamarin.Forms.IValueConverter/System.Object/System.String/System.Object/)の[ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/)クラス。

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

`Binding`コンス トラクターは、6 つのパラメーターを持つため、`source`名前付き引数とパラメーターを指定します。 引数が、`slider`オブジェクト。 

このプログラムを実行するには、少しことにより意外可能性があります。

[![代替コード バインディング](basic-bindings-images/alternativecodebinding-small.png "代替コード バインディング")](basic-bindings-images/alternativecodebinding-large.png#lightbox "代替コードのバインディング")

左側の iOS の画面は、ページが最初に表示されるときの画面の外観を示します。 ここでは、`Label`しますか? 

問題は、 `Slider` 0 の初期値が含まれています。 これにより、`Scale`のプロパティ、`Label`にも、既定値は 1 をオーバーライドする 0 に設定します。 これは、結果、`Label`最初は表示されません。 操作できるように、Android およびユニバーサル Windows プラットフォーム (UWP) のスクリーン ショットで示されて、`Slider`させる、`Label`再度、表示が、その初期消滅は混乱を招く。

なることがわかります、[次の記事](binding-mode.md)を初期化することによってこの問題を回避する方法、`Slider`の既定値から、`Scale`プロパティです。

**代わりに XAML バインディング**ページは、XAML で等価なバインドを示しています。

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

今すぐ、`Binding`マークアップ拡張機能は 2 つのプロパティを設定すると、`Source`と`Path`コンマで区切って指定します。 たい場合は、同じ行に表示されることができます。

```xaml
Scale="{Binding Source={x:Reference slider}, Path=Value}" />
```

`Source`プロパティが設定される埋め込み`x:Reference`設定と同じ構文をそれ以外の場合はマークアップ拡張機能、`BindingContext`です。 中かっこ内に引用符が表示されないことと 2 つのプロパティをコンマで区切る必要があることに注意してください。

コンテンツ プロパティ、`Binding`マークアップ拡張機能が`Path`、ですが、`Path=`式の最初のプロパティである場合、マークアップ拡張機能の一部を除外だけことができます。 除去する、`Path=`パーツ、2 つのプロパティをスワップする必要があります。

```xaml
Scale="{Binding Value, Source={x:Reference slider}}" />
```

XAML マークアップ拡張機能は、中かっこで区切られた通常、オブジェクト要素として、表現できます。

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

これで、`Source`と`Path`プロパティは、XAML 属性の正規: 引用符で囲まれた値が表示され、属性は、コンマで区切られていません。 `x:Reference`マークアップ拡張機能はオブジェクト要素にもなります。

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

これまでに示した例の設定、`BindingContext`プロパティおよび`Source`プロパティの`Binding`を`x:Reference` ページで別のビューを参照するマークアップ拡張機能です。 これら 2 つのプロパティが型`Object`、バインド ソースに適したプロパティを含む任意のオブジェクトに設定することができます。 

今後の記事で紹介して設定できること、`BindingContext`または`Source`プロパティを`x:Static`の静的プロパティまたはフィールドの値を参照するマークアップ拡張機能または`StaticResource`に格納されているオブジェクトを参照するマークアップ拡張機能、リソース ディクショナリでは、直接オブジェクトには、通常 (必ずではありませんが)、または、ViewModel のインスタンス。 

`BindingContext`プロパティを設定することも、`Binding`オブジェクトできるように、`Source`と`Path`のプロパティ`Binding`バインディング コンテキストを定義します。

## <a name="binding-context-inheritance"></a>バインド コンテキストの継承

この記事で、オブジェクトを使用してソースを指定できることを確認した、`BindingContext`プロパティまたは`Source`のプロパティ、`Binding`オブジェクト。 どちらも設定されている場合、`Source`のプロパティ、`Binding`よりも優先、`BindingContext`です。

`BindingContext`プロパティが非常に重要な特性。

*設定、`BindingContext`ビジュアル ツリーを介したプロパティを継承します。*

後ほどこの可能性がありますバインディング式を簡略化するため、場合によっては非常に便利な&mdash;モデル View-viewmodel (MVVM) シナリオで特に&mdash;ことが不可欠です。

**バインド コンテキストの継承**サンプルは、バインド コンテキストの継承の簡単なデモ。

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

`BindingContext`のプロパティ、`StackLayout`に設定されている、`slider`オブジェクト。 両方でこのバインディング コンテキストを継承、`Label`と`BoxView`の両方をされて、`Rotation`プロパティに設定、`Value`のプロパティ、 `Slider`: 

[![バインド コンテキストの継承](basic-bindings-images/bindingcontextinheritance-small.png "バインド コンテキストの継承")](basic-bindings-images/bindingcontextinheritance-large.png#lightbox "コンテキストの継承のバインド")

[次の記事](binding-mode.md)、わかる方法、*バインド モード*ターゲットとソースのオブジェクト間のデータ フローを変更することができます。


## <a name="related-links"></a>関連リンク

- [データ バインディング デモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms 帳からのデータ バインディング章](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
