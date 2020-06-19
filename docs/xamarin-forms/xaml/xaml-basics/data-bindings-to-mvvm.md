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
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 047cf963394325e8f88759ffe9da7dcf2ca3ad12
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84127531"
---
# <a name="part-5-from-data-bindings-to-mvvm"></a>第 5 部 データ バインディングから MVVM まで

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_モデルビュービューモデル (MVVM) アーキテクチャパターンは、XAML を念頭に置いて考案されました。このパターンでは、3つのソフトウェアレイヤー (つまり、ビューと呼ばれる XAML ユーザーインターフェイス) の分離が適用されます。モデルと呼ばれる基になるデータ。ビューとモデルの間の中間点として、ビューモデルと呼ばれるものがあります。ビューとビューモデルは、多くの場合、XAML ファイルで定義されているデータバインディングを使用して接続されます。ビューの BindingContext は、通常、ビューモデルのインスタンスです。_

## <a name="a-simple-viewmodel"></a>単純なビューモデル

Viewmodel の概要として、最初に1つのプログラムを使用しないプログラムを見てみましょう。
前に、新しい XML 名前空間宣言を定義して、XAML ファイルが他のアセンブリのクラスを参照できるようにする方法を説明しました。 名前空間の XML 名前空間宣言を定義するプログラムを次に示し `System` ます。

```csharp
xmlns:sys="clr-namespace:System;assembly=netstandard"
```

プログラムはを使用して、 `x:Static` 静的なプロパティから現在の日付と時刻を取得し、その値をのに `DateTime.Now` 設定でき `DateTime` `BindingContext` `StackLayout` ます。

```xaml
<StackLayout BindingContext="{x:Static sys:DateTime.Now}" …>
```

`BindingContext`は特殊なプロパティです。要素にを設定すると、その要素 `BindingContext` のすべての子によって継承されます。 これは、のすべての子 `StackLayout` が同じであり、 `BindingContext` そのオブジェクトのプロパティへの単純なバインドを含むことができることを意味します。

**ワンショットの DateTime**プログラムでは、2つの子のうち、その値のプロパティへのバインドが含まれて `DateTime` いますが、他の2つの子にはバインドパスが欠落していると思われるバインドが含まれています。 これは `DateTime` 、値自体がに使用されることを意味し `StringFormat` ます。

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

XAML ファイルには、常に現在の時刻を示すクロックが表示されますが、いくつかのコードが必要です。MVVM の観点から考えると、モデルとビューモデルはコードで完全に記述されたクラスです。 ビューは、多くの場合、データバインディングによってビューモデルで定義されたプロパティを参照する XAML ファイルです。

適切なモデルは、ビューモデルに依存しないため、適切なビューモデルはビューを意識しません。 ただし、多くの場合、プログラマは、ビューモデルによって公開されるデータ型を、特定のユーザーインターフェイスに関連付けられたデータ型に合わせて並べ替えます。 たとえば、8ビットの ASCII 文字列を含むデータベースにモデルがアクセスする場合、ユーザーインターフェイスで Unicode を排他的に使用できるように、これらの文字列を Unicode 文字列に変換する必要があります。

MVVM の簡単な例 (ここに示されている例) では、多くの場合、モデルはまったく存在しません。このパターンには、データバインドにリンクされたビューとビューモデルだけが含まれます。

次に示すのは、という名前の1つのプロパティを持つクロックのビューモデルです `DateTime` 。このプロパティは、1秒ごとに更新され `DateTime` ます。

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

Viewmodel は通常、 `INotifyPropertyChanged` インターフェイスを実装します。つまり、クラスは、プロパティのいずれかが変更されるたびにイベントを発生させ `PropertyChanged` ます。 のデータバインディング機構は、 Xamarin.Forms このイベントにハンドラーをアタッチして、 `PropertyChanged` プロパティが変更されたときに通知されるようにし、ターゲットを新しい値で更新したままにします。

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

`ClockViewModel` `BindingContext` `Label` を使用して、プロパティ要素タグを使用して、がに設定されていることに注意してください。 または、をインスタンス化 `ClockViewModel` `Resources` して、 `BindingContext` マークアップ拡張機能を使用してに設定することもでき `StaticResource` ます。 また、分離コードファイルは、ビューモデルをインスタンス化できます。

