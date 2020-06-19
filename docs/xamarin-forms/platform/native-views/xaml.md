---
title: XAML のネイティブ ビュー
description: IOS、Android、およびユニバーサル Windows プラットフォームからのネイティブビューは、XAML ファイルから直接参照でき Xamarin.Forms ます。 プロパティとイベントハンドラーはネイティブビューで設定でき、ビューと対話でき Xamarin.Forms ます。 この記事では、XAML ファイルからネイティブビューを使用する方法について説明 Xamarin.Forms します。
ms.prod: xamarin
ms.assetid: 7A856D31-B300-409E-9AEB-F8A4DB99B37E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0d6aa84a886d450d1dc42ec31edf16380b795404
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84564642"
---
# <a name="native-views-in-xaml"></a>XAML のネイティブ ビュー

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeswitch)

_IOS、Android、およびユニバーサル Windows プラットフォームからのネイティブビューは、XAML ファイルから直接参照でき Xamarin.Forms ます。プロパティとイベントハンドラーはネイティブビューで設定でき、ビューと対話でき Xamarin.Forms ます。この記事では、XAML ファイルからネイティブビューを使用する方法について説明 Xamarin.Forms します。_

ネイティブビューを XAML ファイルに埋め込むには、次のようにし Xamarin.Forms ます。

1. `xmlns`ネイティブビューを含む名前空間の XAML ファイルに名前空間宣言を追加します。
1. XAML ファイルにネイティブビューのインスタンスを作成します。

> [!IMPORTANT]
> ネイティブビューを使用するすべての XAML ページでは、コンパイルされた XAML を無効にする必要があります。 これは、XAML ページの分離コードクラスを属性で修飾することによって実現でき `[XamlCompilation(XamlCompilationOptions.Skip)]` ます。 XAML のコンパイルの詳細については、「 [」の Xamarin.Forms 「Xaml のコンパイル](~/xamarin-forms/xaml/xamlc.md)」を参照してください。

分離コードファイルからネイティブビューを参照するには、共有アセットプロジェクト (SAP) を使用し、条件付きコンパイルディレクティブを使用してプラットフォーム固有のコードをラップする必要があります。 詳細については、「[コードからのネイティブビューの参照」を](#refer-to-native-views-from-code)参照してください。

## <a name="consume-native-views"></a>ネイティブビューの使用

次のコード例は、各プラットフォームのネイティブビューをに使用する方法を示してい Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) ます。

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

`clr-namespace`ネイティブビューの名前空間に対しておよびを指定するだけでなく、 `assembly` `targetPlatform` も指定する必要があります。 これは、、、 `iOS` 、 `Android` `UWP` `Windows` `UWP` `macOS` `GTK` `Tizen` のいずれか `WPF` に設定する必要があります。 実行時には、アプリケーションが実行されているプラットフォームと一致しないを持つ XML 名前空間プレフィックスは、XAML パーサーによって無視され `targetPlatform` ます。

各名前空間宣言は、指定された名前空間から任意のクラスまたは構造体を参照するために使用できます。 たとえば、 `ios` 名前空間宣言を使用して、iOS 名前空間から任意のクラスまたは構造体を参照することができ `UIKit` ます。 ネイティブビューのプロパティは、XAML を使用して設定できますが、プロパティとオブジェクトの種類が一致している必要があります。 たとえば、 `UILabel.TextColor` プロパティは、 `UIColor.Red` `x:Static` マークアップ拡張機能と名前空間を使用してに設定され `ios` ます。

バインド可能なプロパティおよびアタッチ可能なバインド可能なプロパティは、構文を使用してネイティブビューで設定することもでき `Class.BindableProperty="value"` ます。 各ネイティブビューは、クラスから派生したプラットフォーム固有のインスタンスにラップされ `NativeViewWrapper` [`Xamarin.Forms.View`](xref:Xamarin.Forms.View) ます。 ネイティブビューでバインド可能なプロパティまたはアタッチ可能なバインド可能プロパティを設定すると、プロパティ値がラッパーに転送されます。 たとえば、中央の水平レイアウトは、ネイティブビューでを設定することによって指定でき `View.HorizontalOptions="Center"` ます。

> [!NOTE]
> スタイルは、オブジェクトによってサポートされるプロパティのみを対象とするため、ネイティブビューでは使用できないことに注意して `BindableProperty` ください。

