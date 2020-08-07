---
title: Xamarin.Forms;
description: このボタンは、特定のタスクを実行するようにアプリケーションに指示する tap または click に応答します。
ms.prod: xamarin
ms.assetid: 62CAEB63-0800-44F4-9B8C-EE632138C2F5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/21/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7ed24d38c75036245a024eecbef7f9a74380b591
ms.sourcegitcommit: 08290d004d1a7e7ac579bf1f96abf8437921dc70
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/07/2020
ms.locfileid: "87917888"
---
# <a name="no-locxamarinforms-button"></a>Xamarin.Forms;

[![サンプルのダウンロード](~/media/shared/download.png) サンプルをダウンロードします](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos)

_このボタンは、特定のタスクを実行するようにアプリケーションに指示する tap または click に応答します。_

は、 [`Button`](xref:Xamarin.Forms.Button) すべてので最も基本的な対話型コントロールです Xamarin.Forms 。 は、 `Button` 通常、コマンドを示す短いテキスト文字列を表示しますが、ビットマップイメージ、またはテキストとイメージの組み合わせを表示することもできます。 ユーザーは、を `Button` 指で押すか、マウスでクリックしてそのコマンドを開始します。

以下で説明するほとんどのトピックは、 [**Buttondemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos)サンプルのページに対応しています。

## <a name="handling-button-clicks"></a>ボタンのクリックの処理

`Button`[`Clicked`](xref:Xamarin.Forms.Button.Clicked)ユーザーが `Button` 指またはマウスポインターを使用してをタップしたときに発生するイベントを定義します。 このイベントは、の表面から指またはマウスボタンが離されたときに発生し `Button` ます。 タップに応答するには、プロパティがに設定されている `Button` 必要があり [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) `true` ます。

[**Buttondemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos)サンプルの**基本的なボタンクリック**のページは、XAML でをインスタンス化し、そのイベントを処理する方法を示して `Button` `Clicked` います。 **Basicbuttonclickpage .xaml**ファイルには、 `StackLayout` との両方を含むが含まれています `Label` `Button` 。

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

は、 `Button` 許可されているすべての領域を占有する傾向があります。 たとえば、のプロパティを以外の値に設定した場合、 `HorizontalOptions` `Button` はその `Fill` `Button` 親の完全な幅を占有します。

