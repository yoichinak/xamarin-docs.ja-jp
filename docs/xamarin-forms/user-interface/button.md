---
title: Xamarin.Forms のボタン
description: ボタンは、タップまたは特定のタスクを実行するためにアプリケーションに指示するクリックに応答します。
ms.prod: xamarin
ms.assetid: 62CAEB63-0800-44F4-9B8C-EE632138C2F5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/19/2018
ms.openlocfilehash: fbdb611df558c547a2470a8c8a9d7848ef7aa31f
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2018
ms.locfileid: "52171392"
---
# <a name="xamarinforms-button"></a>Xamarin.Forms のボタン

_ボタンは、タップまたは特定のタスクを実行するためにアプリケーションに指示するクリックに応答します。_

[ `Button` ](xref:Xamarin.Forms.Button)はすべての Xamarin.Forms で最も基本的な対話型コントロールです。 `Button`ビットマップ イメージの場合、またはテキストの組み合わせとイメージが表示されますが、コマンドを示す短いテキスト文字列こともできますが通常表示します。 ユーザーが、`Button`を指で、またはそのコマンドを開始する、マウスでクリックします。

以下で説明するトピックのほとんどのページに対応して、 [ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos)サンプル。

## <a name="handling-button-clicks"></a>クリックしたボタンの処理

`Button` 定義、 [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) 、ユーザーがタップしたときに発生するイベント、`Button`指やマウス ポインターを使用します。 画面から指やマウス ボタンが離されたときに、イベントが発生した、`Button`します。 `Button`必要があります、 [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled)プロパティに設定`true`タップに応答します。

**基本的なボタンをクリックして** ページで、 [ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos)サンプル インスタンスを作成する方法を示します、 `Button` XAML およびハンドルでその`Clicked`イベント。 **BasicButtonClickPage.xaml**ファイルが含まれています、`StackLayout`と共に、`Label`と`Button`:

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

`Button`許可されているすべての領域を占有する傾向があります。 設定されていない場合など、`HorizontalOptions`プロパティの`Button`以外のものに`Fill`、`Button`はその親の幅全体を占有します。

