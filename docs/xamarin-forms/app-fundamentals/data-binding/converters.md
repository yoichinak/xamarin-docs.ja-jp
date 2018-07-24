---
title: Xamarin.Forms 値コンバーターのバインディング
description: この記事では、キャストまたは (これは、バインディング コンバーターの場合、またはバインディング値コンバーターでとも呼ばれます) の値コンバーターを実装することで、Xamarin.Forms のデータ バインディング内の値を変換する方法について説明します。
ms.prod: xamarin
ms.assetid: 02B1BBE6-D804-490D-BDD4-8ACED8B70C92
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 28892692133020de1fa5a6eb007bb3f9bcf2612b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997485"
---
# <a name="xamarinforms-binding-value-converters"></a>Xamarin.Forms 値コンバーターのバインディング

データ バインド通常データ転送、ソース プロパティのターゲット プロパティのソース プロパティに、ターゲット プロパティからいくつかの場合。 この転送は、ソースとターゲットのプロパティが同じ型の場合、または 1 つの型は暗黙的な変換を使用して他の型に変換できる場合に簡単です。 そうしない場合は、ときに、型変換が行う必要があります。

[**文字列の書式設定**](string-formatting.md)記事では、使用する方法を説明しました、`StringFormat`を文字列に任意の型を変換へのデータ バインディングのプロパティ。 他の種類の変換では、実装するクラスの特殊なコードを記述する必要があります、 [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter)インターフェイス。 (ユニバーサル Windows プラットフォームには、という名前のようなクラスが含まれています[ `IValueConverter` ](/uwp/api/Windows.UI.Xaml.Data.IValueConverter/)で、`Windows.UI.Xaml.Data`名前空間には、これが`IValueConverter`では、`Xamarin.Forms`名前空間。)。実装するクラス`IValueConverter`と呼ばれます*値コンバーター*、それらも多くの場合と呼びますが、*バインディング コンバーター*または*値コンバーターのバインディング*します。

## <a name="the-ivalueconverter-interface"></a>IValueConverter インターフェイス

型の基になるプロパティのデータ バインドを定義したいとします`int`、ターゲット プロパティが、`bool`します。 生成するには、このデータ バインドする、`false`整数のソースが 0 に等しいを値と`true`それ以外の場合。  

実装するクラスを使用してこれを行う、`IValueConverter`インターフェイス。

```csharp
public class IntToBoolConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (int)value != 0;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (bool)value ? 1 : 0;
    }
}
```

このクラスのインスタンスを設定する、 [ `Converter` ](xref:Xamarin.Forms.Binding.Converter)のプロパティ、`Binding`クラスまたは、 [ `Converter` ](xref:Xamarin.Forms.Xaml.BindingExtension.Converter)のプロパティ、`Binding`マークアップ拡張機能。 このクラスでは、データ バインディングの一部になります。

`Convert`でターゲットをソースからデータを移動すると、メソッドが呼び出された`OneWay`または`TwoWay`バインドします。 `value`パラメーターは、オブジェクトまたはデータ バインディング ソースからの値。 メソッドは、データ バインディング ターゲットの型の値を返す必要があります。 メソッドのキャストをここで示すように、`value`パラメーターを`int`の場合は 0 と比較し、`bool`値を返します。

`ConvertBack`ターゲットからソースへのデータが移動するときに、メソッドが呼び出された`TwoWay`または`OneWayToSource`バインドします。 `ConvertBack` 逆の変換を実行します: と想定して、`value`パラメーターは、 `bool` 、ターゲットからに変換して、`int`ソースの値を返します。

データ バインディングも含まれている場合、`StringFormat`を設定する、値コンバーターが呼び出される前に、結果が文字列として書式設定します。

**を有効にするボタン**ページで、 [**データ バインディング デモ**](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)サンプル データ バインディングでこの値コンバーターを使用する方法を示します。 `IntToBoolConverter`ページのリソース ディクショナリでインスタンス化されます。 参照されているし、`StaticResource`を設定するマークアップ拡張機能、 `Converter` 2 つのデータ バインドのプロパティ。 ページに対する複数のデータ バインドの間でのデータ コンバーターを共有する非常に一般的です。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.EnableButtonsPage"
             Title="Enable Buttons">
    <ContentPage.Resources>
        <ResourceDictionary>
            <local:IntToBoolConverter x:Key="intToBool" />
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Padding="10, 0">
        <Entry x:Name="entry1"
               Text=""
               Placeholder="enter search term"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Search"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                IsEnabled="{Binding Source={x:Reference entry1},
                                    Path=Text.Length,
                                    Converter={StaticResource intToBool}}" />

        <Entry x:Name="entry2"
               Text=""
               Placeholder="enter destination"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Submit"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                IsEnabled="{Binding Source={x:Reference entry2},
                                    Path=Text.Length,
                                    Converter={StaticResource intToBool}}" />
    </StackLayout>
