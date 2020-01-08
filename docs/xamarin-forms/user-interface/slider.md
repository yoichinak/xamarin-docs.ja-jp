---
title: Xamarin.Forms のスライダー
description: Xamarin.Forms、スライダーは、水平のバーから連続する範囲を double 型の値を選択するユーザーによって操作できます。 この記事では、スライダーのクラスを使用して、継続的な値の範囲から値を選択する方法について説明します。
ms.prod: xamarin
ms.assetid: 36B1C645-26E0-4874-B6B6-BDBF77662878
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/27/2019
ms.openlocfilehash: ddb25a1f01f91b627fc0c370f7f21e2a797b72cb
ms.sourcegitcommit: 191f1f3b13a14e2afadcb95126c5f653722f126f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/30/2019
ms.locfileid: "75545583"
---
# <a name="xamarinforms-slider"></a>Xamarin.Forms のスライダー

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos)

_継続的な値の範囲から選択するためには、スライダーを使用します。_

Xamarin.Forms [ `Slider` ](xref:Xamarin.Forms.Slider)水平のバーを選択する、ユーザーが操作できる、`double`を連続する範囲の値。

`Slider`型の 3 つのプロパティを定義します`double`:。

- [`Minimum`](xref:Xamarin.Forms.Slider.Minimum) 既定値が 0 で、範囲の最小値です。
- [`Maximum`](xref:Xamarin.Forms.Slider.Maximum) 既定値は 1 の範囲の最大となります。
- [`Value`](xref:Xamarin.Forms.Slider.Value) 間の範囲スライダーの値は、`Minimum`と`Maximum`あり、既定値は 0 です。

