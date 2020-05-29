---
title: ''
description: ''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 08be571d3ba69891a56c08efd556a999e51431c8
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139855"
---
# <a name="part-4-data-binding-basics"></a>第 4 部 データ バインディングの基礎

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_データバインディングを使用すると、2つのオブジェクトのプロパティをリンクさせて、一方のオブジェクトの変更によってもう一方の変更が行われるようにすることができます。これは非常に有用なツールであり、データバインディングをコードで完全に定義することはできますが、XAML ではショートカットと利便性が提供されます。その結果、の最も重要なマークアップ拡張機能の1つ Xamarin.Forms はバインドです。_

## <a name="data-bindings"></a>データバインディング

データバインディングは、*ソース*と*ターゲット*と呼ばれる、2つのオブジェクトのプロパティを接続します。 コードでは、次の2つの手順が必要です。 `BindingContext` ターゲットオブジェクトのプロパティがソースオブジェクトに設定されている必要があります。また、 `SetBinding` `Binding` オブジェクトのプロパティをソースオブジェクトのプロパティにバインドするには、ターゲットオブジェクトでメソッドを呼び出す必要があります。

ターゲットプロパティは、バインド可能なプロパティである必要があります。これは、ターゲットオブジェクトがから派生する必要があることを意味し `BindableObject` ます。 オンライン Xamarin.Forms ドキュメントでは、バインド可能なプロパティを示しています。 などののプロパティ `Label` `Text` は、バインド可能なプロパティに関連付けられてい `TextProperty` ます。

マークアップでは、コードで必要とされる同じ2つの手順も実行する必要があり `Binding` ます。ただし、マークアップ拡張機能は呼び出しとクラスの代わりに使用し `SetBinding` `Binding` ます。

ただし、XAML でデータバインディングを定義する場合、ターゲットオブジェクトのを設定する方法は複数あり `BindingContext` ます。 分離コードファイルから設定される場合もあります。場合によって `StaticResource` は、またはマークアップ拡張機能を使用します。また、場合によっては、 `x:Static` プロパティ要素タグの内容として設定し `BindingContext` ます。

バインドは、基本的には、プログラムのビジュアルを基になるデータモデルに接続するために使用されます。通常は、パート5で説明したように、MVVM (モデルビューモデル) アプリケーションアーキテクチャの実現に使用され[ます。データバインディングから MVVM へのバインド](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)が可能です。

## <a name="view-to-view-bindings"></a>ビューからビューへのバインド

データバインディングを定義して、同じページ上の2つのビューのプロパティをリンクすることができます。 この場合は、 `BindingContext` マークアップ拡張機能を使用して、対象オブジェクトのを設定し `x:Reference` ます。

次に、と2つのビューを含む XAML ファイルを示し `Slider` `Label` ます。1つは値で回転し、もう1つは値を `Slider` 表示し `Slider` ます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SliderBindingsPage"
             Title="Slider Bindings Page">

    <StackLayout>
        <Label Text="ROTATION"
               BindingContext="{x:Reference Name=slider}"
               Rotation="{Binding Path=Value}"
               FontAttributes="Bold"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="CenterAndExpand" />

        <Label BindingContext="{x:Reference slider}"
               Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
               FontAttributes="Bold"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

には、 `Slider` `x:Name` `Label` `x:Reference` マークアップ拡張機能を使用して2つのビューによって参照される属性が含まれています。

`x:Reference`バインド拡張機能は、という名前のプロパティを定義し、 `Name` この場合は参照先の要素の名前に設定し `slider` ます。 ただし、 `ReferenceExtension` マークアップ拡張機能を定義するクラスは、 `x:Reference` の属性も定義します `ContentProperty` 。これ `Name` は明示的に必須ではないことを意味します。 さまざまなものに対して、最初のには `x:Reference` "Name =" が含まれていますが、2番目の方法では、

```xaml
BindingContext="{x:Reference Name=slider}"
…
BindingContext="{x:Reference slider}"
```

`Binding`マークアップ拡張機能自体は、クラスやクラスと同様に、いくつかのプロパティを持つことができ `BindingBase` `Binding` ます。 `ContentProperty`の `Binding` はですが `Path` 、パスが `Binding` マークアップ拡張機能の最初の項目である場合は、マークアップ拡張機能の "path =" 部分を省略できます。 最初の例では "Path =" が使用されていますが、2番目の例では省略されています。