プロパティは、 `Binding` のプロパティのマークアップ拡張機能によって `Text` 書式設定され `Label` `DateTime` ます。 次のような画面が表示されます。

[![](data-bindings-to-mvvm-images/clock.png "View Displaying Date and Time via ViewModel")](data-bindings-to-mvvm-images/clock-large.png#lightbox "View Displaying Date and Time via ViewModel")

また、 `DateTime` プロパティをピリオドで区切ることで、ビューモデルのプロパティの個々のプロパティにアクセスすることもできます。

```xaml
<Label Text="{Binding DateTime.Second, StringFormat='{0}'}" … >
```

## <a name="interactive-mvvm"></a>対話型 MVVM

MVVM は、基になるデータモデルに基づく対話型ビューに対して、双方向のデータバインディングと共に使用されることがよくあります。

`HslViewModel`値を、、およびの各値に変換するという名前のクラスを次に示し `Color` `Hue` `Saturation` `Luminosity` ます。

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

、、およびの各プロパティを変更すると、プロパティが変更され、を変更すると、 `Hue` `Saturation` `Luminosity` `Color` `Color` 他の3つのプロパティが変更されます。 これは、 `PropertyChanged` プロパティが変更されていない限り、クラスがイベントを呼び出さない点を除いて、無限ループのように思えるかもしれません。 これにより、それ以外の制御されていないフィードバックループが終了します。

次の XAML ファイルには `BoxView` 、 `Color` プロパティがビューモデルのプロパティにバインドされていると、、、およびの各 `Color` プロパティにバインドされた3つ `Slider` および3つの `Label` ビューが `Hue` `Saturation` 含まれてい `Luminosity` ます。

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

それぞれのバインディング `Label` が既定値です `OneWay` 。 値を表示する必要があるだけです。 ただし、各のバインド `Slider` は `TwoWay` です。 これにより、 `Slider` をビューモデルから初期化できます。 `Color` `Aqua` ビューモデルがインスタンス化されるときに、プロパティがに設定されていることに注意してください。 ただし、の変更では、 `Slider` ビューモデルのプロパティに新しい値を設定する必要もあります。これにより、新しい色が計算されます。

[![](data-bindings-to-mvvm-images/hslcolorscroll.png "MVVM using Two-Way Data Bindings")](data-bindings-to-mvvm-images/hslcolorscroll-large.png#lightbox "MVVM using Two-Way Data Bindings")

## <a name="commanding-with-viewmodels"></a>Viewmodel でのコマンドの使用

多くの場合、MVVM パターンはデータ項目の操作に制限されます。これは、ビューモデルの並列データオブジェクトを表示するユーザーインターフェイスオブジェクトです。

ただし、ビューには、ビューモデル内のさまざまなアクションをトリガーするボタンが含まれている必要があります。 ただし、ビューモデルは特定のユーザーインターフェイスパラダイムに関連付けられるため、ビューモデルに `Clicked` ボタンのハンドラーを含めることはできません。

Viewmodel が特定のユーザーインターフェイスオブジェクトから独立していても、モデルビュー内でメソッドを呼び出すことができるようにするために、*コマンド*インターフェイスが存在します。 このコマンドインターフェイスは、の次の要素によってサポートされてい Xamarin.Forms ます。

- `Button`
- `MenuItem`
- `ToolbarItem`
- `SearchBar`
- `TextCell`( `ImageCell` また、
- `ListView`
- `TapGestureRecognizer`

要素と要素を除き、これらの要素は次の `SearchBar` `ListView` 2 つのプロパティを定義します。

- `Command`種類`System.Windows.Input.ICommand`
- `CommandParameter`種類`Object`

は `SearchBar` 、 `SearchCommand` `SearchCommandParameter` プロパティとプロパティを定義しますが、は `ListView` `RefreshCommand` 型のプロパティを定義し `ICommand` ます。

インターフェイスは、 `ICommand` 次の2つのメソッドと1つのイベントを定義します。

- `void Execute(object arg)`
- `bool CanExecute(object arg)`
- `event EventHandler CanExecuteChanged`

ビューモデルでは、型のプロパティを定義でき `ICommand` ます。 これらのプロパティは、 `Command` 各要素のプロパティ、 `Button` またはこのインターフェイスを実装するカスタムビューにバインドできます。 必要に応じて、プロパティを設定して、 `CommandParameter` `Button` このビューモデルプロパティにバインドされている個々のオブジェクト (またはその他の要素) を識別することができます。 内部的には、は、 `Button` `Execute` ユーザーがをタップするたびにメソッドを呼び出し、 `Button` メソッドに渡し `Execute` `CommandParameter` ます。

`CanExecute`メソッドとイベントは、tap が現在無効である可能性がある場合に使用され `CanExecuteChanged` `Button` ます。この場合、は `Button` 自身を無効にする必要があります。 は `Button` 、 `CanExecute` `Command` プロパティが最初に設定されたときと、イベントが発生するたびにを呼び出し `CanExecuteChanged` ます。 が `CanExecute` を返す場合 `false` 、は自身を無効にし、 `Button` 呼び出しを生成しません `Execute` 。

Viewmodel にコマンド実行を追加する方法の詳細については、を Xamarin.Forms 実装する2つのクラスを定義します。は、と `ICommand` `Command` `Command<T>` `T` の引数の型です `Execute` `CanExecute` 。 これらの2つのクラスは、複数のコンストラクターと、 `ChangeCanExecute` オブジェクトがイベントを発生させるためにビューモデルで呼び出すことができるメソッドを定義し `Command` `CanExecuteChanged` ます。

ここでは、電話番号を入力するための簡単なキーパッドのビューモデルを示します。 `Execute` `CanExecute` メソッドとメソッドが、コンストラクターでラムダ関数として定義されていることに注意してください。

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

このビューモデルは、 `AddCharCommand` プロパティが `Command` いくつかのボタン (またはコマンドインターフェイスを持つ他のもの) のプロパティにバインドされていることを前提としています。これらはそれぞれ、によって識別され `CommandParameter` ます。 これらのボタンは、プロパティに文字を追加 `InputString` します。これは、プロパティの電話番号として書式設定され `DisplayText` ます。

また、という型の2番目のプロパティもあり `ICommand` `DeleteCharCommand` ます。 これは、[戻る] ボタンにバインドされていますが、削除する文字がない場合は、ボタンを無効にする必要があります。

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

`Command`このマークアップに表示される最初ののプロパティは、にバインドされています。残りのは、 `Button` `DeleteCharCommand` `AddCharCommand` `CommandParameter` 面に表示される文字と同じを持つにバインドされ `Button` ます。 次のプログラムが動作しています。

[![](data-bindings-to-mvvm-images/keypad.png "Calculator using MVVM and Commands")](data-bindings-to-mvvm-images/keypad-large.png#lightbox "Calculator using MVVM and Commands")

### <a name="invoking-asynchronous-methods"></a>非同期メソッドの呼び出し

コマンドは、非同期メソッドを呼び出すこともできます。 これを実現するには `async` 、 `await` メソッドを指定するときにキーワードとキーワードを使用し `Execute` ます。

```csharp
DownloadCommand = new Command (async () => await DownloadAsync ());
```

これ `DownloadAsync` は、メソッドがで、 `Task` 待機する必要があることを示しています。

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

この一連の記事のすべてのソースコードを含む[Xamlsamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)プログラムは、ホームページのビュービューを使用します。 このビューモデルは、、、およびという3つのプロパティを持つ短いクラスを定義したもので、 `Type` `Title` `Description` 各サンプルページの種類、タイトル、および簡単な説明が含まれています。 さらに、ビューモデルは、 `All` プログラム内のすべてのページのコレクションであるという名前の静的プロパティを定義します。

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

の XAML ファイルは `MainPage` 、プロパティ `ListBox` `ItemsSource` がプロパティに設定され、 `All` `TextCell` `Title` `Description` 各ページのプロパティとプロパティを表示するためのを含むを定義します。

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

ユーザーが項目を選択すると、分離コードファイル内のハンドラーがトリガーされます。 ハンドラーは、の `SelectedItem` プロパティ `ListBox` をに戻し `null` てから、選択したページをインスタンス化して移動します。

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

**Xamarin の進化 2016: MVVM と Prism を使用したシンプルな作成 Xamarin.Forms**

## <a name="summary"></a>まとめ

XAML は、アプリケーションでユーザーインターフェイスを定義するための強力なツールです Xamarin.Forms 。特に、データバインディングと MVVM を使用する場合に使用します。 結果として、コード内のすべてのバックグラウンドサポートを使用して、ユーザーインターフェイスをクリーンで洗練された形式で表現できます。

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
