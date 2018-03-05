---
title: "パート 4 です。 データのバインドの基礎"
description: "データ バインディングを使用すると、変更を他のいずれかで変更できるようにリンクする 2 つのオブジェクトのプロパティ。 これは非常に重要なツールであり、すべてコードでのデータ バインドを定義する XAML 提供ショートカットと利便性。 その結果、Xamarin.Forms で最も重要なマークアップ拡張機能の 1 つはバインディングします。"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: 46e0c1f9b2aff52c1d31774a15e818c78a70056a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="part-4-data-binding-basics"></a>パート 4 です。 データのバインドの基礎

_データ バインディングを使用すると、変更を他のいずれかで変更できるようにリンクする 2 つのオブジェクトのプロパティ。これは非常に重要なツールであり、すべてコードでのデータ バインドを定義する XAML 提供ショートカットと利便性。その結果、Xamarin.Forms で最も重要なマークアップ拡張機能の 1 つはバインディングします。_

## <a name="data-bindings"></a>データ バインディング

データ バインディングと呼ばれる 2 つのオブジェクトのプロパティを接続する、*ソース*と*ターゲット*です。 2 つの手順が必要なコードでは、:`BindingContext`ターゲット オブジェクトのプロパティは、ソース オブジェクトに設定する必要があります、`SetBinding`メソッド (と共にで頻繁に使用、`Binding`クラス) のプロパティにバインドする対象のオブジェクトで呼び出す必要がありますソース オブジェクトのプロパティをオブジェクトです。

ターゲット プロパティは、バインド可能なプロパティは、ターゲット オブジェクトから派生しなければならないことを意味する必要があります`BindableObject`です。 Xamarin.Forms のオンライン ドキュメントでは、どのプロパティがバインド可能なプロパティを示します。 プロパティ`Label`など`Text`バインド可能なプロパティに関連付けられた`TextProperty`です。

マークアップでは、コードでは、必要な 2 つの手順も実行する必要がある点を除いて、`Binding`マークアップ拡張機能に代わる、`SetBinding`を呼び出すと、`Binding`クラスです。

ただし、XAML のデータ バインディングを定義するときにも設定するいくつかの方法、`BindingContext`のターゲット オブジェクト。 使用する、分離コード ファイルから設定されている場合があります、`StaticResource`または`x:Static`マークアップ拡張機能とのコンテンツとしても`BindingContext`プロパティ要素タグ。

バインディングは、最も頻繁に使用 MVVM (モデル-ビュー-ViewModel) アプリケーションのアーキテクチャの実現の通常の基になるデータ モデルと、プログラムのビジュアルを接続するで説明したよう[パート 5 です。MVVM をデータ バインドから](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)が、その他のシナリオが可能です。

## <a name="view-to-view-bindings"></a>ビューをバインド

リンクは同じページ上の 2 つのビューのプロパティへのデータ バインディングを定義することができます。 この場合、設定、`BindingContext`ターゲット オブジェクトを使用して、、`x:Reference`マークアップ拡張機能です。

含む XAML ファイルをここでは、`Slider`と 2 つ`Label`だけ回転うちの 1 つのビュー、`Slider`値とが表示されます、`Slider`値。

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

`Slider`が含まれています、 `x:Name` 2 によって参照される属性`Label`ビューを使用して、`x:Reference`マークアップ拡張機能です。

`x:Reference`という名前のプロパティ バインド拡張機能を定義`Name`をここでは、参照される要素の名前に設定する`slider`です。 ただし、`ReferenceExtension`を定義するクラス、`x:Reference`マークアップ拡張機能も定義、`ContentProperty`属性`Name`、明示的に必要なことができないためです。 さまざまな、についてのみ、最初`x:Reference`が含まれています"名前 ="しますが、2 つ目は使用できません。

```csharp
BindingContext="{x:Reference Name=slider}"
…
BindingContext="{x:Reference slider}"
```

