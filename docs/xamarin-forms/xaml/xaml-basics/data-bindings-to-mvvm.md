---
title: 第 5 部 データ バインディングから MVVM まで
description: MVVM パターンは次の 3 つのソフトウェア レイヤーを分離を強制-ビューと呼ばれる、XAML ユーザー インターフェイスモデルと呼ばれる、基になるデータされ、ビューと、モデルの中間には、ViewModel が呼び出されます。
ms.prod: xamarin
ms.custom: video
ms.assetid: 48B37D44-4FB1-41B2-9A5E-6D383B041F81
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
ms.openlocfilehash: 1a6ab1393cbcd8224411aeea2af2aca27381bba3
ms.sourcegitcommit: 52fb214c0e0243587d4e9ad9306b75e92a8cc8b7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2020
ms.locfileid: "76940367"
---
# <a name="part-5-from-data-bindings-to-mvvm"></a>第 5 部 データ バインディングから MVVM まで

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_モデルビュービューモデル (MVVM) アーキテクチャパターンは、XAML を念頭に置いて考案されました。このパターンでは、3つのソフトウェアレイヤー (つまり、ビューと呼ばれる XAML ユーザーインターフェイス) の分離が適用されます。モデルと呼ばれる基になるデータ。ビューとモデルの間の中間点として、ビューモデルと呼ばれるものがあります。ビューとビューモデルは、多くの場合、XAML ファイルで定義されているデータバインディングを使用して接続されます。ビューの BindingContext は、通常、ビューモデルのインスタンスです。_

## <a name="a-simple-viewmodel"></a>単純な ViewModel

として Viewmodel の概要については、最初に見てみましょうせず 1 つのプログラム。
以前に他のアセンブリ参照クラスの XAML ファイルを許可する新しい XML 名前空間宣言を定義する方法を説明しました。 XML 名前空間宣言を定義するプログラムを次に示します、`System`名前空間。

```csharp
xmlns:sys="clr-namespace:System;assembly=netstandard"
```

プログラムで使用できます`x:Static`、静的なから現在の日付と時刻を取得する`DateTime.Now`プロパティを設定し、`DateTime`値を`BindingContext`で、 `StackLayout`:

```xaml
<StackLayout BindingContext="{x:Static sys:DateTime.Now}" …>
```

`BindingContext` は特殊なプロパティです。要素で `BindingContext` を設定すると、その要素のすべての子によって継承されます。 つまり、すべての子、`StackLayout`これと同じある`BindingContext`、し、そのオブジェクトのプロパティへの単純なバインドを含めることができます。

**One-Shot DateTime**プログラムは、2 つの子のプロパティへのバインドを保持する`DateTime`バインディング パスが見つからないと思われるバインドを含めることの値はその他の 2 つの子。 つまり、`DateTime`自体の値が使用される、 `StringFormat`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=netstandard"
             x:Class="XamlSamples.OneShotDateTimePage"
             Title="One-Shot DateTime Page">

    <StackLayout BindingContext="{x:Static sys:DateTime.Now}"
                 HorizontalOptions="Center"
                 VerticalOptions="Center">

        <Label Text="{Binding Year, StringFormat='The year is {0}'}" />
        <Label Text="{Binding StringFormat='The month is {0:MMMM}'}" />
        <Label Text="{Binding Day, StringFormat='The day is {0}'}" />
        <Label Text="{Binding StringFormat='The time is {0:T}'}" />

    </StackLayout>
</ContentPage>
```

問題は、ページが最初に作成されたときに日付と時刻が1回設定され、変更されないことです。

[![](data-bindings-to-mvvm-images/oneshotdatetime.png "View Displaying Date and Time")](data-bindings-to-mvvm-images/oneshotdatetime-large.png#lightbox "View Displaying Date and Time")

XAML ファイルには、常に現在の時刻を示すクロックが表示されますが、いくつかのコードが必要です。MVVM の観点から考えると、モデルとビューモデルはコードで完全に記述されたクラスです。 このビューは、データ バインディングによって、ビューモデルで定義されたプロパティを参照する XAML ファイルでは多くの場合です。

適切なモデルでは、ViewModel を意識し、適切な ViewModel はビューを意識しません。 ただし、多くの場合、プログラマは、ビューモデルによって公開されるデータ型を、特定のユーザーインターフェイスに関連付けられたデータ型に合わせて並べ替えます。 たとえば、モデルが 8 ビット文字の ASCII 文字列を格納しているデータベースにアクセスする場合、ViewModel 必要がありますを排他的に使用するユーザー インターフェイスで Unicode に対応する Unicode 文字列にそれらの文字列に変換します。

(ここで示すもの) などの MVVM の単純な例で多くの場合、モデルがない、すべてのパターンでは、ビューだけと ViewModel は、データ バインディングにリンクされています。

次に示すのは、`DateTime`という名前の1つのプロパティだけを持つクロックのビューモデルです。この `DateTime` プロパティを1秒ごとに更新します。

```csharp
using System;
using System.ComponentModel;
using Xamarin.Forms;