Android ウィジェットのコンストラクターには、通常、 `Context` 引数として android オブジェクトが必要です。これは、クラスの静的プロパティを通じて使用でき `MainActivity` ます。 そのため、XAML で Android ウィジェットを作成する場合は、 `Context` 通常、 `x:Arguments` `x:Static` マークアップ拡張機能を持つ属性を使用して、オブジェクトをウィジェットのコンストラクターに渡す必要があります。 詳細については、「[ネイティブビューへの引数の引き渡し](#pass-arguments-to-native-views)」を参照してください。

> [!NOTE]
> `x:Name`.NET Standard ライブラリプロジェクトまたは共有アセットプロジェクト (SAP) でネイティブビューに名前を付けることはできません。 これにより、ネイティブ型の変数が生成され、コンパイルエラーが発生します。 ただし、SAP が使用されている場合は、ネイティブビューをインスタンスにラップ `ContentView` し、分離コードファイルで取得できます。 詳細については、「[コードからのネイティブビューの参照」を](#refer-to-native-views-from-code)参照してください。

## <a name="native-bindings"></a>ネイティブバインド

データバインディングは、UI とそのデータソースを同期するために使用され、 Xamarin.Forms アプリケーションがそのデータを表示および操作する方法を簡略化します。 ソースオブジェクトがインターフェイスを実装している `INotifyPropertyChanged` 場合、*ソース*オブジェクトの変更は、バインディングフレームワークによって*ターゲット*オブジェクトに自動的にプッシュされます。また、必要に応じて、*ターゲット*オブジェクトの変更を*ソース*オブジェクトにプッシュすることもできます。

ネイティブビューのプロパティでは、データバインディングを使用することもできます。 次のコード例は、ネイティブビューのプロパティを使用したデータバインディングを示しています。

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

ページには、プロパティがプロパティにバインドされているが含まれてい [`Entry`](xref:Xamarin.Forms.Entry) [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) `NativeSwitchPageViewModel.IsSwitchOn` ます。 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)ページのは、分離コードファイル内のクラスの新しいインスタンスに設定され `NativeSwitchPageViewModel` 、インターフェイスを実装するビューモデルクラスを使用し `INotifyPropertyChanged` ます。

このページには、各プラットフォームのネイティブスイッチも含まれています。 各ネイティブスイッチは、バインディングを使用して [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) プロパティの値を更新し `NativeSwitchPageViewModel.IsSwitchOn` ます。 このため、スイッチがオフの場合、は無効になり、 `Entry` スイッチがオンの場合は `Entry` が有効になります。 次のスクリーンショットは、各プラットフォームでのこの機能を示しています。

![](xaml-images/native-switch-disabled.png "Native Switch Disabled")
![](xaml-images/native-switch-enabled.png "Native Switch Enabled")

双方向のバインディングは、ネイティブプロパティがを実装している `INotifyPropertyChanged` 場合、または iOS でのキー値の観察 (KVO) をサポートしている場合、またはが UWP の場合に、自動的にサポートされ `DependencyProperty` ます。 ただし、多くのネイティブビューでは、プロパティの変更通知はサポートされていません。 これらのビューでは、 [`UpdateSourceEventName`](xref:Xamarin.Forms.Binding.UpdateSourceEventName) バインド式の一部としてプロパティ値を指定できます。 このプロパティは、ターゲットプロパティが変更されたときに通知するネイティブビューのイベントの名前に設定する必要があります。 次に、ネイティブスイッチの値が変更されると、ユーザーがスイッチの値を変更した `Binding` ことが通知され、 `NativeSwitchPageViewModel.IsSwitchOn` プロパティの値が更新されます。

## <a name="pass-arguments-to-native-views"></a>ネイティブビューに引数を渡す

コンストラクター引数は、 `x:Arguments` マークアップ拡張機能を持つ属性を使用してネイティブビューに渡すことができ `x:Static` ます。 また、ネイティブビューファクトリメソッド (メソッドを `public static` 定義するクラスまたは構造体と同じ型のオブジェクトまたは値を返すメソッド) を呼び出すこともできます。これを行うには、属性を使用してメソッドの名前を指定し、属性を使用して引数を指定し `x:FactoryMethod` `x:Arguments` ます。

次のコード例は、両方の手法を示しています。

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

ファクトリメソッドを使用して、 [`UIFont.FromName`](xref:UIKit.UIFont.FromName*) [`UILabel.Font`](xref:UIKit.UILabel.Font) iOS の新しいにプロパティを設定し [`UIFont`](xref:UIKit.UIFont) ます。 `UIFont`名前とサイズは、属性の子であるメソッド引数によって指定され `x:Arguments` ます。

ファクトリメソッドを使用して、 [`Typeface.Create`](xref:Android.Graphics.Typeface.Create*) [`TextView.Typeface`](xref:Android.Widget.TextView.Typeface) Android 上の新しいにプロパティを設定し [`Typeface`](xref:Android.Graphics.Typeface) ます。 `Typeface`ファミリ名とスタイルは、属性の子であるメソッド引数によって指定され `x:Arguments` ます。

[`FontFamily`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.fontfamily)コンストラクターは、 [`TextBlock.FontFamily`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.fontfamily) プロパティを `FontFamily` ユニバーサル Windows プラットフォーム (UWP) の新しいに設定するために使用されます。 名前は、 `FontFamily` 属性の子であるメソッド引数によって指定され `x:Arguments` ます。

> [!NOTE]
> 引数は、コンストラクターまたはファクトリメソッドで必要な型と一致する必要があります。

次のスクリーンショットは、ファクトリメソッドとコンストラクターの引数を指定してさまざまなネイティブビューにフォントを設定した結果を示しています。

![](xaml-images/passing-arguments.png "Setting Fonts on Native Views")

XAML で引数を渡す方法の詳細については、「 [xaml で引数を渡す](~/xamarin-forms/xaml/passing-arguments.md)」を参照してください。

## <a name="refer-to-native-views-from-code"></a>コードからのネイティブビューの参照

ネイティブビューに属性を指定することはできません `x:Name` が、ネイティブビューが [`ContentView`](xref:Xamarin.Forms.ContentView) 属性値を指定するの子である場合は、共有アクセスプロジェクトの分離コードファイルから、XAML ファイルで宣言されたネイティブビューインスタンスを取得でき `x:Name` ます。 次に、分離コードファイル内の条件付きコンパイルディレクティブ内で、次のことを行う必要があります。

1. [`ContentView.Content`](xref:Xamarin.Forms.ContentView.Content)プロパティ値を取得し、それをプラットフォーム固有の型にキャストし `NativeViewWrapper` ます。
1. プロパティを取得 `NativeViewWrapper.NativeElement` し、それをネイティブビュー型にキャストします。

ネイティブ API は、ネイティブビューで呼び出すことによって、必要な操作を実行できます。 また、このアプローチでは、異なるプラットフォームの複数の XAML ネイティブビューを同じにすることができるという利点もあり [`ContentView`](xref:Xamarin.Forms.ContentView) ます。 次のコード例は、この手法を示しています。

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

上の例では、各プラットフォームのネイティブビューがコントロールの子であり、 [`ContentView`](xref:Xamarin.Forms.ContentView) `x:Name` コードビハインドでを取得するために属性値が使用されてい `ContentView` ます。

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

[`ContentView.Content`](xref:Xamarin.Forms.ContentView.Content)ラップされたネイティブビューをプラットフォーム固有のインスタンスとして取得するために、プロパティにアクセスし `NativeViewWrapper` ます。 `NativeViewWrapper.NativeElement`次に、ネイティブな型としてネイティブビューを取得するために、プロパティにアクセスします。 その後、ネイティブビューの API が呼び出され、必要な操作が実行されます。

IOS と Android のネイティブボタンは、 `OnButtonTap` 各ネイティブボタンが `EventHandler` タッチイベントに応答してデリゲートを使用するため、同じイベントハンドラーを共有します。 ただし、ユニバーサル Windows プラットフォーム (UWP) は別のを使用し、 `RoutedEventHandler` `OnButtonTap` この例ではイベントハンドラーを使用します。 したがって、ネイティブボタンをクリックすると、 `OnButtonTap` イベントハンドラーが実行され、名前付きに含まれるネイティブコントロールがスケールおよび回転され [`ContentView`](xref:Xamarin.Forms.ContentView) `contentViewTextParent` ます。 次のスクリーンショットは、各プラットフォームで発生するようすを示しています。

![](xaml-images/contentview.png "ContentView Containing a Native Control")

## <a name="subclass-native-views"></a>サブクラスのネイティブビュー

多くの iOS および Android のネイティブビューは、コントロールを設定するために、プロパティではなくメソッドを使用するため、XAML でのインスタンス化には適していません。 この問題の解決策は、ラッパーでネイティブビューをサブクラス化することです。これは、プロパティを使用してコントロールを設定し、プラットフォームに依存しないイベントを使用する、より XAML に適した API を定義します。 ラップされたネイティブビューは、共有アセットプロジェクト (SAP) に配置し、条件付きコンパイルディレクティブで囲むか、プラットフォーム固有のプロジェクトに配置し、.NET Standard ライブラリプロジェクトの XAML から参照できます。

次のコード例は、サブクラスのネイティブビューを使用するページを示してい Xamarin.Forms ます。

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

このページには、 [`Label`](xref:Xamarin.Forms.Label) ユーザーがネイティブコントロールから選択した果物を表示するが含まれています。 は、 `Label` プロパティにバインドされ `SubclassedNativeControlsPageViewModel.SelectedFruit` ます。 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)ページのは、分離コードファイル内のクラスの新しいインスタンスに設定され `SubclassedNativeControlsPageViewModel` 、インターフェイスを実装するビューモデルクラスを使用し `INotifyPropertyChanged` ます。

