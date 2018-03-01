---
title: "バインディングの値コンバーター"
description: "キャストまたはデータのバインド内の値の変換"
ms.topic: article
ms.prod: xamarin
ms.assetid: 02B1BBE6-D804-490D-BDD4-8ACED8B70C92
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: aaa4c93eda9edb0eb5d568b3470c02352bdb7467
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="binding-value-converters"></a>バインディングの値コンバーター

通常のデータ バインドでは対象のプロパティにし、場合によっては、ターゲット プロパティから、ソース プロパティにデータ ソース プロパティからに転送してください。 この転送は、ソースとターゲットのプロパティが同じ型の場合、または 1 つの型は、暗黙的な変換によって他の型に変換できる場合に簡単です。 そうしない場合は、ときに、型の変換が行う必要があります。

[**文字列の書式設定**](string-formatting.md)資料を使用する方法を説明する、`StringFormat`任意の型を文字列に変換するデータ バインドのプロパティです。 他の種類の変換では、実装するクラスの特殊化されたコードを記述する必要があります、 [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/)インターフェイスです。 (ユニバーサル Windows プラットフォームには、という名前のようなクラスが含まれています[ `IValueConverter` ](/uwp/api/Windows.UI.Xaml.Data.IValueConverter/)で、`Windows.UI.Xaml.Data`が、この名前空間`IValueConverter`では、`Xamarin.Forms`名前空間です。)。実装するクラス`IValueConverter`と呼ばれます*値コンバーター*と呼びますが、多くの場合は*コンバーターをバインド*または*値コンバーターをバインド*です。

## <a name="the-ivalueconverter-interface"></a>IValueConverter インターフェイス

型の基になるプロパティがここでは、データ バインディングを定義すると仮定`int`ターゲット プロパティが、`bool`です。 生成するためにこのデータ バインドする、 `false` integer ソースが 0 に等しい場合は値と`true`それ以外の場合。  

実装するクラスでこれを行う、`IValueConverter`インターフェイス。

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

このクラスのインスタンスを設定する、 [ `Converter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Converter/)のプロパティ、`Binding`クラスまたは、 [ `Converter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.Converter/)のプロパティ、`Binding`マークアップ拡張機能です。 このクラスでは、データ バインディングの一部になります。

`Convert`をターゲットにするソースからデータを移動すると、メソッドが呼び出されます`OneWay`または`TwoWay`バインドします。 `value`パラメーターは、オブジェクトまたはデータ バインディング ソースからの値。 メソッドは、データ バインディング ターゲットの型の値を返す必要があります。 メソッドのキャストを次に示す、`value`パラメーターを`int`の場合は 0 と比較し、`bool`値を返します。

`ConvertBack`データが、ターゲット内のソースに移動すると、メソッドが呼び出されます`TwoWay`または`OneWayToSource`バインドします。 `ConvertBack` 逆の変換を実行します: を想定しています、`value`パラメーターは、 `bool` 、ターゲットからに変換して、`int`ソースの値を返します。

データ バインディングにも含まれている場合、`StringFormat`設定で、値コンバーターが呼び出される結果は文字列として書式設定前にします。

**を有効にするボタン** ページで、 [**データ バインディング デモ**](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)サンプル データ バインディングで値コンバーターを使用する方法を示します。 `IntToBoolConverter`ページのリソース ディクショナリでインスタンス化します。 参照されているし、`StaticResource`を設定するマークアップ拡張機能、 `Converter` 2 つのデータ バインドのプロパティです。 このコマンドは、ページ上の複数のデータ バインディング間でデータ コンバーターを共有するごく一般的です。

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

場合は、アプリケーションの複数のページで値コンバーターを使用することができますでインスタンス化することでリソース ディクショナリ、 **App.xaml**ファイル。

**を有効にするボタン** ページでは、共通に必要な場合を示しています、`Button`ユーザーに入力したテキストに基づいて操作を実行、`Entry`ビュー。 何が型指定されている場合、 `Entry`、`Button`無効にする必要があります。 各`Button`上のデータ バインドを含むその`IsEnabled`プロパティです。 データ バインディング ソースが、`Length`のプロパティ、`Text`の対応するプロパティ`Entry`です。 場合は、その`Length`プロパティが 0 の場合、値コンバーターを返します`true`と`Button`が有効になっています。

[![ボタンを有効にする](converters-images/enablebuttons-small.png "ボタンを有効にする")](converters-images/enablebuttons-large.png "ボタンを有効にします。")

注意して、`Text`プロパティにそれぞれ`Entry`は空の文字列に初期化します。 `Text`プロパティは`null`既定では、データ バインドは機能しないという点です。

他のユーザーが汎用化されて中に、特定のアプリケーションに固有のいくつかの値コンバーターが書き込まれます。 値コンバーターがでのみ使用することがわかっている場合`OneWay`、バインド、`ConvertBack`メソッドは単に返す`null`です。

`Convert`暗黙的に上記の方法が想定する、`value`引数の型が`int`、戻り値型でなければなりません`bool`です。 同様に、`ConvertBack`メソッドを想定する、`value`引数の型が`bool`あり、戻り値は`int`します。 そうしない場合は、ランタイム例外が発生します。

汎用化し、いくつかの異なる種類のデータを受け入れるように、値コンバーターを記述することができます。 `Convert`と`ConvertBack`メソッドを使用できます、`as`または`is`演算子、`value`パラメーターを呼び出すか、または`GetType`にその種類、および操作を何かを決定するパラメーターの適切な。 各メソッドの戻り値の型である、`targetType`パラメーター。 別のターゲットの種類のデータ バインディングで値コンバーターを使用する場合があります。値コンバーターが使用できる、`targetType`適切な型の変換を実行する引数。

