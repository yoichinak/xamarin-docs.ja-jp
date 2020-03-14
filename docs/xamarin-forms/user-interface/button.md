---
title: Xamarin.Forms のボタン
description: ボタンは、タップまたは特定のタスクを実行するためにアプリケーションに指示するクリックに応答します。
ms.prod: xamarin
ms.assetid: 62CAEB63-0800-44F4-9B8C-EE632138C2F5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/04/2019
ms.openlocfilehash: f82d590213076f349b21ebdee2832f2bf474d2f2
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305847"
---
# <a name="xamarinforms-button"></a>Xamarin.Forms のボタン

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos)

_このボタンは、特定のタスクを実行するようにアプリケーションに指示する tap または click に応答します。_

[`Button`](xref:Xamarin.Forms.Button)は、すべての Xamarin. フォームで最も基本的な対話型コントロールです。 通常、`Button` にはコマンドを示す短いテキスト文字列が表示されますが、ビットマップイメージ、またはテキストとイメージの組み合わせを表示することもできます。 ユーザーは、`Button` を指で押すか、マウスでクリックしてそのコマンドを開始します。

以下で説明するほとんどのトピックは、 [**Buttondemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos)サンプルのページに対応しています。

## <a name="handling-button-clicks"></a>クリックしたボタンの処理

