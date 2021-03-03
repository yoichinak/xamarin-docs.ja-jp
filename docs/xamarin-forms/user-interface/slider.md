---
title: Xamarin.Forms Slider
description: スライダーは、連続した Xamarin.Forms 範囲から倍精度浮動小数点値を選択するためにユーザーが操作できる水平バーです。 この記事では、スライダークラスを使用して連続値の範囲から値を選択する方法について説明します。
ms.prod: xamarin
ms.assetid: 36B1C645-26E0-4874-B6B6-BDBF77662878
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/27/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 364cda6372986113e8a782a061783e0ca5455f3b
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93368545"
---
# <a name="xamarinforms-slider"></a>Xamarin.Forms Slider

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos)

_スライダーを使用して、連続する値の範囲から選択します。_

は、連続した Xamarin.Forms [`Slider`](xref:Xamarin.Forms.Slider) 範囲から値を選択するためにユーザーが操作できる水平バーです `double` 。

は、 `Slider` 型の次の3つのプロパティを定義し `double` ます。

- [`Minimum`](xref:Xamarin.Forms.Slider.Minimum) 範囲の最小値を指定します。既定値は0です。
- [`Maximum`](xref:Xamarin.Forms.Slider.Maximum) 範囲の最大値を指定します。既定値は1です。
- [`Value`](xref:Xamarin.Forms.Slider.Value) は、スライダーの値です。この値は、との間の範囲で、 `Minimum` `Maximum` 既定値は0です。

