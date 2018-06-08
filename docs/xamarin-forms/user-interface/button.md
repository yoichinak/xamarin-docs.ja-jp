---
title: Xamarin.Forms ボタン
description: ボタンは、タップまたは特定のタスクを実行するアプリケーションに指示するクリックに応答します。
ms.prod: xamarin
ms.assetid: 62CAEB63-0800-44F4-9B8C-EE632138C2F5
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/01/2018
ms.openlocfilehash: 095736e77b2f502261f9b85ab73c45dce74309b9
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734151"
---
# <a name="xamarinforms-button"></a>Xamarin.Forms ボタン

_ボタンは、タップまたは特定のタスクを実行するアプリケーションに指示するクリックに応答します。_ 

[ `Button` ](xref:Xamarin.Forms.Button)すべて Xamarin.Forms の最も基本的な対話型コントロールです。 `Button`通常が表示されますが、コマンドを示す短いテキスト文字列のことも、ビットマップ イメージまたはテキストの組み合わせとイメージを表示します。 ユーザーが、`Button`指で、またはそのコマンドを開始する、マウスでクリックします。

以下で説明するトピックの大部分のページに対応、 [ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos)サンプルです。

## <a name="handling-button-clicks"></a>処理ボタンをクリックします。

`Button` 定義、 [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked)ユーザーがタップしたときに発生するイベント、`Button`指やマウスのポインターを使用します。 任意の場所から本の指またはマウス ボタンが離されると、イベントが発生した、`Button`です。 `Button`必要があります、 [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled)プロパティに設定`true`タップに対応することにします。 

**基本的なボタンをクリックして**] ページで、 [ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos)サンプル インスタンスを作成する方法を示します、 `Button` XAML およびハンドルでその`Clicked`イベント。 **BasicButtonClickPage.xaml**ファイルが含まれています、`StackLayout`両方を持つ、`Label`と`Button`:

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

`Button`許可されているすべての領域を占有する傾向があります。 設定されていない場合など、`HorizontalOptions`プロパティの`Button`以外の何かに`Fill`、`Button`はその親の幅全体を占有します。