`Button` は、ユーザーが指またはマウスポインターを使用して `Button` をタップしたときに発生する[`Clicked`](xref:Xamarin.Forms.Button.Clicked)イベントを定義します。 このイベントは、`Button`の表面から指またはマウスボタンが離されたときに発生します。 タップに応答するには、`Button` の[`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled)プロパティが `true` に設定されている必要があります。

[**Buttondemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos)サンプルの**基本的なボタンクリック**のページは、XAML で `Button` をインスタンス化し、その `Clicked` イベントを処理する方法を示しています。 **Basicbuttonclickpage .xaml**ファイルには、`Label` と `Button`の両方を含む `StackLayout` が含まれています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.BasicButtonClickPage"
             Title="Basic Button Click">
    <StackLayout>

        <Label x:Name="label"
               Text="Click the Button below"
               FontSize="Large"
               VerticalOptions="CenterAndExpand"
               HorizontalOptions="Center" />

        <Button Text="Click to Rotate Text!"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Clicked="OnButtonClicked" />

    </StackLayout>
</ContentPage>
```

`Button` は、許可されているすべての領域を占有する傾向があります。 たとえば、`Button` の `HorizontalOptions` プロパティを `Fill`以外の値に設定していない場合、`Button` はその親の完全な幅を占有します。

既定では、`Button` は四角形ですが、後[**のセクションで説明する**](#button-appearance)ように、 [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius)プロパティを使用して角を丸くすることができます。

[`Text`](xref:Xamarin.Forms.Button.Text) プロパティは、`Button` に表示するテキストを指定します。 [`Clicked`](xref:Xamarin.Forms.Button.Clicked)イベントは、`OnButtonClicked`という名前のイベントハンドラーに設定されます。 このハンドラーは、分離コードファイル**BasicButtonClickPage.xaml.cs**にあります。

```csharp
public partial class BasicButtonClickPage : ContentPage
{
    public BasicButtonClickPage ()
    {
        InitializeComponent ();
    }

    async void OnButtonClicked(object sender, EventArgs args)
    {
        await label.RelRotateTo(360, 1000);
    }
}
```

`Button` がタップされると、`OnButtonClicked` メソッドが実行されます。 `sender` 引数は、このイベントを担当する `Button` オブジェクトです。 これを使用すると、`Button` オブジェクトにアクセスしたり、同じ `Clicked` イベントを共有する複数の `Button` オブジェクトを区別したりできます。

この特定の `Clicked` ハンドラーは、1000ミリ秒で `Label` 360 °を回転するアニメーション関数を呼び出します。 Windows 10 デスクトップおよびユニバーサル Windows プラットフォーム (UWP) アプリケーションとして iOS および Android のデバイスでを実行しているプログラムを次に示します。

[![基本ボタンのクリック](button-images/BasicButtonClick.png "基本ボタンのクリック")](button-images/BasicButtonClick-Large.png#lightbox "基本ボタンのクリック")

イベントハンドラー内で `await` が使用されているため、`OnButtonClicked` メソッドに `async` 修飾子が含まれていることに注意してください。 `Clicked` イベントハンドラーでは、ハンドラーの本体で `await`が使用されている場合にのみ、`async` 修飾子が必要です。

各プラットフォームは、独自の方法で `Button` をレンダリングします。 [[**ボタンの外観**](#button-appearance)] セクションでは、色を設定し、カスタマイズされた外観に `Button` の境界線を表示する方法について説明します。 `Button` には[`IFontElement`](xref:Xamarin.Forms.Internals.IFontElement)インターフェイスが実装されているため、 [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily)、 [`FontSize`](xref:Xamarin.Forms.Button.FontSize)、および[`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes)の各プロパティが含まれています。

## <a name="creating-a-button-in-code"></a>コードでボタンを作成します。

XAML で `Button` をインスタンス化することは一般的ですが、コードで `Button` を作成することもできます。 これは、アプリケーションで、`foreach` ループで列挙可能なデータに基づいて複数のボタンを作成する必要がある場合に便利です。

**コードボタンのクリック**ページは、**基本的なボタンクリック**ページと機能的に同等のページを作成する方法を示しC#ていますが、完全には次のとおりです。

```csharp
public class CodeButtonClickPage : ContentPage
{
    public CodeButtonClickPage ()
    {
        Title = "Code Button Click";

        Label label = new Label
        {
            Text = "Click the Button below",
            FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)),
            VerticalOptions = LayoutOptions.CenterAndExpand,
            HorizontalOptions = LayoutOptions.Center
        };

        Button button = new Button
        {
            Text = "Click to Rotate Text!",
            VerticalOptions = LayoutOptions.CenterAndExpand,
            HorizontalOptions = LayoutOptions.Center
        };
        button.Clicked += async (sender, args) => await label.RelRotateTo(360, 1000);

        Content = new StackLayout
        {
            Children =
            {
                label,
                button
            }
        };
    }
}
```

クラスのコンス トラクターでは、すべて行われます。 `Clicked` ハンドラーは1つのステートメントだけであるため、単純にイベントにアタッチできます。

```csharp
button.Clicked += async (sender, args) => await label.RelRotateTo(360, 1000);
```

もちろん、イベントハンドラーを別のメソッドとして定義することもできます (**基本的なボタンクリック**の `OnButtonClick` メソッドと同様)。そのメソッドをイベントにアタッチします。

```csharp
button.Clicked += OnButtonClicked;
```

## <a name="disabling-the-button"></a>ボタンを無効にします。

アプリケーションが特定の状態にある場合、特定の `Button` クリックが有効な操作ではないことがあります。 そのような場合は、`IsEnabled` プロパティを `false`に設定して、`Button` を無効にする必要があります。 従来の例は、ファイルオープン `Button`を伴うファイル名の `Entry` コントロールです。 `Button` は、何らかのテキストが `Entry`に入力されている場合にのみ有効にする必要があります。
[**データトリガー**](~/xamarin-forms/app-fundamentals/triggers.md#data-triggers)に関する記事に示すように、このタスクには `DataTrigger` を使用できます。

## <a name="using-the-command-interface"></a>コマンド インターフェイスを使用します。

アプリケーションが `Clicked` イベントを処理せずに `Button` タップに応答する可能性があります。 `Button`_には、コマンドまたは_ _コマンド_実行インターフェイスと呼ばれる別の通知メカニズムが実装されています。 これは、2 つのプロパティで構成されます。

- [`System.Windows.Input`](xref:System.Windows.Input)名前空間で定義されているインターフェイス[`ICommand`](xref:System.Windows.Input.ICommand)型の[`Command`](xref:Xamarin.Forms.Button.Command) 。
- [`Object`](xref:System.Object)型の[`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter)プロパティです。

このアプローチは、モデル-ビュー-ビューモデル (MVVM) アーキテクチャを実装する場合に特に関連データ バインディング、およびに特に適しています。 これらのトピックについては、データ[バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)、 [MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)、 [MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md)の各記事で説明されています。

MVVM アプリケーションでは、データバインディングを使用して XAML `Button` 要素に接続する `ICommand` 型のプロパティがモデルビューによって定義されます。 また、Xamarin. Forms は、`ICommand` インターフェイスを実装する[`Command`](xref:Xamarin.Forms.Command)クラスと[`Command<T>`](xref:Xamarin.Forms.Command`1)クラスを定義し、`ICommand`型のプロパティを定義するためのビューモデルを支援します。

コマンド実行の詳細については、「[**コマンドインターフェイス**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md)」を参照してください。ただし、 [**Buttondemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos)サンプルの**基本的なボタンコマンド**ページでは、基本的な方法を示しています。

`CommandDemoViewModel` クラスは、`Number`という名前の `double` 型のプロパティを定義する非常に単純なビューモデルであり、`MultiplyBy2Command` と `DivideBy2Command`という型の2つのプロパティ `ICommand` を定義します。

```csharp
class CommandDemoViewModel : INotifyPropertyChanged
{
    double number = 1;

    public event PropertyChangedEventHandler PropertyChanged;

    public CommandDemoViewModel()
    {
        MultiplyBy2Command = new Command(() => Number *= 2);

        DivideBy2Command = new Command(() => Number /= 2);
    }

    public double Number
    {
        set
        {
            if (number != value)
            {
                number = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Number"));
            }
        }
        get
        {
            return number;
        }
    }

    public ICommand MultiplyBy2Command { private set; get; }

    public ICommand DivideBy2Command { private set; get; }
}
```

2つの `ICommand` プロパティは、`Command`型の2つのオブジェクトを持つクラスのコンストラクターで初期化されます。 `Command` コンストラクターには、`Number` プロパティを2倍にするか半分にする関数 (`execute` コンストラクター引数と呼ばれる) が含まれています。

**Basicbuttoncommand .xaml**ファイルは、その `BindingContext` を `CommandDemoViewModel`のインスタンスに設定します。 `Label` 要素と2つの `Button` 要素には、`CommandDemoViewModel`内の3つのプロパティへのバインドが含まれています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.BasicButtonCommandPage"
             Title="Basic Button Command">

    <ContentPage.BindingContext>
        <local:CommandDemoViewModel />
    </ContentPage.BindingContext>

    <StackLayout>
        <Label Text="{Binding Number, StringFormat='Value is now {0}'}"
               FontSize="Large"
               VerticalOptions="CenterAndExpand"
               HorizontalOptions="Center" />

        <Button Text="Multiply by 2"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Command="{Binding MultiplyBy2Command}" />

        <Button Text="Divide by 2"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Command="{Binding DivideBy2Command}" />
    </StackLayout>
</ContentPage>
```

2つの `Button` 要素がタップされると、コマンドが実行され、number の値が変わります。

[![[基本] ボタンコマンド](button-images/BasicButtonCommand.png "[基本] ボタンコマンド")](button-images/BasicButtonCommand-Large.png#lightbox)

`Clicked` ハンドラーに対するこのアプローチの利点は、このページの機能を含むすべてのロジックが分離コードファイルではなく、モデルビューに配置されることです。これにより、ビジネスロジックからのユーザーインターフェイスの分離が向上します。

また、`Command` オブジェクトは、`Button` 要素の有効化と無効化を制御することもできます。 たとえば<sup>、2 ~ 2</sup> <sup>&ndash;10</sup>の範囲の数値を制限するとします。 `Button` を有効にする必要がある場合は `true` を返すコンストラクターに別の関数を追加できます (`canExecute` 引数と呼ばれます)。 `CommandDemoViewModel` コンストラクターに対する変更を次に示します。

```csharp
class CommandDemoViewModel : INotifyPropertyChanged
{
    ···
    public CommandDemoViewModel()
    {
        MultiplyBy2Command = new Command(
            execute: () =>
            {
                Number *= 2;
                ((Command)MultiplyBy2Command).ChangeCanExecute();
                ((Command)DivideBy2Command).ChangeCanExecute();
            },
            canExecute: () => Number < Math.Pow(2, 10));

        DivideBy2Command = new Command(
            execute: () =>
            {
                Number /= 2;
                ((Command)MultiplyBy2Command).ChangeCanExecute();
                ((Command)DivideBy2Command).ChangeCanExecute();
            },
            canExecute: () => Number > Math.Pow(2, -10));
    }
    ···
}
```

`Command` メソッドが `canExecute` メソッドを呼び出し、`Button` を無効にする必要があるかどうかを判断するには、`Command` の `ChangeCanExecute` メソッドの呼び出しが必要です。 このコードを変更すると、数値が上限に達すると `Button` が無効になります。

[![基本ボタンコマンド-変更済み](button-images/BasicButtonCommandModified.png "基本ボタンコマンド-変更済み")](button-images/BasicButtonCommandModified-Large.png#lightbox)

2つ以上の `Button` 要素が同じ `ICommand` プロパティにバインドされる可能性があります。 `Button` 要素は `Button`の[`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter)プロパティを使用して区別できます。 この場合は、ジェネリック[`Command<T>`](xref:Xamarin.Forms.Command`1)クラスを使用します。 `CommandParameter` オブジェクトは、`execute` メソッドと `canExecute` メソッドに引数として渡されます。 この手法の詳細については、[**コマンドインターフェイス**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding)の記事の「[**基本的な**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding)コマンド実行」セクションを参照してください。

また、 [**Buttondemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos)のサンプルでは、`MainPage` クラスでもこの手法を使用します。 **Mainpage.xaml**ファイルには、サンプルの各ページの `Button` が含まれています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.MainPage"
             Title="Button Demos">
    <ScrollView>
        <FlexLayout Direction="Column"
                    JustifyContent="SpaceEvenly"
                    AlignItems="Center">

            <Button Text="Basic Button Click"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:BasicButtonClickPage}" />

            <Button Text="Code Button Click"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:CodeButtonClickPage}" />

            <Button Text="Basic Button Command"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:BasicButtonCommandPage}" />

            <Button Text="Press and Release Button"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:PressAndReleaseButtonPage}" />

            <Button Text="Button Appearance"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:ButtonAppearancePage}" />

            <Button Text="Toggle Button Demo"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:ToggleButtonDemoPage}" />

            <Button Text="Image Button Demo"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:ImageButtonDemoPage}" />

        </FlexLayout>
    </ScrollView>
</ContentPage>
```

各 `Button` には、`NavigateCommand`という名前のプロパティにバインドされた `Command` プロパティがあり、`CommandParameter` はプロジェクト内のいずれかのページクラスに対応する[`Type`](xref:System.Type)オブジェクトに設定されます。

この `NavigateCommand` プロパティは `ICommand` 型で、分離コードファイルで定義されています。

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();

        NavigateCommand = new Command<Type>(async (Type pageType) =>
        {
            Page page = (Page)Activator.CreateInstance(pageType);
            await Navigation.PushAsync(page);
        });

        BindingContext = this;
    }

    public ICommand NavigateCommand { private set; get; }
}
```

コンストラクターは、XAML ファイルで設定されて `Type` いる `CommandParameter` オブジェクトの型であるため、`NavigateCommand` プロパティを `Command<Type>` オブジェクトに初期化します。 これは、`execute` メソッドに、この `CommandParameter` オブジェクトに対応する `Type` 型の引数があることを意味します。 関数では、ページをインスタンス化し、し、それに移動します。

コンストラクターが終了するのは、その `BindingContext` をそれ自体に設定することです。 これは、XAML ファイルのプロパティを `NavigateCommand` プロパティにバインドするために必要です。

## <a name="pressing-and-releasing-the-button"></a>キーを押すと、ボタンを放す

`Clicked` イベントのほかに、`Button` は [`Pressed`](xref:Xamarin.Forms.Button.Pressed) イベントと [`Released`](xref:Xamarin.Forms.Button.Released) イベントも定義します。 `Pressed` イベントは、指が `Button`を押したとき、または `Button`上にポインターを置いた状態でマウスボタンが押されたときに発生します。 `Released` イベントは、指またはマウスボタンが離されたときに発生します。 一般に、`Clicked` イベントも `Released` イベントと同時に発生しますが、指またはマウスポインターが `Button` の画面から離れると、`Clicked` イベントが発生しない可能性があります。

`Pressed` イベントと `Released` イベントはよく使用されませんが、 **[プレスアンドリリース] ボタン**ページで説明されているように、特殊な目的で使用できます。 XAML ファイルには、`Label` と、`Pressed` および `Released` イベントに関連付けられたハンドラーを含む `Button` が含まれています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.PressAndReleaseButtonPage"
             Title="Press and Release Button">
    <StackLayout>

        <Label x:Name="label"
               Text="Press and hold the Button below"
               FontSize="Large"
               VerticalOptions="CenterAndExpand"
               HorizontalOptions="Center" />

        <Button Text="Press to Rotate Text!"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Pressed="OnButtonPressed"
                Released="OnButtonReleased" />

    </StackLayout>
</ContentPage>
```

分離コードファイルは、`Pressed` イベントが発生したときに `Label` をアニメーション化しますが、`Released` イベントが発生したときにローテーションを中断します。

```csharp
public partial class PressAndReleaseButtonPage : ContentPage
{
    bool animationInProgress = false;
    Stopwatch stopwatch = new Stopwatch();

    public PressAndReleaseButtonPage ()
    {
        InitializeComponent ();
    }

    void OnButtonPressed(object sender, EventArgs args)
    {
        stopwatch.Start();
        animationInProgress = true;

        Device.StartTimer(TimeSpan.FromMilliseconds(16), () =>
        {
            label.Rotation = 360 * (stopwatch.Elapsed.TotalSeconds % 1);

            return animationInProgress;
        });
    }

    void OnButtonReleased(object sender, EventArgs args)
    {
        animationInProgress = false;
        stopwatch.Stop();
    }
}
```

結果として、`Label` は指が `Button`と接触している間だけ回転し、指が離されると停止します。

[![プレスアンドリリースボタン](button-images/PressAndReleaseButton.png "プレスアンドリリースボタン")](button-images/PressAndReleaseButton-Large.png)

このような動作には、ゲーム用のアプリケーションがあります。 `Button` 上に指を置いた指は、画面上のオブジェクトを特定の方向に移動させることができます。

<a name="button-appearance" />

## <a name="button-appearance"></a>ボタンの外観

`Button` は、その外観に影響を与えるいくつかのプロパティを継承または定義します。

- [`TextColor`](xref:Xamarin.Forms.Button.TextColor)は `Button` テキストの色です。
- [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor)は、そのテキストの背景色です。
- [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor)は、`Button` 周辺の領域の色です。
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily)は、テキストに使用されるフォントファミリです。
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize)はテキストのサイズです。
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes)テキストが斜体か太字かを示します
- [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth)は境界線の幅です。
- [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius)は `Button` の角の半径です。
- `CharacterSpacing` `Button` テキストの文字間隔

> [!NOTE]
> `Button` クラスには、`Button`のレイアウト動作を制御するプロパティ[`Margin`](xref:Xamarin.Forms.View.Margin)と[`Padding`](xref:Xamarin.Forms.Button.Padding)もあります。 詳細については「[Margin and Padding](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)」 (余白とスペース) を参照してください。

これらの6つのプロパティ (`FontFamily` と `FontAttributes`を除く) の効果は、 **[ボタンの外観]** ページに示されています。 別のプロパティである[`Image`](xref:Xamarin.Forms.Button.ImageSource)については、「[**ボタンを使用したビットマップの使用**](#image-button)」セクションで説明します。

**[ボタンの外観**] ページのすべてのビューとデータバインディングは、XAML ファイルで定義されています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.ButtonAppearancePage"
             Title="Button Appearance">
    <StackLayout>
        <Button x:Name="button"
                Text="Button"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                TextColor="{Binding Source={x:Reference textColorPicker},
                                    Path=SelectedItem.Color}"
                BackgroundColor="{Binding Source={x:Reference backgroundColorPicker},
                                          Path=SelectedItem.Color}"
                BorderColor="{Binding Source={x:Reference borderColorPicker},
                                      Path=SelectedItem.Color}" />

        <StackLayout BindingContext="{x:Reference button}"
                     Padding="10">

            <Slider x:Name="fontSizeSlider"
                    Maximum="48"
                    Minimum="1"
                    Value="{Binding FontSize}" />

            <Label Text="{Binding Source={x:Reference fontSizeSlider},
                                  Path=Value,
                                  StringFormat='FontSize = {0:F0}'}"
                   HorizontalTextAlignment="Center" />

            <Slider x:Name="borderWidthSlider"
                    Minimum="-1"
                    Maximum="12"
                    Value="{Binding BorderWidth}" />

            <Label Text="{Binding Source={x:Reference borderWidthSlider},
                                  Path=Value,
                                  StringFormat='BorderWidth = {0:F0}'}"
                   HorizontalTextAlignment="Center" />

            <Slider x:Name="cornerRadiusSlider"
                    Minimum="-1"
                    Maximum="24"
                    Value="{Binding CornerRadius}" />

            <Label Text="{Binding Source={x:Reference cornerRadiusSlider},
                                  Path=Value,
                                  StringFormat='CornerRadius = {0:F0}'}"
                   HorizontalTextAlignment="Center" />

            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>

                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>

                <Grid.Resources>
                    <Style TargetType="Label">
                        <Setter Property="VerticalOptions" Value="Center" />
                    </Style>
                </Grid.Resources>

                <Label Text="Text Color:"
                       Grid.Row="0" Grid.Column="0" />

                <Picker x:Name="textColorPicker"
                        ItemsSource="{Binding Source={x:Static local:NamedColor.All}}"
                        ItemDisplayBinding="{Binding FriendlyName}"
                        SelectedIndex="0"
                        Grid.Row="0" Grid.Column="1" />

                <Label Text="Background Color:"
                       Grid.Row="1" Grid.Column="0" />

                <Picker x:Name="backgroundColorPicker"
                        ItemsSource="{Binding Source={x:Static local:NamedColor.All}}"
                        ItemDisplayBinding="{Binding FriendlyName}"
                        SelectedIndex="0"
                        Grid.Row="1" Grid.Column="1" />

                <Label Text="Border Color:"
                       Grid.Row="2" Grid.Column="0" />

                <Picker x:Name="borderColorPicker"
                        ItemsSource="{Binding Source={x:Static local:NamedColor.All}}"
                        ItemDisplayBinding="{Binding FriendlyName}"
                        SelectedIndex="0"
                        Grid.Row="2" Grid.Column="1" />
            </Grid>
        </StackLayout>
    </StackLayout>