このページには、各プラットフォームのネイティブピッカービューも含まれています。 各ネイティブビューには、そのプロパティをコレクションにバインドすることによって、果物のコレクションが表示され `ItemSource` `SubclassedNativeControlsPageViewModel.Fruits` ます。 これにより、次のスクリーンショットに示すように、ユーザーは果物を選択できます。

![](xaml-images/sub-classed.png "Subclassed Native Views")

IOS と Android では、ネイティブピッカーはメソッドを使用してコントロールを設定します。 そのため、これらのピッカーは、XAML をわかりやすくするためにプロパティを公開するためにサブクラス化する必要があります。 ユニバーサル Windows プラットフォーム (UWP) では、は `ComboBox` 既に XAML フレンドリであるため、サブクラス化は必要ありません。

### <a name="ios"></a>iOS

IOS 実装では、ビューがサブクラス化され、 [`UIPickerView`](xref:UIKit.UIPickerView) XAML から簡単に使用できるプロパティとイベントが公開されます。

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

`MyUIPickerView`クラスは、 `ItemsSource` `SelectedItem` プロパティとプロパティ、およびイベントを公開し `SelectedItemChanged` ます。 には、 [`UIPickerView`](xref:UIKit.UIPickerView) [`UIPickerViewModel`](xref:UIKit.UIPickerViewModel) `MyUIPickerView` プロパティとイベントによってアクセスされる、基になるデータモデルが必要です。 `UIPickerViewModel`データモデルは、クラスによって提供され `PickerModel` ます。

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