</ContentPage>
```

アプリケーションの複数のページで、値コンバーターを使用する場合はインスタンス化することでリソース ディクショナリに、 **App.xaml**ファイル。

**を有効にするボタン**ページは、共通に必要なときに、`Button`ユーザーに入力したテキストに基づいて操作を実行、`Entry`ビュー。 何も入力されている場合、 `Entry`、`Button`無効にする必要があります。 各`Button`がデータ バインドに含まれています、`IsEnabled`プロパティ。 データ バインディング ソースが、`Length`のプロパティ、`Text`プロパティの対応する`Entry`します。 その場合`Length`プロパティが 0 の場合、値コンバーターを返します`true`と`Button`が有効になって。

[![ボタンを有効にする](converters-images/enablebuttons-small.png "ボタンを有効にする")](converters-images/enablebuttons-large.png#lightbox "ボタンを有効にします。")

注意、`Text`各プロパティ`Entry`は空の文字列に初期化されます。 `Text`プロパティは`null`既定、およびデータ バインドは機能しない場合。

他のユーザーは、一般に、特定のアプリケーション固有のいくつかの値コンバーターが書き込まれます。 値コンバーターがでのみ使用することがわかっている場合`OneWay`、バインド、`ConvertBack`メソッドは単に返す`null`します。

`Convert`暗黙的に上記の方法を前提としていますが、`value`引数が型の`int`型の戻り値がある必要があります`bool`します。 同様に、`ConvertBack`メソッドを前提としていますが、`value`引数が型の`bool`、戻り値は`int`します。 そうしない場合は、ランタイム例外が発生します。

値コンバーターをより一般化して、いくつかの異なる種類のデータを受け入れるように記述できます。 `Convert`と`ConvertBack`メソッドを使用できます、`as`または`is`演算子、`value`パラメーターを呼び出すか、または`GetType`何か、型、および操作を決定するには、そのパラメーターに適切な。 各メソッドの戻り値の型がで指定された、`targetType`パラメーター。 ターゲットの種類のデータ バインディングで値コンバーターを使用する場合があります。値コンバーターが使用できる、`targetType`適切な型の変換を実行する引数。

実行される変換が異なるさまざまなカルチャ用の場合を使用して、`culture`この目的のためのパラメーター。 `parameter`引数`Convert`と`ConvertBack`はこの記事の後半で説明します。

## <a name="binding-converter-properties"></a>バインディング コンバーター プロパティ

値コンバーターのクラスには、プロパティとジェネリック パラメーターを持つことができます。 この特定の値コンバーターの変換、`bool`型のオブジェクトをソースから`T`ターゲット。

```csharp
public class BoolToObjectConverter<T> : IValueConverter
{
    public T TrueObject { set; get; }