</ContentPage>
```

ページの上部にある `Button` には、ページの下部にある `Picker` 要素にバインドされた3つの `Color` プロパティがあります。 `Picker` 要素の項目は、プロジェクトに含まれている `NamedColor` クラスの色です。 3つの `Slider` 要素には、`Button`の `FontSize`、`BorderWidth`、および `CornerRadius` プロパティへの双方向のバインディングが含まれています。

このプログラムではこれらすべてのプロパティの組み合わせで実験を行うことができます。

[![ボタンの外観](button-images/ButtonAppearance.png "ボタンの外観")](button-images/ButtonAppearance-Large.png)

`Button` の境界線を表示するには、`BorderColor` を `Default`以外に設定し、`BorderWidth` を正の値に設定する必要があります。

IOS では、大きな罫線の幅が `Button` の内部に割り込ん、テキストの表示が妨げられることがわかります。 IOS `Button`で罫線を使用することを選択した場合は、その可視性を維持するために、`Text` プロパティの先頭と末尾にスペースを使用することをお勧めします。

UWP で、`Button` の半分を超える `CornerRadius` を選択すると、例外が発生します。

## <a name="button-visual-states"></a>ボタンのビジュアル状態

[`Button`](xref:Xamarin.Forms.Button)には、有効になっている場合にユーザーが押したときに `Button` に対する視覚的な変更を開始するために使用できる `Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState)があります。