namespace XamlSamples
{
    class ClockViewModel : INotifyPropertyChanged
    {
        DateTime dateTime;

        public event PropertyChangedEventHandler PropertyChanged;

        public ClockViewModel()
        {
            this.DateTime = DateTime.Now;

            Device.StartTimer(TimeSpan.FromSeconds(1), () =>
                {
                    this.DateTime = DateTime.Now;
                    return true;
                });
        }

        public DateTime DateTime
        {
            set
            {
                if (dateTime != value)
                {
                    dateTime = value;

                    if (PropertyChanged != null)
                    {
                        PropertyChanged(this, new PropertyChangedEventArgs("DateTime"));
                    }
                }
            }
            get
            {
                return dateTime;
            }
        }
    }
}
```

一般に Viewmodel を実装、`INotifyPropertyChanged`クラスが発生することを意味するインターフェイスを`PropertyChanged`そのプロパティの 1 つが変更されるたびにイベント。 Xamarin.Forms でのデータ バインド メカニズムでは、これにハンドラーをアタッチ`PropertyChanged`イベント プロパティが変更されたときに通知できるようにして、ターゲットが新しい値で更新します。

このビューモデルに基づいてクロックはようにシンプルになります。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.ClockPage"
             Title="Clock Page">

    <Label Text="{Binding DateTime, StringFormat='{0:T}'}"
           FontSize="Large"
           HorizontalOptions="Center"
           VerticalOptions="Center">
        <Label.BindingContext>
            <local:ClockViewModel />
        </Label.BindingContext>
    </Label>
</ContentPage>
```

通知方法、`ClockViewModel`に設定されている、`BindingContext`の`Label`プロパティ要素タグを使用します。 または、インスタンス化できます、`ClockViewModel`で、`Resources`コレクションに設定し、`BindingContext`を使用して、`StaticResource`マークアップ拡張機能。 または、分離コード ファイルには、ViewModel がインスタンス化できます。

`Binding`でマークアップ拡張機能、`Text`のプロパティ、`Label`形式、`DateTime`プロパティ。 表示を次に示します。

