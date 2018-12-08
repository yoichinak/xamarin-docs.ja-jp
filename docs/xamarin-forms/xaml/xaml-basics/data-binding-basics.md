---
title: パート 4 です。 データ バインディングの基礎
description: データ バインドでは、ニォュォケォネェャ変ェ、他のいずれかが変更されるようにリンクする 2 つのオブジェクトのプロパティを許可します。
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 342288C3-BB4C-4924-B178-72E112D777BA
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
ms.openlocfilehash: bd13163b513ea1f6b0381e99e65d0bd727f97735
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53055729"
---
# <a name="part-4-data-binding-basics"></a>パート 4 です。 データ バインディングの基礎

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)

_データ バインドでは、ニォュォケォネェャ変ェ、他のいずれかが変更されるようにリンクする 2 つのオブジェクトのプロパティを許可します。これは非常に貴重なツールでは、およびショートカットと利便性全体をコードでは、データ バインドを定義できます、XAML を提供します。その結果、Xamarin.Forms で最も重要なマークアップ拡張機能の 1 つはバインドです。_

## <a name="data-bindings"></a>データ バインディング

データ バインドと呼ばれる 2 つのオブジェクトのプロパティを接続、*ソース*と*ターゲット*します。 コードでは、2 つの手順が必要です。`BindingContext`ソース オブジェクトにターゲット オブジェクトのプロパティを設定する必要があります、`SetBinding`メソッド (と組み合わせて使用して多くの場合、`Binding`クラス) プロパティをバインドする対象のオブジェクトで呼び出す必要がありますソース オブジェクトのプロパティにオブジェクト。

ターゲット プロパティは、バインド可能なプロパティは、ターゲット オブジェクトをする必要がありますから派生させることである必要があります`BindableObject`します。 Xamarin.Forms のオンライン ドキュメントでは、どのプロパティは、バインド可能なプロパティを示します。 プロパティの`Label`など`Text`バインド可能なプロパティに関連付けられた`TextProperty`します。

マークアップでは、コードでは、必要な 2 つの手順も実行する必要がある点を除いて、`Binding`のマークアップ拡張機能に代わる、`SetBinding`を呼び出すと、`Binding`クラス。

ただし、XAML でデータ バインドを定義する場合があります設定する複数の方法、`BindingContext`のターゲット オブジェクト。 使用する、分離コード ファイルから設定されても、`StaticResource`または`x:Static`マークアップ拡張機能とのコンテンツとしても`BindingContext`プロパティ要素タグ。

バインド最もよく使用される MVVM (モデル-ビュー-ビューモデル) アプリケーション アーキテクチャの実現化では、通常、基になるデータ モデルをプログラムのビジュアルを接続するで説明したよう[パート 5 です。MVVM へのデータ バインディングから](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)が、その他のシナリオが考えられます。

## <a name="view-to-view-bindings"></a>ビューからビューへのバインド

リンクの同じページ上の 2 つのビューのプロパティへのデータ バインディングを定義できます。 この場合、設定、`BindingContext`ターゲット オブジェクトを使用して、`x:Reference`マークアップ拡張機能。

含む XAML ファイルをここでは、`Slider`と 2 つ`Label`ビュー、うちの 1 つは、回転、`Slider`値とが表示されます、`Slider`値。

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

`Slider`が含まれています、 `x:Name` 2 によって参照される属性`Label`ビューを使用して、`x:Reference`マークアップ拡張機能。

`x:Reference`という名前のプロパティを定義するバインド拡張機能`Name`ここで参照先の要素の名前に設定する`slider`します。 ただし、`ReferenceExtension`クラスを定義する、`x:Reference`マークアップ拡張機能にも定義されています、`ContentProperty`属性`Name`、明示的に必要ないことを意味します。 、さまざまな最初の`x:Reference`が含まれています"名前 ="しますが、2 つ目は使用できません。

```csharp
BindingContext="{x:Reference Name=slider}"
…
BindingContext="{x:Reference slider}"
```

