---
title: 第 5 部 データ バインディングから MVVM まで
description: MVVM パターンでは、ビューと呼ばれる3つのソフトウェアレイヤー (XAML ユーザーインターフェイス) の分離が適用されます。モデルと呼ばれる基になるデータ。ビューとモデルの間の中間点として、ビューモデルと呼ばれるものがあります。
ms.prod: xamarin
ms.custom: video
ms.assetid: 48B37D44-4FB1-41B2-9A5E-6D383B041F81
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
ms.openlocfilehash: 80386780d52f9a28d421d2a83981085956d06ea5
ms.sourcegitcommit: efbc69acf4ea484d8815311b058114379c9db8a2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/08/2019
ms.locfileid: "73842948"
---
# <a name="part-5-from-data-bindings-to-mvvm"></a>第 5 部 データ バインディングから MVVM まで

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_モデルビュービューモデル (MVVM) アーキテクチャパターンは、XAML を念頭に置いて考案されました。このパターンでは、3つのソフトウェアレイヤー (つまり、ビューと呼ばれる XAML ユーザーインターフェイス) の分離が適用されます。モデルと呼ばれる基になるデータ。ビューとモデルの間の中間点として、ビューモデルと呼ばれるものがあります。ビューとビューモデルは、多くの場合、XAML ファイルで定義されているデータバインディングを使用して接続されます。ビューの BindingContext は、通常、ビューモデルのインスタンスです。_

## <a name="a-simple-viewmodel"></a>単純なビューモデル

Viewmodel の概要として、最初に1つのプログラムを使用しないプログラムを見てみましょう。
前に、新しい XML 名前空間宣言を定義して、XAML ファイルが他のアセンブリのクラスを参照できるようにする方法を説明しました。 `System` 名前空間の XML 名前空間宣言を定義するプログラムを次に示します。