既定では、`Button`は、四角形を使用して、it が丸められますの角を与えることができますが、 [ `CornerRadius` ](xref:Xamarin.Forms.Button.CornerRadius)プロパティに次のセクションで説明[**ボタンの外観**](#button-appearance).

[ `Text` ](xref:Xamarin.Forms.Button.Text)プロパティに表示されるテキストを指定します、`Button`します。 [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked)という名前のイベント ハンドラーにイベントが設定されて`OnButtonClicked`します。 このハンドラーは、分離コード ファイルにある**BasicButtonClickPage.xaml.cs**:

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

ときに、`Button`がタップされた、`OnButtonClicked`メソッドを実行します。 `sender`引数は、`Button`オブジェクトがこのイベントを担当します。 これを使用して、アクセスすることができます、`Button`オブジェクト、または複数を区別する`Button`オブジェクトが同じ共有`Clicked`イベント。

この特定の`Clicked`ハンドラーに回転するアニメーション関数を呼び出す、 `Label` 360 度 (1000 ミリ秒単位)。 Windows 10 デスクトップおよびユニバーサル Windows プラットフォーム (UWP) アプリケーションとして iOS および Android のデバイスでを実行しているプログラムを次に示します。

[![基本的なボタンのクリック](button-images/BasicButtonClick.png "基本的なボタンのクリック")](button-images/BasicButtonClick-Large.png#lightbox "基本的なボタンのクリック")

注意、`OnButtonClicked`メソッドが含まれています、`async`修飾子のため`await`イベント ハンドラー内で使用されます。 A`Clicked`イベント ハンドラーが必要です、`async`ハンドラーの本体で使用する場合にのみ、修飾子`await`します。

各プラットフォームのレンダリング、`Button`独自の特定の方法でします。 [**ボタンの外観**](#button-appearance)  セクションで、色を設定し、確認する方法について説明、`Button`境界線の外観をさらにカスタマイズを表示します。 `Button` 実装して、 [ `IFontElement` ](xref:Xamarin.Forms.Internals.IFontElement)インターフェイスを含むよう[ `FontFamily` ](xref:Xamarin.Forms.Button.FontFamily)、 [ `FontSize` ](xref:Xamarin.Forms.Button.FontSize)、および[ `FontAttributes` ](xref:Xamarin.Forms.Button.FontAttributes)プロパティ。

## <a name="creating-a-button-in-code"></a>コードでボタンを作成します。

インスタンスを作成するが一般的、 `Button` 、XAML で作成することもできますが、`Button`コードでします。 アプリケーションでの列挙型のデータに基づいて複数のボタンを作成する必要がある場合は、便利なこのする可能性があります、`foreach`ループします。

**コード ボタンのクリックして**ページは、機能的に等価であるページを作成する方法を示します、**基本的なボタンのクリックして**ページがまったくC#:

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

クラスのコンス トラクターでは、すべて行われます。 `Clicked`ハンドラーが長い 1 つだけのステートメントで、非常に単純に、イベントにアタッチできます。

```csharp
button.Clicked += async (sender, args) => await label.RelRotateTo(360, 1000);
```

もちろん、定義することも、イベント ハンドラーを別のメソッドとして (と同じように、`OnButtonClick`メソッド**基本的なボタンのクリックして**) とそのメソッドをイベントにアタッチします。

```csharp
button.Clicked += OnButtonClicked;
```

## <a name="disabling-the-button"></a>ボタンを無効にします。

特定の場所の特定の状態でアプリケーションが場合がありますが`Button`クリックは、有効な操作ではありません。 その場合、`Button`設定して無効にする必要があります、`IsEnabled`プロパティを`false`します。 典型的な例は、`Entry`ファイルを開く、ファイル名の制御`Button`:`Button`にいくつかのテキストが入力されている場合にのみ有効にする必要があります、`Entry`します。
使用することができます、`DataTrigger`ように、このタスク、 [**データ トリガー** ](~/xamarin-forms/app-fundamentals/triggers.md#data-triggers)記事。

## <a name="using-the-command-interface"></a>コマンド インターフェイスを使用します。

アプリケーションに応答することは`Button`タップ処理せず、`Clicked`イベント。 `Button`と呼ばれる別の通知メカニズムを実装して、_コマンド_または_コマンド実行_インターフェイス。 これは、2 つのプロパティで構成されます。

- [`Command`](xref:Xamarin.Forms.Button.Command) 型の[ `ICommand` ](xref:System.Windows.Input.ICommand)、インターフェイスで定義されている、 [ `System.Windows.Input` ](xref:System.Windows.Input)名前空間。
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) 型のプロパティ[ `Object`](xref:System.Object)します。

このアプローチは、モデル-ビュー-ビューモデル (MVVM) アーキテクチャを実装する場合に特に関連データ バインディング、およびに特に適しています。 これらのトピックが、記事で説明した[データ バインディングの](~/xamarin-forms/app-fundamentals/data-binding/index.md)、[データ バインディングから mvvm まで](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)、および[MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md)します。

型のプロパティを定義するビューモデル MVVM アプリケーションで`ICommand`し、XAML に接続されている`Button`データ バインドを持つ要素。 Xamarin.Forms も定義[ `Command` ]((xref:Xamarin.Forms.Command`1))と[ `Command<T>` ](xref:Xamarin.Forms.Command`1)実装するクラス、`ICommand`インターフェイスし、型のプロパティを定義する際に、ViewModelを支援する`ICommand`.

情報の記事で詳しく説明は、コマンドを実行[**のコマンド インターフェイス**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md)が、**基本的なボタン コマンド**ページで、 [ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos)サンプルは、基本的なアプローチを示しています。

`CommandDemoViewModel`クラスは、型のプロパティを定義する非常に単純な ViewModel`double`という名前の`Number`、型の 2 つのプロパティと`ICommand`という名前の`MultiplyBy2Command`と`DivideBy2Command`:

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

2 つ`ICommand`プロパティは型の 2 つのオブジェクト クラスのコンス トラクターで初期化される`Command`します。 `Command`コンス トラクターは、少し関数を含める (と呼ばれる、`execute`コンス トラクターの引数) を 2 倍にするとき、または部分が、`Number`プロパティ。

**BasicButtonCommand.xaml**ファイルのセット、`BindingContext`のインスタンスに`CommandDemoViewModel`します。 `Label`要素と 2 つ`Button`要素は、3 つのプロパティへのバインドを保持`CommandDemoViewModel`:

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

2 つとして`Button`要素がタップされた、コマンドの実行、および数の値が変更します。

[![基本的なボタン コマンド](button-images/BasicButtonCommand.png "基本的なボタン コマンド")](button-images/BasicButtonCommand-Large.png#lightbox)

経由でこのアプローチの利点`Clicked`ハンドラーは、このページの機能に関連するすべてのロジックがある、分離コード ファイルではなく、ViewModel でビジネス ロジックからユーザー インターフェイスの優れた分離を実現します。

ことも、`Command`を有効にしての無効化を制御するオブジェクト、`Button`要素。 たとえば、2 の間の数値の値の範囲を制限する<sup>10</sup>と 2 つ<sup>&ndash;10</sup>します。 コンス トラクターに別の関数を追加することができます (と呼ばれる、`canExecute`引数) を返す`true`場合、`Button`有効にする必要があります。 変更をここでは、`CommandDemoViewModel`コンス トラクター。

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

呼び出し、`ChangeCanExecute`メソッドの`Command`に必要なように、`Command`メソッドを呼び出すことができます、`canExecute`メソッド決定かどうか、`Button`いない、または無効にする必要があります。 このコード変更の数と、制限に達する、`Button`は無効です。

[![基本的なボタン コマンド - 変更](button-images/BasicButtonCommandModified.png "変更 - [基本] ボタンのコマンド")](button-images/BasicButtonCommandModified-Large.png#lightbox)

2 つ以上のことは`Button`要素をバインドする同じ`ICommand`プロパティ。 `Button`を使用して要素を区別する、 [ `CommandParameter` ](xref:Xamarin.Forms.Button.CommandParameter)プロパティの`Button`します。 ジェネリックを使用したいここでは、 [ `Command<T>` ](xref:Xamarin.Forms.Command`1)クラス。 `CommandParameter`オブジェクトが引数として渡されるし、`execute`と`canExecute`メソッド。 この手法で詳細に表示されます、 [**コマンド実行の基本的な**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding)のセクション、 [**コマンド インターフェイス**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding)記事。

[ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos)サンプルでは、この手法で使用もその`MainPage`クラス。 **MainPage.xaml**ファイルが含まれています、`Button`サンプルの各ページ。

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

各`Button`がその`Command`プロパティという名前のプロパティにバインドされて`NavigateCommand`、および`CommandParameter`に設定されている、 [ `Type` ](xref:System.Type)プロジェクト内のページ クラスのいずれかに対応するオブジェクト。

`NavigateCommand`プロパティの型は`ICommand`分離コード ファイルで定義されます。

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

コンス トラクターによって初期化、`NavigateCommand`プロパティを`Command<Type>`オブジェクト`Type`の種類、 `CommandParameter` XAML ファイル内のオブジェクト セットします。 つまり、`execute`メソッドが型の引数`Type`これに対応する`CommandParameter`オブジェクト。 関数では、ページをインスタンス化し、し、それに移動します。

通知を設定して、コンス トラクターが最後にあるその`BindingContext`自体にします。 これは、プロパティにバインドする XAML ファイルに必要な`NavigateCommand`プロパティ。

## <a name="pressing-and-releasing-the-button"></a>キーを押すと、ボタンを放す

それに、`Clicked`イベント、`Button`も定義[ `Pressed` ](xref:Xamarin.Forms.Button.Pressed)と[ `Released` ](xref:Xamarin.Forms.Button.Released)イベント。 `Pressed`の指を押したときに発生するイベントを`Button`、またはポインターの上に置かれたマウス ボタンが押された、`Button`します。 `Released`イベントは、指やマウス ボタンが離されたときに発生します。 一般に、`Clicked`発生と同時にも、`Released`の画面から離れた場所スライド指やマウスのポインターの場合は、イベント、`Button`リリースされる前に、`Clicked`イベントが発生しない可能性が。

`Pressed`と`Released`イベントは、多くの場合、使用されないがで示した、特殊な用途で使用できます、**リリース ボタン**ページ。 XAML ファイルが含まれています、`Label`と`Button`、アタッチされたハンドラーで、`Pressed`と`Released`イベント。

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

分離コード ファイルをアニメーション化、`Label`ときに、`Pressed`イベントが発生したが、回転を中断します。 ときに、`Released`イベントが発生します。

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

その結果、`Label`だけ回転指が接触したが、 `Button`、し、指を離したときに停止します。

[![キーを押すし、ボタンを離します](button-images/PressAndReleaseButton.png "キーを押すし、ボタンのリリース")](button-images/PressAndReleaseButton-Large.png)

このような動作がゲーム用のアプリケーション: 指で保持されている、 `Button` on 画面オブジェクトを特定の方向に移動を行う場合があります。

<a name="button-appearance" />

## <a name="button-appearance"></a>ボタンの外観

`Button`継承されているかの外観に影響を与えるいくつかのプロパティを定義します。

- [`TextColor`](xref:Xamarin.Forms.Button.TextColor) 色を表す、`Button`テキスト
- [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) そのテキストの背景の色は、します。
- [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) 周り領域の色が、 `Button`
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily) テキストのフォント ファミリが使用します。
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize) テキストのサイズは、します。
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes) テキストが斜体または太字のかどうかを示します
- [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) 罫線の幅は、します。
- [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius) 角の半径は、 `Button`

> [!NOTE]
> `Button`クラスもあります[ `Margin` ](xref:Xamarin.Forms.View.Margin)と[ `Padding` ](xref:Xamarin.Forms.Button.Padding)のレイアウト動作を制御するプロパティ、`Button`します。 詳細については、次を参照してください。[余白やパディング](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)します。

これらのプロパティの 6 つの効果 (を除く`FontFamily`と`FontAttributes`) で説明されています、**ボタンの外観**ページ。 別のプロパティ、 [ `Image`](xref:Xamarin.Forms.Button.Image)は、セクションで説明[**ボタンにビットマップを使用して**](#image-button)します。

すべてのビューとデータ バインドで、**ボタンの外観**ページは、XAML ファイルで定義されています。

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

`Button` 、ページの上部にある 3 つを持つ`Color`プロパティにバインド`Picker`ページの下部にある要素。 内の項目、`Picker`要素は、色、`NamedColor`クラスは、プロジェクトに追加します。 次の 3 つ`Slider`双方向のバインドの要素を格納、 `FontSize`、 `BorderWidth`、および`CornerRadius`のプロパティ、`Button`します。

このプログラムではこれらすべてのプロパティの組み合わせで実験を行うことができます。

[![ボタンの外観](button-images/ButtonAppearance.png "外観のボタン")](button-images/ButtonAppearance-Large.png)

参照してください、`Button`罫線を設定する必要があります、`BorderColor`以外のものに`Default`、および`BorderWidth`を正の値。

Ios では、大規模な境界線の幅がの内部に重なることを確認します、`Button`と干渉するテキストを表示します。 IOS の枠線を使用するように選択したかどうかは`Button`を開始および終了する便利、`Text`プロパティにスペースをその可視性を保持します。

UWP でを選択すると、`CornerRadius`の高さの半分を超えている、`Button`例外を発生させます。

## <a name="button-visual-states"></a>ボタンのビジュアル状態

[`Button`](xref:Xamarin.Forms.Button) `Pressed` [ `VisualState` ](xref:Xamarin.Forms.VisualState)を視覚的な変更の開始に使用できる、`Button`が有効になっている、ユーザーによって押されたときにします。

次の XAML の例のビジュアル状態を定義する方法を示しています、`Pressed`状態。

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
</ImageButton>
```

`Pressed` [ `VisualState` ](xref:Xamarin.Forms.VisualState)される場合、 [ `Button` ](xref:Xamarin.Forms.Button)を押すと、その[ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale)からプロパティを変更する、1 に 0.8 の既定値。 `Normal` `VisualState`される場合、`Button`通常の状態では、その`Scale`プロパティを 1 に設定されます。 そのため、全体の効果では、ときに、`Button`を押すと、これは再スケーリング、若干小さいとタイミングを`Button`がリリースされると、これは再スケーリングの既定のサイズにします。

表示状態の詳細については、次を参照してください。 [、Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)します。

## <a name="creating-a-toggle-button"></a>トグル ボタンを作成します。

サブクラス化することは`Button`オン/オフ スイッチのように動作するよう: 1 回のボタンをタップすると、ボタンをオンし、オフを切り替えるためには、もう一度タップします。

次`ToggleButton`クラスから派生`Button`という名前の新しいイベントを定義および`Toggled`というブール型プロパティと`IsToggled`します。 これらは、Xamarin.Forms で定義された 2 つの同じプロパティ[ `Switch` ](xref:Xamarin.Forms.Switch):

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

`ToggleButton`コンス トラクターにハンドラーをアタッチする、`Clicked`イベントの値を変更できるように、`IsToggled`プロパティ。 `OnIsToggledChanged`メソッドの起動、`Toggled`イベント。

最後の行、`OnIsToggledChanged`メソッドは、静的な`VisualStateManager.GoToState`メソッドを 2 つのテキスト文字列"ToggledOn"と"ToggledOff"。 読み取ることができますこのメソッドと、アプリケーションが、記事の表示状態に応答方法に関する[ **、Xamarin.Forms Visual State Manager**](~/xamarin-forms/user-interface/visual-state-manager.md)します。

`ToggleButton`呼び出しを`VisualStateManager.GoToState`、クラス自体が追加の機能に基づいて、ボタンの外観を変更する必要がある、`IsToggled`状態。 ホストする XAML の責任は、`ToggleButton`します。

**トグル ボタンのデモ**ページには、2 つのインスタンスが含まれています。 `ToggleButton`、Visual State Manager のマークアップを設定するなど、 `Text`、 `BackgroundColor`、および`TextColor`のビジュアルの状態に基づいてボタン。

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

`Toggled`分離コード ファイルには、イベント ハンドラー。 設定を担当している、`FontAttributes`のプロパティ、`Label`ボタンの状態に基づいて。

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

[![ボタンのデモを切り替える](button-images/ToggleButtonDemo.png "ボタンのデモを切り替える")](button-images/ToggleButtonDemo-Large.png#lightbox)

<a name="image-button" />

## <a name="using-bitmaps-with-buttons"></a>ボタンにビットマップを使用します。

`Button`クラスを定義、 [ `Image` ](xref:Xamarin.Forms.Button.Image)プロパティにビットマップ イメージを表示することができる`Button`、単独または組み合わせてテキスト。 テキストとイメージの配置方法を指定することもできます。

`Image`プロパティの型は[ `FileImageSource` ](xref:Xamarin.Forms.FileImageSource)、つまり、や .NET Standard ライブラリ プロジェクトではなく、個々 のプラットフォームのプロジェクト内のリソースとして、ビットマップを格納する必要があります。

Xamarin.Forms でサポートされている各プラットフォームで、アプリケーションが実行するさまざまなデバイスのさまざまなピクセルの解像度の複数のサイズに格納されるイメージできます。 これら複数のビットマップをという名前またはデバイスのビデオに、オペレーティング システムが最適な一致を選択できますように格納されている画面の解像度。

ビットマップの`Button`、最適なサイズは 32 ビットおよび 64 のデバイスに依存しない単位間では、通常は、目的に規模によって。 この例で使用されるイメージは、48 のデバイスに依存しない単位のサイズに基づいています。

IOS プロジェクトで、**リソース**フォルダーには、このイメージの 3 つのサイズが含まれています。

- として格納されている、48 ピクセルの正方形ビットマップ **/Resources/MonkeyFace.png**
- として格納されている、96 ピクセルの正方形ビットマップ **/Resource/MonkeyFace@2x.png**
- として格納されている、144 ピクセルの正方形ビットマップ **/Resource/MonkeyFace@3x.png**

すべての 3 つのビットマップに付与された、**ビルド アクション**の**BundleResource**します。

Android のプロジェクトのすべてのビットマップが同じ名前を持つのさまざまなサブフォルダーに保存されている、**リソース**フォルダー。

- として格納されている、72 ピクセルの正方形ビットマップ **/Resources/drawable-hdpi/MonkeyFace.png**
- として格納されている、96 ピクセルの正方形ビットマップ **/Resources/drawable-xhdpi/MonkeyFace.png**
- として格納されている、144 ピクセルの正方形ビットマップ **/Resources/drawable-xxhdpi/MonkeyFace.png**
- として格納されている、192 ピクセルの正方形ビットマップ **/Resources/drawable-xxxhdpi/MonkeyFace.png**

これらに付与された、**ビルド アクション**の**AndroidResource**します。

UWP プロジェクトでのビットマップに格納できる任意の場所、プロジェクトが、カスタム フォルダーで一般的に格納されている、または**資産**既存のフォルダー。 UWP プロジェクトには、これらのビットマップが含まれています。

- として格納されている、48 ピクセルの正方形ビットマップ **/Assets/MonkeyFace.scale-100.png**
- として格納されている、96 ピクセルの正方形ビットマップ **/Assets/MonkeyFace.scale-200.png**
- として格納されている、192 ピクセルの正方形ビットマップ **/Assets/MonkeyFace.scale-400.png**

これらがすべて与え、**ビルド アクション**の**コンテンツ**します。

指定できますが、どのように`Text`と`Image`プロパティ上に配置されます、`Button`を使用して、 [ `ContentLayout` ](xref:Xamarin.Forms.Button.ContentLayout)のプロパティ`Button`。 このプロパティの型は[ `ButtonContentLayout` ](xref:Xamarin.Forms.Button.ButtonContentLayout)、これは埋め込みクラスに`Button`します。 [コンス トラクター](xref:Xamarin.Forms.Button.ButtonContentLayout.%23ctor(Xamarin.Forms.Button.ButtonContentLayout.ImagePosition,System.Double))に 2 つの引数があります。

- メンバー、 [ `ImagePosition` ](xref:Xamarin.Forms.Button.ButtonContentLayout.ImagePosition)列挙: `Left`、 `Top`、 `Right`、または`Bottom`テキストの基準としたビットマップがどのように表示されるかを示します。
- A`double`ビットマップとテキスト間の間隔の値。

既定値は`Left`と 10 個のユニットです。 2 つの読み取り専用プロパティ`ButtonContentLayout`という[ `Position` ](xref:Xamarin.Forms.Button.ButtonContentLayout.Position)と[ `Spacing` ](xref:Xamarin.Forms.Button.ButtonContentLayout.Spacing)これらのプロパティの値を指定します。

コードで作成、`Button`設定と、`ContentLayout`このようなプロパティ。

```csharp
Button button = new Button
{
    Text = "button text",
    Image = new FileImageSource
    {
        File = "image filename"
    },
    ContentLayout = new Button.ButtonContentLayout(Button.ButtonContentLayout.ImagePosition.Right, 20)
};
```

XAML では、列挙型のメンバーまたは間隔を指定する必要があります。 またはコンマで区切られた任意の順序で両方。

```xaml
<Button Text="button text"
        Image="image filename"
        ContentLayout="Right, 20" />
```

**イメージ ボタン デモ**ページ使用`OnPlatform`を iOS、Android、および UWP のビットマップ ファイルの異なるファイル名を指定します。 プラットフォームごとに同じファイル名を使用して、使用しないようにしたい場合`OnPlatform`プロジェクトのルート ディレクトリに UWP ビットマップを格納する必要があります。

最初の`Button`上、**イメージ ボタン デモ**ページ セット、`Image`プロパティが、`Text`プロパティ。

```xaml
<Button>
    <Button.Image>
        <OnPlatform x:TypeArguments="FileImageSource">
            <On Platform="iOS, Android" Value="MonkeyFace.png" />
            <On Platform="UWP" Value="Assets/MonkeyFace.png" />
        </OnPlatform>
    </Button.Image>
</Button>
```

UWP のビットマップが、プロジェクトのルート ディレクトリに格納されている場合は、このマークアップをかなり簡略します。

```xaml
<Button Image="MonkeyFace.png" />
```

多くのマークアップを繰り返し実行するを回避するために、 **ImageButtonDemo.xaml**ファイル、暗黙的な`Style`設定にも定義されています、`Image`プロパティ。 これは、`Style`を他の 5 つが自動的に適用`Button`要素。 完全な XAML ファイルを次に示します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.ImageButtonDemoPage">

    <FlexLayout Direction="Column"
                JustifyContent="SpaceEvenly"
                AlignItems="Center">

        <FlexLayout.Resources>
            <Style TargetType="Button">
                <Setter Property="Image">
                    <OnPlatform x:TypeArguments="FileImageSource">
                        <On Platform="iOS, Android" Value="MonkeyFace.png" />
                        <On Platform="UWP" Value="Assets/MonkeyFace.png" />
                    </OnPlatform>
                </Setter>
            </Style>
        </FlexLayout.Resources>

        <Button>
            <Button.Image>
                <OnPlatform x:TypeArguments="FileImageSource">
                    <On Platform="iOS, Android" Value="MonkeyFace.png" />
                    <On Platform="UWP" Value="Assets/MonkeyFace.png" />
                </OnPlatform>
            </Button.Image>
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

最後の 4 つ`Button`要素の使用、`ContentLayout`位置とテキストおよびビットマップの間隔を指定するプロパティ。

[![ボタンのデモをイメージ](button-images/ImageButtonDemo.png "イメージ ボタンのデモ")](button-images/ImageButtonDemo-Large.png#lightbox)

処理できるさまざまな方法が表示されました`Button`イベントと変更、`Button`外観。

## <a name="related-links"></a>関連リンク

- [ButtonDemos サンプル](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos)
- [API ボタン](xref:Xamarin.Forms.Button)
