---
title: スライダーを使用します。
description: 連続値の範囲から選択する場合、スライダーを使用します。
ms.prod: xamarin
ms.assetid: 36B1C645-26E0-4874-B6B6-BDBF77662878
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/16/2018
ms.openlocfilehash: 99109f6377037ffb9f622b7ddb237b42d241e505
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="using-slider"></a>スライダーを使用します。

_連続値の範囲から選択する場合、スライダーを使用します。_

Xamarin.Forms [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/)水平のバーを選択する、ユーザーが操作できる、`double`連続した範囲からの値。 

`Slider`型の 3 つのプロパティを定義`double`:

- [`Minimum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Minimum/) 既定値は 0、範囲の最小です。
- [`Maximum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Maximum/) 既定値 1 の範囲の最大です。
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Value/) 間の範囲は、スライダーの値は、`Minimum`と`Maximum`あり、既定値は 0 です。

3 つすべてのプロパティが裏付け`BindableProperty`オブジェクト。 `Value`プロパティがの既定のバインド モード`BindingMode.TwoWay`、使用するアプリケーションでバインディング ソースとして適切であることを意味する、[モデル View-viewmodel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md)アーキテクチャ。 

> [!WARNING]
> 内部的には、`Slider`確実`Minimum`はより小さい`Maximum`です。 場合`Minimum`または`Maximum`これまでに設定できるように`Minimum`はより小さくない`Maximum`例外が発生します。 参照してください、 [**予防措置**](#precautions)設定の詳細については、後述の「、`Minimum`と`Maximum`プロパティです。

`Slider`型に変換、`Value`プロパティ間であること、`Minimum`と`Maximum`、包括的です。 場合、`Minimum`プロパティがより大きい値に設定、 `Value` 、プロパティ、`Slider`設定、`Value`プロパティを`Minimum`です。 同様に場合、`Maximum`値に設定されているより小さい`Value`、し`Slider`設定、`Value`プロパティを`Maximum`です。 

`Slider` 定義、 [ `ValueChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Slider.ValueChanged/)ときに発生するイベント、`Value`いずれかのユーザー操作によって変更、`Slider`プログラムが設定した場合や、`Value`プロパティを直接です。 A`ValueChanged`イベントが発生時にも、`Value`プロパティは、前の段落で説明したように強制変換されます。 

[ `ValueChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ValueChangedEventArgs/)に付属しているオブジェクト、`ValueChanged`イベントの種類の両方の 2 つのプロパティには、 `double`: [ `OldValue` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ValueChangedEventArgs.OldValue/)と[ `NewValue` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ValueChangedEventArgs.NewValue/). 時に、イベントが発生したの値`NewValue`と同じ、`Value`のプロパティ、`Slider`オブジェクト。

> [!WARNING]
> 制約なしの水平レイアウト オプションを使用しない`Center`、 `Start`、または`End`で`Slider`です。 Android と UWP の両方で、`Slider`が折りたたまれて、長さが 0 と ios の場合、バーのバーには非常に短い時間です。 既定値を保持`HorizontalOptions`の設定`Fill`、しの幅を使用しないように`Auto`設定時に`Slider`で、`Grid`レイアウトです。

## <a name="basic-slider-code-and-markup"></a>スライダーの基本的なコードとマークアップ

[ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos)サンプル機能的に同等ですが、さまざまな方法で実装されている 3 つのページから始まります。 最初のページのみの c# コードを使用して、2 つ目はコードでは、イベント ハンドラーと XAML を使用および 3 番目は XAML ファイルでデータ バインディングを使用して、イベント ハンドラーを回避することです。

### <a name="creating-a-slider-in-code"></a>コードで、スライダーの作成

**基本的なスライダー コード** ページで、 [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos)サンプルを作成する表示を示しています、`Slider`と 2 つ`Label`コード内のオブジェクト。

```csharp
public class BasicSliderCodePage : ContentPage
{
    public BasicSliderCodePage()
    {
        Label rotationLabel = new Label
        {
            Text = "ROTATING TEXT",
            FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)),
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        Label displayLabel = new Label
        {
            Text = "(uninitialized)",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        Slider slider = new Slider
        {
            Maximum = 360
        };
        slider.ValueChanged += (sender, args) =>
        {
            rotationLabel.Rotation = slider.Value;
            displayLabel.Text = String.Format("The Slider value is {0}", args.NewValue);
        };

        Title = "Basic Slider Code";
        Padding = new Thickness(10, 0);
        Content = new StackLayout
        {
            Children =
            {
                rotationLabel,
                slider,
                displayLabel
            }
        };
    }
}
```

`Slider`が初期化されて、 `Maximum` 360 のプロパティです。 `ValueChanged`のハンドラー、`Slider`を使用して、`Value`のプロパティ、`slider`を設定するオブジェクト、`Rotation`最初の`Label`を使用して、`String.Format`メソッドを`NewValue`のプロパティ、イベント引数を設定する、 `Text` 2 番目のプロパティ`Label`です。 現在の値を取得する 2 つの方法、`Slider`は互換性があります。 

デバイスを iOS、Android、およびユニバーサル Windows プラットフォーム (UWP) で実行されているプログラムを次に示します。

[![基本的なスライダー コード](slider-images/BasicSliderCode.png "基本的なスライダー コード")](slider-images/BasicSliderCode-Large.png#lightbox)

2 番目`Label`まで「(初期化されていない)」のテキストを表示、`Slider`操作は、最初のケースを`ValueChanged`イベントが発生します。 表示される小数点以下桁数が 3 つのプラットフォームの異なることに注意してください。 これらの相違点は、実装に関連する、プラットフォームの`Slider`セクションでは、この記事の後半で説明して[プラットフォームの実装の違い](#implementations)です。

### <a name="creating-a-slider-in-xaml"></a>XAML でスライダーの作成

**スライダーの基本的な XAML**はページが機能的には、同じ**基本的なスライダー コード**は実装が XAML でほとんどの場合。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SliderDemos.BasicSliderXamlPage"
             Title="Basic Slider XAML"
             Padding="10, 0">
    <StackLayout>
        <Label x:Name="rotatingLabel" 
               Text="ROTATING TEXT"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider Maximum="360"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="displayLabel"
               Text="(uninitialized)"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

分離コード ファイルにはハンドラーが含まれています、`ValueChanged`イベント。

```csharp
public partial class BasicSliderXamlPage : ContentPage
{
    public BasicSliderXamlPage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        double value = args.NewValue;
        rotatingLabel.Rotation = value;
        displayLabel.Text = String.Format("The Slider value is {0}", value);
    }
}
```

イベント ハンドラーを取得することも、`Slider`を通じてイベントを発生させているを`sender`引数。 `Value`プロパティには、現在の値が含まれています。

```csharp
double value = ((Slider)sender).Value;
```

場合、`Slider`オブジェクトの XAML ファイルの名前が指定された、`x:Name`属性 (たとえば、"slider")、イベント ハンドラーがそのオブジェクトを直接参照します。

```csharp
double value = slider.Value;
```

### <a name="data-binding-the-slider"></a>データ バインディング、スライダー

**スライダーの基本的なバインディング**ページはほぼ同等性を排除するプログラムを記述する方法を示します、`Value`イベント ハンドラーを使用して[データ バインディングの](~/xamarin-forms/app-fundamentals/data-binding/index.md):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SliderDemos.BasicSliderBindingsPage"
             Title="Basic Slider Bindings"
             Padding="10, 0">
    <StackLayout>
        <Label Text="ROTATING TEXT"
               Rotation="{Binding Source={x:Reference slider}, 
                                  Path=Value}"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Maximum="360" />

        <Label x:Name="displayLabel"
               Text="{Binding Source={x:Reference slider}, 
                              Path=Value, 
                              StringFormat='The Slider value is {0:F0}'}"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

`Rotation`最初の`Label`にバインドされて、`Value`のプロパティ、`Slider`をそのまま、 `Text` 2 番目のプロパティ`Label`で、`StringFormat`仕様です。 **スライダーの基本的なバインディング**ページ関数は少し異なる方法で、次の 2 つ前のページから: ページが最初に表示される、2 番目`Label`値を持つテキスト文字列が表示されます。 これは、データ バインディングを使用すると便利です。 具体的には初期化するために必要データ バインドせずにテキストを表示する、`Text`のプロパティ、`Label`の発生をシミュレートまたは、`ValueChanged`クラス コンス トラクターからイベント ハンドラーを呼び出すことによってイベント。

<a name="precautions" />

## <a name="precautions"></a>注意事項

値、`Minimum`プロパティの値より小さくする必要があります常に、`Maximum`プロパティです。 次のコード スニペットの原因として、`Slider`から例外が発生します。

```csharp
// Throws an exception!
Slider slider = new Slider
{
    Minimum = 10,
    Maximum = 20
};
```

C# コンパイラが順番に、これら 2 つのプロパティを設定するコードを生成、いつ、 `Minimum` 10 に設定されて、既定値より大きい`Maximum`1 の値。 設定してここで例外を回避することができます、`Maximum`プロパティ最初。

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

設定`Maximum`20 に問題はありませんが、既定値より大きいため`Minimum`0 を設定します。 ときに`Minimum`が設定されている、値より小さい`Maximum`20 の値。

XAML で、同じ問題が存在します。 確実にする順序でプロパティを設定`Maximum`よりも大きいは常に`Minimum`:

```xaml
<Slider Maximum="20"
        Minimum="10" ... />
```

設定することができます、`Minimum`と`Maximum`値、負の数値には、順序でのみ、`Minimum`は常により小さい`Maximum`:

```xaml
<Slider Minimum="-20"
        Maximum="-10" ... />
```

`Value`以上に、プロパティは常に、`Minimum`値に等しいまたはそれよりも小さいと`Maximum`です。 場合`Value`設定が範囲外の値に値が、範囲内でに強制変換されますが、例外は発生しません。 このコードは、たとえば、*いない*で例外が発生します。

```csharp
Slider slider = new Slider
{
    Value = 10
};
```

代わりに、`Value`プロパティに強制変換、 `Maximum` 1 の値。

上記のコード スニペットを次に示します。 

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

ときに`Minimum`に設定されている、10、`Value`も 10 に設定します。 

場合、`ValueChanged`時にイベント ハンドラーがアタッチされている、`Value`プロパティが 0 の場合の既定値以外に強制変換、`ValueChanged`イベントが発生します。 XAML のスニペットを次に示します。 

```xaml
<Slider ValueChanged="OnSliderValueChanged"
        Maximum="20"
        Minimum="10" />
```

ときに`Minimum`を 10 に設定されている`Value`を 10 に設定されても、`ValueChanged`イベントが発生します。 これは、ページの残りの部分を構築すると、およびハンドラーは可能性があります ページで、まだ作成されていないその他の要素を参照しようとします。 前に発生する可能性があります。 一部のコードを追加する、`ValueChanged`をチェックするハンドラー`null`ページ上の他の要素の値。 または、設定することができます、`ValueChanged`後のイベント ハンドラー、`Slider`値が初期化されています。

<a name="implementations" />

## <a name="platform-implementation-differences"></a>プラットフォームの実装の違い

値を表示する前に示したスクリーン ショット、 `Slider` 10 進数のポイントの数が異なる。 これは、方法に関係の`Slider`Android と UWP プラットフォーム上に実装されています。

### <a name="the-android-implementation"></a>Android の実装 

Android 実装`Slider`は、Android に基づいて[ `SeekBar` ](https://developer.xamarin.com/api/type/Android.Widget.SeekBar/)常に設定し、 [ `Max` ](https://developer.xamarin.com/api/property/Android.Widget.ProgressBar.Max/) 1000 プロパティです。 つまり、 `Slider` Android でのみ 1,001 不連続の値を持ちます。 設定した場合、`Slider`が、 `Minimum` 0 のおよび`Maximum`5000、として、`Slider`操作は、`Value`プロパティが 0、5、10、15 などの値。 

### <a name="the-uwp-implementation"></a>UWP 実装では、

UWP 実装`Slider`UWP に基づきます[ `Slider` ](/uwp/api/windows.ui.xaml.controls.slider)コントロール。 `StepFrequency`の UWP プロパティ`Slider`の差に設定されている、`Maximum`と`Minimum`プロパティが 10 日ですが 1 より大きくないで割った値します。 

たとえば、0 ~ 1 の既定の範囲、 `StepFrequency` 0.1 にプロパティを設定します。 として、`Slider`操作は、`Value`プロパティは 0、0.1、0.2、0.3、0.4、0.5、0.6、0.7、0.8、0.9、および 1.0 に制限します。 (これは、最後のページで、 [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos)サンプルです)。ときの違い、`Maximum`と`Minimum`プロパティは、10 以上、`StepFrequency`を 1 に設定されていると`Value`プロパティが整数値。 

### <a name="the-stepslider-solution"></a>StepSlider ソリューション

汎用性`StepSlider`は、後ほど[章 27。カスタム レンダラー](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf)書籍の*Xamarin.Forms を使用したモバイル アプリを作成する*です。 `StepSlider`に似ていますが`Slider`が追加、`Steps`間の値の数を指定するプロパティ`Minimum`と`Maximum`です。

## <a name="sliders-for-color-selection"></a>色の選択 のスライダー

最後の 2 つのページ、 [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos)どちらも使用する 3 つのサンプル`Slider`の色の選択用のインスタンス。 最初のページでは、分離コード ファイル内のすべての対話を処理し、2 番目のページで、ViewModel データ バインディングを使用する方法を示しています。 

### <a name="handling-sliders-in-the-code-behind-file"></a>分離コード ファイルでスライダーの処理 

**RGB 色スライダー**ページをインスタンス化、 `BoxView` 、色を表示する次の 3 つ`Slider`、色、および 3 の赤、緑、および青のコンポーネントを選択するインスタンス`Label`それらの色を表示するための要素値:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SliderDemos.RgbColorSlidersPage"
             Title="RGB Color Sliders">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Slider">
                <Setter Property="Maximum" Value="255" />
            </Style>
            
            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10">
        <BoxView x:Name="boxView"
                 Color="Black"
                 VerticalOptions="FillAndExpand" />

        <Slider x:Name="redSlider"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="redLabel" />

        <Slider x:Name="greenSlider" 
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="greenLabel" />

        <Slider x:Name="blueSlider" 
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="blueLabel" />
    </StackLayout>
</ContentPage>
```

A `Style` 3 つすべては、 `Slider` 0 ~ 255 の範囲を要素。 `Slider`要素が同じ`ValueChanged`分離コード ファイルで実装されるハンドラー。

```csharp
public partial class RgbColorSlidersPage : ContentPage
{
    public RgbColorSlidersPage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (sender == redSlider)
        {
            redLabel.Text = String.Format("Red = {0:X2}", (int)args.NewValue);
        }
        else if (sender == greenSlider)
        {
            greenLabel.Text = String.Format("Green = {0:X2}", (int)args.NewValue);
        }
        else if (sender == blueSlider)
        {
            blueLabel.Text = String.Format("Blue = {0:X2}", (int)args.NewValue);
        }

        boxView.Color = Color.FromRgb((int)redSlider.Value,
                                      (int)greenSlider.Value,
                                      (int)blueSlider.Value);
    }
}
```

最初のセクションを設定、`Text`のいずれかのプロパティ、`Label`インスタンスの値を示す短いテキスト文字列を`Slider`16 進数。 3 つを次に、すべて`Slider`インスタンスが作成するためにアクセスは、 `Color` RGB コンポーネントからの値。

[![RGB 色スライダー](slider-images/RgbColorSliders.png "RGB 色スライダー")](slider-images/RgbColorSliders-Large.png#lightbox)

### <a name="binding-the-slider-to-a-viewmodel"></a>スライダーを ViewModel にバインドします。

**HSL の色スライダー**ページ、ViewModel を使用して作成するために使用する計算を実行する方法を示しています、`Color`色合い、鮮やかさ、明るさの値からの値。 すべての ViewModels と同様に、`HSLColorViewModel`クラスが実装する、`INotifyPropertyChanged`インターフェイス、および発生、`PropertyChanged`イベント プロパティの 1 つが変更されるたびに。

```csharp
public class HslColorViewModel : INotifyPropertyChanged
{
    Color color;

    public event PropertyChangedEventHandler PropertyChanged;

    public double Hue
    {
        set
        {
            if (color.Hue != value)
            {
                Color = Color.FromHsla(value, color.Saturation, color.Luminosity);
            }
        }
        get 
        {
            return color.Hue;
        }
    }

    public double Saturation
    {
        set
        {
            if (color.Saturation != value)
            {
                Color = Color.FromHsla(color.Hue, value, color.Luminosity);
            }
        }
        get
        {
            return color.Saturation;
        }
    }

    public double Luminosity
    {
        set
        {
            if (color.Luminosity != value)
            {
                Color = Color.FromHsla(color.Hue, color.Saturation, value);
            }
        }
        get
        {
            return color.Luminosity;
        }
    }

    public Color Color
    {
        set
        {
            if (color != value)
            {
                color = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Hue"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Saturation"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Luminosity"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Color"));
            }
        }
        get
        {
            return color;
        }
    }
}
```

ViewModels と`INotifyPropertyChanged`インターフェイスは、記事で説明した[データ バインド](~/xamarin-forms/app-fundamentals/data-binding/index.md)です。

**HslColorSlidersPage.xaml**ファイルをインスタンス化、`HslColorViewModel`をページの設定と`BindingContext`プロパティです。 これにより、ViewModel でプロパティにバインドする XAML ファイル内のすべての要素です。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:SliderDemos"
             x:Class="SliderDemos.HslColorSlidersPage"
             Title="HSL Color Sliders">

    <ContentPage.BindingContext>
        <local:HslColorViewModel Color="Chocolate" />
    </ContentPage.BindingContext>

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10">
        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <Slider Value="{Binding Hue}" />
        <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}" />

        <Slider Value="{Binding Saturation}" />
        <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}" />

        <Slider Value="{Binding Luminosity}" />
        <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}" />
    </StackLayout>
</ContentPage> 
```

として、`Slider`要素を操作すると、`BoxView`と`Label`ViewModel から要素が更新されます。

[![HSL の色スライダー](slider-images/HslColorSliders.png "HSL の色スライダー")](slider-images/HslColorSliders-Large.png#lightbox)

`StringFormat`コンポーネント、`Binding`小数点以下 2 桁を表示する"F2"の形式のマークアップ拡張機能を設定します。 (データ バインディングの書式設定文字列が、記事で説明した[文字列の書式設定](~/xamarin-forms/app-fundamentals/data-binding/string-formatting.md))。ただし、UWP バージョンのプログラムは、0、0.1、0.2 の値に制限しています.0.9、または 1.0 です。 これは、UWP の実装の直接の結果`Slider`セクションで説明した[プラットフォームの実装の違い](#implementations)です。

## <a name="related-links"></a>関連リンク

- [スライダーのデモ サンプル](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos)
- [スライダー API](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/)