    public T FalseObject { set; get; }

    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (bool)value ? TrueObject : FalseObject;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return ((T)value).Equals(TrueObject);
    }
}
```

**スイッチ インジケーター**ページは、これを使用しての値を表示する方法を示します、`Switch`ビュー。 このページに共通するリソース ディクショナリ内のリソースとして値コンバーターをインスタンス化は、別の方法を示しています。 間にそれぞれの値コンバーターがインスタンス化される`Binding.Converter`プロパティ要素タグ。 `x:TypeArguments`汎用の引数を示しますと`TrueObject`と`FalseObject`はどちらもその型のオブジェクトに設定します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.SwitchIndicatorsPage"
             Title="Switch Indicators">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="FontSize" Value="18" />
                <Setter Property="VerticalOptions" Value="Center" />
            </Style>

            <Style TargetType="Switch">
                <Setter Property="VerticalOptions" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Padding="10, 0">
        <StackLayout Orientation="Horizontal"
                     VerticalOptions="CenterAndExpand">
            <Label Text="Subscribe?" />
            <Switch x:Name="switch1" />
            <Label>
                <Label.Text>
                    <Binding Source="{x:Reference switch1}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="x:String"
                                                         TrueObject="Of course!"
                                                         FalseObject="No way!" />
                        </Binding.Converter>
                    </Binding>
                </Label.Text>
            </Label>
        </StackLayout>

        <StackLayout Orientation="Horizontal"
                     VerticalOptions="CenterAndExpand">
            <Label Text="Allow popups?" />
            <Switch x:Name="switch2" />
            <Label>
                <Label.Text>
                    <Binding Source="{x:Reference switch2}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="x:String"
                                                         TrueObject="Yes"
                                                         FalseObject="No" />
                        </Binding.Converter>
                    </Binding>
                </Label.Text>
                <Label.TextColor>
                    <Binding Source="{x:Reference switch2}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="Color"
                                                         TrueObject="Green"
                                                         FalseObject="Red" />
                        </Binding.Converter>
                    </Binding>
                </Label.TextColor>
            </Label>
        </StackLayout>

        <StackLayout Orientation="Horizontal"
                     VerticalOptions="CenterAndExpand">
            <Label Text="Learn more?" />
            <Switch x:Name="switch3" />
            <Label FontSize="18"
                   VerticalOptions="Center">
                <Label.Style>
                    <Binding Source="{x:Reference switch3}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="Style">
                                <local:BoolToObjectConverter.TrueObject>
                                    <Style TargetType="Label">
                                        <Setter Property="Text" Value="Indubitably!" />
                                        <Setter Property="FontAttributes" Value="Italic, Bold" />
                                        <Setter Property="TextColor" Value="Green" />
                                    </Style>                                    
                                </local:BoolToObjectConverter.TrueObject>

                                <local:BoolToObjectConverter.FalseObject>
                                    <Style TargetType="Label">
                                        <Setter Property="Text" Value="Maybe later" />
                                        <Setter Property="FontAttributes" Value="None" />
                                        <Setter Property="TextColor" Value="Red" />
                                    </Style>
                                </local:BoolToObjectConverter.FalseObject>
                            </local:BoolToObjectConverter>
                        </Binding.Converter>
                    </Binding>
                </Label.Style>
            </Label>
        </StackLayout>
    </StackLayout>
</ContentPage>
```

最後の 3 つ`Switch`と`Label`ペア、汎用の引数に設定されている`Style`、全体と`Style`の値のオブジェクトが提供されている`TrueObject`と`FalseObject`します。 これらの暗黙的なスタイルをオーバーライド`Label`リソース ディクショナリに設定して、そのためそのスタイルのプロパティに明示的に割り当て、`Label`します。 切り替え、 `Switch` 、対応すると、`Label`変更が反映されます。

