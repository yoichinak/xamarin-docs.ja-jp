---
title: パート 5 です。 MVVM へのデータ バインディング
description: 'MVVM パターンは、次の 3 つのソフトウェア レイヤー間の分離を強制: ビューと呼ばれる、XAML ユーザー インターフェイスは、モデルと呼ばれる、基になるデータされ、ビューと、モデル間の媒介には、ViewModel が呼び出されます。'
ms.prod: xamarin
ms.assetid: 48B37D44-4FB1-41B2-9A5E-6D383B041F81
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: e83f8a585c4badc31bffaea53bb2f183e7b11fc9
ms.sourcegitcommit: d70fcc6380834127fdc58595aace55b7821f9098
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/19/2018
ms.locfileid: "36268863"
---
# <a name="part-5-from-data-bindings-to-mvvm"></a>パート 5 です。 MVVM へのデータ バインディング

_モデル View-viewmodel (MVVM) アーキテクチャ パターンは、注意を XAML 考案されました。パターンは、次の 3 つのソフトウェア レイヤー間の分離を強制: ビューと呼ばれる、XAML ユーザー インターフェイスは、モデルと呼ばれる、基になるデータされ、ビューと、モデル間の媒介には、ViewModel が呼び出されます。ビューと、ViewModel は、多くの場合、XAML ファイルで定義されているデータ バインディングを通じて接続されます。BindingContext のビューは、通常、ViewModel のインスタンスです。_

## <a name="a-simple-viewmodel"></a>単純な ViewModel

として ViewModels の概要については、まずを見てみましょう 1 つもないプログラム。
以前クラスを参照する他のアセンブリ内の XAML ファイルを許可する新しい XML 名前空間宣言を定義する方法を説明します。 ここでは、プログラムの XML 名前空間宣言を定義する、`System`名前空間。