実行される変換が複数の異なるカルチャの別の場合を使用して、`culture`この目的のためのパラメーターです。 `parameter`引数`Convert`と`ConvertBack`はこの記事の後半で説明します。

## <a name="binding-converter-properties"></a>コンバーターのバインドのプロパティ

値コンバーターのクラスには、プロパティとジェネリック パラメーターを持つことができます。 この特定の値コンバーターの変換、`bool`型のオブジェクトへのソースから`T`ターゲット。

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

**スイッチ インジケーター**  ページでは、これを使用しての値を表示する方法を示します、`Switch`ビュー。 リソース ディクショナリ内のリソースとして値コンバーターのインスタンスを作成する一般的なは、このページを示していますの代わり: 間でそれぞれの値コンバーターがインスタンス化される`Binding.Converter`プロパティ要素タグ。 `x:TypeArguments` 、汎用引数を示すと`TrueObject`と`FalseObject`はどちらもその型のオブジェクトに設定。

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

最後の 3 つ`Switch`と`Label`ペア、汎用引数に設定されている`Style`、全体と`Style`の値のオブジェクトが提供される`TrueObject`と`FalseObject`です。 これらのオーバーライドの暗黙的なスタイル`Label`リソース ディクショナリで設定、したがってそのスタイルのプロパティに明示的に割り当て、`Label`です。 切り替え、 `Switch` 、対応すると、`Label`変更を反映するように。

[![インジケーターを切り替える](converters-images/switchindicators-small.png "インジケーターを切り替える")](converters-images/switchindicators-large.png "インジケーターを切り替える")

使用することも[ `Triggers` ](~/xamarin-forms/app-fundamentals/triggers.md)他のビューに基づくユーザー インターフェイスで同じ変更を実装します。

## <a name="binding-converter-parameters"></a>コンバーター パラメーターのバインド

`Binding`クラスを定義、 [ `ConverterParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.ConverterParameter/)プロパティ、および`Binding`マークアップ拡張機能も定義、 [ `ConverterParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.ConverterParameter/)プロパティです。 このプロパティを設定すると場合に、値が渡される、`Convert`と`ConvertBack`メソッドとして、`parameter`引数。 値コンバーターのインスタンスがいくつかのデータ バインディング間で共有されている場合でも、`ConverterParameter`若干異なる変換を実行する異なる場合があります。

使用`ConverterParameter`色の選択のプログラムの説明。 ここで、`RgbColorViewModel`型の 3 つのプロパティを持つ`double`という名前`Red`、 `Green`、および`Blue`構築するために使用されている、`Color`値。

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

`Red`、 `Green`、および`Blue`プロパティ範囲 0 ~ 1 の間です。 ただし、するには、コンポーネントが 2 桁の 16 進数値として表示されることをお勧めします。

XAML では、16 進数の値としてこれらを表示するにする必要がある乗算 255 を整数に変換されで"X2"の仕様を使用して書式設定された、`StringFormat`プロパティです。 最初の 2 つのタスク (255 で乗算および整数に変換する) は、値コンバーターで処理されることができます。 値コンバーターのように、できるだけ汎用化するために、乗算の係数を指定できます、`ConverterParameter`が入ることを意味するプロパティ、`Convert`と`ConvertBack`メソッドとして、`parameter`引数。

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

`Convert`から変換、`double`に`int`で乗算しながら、`parameter`値。、 `ConvertBack` 、整数の除算`value`引数で`parameter`を返します、`double`結果。 (プログラムでは、次に示す、値コンバーターはのみに関連して使用文字列の書式設定、したがって`ConvertBack`は使用されません)。

型、`parameter`引数があるコードか XAML でのデータ バインディングを定義するかどうかに応じて異なる可能性があります。 場合、`ConverterParameter`のプロパティ`Binding`設定されているコードでは数値の値に設定する可能性があります。

```csharp
binding.ConverterParameter = 255;
```

`ConverterParameter`プロパティの型は`Object`ので、c# コンパイラが、整数としてリテラル 255 を解釈し、その値にプロパティを設定します。

ただし、XAML で、`ConverterParameter`は次のように設定する可能性があります。

```xaml
<Label Text="{Binding Red,
                      Converter={StaticResource doubleToInt},
                      ConverterParameter=255,
                      StringFormat='Red = {0:X2}'}" />
```

255 の外観、番号ですがあるため`ConverterParameter`の種類は`Object`、XAML パーサーは文字列として、255 を処理します。

そのため前に、示した値コンバーターに個別にはが含まれています`GetParameter`のケースを処理するメソッド`parameter`型`double`、 `int`、または`string`です。  

**RGB カラー セレクター**ページをインスタンス化`DoubleToIntConverter`次の 2 つの暗黙的なスタイルの定義のリソース ディクショナリで。

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

値、`Red`と`Green`でプロパティが表示されます、`Binding`マークアップ拡張機能です。 `Blue`プロパティ、ただし、インスタンスを作成、 `Binding` 、明示的な方法を示すためにクラス`double`値に設定することができます`ConverterParameter`プロパティです。

結果を次に示します。

[![RGB 色セレクター](converters-images/rgbcolorselector-small.png "RGB カラー セレクター")](converters-images/rgbcolorselector-large.png "RGB 色セレクター")


## <a name="related-links"></a>関連リンク

- [データ バインディング デモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms 帳からのデータ バインディング章](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