3つのプロパティはすべて、オブジェクトによってバックアップされ `BindableProperty` ます。 `Value`プロパティには、の既定のバインディングモードがあります。これは、 `BindingMode.TwoWay` [モデルビュービューモデル (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md)アーキテクチャを使用するアプリケーションでバインディングソースとして適切であることを意味します。

> [!WARNING]
> 内部的には、 `Slider` `Minimum` はよりも小さいことを保証し `Maximum` ます。 またはが `Minimum` `Maximum` より小さい値に設定されている場合は `Minimum` `Maximum` 、例外が発生します。 プロパティとプロパティの設定の詳細については、以下の「 [**予防策**](#precautions) 」セクションを参照してください `Minimum` `Maximum` 。

は、 `Slider` `Value` との間であるように、プロパティを強制し `Minimum` `Maximum` ます。 プロパティ `Minimum` がプロパティよりも大きい値に設定されている場合、 `Value` は `Slider` `Value` プロパティをに設定し `Minimum` ます。 同様に、 `Maximum` がよりも小さい値に設定されている場合は、 `Value` `Slider` プロパティをに設定し `Value` `Maximum` ます。

`Slider` の [`ValueChanged`](xref:Xamarin.Forms.Slider.ValueChanged) `Value` ユーザー操作によって、 `Slider` またはプログラムによってプロパティが直接設定されたときに、が変更されたときに発生するイベントを定義し `Value` ます。 `ValueChanged` `Value` 前の段落で説明したように、プロパティが強制変換された場合にもイベントが発生します。

[`ValueChangedEventArgs`](xref:Xamarin.Forms.ValueChangedEventArgs)イベントに付随するオブジェクトに `ValueChanged` は、型と型の2つのプロパティがあり `double` [`OldValue`](xref:Xamarin.Forms.ValueChangedEventArgs.OldValue) [`NewValue`](xref:Xamarin.Forms.ValueChangedEventArgs.NewValue) ます。 イベントが発生した時点で、の値 `NewValue` は `Value` オブジェクトのプロパティと同じです `Slider` 。

`Slider` は、 `DragStarted` `DragCompleted` ドラッグ操作の開始時と終了時に発生するイベントとイベントも定義します。 イベントとは異なり、イベント [`ValueChanged`](xref:Xamarin.Forms.Slider.ValueChanged) `DragStarted` と `DragCompleted` イベントは、ユーザー操作によってのみ発生し `Slider` ます。 イベントが `DragStarted` 発生すると、 `DragStartedCommand` 型のが `ICommand` 実行されます。 同様に、イベントが発生すると、 `DragCompleted` `DragCompletedCommand` 型のが `ICommand` 実行されます。

> [!WARNING]
> `Center`、 `Start` 、またはでは、制約のない水平レイアウトオプションを使用しないで `End` `Slider` ください。 Android と UWP の両方で、は `Slider` 長さが0のバーに折りたたまれ、iOS ではバーが非常に短くなっています。 の既定の `HorizontalOptions` 設定をそのまま `Fill` 使用して、レイアウトを配置するときにの幅を使用しないようにし `Auto` `Slider` `Grid` ます。

では、 `Slider` 外観に影響を与えるいくつかのプロパティも定義されています。

- [`MinimumTrackColor`](xref:Xamarin.Forms.Slider.MinimumTrackColorProperty) は、つまみの左側のバーの色です。
- [`MaximumTrackColor`](xref:Xamarin.Forms.Slider.MaximumTrackColorProperty) は、つまみの右側のバーの色です。
- [`ThumbColor`](xref:Xamarin.Forms.Slider.ThumbColorProperty) は、thumb の色です。
- [`ThumbImageSource`](xref:Xamarin.Forms.Slider.ThumbImageSourceProperty) は、型の thumb に使用するイメージです [`ImageSource`](xref:Xamarin.Forms.ImageSource) 。

> [!NOTE]
> `ThumbColor`プロパティと `ThumbImageSource` プロパティは相互に排他的です。 両方のプロパティが設定されている場合は、 `ThumbImageSource` プロパティが優先されます。

## <a name="basic-slider-code-and-markup"></a>基本スライダーコードとマークアップ

[**SliderDemos**](/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos)サンプルは、機能的には異なる3つのページから始まりますが、さまざまな方法で実装されています。 最初のページでは C# コードのみを使用し、2番目のページでは XAML をコード内のイベントハンドラーと共に使用します。3番目のページでは、XAML ファイルのデータバインディングを使用してイベントハンドラーを回避できます。

### <a name="creating-a-slider-in-code"></a>コードでのスライダーの作成

[**SliderDemos**](/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos)サンプルの **基本的なスライダーコード** ページでは、 `Slider` コード内にとの2つのオブジェクトを作成する方法を示してい `Label` ます。

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

は、 `Slider` 360 のプロパティを持つように初期化され `Maximum` ます。 `ValueChanged`のハンドラーは、 `Slider` `Value` オブジェクトのプロパティを使用し `slider` `Rotation` て最初ののプロパティを設定 `Label` し、メソッドを `String.Format` イベント引数のプロパティと共に使用して `NewValue` `Text` 、2番目のプロパティを設定し `Label` ます。 の現在の値を取得するための2つの方法 `Slider` は、交換可能です。

IOS デバイスと Android デバイスで実行されているプログラムを次に示します。

[![基本スライダーコード](slider-images/BasicSliderCode.png "基本スライダーコード")](slider-images/BasicSliderCode-Large.png#lightbox)

2番目のは `Label` 、が操作されるまで "(初期化されていません)" というテキストを表示し `Slider` ます。これにより、最初の `ValueChanged` イベントが発生します。 表示される小数点以下の桁数は、プラットフォームごとに異なります。 これらの違いは、のプラットフォーム実装に関連 `Slider` しています。この点については、この記事で後述する「 [プラットフォームの実装の相違点](#platform-implementation-differences)」を参照してください。

### <a name="creating-a-slider-in-xaml"></a>XAML でのスライダーの作成

**基本的なスライダーの xaml** ページは、**基本的なスライダーコード** と同じですが、主に xaml で実装されています。

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

分離コードファイルには、イベントのハンドラーが含まれてい `ValueChanged` ます。

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

また、イベントハンドラーは、引数を使用してイベントを発生させるを取得することもでき `Slider` `sender` ます。 プロパティには `Value` 、現在の値が含まれます。

```csharp
double value = ((Slider)sender).Value;
```

オブジェクトに `Slider` 属性を持つ XAML ファイル内の名前が指定されている場合 `x:Name` (たとえば、"slider")、イベントハンドラーはそのオブジェクトを直接参照できます。

```csharp
double value = slider.Value;
```

### <a name="data-binding-the-slider"></a>スライダーのデータバインディング

[ **基本スライダーのバインド** ] ページには、 `Value` [データバインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)を使用してイベントハンドラーを削除するほぼ同等のプログラムを作成する方法が示されています。

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

最初ののプロパティはのプロパティ `Rotation` `Label` にバインドされ `Value` `Slider` ます。これは、 `Text` 2 番目のプロパティが `Label` 仕様であるためです `StringFormat` 。 **基本スライダーのバインド** ページは、前の2つのページとは少し異なる方法で機能します。ページが最初に表示されたとき、2番目のページに `Label` 値が表示されます。 これは、データバインディングを使用する利点です。 データバインディングなしでテキストを表示するには、のプロパティを明示的に初期化する `Text` `Label` か、 `ValueChanged` クラスコンストラクターからイベントハンドラーを呼び出すことによってイベントの発生をシミュレートする必要があります。

## <a name="precautions"></a>予防

プロパティの値は、 `Minimum` 常にプロパティの値よりも小さくする必要があり `Maximum` ます。 次のコードスニペットでは、によって `Slider` 例外が発生します。

```csharp
// Throws an exception!
Slider slider = new Slider
{
    Minimum = 10,
    Maximum = 20
};
```

C# コンパイラは、これら2つのプロパティを順番に設定するコードを生成し `Minimum` ます。また、プロパティが10に設定されている場合は、既定値の1よりも大きい値になり `Maximum` ます。 この場合、この例外を回避するには、最初にプロパティを設定し `Maximum` ます。

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

`Maximum`を20に設定することは、既定値の0よりも大きいため、問題にはなりません `Minimum` 。 `Minimum`が設定されている場合、値は20より小さい値になり `Maximum` ます。

XAML にも同じ問題があります。 が常により大きいことを保証する順序でプロパティを設定し `Maximum` `Minimum` ます。

```xaml
<Slider Maximum="20"
        Minimum="10" ... />
```

との値は、負の数値に設定できますが、 `Minimum` `Maximum` `Minimum` が常により小さい順序でのみ使用でき `Maximum` ます。

```xaml
<Slider Minimum="-20"
        Maximum="-10" ... />
```

プロパティは、 `Value` 常に値以上で、以下の値以上 `Minimum` `Maximum` です。 `Value`がその範囲外の値に設定されている場合、値はその範囲内に強制的に変換されますが、例外は発生しません。 たとえば、次のコードでは例外が発生し *ません* 。

```csharp
Slider slider = new Slider
{
    Value = 10
};
```

代わりに、 `Value` プロパティは強制的に値1に変換され `Maximum` ます。

上記のコードスニペットを次に示します。

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

を `Minimum` 10 に設定すると、 `Value` も10に設定されます。

`ValueChanged` `Value` プロパティが既定値の0以外に強制的に変換されるときにイベントハンドラーがアタッチされている場合は、 `ValueChanged` イベントが発生します。 XAML のスニペットを次に示します。

```xaml
<Slider ValueChanged="OnSliderValueChanged"
        Maximum="20"
        Minimum="10" />
```

を `Minimum` 10 に設定すると、 `Value` も10に設定され、 `ValueChanged` イベントが発生します。 これは、ページの残りの部分が構築される前に発生し、ハンドラーは、まだ作成されていないページ上の他の要素を参照しようとする場合があります。 `ValueChanged` `null` ページ上の他の要素の値を確認するコードをハンドラーに追加することができます。 または、 `ValueChanged` `Slider` 値が初期化された後にイベントハンドラーを設定できます。

## <a name="platform-implementation-differences"></a>プラットフォームの実装の違い

前に示したスクリーンショットには、の値と、 `Slider` 小数点の数が異なります。 これは、を `Slider` Android および UWP プラットフォームに実装する方法に関連しています。

### <a name="the-android-implementation"></a>Android の実装

の Android 実装 `Slider` は android に基づい [`SeekBar`](xref:Android.Widget.SeekBar) ており、プロパティは常に1000に設定され [`Max`](xref:Android.Widget.ProgressBar.Max) ます。 つまり、Android のは、 `Slider` 1001 の不連続値のみを持ちます。 を0に設定し、を5000に設定した場合、 `Slider` `Minimum` `Maximum` `Slider` が操作されると、 `Value` プロパティの値は0、5、10、15のようになります。

### <a name="the-uwp-implementation"></a>UWP 実装

の UWP 実装 `Slider` は、uwp コントロールに基づいてい [`Slider`](/uwp/api/windows.ui.xaml.controls.slider) ます。 `StepFrequency`UWP のプロパティは、 `Slider` プロパティとプロパティの差を10で割った値に設定されていますが、1を超えて `Maximum` `Minimum` いません。

たとえば、0 ~ 1 の既定の範囲で `StepFrequency` は、プロパティは0.1 に設定されます。 `Slider`が操作されると、 `Value` プロパティは0、0.1、0.2、0.3、0.4、0.5、0.6、0.7、0.8、0.9、および1.0 に制限されます。 (これは、 [**SliderDemos**](/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos) サンプルの最後のページで明らかになっています)。 `Maximum` プロパティとプロパティの差 `Minimum` が10以上の場合、 `StepFrequency` は1に設定され、プロパティには `Value` 整数値が含まれます。

### <a name="the-stepslider-solution"></a>StepSlider ソリューション

詳細について `StepSlider` は、27章で説明されてい [ます。](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf)*で Mobile Apps を Xamarin.Forms 作成する* ブックのカスタムレンダラー。 は `StepSlider` に似てい `Slider` ますが、 `Steps` との間の値の数を指定するプロパティが追加されてい `Minimum` `Maximum` ます。

## <a name="sliders-for-color-selection"></a>色の選択のスライダー

[**SliderDemos**](/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos)サンプルの最後の2ページでは、色の選択に3つのインスタンスが使用されて `Slider` います。 最初のページは分離コードファイル内のすべての対話を処理しますが、2番目のページでは、データバインディングをビューモデルで使用する方法を示しています。

### <a name="handling-sliders-in-the-code-behind-file"></a>分離コードファイルのスライダーの処理

**RGB カラースライダー** ページでは、をインスタンス化して `BoxView` 色を表示し、3つのインスタンスを使用して `Slider` 色の赤、緑、および青のコンポーネントを選択し、 `Label` それらの色の値を表示するための3つの要素を指定します。

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

は、 `Style` 3 つ `Slider` の要素すべてに 0 ~ 255 の範囲を与えます。 要素は、 `Slider` `ValueChanged` 分離コードファイルに実装されているのと同じハンドラーを共有します。

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

最初のセクションでは、 `Text` いずれかのインスタンスのプロパティを `Label` 、の値を16進数で示す短いテキスト文字列に設定し `Slider` ます。 次に、3つのすべての `Slider` インスタンスにアクセスし `Color` て、RGB コンポーネントから値を作成します。

[![RGB カラースライダー](slider-images/RgbColorSliders.png "RGB カラースライダー")](slider-images/RgbColorSliders-Large.png#lightbox)

### <a name="binding-the-slider-to-a-viewmodel"></a>ビューモデルへのスライダーのバインド

[ **HSL カラー] スライダー** ページでは、モデルビューを使用して、 `Color` 色合い、鮮やかさ、および明るさの値から値を作成するための計算を実行する方法を示します。 すべての Viewmodel と同様に、 `HSLColorViewModel` クラスは `INotifyPropertyChanged` インターフェイスを実装し、 `PropertyChanged` プロパティのいずれかが変更されるたびにイベントを発生させます。

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

Viewmodel と `INotifyPropertyChanged` インターフェイスについては、「 [データバインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)」で説明します。

**HslColorSlidersPage** ファイルは、をインスタンス化 `HslColorViewModel` し、ページのプロパティに設定します。 `BindingContext` これにより、XAML ファイル内のすべての要素を、ビューモデルのプロパティにバインドできます。

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

要素が `Slider` 操作されると、 `BoxView` `Label` 要素と要素がビューモデルから更新されます。

[![HSL の色スライダー](slider-images/HslColorSliders.png "HSL の色スライダー")](slider-images/HslColorSliders-Large.png#lightbox)

`StringFormat` `Binding` マークアップ拡張機能のコンポーネントは、小数点以下2桁を表示する "F2" の形式に設定されています。 (データバインディングでの文字列の書式設定については、「 [文字列の書式設定](~/xamarin-forms/app-fundamentals/data-binding/string-formatting.md)」で説明しています)。ただし、プログラムの UWP バージョンは、0、0.1、0.2、...0.9 および1.0。 これは、 `Slider` 前述の「 [プラットフォームの実装の相違点](#platform-implementation-differences)」で説明した UWP の実装の直接的な結果です。

## <a name="related-links"></a>関連リンク

- [スライダーデモのサンプル](/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos)
- [スライダー API](xref:Xamarin.Forms.Slider)