次の XAML の例は、`Pressed` 状態の表示状態を定義する方法を示しています。

```xaml
<Button Text="Click me!"
        ...>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="Scale"
                            Value="1" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Pressed">
                <VisualState.Setters>
                    <Setter Property="Scale"
                            Value="0.8" />
                </VisualState.Setters>
            </VisualState>

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Button>
```

`Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState)は、 [`Button`](xref:Xamarin.Forms.Button)が押されたときに[`Scale`](xref:Xamarin.Forms.VisualElement.Scale)プロパティが既定値の1から0.8 に変更されることを指定します。 `Normal` `VisualState` は、`Button` が通常の状態であるときに、その `Scale` プロパティが1に設定されることを指定します。 したがって、全体的な影響として、`Button` が押されているときには少し小さくスケール、`Button` が解放されると、既定のサイズにスケールます。

表示状態の詳細については、「 [Xamarin Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)」を参照してください。

## <a name="creating-a-toggle-button"></a>トグル ボタンを作成します。

`Button` をサブクラス化して、オンオフスイッチのように動作させることができます。ボタンを1回タップすると、ボタンがオンに切り替わり、もう一度タップしてオフに切り替わります。

次の `ToggleButton` クラスは `Button` から派生し、`Toggled` という名前の新しいイベントと `IsToggled`という名前のブール型プロパティを定義します。 これらは、Xamarin. Forms [`Switch`](xref:Xamarin.Forms.Switch)によって定義されている2つのプロパティと同じです。

```csharp
class ToggleButton : Button
{
    public event EventHandler<ToggledEventArgs> Toggled;

