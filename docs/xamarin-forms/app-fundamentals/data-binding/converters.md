---
title: Xamarin.Forms バインディングの値コンバーター
description: この記事では、値コンバーター (バインディング コンバーターまたはバインディング値コンバーターとも呼ばれます) を実装することで、Xamarin.Forms データ バインディング内で値をキャストまたは変換する方法について説明します。
ms.prod: xamarin
ms.assetid: 02B1BBE6-D804-490D-BDD4-8ACED8B70C92
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 34b449aa358874f06a495ec52578dcca2dd13767
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68654735"
---
# <a name="xamarinforms-binding-value-converters"></a>Xamarin.Forms バインディングの値コンバーター

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)

データ バインディングでは通常、ソース プロパティからターゲット プロパティへ、または、場合によってはターゲット プロパティからソース プロパティへ、データを転送します。 ソース プロパティとターゲット プロパティの型が同じ場合、または、暗黙の型変換によって一方の型がもう一方の型に変換できる場合は、この転送はすみやかに進みます。 それ以外の場合は、型変換を行う必要があります。

[**文字列の書式設定**](string-formatting.md)に関する記事では、データ バインディングの `StringFormat` プロパティを使用して任意の型を文字列に変換する方法を確認しました。 それ以外の型の変換では、[`IValueConverter`](xref:Xamarin.Forms.IValueConverter) インターフェイスを実装するクラス内に専用のコードを記述する必要があります (ユニバーサル Windows プラットフォームには、`Windows.UI.Xaml.Data` 名前空間内に [`IValueConverter`](/uwp/api/Windows.UI.Xaml.Data.IValueConverter/) という名前の類似のクラスが含まれますが、この `IValueConverter` は `Xamarin.Forms` 名前空間内にあります)。`IValueConverter` を実装するクラスは、*値コンバーター*と呼ばれますが、*バインディング コンバーター*または*バインディング値コンバーター*と呼ばれることもよくあります。

## <a name="the-ivalueconverter-interface"></a>IValueConverter インターフェイス

ソース プロパティの型は `int` だが、ターゲット プロパティは `bool` の場合のデータ バインディングの定義を検討します。 このデータ バインディングでは、整数のソースが 0 に等しい場合に `false` を、それ以外の場合は `true` を生成する予定です。  

これは、`IValueConverter` インターフェイスを実装するクラスを使って実現できます。

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

このクラスのインスタンスを `Binding` クラスの [`Converter`](xref:Xamarin.Forms.Binding.Converter) プロパティまたは `Binding` マークアップ拡張の [`Converter`](xref:Xamarin.Forms.Xaml.BindingExtension.Converter) プロパティに設定します。 このクラスは、データ バインディングの一部になります。

`OneWay` または `TwoWay` バインディング内でソースからターゲットにデータを移行するときに、`Convert` メソッドが呼び出されます。 `value` パラメーターは、データ バインディング ソースからのオブジェクトまたは値になります。 メソッドは、データ バインディング ターゲットの型の値を返す必要があります。 ここに示したメソッドでは、`value` パラメーターを `int` にキャストして、その値を `bool` 戻り値の 0 と比較しています。

`TwoWay` または `OneWayToSource` バインディング内でターゲットから ースにデータを移行するときに、`ConvertBack` メソッドが呼び出されます。 `ConvertBack` は逆の変換を実行します。`value` パラメーターがターゲットからの `bool` であり、その値をソースの `int` 戻り値に変換することを想定しています。

データ バインディングにも `StringFormat` 設定が含まれている場合、結果が文字列として書式設定される前に、値コンバーターが呼び出されます。

[**Data Binding Demos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos) (データ バインディング デモ) サンプルの**Enable Buttons** (ボタンの有効化) のページでは、データ バインディング内でこの値コンバーターを使用する方法のサンプルを示しています。 `IntToBoolConverter` は、ページのリソース ディクショナリ内でインスタンス化されます。 その後、2 つのデータ バインディングに `Converter` プロパティを設定するために、`StaticResource` マークアップ拡張を使って参照されます。 ページ上の複数のデータ バインディング間でデータ コンバーターを共有することは、極めて一般的です

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