クラスは、プロパティを使用して、 `PickerModel` クラスの基になるストレージを提供し `MyUIPickerView` `Items` ます。 で選択された項目が変更されるたびに、 `MyUIPickerView` [`Selected`](xref:UIKit.UIPickerViewModel.Selected*) メソッドが実行され、選択したインデックスが更新され、イベントが発生し `ItemChanged` ます。 これにより、 `SelectedItem` プロパティは常にユーザーが選択した最後の項目を返します。 また、クラスは、 `PickerModel` インスタンスのセットアップに使用されるメソッドをオーバーライドし `MyUIPickerView` ます。

### <a name="android"></a>Android

Android 実装はビューをサブクラス化し、 [`Spinner`](xref:Android.Widget.Spinner) XAML から簡単に使用できるプロパティとイベントを公開します。

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

`MySpinner`クラスは、 `ItemsSource` `SelectedObject` プロパティとプロパティ、およびイベントを公開し `ItemSelected` ます。 クラスによって表示される項目 `MySpinner` は、ビューに関連付けられているによって提供され [`Adapter`](xref:Android.Widget.Adapter) `Adapter` ます。また、プロパティが最初に設定されたときに、項目がに設定され `ItemsSource` ます。 クラスで選択された項目が変更されるたび `MySpinner` に、イベントハンドラーによって `OnBindableSpinnerItemSelected` プロパティが更新され `SelectedObject` ます。

## <a name="related-links"></a>関連リンク

- [NativeSwitch (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeswitch)
- [Forms2Native (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/forms2native)
- [NativeViewInsideContentView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeviewinsidecontentview)
- [SubclassedNativeControls (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-subclassednativecontrols)
- [ネイティブ フォーム](~/xamarin-forms/platform/native-forms.md)
- [XAML での引数の受け渡し](~/xamarin-forms/xaml/passing-arguments.md)