既定では、は `Button` 四角形ですが、以下のセクションで説明するように、プロパティを使用して角を丸くすることができ [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius) ます。 [**Button appearance**](#button-appearance)

[`Text`](xref:Xamarin.Forms.Button.Text)プロパティは、に表示されるテキストを指定し `Button` ます。 [`Clicked`](xref:Xamarin.Forms.Button.Clicked)イベントは、という名前のイベントハンドラーに設定され `OnButtonClicked` ます。 このハンドラーは、分離コードファイル**BasicButtonClickPage.xaml.cs**にあります。

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

`Button` をタップすると、`OnButtonClicked` メソッドが実行されます。 `sender`引数は、 `Button` このイベントを処理するオブジェクトです。 これを使用すると、オブジェクトにアクセスし `Button` たり、同じイベントを共有する複数のオブジェクトを区別したりでき `Button` `Clicked` ます。

この `Clicked` ハンドラーは、 `Label` 1000 ミリ秒で360°を回転させるアニメーション関数を呼び出します。 IOS および Android デバイスで実行されているプログラムと、Windows 10 デスクトップ上のユニバーサル Windows プラットフォーム (UWP) アプリケーションを次に示します。

[![基本ボタンのクリック](button-images/BasicButtonClick.png "基本ボタンのクリック")](button-images/BasicButtonClick-Large.png#lightbox "基本ボタンのクリック")

`OnButtonClicked` `async` `await` イベントハンドラー内でが使用されているため、メソッドに修飾子が含まれていることに注意してください。 `Clicked`イベントハンドラーは、 `async` ハンドラーの本体でが使用されている場合にのみ、修飾子を必要とし `await` ます。

各プラットフォームは、 `Button` 独自の方法でをレンダリングします。 [[**ボタンの外観**](#button-appearance)] セクションでは、色を設定し、カスタマイズさ `Button` れた外観を表示するために境界線を表示する方法について説明します。 `Button`インターフェイスを実装し [`IFontElement`](xref:Xamarin.Forms.Internals.IFontElement) ます。そのため、、、およびの各プロパティが含まれ [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily) [`FontSize`](xref:Xamarin.Forms.Button.FontSize) [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes) ます。

## <a name="creating-a-button-in-code"></a>コードでのボタンの作成

XAML では、をインスタンス化するのが一般的です `Button` が、コードでを作成することもでき `Button` ます。 これは、アプリケーションで、ループで列挙可能なデータに基づいて複数のボタンを作成する必要がある場合に便利です `foreach` 。

**コードボタンのクリック**ページは、**基本的なボタンクリック**ページと機能的に同等のページを作成する方法を示していますが、完全に C# では次のようになります。

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

クラスのコンストラクターですべてが実行されます。 `Clicked`ハンドラーは1つのステートメントだけであるため、単純にイベントにアタッチできます。

```csharp
button.Clicked += async (sender, args) => await label.RelRotateTo(360, 1000);
```

もちろん、イベントハンドラーを別のメソッドとして定義し ( `OnButtonClick` **基本的なボタンクリック**のメソッドと同じように)、そのメソッドをイベントにアタッチすることもできます。

```csharp
button.Clicked += OnButtonClicked;
```

## <a name="disabling-the-button"></a>ボタンを無効にする

アプリケーションが特定の状態にある場合、特定の `Button` クリックが有効な操作ではないことがあります。 そのような場合は、 `Button` プロパティをに設定することで、を無効にする必要があり `IsEnabled` `false` ます。 従来の例とし `Entry` ては、ファイルオープンを伴うファイル名のコントロールがあります `Button` 。は、に `Button` 入力されたテキストがある場合にのみ有効にする必要があり `Entry` ます。
`DataTrigger`このタスクには、[**データトリガー**](~/xamarin-forms/app-fundamentals/triggers.md#data-triggers)に関する記事に示されているように、を使用できます。

## <a name="using-the-command-interface"></a>コマンドインターフェイスの使用

アプリケーションは、イベントを処理することなく、タップに応答することができ `Button` `Clicked` ます。 は、 `Button` コマンドまたは_コマンド_実行インターフェイスと呼ば_commanding_れる別の通知機構を実装します。 これは、次の2つのプロパティで構成されます。

- [`Command`](xref:Xamarin.Forms.Button.Command)型の [`ICommand`](xref:System.Windows.Input.ICommand) 。名前空間で定義されているインターフェイス [`System.Windows.Input`](xref:System.Windows.Input) 。
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter)型のプロパティ [`Object`](xref:System.Object) 。

この方法は、特にモデルビューモデル (MVVM) アーキテクチャを実装する場合に、データバインディングとの接続に適しています。 これらのトピックについては、データ[バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)、 [MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)、 [MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md)の各記事で説明されています。

MVVM アプリケーションでは、ビューモデルは、 `ICommand` データバインディングを使用して XAML 要素に接続される型のプロパティを定義し `Button` ます。 Xamarin.Formsまた、 [`Command`](xref:Xamarin.Forms.Command) は、インターフェイスを実装するクラスとクラスを定義し、 [`Command<T>`](xref:Xamarin.Forms.Command`1) `ICommand` 型のプロパティを定義する際にビューモデルを支援し `ICommand` ます。

コマンド実行の詳細については、「[**コマンドインターフェイス**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md)」を参照してください。ただし、 [**Buttondemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos)サンプルの**基本的なボタンコマンド**ページでは、基本的な方法を示しています。

クラスは、という `CommandDemoViewModel` 名前の型のプロパティ `double` と、 `Number` `ICommand` という名前の型の2つの `MultiplyBy2Command` プロパティ `DivideBy2Command` を定義する、非常に単純なビューモデルです。

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

2つの `ICommand` プロパティは、クラスのコンストラクターで、型の2つのオブジェクトを使用して初期化され `Command` ます。 コンストラクターには、 `Command` `execute` プロパティを2倍にするか半分にする関数 (コンストラクター引数と呼ばれる) が含まれて `Number` います。

**Basicbuttoncommand .xaml**ファイルは、 `BindingContext` をのインスタンスに設定し `CommandDemoViewModel` ます。 `Label`要素と2つの要素には、 `Button` 次の3つのプロパティへのバインドが含まれてい `CommandDemoViewModel` ます。

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

ハンドラーに対するこのアプローチの利点は、 `Clicked` このページの機能を含むすべてのロジックが分離コードファイルではなく、モデルビューに配置されることです。これにより、ビジネスロジックからのユーザーインターフェイスの分離が向上します。

また、オブジェクトを使用して、 `Command` 要素の有効化と無効化を制御することもでき `Button` ます。 たとえば<sup>、2 ~ 2 10 の</sup>範囲の数値を制限する<sup> &ndash; とします。</sup> を `canExecute` `true` 有効にする必要がある場合は、を返すコンストラクターに別の関数を追加できます (引数と呼ばれ `Button` ます)。 コンストラクターに対する変更を次に示し `CommandDemoViewModel` ます。

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

メソッドがメソッドを `ChangeCanExecute` 呼び出し、を `Command` 無効に `Command` `canExecute` する必要があるかどうかを判断するには、のメソッドを呼び出す必要があり `Button` ます。 このコードを変更すると、数が制限に達すると、 `Button` が無効になります。

[![基本ボタンコマンド-変更済み](button-images/BasicButtonCommandModified.png "基本ボタンコマンド-変更済み")](button-images/BasicButtonCommandModified-Large.png#lightbox)

2つ以上の `Button` 要素が同じプロパティにバインドされる可能性があり `ICommand` ます。 要素は、 `Button` のプロパティを使用して区別でき [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) `Button` ます。 この場合は、ジェネリッククラスを使用し [`Command<T>`](xref:Xamarin.Forms.Command`1) ます。 オブジェクトは、 `CommandParameter` メソッドおよびメソッドに引数として渡され `execute` `canExecute` ます。 この手法の詳細については、[**コマンドインターフェイス**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding)の記事の「[**基本的な**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding)コマンド実行」セクションを参照してください。

[**Buttondemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos)のサンプルでは、クラスでもこの手法を使用し `MainPage` ます。 **Mainpage.xaml**ファイルには、 `Button` サンプルの各ページのが含まれています。

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

各は `Button` `Command` 、という名前のプロパティにバインドされたプロパティを持ち、 `NavigateCommand` `CommandParameter` は [`Type`](xref:System.Type) プロジェクト内のいずれかのページクラスに対応するオブジェクトに設定されます。

この `NavigateCommand` プロパティの型はで `ICommand` 、分離コードファイルで定義されています。

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

コンストラクターは、 `NavigateCommand` `Command<Type>` `Type` が XAML ファイルで設定されているオブジェクトの型であるため、プロパティをオブジェクトに初期化し `CommandParameter` ます。 これは、メソッドに、 `execute` `Type` このオブジェクトに対応する型の引数があることを意味し `CommandParameter` ます。 関数は、ページをインスタンス化し、そのページに移動します。

コンストラクターは、をそれ自体に設定することによって終了し `BindingContext` ます。 これは、XAML ファイルのプロパティをプロパティにバインドするために必要です `NavigateCommand` 。

## <a name="pressing-and-releasing-the-button"></a>ボタンを押しながら離す

イベント以外 `Clicked` にも、イベント `Button` とイベントが定義され [`Pressed`](xref:Xamarin.Forms.Button.Pressed) [`Released`](xref:Xamarin.Forms.Button.Released) ます。 イベントは、指がで押され `Pressed` たとき、 `Button` またはマウスボタンが上にポインターを置いた状態で押されると発生し `Button` ます。 `Released`イベントは、指またはマウスボタンが離されたときに発生します。 一般に、イベントは `Clicked` イベントと同時に発生し `Released` ますが、指またはマウスポインターがの表面から離れ `Button` てから離されると、イベントは発生し `Clicked` ない可能性があります。

`Pressed`イベントとイベントは頻繁には使用され `Released` ませんが、[**プレスアンドリリース] ボタン**ページで説明されているように、特殊な目的で使用できます。 XAML ファイルには、および `Label` `Button` イベントにアタッチされたハンドラーを持つとが含まれてい `Pressed` `Released` ます。

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

分離コードファイルは、イベントが発生したときにをアニメーション化し `Label` `Pressed` ますが、イベントの発生時にはローテーションを中断し `Released` ます。

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

結果として、は `Label` 指がと接触している間だけ回転 `Button` し、指が離されると停止します。

[![プレスアンドリリースボタン](button-images/PressAndReleaseButton.png "プレスアンドリリースボタン")](button-images/PressAndReleaseButton-Large.png)

このような動作には、ゲーム用のアプリケーションがあります。に指を置くと、 `Button` 画面上のオブジェクトが特定の方向に移動する可能性があります。

## <a name="button-appearance"></a>ボタンの外観

は、 `Button` その外観に影響を与えるいくつかのプロパティを継承または定義します。

- [`TextColor`](xref:Xamarin.Forms.Button.TextColor)テキストの色です。 `Button`
- [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor)は、そのテキストの背景色です。
- [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor)は、を囲む領域の色です。`Button`
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily)テキストに使用するフォントファミリを入力します。
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize)は、テキストのサイズです。
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes)テキストが斜体と太字のどちらであるかを示します。
- [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth)罫線の幅を示します。
- [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius)は、の角の半径です。`Button`
- [`CharacterSpacing`](xref:Xamarin.Forms.Button.CharacterSpacing)テキストの文字間隔を示し `Button` ます。
- `TextTransform`テキストの文字種を決定し `Button` ます。

> [!NOTE]
> クラスには `Button` [`Margin`](xref:Xamarin.Forms.View.Margin) [`Padding`](xref:Xamarin.Forms.Button.Padding) 、のレイアウト動作を制御するプロパティとプロパティもあり `Button` ます。 詳細については「[Margin and Padding](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)」 (余白とスペース) を参照してください。

これらの6つのプロパティ (およびを除く) の効果は、 `FontFamily` `FontAttributes` ボタンの**外観**ページで説明されています。 もう1つのプロパティに [`Image`](xref:Xamarin.Forms.Button.ImageSource) ついては、「[**ボタンを使用したビットマップの使用**](#using-bitmaps-with-buttons)」セクションで説明します。

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

`Button`ページの上部にあるには、 `Color` `Picker` ページの下部にある要素にバインドされた3つのプロパティがあります。 要素の項目は、 `Picker` `NamedColor` プロジェクトに含まれているクラスの色です。 3つ `Slider` の要素に `FontSize` は、の、、およびの各プロパティへの双方向のバインディングが含まれて `BorderWidth` `CornerRadius` `Button` います。

このプログラムを使用すると、次のすべてのプロパティの組み合わせを試すことができます。

[![ボタンの外観](button-images/ButtonAppearance.png "ボタンの外観")](button-images/ButtonAppearance-Large.png)

境界線を表示するには、を `Button` `BorderColor` 以外の値に設定し `Default` 、を正の値に設定する必要があり `BorderWidth` ます。

IOS では、大きな罫線の幅がの内部に割り込ん、 `Button` テキストの表示を妨げていることがわかります。 IOS で境界線を使用することを選択した場合は `Button` 、 `Text` その可視性を維持するために、プロパティの先頭と末尾にスペースを使用することをお勧めします。

UWP で、の高さの半分を超えるを選択すると、 `CornerRadius` `Button` 例外が発生します。

## <a name="button-visual-states"></a>ボタンの表示状態

[`Button`](xref:Xamarin.Forms.Button)が有効になっている場合、ユーザーが押された `Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) ときにに対するビジュアルの変更を開始するために使用できる `Button` 。

次の XAML の例は、状態の表示状態を定義する方法を示してい `Pressed` ます。

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

が押されたときに、その `Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) [`Button`](xref:Xamarin.Forms.Button) [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) プロパティが既定値の1から0.8 に変更されることを指定します。 `Normal` `VisualState` が通常の状態の場合、 `Button` `Scale` プロパティは1に設定されることを指定します。 したがって、全体の効果として、 `Button` が押されている場合は、スケールが少し小さくなり、が解放されると、 `Button` 既定のサイズにスケールされます。

表示状態の詳細については、「 [ Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)」を参照してください。

## <a name="creating-a-toggle-button"></a>トグルボタンを作成する

サブクラスを使用する `Button` と、オンオフスイッチのように動作することができます。ボタンを1回タップすると、ボタンがオンに切り替わり、もう一度タップするとオフになります。

次のクラスはから派生し、という名前の `ToggleButton` `Button` 新しいイベント `Toggled` とという名前のブール型プロパティを定義し `IsToggled` ます。 によって定義される2つのプロパティは、次の Xamarin.Forms [`Switch`](xref:Xamarin.Forms.Switch) とおりです。

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

コンストラクターは、 `ToggleButton` イベントにハンドラーをアタッチして、 `Clicked` プロパティの値を変更できるようにし `IsToggled` ます。 メソッドは、 `OnIsToggledChanged` イベントを発生させ `Toggled` ます。

メソッドの最後の行は、 `OnIsToggledChanged` `VisualStateManager.GoToState` 2 つのテキスト文字列 "ToggledOn" と "ToggledOff" を使用して静的メソッドを呼び出します。 このメソッドについて、また、 [** Xamarin.Forms Visual State Manager の**](~/xamarin-forms/user-interface/visual-state-manager.md)記事で、アプリケーションが視覚的な状態に応答する方法について確認できます。

は `ToggleButton` を呼び出すため `VisualStateManager.GoToState` 、クラス自体には、その状態に基づいてボタンの外観を変更するための追加機能を含める必要はありません `IsToggled` 。 これは、をホストする XAML の役割です `ToggleButton` 。

**トグルボタンのデモ**ページには、表示 `ToggleButton` `Text` `BackgroundColor` `TextColor` 状態に基づいてボタンの、、およびを設定する visual state Manager マークアップを含む、の2つのインスタンスが含まれています。

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

`Toggled`イベントハンドラーは、分離コードファイルに含まれています。 これらは `FontAttributes` 、 `Label` ボタンの状態に基づいてのプロパティを設定する役割を担います。

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

IOS、Android、UWP で実行されているプログラムを次に示します。

[![トグルボタンのデモ](button-images/ToggleButtonDemo.png "トグルボタンのデモ")](button-images/ToggleButtonDemo-Large.png#lightbox)

## <a name="using-bitmaps-with-buttons"></a>ボタンにビットマップを使用する

クラスは、 `Button` [`ImageSource`](xref:Xamarin.Forms.Button.Image) のビットマップイメージを単独で、 `Button` またはテキストと組み合わせて表示できるプロパティを定義します。 また、テキストとイメージの配置方法を指定することもできます。

`ImageSource`プロパティの型はです [`ImageSource`](xref:Xamarin.Forms.ImageSource) 。これは、ファイル、埋め込みリソース、URI、またはストリームからビットマップを読み込むことができることを意味します。

> [!NOTE]
> は、 `Button` アニメーション gif を読み込むことができますが、gif の最初のフレームのみを表示します。

でサポートされている各プラットフォームでは、 Xamarin.Forms アプリケーションが実行される可能性のあるさまざまなデバイスのピクセル解像度に応じて、イメージを複数のサイズで格納できます。 これらの複数のビットマップには名前が付けられるか、またはオペレーティングシステムがデバイスのビデオディスプレイ解像度に最適な一致を選択できるように格納されます。

のビットマップの場合 `Button` 、通常、最適なサイズは、必要なサイズに応じて、32から64のデバイスに依存しない単位です。 この例で使用されるイメージは、デバイスに依存しない48のサイズに基づいています。

IOS プロジェクトの**Resources**フォルダーには、次の3つのイメージのサイズが含まれています。

- **/Resources/MonkeyFace.png**として格納された48ピクセルの四角形のビットマップ
- として格納された96ピクセルの四角形のビットマップ**/Resource/MonkeyFace@2x.png**
- として格納された144ピクセルの四角形のビットマップ**/Resource/MonkeyFace@3x.png**

3つのすべてのビットマップには、 **BundleResource**の**ビルドアクション**が割り当てられています。

Android プロジェクトの場合、すべてのビットマップに同じ名前が付いていますが、 **Resources**フォルダーの別のサブフォルダーに格納されています。

- **/Resourcesable/MonkeyFace.png**として格納された72ピクセルの四角形ビットマップ
- **/Resourcesablexhdpi/MonkeyFace.png**として格納された96ピクセルの四角形ビットマップ
- **/MonkeyFace.png**として格納された144ピクセルの四角形のビットマップ。
- / **Resourcesablez Hdpi/MonkeyFace.png**として格納された192ピクセルの四角形ビットマップ

これらには、 **Androidresource**の**ビルドアクション**が指定されました。

UWP プロジェクトでは、プロジェクト内の任意の場所にビットマップを保存できますが、通常はカスタムフォルダーまたは**アセット**の既存のフォルダーに格納されます。 UWP プロジェクトには、次のビットマップが含まれています。

- **/Assets/MonkeyFace.scale-100.png**として格納された48ピクセルの四角形のビットマップ
- **/Assets/MonkeyFace.scale-200.png**として格納された96ピクセルの四角形のビットマップ
- **/Assets/MonkeyFace.scale-400.png**として格納された192ピクセルの四角形のビットマップ

すべての**コンテンツ**の**ビルドアクション**が指定されました。

`Text` `ImageSource` `Button` のプロパティを使用して、およびプロパティをにどのように配置するかを指定でき [`ContentLayout`](xref:Xamarin.Forms.Button.ContentLayout) `Button` ます。 このプロパティは、の [`ButtonContentLayout`](xref:Xamarin.Forms.Button.ButtonContentLayout) 埋め込みクラスである型です `Button` 。 [コンストラクター] ( Xamarin.Forms xref:ボタン. ButtonContentLayout .% 23ctor ( Xamarin.Forms .ボタン. ButtonContentLayout. ImagePosition, system.string) には、次の2つの引数があります。

- 列挙体のメンバー [`ImagePosition`](xref:Xamarin.Forms.Button.ButtonContentLayout.ImagePosition) 。 `Left` 、、 `Top` `Right` 、または、ビットマップが `Bottom` テキストに対してどのように表示されるかを示します。
- `double`ビットマップとテキストの間隔を示す値。

既定値は、 `Left` 10 ユニットです。 という名前の2つの読み取り専用プロパティは、 `ButtonContentLayout` [`Position`](xref:Xamarin.Forms.Button.ButtonContentLayout.Position) [`Spacing`](xref:Xamarin.Forms.Button.ButtonContentLayout.Spacing) これらのプロパティの値を提供します。

コードでは、を作成し、次 `Button` のようにプロパティを設定でき `ContentLayout` ます。

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

XAML では、コンマで区切られた任意の順序で、列挙型メンバー、間隔、またはその両方を指定する必要があります。

```xaml
<Button Text="button text"
        ImageSource="image filename"
        ContentLayout="Right, 20" />
```

**イメージボタンのデモ**ページでは、を使用して、 `OnPlatform` iOS、Android、UWP ビットマップファイルに異なるファイル名を指定します。 プラットフォームごとに同じファイル名を使用し、を使用しないようにするには `OnPlatform` 、UWP ビットマップをプロジェクトのルートディレクトリに格納する必要があります。

イメージボタンのデモページの最初のページは、プロパティを `Button` 設定し**Image Button Demo** `Image` ますが、プロパティは設定しません `Text` 。

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

UWP ビットマップがプロジェクトのルートディレクトリに格納されている場合、このマークアップは大幅に簡素化できます。

```xaml
<Button ImageSource="MonkeyFace.png" />
```

**Imagebuttondemo .xaml**ファイルで多数の繰り返し実行するマークアップを回避するために、 `Style` プロパティを設定するための暗黙的なも定義されてい `ImageSource` ます。 これ `Style` は、他の5つの要素に自動的に適用され `Button` ます。 完全な XAML ファイルを次に示します。

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

最後の4つの要素は、プロパティを使用して、 `Button` `ContentLayout` テキストとビットマップの位置と間隔を指定します。

[![イメージボタンのデモ](button-images/ImageButtonDemo.png "イメージボタンのデモ")](button-images/ImageButtonDemo-Large.png#lightbox)

これで、 `Button` イベントを処理したり、外観を変更したりするためのさまざまな方法を確認できました `Button` 。

## <a name="related-links"></a>関連リンク

- [ButtonDemos のサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos)
- [ボタンの API](xref:Xamarin.Forms.Button)