```csharp
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

プログラムで使用できる`x:Static`静的から現在の日付と時刻を取得する`DateTime.Now`プロパティを設定し、`DateTime`値を`BindingContext`で、 `StackLayout`:

```xaml
<StackLayout BindingContext="{x:Static sys:DateTime.Now}" …>
```

`BindingContext` 非常に特殊なプロパティ: 設定すると、`BindingContext`要素、その要素のすべての子から継承されます。 つまり、すべての子、`StackLayout`この同じである`BindingContext`、そのオブジェクトのプロパティへの単純なバインドがあるとします。

**One-Shot DateTime**子の 2 つのプログラムのプロパティへのバインドを保持する`DateTime`値が他の 2 つの子は、バインド パスが見つからないのバインドを保持します。 つまり、`DateTime`値自体はの使用、 `StringFormat`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
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

もちろん、大きな問題は日付と時刻が、ページを最初にビルド後のセットと決して変更。

[![](data-bindings-to-mvvm-images/oneshotdatetime.png "日付と時刻を表示するビュー")](data-bindings-to-mvvm-images/oneshotdatetime-large.png#lightbox "日付と時刻を表示するビュー")

XAML ファイルは、現在の時刻が常に表示される時計を表示できますが、一部のコードを使用すればが必要があります。ときに MVVM、モデルおよび ViewModel の観点から考えることには、すべてコードで記述されたクラスです。 ビューは、データ バインディングによって、ViewModel で定義されたプロパティを参照する XAML ファイルでは多くの場合です。

適切なモデルでは、ViewModel を意識し、適切な ViewModel はビューを意識します。 ただし、ほとんどの場合、プログラマが調整を特定のユーザー インターフェイスに関連付けられているデータ型に ViewModel によって公開されるデータ型。 たとえば、モデルでは、8 ビット文字の ASCII 文字列を格納しているデータベースにアクセスは場合、ViewModel 必要がありますにそれらの文字列に排他的に使用するユーザー インターフェイスで Unicode に対応する Unicode 文字列に変換します。

(ここに示すもの) などの MVVM の単純な例で多くの場合、モデルはありませんすべてのビューだけでは、パターンとデータ バインディングにリンクされている ViewModel です。

プロパティだけを 1 つという名前のクロックの ViewModel を次に示します`DateTime`、する更新プログラムがあるが、`DateTime`毎秒のプロパティ。

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

一般に ViewModels を実装、`INotifyPropertyChanged`インターフェイスで、クラスが発生することを意味、`PropertyChanged`イベントのプロパティの 1 つが変更されるたびにします。 Xamarin.Forms で、データ バインディング機構では、これにハンドラーをアタッチ`PropertyChanged`イベント プロパティが変更されたときに通知できるように行い、新しい値で更新対象を保持します。

この ViewModel に基づいてクロックはこれと同じくらい簡単にできます。

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

通知方法、`ClockViewModel`に設定されている、`BindingContext`の`Label`プロパティ要素タグを使用します。 代わりに、インスタンスを作成できる、`ClockViewModel`で、`Resources`コレクションに設定し、`BindingContext`を介して、`StaticResource`マークアップ拡張機能です。 または、分離コード ファイルには、ViewModel をインスタンス化します。

`Binding`でマークアップ拡張機能、`Text`のプロパティ、`Label`形式、`DateTime`プロパティです。 表示を次に示します。

[![](data-bindings-to-mvvm-images/clock.png "ViewModel を介しての日時を表示するビュー")](data-bindings-to-mvvm-images/clock-large.png#lightbox "ViewModel を介しての日時を表示するビュー")

個々 のプロパティにアクセスすることも、`DateTime`プロパティをピリオドで区切って ViewModel のプロパティ。

```xaml
<Label Text="{Binding DateTime.Second, StringFormat='{0}'}" … >
```

## <a name="interactive-mvvm"></a>対話型 MVVM

MVVM には、基になるデータ モデルに基づく対話型ビューの双方向データ バインドはよく使用されます。

という名前のクラスを次に示します`HslViewModel`変換する、`Color`値に`Hue`、 `Saturation`、および`Luminosity`値、およびその逆の場合。

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

変更、 `Hue`、 `Saturation`、および`Luminosity`プロパティ原因、`Color`を変更するプロパティが変更され`Color`により、他の 3 つのプロパティを変更します。 これは、見えますが、無限ループ クラスが起動しないことを除き、`PropertyChanged`イベント プロパティが実際に変更された場合を除き、します。 これにより、end が、それ以外の場合制御不能フィードバック ループに追加します。

次の XAML ファイルが含まれています、`BoxView`が`Color`プロパティにバインド、 `Color` 、ViewModel と 3 つのプロパティ`Slider`3`Label`にバインドされているビュー、 `Hue`、 `Saturation`、および`Luminosity`プロパティ。

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

各バインド`Label`既定値は、`OneWay`です。 のみ値を表示する必要があります。 各バインディングが、`Slider`は`TwoWay`します。 これにより、 `Slider` ViewModel から初期化されるようにします。 注意して、`Color`プロパティに設定されている`Aqua`ViewModel がインスタンス化されるときです。 変更は、`Slider`で新しい色を計算すると、ViewModel プロパティの新しい値を設定する必要もあります。

[![](data-bindings-to-mvvm-images/hslcolorscroll.png "双方向データ バインドを使用して MVVM")](data-bindings-to-mvvm-images/hslcolorscroll-large.png#lightbox "MVVM 双方向データ バインディングの使用")

## <a name="commanding-with-viewmodels"></a>ViewModels とコマンド実行

多くの場合、MVVM パターンはデータ項目の操作に制限: ビューのユーザー インターフェイス オブジェクトの並列、ViewModel 内のデータ オブジェクト。

ただし、必要があります、ビュー、ViewModel でさまざまなアクションをトリガーするボタンが含まれています。 ViewModel には必要がありますが含まれていませんが、`Clicked`ハンドラー ボタンに対して、特定のユーザー インターフェイスのパラダイムに ViewModel を関連付けることがあるためです。

特定のユーザー インターフェイス オブジェクトの依存関係のないが、まだ、ViewModel 内で呼び出されるメソッドを許可する ViewModels を許可する、*コマンド*インターフェイスが存在します。 このコマンド インターフェイスは、Xamarin.Forms では、次の要素によってサポートされます。

-  `Button`
-  `MenuItem`
-  `ToolbarItem`
-  `SearchBar`
-  `TextCell` (したがっても`ImageCell`)
-  `ListView`
-  `TapGestureRecognizer`

例外を除いて、`SearchBar`と`ListView`要素、これらの要素が 2 つのプロパティを定義します。

-  `Command` 型の  `System.Windows.Input.ICommand`
-  `CommandParameter` 型の  `Object`

`SearchBar`定義`SearchCommand`と`SearchCommandParameter`プロパティ中、`ListView`を定義、`RefreshCommand`型のプロパティ`ICommand`です。

`ICommand`インターフェイスは、2 つのメソッドと 1 つのイベントを定義します。

-  `void Execute(object arg)`
-  `bool CanExecute(object arg)`
-  `event EventHandler CanExecuteChanged`

型のプロパティを定義できます、ViewModel`ICommand`です。 これらのプロパティをバインドできます、`Command`の各プロパティ`Button`またはその他の要素、またはこのインターフェイスを実装するカスタム ビューなどです。 必要に応じて設定することができます、`CommandParameter`個々 に識別するプロパティ`Button`オブジェクト (またはその他の要素) この ViewModel プロパティにバインドされます。 内部的には、`Button`呼び出し、`Execute`メソッド、ユーザーがタップするたびに、`Button`に渡す、`Execute`メソッドその`CommandParameter`です。

`CanExecute`メソッドおよび`CanExecuteChanged`場合にイベントを使用場所、 `Button` tap できない可能性があります、現在有効な場合、`Button`自体を無効にする必要があります。 `Button`呼び出し`CanExecute`ときに、`Command`プロパティが最初に設定されているとするたびに、`CanExecuteChanged`イベントが発生します。 場合`CanExecute`返します`false`、`Button`自体が無効になり、生成しない`Execute`呼び出しです。

実装する 2 つのクラスを定義する Xamarin.Forms ヘルプについては、ViewModels にコマンドを追加することで、 `ICommand`:`Command`と`Command<T>`場所`T`への引数の型は、`Execute`と`CanExecute`です。 これら 2 つのクラス定義の複数のコンス トラクターと`ChangeCanExecute`、ViewModel が強制的に呼び出すことができるメソッド、`Command`発生させるオブジェクト、`CanExecuteChanged`イベント。

ここでは、電話番号を入力するためのものでは、単純なキーパッドの ViewModel です。 注意して、`Execute`と`CanExecute`メソッドは、コンス トラクターでのラムダ関数右として定義されます。

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

この ViewModel が想定する、`AddCharCommand`プロパティにバインドされる、`Command`によって識別されるそれぞれのいくつかのボタン (またはそれ以外のコマンド インターフェイスを持つ) のプロパティ、`CommandParameter`です。 これらのボタンを追加する文字、`InputString`の電話番号として書式設定し、プロパティ、`DisplayText`プロパティです。

型の 2 番目のプロパティも`ICommand`という`DeleteCharCommand`です。 これは、バック スペース ボタンにバインドが、削除する文字がない場合に、ボタンを無効にする必要があります。

次のキーパッドがいないと視覚的に、高度ながあります。 代わりに、マークアップは、さらに明確に、コマンド インターフェイスの使用を示すために最低限に削減されました。

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

`Command`最初の`Button`これに表示されるマークアップにバインドされて、 `DeleteCharCommand`; に残りの部分がバインドされて、`AddCharCommand`で、`CommandParameter`は、文字と同じに表示される、`Button`面。 アクションで、プログラムを次に示します。

[![](data-bindings-to-mvvm-images/keypad.png "MVVM とコマンドを使用して、電卓")](data-bindings-to-mvvm-images/keypad-large.png#lightbox "MVVM とコマンドを使用して、電卓")

### <a name="invoking-asynchronous-methods"></a>非同期メソッドの呼び出し

コマンドでは、非同期メソッドを呼び出すことができますも。 使用してこれは、`async`と`await`キーワードを指定する場合、`Execute`メソッド。

```csharp
DownloadCommand = new Command (async () => await DownloadAsync ());
```

これが示す、`DownloadAsync`メソッドは、`Task`れ、待機する必要があります。

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

[XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)この一連の記事内のすべてのソース コードを含むプログラムはそのホーム ページの ViewModel を使用します。 この ViewModel が短いという 3 つのプロパティを持つクラスの定義`Type`、 `Title`、および`Description`サンプル ページ、タイトル、および短い説明のそれぞれの型が含まれています。 また、という名前の静的プロパティの定義、ViewModel`All`プログラム内のすべてのページのコレクションであります。

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

XAML ファイル`MainPage`定義、`ListBox`が`ItemsSource`プロパティに設定されて`All`プロパティとが含まれている、`TextCell`を表示するため、`Title`と`Description`各ページのプロパティ。

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

ページは、スクロール可能な一覧に表示されます。

[![](data-bindings-to-mvvm-images/mainpage.png "ページの一覧をスクロール可能な")](data-bindings-to-mvvm-images/mainpage-large.png#lightbox "ページのスクロール可能な一覧")

ユーザーが項目を選択したときに、分離コード ファイル内のハンドラーがトリガーされます。 ハンドラーを設定、`SelectedItem`のプロパティ、`ListBox`に`null`選択したページをインスタンス化して、移動しました。

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

**Xamarin.Forms およびプリズムで簡単に行う Xamarin では、2016 を発展: MVVM**

## <a name="summary"></a>まとめ

XAML は、Xamarin.Forms アプリケーションは、特にデータ バインディングでユーザー インターフェイスを定義するための強力なツールおよび MVVM を使用します。 すべてのバック グラウンド サポート コードでのユーザー インターフェイスのクリーン、洗練され、潜在的に使いやすい形式になります。


## <a name="related-links"></a>関連リンク

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [第 1 部XAML の概要](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [第 2 部基本的な XAML 構文](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [第 3 部XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [第 4 部データ バインディングの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