    public static BindableProperty IsToggledProperty =
        BindableProperty.Create("IsToggled", typeof(bool), typeof(ToggleButton), false,
                                propertyChanged: OnIsToggledChanged);

    public ToggleButton()
    {
        Clicked += (sender, args) => IsToggled ^= true;
    }

    public bool IsToggled
    {
        set { SetValue(IsToggledProperty, value); }
        get { return (bool)GetValue(IsToggledProperty); }
    }

    protected override void OnParentSet()
    {
        base.OnParentSet();
        VisualStateManager.GoToState(this, "ToggledOff");
    }

    static void OnIsToggledChanged(BindableObject bindable, object oldValue, object newValue)
    {
        ToggleButton toggleButton = (ToggleButton)bindable;
        bool isToggled = (bool)newValue;

        // Fire event
        toggleButton.Toggled?.Invoke(toggleButton, new ToggledEventArgs(isToggled));

        // Set the visual state
        VisualStateManager.GoToState(toggleButton, isToggled ? "ToggledOn" : "ToggledOff");
    }
}
```

`ToggleButton` コンストラクターは、`Clicked` イベントにハンドラーをアタッチして、`IsToggled` プロパティの値を変更できるようにします。 `OnIsToggledChanged` メソッドは、`Toggled` イベントを発生させます。

`OnIsToggledChanged` メソッドの最後の行は、2つのテキスト文字列 "ToggledOn" と "ToggledOff" を持つ静的 `VisualStateManager.GoToState` メソッドを呼び出します。 このメソッドについて、およびアプリケーションがビジュアル状態に応答する方法については、「 [**Xamarin. Forms Visual State Manager**](~/xamarin-forms/user-interface/visual-state-manager.md)」を参照してください。

`ToggleButton` によって `VisualStateManager.GoToState`が呼び出されるため、クラス自体には、その `IsToggled` の状態に基づいてボタンの外観を変更するための追加の機能を含める必要はありません。 これは、`ToggleButton`をホストする XAML の役割です。

**トグルボタンのデモ**ページには `ToggleButton`の2つのインスタンスが含まれています。これには、表示状態に基づいてボタンの `Text`、`BackgroundColor`、および `TextColor` を設定する Visual state Manager マークアップが含まれます。

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.ToggleButtonDemoPage"
             Title="Toggle Button Demo">

    <ContentPage.Resources>
        <Style TargetType="local:ToggleButton">
            <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            <Setter Property="HorizontalOptions" Value="Center" />
        </Style>
    </ContentPage.Resources>

    <StackLayout Padding="10, 0">
        <local:ToggleButton Toggled="OnItalicButtonToggled">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ToggleStates">
                    <VisualState Name="ToggledOff">
                        <VisualState.Setters>
                            <Setter Property="Text" Value="Italic Off" />
                            <Setter Property="BackgroundColor" Value="#C0C0C0" />
                            <Setter Property="TextColor" Value="Black" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState Name="ToggledOn">
                        <VisualState.Setters>
                            <Setter Property="Text" Value=" Italic On " />
                            <Setter Property="BackgroundColor" Value="#404040" />
                            <Setter Property="TextColor" Value="White" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </local:ToggleButton>

        <local:ToggleButton Toggled="OnBoldButtonToggled">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ToggleStates">
                    <VisualState Name="ToggledOff">
                        <VisualState.Setters>
                            <Setter Property="Text" Value="Bold Off" />
                            <Setter Property="BackgroundColor" Value="#C0C0C0" />
                            <Setter Property="TextColor" Value="Black" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState Name="ToggledOn">
                        <VisualState.Setters>
                            <Setter Property="Text" Value=" Bold On " />
                            <Setter Property="BackgroundColor" Value="#404040" />
                            <Setter Property="TextColor" Value="White" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </local:ToggleButton>

        <Label x:Name="label"
               Text="Just a little passage of some sample text that can be formatted in italic or boldface by toggling the two buttons."
               FontSize="Large"
               HorizontalTextAlignment="Center"
               VerticalOptions="CenterAndExpand" />

    </StackLayout>
</ContentPage>
```