`Binding`自体のマークアップ拡張機能は同じようにいくつかのプロパティを持つことができます、`BindingBase`と`Binding`クラスです。 `ContentProperty`の`Binding`は`Path`、ですが、"パス ="パスが最初の項目である場合は、マークアップ拡張機能の一部を省略するとことができます、`Binding`マークアップ拡張機能です。 最初の例は、"パス ="しますが、2 番目の例では、指定を省略します。

```csharp
Rotation="{Binding Path=Value}"
…
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

プロパティは、すべて 1 つの直線上に存在できます。 または複数の行に区切られました。

```csharp
Text="{Binding Value, 
               StringFormat='The angle is {0:F0} degrees'}"
```

便利な処理を行います。

通知、`StringFormat`プロパティの 1 秒間`Binding`マークアップ拡張機能です。 Xamarin.Forms でバインドは行われず、暗黙の型変換して型コンバーターを提供または使用する必要がありますを文字列として、非文字列オブジェクトを表示する必要がある場合`StringFormat`です。 背後では、静的な`String.Format`メソッドが実装に使用される`StringFormat`です。 問題がある可能性がある、.NET の書式指定には、マークアップ拡張機能を区切るためにも使用される、中かっこが含まれまるためです。 これには、XAML パーサーの混乱を招く危険性が作成されます。 防ぐには、するには、単一引用符で全体の書式指定文字列を配置します。

```csharp
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

実行中のプログラムを次に示します。

[ ![](data-binding-basics-images/sliderbinding.png "ビューをバインド")](data-binding-basics-images/sliderbinding-large.png "ビューをバインド ")

## <a name="the-binding-mode"></a>バインド モード 

1 つのビューには、複数のプロパティにデータ バインドがあります。 ただし、各ビューで保持できる 1 つだけ`BindingContext`そのビューで複数のデータ バインディングでは、すべて必要がありますので、同じオブジェクトのプロパティを参照します。

この問題をしたり、他のソリューションに、`Mode`のメンバーに設定されているプロパティ、`BindingMode`列挙。

- `Default` 
- `OneWay` -の値は、ソースからターゲットに転送
- `OneWayToSource` 値が、ターゲットからソースに転送
- `TwoWay` — ソースとターゲット間の値に両方の方法が転送されます。

次のプログラムがの 1 つの一般的な用途を示す、`OneWayToSource`と`TwoWay`バインディング モード。 次の 4 つ`Slider`ビューがコントロールするものでは、 `Scale`、 `Rotate`、 `RotateX`、および`RotateY`のプロパティ、`Label`です。 最初は見えますとしてのこれら 4 つのプロパティ、`Label`各が設定されているために、データ バインドのターゲットをする必要があります、`Slider`です。 ただし、`BindingContext`の`Label`1 つのオブジェクトを指定でき、4 つの異なるスライダーがあります。

そのため、すべてのバインディングで設定されて一見後方方法:`BindingContext`の 4 つのスライダーのそれぞれに設定されている、 `Label`、しにバインドを設定、`Value`スライダーのプロパティです。 使用して、`OneWayToSource`と`TwoWay`モードの場合、これら`Value`プロパティは、ソースのプロパティを設定できます、 `Scale`、 `Rotate`、 `RotateX`、および`RotateY`のプロパティ、 `Label`:

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

うち 3 つのバインディング、`Slider`ビューは`OneWayToSource`つまりを`Slider`値のプロパティで変更すると、その`BindingContext`、これは、`Label`という名前`label`です。 これらの 3 つ`Slider`ビューへの変更が発生する、 `Rotate`、 `RotateX`、および`RotateY`のプロパティ、`Label`です。

ただし、バインドを`Scale`プロパティは`TwoWay`します。 これは、ため、`Scale`プロパティには、既定値を使用して、1 は、`TwoWay`原因のバインド、`Slider`初期値を 0 ではなく、1 に設定してです。 バインディングが場合`OneWayToSource`、`Scale`プロパティから 0 に初期設定は、`Slider`既定値です。 `Label`見えない、およびユーザーには混乱を招く可能性があります。

 [ ![](data-binding-basics-images/slidertransforms.png "旧バージョンとのバインド")](data-binding-basics-images/slidertransforms-large.png "逆方向のバインド")