[![インジケーターの切り替え](converters-images/switchindicators-small.png "インジケーターを切り替える")](converters-images/switchindicators-large.png#lightbox "インジケーターの切り替え")

使用することも[ `Triggers` ](~/xamarin-forms/app-fundamentals/triggers.md)のような変更を他のビューに基づくユーザー インターフェイスに実装します。

## <a name="binding-converter-parameters"></a>コンバーター パラメーターのバインド

`Binding`クラスを定義、 [ `ConverterParameter` ](xref:Xamarin.Forms.Binding.ConverterParameter)プロパティ、および`Binding`マークアップ拡張機能にも定義されています、 [ `ConverterParameter` ](xref:Xamarin.Forms.Xaml.BindingExtension.ConverterParameter)プロパティ。 このプロパティを設定すると場合に、値が渡される、`Convert`と`ConvertBack`メソッドとして、`parameter`引数。 値コンバーターのインスタンスがいくつかのデータ バインディング間で共有されている場合でも、`ConverterParameter`若干異なる変換を実行するさまざまなことができます。

使用`ConverterParameter`色の選択のプログラムでは示されています。 ここで、`RgbColorViewModel`型の 3 つのプロパティを持つ`double`という名前`Red`、`Green`と`Blue`を構築するために使用する、`Color`値。

```csharp
public class RgbColorViewModel : INotifyPropertyChanged
{
    Color color;
    string name;

    public event PropertyChangedEventHandler PropertyChanged;

    public double Red
    {
        set
        {
            if (color.R != value)
            {
                Color = new Color(value, color.G, color.B);
            }
        }
        get
        {
            return color.R;
        }
    }

    public double Green
    {
        set
        {
            if (color.G != value)
            {
                Color = new Color(color.R, value, color.B);
            }
        }
        get
        {
            return color.G;
        }
    }

    public double Blue
    {
        set
        {
            if (color.B != value)
            {
                Color = new Color(color.R, color.G, value);
            }
        }
        get
        {
            return color.B;
        }
    }

    public Color Color
    {
        set
        {
            if (color != value)
            {
                color = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Red"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Green"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Blue"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Color"));

                Name = NamedColor.GetNearestColorName(color);
            }
        }
        get
        {
            return color;
        }
    }

    public string Name
    {
        private set
        {
            if (name != value)
            {
                name = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Name"));
            }
        }
        get
        {
            return name;
        }
    }
}
```

`Red`、 `Green`、および`Blue`0 から 1 までの範囲のプロパティ。 ただし、コンポーネントが 2 桁の 16 進数値として表示されることができます。

XAML では、16 進数の値としてこれらを表示するがあり必要があります 255 を掛けた値、整数に変換で"X2"の仕様を使用して書式設定された、`StringFormat`プロパティ。 値コンバーターでは、最初の 2 つのタスク (255 を掛けることと、整数に変換する) を処理できます。 値コンバーターをできるだけ汎用化するために、乗算の係数を指定できます、`ConverterParameter`プロパティで、入力したことを意味、`Convert`と`ConvertBack`メソッドとして、`parameter`引数。

```csharp
public class DoubleToIntConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (int)Math.Round((double)value * GetParameter(parameter));
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (int)value / GetParameter(parameter);
    }

    double GetParameter(object parameter)
    {
        if (parameter is double)
            return (double)parameter;

        else if (parameter is int)
            return (int)parameter;

        else if (parameter is string)
            return double.Parse((string)parameter);

        return 1;
    }
}
```

`Convert`から変換を`double`に`int`を乗算しながら、 `parameter` ; 値、 `ConvertBack` 、整数の除算`value`引数で`parameter`を返します、`double`結果。 (次に示すプログラムで、値コンバーターが使用される文字列の書式設定、のみに関連ため`ConvertBack`は使用されません)。

種類、`parameter`引数があるコードまたは XAML でデータ バインディングが定義されているかどうかによって異なる可能性があります。 場合、`ConverterParameter`プロパティの`Binding`設定されているコードでは数値の値に設定する可能性があります。

```csharp
binding.ConverterParameter = 255;
```

`ConverterParameter`プロパティの型は`Object`ので、c# コンパイラは、整数としてリテラル 255 を解釈し、プロパティ値を設定します。

ただし、XAML で、`ConverterParameter`は次のように設定する可能性があります。

```xaml
<Label Text="{Binding Red,
                      Converter={StaticResource doubleToInt},
                      ConverterParameter=255,
                      StringFormat='Red = {0:X2}'}" />
```

ため、255 はなどの番号ですが`ConverterParameter`の種類は`Object`、XAML パーサーは文字列として、255 を扱います。

そのため、値コンバーター前に示したには個別が含まれています`GetParameter`のケースを処理するメソッドを`parameter`型`double`、 `int`、または`string`します。  

**RGB カラー セレクター**ページをインスタンス化`DoubleToIntConverter`次の 2 つの暗黙的なスタイルの定義のリソース ディクショナリに。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.RgbColorSelectorPage"
             Title="RGB Color Selector">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Slider">
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>

            <local:DoubleToIntConverter x:Key="doubleToInt" />
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <StackLayout.BindingContext>
            <local:RgbColorViewModel Color="Gray" />
        </StackLayout.BindingContext>

        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <StackLayout Margin="10, 0">
            <Label Text="{Binding Name}" />

            <Slider Value="{Binding Red}" />
            <Label Text="{Binding Red,
                                  Converter={StaticResource doubleToInt},
                                  ConverterParameter=255,
                                  StringFormat='Red = {0:X2}'}" />

            <Slider Value="{Binding Green}" />
            <Label Text="{Binding Green,
                                  Converter={StaticResource doubleToInt},
                                  ConverterParameter=255,
                                  StringFormat='Green = {0:X2}'}" />

            <Slider Value="{Binding Blue}" />
            <Label>
                <Label.Text>
                    <Binding Path="Blue"
                             StringFormat="Blue = {0:X2}"
                             Converter="{StaticResource doubleToInt}">
                        <Binding.ConverterParameter>
                            <x:Double>255</x:Double>
                        </Binding.ConverterParameter>
                    </Binding>
                </Label.Text>
            </Label>
        </StackLayout>
    </StackLayout>
</ContentPage>    
```

値、`Red`と`Green`でプロパティが表示されます、`Binding`マークアップ拡張機能。 `Blue`プロパティ、ただし、インスタンスを作成、`Binding`クラスを明示的な方法を示すために`double`に値を設定することができます`ConverterParameter`プロパティ。

結果を次に示します。

[![RGB 色セレクター](converters-images/rgbcolorselector-small.png "RGB カラー セレクター")](converters-images/rgbcolorselector-large.png#lightbox "RGB カラー セレクター")


## <a name="related-links"></a>関連リンク

- [データ バインディング デモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms book からデータ バインド」の章](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