`Toggled` イベントハンドラーは、分離コードファイルに含まれています。 これらは、ボタンの状態に基づいて `Label` の [`FontAttributes`] プロパティを設定する役割を担います。

```csharp
public partial class ToggleButtonDemoPage : ContentPage
{
    public ToggleButtonDemoPage ()
    {
        InitializeComponent ();
    }

    void OnItalicButtonToggled(object sender, ToggledEventArgs args)
    {
        if (args.Value)
        {
            label.FontAttributes |= FontAttributes.Italic;
        }
        else
        {
            label.FontAttributes &= ~FontAttributes.Italic;
        }
    }

    void OnBoldButtonToggled(object sender, ToggledEventArgs args)
    {
        if (args.Value)
        {
            label.FontAttributes |= FontAttributes.Bold;
        }
        else
        {
            label.FontAttributes &= ~FontAttributes.Bold;
        }
    }
}
```

IOS、Android、および UWP で実行されているプログラムを次に示します。

[![トグルボタンのデモ](button-images/ToggleButtonDemo.png "トグルボタンのデモ")](button-images/ToggleButtonDemo-Large.png#lightbox)

<a name="image-button" />

## <a name="using-bitmaps-with-buttons"></a>ボタンにビットマップを使用します。

`Button` クラスは[`ImageSource`](xref:Xamarin.Forms.Button.Image)プロパティを定義します。このプロパティを使用すると、`Button`にビットマップイメージを単独で表示することも、テキストと組み合わせて表示することもできます。 テキストとイメージの配置方法を指定することもできます。

`ImageSource` プロパティは[`ImageSource`](xref:Xamarin.Forms.ImageSource)型です。つまり、ビットマップをファイル、埋め込みリソース、URI、またはストリームから読み込むことができます。

> [!NOTE]
> `Button` は、アニメーション GIF を読み込むことができますが、GIF の最初のフレームのみが表示されます。

Xamarin.Forms でサポートされている各プラットフォームで、アプリケーションが実行するさまざまなデバイスのさまざまなピクセルの解像度の複数のサイズに格納されるイメージできます。 これら複数のビットマップをという名前またはデバイスのビデオに、オペレーティング システムが最適な一致を選択できますように格納されている画面の解像度。

`Button`上のビットマップの場合、最適なサイズは通常、必要な大きさに応じて、32 ~ 64 のデバイスに依存しない単位です。 この例で使用されるイメージは、48 のデバイスに依存しない単位のサイズに基づいています。

IOS プロジェクトの**Resources**フォルダーには、次の3つのイメージのサイズが含まれています。

- **/Resources/MonkeyFace.png**として格納された48ピクセルの四角形ビットマップ
- **/Resource/MonkeyFace@2x.png** として格納された96ピクセルの四角形のビットマップ。
- **/Resource/MonkeyFace@3x.png** として格納された144ピクセルの四角形のビットマップ。

3つのすべてのビットマップには、 **BundleResource**の**ビルドアクション**が割り当てられています。

Android プロジェクトの場合、すべてのビットマップに同じ名前が付いていますが、 **Resources**フォルダーの別のサブフォルダーに格納されています。

- **/Resources/drawable-hdpi/MonkeyFace.png**として格納された72ピクセルの四角形ビットマップ
- **/Resources/drawable-xhdpi/MonkeyFace.png**として格納された96ピクセルの四角形ビットマップ
- **/Resources/drawable-xxhdpi/MonkeyFace.png**として格納された144ピクセルの四角形ビットマップ
- **/Resources/drawable-xxxhdpi/MonkeyFace.png**として格納された192ピクセルの四角形ビットマップ

これらには、 **Androidresource**の**ビルドアクション**が指定されました。

UWP プロジェクトでは、プロジェクト内の任意の場所にビットマップを保存できますが、通常はカスタムフォルダーまたは**アセット**の既存のフォルダーに格納されます。 UWP プロジェクトには、これらのビットマップが含まれています。

- **/Assets/MonkeyFace.scale-100.png**として格納された48ピクセルの四角形ビットマップ
- **/Assets/MonkeyFace.scale-200.png**として格納された96ピクセルの四角形ビットマップ
- **/Assets/MonkeyFace.scale-400.png**として格納された192ピクセルの四角形ビットマップ

すべての**コンテンツ**の**ビルドアクション**が指定されました。

`Button`の[`ContentLayout`](xref:Xamarin.Forms.Button.ContentLayout)プロパティを使用して、`Text` および `ImageSource` プロパティを `Button` にどのように配置するかを指定できます。 このプロパティの型は[`ButtonContentLayout`](xref:Xamarin.Forms.Button.ButtonContentLayout)です。これは `Button`の埋め込みクラスです。 [コンストラクター](xref:Xamarin.Forms.Button.ButtonContentLayout.%23ctor(Xamarin.Forms.Button.ButtonContentLayout.ImagePosition,System.Double))には、次の2つの引数があります。

- [`ImagePosition`](xref:Xamarin.Forms.Button.ButtonContentLayout.ImagePosition)列挙体のメンバー。 `Left`、`Top`、`Right`、または `Bottom`、テキストに対するビットマップの相対的な表示方法を示します。
- ビットマップとテキストの間の間隔を示す `double` 値。

既定値は `Left` と10単位です。 `ButtonContentLayout` 名前付き[`Position`](xref:Xamarin.Forms.Button.ButtonContentLayout.Position)と[`Spacing`](xref:Xamarin.Forms.Button.ButtonContentLayout.Spacing)の2つの読み取り専用プロパティは、これらのプロパティの値を提供します。

コードでは、`Button` を作成し、次のように `ContentLayout` プロパティを設定できます。

```csharp
Button button = new Button
{
    Text = "button text",
    ImageSource = new FileImageSource
    {
        File = "image filename"
    },
    ContentLayout = new Button.ButtonContentLayout(Button.ButtonContentLayout.ImagePosition.Right, 20)
};
```

XAML では、列挙型のメンバーまたは間隔を指定する必要があります。 またはコンマで区切られた任意の順序で両方。

```xaml
<Button Text="button text"
        ImageSource="image filename"
        ContentLayout="Right, 20" />
```

**イメージボタンのデモ**ページでは、`OnPlatform` を使用して、IOS、ANDROID、UWP ビットマップファイルに異なるファイル名を指定します。 プラットフォームごとに同じファイル名を使用し、`OnPlatform`を使用しないようにするには、UWP ビットマップをプロジェクトのルートディレクトリに格納する必要があります。

**イメージボタンのデモ**ページの最初の `Button` では、`Image` プロパティを設定しますが、`Text` プロパティは設定しません。

```xaml
<Button>
    <Button.ImageSource>
        <OnPlatform x:TypeArguments="ImageSource">
            <On Platform="iOS, Android" Value="MonkeyFace.png" />
            <On Platform="UWP" Value="Assets/MonkeyFace.png" />
        </OnPlatform>
    </Button.ImageSource>
</Button>
```

UWP のビットマップが、プロジェクトのルート ディレクトリに格納されている場合は、このマークアップをかなり簡略します。

```xaml
<Button ImageSource="MonkeyFace.png" />
```

**Imagebuttondemo .xaml**ファイルに多数の繰り返し実行するマークアップを回避するために、`ImageSource` プロパティを設定するための暗黙的な `Style` も定義されています。 この `Style` は、他の5つの `Button` 要素に自動的に適用されます。 完全な XAML ファイルを次に示します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.ImageButtonDemoPage">

    <FlexLayout Direction="Column"
                JustifyContent="SpaceEvenly"
                AlignItems="Center">

        <FlexLayout.Resources>
            <Style TargetType="Button">
                <Setter Property="ImageSource">
                    <OnPlatform x:TypeArguments="ImageSource">
                        <On Platform="iOS, Android" Value="MonkeyFace.png" />
                        <On Platform="UWP" Value="Assets/MonkeyFace.png" />
                    </OnPlatform>
                </Setter>
            </Style>
        </FlexLayout.Resources>

        <Button>
            <Button.ImageSource>
                <OnPlatform x:TypeArguments="ImageSource">
                    <On Platform="iOS, Android" Value="MonkeyFace.png" />
                    <On Platform="UWP" Value="Assets/MonkeyFace.png" />
                </OnPlatform>
            </Button.ImageSource>
        </Button>

        <Button Text="Default" />

        <Button Text="Left - 10"
                ContentLayout="Left, 10" />

        <Button Text="Top - 10"
                ContentLayout="Top, 10" />

        <Button Text="Right - 20"
                ContentLayout="Right, 20" />

        <Button Text="Bottom - 20"
                ContentLayout="Bottom, 20" />
    </FlexLayout>
</ContentPage>
```

最後の4つの `Button` 要素は、`ContentLayout` プロパティを使用して、テキストとビットマップの位置とスペースを指定します。

[![イメージボタンのデモ](button-images/ImageButtonDemo.png "イメージボタンのデモ")](button-images/ImageButtonDemo-Large.png#lightbox)

`Button` イベントを処理し、`Button` の外観を変更するためのさまざまな方法を説明しました。

## <a name="related-links"></a>関連リンク

- [ButtonDemos のサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos)
- [ボタンの API](xref:Xamarin.Forms.Button)
