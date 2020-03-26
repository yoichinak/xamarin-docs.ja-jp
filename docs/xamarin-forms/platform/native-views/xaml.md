---
title: XAML のネイティブ ビュー
description: IOS、Android、およびユニバーサル Windows プラットフォームからのネイティブ ビューは、Xamarin.Forms XAML ファイルから直接参照できます。 プロパティとイベント ハンドラーは、ネイティブのビューを設定でき、Xamarin.Forms のビューとやり取りすることができます。 この記事では、Xamarin.Forms XAML ファイルからのネイティブ ビューを使用する方法を示します。
ms.prod: xamarin
ms.assetid: 7A856D31-B300-409E-9AEB-F8A4DB99B37E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2019
ms.openlocfilehash: 6d3954f44cd769ed02535eb260b9952e81e67c98
ms.sourcegitcommit: d83c6af42ed26947aa7c0ecfce00b9ef60f33319
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/25/2020
ms.locfileid: "80247627"
---
# <a name="native-views-in-xaml"></a>XAML のネイティブ ビュー

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeswitch)

_IOS、Android、およびユニバーサル Windows プラットフォームからのネイティブビューは、Xamarin の XAML ファイルから直接参照できます。プロパティとイベントハンドラーは、ネイティブビューで設定でき、Xamarin のビューと対話できます。この記事では、Xamarin の XAML ファイルからネイティブビューを使用する方法について説明します。_

この記事では、次のトピックについて説明します。