3 つすべてのプロパティが支え`BindableProperty`オブジェクト。 `Value`プロパティの既定のバインド モードは、 `BindingMode.TwoWay`、使用するアプリケーションでバインディング ソースとして適切であることを意味する、[モデル-ビュー-ビューモデル (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md)アーキテクチャ。

> [!WARNING]
> 内部的には、`Slider`により`Minimum`がより小さい`Maximum`します。 場合`Minimum`または`Maximum`がこれまでに設定できるように`Minimum`はより小さくない`Maximum`例外が発生します。 参照してください、 [**予防策**](#precautions)設定の詳細については、以下のセクション、`Minimum`と`Maximum`プロパティ。

`Slider`型に変換、`Value`プロパティ間なるように`Minimum`と`Maximum`までの値。 場合、`Minimum`プロパティがより大きい値に設定、`Value`プロパティ、`Slider`設定、`Value`プロパティを`Minimum`します。 同様に場合、`Maximum`値に設定されているより小さい`Value`、し`Slider`設定、`Value`プロパティを`Maximum`します。

`Slider` 定義、 [ `ValueChanged` ](xref:Xamarin.Forms.Slider.ValueChanged)ときに発生するイベント、`Value`いずれかのユーザー操作によって、変更、`Slider`プログラムを設定した場合、または、`Value`プロパティを直接。 A`ValueChanged`イベントがときに発生することはまた、`Value`プロパティは、前の段落で説明したように強制変換されます。

[ `ValueChangedEventArgs` ](xref:Xamarin.Forms.ValueChangedEventArgs)に付属しているオブジェクト、`ValueChanged`イベントには 2 つのプロパティが両方の種類の`double`: [ `OldValue` ](xref:Xamarin.Forms.ValueChangedEventArgs.OldValue)と[ `NewValue` ](xref:Xamarin.Forms.ValueChangedEventArgs.NewValue). 時にイベントが発生して、値の`NewValue`と同じ、`Value`のプロパティ、`Slider`オブジェクト。

`Slider` は、ドラッグ操作の開始時と終了時に発生する `DragStarted` および `DragCompleted` イベントも定義します。 [`ValueChanged`](xref:Xamarin.Forms.Slider.ValueChanged)イベントとは異なり、`DragStarted` イベントと `DragCompleted` イベントは、ユーザーが `Slider`を操作するだけで発生します。 `DragStarted` イベントが発生すると、`ICommand`型の `DragStartedCommand`が実行されます。 同様に、`DragCompleted` イベントが発生すると、`ICommand`型の `DragCompletedCommand`が実行されます。

> [!WARNING]
> 水平レイアウトの制約のないオプションを使用しない`Center`、 `Start`、または`End`で`Slider`します。 Android と、UWP の両方で、`Slider`バーの長さが 0、および ios では、バーに折りたたまれては非常に短いです。 既定値を保持`HorizontalOptions`設定`Fill`の幅を使用しないと`Auto`設定時に`Slider`で、`Grid`レイアウト。

`Slider`も外観に影響を与えるいくつかのプロパティを定義します。

- [`MinimumTrackColor`](xref:Xamarin.Forms.Slider.MinimumTrackColorProperty) バーのつまみの左側にある色です。
- [`MaximumTrackColor`](xref:Xamarin.Forms.Slider.MaximumTrackColorProperty) バーのつまみの右側にある色です。
- [`ThumbColor`](xref:Xamarin.Forms.Slider.ThumbColorProperty) つまみの色です。
- [`ThumbImageSource`](xref:Xamarin.Forms.Slider.ThumbImageSourceProperty) 一般的に、型に使用するイメージ[ `ImageSource`](xref:Xamarin.Forms.ImageSource)します。

> [!NOTE]
> `ThumbColor`と`ThumbImageSource`プロパティは相互に排他的です。 両方のプロパティが設定されている場合、`ThumbImageSource`プロパティが優先されます。

## <a name="basic-slider-code-and-markup"></a>スライダーの基本的なコードとマークアップ

[ **SliderDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos)サンプル機能的に同等ですが、さまざまな方法で実装されている 3 つのページから始まります。 最初のページは、c# コードを使用して、2 つ目は、コードでは、イベント ハンドラーで XAML を使用して、3 番目は、XAML ファイルでデータ バインディングを使用して、イベント ハンドラーを回避すること。

### <a name="creating-a-slider-in-code"></a>コードで、スライダーの作成

**基本的なスライダー コード**ページで、 [ **SliderDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos)サンプルを作成する表示を示しています、`Slider`と 2 つ`Label`コード内のオブジェクト。

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

`Slider`が初期化されて、 `Maximum` 360 のプロパティ。 `ValueChanged`のハンドラー、`Slider`を使用して、`Value`のプロパティ、`slider`を設定するオブジェクト、`Rotation`最初の`Label`を使用して、`String.Format`メソッドを`NewValue`のプロパティ、イベント引数を設定する、`Text`プロパティは、2 つ目の`Label`します。 現在の値を取得する 2 つの方法、`Slider`を交換できます。

IOS デバイスと Android デバイスで実行されているプログラムを次に示します。

[![基本スライダーコード](slider-images/BasicSliderCode.png "基本スライダーコード")](slider-images/BasicSliderCode-Large.png#lightbox)

2 番目の`Label`まで「(初期化されていない)」のテキストが表示されます、`Slider`操作は、これにより、最初`ValueChanged`イベントが。 表示される小数点以下桁数はプラットフォームごとに異なることに注意してください。 これらの違いに関連するプラットフォームの実装の`Slider`セクションでは、この記事の後半で説明[プラットフォームの実装の違い](#implementations)します。

### <a name="creating-a-slider-in-xaml"></a>XAML でスライダーの作成

**スライダーの基本的な XAML**ページは機能的に同じ**スライダーの基本的なコード**は実装が XAML でほとんどの場合。

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

イベント ハンドラーを取得することも、`Slider`を介してイベントを発生させるですが、`sender`引数。 `Value`プロパティには、現在の値が含まれています。

```csharp
double value = ((Slider)sender).Value;
```

場合、`Slider`オブジェクトが使用して、XAML ファイルの名前が付与された、`x:Name`属性 (たとえば、"slider")、イベント ハンドラーがそのオブジェクトを直接参照します。

```csharp
double value = slider.Value;
```

### <a name="data-binding-the-slider"></a>データ バインディング、スライダー

**スライダーの基本的なバインディング**ページは、ほぼ等価性を排除するプログラムを記述する方法を示しています、`Value`イベント ハンドラーを使用して[データ バインディングの](~/xamarin-forms/app-fundamentals/data-binding/index.md):

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

`Rotation`最初の`Label`にバインドされて、`Value`のプロパティ、`Slider`は、`Text`プロパティは、2 つ目の`Label`で、`StringFormat`仕様。 **スライダーの基本的なバインディング**ページ関数を少し異なる方法で 2 つの前のページから: ページが最初に表示される、2 番目の`Label`値を持つテキスト文字列が表示されます。 データ バインディングを使用すると便利です。 データ バインドせずにテキストを表示する、具体的には初期化する必要があるが、`Text`のプロパティ、`Label`またはの起動処理をシミュレートする、`ValueChanged`クラス コンス トラクターからイベント ハンドラーを呼び出すことによってイベント。

<a name="precautions" />

## <a name="precautions"></a>注意事項

値、`Minimum`プロパティの値より小さくする必要があります常に、`Maximum`プロパティ。 次のコード スニペットの原因、`Slider`例外が発生します。

```csharp
// Throws an exception!
Slider slider = new Slider
{
    Minimum = 10,
    Maximum = 20
};
```

C# コンパイラが順番に、これら 2 つのプロパティを設定するコードを生成とタイミング、 `Minimum` 10 に設定されて、既定値より大きい`Maximum`1 の値。 設定してここで、例外を回避することができます、`Maximum`プロパティ最初。

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

設定`Maximum`20 に問題には、既定値より大きいため、 `Minimum` 0 の値。 ときに`Minimum`が設定された場合、値がより小さい`Maximum`20 の値。

XAML で同じ問題が存在します。 確実な順序でプロパティを設定`Maximum`よりも大きいは常に`Minimum`:

```xaml
<Slider Maximum="20"
        Minimum="10" ... />
```

設定することができます、`Minimum`と`Maximum`値の順序でのみが、負の数値を`Minimum`は常により小さい`Maximum`:

```xaml
<Slider Minimum="-20"
        Maximum="-10" ... />
```

`Value`より大きいまたは等しい、プロパティは常に、`Minimum`値以下と等しい、`Maximum`します。 場合`Value`設定されている範囲外の値に値が、範囲でなければに強制変換されますが、例外は発生しません。 このコードは、たとえば、*いない*例外を発生させます。

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

ときに`Minimum`は 10 に設定し、`Value`も 10 に設定されます。

場合、`ValueChanged`時にイベント ハンドラーがアタッチされている、`Value`プロパティが 0 の場合の既定値以外に強制変換、`ValueChanged`イベントが発生します。 次の XAML スニペットに示します。

```xaml
<Slider ValueChanged="OnSliderValueChanged"
        Maximum="20"
        Minimum="10" />
```

ときに`Minimum`10 に設定されている`Value`10 に設定されても、`ValueChanged`イベントが発生します。 これは、エラーは、ページの残りの部分が作成されると、ハンドラーは、まだ作成されていない他の要素 ページを参照しよう前に発生する可能性があります。 いくつかのコードを追加したい場合があります、`ValueChanged`をチェックするハンドラー`null`ページ上の他の要素の値。 または、設定することができます、`ValueChanged`後のイベント ハンドラー、`Slider`値が初期化されています。

<a name="implementations" />

## <a name="platform-implementation-differences"></a>プラットフォームの実装の違い

値を表示する前に示したスクリーン ショット、 `Slider` 10 進数のポイントの数が異なる。 これは、方法に関係、 `Slider` Android、UWP プラットフォームで実装されます。

### <a name="the-android-implementation"></a>Android の実装

Android の実装の`Slider`は、Android に基づいて[ `SeekBar` ](xref:Android.Widget.SeekBar) 、常に設定し、 [ `Max` ](xref:Android.Widget.ProgressBar.Max) 1000 プロパティ。 つまり、 `Slider` android が 1,001 のみ個別の値。 設定した場合、`Slider`が、 `Minimum` 0 と`Maximum`5000、として、`Slider`操作は、`Value`プロパティが 0、5、10、15、およびその他の値。

### <a name="the-uwp-implementation"></a>UWP の実装

UWP 実装`Slider`は UWP に基づいて[ `Slider` ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.slider)コントロール。 `StepFrequency`プロパティ、UWP の`Slider`の差に設定されている、`Maximum`と`Minimum`プロパティが 10 日ですが 1 より大きくないで割った値します。

たとえば、既定の 0 ~ 1 の範囲、 `StepFrequency` 0.1 にプロパティを設定します。 として、`Slider`は、操作、`Value`プロパティは 0、0.1、0.2、0.3、0.4、0.5、0.6、0.7、0.8、0.9、または 1.0 に制限されます。 (これは、 [**SliderDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos)サンプルの最後のページで明らかになっています)。`Maximum` プロパティと `Minimum` プロパティの差が10以上の場合、`StepFrequency` は1に設定され、`Value` プロパティには整数値が含まれます。

### <a name="the-stepslider-solution"></a>StepSlider ソリューション

より汎用性のある `StepSlider` については、 [27 章を参照してください。](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf) *Xamarin. Forms を使用して Mobile Apps を作成する*ブックのカスタムレンダラー。 `StepSlider`のような`Slider`が追加されて、`Steps`までの値の数を指定するプロパティ`Minimum`と`Maximum`。

## <a name="sliders-for-color-selection"></a>スライダーの色の選択

最終的な 2 つのページ、 [ **SliderDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos)両方を使用して 3 つのサンプル`Slider`色の選択のインスタンス。 最初のページは、2 番目のページは、ViewModel でデータ バインディングを使用する方法を示しています。 中に、分離コード ファイル内のすべての対話を処理します。

### <a name="handling-sliders-in-the-code-behind-file"></a>スライダーを分離コード ファイルの処理

**RGB カラー スライダー**ページをインスタンス化、 `BoxView` 、色を表示する 3 つ`Slider`色、および 3 の赤、緑、および青のコンポーネントを選択するインスタンス`Label`これらの色を表示するための要素値:

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

A`Style`では 3 つすべて`Slider`0 ~ 255 の範囲の要素。 `Slider`要素が同じ共有`ValueChanged`ハンドラーで、分離コード ファイルに実装されます。

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

最初のセクションの設定、`Text`のいずれかのプロパティ、`Label`インスタンスの値を示す短いテキスト文字列を`Slider`16 進数。 3 つは、すべて`Slider`のインスタンスを作成するアクセスされる、 `Color` RGB コンポーネントからの値。

[![RGB カラースライダー](slider-images/RgbColorSliders.png "RGB カラースライダー")](slider-images/RgbColorSliders-Large.png#lightbox)

### <a name="binding-the-slider-to-a-viewmodel"></a>スライダーを ViewModel にバインドします。

**HSL カラー スライダー**ページは、ViewModel を使用して作成するために使用する計算を実行する方法を示しています、`Color`色合い、鮮やかさ、および明るさの値から値。 などのすべての Viewmodel、`HSLColorViewModel`クラスが実装する、`INotifyPropertyChanged`インターフェイス、および発生、`PropertyChanged`プロパティの 1 つが変更されるたびにイベント。

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

Viewmodel と`INotifyPropertyChanged`インターフェイスが、情報の記事で説明した[データ バインディングの](~/xamarin-forms/app-fundamentals/data-binding/index.md)します。

**HslColorSlidersPage.xaml**ファイルをインスタンス化、`HslColorViewModel`にページの設定と`BindingContext`プロパティ。 これにより、ビューモデルのプロパティにバインドする XAML ファイル内のすべての要素。

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

として、`Slider`要素を操作すると、`BoxView`と`Label`の ViewModel から要素が更新されます。

[![HSL の色スライダー](slider-images/HslColorSliders.png "HSL の色スライダー")](slider-images/HslColorSliders-Large.png#lightbox)

`StringFormat`のコンポーネントである、`Binding`マークアップ拡張機能は 2 つの小数点以下桁数を表示する"F2"の形式を設定します。 (データバインディングでの文字列の書式設定については、「[文字列の書式設定](~/xamarin-forms/app-fundamentals/data-binding/string-formatting.md)」で説明しています)。ただし、プログラムの UWP バージョンは、0、0.1、0.2、...0.9 および1.0。 これは、UWP の実装の直接的な結果`Slider`でセクションの説明に従って[プラットフォームの実装の違い](#implementations)します。

## <a name="related-links"></a>関連リンク

- [スライダーのデモ サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos)
- [スライダー API](xref:Xamarin.Forms.Slider)