`Binding`マークアップ拡張機能自体と同じようにいくつかのプロパティを持つことができます、`BindingBase`と`Binding`クラス。 `ContentProperty`の`Binding`は`Path`が、"パス ="パスが最初の項目である場合、マークアップ拡張機能の一部を省略できます、`Binding`マークアップ拡張機能。 最初の例では"パス ="しますが、2 番目の例で省略されます。

```csharp
Rotation="{Binding Path=Value}"
…
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

プロパティは、1 行で指定できます。 または複数行に分けられます。

```csharp
Text="{Binding Value,
               StringFormat='The angle is {0:F0} degrees'}"
```

便利な操作を行います。

通知、`StringFormat`プロパティの 1 秒間`Binding`マークアップ拡張機能。 Xamarin.Forms でのバインドは行いません、暗黙的な型変換と型コンバーターを提供または使用する必要がありますを文字列として文字列以外のオブジェクトを表示する必要がある場合`StringFormat`します。 背後では、静的な`String.Format`メソッドを実装するために使用`StringFormat`します。 .NET 書式設定の仕様はマークアップ拡張機能を区切るためにも使用すると、中かっこも含まれるので可能性のある問題です。 これには、XAML パーサーの混乱を招く危険性が作成されます。 これを避けるには、単一引用符で書式設定文字列全体を配置します。

```csharp
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

実行中のプログラムを次に示します。