## <a name="bindings-and-collections"></a>バインドとコレクション

XAML とデータ バインディングは、テンプレートのより強力な紹介 nothing`ListView`です。

`ListView` 定義、`ItemsSource`型のプロパティ`IEnumerable`、そのコレクションに項目を表示します。 これらの項目は、任意の型のオブジェクトを指定できます。 既定では、`ListView`を使用して、`ToString`そのアイテムを表示するには、各項目のメソッドです。 たい部分、つまり場合もありますが、多くの場合、`ToString`オブジェクトの完全修飾クラス名のみを返します。

ただし、内の項目、`ListView`コレクションを使用して、希望方法が表示されることができます、*テンプレート*から派生するクラスが含まれます`Cell`です。 各項目のテンプレートが複製されると、 `ListView`、され、テンプレートに設定されているデータ バインディングは、個々 のクローンに転送します。

非常に多くの場合は、カスタムのセルを使用してこれらの項目を作成するおく、`ViewCell`クラスです。 このプロセスは、コードでは、ある程度乱れたが XAML では非常に簡単です。

含まれて、XamlSamples プロジェクトと呼ばれるクラス`NamedColor`です。 各`NamedColor`オブジェクトが持つ`Name`と`FriendlyName`型のプロパティ`string`、および`Color`型のプロパティ`Color`です。 さらに、`NamedColor`型の 141 静的な読み取り専用のフィールドを持つ`Color`、Xamarin.Forms で定義された色に対応する`Color`クラスです。 静的コンス トラクターを作成、`IEnumerable<NamedColor>`を含むコレクション`NamedColor`オブジェクトをこれらの静的フィールドに対応して、そのパブリック静的に割り当てられます`All`プロパティです。

設定する静的`NamedColor.All`プロパティを`ItemsSource`の`ListView`やすいを使用して、`x:Static`マークアップ拡張機能。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.ListViewDemoPage"
             Title="ListView Demo Page">

    <ListView ItemsSource="{x:Static local:NamedColor.All}" />