- [ネイティブビュー](#consuming)の使用– XAML からネイティブビューを使用するプロセス。
- [ネイティブバインディングの使用](#native_bindings)–ネイティブビューのプロパティとの間のデータバインディング。
- ネイティブビュー[に引数を渡す](#passing_arguments)–ネイティブビューコンストラクターに引数を渡し、ネイティブビューファクトリメソッドを呼び出します。
- [コードからのネイティブビューの参照](#native_view_code)– XAML ファイルで宣言されたネイティブビューインスタンスを分離コードファイルから取得します。
- [ネイティブビュー](#subclassing)のサブクラス化–ネイティブビューをサブクラス化して、XAML を使いやすい API を定義します。  

<a name="overview" />

## <a name="overview"></a>概要

Xamarin.Forms XAML ファイルにネイティブのビューを埋め込むには。

1. ネイティブビューを含む名前空間の XAML ファイルに `xmlns` 名前空間宣言を追加します。
1. XAML ファイルでのネイティブ ビューのインスタンスを作成します。

> [!IMPORTANT]
> ネイティブビューを使用するすべての XAML ページでは、コンパイルされた XAML を無効にする必要があります。 これは、XAML ページの分離コードクラスを `[XamlCompilation(XamlCompilationOptions.Skip)]` 属性で修飾することによって実現できます。 XAML コンパイルの詳細については、「 [Xamarin. Forms での xaml のコンパイル](~/xamarin-forms/xaml/xamlc.md)」を参照してください。

分離コード ファイルからのネイティブ ビューを参照するには、共有資産プロジェクト (SAP) を使用して、条件付きコンパイル ディレクティブを使用してプラットフォーム固有のコードをラップする必要があります。 詳細については、「[コードからのネイティブビューの参照](#native_view_code)」を参照してください。

<a name="consuming" />

## <a name="consuming-native-views"></a>ネイティブ ビューの使用

次のコード例は、各プラットフォームのネイティブビューを Xamarin. Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage)に使用する方法を示しています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:win="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        x:Class="NativeViews.NativeViewDemo">
    <StackLayout Margin="20">
        <ios:UILabel Text="Hello World" TextColor="{x:Static ios:UIColor.Red}" View.HorizontalOptions="Start" />
        <androidWidget:TextView Text="Hello World" x:Arguments="{x:Static androidLocal:MainActivity.Instance}" />
        <win:TextBlock Text="Hello World" />
    </StackLayout>
</ContentPage>
```

ネイティブビュー名前空間の `clr-namespace` と `assembly` を指定するだけでなく、`targetPlatform` も指定する必要があります。 これは、`iOS`、`Android`、`UWP`、`Windows` (`UWP`と同等)、`macOS`、`GTK`、`Tizen`、`WPF`に設定する必要があります。 実行時には、アプリケーションが実行されているプラットフォームと一致しない `targetPlatform` を持つ XML 名前空間プレフィックスは、XAML パーサーによって無視されます。

指定した名前空間から任意のクラスまたは構造体を参照する各名前空間宣言を使用できます。 たとえば、`ios` 名前空間宣言を使用して、iOS `UIKit` 名前空間から任意のクラスまたは構造体を参照することができます。 ネイティブ ビューのプロパティは、XAML で設定できますが、プロパティとオブジェクトの型が一致する必要があります。 たとえば、`UILabel.TextColor` プロパティは、`x:Static` マークアップ拡張機能と `ios` 名前空間を使用して `UIColor.Red` に設定されます。

バインド可能なプロパティとアタッチ可能なバインド可能なプロパティは、`Class.BindableProperty="value"` 構文を使用してネイティブビューで設定することもできます。 各ネイティブビューは、 [`Xamarin.Forms.View`](xref:Xamarin.Forms.View)クラスから派生したプラットフォーム固有の `NativeViewWrapper` インスタンスにラップされます。 ラッパーにプロパティの値を転送するネイティブのビューにバインド可能なプロパティまたは添付のバインド可能なプロパティを設定します。 たとえば、中央の水平レイアウトは、ネイティブビューで `View.HorizontalOptions="Center"` を設定することによって指定できます。

> [!NOTE]
> スタイルは、`BindableProperty` オブジェクトによってサポートされるプロパティのみをターゲットにすることができるため、ネイティブビューでは使用できないことに注意してください。

Android ウィジェットのコンストラクターでは、通常、引数として Android `Context` オブジェクトが必要です。これは、`MainActivity` クラスの静的プロパティを介して使用できます。 そのため、XAML で Android ウィジェットを作成する場合、通常、`Context` オブジェクトは、`x:Arguments` 属性と `x:Static` マークアップ拡張機能を使用して、ウィジェットのコンストラクターに渡す必要があります。 詳細については、「[ネイティブビューへの引数の引き渡し](#passing_arguments)」を参照してください。

> [!NOTE]
> `x:Name` によるネイティブビューへの名前付けは、.NET Standard ライブラリプロジェクトまたは共有アセットプロジェクト (SAP) では実行できないことに注意してください。 そうと、コンパイル エラーの原因は、ネイティブの型の変数が生成されます。 ただし、SAP が使用されている場合は、ネイティブビューを `ContentView` インスタンスにラップし、分離コードファイルで取得できます。 詳細については、「[コードからのネイティブビューの参照](#native_view_code)」を参照してください。

<a name="native_bindings" />

## <a name="native-bindings"></a>ネイティブのバインド

データ バインディングは、そのデータ ソースと UI を同期するために使用し、Xamarin.Forms アプリケーションの表示し、そのデータをやり取りするを合理化します。 ソースオブジェクトが `INotifyPropertyChanged` インターフェイスを実装している場合、*ソース*オブジェクトの変更はバインドフレームワークによって*ターゲット*オブジェクトに自動的にプッシュされます。また、*ターゲット*オブジェクトの変更は、必要に応じて*ソース*オブジェクトにプッシュすることもできます。

ネイティブ ビューのプロパティは、データ バインディングにも使用できます。 次のコード例では、ネイティブ ビューのプロパティを使用してデータ バインディングを示しています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:win="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:NativeSwitch"
        x:Class="NativeSwitch.NativeSwitchPage">
    <StackLayout Margin="20">
        <Label Text="Native Views Demo" FontAttributes="Bold" HorizontalOptions="Center" />
        <Entry Placeholder="This Entry is bound to the native switch" IsEnabled="{Binding IsSwitchOn}" />
        <ios:UISwitch On="{Binding Path=IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=ValueChanged}"
            OnTintColor="{x:Static ios:UIColor.Red}"
            ThumbTintColor="{x:Static ios:UIColor.Blue}" />
        <androidWidget:Switch x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
            Checked="{Binding Path=IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=CheckedChange}"
            Text="Enable Entry?" />
        <win:ToggleSwitch Header="Enable Entry?"
            OffContent="No"
            OnContent="Yes"
            IsOn="{Binding IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=Toggled}" />
    </StackLayout>
</ContentPage>

```

このページには、 [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled)プロパティが `NativeSwitchPageViewModel.IsSwitchOn` プロパティにバインドされた[`Entry`](xref:Xamarin.Forms.Entry)が含まれています。 ページの[`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)は、分離コードファイルの `NativeSwitchPageViewModel` クラスの新しいインスタンスに設定され、`INotifyPropertyChanged` インターフェイスを実装するビューモデルクラスを使用します。

ページには、各プラットフォームのネイティブのスイッチも含まれています。 各ネイティブスイッチは、 [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay)バインディングを使用して、`NativeSwitchPageViewModel.IsSwitchOn` プロパティの値を更新します。 このため、スイッチがオフの場合、`Entry` は無効になり、スイッチをオンにすると `Entry` が有効になります。 次のスクリーン ショットは、各プラットフォームでこの機能を示します。

![](xaml-images/native-switch-disabled.png "Native Switch Disabled")
![](xaml-images/native-switch-enabled.png "Native Switch Enabled")

双方向のバインディングは、ネイティブプロパティが `INotifyPropertyChanged`を実装している場合、または iOS でのキー値の観察 (KVO) をサポートしている場合、または UWP の `DependencyProperty` である場合に、自動的にサポートされます。 ただし、多くのネイティブ ビューでは、プロパティの変更通知をサポートしません。 これらのビューでは、バインド式の一部として[`UpdateSourceEventName`](xref:Xamarin.Forms.Binding.UpdateSourceEventName)プロパティ値を指定できます。 このプロパティは、ターゲット プロパティが変更されたときに通知するネイティブ ビュー内のイベントの名前に設定する必要があります。 次に、ネイティブスイッチの値が変更されると、ユーザーがスイッチの値を変更したことと、`NativeSwitchPageViewModel.IsSwitchOn` プロパティの値が更新されたことが `Binding` クラスに通知されます。

<a name="passing_arguments" />

## <a name="passing-arguments-to-native-views"></a>ネイティブ ビューに引数を渡す

コンストラクター引数は、`x:Static` マークアップ拡張機能を持つ `x:Arguments` 属性を使用してネイティブビューに渡すことができます。 また、ネイティブビューファクトリメソッド`public static` (メソッドを定義するクラスまたは構造体と同じ型のオブジェクトまたは値を返すメソッド) を呼び出すこともできます。そのためには、`x:FactoryMethod` 属性を使用してメソッドの名前を指定し、`x:Arguments` 属性を使用してその引数を指定します。

次のコード例では、両方の方法を示しています。

```xaml
<ContentPage ...
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidGraphics="clr-namespace:Android.Graphics;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winMedia="clr-namespace:Windows.UI.Xaml.Media;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winText="clr-namespace:Windows.UI.Text;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winui="clr-namespace:Windows.UI;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows">
        ...
        <ios:UILabel Text="Simple Native Color Picker" View.HorizontalOptions="Center">
            <ios:UILabel.Font>
                <ios:UIFont x:FactoryMethod="FromName">
                    <x:Arguments>
                        <x:String>Papyrus</x:String>
                        <x:Single>24</x:Single>
                    </x:Arguments>
                </ios:UIFont>
            </ios:UILabel.Font>
        </ios:UILabel>
        <androidWidget:TextView x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                    Text="Simple Native Color Picker"
                    TextSize="24"
                    View.HorizontalOptions="Center">
            <androidWidget:TextView.Typeface>
                <androidGraphics:Typeface x:FactoryMethod="Create">
                    <x:Arguments>
                        <x:String>cursive</x:String>
                        <androidGraphics:TypefaceStyle>Normal</androidGraphics:TypefaceStyle>
                    </x:Arguments>
                </androidGraphics:Typeface>
            </androidWidget:TextView.Typeface>
        </androidWidget:TextView>
        <winControls:TextBlock Text="Simple Native Color Picker"
                    FontSize="20"
                    FontStyle="{x:Static winText:FontStyle.Italic}"
                    View.HorizontalOptions="Center">
            <winControls:TextBlock.FontFamily>
                <winMedia:FontFamily>
                    <x:Arguments>
                        <x:String>Georgia</x:String>
                    </x:Arguments>
                </winMedia:FontFamily>
            </winControls:TextBlock.FontFamily>
        </winControls:TextBlock>
        ...
</ContentPage>
```

[`UIFont.FromName`](xref:UIKit.UIFont.FromName*)ファクトリメソッドは、 [`UILabel.Font`](xref:UIKit.UILabel.Font)プロパティを iOS の新しい[`UIFont`](xref:UIKit.UIFont)に設定するために使用されます。 `UIFont` の名前とサイズは、`x:Arguments` 属性の子であるメソッド引数によって指定されます。

[`Typeface.Create`](xref:Android.Graphics.Typeface.Create*)ファクトリメソッドは、 [`TextView.Typeface`](xref:Android.Widget.TextView.Typeface)プロパティを Android の新しい[`Typeface`](xref:Android.Graphics.Typeface)に設定するために使用されます。 `Typeface` ファミリの名前とスタイルは、`x:Arguments` 属性の子であるメソッド引数によって指定されます。

[`FontFamily`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.fontfamily)コンストラクターは、 [`TextBlock.FontFamily`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.fontfamily)プロパティをユニバーサル Windows プラットフォーム (UWP) の新しい `FontFamily` に設定するために使用されます。 `FontFamily` 名は、`x:Arguments` 属性の子であるメソッド引数によって指定されます。

> [!NOTE]
> 引数は、コンス トラクターまたはファクトリ メソッドで必要な型と一致する必要があります。

次のスクリーン ショットでは、別のネイティブ ビューのフォントを設定するファクトリ メソッドとコンス トラクターの引数を指定して結果を表示します。

![](xaml-images/passing-arguments.png "Setting Fonts on Native Views")

XAML で引数を渡す方法の詳細については、「 [xaml で引数を渡す](~/xamarin-forms/xaml/passing-arguments.md)」を参照してください。

<a name="native_view_code" />

## <a name="referring-to-native-views-from-code"></a>コードからネイティブ ビューを参照します。

`x:Name` 属性でネイティブビューに名前を指定することはできませんが、ネイティブビューが `x:Name` 属性値を指定する[`ContentView`](xref:Xamarin.Forms.ContentView)の子である場合は、共有アクセスプロジェクトの分離コードファイルから XAML ファイルで宣言されたネイティブビューインスタンスを取得できます。 次に、分離コード ファイルで条件付きコンパイル ディレクティブ内で行う必要があります。

1. [`ContentView.Content`](xref:Xamarin.Forms.ContentView.Content)プロパティ値を取得し、それをプラットフォーム固有の `NativeViewWrapper` 型にキャストします。
1. `NativeViewWrapper.NativeElement` プロパティを取得し、それをネイティブビュー型にキャストします。

ネイティブの API は、目的の操作を実行するネイティブのビューを起動できます。 このアプローチでは、異なるプラットフォームの複数の XAML ネイティブビューが同じ[`ContentView`](xref:Xamarin.Forms.ContentView)の子になることができるという利点もあります。 次のコード例では、この手法を示します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:NativeViewInsideContentView"
        x:Class="NativeViewInsideContentView.NativeViewInsideContentViewPage">
    <StackLayout Margin="20">
        <ContentView x:Name="contentViewTextParent" HorizontalOptions="Center" VerticalOptions="CenterAndExpand">
            <ios:UILabel Text="Text in a UILabel" TextColor="{x:Static ios:UIColor.Red}" />
            <androidWidget:TextView x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                Text="Text in a TextView" />
              <winControls:TextBlock Text="Text in a TextBlock" />
        </ContentView>
        <ContentView x:Name="contentViewButtonParent" HorizontalOptions="Center" VerticalOptions="EndAndExpand">
            <ios:UIButton TouchUpInside="OnButtonTap" View.HorizontalOptions="Center" View.VerticalOptions="Center" />
            <androidWidget:Button x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                Text="Scale and Rotate Text"
                Click="OnButtonTap" />
            <winControls:Button Content="Scale and Rotate Text" />
        </ContentView>
    </StackLayout>
</ContentPage>
```

上記の例では、各プラットフォームのネイティブビューは[`ContentView`](xref:Xamarin.Forms.ContentView)コントロールの子であり、`x:Name` 属性値を使用して分離コード内の `ContentView` を取得します。

```csharp
public partial class NativeViewInsideContentViewPage : ContentPage
{
    public NativeViewInsideContentViewPage()
    {
        InitializeComponent();

#if __IOS__
        var wrapper = (Xamarin.Forms.Platform.iOS.NativeViewWrapper)contentViewButtonParent.Content;
        var button = (UIKit.UIButton)wrapper.NativeView;
        button.SetTitle("Scale and Rotate Text", UIKit.UIControlState.Normal);
        button.SetTitleColor(UIKit.UIColor.Black, UIKit.UIControlState.Normal);
#endif
#if __ANDROID__
        var wrapper = (Xamarin.Forms.Platform.Android.NativeViewWrapper)contentViewTextParent.Content;
        var textView = (Android.Widget.TextView)wrapper.NativeView;
        textView.SetTextColor(Android.Graphics.Color.Red);
#endif
#if WINDOWS_UWP
        var textWrapper = (Xamarin.Forms.Platform.UWP.NativeViewWrapper)contentViewTextParent.Content;
        var textBlock = (Windows.UI.Xaml.Controls.TextBlock)textWrapper.NativeElement;
        textBlock.Foreground = new Windows.UI.Xaml.Media.SolidColorBrush(Windows.UI.Colors.Red);
        var buttonWrapper = (Xamarin.Forms.Platform.UWP.NativeViewWrapper)contentViewButtonParent.Content;
        var button = (Windows.UI.Xaml.Controls.Button)buttonWrapper.NativeElement;
        button.Click += (sender, args) => OnButtonTap(sender, EventArgs.Empty);
#endif
    }

    async void OnButtonTap(object sender, EventArgs e)
    {
        contentViewButtonParent.Content.IsEnabled = false;
        contentViewTextParent.Content.ScaleTo(2, 2000);
        await contentViewTextParent.Content.RotateTo(360, 2000);
        contentViewTextParent.Content.ScaleTo(1, 2000);
        await contentViewTextParent.Content.RelRotateTo(360, 2000);
        contentViewButtonParent.Content.IsEnabled = true;
    }
}
```

[`ContentView.Content`](xref:Xamarin.Forms.ContentView.Content)プロパティにアクセスして、ラップされたネイティブビューをプラットフォーム固有の `NativeViewWrapper` インスタンスとして取得します。 次に、ネイティブの型としてネイティブビューを取得するために、`NativeViewWrapper.NativeElement` プロパティにアクセスします。 ネイティブ ビューの API は、目的の操作を実行し、呼び出されます。

IOS と Android のネイティブボタンは、同じ `OnButtonTap` イベントハンドラーを共有します。これは、各ネイティブボタンがタッチイベントに応答して `EventHandler` デリゲートを使用するためです。 ただし、ユニバーサル Windows プラットフォーム (UWP) は別の `RoutedEventHandler`を使用し、この例では `OnButtonTap` イベントハンドラーを使用します。 したがって、ネイティブボタンがクリックされると、`OnButtonTap` イベントハンドラーが実行され、`contentViewTextParent`という名前の[`ContentView`](xref:Xamarin.Forms.ContentView)内に含まれるネイティブコントロールがスケールおよび回転されます。 次のスクリーン ショットは、各プラットフォームで発生しているこのを示しています。

![](xaml-images/contentview.png "ContentView Containing a Native Control")

<a name="subclassing" />

## <a name="subclassing-native-views"></a>ネイティブ ビューのサブクラス化

IOS と Android のネイティブ ビューの多くでは、コントロールを設定するプロパティではなく、メソッドを使用するために、XAML でインスタンス化するには適しません。 この問題の解決策が、プロパティを使用して、コントロールを設定して、プラットフォームに依存しないイベントを使用する複数の XAML に適した API を定義するラッパーのネイティブ ビューのサブクラスです。 ラップされたネイティブ ビューし共有資産プロジェクト (SAP) での配置し、条件付きコンパイル ディレクティブで囲まれてまたはプラットフォームに固有のプロジェクトに配置でき、.NET Standard ライブラリ プロジェクトでは、XAML から参照されています。

次のコード例では、サブクラス化ネイティブ ビューを使用するための Xamarin.Forms ページを示しています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:iosLocal="clr-namespace:SubclassedNativeControls.iOS;assembly=SubclassedNativeControls.iOS;targetPlatform=iOS"
        xmlns:android="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SubclassedNativeControls.Droid;assembly=SubclassedNativeControls.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:SubclassedNativeControls"
        x:Class="SubclassedNativeControls.SubclassedNativeControlsPage">
    <StackLayout Margin="20">
        <Label Text="Subclassed Native Views Demo" FontAttributes="Bold" HorizontalOptions="Center" />
        <StackLayout Orientation="Horizontal">
          <Label Text="You have chosen:" />
          <Label Text="{Binding SelectedFruit}" />      
        </StackLayout>
        <iosLocal:MyUIPickerView ItemsSource="{Binding Fruits}"
            SelectedItem="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=SelectedItemChanged}" />
        <androidLocal:MySpinner x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
            ItemsSource="{Binding Fruits}"
            SelectedObject="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=ItemSelected}" />
        <winControls:ComboBox ItemsSource="{Binding Fruits}"
            SelectedItem="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=SelectionChanged}" />
    </StackLayout>
</ContentPage>
```

このページには、ユーザーがネイティブコントロールから選択した果物を表示する[`Label`](xref:Xamarin.Forms.Label)が含まれています。 `Label` `SubclassedNativeControlsPageViewModel.SelectedFruit` プロパティにバインドされます。 ページの[`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)は、分離コードファイルの `SubclassedNativeControlsPageViewModel` クラスの新しいインスタンスに設定され、`INotifyPropertyChanged` インターフェイスを実装するビューモデルクラスを使用します。

ページには、各プラットフォームのネイティブ ピッカー ビューも含まれています。 各ネイティブビューには、その `ItemSource` プロパティを `SubclassedNativeControlsPageViewModel.Fruits` コレクションにバインドすることによって、果物のコレクションが表示されます。 これにより、次のスクリーン ショットに示すように、ユーザー、フルーツを選択します。

![](xaml-images/sub-classed.png "Subclassed Native Views")

IOS と Android では、ネイティブの選択は、コントロールを設定するのにメソッドを使用します。 そのため、これらの選択は、XAML に対応できるようにするプロパティを公開するサブクラス化する必要があります。 ユニバーサル Windows プラットフォーム (UWP) では、`ComboBox` は既に XAML フレンドリであるため、サブクラス化は必要ありません。

### <a name="ios"></a>iOS

IOS 実装では、 [`UIPickerView`](xref:UIKit.UIPickerView)ビューがサブクラス化され、XAML から簡単に使用できるプロパティとイベントが公開されます。

```csharp
public class MyUIPickerView : UIPickerView
{
    public event EventHandler<EventArgs> SelectedItemChanged;

    public MyUIPickerView()
    {
        var model = new PickerModel();
        model.ItemChanged += (sender, e) =>
        {
            if (SelectedItemChanged != null)
            {
                SelectedItemChanged.Invoke(this, e);
            }
        };
        Model = model;
    }

    public IList<string> ItemsSource
    {
        get
        {
            var pickerModel = Model as PickerModel;
            return (pickerModel != null) ? pickerModel.Items : null;
        }
        set
        {
            var model = Model as PickerModel;
            if (model != null)
            {
                model.Items = value;
            }
        }
    }

    public string SelectedItem
    {
        get { return (Model as PickerModel).SelectedItem; }
        set { }
    }
}
```

`MyUIPickerView` クラスは、`ItemsSource` プロパティと `SelectedItem` プロパティ、および `SelectedItemChanged` イベントを公開します。 [`UIPickerView`](xref:UIKit.UIPickerView)には、基になる[`UIPickerViewModel`](xref:UIKit.UIPickerViewModel)データモデルが必要です。これは、`MyUIPickerView` のプロパティとイベントによってアクセスされます。 `UIPickerViewModel` データモデルは、`PickerModel` クラスによって提供されます。

```csharp
class PickerModel : UIPickerViewModel
{
    int selectedIndex = 0;
    public event EventHandler<EventArgs> ItemChanged;
    public IList<string> Items { get; set; }

    public string SelectedItem
    {
        get
        {
            return Items != null && selectedIndex >= 0 && selectedIndex < Items.Count ? Items[selectedIndex] : null;
        }
    }

    public override nint GetRowsInComponent(UIPickerView pickerView, nint component)
    {
        return Items != null ? Items.Count : 0;
    }

    public override string GetTitle(UIPickerView pickerView, nint row, nint component)
    {
        return Items != null && Items.Count > row ? Items[(int)row] : null;
    }

    public override nint GetComponentCount(UIPickerView pickerView)
    {
        return 1;
    }

    public override void Selected(UIPickerView pickerView, nint row, nint component)
    {
        selectedIndex = (int)row;
        if (ItemChanged != null)
        {
            ItemChanged.Invoke(this, new EventArgs());
        }
    }
}
```

`PickerModel` クラスは、`Items` プロパティを使用して、`MyUIPickerView` クラスの基になるストレージを提供します。 `MyUIPickerView` で選択した項目が変更されるたびに、 [`Selected`](xref:UIKit.UIPickerViewModel.Selected*)メソッドが実行され、選択したインデックスが更新され、`ItemChanged` イベントが発生します。 これにより、`SelectedItem` プロパティは常に、ユーザーによって選択された最後の項目を返します。 また、`PickerModel` クラスは、`MyUIPickerView` インスタンスのセットアップに使用されるメソッドをオーバーライドします。

### <a name="android"></a>Android

Android 実装では、 [`Spinner`](xref:Android.Widget.Spinner)ビューがサブクラス化され、XAML から簡単に使用できるプロパティとイベントが公開されます。

```csharp
class MySpinner : Spinner
{
    ArrayAdapter adapter;
    IList<string> items;

    public IList<string> ItemsSource
    {
        get { return items; }
        set
        {
            if (items != value)
            {
                items = value;
                adapter.Clear();

                foreach (string str in items)
                {
                    adapter.Add(str);
                }
            }
        }
    }

    public string SelectedObject
    {
        get { return (string)GetItemAtPosition(SelectedItemPosition); }
        set
        {
            if (items != null)
            {
                int index = items.IndexOf(value);
                if (index != -1)
                {
                    SetSelection(index);
                }
            }
        }
    }

    public MySpinner(Context context) : base(context)
    {
        ItemSelected += OnBindableSpinnerItemSelected;

        adapter = new ArrayAdapter(context, Android.Resource.Layout.SimpleSpinnerItem);
        adapter.SetDropDownViewResource(Android.Resource.Layout.SimpleSpinnerDropDownItem);
        Adapter = adapter;
    }

    void OnBindableSpinnerItemSelected(object sender, ItemSelectedEventArgs args)
    {
        SelectedObject = (string)GetItemAtPosition(args.Position);
    }
}
```

`MySpinner` クラスは、`ItemsSource` プロパティと `SelectedObject` プロパティ、および `ItemSelected` イベントを公開します。 `MySpinner` クラスによって表示される項目は、ビューに関連付けられた[`Adapter`](xref:Android.Widget.Adapter)によって提供され、`ItemsSource` プロパティが最初に設定されたときに項目が `Adapter` に設定されます。 `MySpinner` クラスで選択された項目が変更されるたびに、`OnBindableSpinnerItemSelected` イベントハンドラーによって `SelectedObject` プロパティが更新されます。

## <a name="summary"></a>まとめ

この記事では、Xamarin.Forms XAML ファイルからのネイティブ ビューを使用する方法を示しました。 プロパティとイベント ハンドラーは、ネイティブのビューを設定でき、Xamarin.Forms のビューとやり取りすることができます。

## <a name="related-links"></a>関連リンク

- [NativeSwitch (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeswitch)
- [Forms2Native (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/forms2native)
- [NativeViewInsideContentView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeviewinsidecontentview)
- [SubclassedNativeControls (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-subclassednativecontrols)
- [ネイティブ フォーム](~/xamarin-forms/platform/native-forms.md)
- [XAML での引数の受け渡し](~/xamarin-forms/xaml/passing-arguments.md)