```csharp
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

プログラムでは、`x:Static` を使用して、静的な `DateTime.Now` プロパティから現在の日付と時刻を取得し、その `DateTime` 値を `StackLayout`の `BindingContext` に設定できます。

```xaml
<StackLayout BindingContext="{x:Static sys:DateTime.Now}" …>
```

`BindingContext` は特殊なプロパティです。要素で `BindingContext` を設定すると、その要素のすべての子によって継承されます。 これは、`StackLayout` のすべての子が同じ `BindingContext`を持ち、そのオブジェクトのプロパティへの単純なバインドを含むことができることを意味します。

**ワンショットの DateTime**プログラムでは、2つの子のプロパティへ `DateTime` のバインドが含まれていますが、他の2つの子にはバインドパスが欠落していると思われるバインドが含まれています。 これは、`DateTime` 値自体が `StringFormat`に使用されることを意味します。

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

問題は、ページが最初に作成されたときに日付と時刻が1回設定され、変更されないことです。

[![](data-bindings-to-mvvm-images/oneshotdatetime.png "View Displaying Date and Time")](data-bindings-to-mvvm-images/oneshotdatetime-large.png#lightbox "View Displaying Date and Time")

XAML ファイルには、常に現在の時刻を示すクロックが表示されますが、いくつかのコードが必要です。MVVM の観点から考えると、モデルとビューモデルはコードで完全に記述されたクラスです。 ビューは、多くの場合、データバインディングによってビューモデルで定義されたプロパティを参照する XAML ファイルです。

適切なモデルは、ビューモデルに依存しないため、適切なビューモデルはビューを意識しません。 ただし、多くの場合、プログラマは、ビューモデルによって公開されるデータ型を、特定のユーザーインターフェイスに関連付けられたデータ型に合わせて並べ替えます。 たとえば、8ビットの ASCII 文字列を含むデータベースにモデルがアクセスする場合、ユーザーインターフェイスで Unicode を排他的に使用できるように、これらの文字列を Unicode 文字列に変換する必要があります。

MVVM の簡単な例 (ここに示されている例) では、多くの場合、モデルはまったく存在しません。このパターンには、データバインドにリンクされたビューとビューモデルだけが含まれます。

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

Viewmodel は一般に `INotifyPropertyChanged` インターフェイスを実装します。つまり、クラスは、プロパティのいずれかが変更されるたびに `PropertyChanged` イベントを発生させます。 Xamarin のデータバインディング機構は、この `PropertyChanged` イベントにハンドラーをアタッチします。これにより、プロパティが変更されたときに通知を受け、ターゲットを新しい値で更新したままにすることができます。

このビューモデルに基づくクロックは、次のように簡単に行うことができます。

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

プロパティ要素タグを使用して、`ClockViewModel` が `Label` の `BindingContext` に設定されていることに注目してください。 または、`Resources` コレクション内の `ClockViewModel` をインスタンス化し、`StaticResource` マークアップ拡張機能を使用して `BindingContext` に設定することもできます。 また、分離コードファイルは、ビューモデルをインスタンス化できます。

`Label` の `Text` プロパティの `Binding` マークアップ拡張機能は、`DateTime` プロパティを書式設定します。 次のような画面が表示されます。

[![](data-bindings-to-mvvm-images/clock.png "View Displaying Date and Time via ViewModel")](data-bindings-to-mvvm-images/clock-large.png#lightbox "View Displaying Date and Time via ViewModel")

また、プロパティをピリオドで区切ることで、ビューモデルの `DateTime` プロパティの個々のプロパティにアクセスすることもできます。

```xaml
<Label Text="{Binding DateTime.Second, StringFormat='{0}'}" … >
```

## <a name="interactive-mvvm"></a>対話型 MVVM

MVVM は、基になるデータモデルに基づく対話型ビューに対して、双方向のデータバインディングと共に使用されることがよくあります。

`Color` 値を `Hue`、`Saturation`、`Luminosity` 値に変換する `HslViewModel` という名前のクラスを次に示します。

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

`Hue`、`Saturation`、および `Luminosity` プロパティを変更すると、`Color` プロパティが変更され、`Color` を変更すると、他の3つのプロパティが変更されます。 これは、プロパティが変更されていない限り、クラスが `PropertyChanged` イベントを呼び出さない点を除いて、無限ループのように思えるかもしれません。 これにより、それ以外の制御されていないフィードバックループが終了します。

次の XAML ファイルには、`Color` プロパティがビューモデルの `Color` プロパティにバインドされている `BoxView` と、`Label`、`Hue`、および `Saturation`の各プロパティにバインドされた3つの `Slider` と3つの `Luminosity` ビューが含まれています。

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

各 `Label` のバインドは、既定の `OneWay`です。 値を表示する必要があるだけです。 ただし、各 `Slider` のバインドは `TwoWay`ます。 これにより、`Slider` をビューモデルから初期化できます。 ビューモデルがインスタンス化されるときに、`Color` プロパティが `Aqua` に設定されていることに注意してください。 ただし、`Slider` の変更では、ビューモデルのプロパティに新しい値を設定する必要もあります。これにより、新しい色が計算されます。

[![](data-bindings-to-mvvm-images/hslcolorscroll.png "MVVM using Two-Way Data Bindings")](data-bindings-to-mvvm-images/hslcolorscroll-large.png#lightbox "MVVM using Two-Way Data Bindings")

## <a name="commanding-with-viewmodels"></a>Viewmodel でのコマンドの使用

多くの場合、MVVM パターンはデータ項目の操作に制限されます。これは、ビューモデルの並列データオブジェクトを表示するユーザーインターフェイスオブジェクトです。

ただし、ビューには、ビューモデル内のさまざまなアクションをトリガーするボタンが含まれている必要があります。 ただし、ビューモデルは特定のユーザーインターフェイスパラダイムに関連付けられるため、ビューモデルにボタンの `Clicked` ハンドラーを含めることはできません。

Viewmodel が特定のユーザーインターフェイスオブジェクトから独立していても、モデルビュー内でメソッドを呼び出すことができるようにするために、*コマンド*インターフェイスが存在します。 このコマンドインターフェイスは、Xamarin の次の要素によってサポートされています。

- `Button`
- `MenuItem`
- `ToolbarItem`
- `SearchBar`
- `TextCell` (また `ImageCell`)
- `ListView`
- `TapGestureRecognizer`

`SearchBar` と `ListView` 要素を除き、これらの要素は次の2つのプロパティを定義します。

- `System.Windows.Input.ICommand` 型の `Command`
- `Object` 型の `CommandParameter`

`SearchBar` は `SearchCommand` プロパティと `SearchCommandParameter` プロパティを定義しますが、`ListView` は `RefreshCommand` 型の `ICommand`プロパティを定義します。

`ICommand` インターフェイスは、次の2つのメソッドと1つのイベントを定義します。

- `void Execute(object arg)`
- `bool CanExecute(object arg)`
- `event EventHandler CanExecuteChanged`

ビューモデルでは、`ICommand`型のプロパティを定義できます。 これらのプロパティは、各 `Button` または他の要素の `Command` プロパティ、またはこのインターフェイスを実装するカスタムビューにバインドできます。 必要に応じて、`CommandParameter` プロパティを設定して、このビューモデルプロパティにバインドされている個々の `Button` オブジェクト (またはその他の要素) を識別できます。 内部的には、`Button` は、ユーザーが `Button`をタップするたびに `Execute` メソッドを呼び出し、その `CommandParameter`を `Execute` メソッドに渡します。

`CanExecute` メソッドと `CanExecuteChanged` イベントは、`Button` タップが現在無効である可能性がある場合に使用されます。この場合、`Button` はそれ自体を無効にする必要があります。 `Button` は、`Command` プロパティが最初に設定されたときと、`CanExecuteChanged` イベントが発生するたびに `CanExecute` を呼び出します。 `CanExecute` が `false`を返す場合、`Button` はそれ自体を無効にし、`Execute` 呼び出しを生成しません。

Viewmodel にコマンド実行を追加する方法については、Xamarin. Forms には `ICommand`を実装する2つのクラスが定義されています。 `Command` と `Command<T>` `T` は `Execute` する引数の型であり、`CanExecute`します。 これらの2つのクラスは、いくつかのコンストラクターと `ChangeCanExecute` メソッドを定義しています。このメソッドを使用すると、`Command` オブジェクトに `CanExecuteChanged` イベントを強制的に発生させることができます。

ここでは、電話番号を入力するための簡単なキーパッドのビューモデルを示します。 `Execute` および `CanExecute` メソッドが、コンストラクターでラムダ関数として定義されていることに注意してください。

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

このモデルは、`AddCharCommand` プロパティが、`CommandParameter`によって識別されるいくつかのボタン (またはコマンドインターフェイスを持つその他のボタン) の `Command` プロパティにバインドされていることを前提としています。 これらのボタンは `InputString` プロパティに文字を追加し、`DisplayText` プロパティの電話番号として書式設定します。

また、`DeleteCharCommand`という `ICommand` 型の2番目のプロパティもあります。 これは、[戻る] ボタンにバインドされていますが、削除する文字がない場合は、ボタンを無効にする必要があります。

次のキーパッドは、見た目ほど洗練されていません。 代わりに、コマンドインターフェイスの使用方法をより明確に示すために、マークアップが最小限に抑えられています。

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

このマークアップに表示される最初の `Button` の `Command` プロパティは `DeleteCharCommand`にバインドされます。残りの部分は、`Button` 面に表示される文字と同じ `CommandParameter` を持つ `AddCharCommand` にバインドされます。 次のプログラムが動作しています。

[![](data-bindings-to-mvvm-images/keypad.png "Calculator using MVVM and Commands")](data-bindings-to-mvvm-images/keypad-large.png#lightbox "Calculator using MVVM and Commands")

### <a name="invoking-asynchronous-methods"></a>非同期メソッドの呼び出し

コマンドは、非同期メソッドを呼び出すこともできます。 これは、`Execute` メソッドを指定するときに、`async` キーワードと `await` キーワードを使用して実現されます。

```csharp
DownloadCommand = new Command (async () => await DownloadAsync ());
```

これは、`DownloadAsync` メソッドが `Task` であり、待機する必要があることを示しています。

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

## <a name="implementing-a-navigation-menu"></a>ナビゲーションメニューの実装

この一連の記事のすべてのソースコードを含む[Xamlsamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)プログラムは、ホームページのビュービューを使用します。 このビューモデルは、`Type`、`Title`、および `Description` という名前の3つのプロパティを持つ短いクラスを定義したもので、各サンプルページの種類、タイトル、および簡単な説明が含まれています。 さらに、ビューモデルは、プログラム内のすべてのページのコレクションである `All` という名前の静的プロパティを定義します。

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

`MainPage` の XAML ファイルは、`ItemsSource` プロパティが `All` プロパティに設定され、各ページの `TextCell` および `Title` プロパティを表示するための `Description` を含む `ListBox` を定義します。

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

ページはスクロール可能な一覧に表示されます。

[![](data-bindings-to-mvvm-images/mainpage.png "Scrollable list of pages")](data-bindings-to-mvvm-images/mainpage-large.png#lightbox "Scrollable list of pages")

ユーザーが項目を選択すると、分離コードファイル内のハンドラーがトリガーされます。 ハンドラーは、`ListBox` の `SelectedItem` プロパティを `null` に戻し、選択したページをインスタンス化して、そのページに移動します。

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

**Xamarin の進化 2016: MVVM と Prism を使用したシンプルな作成**

## <a name="summary"></a>まとめ

XAML は、Xamarin. フォームアプリケーションでユーザーインターフェイスを定義するための強力なツールです。特に、データバインディングと MVVM を使用する場合に使用します。 結果として、コード内のすべてのバックグラウンドサポートを使用して、ユーザーインターフェイスをクリーンで洗練された形式で表現できます。

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