</ContentPage>
```

型の真の項目が結果の表示を確立`XamlSamples.NamedColor`:

[ ![](data-binding-basics-images/listview1.png "コレクションへのバインド")](data-binding-basics-images/listview1-large.png "コレクションへのバインド")

多くの情報ではありませんが、`ListView`はスクロール可能なと選択可能です。

項目のテンプレートを定義するにしておく離れて、`ItemTemplate`プロパティ要素としてのプロパティに設定し、 `DataTemplate`、どの参照、`ViewCell`です。 `View`のプロパティ、`ViewCell`各項目を表示する 1 つまたは複数のビューのレイアウトを定義することができます。 単純な例を次に示します。

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

`Label`要素に設定されて、`View`のプロパティ、`ViewCell`です。 (、`ViewCell.View`タグ不要なため、`View`プロパティのコンテンツ プロパティは、 `ViewCell`)。このマークアップを表示、`FriendlyName`の各プロパティ`NamedColor`オブジェクト。

[ ![](data-binding-basics-images/listview2.png "DataTemplate を含むコレクションへのバインド")](data-binding-basics-images/listview2-large.png "DataTemplate を含むコレクションへのバインド")

ずっといいです。 今すぐ必要な詳細については、実際の色と項目テンプレートを一新します。 このテンプレートをサポートするには、いくつかの値とオブジェクトをページのリソース ディクショナリで定義されています。

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

    <ListView ItemsSource="{x:Static local:NamedColor.All}">
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

使用に注意してください`OnPlatform`のサイズを定義する、`BoxView`との高さ、`ListView`行です。 3 つすべてのプラットフォームの値は、同じですが、マークアップがの表示を微調整するその他の値に合わせて変更簡単に可能性があります。 

## <a name="binding-value-converters"></a>バインディングの値コンバーター

前の**ListView デモ**XAML ファイルは、個人、表示`R`、 `G`、および`B`、Xamarin.Forms のプロパティ`Color`構造体。 これらのプロパティが型`double`と 0 ~ 1 の範囲です。 16 進数値を表示するには場合、単には使用できません`StringFormat`"X2"仕様を書式設定にします。 整数およびだけでなくにのみ機能する、`double`値は 255 で乗算する必要があります。

この小さな問題が解決、*値コンバーター*もという、*バインディング コンバーター*です。 これは、クラスを実装するクラス、`IValueConverter`という 2 つのメソッドがあることを意味するインターフェイス`Convert`と`ConvertBack`です。 `Convert`メソッドは値がソースからターゲットに転送されるとき、`ConvertBack`メソッドが呼び出されて転送のターゲットからソース`OneWayToSource`または`TwoWay`バインド。

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

`ConvertBack`のみであるため、バインド方法の 1 つのソースからターゲットに、メソッドがこのプログラムでロールを再生できません。 

バインディングとバインド コンバーターを参照して、`Converter`プロパティです。 バインディングのコンバーターで指定されたパラメーターも使用できます、`ConverterParameter`プロパティです。 一部の汎用性のこれは、乗数を指定する方法。 バインディング コンバーター チェックの有効なコンバーター パラメーター`double`値。

コンバーターがリソース ディクショナリでインスタンス化されるため、複数のバインディング間で共有することができます。

```xaml
<local:DoubleToIntConverter x:Key="intConverter" />
```

3 つのデータ バインディングは、この単一のインスタンスを参照します。 注意して、`Binding`マークアップ拡張機能が含まれていますが、埋め込み`StaticResource`マークアップ拡張機能。

```xaml
<Label Text="{Binding Color.R,
                      Converter={StaticResource intConverter},
                      ConverterParameter=255,
                      StringFormat='R={0:X2}'}" />
```

結果を次に示します。

[ ![](data-binding-basics-images/listview3.png "DataTemplate とコンバーター コレクションへのバインド")](data-binding-basics-images/listview3-large.png "DataTemplate とコンバーター コレクションへのバインド")

`ListView`は、基になるで動的に発生する可能性の変更の処理に非常に高度な場合に限り、データは、特定の手順を実行します。 項目のコレクションに割り当てられている場合、`ItemsSource`のプロパティ、`ListView`実行時に変更 — に項目を追加できる場合は、またはコレクションから削除する: を使用して、`ObservableCollection`これらのアイテムのクラスです。 `ObservableCollection` 実装する、`INotifyCollectionChanged`インターフェイス、および`ListView`のハンドラーをインストール、`CollectionChanged`イベント。

実行時に、項目自体のプロパティを変更する場合、コレクション内の項目を実装する必要があります、`INotifyPropertyChanged`インターフェイスとシグナル値の変更にプロパティを使用して、`PropertyChanged`イベント。 これは、このシリーズの次の部分で示されて[パート 5 です。MVVM へのデータ バインディングから](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)です。

## <a name="summary"></a>まとめ

データ バインディングは、プロパティ ページ内の 2 つのオブジェクト間、または visual オブジェクト間のリンクと、データを基になる強力なメカニズムを提供します。 アプリケーションでは、データ ソースに関する作業を開始、一般的なアプリケーション アーキテクチャ パターン開始の有効なパラダイムとして出現します。 これは、「[パート 5 です。MVVM をデータ バインドから](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)です。



## <a name="related-links"></a>関連リンク

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [第 1 部XAML (サンプル) の概要](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [第 2 部重要なは、XAML 構文 (サンプル)](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [第 3 部XAML マークアップ拡張機能 (サンプル)](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [第 5 部MVVM (サンプル) へのデータ バインディング](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