[![](data-bindings-to-mvvm-images/clock.png "View Displaying Date and Time via ViewModel")](data-bindings-to-mvvm-images/clock-large.png#lightbox "View Displaying Date and Time via ViewModel")

個々 のプロパティにアクセスすることも、`DateTime`プロパティをピリオドで区切って ViewModel のプロパティ。

```xaml
<Label Text="{Binding DateTime.Second, StringFormat='{0}'}" … >
```

## <a name="interactive-mvvm"></a>対話型の MVVM

MVVM は、基になるデータモデルに基づく対話型ビューに対して、双方向のデータバインディングと共に使用されることがよくあります。

という名前のクラスを次に示します`HslViewModel`に変換する、`Color`値に`Hue`、 `Saturation`、および`Luminosity`値、およびその逆の場合。

```csharp
using System;
using System.ComponentModel;
using Xamarin.Forms;

namespace XamlSamples
{
    public class HslViewModel : INotifyPropertyChanged
    {
        double hue, saturation, luminosity;
        Color color;

        public event PropertyChangedEventHandler PropertyChanged;

        public double Hue
        {
            set
            {
                if (hue != value)
                {
                    hue = value;
                    OnPropertyChanged("Hue");
                    SetNewColor();
                }
            }
            get
            {
                return hue;
            }
        }

        public double Saturation
        {
            set
            {
                if (saturation != value)
                {
                    saturation = value;
                    OnPropertyChanged("Saturation");
                    SetNewColor();
                }
            }
            get
            {
                return saturation;
            }
        }

        public double Luminosity
        {
            set
            {
                if (luminosity != value)
                {
                    luminosity = value;
                    OnPropertyChanged("Luminosity");
                    SetNewColor();
                }
            }
            get
            {
                return luminosity;
            }
        }

        public Color Color
        {
            set
            {
                if (color != value)
                {
                    color = value;
                    OnPropertyChanged("Color");

                    Hue = value.Hue;
                    Saturation = value.Saturation;
                    Luminosity = value.Luminosity;
                }
            }
            get
            {
                return color;
            }
        }

        void SetNewColor()
        {
            Color = Color.FromHsla(Hue, Saturation, Luminosity);
        }

        protected virtual void OnPropertyChanged(string propertyName)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

変更、 `Hue`、`Saturation`と`Luminosity`プロパティ原因、`Color`プロパティを変更して変更`Color`とその他の 3 つのプロパティを変更します。 これは、プロパティが変更されていない限り、クラスが `PropertyChanged` イベントを呼び出さない点を除いて、無限ループのように思えるかもしれません。 これにより、それ以外の場合に制御不能のフィードバック ループします。

次の XAML ファイルが含まれています、`BoxView`が`Color`プロパティにバインドする、`Color`ビューモデル、および 3 つのプロパティ`Slider`と 3 つ`Label`にバインドされたビュー、 `Hue`、 `Saturation`、および`Luminosity`プロパティ。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.HslColorScrollPage"
             Title="HSL Color Scroll Page">
    <ContentPage.BindingContext>
        <local:HslViewModel Color="Aqua" />
    </ContentPage.BindingContext>

    <StackLayout Padding="10, 0">
        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}"
               HorizontalOptions="Center" />

        <Slider Value="{Binding Hue, Mode=TwoWay}" />

        <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}"
               HorizontalOptions="Center" />

        <Slider Value="{Binding Saturation, Mode=TwoWay}" />

        <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}"
               HorizontalOptions="Center" />

        <Slider Value="{Binding Luminosity, Mode=TwoWay}" />
    </StackLayout>
</ContentPage>
```

各バインド`Label`既定`OneWay`します。 のみ、値を表示する必要があります。 各バインド`Slider`は`TwoWay`します。 これにより、 `Slider` ViewModel から初期化します。 注意、`Color`プロパティに設定されて`Aqua`ビューモデルがインスタンス化されます。 変更は、`Slider`も新しい色を計算し、ViewModel のプロパティの新しい値を設定する必要があります。

[![](data-bindings-to-mvvm-images/hslcolorscroll.png "MVVM using Two-Way Data Bindings")](data-bindings-to-mvvm-images/hslcolorscroll-large.png#lightbox "MVVM using Two-Way Data Bindings")

## <a name="commanding-with-viewmodels"></a>Viewmodel のコマンド実行

多くの場合、MVVM パターンはデータ項目の操作に制限されています: ビュー内のユーザー インターフェイス オブジェクトの並列ビューモデル内のデータ オブジェクト。

ただし、必要があります、ビュー、ビューモデルでさまざまなアクションをトリガーするボタンが含まれます。 ViewModel が含めることはできませんが、`Clicked`ボタンのハンドラーが特定のユーザー インターフェイスのパラダイムに ViewModel を占有するので。

ビューモデルには、特定のユーザー インターフェイス オブジェクトの詳細独立しても、ビューモデル内で呼び出されるメソッドを許可を許可する、*コマンド*インターフェイスが存在します。 このコマンドのインターフェイスは Xamarin.Forms では、次の要素でサポートされています。

- `Button`
- `MenuItem`
- `ToolbarItem`
- `SearchBar`
- `TextCell` (つまりも`ImageCell`)
- `ListView`
- `TapGestureRecognizer`

例外です、`SearchBar`と`ListView`要素、これらの要素が 2 つのプロパティを定義します。

- `Command` 型の  `System.Windows.Input.ICommand`
- `CommandParameter` 型の  `Object`

`SearchBar`定義`SearchCommand`と`SearchCommandParameter`プロパティ、中に、`ListView`定義、`RefreshCommand`型のプロパティ`ICommand`します。

`ICommand`インターフェイスは、2 つのメソッドと 1 つのイベントを定義します。

- `void Execute(object arg)`
- `bool CanExecute(object arg)`
- `event EventHandler CanExecuteChanged`

ViewModel 型のプロパティを定義できます`ICommand`します。 これらのプロパティをバインドすることができますし、`Command`の各プロパティ`Button`またはその他の要素、またはこのインターフェイスを実装するカスタム ビューなど。 必要に応じて設定することができます、`CommandParameter`個々 に識別するためにプロパティ`Button`オブジェクト (またはその他の要素) この ViewModel のプロパティにバインドします。 内部的には、`Button`呼び出し、`Execute`メソッド、ユーザーがタップするたびに、`Button`に渡し、`Execute`メソッドその`CommandParameter`します。

`CanExecute`メソッドと`CanExecuteChanged`場合にイベントを使用場所、`Button`タップできない可能性があります、現在有効な場合、`Button`自体が無効にする必要があります。 `Button`呼び出し`CanExecute`ときに、`Command`プロパティの最初の設定や、常に、`CanExecuteChanged`イベントが発生します。 場合`CanExecute`返します`false`、`Button`自体を無効にしが生成されない`Execute`呼び出し。

Viewmodel にコマンド実行を追加する方法については、Xamarin. Forms には `ICommand`を実装する2つのクラスが定義されています。 `Command` と `Command<T>` `T` は `Execute` する引数の型であり、`CanExecute`します。 これら 2 つのクラスがいくつかのコンス トラクターを定義および`ChangeCanExecute`ビューモデルが強制的に呼び出すことができるメソッド、`Command`させるオブジェクト、`CanExecuteChanged`イベント。

電話番号を入力することが想定されている単純なキーパッドの ViewModel を次に示します。 注意、`Execute`と`CanExecute`メソッドは、ラムダ関数のコンス トラクター内で直接として定義されます。

```csharp
using System;
using System.ComponentModel;
using System.Windows.Input;
using Xamarin.Forms;

namespace XamlSamples
{
    class KeypadViewModel : INotifyPropertyChanged
    {
        string inputString = "";
        string displayText = "";
        char[] specialChars = { '*', '#' };

        public event PropertyChangedEventHandler PropertyChanged;

        // Constructor
        public KeypadViewModel()
        {
            AddCharCommand = new Command<string>((key) =>
                {
                    // Add the key to the input string.
                    InputString += key;
                });

            DeleteCharCommand = new Command(() =>
                {
                    // Strip a character from the input string.
                    InputString = InputString.Substring(0, InputString.Length - 1);
                },
                () =>
                {
                    // Return true if there's something to delete.
                    return InputString.Length > 0;
                });
        }

        // Public properties
        public string InputString
        {
            protected set
            {
                if (inputString != value)
                {
                    inputString = value;
                    OnPropertyChanged("InputString");
                    DisplayText = FormatText(inputString);

                    // Perhaps the delete button must be enabled/disabled.
                    ((Command)DeleteCharCommand).ChangeCanExecute();
                }
            }

            get { return inputString; }
        }

        public string DisplayText
        {
            protected set
            {
                if (displayText != value)
                {
                    displayText = value;
                    OnPropertyChanged("DisplayText");
                }
            }
            get { return displayText; }
        }

        // ICommand implementations
        public ICommand AddCharCommand { protected set; get; }

        public ICommand DeleteCharCommand { protected set; get; }

        string FormatText(string str)
        {
            bool hasNonNumbers = str.IndexOfAny(specialChars) != -1;
            string formatted = str;

            if (hasNonNumbers || str.Length < 4 || str.Length > 10)
            {
            }
            else if (str.Length < 8)
            {
                formatted = String.Format("{0}-{1}",
                                          str.Substring(0, 3),
                                          str.Substring(3));
            }
            else
            {
                formatted = String.Format("({0}) {1}-{2}",
                                          str.Substring(0, 3),
                                          str.Substring(3, 3),
                                          str.Substring(6));
            }
            return formatted;
        }

        protected void OnPropertyChanged(string propertyName)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

このビューモデルを前提としていますが、`AddCharCommand`プロパティにバインドする、`Command`で識別されるそれぞれのいくつかのボタン (またはその他のコマンド インターフェイスを持つ) のプロパティ、`CommandParameter`します。 これらのボタンに追加する文字、`InputString`の電話番号として書式設定し、プロパティ、`DisplayText`プロパティ。

型の 2 番目のプロパティも`ICommand`という`DeleteCharCommand`します。 これは、バック スペース ボタンにバインドしますが、削除する文字がない場合に、ボタンを無効にする必要があります。

次キーパッドがいないと視覚的に高度なとして可能性があります。 代わりに、マークアップがコマンド インターフェイスの使用より明確に示すために最小限に削減されました。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.KeypadPage"
             Title="Keypad Page">

    <Grid HorizontalOptions="Center"
          VerticalOptions="Center">
        <Grid.BindingContext>
            <local:KeypadViewModel />
        </Grid.BindingContext>

        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="80" />
            <ColumnDefinition Width="80" />
            <ColumnDefinition Width="80" />
        </Grid.ColumnDefinitions>

        <!-- Internal Grid for top row of items -->
        <Grid Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="3">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="Auto" />
            </Grid.ColumnDefinitions>

            <Frame Grid.Column="0"
                   OutlineColor="Accent">
                <Label Text="{Binding DisplayText}" />
            </Frame>

            <Button Text="&#x21E6;"
                    Command="{Binding DeleteCharCommand}"
                    Grid.Column="1"
                    BorderWidth="0" />
        </Grid>

        <Button Text="1"
                Command="{Binding AddCharCommand}"
                CommandParameter="1"
                Grid.Row="1" Grid.Column="0" />

        <Button Text="2"
                Command="{Binding AddCharCommand}"
                CommandParameter="2"
                Grid.Row="1" Grid.Column="1" />

        <Button Text="3"
                Command="{Binding AddCharCommand}"
                CommandParameter="3"
                Grid.Row="1" Grid.Column="2" />

        <Button Text="4"
                Command="{Binding AddCharCommand}"
                CommandParameter="4"
                Grid.Row="2" Grid.Column="0" />

        <Button Text="5"
                Command="{Binding AddCharCommand}"
                CommandParameter="5"
                Grid.Row="2" Grid.Column="1" />

        <Button Text="6"
                Command="{Binding AddCharCommand}"
                CommandParameter="6"
                Grid.Row="2" Grid.Column="2" />

        <Button Text="7"
                Command="{Binding AddCharCommand}"
                CommandParameter="7"
                Grid.Row="3" Grid.Column="0" />

        <Button Text="8"
                Command="{Binding AddCharCommand}"
                CommandParameter="8"
                Grid.Row="3" Grid.Column="1" />

        <Button Text="9"
                Command="{Binding AddCharCommand}"
                CommandParameter="9"
                Grid.Row="3" Grid.Column="2" />

        <Button Text="*"
                Command="{Binding AddCharCommand}"
                CommandParameter="*"
                Grid.Row="4" Grid.Column="0" />

        <Button Text="0"
                Command="{Binding AddCharCommand}"
                CommandParameter="0"
                Grid.Row="4" Grid.Column="1" />

        <Button Text="#"
                Command="{Binding AddCharCommand}"
                CommandParameter="#"
                Grid.Row="4" Grid.Column="2" />
    </Grid>
</ContentPage>
```

`Command`最初の`Button`これに表示されるマークアップにバインドされて、 `DeleteCharCommand`; に残りの部分がバインドされて、`AddCharCommand`で、`CommandParameter`に表示される文字とは同じ、`Button`顔。 アクションで、プログラムを示します。

[![](data-bindings-to-mvvm-images/keypad.png "Calculator using MVVM and Commands")](data-bindings-to-mvvm-images/keypad-large.png#lightbox "Calculator using MVVM and Commands")

### <a name="invoking-asynchronous-methods"></a>非同期メソッドの呼び出し

コマンドは、非同期メソッドを呼び出すこともできます。 使用してこれは、`async`と`await`キーワードを指定するとき、`Execute`メソッド。

```csharp
DownloadCommand = new Command (async () => await DownloadAsync ());
```

これが示す、`DownloadAsync`メソッドは、`Task`待機する必要があります。

```csharp
async Task DownloadAsync ()
{
    await Task.Run (() => Download ());
}

void Download ()
{
    ...
}
```

## <a name="implementing-a-navigation-menu"></a>ナビゲーション メニューの実装

[XamlSamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)この一連の記事内のすべてのソース コードを含むプログラムがそのホーム ページの ViewModel を使用します。 このビューモデルが短いという名前の 3 つのプロパティを持つクラスの定義を`Type`、 `Title`、および`Description`サンプル ページ、タイトル、および簡単な説明のそれぞれの型が含まれています。 さらに、ViewModel はという名前の静的プロパティを定義します。`All`プログラム内のすべてのページのコレクションには。

```csharp
public class PageDataViewModel
{
    public PageDataViewModel(Type type, string title, string description)
    {
        Type = type;
        Title = title;
        Description = description;
    }

    public Type Type { private set; get; }

    public string Title { private set; get; }

    public string Description { private set; get; }

    static PageDataViewModel()
    {
        All = new List<PageDataViewModel>
        {
            // Part 1. Getting Started with XAML
            new PageDataViewModel(typeof(HelloXamlPage), "Hello, XAML",
                                  "Display a Label with many properties set"),

            new PageDataViewModel(typeof(XamlPlusCodePage), "XAML + Code",
                                  "Interact with a Slider and Button"),

            // Part 2. Essential XAML Syntax
            new PageDataViewModel(typeof(GridDemoPage), "Grid Demo",
                                  "Explore XAML syntax with the Grid"),

            new PageDataViewModel(typeof(AbsoluteDemoPage), "Absolute Demo",
                                  "Explore XAML syntax with AbsoluteLayout"),

            // Part 3. XAML Markup Extensions
            new PageDataViewModel(typeof(SharedResourcesPage), "Shared Resources",
                                  "Using resource dictionaries to share resources"),

            new PageDataViewModel(typeof(StaticConstantsPage), "Static Constants",
                                  "Using the x:Static markup extensions"),

            new PageDataViewModel(typeof(RelativeLayoutPage), "Relative Layout",
                                  "Explore XAML markup extensions"),

            // Part 4. Data Binding Basics
            new PageDataViewModel(typeof(SliderBindingsPage), "Slider Bindings",
                                  "Bind properties of two views on the page"),

            new PageDataViewModel(typeof(SliderTransformsPage), "Slider Transforms",
                                  "Use Sliders with reverse bindings"),

            new PageDataViewModel(typeof(ListViewDemoPage), "ListView Demo",
                                  "Use a ListView with data bindings"),

            // Part 5. From Data Bindings to MVVM
            new PageDataViewModel(typeof(OneShotDateTimePage), "One-Shot DateTime",
                                  "Obtain the current DateTime and display it"),

            new PageDataViewModel(typeof(ClockPage), "Clock",
                                  "Dynamically display the current time"),

            new PageDataViewModel(typeof(HslColorScrollPage), "HSL Color Scroll",
                                  "Use a view model to select HSL colors"),

            new PageDataViewModel(typeof(KeypadPage), "Keypad",
                                  "Use a view model for numeric keypad logic")
        };
    }

    public static IList<PageDataViewModel> All { private set; get; }
}
```

XAML ファイル`MainPage`定義、`ListBox`が`ItemsSource`プロパティに設定されて`All`プロパティとが含まれています、`TextCell`を表示するため、`Title`と`Description`各ページのプロパティ。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.MainPage"
             Padding="5, 0"
             Title="XAML Samples">

    <ListView ItemsSource="{x:Static local:PageDataViewModel.All}"
              ItemSelected="OnListViewItemSelected">
        <ListView.ItemTemplate>
            <DataTemplate>
                <TextCell Text="{Binding Title}"
                          Detail="{Binding Description}" />
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>
```

スクロール可能な一覧には、ページを表示します。

[![](data-bindings-to-mvvm-images/mainpage.png "Scrollable list of pages")](data-bindings-to-mvvm-images/mainpage-large.png#lightbox "Scrollable list of pages")

ユーザーが項目を選択すると、分離コード ファイル内のハンドラーがトリガーされます。 ハンドラーのセット、`SelectedItem`のプロパティ、`ListBox`に`null`選択したページをインスタンス化して、それに移動します。

```csharp
private async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs args)
{
    (sender as ListView).SelectedItem = null;

    if (args.SelectedItem != null)
    {
        PageDataViewModel pageData = args.SelectedItem as PageDataViewModel;
        Page page = (Page)Activator.CreateInstance(pageData.Type);
        await Navigation.PushAsync(page);
    }
}
```

## <a name="video"></a>ビデオ

> [!VIDEO https://youtube.com/embed/DYRLcqG2BAY]

**Xamarin.Forms と Prism で簡単に行う Xamarin Evolve 2016 です: MVVM**

## <a name="summary"></a>要約

XAML は、データ バインディング時に特に、Xamarin.Forms アプリケーションでユーザー インターフェイスを定義するための強力なツールと、MVVM を使用します。 結果はクリーン、洗練された、および潜在的に理解できる使いやすいユーザー インターフェイスのすべてのバック グラウンド サポート コードで表したものです。

## <a name="related-links"></a>関連リンク

- [XamlSamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [パート1。XAML を使用したはじめに](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [第2部。必須の XAML 構文](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [パート3。XAML マークアップ拡張機能](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [パート4。データバインディングの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)

## <a name="related-videos"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Series/Xamarin-101/XamarinForms-MVVM-with-XAML-6-of-11/player]

> [!Video https://channel9.msdn.com/Series/Xamarin-101/XamarinForms-Navigation-with-XAML-7-of-11/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