既定では、`Button`が長方形を使用して、it が丸められますの角を与えることができますが、 [ `CornerRadius` ](xref:Xamarin.Forms.Button.CornerRadius)プロパティのセクションで説明するよう[**外観のボタンをクリックして**](#button-appearance).

[ `Text` ](xref:Xamarin.Forms.Button.Text)プロパティに表示されるテキストを指定する、`Button`です。 [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked)名前付きイベント ハンドラーにイベントが設定されている`OnButtonClicked`です。 このハンドラーは、分離コード ファイルにある**BasicButtonClickPage.xaml.cs**:

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

ときに、`Button`タップは、`OnButtonClicked`メソッドが実行します。 `sender`引数は、`Button`オブジェクトがこのイベントを担当します。 これを使用して、アクセスすることができます、`Button`オブジェクト、または複数を区別する`Button`オブジェクトが同じ共有`Clicked`イベント。

この特定の`Clicked`ハンドラーに回転するアニメーション関数を呼び出す、 `Label` 360 度 (1000 ミリ秒単位)。 IOS および Android デバイス、およびユニバーサル Windows プラットフォーム (UWP) アプリケーションとしてを Windows 10 デスクトップで実行されているプログラムを次に示します。

[![[基本] ボタンをクリックして](button-images/BasicButtonClick.png "基本的なボタンのクリック")](button-images/BasicButtonClick-Large.png#lightbox "基本的なボタンをクリック")

注意して、`OnButtonClicked`メソッドが含まれています、`async`修飾子のため`await`は、イベント ハンドラー内で使用します。 A`Clicked`イベント ハンドラーが必要です、`async`修飾子ハンドラーの本体で使用する場合にのみ`await`です。

各プラットフォームの表示、`Button`独自の特定の方法でします。 [**ボタンの外観**](#button-appearance)  セクションで、色を設定し、確認する方法が表示されます、`Button`境界線のより細かくカスタマイズされた外観を表示します。 `Button` 実装する、 [ `IFontElement` ](xref:Xamarin.Forms.Internals.IFontElement)が含まれているように、インターフェイス[ `FontFamily` ](xref:Xamarin.Forms.Button.FontFamily)、 [ `FontSize` ](xref:Xamarin.Forms.Button.FontSize)、および[ `FontAttributes` ](xref:Xamarin.Forms.Button.FontAttributes)プロパティです。

## <a name="creating-a-button-in-code"></a>コードでボタンを作成します。

インスタンスを作成するが一般的、 `Button` XAML では、作成することもできますが、`Button`コードでします。 これは、便利なときの列挙可能であるデータに基づく複数のボタンを作成する必要がある、アプリケーション、`foreach`ループします。

**コード ボタンのクリックして**ページは、機能的に等価であるページを作成する方法を示します、**基本的なボタンをクリックして**ページが完全に C# の場合。

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

すべてのものは、クラスのコンス トラクターで行われます。 `Clicked`ハンドラー長いのみ 1 つのステートメントは、イベントに非常に簡単に、アタッチすることができます。

```csharp
button.Clicked += async (sender, args) => await label.RelRotateTo(360, 1000);
```

もちろん、定義することも、イベント ハンドラーを別のメソッドとして (と同じように、`OnButtonClick`メソッドで**基本的なボタンをクリックして**) とそのメソッドをイベントにアタッチします。

```csharp
button.Clicked += OnButtonClicked;
```

## <a name="disabling-the-button"></a>ボタンを無効にします。

アプリケーションは特定の状態で、特定`Button`クリックが有効な操作ではありません。 ような場合、`Button`設定によって無効にする必要があります、`IsEnabled`プロパティを`false`です。 典型的な例は、`Entry`ファイルを開く、ファイル名の制御`Button`:`Button`にいくつかのテキストが型指定されている場合にのみ有効にする必要があります、`Entry`です。 使用することができます、`DataTrigger`で示すように、このタスクの[**データ トリガー** ](~/xamarin-forms/app-fundamentals/triggers.md#data-triggers)資料です。

## <a name="using-the-command-interface"></a>コマンド インターフェイスを使用します。

アプリケーションに応答することは`Button`タップ処理せず、`Clicked`イベント。 `Button`と呼ばれる代替の通知メカニズムを実装して、_コマンド_または_コマンド実行_インターフェイスです。 これは、2 つのプロパティで構成されます。

- [`Command`](xref:Xamarin.Forms.Button.Command) 型の[ `ICommand`](xref:System.Windows.Input.ICommand)で定義されているインターフェイス、 [ `System.Windows.Input` ](xref:System.Windows.Input)名前空間。
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) 型のプロパティ[ `Object`](xref:System.Object)です。

この方法は、モデル View-viewmodel (MVVM) アーキテクチャを実装する場合に特に関連のデータ バインディング、およびに特に適しています。 これらのトピックは、記事で説明[データ バインディングの](~/xamarin-forms/app-fundamentals/data-binding/index.md)、 [MVVM へのデータのバインドから](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)、および[MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md)です。

型のプロパティを定義する、ViewModel MVVM アプリケーションで`ICommand`XAML にし、接続されている`Button`データ バインディングを持つ要素。 Xamarin.Forms も定義[ `Command` ]((xref:Xamarin.Forms.Command`1))と[ `Command<T>` ](xref:Xamarin.Forms.Command`1)を実装するクラス、`ICommand`インターフェイスし、の種類のプロパティを定義する、ViewModel手助け`ICommand`.

アーティクルでさらに詳しく記載されてコマンド実行[**のコマンド インターフェイス**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md)ですが、**基本的なボタン コマンド** ページで、 [ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos)サンプルは、基本的な方法を示しています。

`CommandDemoViewModel`クラスは、型のプロパティを定義する非常に単純な ViewModel`double`という`Number`、型の 2 つのプロパティと`ICommand`という名前`MultiplyBy2Command`と`DivideBy2Command`:

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

2 つ`ICommand`プロパティが型の 2 つのオブジェクトを持つクラスのコンス トラクターで初期化される`Command`です。 `Command`コンス トラクターは、小さい関数を含める (と呼ばれる、`execute`コンス トラクターの引数) を 2 倍にするとき、または部分、`Number`プロパティです。

**BasicButtonCommand.xaml**ファイルのセットをその`BindingContext`のインスタンスに`CommandDemoViewModel`です。 `Label`要素と 2 つ`Button`要素は、次の 3 つのプロパティへのバインドを保持`CommandDemoViewModel`:

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

2 つとして`Button`要素でタップは、コマンドの実行、および数の値を変更します。

[![基本的なボタン コマンド](button-images/BasicButtonCommand.png "基本的なボタン コマンド")](button-images/BasicButtonCommand-Large.png#lightbox)

経由でこの方法の利点`Clicked`ハンドラーは、このページの機能に関連するすべてのロジックがあること、分離コード ファイルではなく、ViewModel で分離を強化し、ユーザー インターフェイスのビジネス ロジックを実現します。

ことも、`Command`の有効化との無効化を制御するオブジェクト、`Button`要素。 たとえば、2 の間の数値の値の範囲を制限する<sup>10</sup>および 2<sup>&ndash;10</sup>です。 コンス トラクターに別の関数を追加することができます (と呼ばれる、`canExecute`引数) を返す`true`場合、`Button`有効にする必要があります。 ここでは、変更が、`CommandDemoViewModel`コンス トラクター。

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

呼び出し、`ChangeCanExecute`メソッドの`Command`ために必要なできるように、`Command`メソッドを呼び出すことができます、`canExecute`メソッドとを特定するかどうか、`Button`無効かどうか。 このコードの変更の数と制限に達する、`Button`は無効になります。

[![変更の基本的なボタン コマンド](button-images/BasicButtonCommandModified.png "基本的なボタン コマンドの変更")](button-images/BasicButtonCommandModified-Large.png#lightbox)

2 つ以上のことが`Button`要素に同じバインドを`ICommand`プロパティです。 `Button`を使用して要素を識別できますが、 [ `CommandParameter` ](xref:Xamarin.Forms.Button.CommandParameter)プロパティ`Button`です。 この例を使用するジェネリック[ `Command<T>` ](xref:Xamarin.Forms.Command`1)クラスです。 `CommandParameter`オブジェクトが引数として渡され、`execute`と`canExecute`メソッドです。 この手法がで詳細に示すように、 [**基本的なコマンドの実行**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding)のセクション、 [**コマンド インターフェイス**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding)資料です。

[ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos)サンプルでは、この手法でその`MainPage`クラスです。 **MainPage.xaml**ファイルが含まれています、`Button`サンプルの各ページ。

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

各`Button`がその`Command`プロパティという名前のプロパティにバインドされる`NavigateCommand`、および`CommandParameter`に設定されている、 [ `Type` ](xref:System.Type)プロジェクト内のページ クラスのいずれかに対応するオブジェクト。

`NavigateCommand`プロパティの型は`ICommand`分離コード ファイルで定義されたとします。

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

コンス トラクターは、`NavigateCommand`プロパティを`Command<Type>`オブジェクト`Type`の種類、 `CommandParameter` XAML ファイル内のオブジェクト セットです。 つまり、`execute`メソッドが型の引数を持つ`Type`これに対応する`CommandParameter`オブジェクト。 関数は、ページをインスタンス化し、それに移動し。

コンス トラクターが終了して設定をその`BindingContext`をそれ自体にします。 これは、プロパティにバインドする XAML ファイルに必要な`NavigateCommand`プロパティです。

## <a name="pressing-and-releasing-the-button"></a>キーを押してボタンを離すと、

さらに、`Clicked`イベント、`Button`も定義[ `Pressed` ](xref:Xamarin.Forms.Button.Pressed)と[ `Released` ](xref:Xamarin.Forms.Button.Released)イベント。 `Pressed`イベント上に指を押したときに発生、 `Button`、ポインターの上に置くと、マウス ボタンが押されたか、`Button`です。 `Released`イベント本の指またはマウス ボタンが離されたときに発生します。 一般に、`Clicked`発生と同時にも、`Released`の任意の場所から離れた場所スライド指やマウスのポインターの場合は、イベント、`Button`リリースされる前に、`Clicked`イベントが発生するされません。

`Pressed`と`Released`イベントが多くの場合、使用しないで示したように特別な目的で使用できます、**リリース ボタン**ページ。 XAML ファイルに含まれる、`Label`と`Button`ハンドラーのアタッチを使用して、`Pressed`と`Released`イベント。

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

分離コード ファイルをアニメーション化、`Label`ときに、`Pressed`イベントが発生しても、回転を中断時に、`Released`イベントが発生します。

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

結果を`Label`連絡がある、指だけが、`Button`指がリリースされたときに停止するとします。

[![キーを押すし、ボタンを離します](button-images/PressAndReleaseButton.png "キーを押すし、ボタンを放す")](button-images/PressAndReleaseButton-Large.png)

このような動作がゲーム用のアプリケーション: 指で保持されている、 `Button` on 画面オブジェクトを特定の方向に移動を行う可能性があります。 

<a name="button-appearance" />

## <a name="button-appearance"></a>ボタンの外観

`Button`継承しているか、その外観に影響するいくつかのプロパティを定義します。

- [`TextColor`](xref:Xamarin.Forms.Button.TextColor) 色である、`Button`テキスト
- [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) そのテキストに背景色
- [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) エリアの周囲の色である、 `Button`
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily) テキストのフォント ファミリが使用します。
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize) テキストのサイズは、します。
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes) テキストが斜体または太字かどうかを示します
- [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) 罫線の幅には 
- [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius) 角を丸める

これらのプロパティの 6 文字の効果 (を除く`FontFamily`と`FontAttributes`) で説明されて、**ボタンの外観**ページ。 別のプロパティ、 [ `Image` ](xref:Xamarin.Forms.Button.Image)、セクションで説明した[**ボタンにビットマップを使用して**](#image-button)です。

すべてのビューとデータ バインディングの**ボタンの外観**ページは、XAML ファイルで定義されます。

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

`Button`ページの上部にあるが、3 つ`Color`プロパティにバインド`Picker`ページの下部にある要素。 内の項目、`Picker`要素は、色、`NamedColor`プロジェクトに含まれるクラスです。 次の 3 つ`Slider`双方向のバインドの要素を格納、 `FontSize`、 `BorderWidth`、および`CornerRadius`のプロパティ、`Button`です。

このプログラムでは、これらすべてのプロパティの組み合わせをテストすることができます。

[![ボタンの外観](button-images/ButtonAppearance.png "ボタンの外観")](button-images/ButtonAppearance-Large.png)

表示する、`Button`枠線を設定する必要があります、`BorderColor`以外の何かに`Default`、および`BorderWidth`正の値にします。

Ios の場合かがわかりますの内部に大規模な境界線の幅が重なること、`Button`テキストの表示に干渉することです。 IOS の枠線を使用するかどうかを選択する`Button`、開始および終了にしておく可能性があります、`Text`可視性を保持するスペースを持つプロパティです。

UWP でを選択すると、`CornerRadius`の半分の高さを超えている、`Button`例外が発生します。

## <a name="creating-a-toggle-button"></a>トグル ボタンを作成します。

サブクラス化可能であれば`Button`のオン/オフ スイッチと同様に動作するよう: ボタンで切り替えるし、オフに再度タップするボタンを 1 回 をタップします。

次`ToggleButton`クラスから派生`Button`という名前の新しいイベントを定義および`Toggled`という名前のブール型プロパティと`IsToggled`です。 これらは、同じ 2 つのプロパティ、Xamarin.Forms で定義された[ `Switch` ](xref:Xamarin.Forms.Switch):

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

`ToggleButton`コンス トラクターにハンドラーをアタッチする、`Clicked`イベントがの値を変更できるように、`IsToggled`プロパティです。 `OnIsToggledChanged`メソッドが起動、`Toggled`イベント。 

最後の行、`OnIsToggledChanged`メソッドを呼び出す、静的な`VisualStateManager.GoToState`"ToggledOn"と"ToggledOff"文字列を 2 つのテキストを持つメソッドです。 読み取ることができるはこの方法と、アプリケーションが、記事の内容の表示状態に応答できる方法に関する[ **、Xamarin.Forms Visual State Manager**](~/xamarin-forms/user-interface/visual-state-manager.md)です。 

`ToggleButton`呼び出しを行う`VisualStateManager.GoToState`、クラス自体は追加の機能に基づいて、ボタンの外観を変更する必要はありません、`IsToggled`状態です。 ホストする XAML の責任は、`ToggleButton`です。 

**トグル ボタンのデモ**ページには、2 つのインスタンスが含まれています。 `ToggleButton`、設定 Visual State Manager マークアップを含む、 `Text`、 `BackgroundColor`、および`TextColor`ビジュアルの状態に基づいて、ボタンの。 

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

`Toggled`分離コード ファイルには、イベント ハンドラー。 設定を担当している、`FontAttributes`のプロパティ、`Label`ボタンの状態に基づきます。

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

IOS、Android、および、UWP で実行されているプログラムを次に示します。

[![ボタンのデモを切り替える](button-images/ToggleButtonDemo.png "ボタンのデモを切り替える")](button-images/ToggleButtonDemo-Large.png#lightbox)

<a name="image-button" />

## <a name="using-bitmaps-with-buttons"></a>ボタンにビットマップを使用します。

`Button`クラスを定義、 [ `Image` ](xref:Xamarin.Forms.Button.Image)プロパティにビットマップ イメージを表示できるようにする、 `Button`、単独、またはテキストと組み合わせる。 テキストおよびイメージの配置方法を指定することもできます。

`Image`プロパティの型は[ `FileImageSource` ](xref:Xamarin.Forms.FileImageSource)、つまり、プロジェクトでは、個々 のプラットフォーム、および標準的な .NET のライブラリ プロジェクトではなく、リソースとして、ビットマップを格納する必要があります。 

Xamarin.Forms でサポートされている各プラットフォームにより、イメージを複数のサイズをアプリケーションが実行されるさまざまなデバイスの別のピクセルの解像度で保存できます。 複数のビットマップの名前付きまたはオペレーティング システムは、デバイスのビデオは最適な一致を選択できます、このような方法で格納される画面の解像度。 

上のビットマップの`Button`、最適なサイズは 32、64 のデバイスに依存しない単位間で、通常、規模によって場合があります。 この例では使用されている画像は、48 のデバイスに依存しない単位のサイズに基づきます。

IOS プロジェクトで、**リソース**フォルダーには、このイメージのサイズには 3 種類が含まれています。

- 48 ピクセルの四角形ビットマップとして格納されている **/Resources/MonkeyFace.png**
- 96 ピクセルの四角形ビットマップとして格納されています。 **/Resource/MonkeyFace@2x.png**
- 144 ピクセルの四角形ビットマップとして格納されています。 **/Resource/MonkeyFace@3x.png**

3 つすべてのビットマップに付与された、**ビルド アクション**の**BundleResource**です。

Android のプロジェクトのすべてのビットマップが同じ名前を持つのさまざまなサブフォルダーに格納されて、**リソース**フォルダー。

- 72 ピクセルの四角形ビットマップとして格納されている **/Resources/drawable-hdpi/MonkeyFace.png**
- 96 ピクセルの四角形ビットマップとして格納されている **/Resources/drawable-xhdpi/MonkeyFace.png**
- 144 ピクセルの四角形ビットマップとして格納されている **/Resources/drawable-xxhdpi/MonkeyFace.png**
- 192 ピクセルの四角形ビットマップとして格納されている **/Resources/drawable-xxxhdpi/MonkeyFace.png**

これらに付与された、**ビルド アクション**の**AndroidResource**です。

UWP プロジェクトがプロジェクトで、ビットマップをどこでも格納できますが、カスタム フォルダーで一般的に格納されている、または**資産**既存のフォルダーです。 UWP プロジェクトには、これらのビットマップが含まれています。

- 48 ピクセルの四角形ビットマップとして格納されている **/Assets/MonkeyFace.scale-100.png**
- 96 ピクセルの四角形ビットマップとして格納されている **/Assets/MonkeyFace.scale-200.png**
- 192 ピクセルの四角形ビットマップとして格納されている **/Assets/MonkeyFace.scale-400.png**

これらすべて受賞、**ビルド アクション**の**コンテンツ**です。

指定できる方法、`Text`と`Image`上でプロパティの配置、`Button`を使用して、 [ `ContentLayout` ](xref:Xamarin.Forms.Button.ContentLayout)プロパティの`Button`します。 このプロパティの型は[ `ButtonContentLayout` ](xref:Xamarin.Forms.Button.ButtonContentLayout)、これに埋め込まれたクラス`Button`です。 [コンス トラクター](xref:Xamarin.Forms.Button.ButtonContentLayout.%23ctor(Xamarin.Forms.Button.ButtonContentLayout.ImagePosition,System.Double))が 2 つの引数。

- メンバー、 [ `ImagePosition` ](xref:Xamarin.Forms.Button.ButtonContentLayout.ImagePosition)列挙: `Left`、 `Top`、 `Right`、または`Bottom`テキストの基準としたビットマップの表示方法を示すです。
- A`double`ビットマップとテキスト間の間隔の値。

既定値は`Left`および 10 ユニットです。 2 つの読み取り専用プロパティ`ButtonContentLayout`という[ `Position` ](xref:Xamarin.Forms.Button.ButtonContentLayout.Position)と[ `Spacing` ](xref:Xamarin.Forms.Button.ButtonContentLayout.Spacing)これらのプロパティの値を指定します。

コードでは、作成することができます、`Button`設定と、`ContentLayout`次のようなプロパティ。

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

XAML では、列挙体のメンバーまたは間隔を指定する必要があります。 またはコンマで区切って任意の順序で両方。

```xaml
<Button Text="button text"
        Image="image filename"
        ContentLayout="Right, 20" />
```

**イメージ ボタン デモ**ページ使用`OnPlatform`を iOS、Android、および UWP ビットマップ ファイルの異なるファイル名を指定します。 3 つすべてのプラットフォームの同じファイル名を使用して、使用しないようにしたい場合`OnPlatform`、UWP ビットマップをプロジェクトのルート ディレクトリに格納する必要があります。

最初の`Button`上、**イメージ ボタン デモ**セット ページ、`Image`プロパティは対象外、`Text`プロパティ。

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

UWP ビットマップは、プロジェクトのルート ディレクトリに保存されている場合、このマークアップはかなり簡略化できます。

```xaml
<Button Image="MonkeyFace.png" />
```

多くのマークアップを繰り返し実行するを避けるために、 **ImageButtonDemo.xaml**ファイル、暗黙的な`Style`設定にも定義されています、`Image`プロパティです。 これは、`Style`を他の 5 つが自動的に適用`Button`要素。 完全な XAML ファイルを次に示します。

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

[![ボタンのデモをイメージ](button-images/ImageButtonDemo.png "デモのボタンの画像")](button-images/ImageButtonDemo-Large.png#lightbox)

処理できるさまざまな方法を確認したようになりました`Button`イベントと変更、`Button`外観です。

## <a name="related-links"></a>関連リンク

- [ButtonDemos サンプル](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos)
- [ボタン API](xref:Xamarin.Forms.Button)