アプリケーションの複数のページで値コンバーターが使用されている場合は、**App.xaml** ファイルのリソース ディクショナリ内でインスタンス化できます。

**Enable Buttons** のページでは、ユーザーが `Entry` ビューに入力するテキストに基づいて `Button` による操作が実行される場合の、一般的なニーズを示しています。 `Entry` に何も入力されなかった場合は、`Button` を無効にする必要があります。 各 `Button` は、`IsEnabled` プロパティ上にデータ バインディングを含みます。 データ バインディング ソースは、対応する `Entry` の `Text` プロパティの `Length` プロパティです。 その `Length` プロパティが 0 ではない場合、値コンバーターは `true` を返し、`Button` は有効になります。

[![Enable Buttons (ボタンの有効化)](converters-images/enablebuttons-small.png "Enable Buttons (ボタンの有効化)")](converters-images/enablebuttons-large.png#lightbox "Enable Buttons (ボタンの有効化)")

各 `Entry` の `Text` プロパティは、空の文字列に初期化されることに注目してください。 `Text` プロパティは既定で `null` であり、その場合、データ バインディングは機能しません。

値コンバーターが、明示的に特定のアプリケーションに向けて記述されている場合もありますが、それ以外は汎用化されています。 値コンバーターが `OneWay` バインディング内のみで使用されることがわかっている場合、`ConvertBack` メソッドは単純に `null` を返すことができます。

上記の `Convert` メソッドでは、暗黙的に `value` 引数の型は `int` であり、戻り値の型は必ず `bool` であることを想定しています。 同様に、`ConvertBack` メソッドでは、`value` 引数の型は `bool` であり、戻り値は `int` であることを想定しています。 これに該当しない場合、ランタイム例外が発生します。

より汎用化されるように、また、複数の異なるデータ型を許可するように、値コンバーターを記述できます。 `Convert` および `ConvertBack` メソッドでは、`value` パラメーターと共に `as` または `is` 演算子を使用できます。または、このパラメーター上で `GetType` を呼び出して、値の型を特定してから、適切な処理を実行することが可能です。 各メソッドの戻り値で想定される型は、`targetType` パラメーターによって指定されます。 場合によっては、値コンバーターはさまざまなターゲット型のデータ バインディングに使用されます。値コンバーターは `targetType` 引数を使用して、正しい型への変換を実行できます。

別のカルチャに対しては実行される変換が異なる場合には、この目的のために `culture` パラメーターを使用します。 `Convert` および `ConvertBack` への `parameter` 引数については、この記事で後述します。

## <a name="binding-converter-properties"></a>バインディング コンバーター プロパティ

値コンバーターのクラスには、プロパティとジェネリック パラメーターを含めることができます。 この特定の値コンバーターは、ソースからターゲットの `T` 型オブジェクトに `bool` を変換します。

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

**Switch Indicators** (インジケーターの切り替え) のページは、`Switch` ビューの値を表示するために使用できる方法を示しています。 リソース ディクショナリ内でリソースとして値コンバーターをインスタンス化することが一般的ですが、このページでは別の方法を示しています。各値コンバーターが `Binding.Converter` プロパティ要素タグ間でインスタンス化されています。 `x:TypeArguments` は汎用の引数を示し、`TrueObject` および `FalseObject` は両方とも、その型のオブジェクトに設定されます。

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

最後の 3 つの `Switch` と `Label` のペアでは、汎用の引数が `Style` に設定され、`Style` オブジェクト全体が `TrueObject` および `FalseObject` の値に指定されています。 これらは、リソース ディクショナリ内の `Label` 設定の暗黙的な形式をオーバーライドしているので、その形式のプロパティが `Label` に明示的に割り当てられます。 `Switch` を切り替えると、対応する `Label` に変更が反映されます。

[![Switch Indicators (インジケーターの切り替え)](converters-images/switchindicators-small.png "Switch Indicators (インジケーターの切り替え)")](converters-images/switchindicators-large.png#lightbox "Switch Indicators (インジケーターの切り替え)")

また、[`Triggers`](~/xamarin-forms/app-fundamentals/triggers.md) を使用して、他のビューに基づくユーザー インターフェイス内に類似の変更を実装することも可能です。

## <a name="binding-converter-parameters"></a>バインディング コンバーター パラメーター

`Binding` クラスは [`ConverterParameter`](xref:Xamarin.Forms.Binding.ConverterParameter) プロパティを定義し、`Binding` マークアップ拡張も [`ConverterParameter`](xref:Xamarin.Forms.Xaml.BindingExtension.ConverterParameter) プロパティを定義します。 このプロパティが設定された場合、値は `Convert` および `ConvertBack` メソッドに `parameter` 引数として渡されます。 複数のデータ バインディング間で値コンバーターのインスタンスが共有されたとしても、少し違った変換を実行するために、`ConverterParameter` は異なる可能性があります。

色選択のプログラムを利用して、`ConverterParameter` の使用例を示します。 ここで、`RgbColorViewModel` には、`Color` 値を構成するために使用される `Red`、`Green`、`Blue` という名前の `double` 型の 3 つのプロパティがあります。

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

`Red`、`Green`、および `Blue` プロパティの範囲は 0 から 1 の間になります。 ただし、コンポーネントが 2 桁の 16 進数値として表示される方が望ましい場合があります。

これらを XAML 内で 16 進数値として表示するには、255 で乗算し、整数に変換してから、`StringFormat` プロパティの "X2" の仕様を使って書式設定する必要があります。 最初の 2 つのタスク (255 で乗算し、整数に変換する) は、値コンバーターによって処理できます。 値コンバーターをできる限り汎用化するために、乗算の係数は `ConverterParameter` プロパティを使って指定できます。つまり、`Convert` および `ConvertBack` メソッドを `parameter` 引数として入力することになります。

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

`Convert` は `parameter` 値で乗算している間に `double` から `int` への変換を行います。`ConvertBack` は整数の `value` 引数を `parameter` で除算し、`double` の結果を返します (以下に示すプログラムでは、値コンバーターが文字列の書式設定との関連付けのみに使用されているので、`ConvertBack` は使用されていません)。

`parameter` 引数の型は、データ バインディングがコード内または XAML 内のどちらに定義されているかに応じて異なる可能性があります。 コード内で `Binding` の `ConverterParameter` プロパティが設定されている場合、通常は、数値型の値に設定されます。

```csharp
binding.ConverterParameter = 255;
```

`ConverterParameter` プロパティは `Object` 型なので、C# コンパイラはリテラルの 255 を整数として解釈し、プロパティにその値を設定します。

しかし、XAML では通常、`ConverterParameter` は次のように設定されます。

```xaml
<Label Text="{Binding Red,
                      Converter={StaticResource doubleToInt},
                      ConverterParameter=255,
                      StringFormat='Red = {0:X2}'}" />
```

255 は数字のように見えますが、`ConverterParameter` は `Object` 型なので、XAML パーサーでは 255 を文字列として処理します。

そのため、上記の値コンバーターには、`parameter` が `double`、`int`、または `string` 型の場合を処理する別個の `GetParameter` メソッドが含まれています。  

**RGB Color Selector** (RGB カラー セレクター) のページでは、2 つの暗黙的な形式の定義に従って、リソース ディクショナリ内で `DoubleToIntConverter` をインスタンス化します。

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

`Red` および `Green` プロパティの値は、`Binding` マークアップ拡張を使って表示されます。 しかし、明示的な `double` 値を `ConverterParameter` プロパティに設定できる方法を示すために、`Blue` プロパティは `Binding` クラスをインスタンス化します。

結果は次のようになります。

[![RGB Color Selector (RGB カラー セレクター)](converters-images/rgbcolorselector-small.png "RGB Color Selector (RGB カラー セレクター)")](converters-images/rgbcolorselector-large.png#lightbox "RGB Color Selector (RGB カラー セレクター)")


## <a name="related-links"></a>関連リンク

- [データ バインディングのデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)
- [Xamarin.Forms 書籍のデータ バインディングに関する章](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