```xaml
Rotation="{Binding Path=Value}"
…
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

すべてのプロパティは、1行に配置することも、複数の行に分割することもできます。

```xaml
Text="{Binding Value,
               StringFormat='The angle is {0:F0} degrees'}"
```

何でも便利です。

`StringFormat`2 番目の `Binding` マークアップ拡張機能のプロパティに注目してください。 では Xamarin.Forms 、バインディングは暗黙的な型変換を実行しません。文字列以外のオブジェクトを文字列として表示する必要がある場合は、型コンバーターを指定するか、を使用する必要があり `StringFormat` ます。 背後では、静的メソッドを使用してを `String.Format` 実装 `StringFormat` します。 .NET の書式指定の仕様には、マークアップ拡張機能の区切りにも使用される中かっこが含まれているため、これは問題となる可能性があります。 これにより、XAML パーサーが混乱するリスクが生じます。 これを回避するには、書式設定文字列全体を単一引用符で囲みます。

```xaml
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

実行中のプログラムは次のようになります。

[![ビューからビューへのバインド](data-binding-basics-images/sliderbinding.png)](data-binding-basics-images/sliderbinding-large.png#lightbox)

## <a name="the-binding-mode"></a>バインドモード

1つのビューで、いくつかのプロパティにデータバインディングを含めることができます。 ただし、各ビューに含めることができるのは1つだけなので、 `BindingContext` そのビューの複数のデータバインディングが、同じオブジェクトのすべての参照プロパティを持つ必要があります。

この問題およびその他の問題の解決策としては、 `Mode` 列挙体のメンバーに設定されているプロパティが `BindingMode` あります。

- `Default`
- `OneWay`—値はソースからターゲットに転送されます。
- `OneWayToSource`-ターゲットからソースに値が転送されます。
- `TwoWay`-値は、ソースとターゲットの間で両方の方法で転送されます。
- `OneTime`-データがソースからターゲットに移動しますが、変更された場合にのみ `BindingContext`

次のプログラムは、とのバインディングモードの1つの一般的な使用方法を示して `OneWayToSource` `TwoWay` います。 4つ `Slider` のビューは、 `Scale` の、 `Rotate` 、 `RotateX` 、およびの各プロパティを制御するため `RotateY` のもの `Label` です。 最初 `Label` は、それぞれがによって設定されているため、の4つのプロパティがデータバインディングターゲットであると考えられ `Slider` ます。 ただし、のは、 `BindingContext` `Label` オブジェクトを1つだけ持つことができ、4つの異なるスライダーがあります。

そのため、すべてのバインドは、一見した逆方向の方法で設定されます。4つのスライダーのすべてがに設定され、 `BindingContext` 各 `Label` スライダーのプロパティにバインドが設定されます。 `Value` これらのプロパティは、およびモードを使用することにより、の、、、およびの各プロパティである `OneWayToSource` `TwoWay` `Value` ソースプロパティを設定でき `Scale` `Rotate` `RotateX` `RotateY` `Label` ます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SliderTransformsPage"
             Padding="5"
             Title="Slider Transforms Page">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>

        <!-- Scaled and rotated Label -->
        <Label x:Name="label"
               Text="TEXT"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <!-- Slider and identifying Label for Scale -->
        <Slider x:Name="scaleSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="1" Grid.Column="0"
                Maximum="10"
                Value="{Binding Scale, Mode=TwoWay}" />

        <Label BindingContext="{x:Reference scaleSlider}"
               Text="{Binding Value, StringFormat='Scale = {0:F1}'}"
               Grid.Row="1" Grid.Column="1"
               VerticalTextAlignment="Center" />

        <!-- Slider and identifying Label for Rotation -->
        <Slider x:Name="rotationSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="2" Grid.Column="0"
                Maximum="360"
                Value="{Binding Rotation, Mode=OneWayToSource}" />

        <Label BindingContext="{x:Reference rotationSlider}"
               Text="{Binding Value, StringFormat='Rotation = {0:F0}'}"
               Grid.Row="2" Grid.Column="1"
               VerticalTextAlignment="Center" />

        <!-- Slider and identifying Label for RotationX -->
        <Slider x:Name="rotationXSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="3" Grid.Column="0"
                Maximum="360"
                Value="{Binding RotationX, Mode=OneWayToSource}" />

        <Label BindingContext="{x:Reference rotationXSlider}"
               Text="{Binding Value, StringFormat='RotationX = {0:F0}'}"
               Grid.Row="3" Grid.Column="1"
               VerticalTextAlignment="Center" />

        <!-- Slider and identifying Label for RotationY -->
        <Slider x:Name="rotationYSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="4" Grid.Column="0"
                Maximum="360"
                Value="{Binding RotationY, Mode=OneWayToSource}" />

        <Label BindingContext="{x:Reference rotationYSlider}"
               Text="{Binding Value, StringFormat='RotationY = {0:F0}'}"
               Grid.Row="4" Grid.Column="1"
               VerticalTextAlignment="Center" />
    </Grid>
</ContentPage>
```

3つのビューのバインド `Slider` はです `OneWayToSource` 。つまり、値によって、 `Slider` という名前ののプロパティが変更され `BindingContext` `Label` `label` ます。 これら3つ `Slider` のビューによって、 `Rotate` の、、およびの各プロパティが変更さ `RotateX` `RotateY` `Label` れます。

ただし、プロパティのバインド `Scale` は `TwoWay` です。 これは、 `Scale` プロパティの既定値は1であり、バインドを使用すると、 `TwoWay` `Slider` 初期値が0ではなく1に設定されるためです。 そのバインディングが `OneWayToSource` の場合、 `Scale` プロパティは最初に既定値から0に設定され `Slider` ます。 は `Label` 表示されず、ユーザーの混乱を招く可能性があります。

 [![後方バインド](data-binding-basics-images/slidertransforms.png)](data-binding-basics-images/slidertransforms-large.png#lightbox)

 > [!NOTE]
 > クラスには、 [`VisualElement`](xref:Xamarin.Forms.VisualElement) [`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX) プロパティとプロパティもあり [`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY) ます。これらのプロパティは、 `VisualElement` x 軸と y 軸のそれぞれに対してスケールを設定します。

## <a name="bindings-and-collections"></a>バインドとコレクション

XAML とデータバインディングの機能が、テンプレート化されたよりも優れていることは何もありません `ListView` 。

`ListView``ItemsSource`型のプロパティを定義 `IEnumerable` し、そのコレクション内の項目を表示します。 これらの項目は、任意の型のオブジェクトにすることができます。 既定では、は `ListView` `ToString` 各項目のメソッドを使用してその項目を表示します。 これが必要なものである場合もありますが、多くの場合、は `ToString` オブジェクトの完全修飾クラス名のみを返します。

ただし、コレクション内の項目は、 `ListView` から派生したクラスを含む*テンプレート*を使用することによって、任意の方法で表示でき `Cell` ます。 テンプレートは、のすべての項目に対して複製され、 `ListView` テンプレートに設定されているデータバインディングが個々の複製に転送されます。

多くの場合、クラスを使用してこれらの項目のカスタムセルを作成する必要があり `ViewCell` ます。 このプロセスはコードの中で少し乱雑になりますが、XAML では非常に単純になります。

XamlSamples プロジェクトには、というクラスが含まれてい `NamedColor` ます。 各 `NamedColor` オブジェクトには、 `Name` `FriendlyName` 型のプロパティとプロパティ、 `string` および型のプロパティがあり `Color` `Color` ます。 さらに、に `NamedColor` は、 `Color` クラスで定義されている色に対応する型の静的な読み取り専用フィールドが141されてい Xamarin.Forms `Color` ます。 静的コンストラクターは、 `IEnumerable<NamedColor>` これらの静的フィールドに対応するオブジェクトを格納するコレクションを作成 `NamedColor` し、それをパブリックな静的プロパティに割り当て `All` ます。

の静的プロパティは、 `NamedColor.All` `ItemsSource` `ListView` マークアップ拡張機能を使用して簡単に設定でき `x:Static` ます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.ListViewDemoPage"
             Title="ListView Demo Page">

    <ListView ItemsSource="{x:Static local:NamedColor.All}" />

</ContentPage>
```

結果の表示では、項目が真の型であることが確立され `XamlSamples.NamedColor` ます。

[![コレクションへのバインド](data-binding-basics-images/listview1.png)](data-binding-basics-images/listview1-large.png#lightbox)

これはあまり多くの情報ではありませんが、 `ListView` スクロールと選択が可能です。

項目のテンプレートを定義するには、プロパティを `ItemTemplate` プロパティ要素として分割し、それをに設定します。これにより、が `DataTemplate` 参照さ `ViewCell` れます。 `View`のプロパティには、 `ViewCell` 各項目を表示する1つ以上のビューのレイアウトを定義できます。 次に単純な例を示します。

```xaml
<ListView ItemsSource="{x:Static local:NamedColor.All}">
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <ViewCell.View>
                    <Label Text="{Binding FriendlyName}" />
                </ViewCell.View>
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

> [!NOTE]
> セルのバインドソースとセルの子は、 `ListView.ItemsSource` コレクションです。

`Label`要素は `View` 、のプロパティに設定され `ViewCell` ます。 ( `ViewCell.View` プロパティはの content プロパティであるため、タグは必要ありません `View` `ViewCell` )。このマークアップでは、 `FriendlyName` 各オブジェクトのプロパティが表示され `NamedColor` ます。

[![System.windows.datatemplate> を使用してコレクションにバインドする](data-binding-basics-images/listview2.png)](data-binding-basics-images/listview2-large.png#lightbox)

ずっといいです。 ここで必要なのは、項目テンプレートを整理して、詳細情報と実際の色を設定することだけです。 このテンプレートをサポートするために、ページのリソースディクショナリでいくつかの値とオブジェクトが定義されています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.ListViewDemoPage"
             Title="ListView Demo Page">

    <ContentPage.Resources>
        <ResourceDictionary>
            <OnPlatform x:Key="boxSize"
                        x:TypeArguments="x:Double">
                <On Platform="iOS, Android, UWP" Value="50" />
            </OnPlatform>

            <OnPlatform x:Key="rowHeight"
                        x:TypeArguments="x:Int32">
                <On Platform="iOS, Android, UWP" Value="60" />
            </OnPlatform>

            <local:DoubleToIntConverter x:Key="intConverter" />

        </ResourceDictionary>
    </ContentPage.Resources>

    <ListView ItemsSource="{x:Static local:NamedColor.All}"
              RowHeight="{StaticResource rowHeight}">
        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <StackLayout Padding="5, 5, 0, 5"
                                 Orientation="Horizontal"
                                 Spacing="15">

                        <BoxView WidthRequest="{StaticResource boxSize}"
                                 HeightRequest="{StaticResource boxSize}"
                                 Color="{Binding Color}" />

                        <StackLayout Padding="5, 0, 0, 0"
                                     VerticalOptions="Center">

                            <Label Text="{Binding FriendlyName}"
                                   FontAttributes="Bold"
                                   FontSize="Medium" />

                            <StackLayout Orientation="Horizontal"
                                         Spacing="0">
                                <Label Text="{Binding Color.R,
                                       Converter={StaticResource intConverter},
                                       ConverterParameter=255,
                                       StringFormat='R={0:X2}'}" />

                                <Label Text="{Binding Color.G,
                                       Converter={StaticResource intConverter},
                                       ConverterParameter=255,
                                       StringFormat=', G={0:X2}'}" />

                                <Label Text="{Binding Color.B,
                                       Converter={StaticResource intConverter},
                                       ConverterParameter=255,
                                       StringFormat=', B={0:X2}'}" />
                            </StackLayout>
                        </StackLayout>
                    </StackLayout>
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>
```

の使用により、 `OnPlatform` 行のサイズと高さが定義されていることに注意して `BoxView` `ListView` ください。 すべてのプラットフォームの値は同じですが、マークアップを使用すると、他の値で表示を微調整することが簡単にできます。

## <a name="binding-value-converters"></a>値コンバーターのバインド

前の**ListView Demo** XAML ファイルには、 `R` 構造体の個々の、、およびプロパティが表示され `G` `B` Xamarin.Forms `Color` ます。 これらのプロパティは `double` 、0から1までの型および範囲です。 16進値を表示する場合は、単に `StringFormat` "X2" 書式指定を使用することはできません。 これは整数に対してのみ機能し、以外の `double` 値を255で乗算する必要があります。

この小さな問題は、*バインディングコンバーター*とも呼ばれる*値コンバーター*を使用して解決されました。 これは、インターフェイスを実装するクラスです。これは、 `IValueConverter` とという2つのメソッドがあることを意味し `Convert` `ConvertBack` ます。 メソッドは、 `Convert` 値がソースからターゲットに転送されるときに呼び出されます。 `ConvertBack` またはバインディングでターゲットからソースへの転送に対してメソッドが呼び出され `OneWayToSource` `TwoWay` ます。

```csharp
using System;
using System.Globalization;
using Xamarin.Forms;

namespace XamlSamples
{
    class DoubleToIntConverter : IValueConverter
    {
        public object Convert(object value, Type targetType,
                              object parameter, CultureInfo culture)
        {
            double multiplier;

            if (!Double.TryParse(parameter as string, out multiplier))
                multiplier = 1;

            return (int)Math.Round(multiplier * (double)value);
        }

        public object ConvertBack(object value, Type targetType,
                                  object parameter, CultureInfo culture)
        {
            double divider;

            if (!Double.TryParse(parameter as string, out divider))
                divider = 1;

            return ((double)(int)value) / divider;
        }
    }
}
```

`ConvertBack`バインドがソースからターゲットへの唯一の方法であるため、このプログラムではメソッドは役割を果たしません。

バインディングは、プロパティを使用してバインディングコンバーターを参照し `Converter` ます。 バインディングコンバーターは、プロパティで指定されたパラメーターを受け取ることもでき `ConverterParameter` ます。 いくつかの汎用性のために、これが乗数の指定方法です。 バインディングコンバーターは、コンバーターのパラメーターに有効な値があるかどうかを確認し `double` ます。

コンバーターはリソースディクショナリでインスタンス化されるため、複数のバインディング間で共有できます。

```xaml
<local:DoubleToIntConverter x:Key="intConverter" />
```

この1つのインスタンスを参照するデータバインディングは3つあります。 マークアップ拡張機能に、 `Binding` 埋め込み `StaticResource` マークアップ拡張機能が含まれていることに注意してください。

```xaml
<Label Text="{Binding Color.R,
                      Converter={StaticResource intConverter},
                      ConverterParameter=255,
                      StringFormat='R={0:X2}'}" />
```

結果は次のようになります。

[![System.windows.datatemplate> およびコンバーターを使用したコレクションへのバインド](data-binding-basics-images/listview3.png)](data-binding-basics-images/listview3-large.png#lightbox)

は、 `ListView` 基になるデータで動的に発生する可能性がある変更を処理するうえで非常に洗練されていますが、特定の手順を実行する場合に限られます。 のプロパティに割り当てられた項目のコレクションが `ItemsSource` `ListView` 実行時に変更された場合 (つまり、項目をコレクションに追加またはコレクションから削除できる場合) は、 `ObservableCollection` これらの項目に対してクラスを使用します。 `ObservableCollection``INotifyCollectionChanged`インターフェイスを実装し、 `ListView` イベントのハンドラーをインストールし `CollectionChanged` ます。

実行時に項目のプロパティが変更された場合、コレクション内の項目はインターフェイスを実装 `INotifyPropertyChanged` し、イベントを使用してプロパティ値の変更を通知する必要があり `PropertyChanged` ます。 これについては、このシリーズの第5部で説明されてい[ます。データバインドから MVVM へ](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)。

## <a name="summary"></a>[概要]

データバインディングは、ページ内の2つのオブジェクト間、またはビジュアルオブジェクトと基になるデータの間でプロパティをリンクするための強力なメカニズムを提供します。 しかし、アプリケーションがデータソースの操作を開始すると、一般的なアプリケーションアーキテクチャパターンが有用なパラダイムとして浮上し始めます。 これについては、[パート5で説明します。データバインドから MVVM へ](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)。

## <a name="related-links"></a>関連リンク

- [XamlSamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [パート1。XAML を使用したはじめに (サンプル)](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [第2部。必須の XAML 構文 (サンプル)](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [パート3。XAML マークアップ拡張機能 (サンプル)](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [パート5。データバインディングから MVVM (サンプル)](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