[![](data-binding-basics-images/sliderbinding.png "ビューからビューへのバインド")](data-binding-basics-images/sliderbinding-large.png#lightbox "ビューからビューへのバインド ")

## <a name="the-binding-mode"></a>バインド モード

1 つのビューには、そのプロパティのいくつかのデータ バインドをことができます。 ただし、各ビューで保持できる 1 つだけ`BindingContext`そのビューで複数のデータ バインドでは、すべて必要がありますので、同じオブジェクトのプロパティを参照します。

この問題やその他の問題の解決策では、`Mode`プロパティのメンバーに設定されている、`BindingMode`列挙体。

- `Default`
- `OneWay` -値は、ソースからターゲットに転送されます
- `OneWayToSource` -値は、ターゲットからソースに転送されます
- `TwoWay` — ソースとターゲット間の値に両方の方法が転送されます。

次のプログラムの 1 つの一般的な用途を示します、`OneWayToSource`と`TwoWay`バインディング モード。 4 つ`Slider`ビューは、コントロール、 `Scale`、 `Rotate`、 `RotateX`、および`RotateY`のプロパティを`Label`します。 最初、見えますとしてこれら 4 つのプロパティの`Label`各によって設定されるため、データ バインドのターゲットがあります、`Slider`します。 ただし、`BindingContext`の`Label`は 1 つだけのオブジェクトとは異なる 4 つのスライダーがあります。

そのため、すべてのバインディング設定一見下位方法:`BindingContext`の 4 つのスライダーのそれぞれに設定されて、`Label`とに、バインドが設定されて、`Value`スライダーのプロパティ。 使用して、`OneWayToSource`と`TwoWay`モードでは、これら`Value`プロパティは、ソースのプロパティを設定できます、 `Scale`、 `Rotate`、 `RotateX`、および`RotateY`のプロパティ、 `Label`:

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

3 つのバインド、`Slider`ビューは`OneWayToSource`つまりを`Slider`値のプロパティを変更すると、その`BindingContext`、これは、`Label`という名前`label`します。 これらの 3 つ`Slider`ビューへの変更が発生する、 `Rotate`、 `RotateX`、および`RotateY`のプロパティ、`Label`します。

ただし、バインド、`Scale`プロパティは`TwoWay`します。 これは、ため、`Scale`プロパティを使用して、1 つの既定値を持つ、`TwoWay`バインド エラーの原因、`Slider`初期値を 0 ではなく 1 に設定しています。 場合、そのバインドが`OneWayToSource`、`Scale`プロパティから 0 に初期設定は、`Slider`既定値。 `Label`見えない、およびユーザーにによって混乱が発生する可能性があります。

 [![](data-binding-basics-images/slidertransforms.png "下位のバインド")](data-binding-basics-images/slidertransforms-large.png#lightbox "下位のバインド")

 > [!NOTE]
 > [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)クラスもあります[ `ScaleX` ](xref:Xamarin.Forms.VisualElement.ScaleX)と[ `ScaleY` ](xref:Xamarin.Forms.VisualElement.ScaleY)スケールのプロパティ、 `VisualElement` x 軸と y 軸にそれぞれします。

## <a name="bindings-and-collections"></a>バインドとコレクション

強力な XAML とデータ バインディング、テンプレート化されたより紹介 nothing`ListView`します。

`ListView` 定義、`ItemsSource`型のプロパティ`IEnumerable`、そのコレクション内の項目を表示します。 これらのアイテムには、任意の型のオブジェクトを指定できます。 既定では、`ListView`を使用して、`ToString`その項目を表示するには、各項目のメソッド。 こそが目的の場合によっては、多くの場合、`ToString`オブジェクトの完全修飾クラス名のみを返します。

ただし、内の項目、`ListView`コレクションを使用するとし、希望する方法を表示できる、*テンプレート*から派生したクラスが含まれます`Cell`します。 テンプレートは各項目の複製、 `ListView`、され、テンプレートに設定されているデータ バインドは、個々 のクローンに転送します。

カスタムのセルを使用してこれらの項目を作成したい非常に多くの場合、`ViewCell`クラス。 このプロセスがコードでは、やや面倒ですが、XAML でが非常に簡単です。

プロジェクトがという名前のクラスを XamlSamples に含まれる`NamedColor`します。 各`NamedColor`オブジェクトが`Name`と`FriendlyName`型のプロパティ`string`、および`Color`型のプロパティ`Color`します。 さらに、`NamedColor`型の 141 静的読み取り専用のフィールドを持つ`Color`、Xamarin.Forms で定義された色に対応する`Color`クラス。 静的コンス トラクターを作成、`IEnumerable<NamedColor>`を含むコレクション`NamedColor`オブジェクトをこれらの静的フィールドに対応して、そのパブリック静的に割り当てられます`All`プロパティ。

静的な設定`NamedColor.All`プロパティを`ItemsSource`の`ListView`簡単を使用して、`x:Static`マークアップ拡張機能。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.ListViewDemoPage"
             Title="ListView Demo Page">

    <ListView ItemsSource="{x:Static local:NamedColor.All}" />

</ContentPage>
```

型の項目が本当には、結果の表示の確立`XamlSamples.NamedColor`:

[![](data-binding-basics-images/listview1.png "コレクションへのバインディング")](data-binding-basics-images/listview1-large.png#lightbox "コレクションへのバインディング")

多くの情報ではありませんが、`ListView`スクロールと選択可能なのです。

項目のテンプレートを定義するために分割する、`ItemTemplate`プロパティ要素としてのプロパティに設定し、 `DataTemplate`、どの参照、 `ViewCell`。 `View`のプロパティ、`ViewCell`各項目を表示する 1 つまたは複数のビューのレイアウトを定義することができます。 簡単な例を次に示します。

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

`Label`要素に設定されて、`View`のプロパティ、`ViewCell`します。 (、`ViewCell.View`ため、タグが必要ありません、`View`プロパティは、コンテンツのプロパティの`ViewCell`)。このマークアップの表示、`FriendlyName`の各プロパティ`NamedColor`オブジェクト。

[![](data-binding-basics-images/listview2.png "DataTemplate でコレクションへのバインディング")](data-binding-basics-images/listview2-large.png#lightbox "DataTemplate でコレクションへのバインディング")

ずっといいです。 後のみが必要なは、詳細については、実際の色と項目テンプレートの見栄えをよくです。 このテンプレートをサポートするには、いくつかの値とオブジェクトをページのリソース ディクショナリで定義されています。

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

使用に注意してください`OnPlatform`のサイズを定義する、`BoxView`との高さ、`ListView`行。 すべてのプラットフォームの値は、同じですが、マークアップが表示を微調整するその他の値に作り変える簡単に可能性があります。

## <a name="binding-value-converters"></a>値コンバーターのバインディング

前の**ListView デモ**XAML ファイルでは、個人を表示します。 `R`、 `G`、と`B`、Xamarin.Forms のプロパティ`Color`構造体。 これらのプロパティが型`double`および 0 ~ 1 の範囲。 単に使用できません、16 進値を表示する場合は、 `StringFormat` "X2"の仕様を書式設定にします。 さらに、整数の場合にのみ機能する、`double`値は 255 で乗算する必要があります。

このささいな問題が解決を*値コンバーター*も呼ばれ、*バインディング コンバーター*します。 これは、実装するクラス、`IValueConverter`という 2 つのメソッドがあることを意味するインターフェイスを`Convert`と`ConvertBack`します。 `Convert`値がソースからターゲットに転送されたときに、メソッドが呼び出された、`ConvertBack`メソッドが転送でソース ターゲットからと呼ばれる`OneWayToSource`または`TwoWay`バインド。

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

`ConvertBack`のみであるため、バインド方法の 1 つのソースからターゲットに、メソッドがこのプログラムでロールを再生されません。

バインディングのバインディング コンバーターを参照して、`Converter`プロパティ。 バインディング コンバーターに指定したパラメーターを受け付けることも、`ConverterParameter`プロパティ。 いくつかの汎用性の乗数を指定する方法になります。 バインディング コンバーターの有効なコンバーターのパラメーターを確認します。`double`値。

コンバーターはリソース ディクショナリにインスタンス化されるので、複数のバインドの間で共有できます。

```xaml
<local:DoubleToIntConverter x:Key="intConverter" />
```

3 つのデータ バインドでは、この 1 つのインスタンスを参照します。 なお、`Binding`マークアップ拡張機能が含まれていますが、埋め込み`StaticResource`マークアップ拡張機能。

```xaml
<Label Text="{Binding Color.R,
                      Converter={StaticResource intConverter},
                      ConverterParameter=255,
                      StringFormat='R={0:X2}'}" />
```

結果を次に示します。

[![](data-binding-basics-images/listview3.png "DataTemplate とコンバーターでコレクションへのバインディング")](data-binding-basics-images/listview3-large.png#lightbox "DataTemplate とコンバーターでコレクションへのバインディング")

`ListView`がいくつかの手順を実行する場合にのみ、基になるデータで動的に発生した変更の処理では非常に洗練します。 項目のコレクションに割り当てられている場合、`ItemsSource`のプロパティ、`ListView`ランタイム中の変更-に項目を追加できる場合は、またはコレクションから削除する: を使用して、`ObservableCollection`これらの項目のクラス。 `ObservableCollection` 実装して、`INotifyCollectionChanged`インターフェイス、および`ListView`のハンドラーをインストール、`CollectionChanged`イベント。

実行時に、項目自体のプロパティを変更するかどうかは、コレクション内の項目を実装する必要があります、`INotifyPropertyChanged`を使用してプロパティ値に対する変更のインターフェイスとシグナル、`PropertyChanged`イベント。 これは、説明については、このシリーズの次の部分の[パート 5 です。MVVM へのデータ バインディングから](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)します。

## <a name="summary"></a>まとめ

データ バインドでは、プロパティ ページ内の 2 つのオブジェクト間、またはビジュアル オブジェクト間のリンクと、基になるデータの強力なメカニズムを提供します。 便利なパラダイムとして登場する一般的なアプリケーション アーキテクチャ パターンを開始、アプリケーションのデータ ソースでの作業の開始時にします。 これについては[パート 5 です。MVVM へのデータ バインディングから](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)します。



## <a name="related-links"></a>関連リンク

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [第 1 部XAML (サンプル) の概要](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [第 2 部重要な XAML 構文 (サンプル)](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [第 3 部XAML マークアップ拡張機能 (サンプル)](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [第 5 部MVVM (サンプル) へのデータ バインディングから](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
