---
title: XAML でのネイティブのビュー
description: IOS、Android、およびユニバーサル Windows プラットフォームのネイティブのビューは、Xamarin.Forms の XAML ファイルから直接参照できます。 プロパティとイベント ハンドラーは、ネイティブのビューを設定でき、Xamarin.Forms のビューとやり取りすることができます。 この記事では、Xamarin.Forms の XAML ファイルからのネイティブのビューを使用する方法を示します。
ms.prod: xamarin
ms.assetid: 7A856D31-B300-409E-9AEB-F8A4DB99B37E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/24/2016
ms.openlocfilehash: 6dbad7352a089f482fa3a396505507da58771cef
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="native-views-in-xaml"></a>XAML でのネイティブのビュー

_IOS、Android、およびユニバーサル Windows プラットフォームのネイティブのビューは、Xamarin.Forms の XAML ファイルから直接参照できます。プロパティとイベント ハンドラーは、ネイティブのビューを設定でき、Xamarin.Forms のビューとやり取りすることができます。この記事では、Xamarin.Forms の XAML ファイルからのネイティブのビューを使用する方法を示します。_

この記事では、次のトピックについて説明します。

- [ネイティブのビューを消費して](#consuming)– XAML からのネイティブのビューを使用するためのプロセスです。
- [ネイティブのバインドを使用して](#native_bindings)– データからネイティブのビューのプロパティをバインドします。
- [ネイティブのビューに引数を渡す](#passing_arguments)– ネイティブ ビュー コンス トラクターに引数を渡すと、ネイティブのビューのファクトリ メソッドを呼び出すことです。
- [コードからネイティブのビューを参照する](#native_view_code)– その分離コード ファイルからの XAML ファイルで宣言されているネイティブ ビューのインスタンスを取得します。
- [ネイティブのビューをサブクラス化](#subclassing)– XAML フレンドリな API の定義にネイティブのビューをサブクラス化します。  

<a name="overview" />

## <a name="overview"></a>概要

Xamarin.Forms の XAML ファイルへのネイティブのビューを埋め込むには。

1. 追加、`xmlns`ネイティブ ビューを含む名前空間の XAML ファイルの名前空間宣言します。
1. XAML ファイルでは、ネイティブのビューのインスタンスを作成します。

> [!NOTE]
> ネイティブ ビューを使用している任意の XAML ページ、XAMLC をオフにする必要があります。

分離コード ファイルからネイティブのビューを参照するには共有アセット プロジェクト (SAP) を使用して条件付きコンパイル ディレクティブを使用してプラットフォーム固有のコードをラップします。 詳細については、次を参照してください。[コードからネイティブのビューを参照する](#native_view_code)です。

<a name="consuming" />

## <a name="consuming-native-views"></a>ネイティブのビューを使用

次のコード例は、Xamarin.Forms の各プラットフォームのネイティブのビューの使用を示します[ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/):

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

指定するだけでなく、`clr-namespace`と`assembly`ネイティブ ビューの名前空間で、`targetPlatform`も指定する必要があります。 これは、値のいずれかに設定する必要があります、 [ `TargetPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetPlatform/)列挙型、通常に設定して`iOS`、 `Android`、または`Windows`です。 実行時に、XAML パーサーが XML 名前空間プレフィックスを持つを無視する`targetPlatform`アプリケーションが実行されているプラットフォームと一致しません。

指定した名前空間、クラスまたは構造体を参照する各名前空間宣言を使用できます。 たとえば、 `ios` iOS から任意のクラスまたは構造体を参照する名前空間宣言を使用可能`UIKit`名前空間。 ネイティブのビューのプロパティは、XAML で設定できますが、プロパティとオブジェクトの種類が一致する必要があります。 たとえば、`UILabel.TextColor`プロパティに設定されている`UIColor.Red`を使用して、`x:Static`マークアップ拡張機能と`ios`名前空間。

バインド可能なプロパティおよび接続されているバインド可能なプロパティも設定できますネイティブ ビューを使用して、`Class.BindableProperty="value"`構文です。 各ネイティブのビューは、プラットフォーム固有にラップされて`NativeViewWrapper`から派生するインスタンス、 [ `Xamarin.Forms.View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)クラスです。 ラッパーにプロパティの値を転送するネイティブのビューにバインド可能なプロパティまたは接続されているバインド可能なプロパティを設定します。 中央の水平レイアウトを指定して、設定など、`View.HorizontalOptions="Center"`ネイティブ ビューにします。

> [!NOTE]
> スタイルできますのみでバックアップされているプロパティをターゲットにするために、ネイティブのビューがあるスタイルを使用することはできません注意してください`BindableProperty`オブジェクト。

Android のウィジェットのコンス トラクターは通常、Android を必要と`Context`オブジェクトでは、引数が、これが利用できる静的プロパティを通じて、`MainActivity`クラスです。 したがって、XAML では、Android のウィジェットを作成するときに、`Context`オブジェクトは、ウィジェットのコンス トラクターに渡すことが一般的を使用して、`x:Arguments`属性が、`x:Static`マークアップ拡張機能です。 詳細については、次を参照してください。[をネイティブのビューに渡す引数](#passing_arguments)です。

> [!NOTE]
> ネイティブのビューの名前を付けることに注意してください`x:Name`ポータブル クラス ライブラリ (PCL) プロジェクトまたは共有アセット プロジェクト (SAP) では不可能です。 これにより、コンパイル エラーの原因は、ネイティブ型の変数が生成されます。 ただし、ネイティブのビューにラップできます`ContentView`インスタンスおよび SAP が使用されていること、分離コード ファイルで取得します。 詳細については、次を参照してください。[コードからネイティブのビューを参照する](#native_view_code)です。

<a name="native_bindings" />

## <a name="native-bindings"></a>ネイティブのバインド

データのバインドは、UI をそのデータ ソースと同期するために使用して、Xamarin.Forms アプリケーションの表示し、そのデータをやり取りする簡素化します。 ソース オブジェクトが実装されている、`INotifyPropertyChanged`インターフェイスでの変更、*ソース*オブジェクトに自動的にプッシュする、*ターゲット*オブジェクト、バインディングのフレームワーク、および、の変更*ターゲット*オブジェクトにプッシュできます必要に応じて、*ソース*オブジェクト。

ネイティブのビューのプロパティは、データ バインディングにも使用できます。 次のコード例では、ネイティブのビューのプロパティを使用してデータ バインディングを示します。

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

このページには、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)が[ `IsEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/)プロパティにバインドされて、`NativeSwitchPageViewModel.IsSwitchOn`プロパティです。 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)ページの新しいインスタンスに設定されて、 `NativeSwitchPageViewModel` ViewModel クラスの実装に、分離コード ファイル内のクラス、`INotifyPropertyChanged`インターフェイスです。

ページには、各プラットフォームのネイティブのスイッチも含まれています。 各ネイティブのスイッチを使用して、 [ `TwoWay` ](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/)の値を更新するバインディング、`NativeSwitchPageViewModel.IsSwitchOn`プロパティです。 したがって、ときに、スイッチがオフ、`Entry`は無効になりますがある場合、スイッチ、および、`Entry`を有効にします。 次のスクリーン ショットは、各プラットフォームでこの機能を示します。

![](xaml-images/native-switch-disabled.png "ネイティブのスイッチを無効化")
![](xaml-images/native-switch-enabled.png "ネイティブ スイッチが有効になっています。")

ネイティブのプロパティを実装することは双方向のバインディングが自動的にサポート`INotifyPropertyChanged`、ios、キーと値を観察し (KVO) をサポートしているか、 `DependencyProperty` UWP にします。 ただし、ネイティブの複数のビューには、プロパティの変更通知をサポートしません。 これらのビューを指定できます、 [ `UpdateSourceEventName` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.UpdateSourceEventName/)バインド式の一部としてプロパティ値。 このプロパティは、対象となるプロパティが変更されたときに通知するネイティブ ビュー内のイベントの名前に設定する必要があります。 次に、ネイティブ スイッチの値が変更されたとき、`Binding`クラスはユーザーのスイッチの値が変更されたことを通知し、`NativeSwitchPageViewModel.IsSwitchOn`プロパティの値を更新します。

<a name="passing_arguments" />

## <a name="passing-arguments-to-native-views"></a>ネイティブのビューに引数を渡す

コンス トラクターの引数を使用してネイティブのビューに渡すことができます、`x:Arguments`属性が、`x:Static`マークアップ拡張機能です。 さらに、ネイティブのビューのファクトリ メソッド (`public static`オブジェクトまたはクラスまたはメソッドを定義する構造体と同じ型の値を返すメソッドを)、メソッドを指定して呼び出すことができる名前を使用して、`x:FactoryMethod`属性、およびその引数使用して、`x:Arguments`属性。

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

[ `UIFont.FromName` ](https://developer.xamarin.com/api/member/UIKit.UIFont.FromName/)ファクトリ メソッドを使用して、設定、 [ `UILabel.Font` ](https://developer.xamarin.com/api/property/UIKit.UILabel.Font/)プロパティを新しい[ `UIFont` ](https://developer.xamarin.com/api/type/UIKit.UIFont/) iOS でします。 `UIFont`名およびサイズがの子であるメソッドの引数で指定された、`x:Arguments`属性。

[ `Typeface.Create` ](https://developer.xamarin.com/api/member/Android.Graphics.Typeface.Create/p/System.String/Android.Graphics.TypefaceStyle/)ファクトリ メソッドを使用して、設定、 [ `TextView.Typeface` ](https://developer.xamarin.com/api/property/Android.Widget.TextView.Typeface/)プロパティを新しい[ `Typeface` ](https://developer.xamarin.com/api/type/Android.Graphics.Typeface/) Android でします。 `Typeface`ファミリ名とスタイルの子であるメソッドの引数によって指定されます、`x:Arguments`属性。

[ `FontFamily` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.fontfamily)コンス トラクターは、設定に使用される、 [ `TextBlock.FontFamily` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.fontfamily)プロパティを新しい`FontFamily`ユニバーサル Windows プラットフォーム (UWP) にします。 `FontFamily`の子であるメソッドの引数で指定された名前、`x:Arguments`属性。

> [!NOTE]
> 引数は、コンス トラクターまたはファクトリ メソッドで必要な型に一致する必要があります。

次のスクリーン ショットは、ネイティブの異なるビューのフォントを設定するファクトリ メソッドとコンス トラクター引数を指定する結果を表示します。

![](xaml-images/passing-arguments.png "ネイティブのビューのフォントの設定")

詳細については XAML で引数を渡し、次を参照してください。 [XAML で引数を渡す](~/xamarin-forms/xaml/passing-arguments.md)です。

<a name="native_view_code" />

## <a name="referring-to-native-views-from-code"></a>コードからネイティブのビューを参照します。

ネイティブ ビューの名前を付けることはできませんが、`x:Name`ネイティブ ビューが、の子プロジェクトでは共有のアクセス、分離コードファイルからXAMLファイルで宣言されているインスタンスのネイティブの表示を取得することは、属性、[ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)を指定する、`x:Name`属性の値。 次に、分離コード ファイル内の条件付きコンパイル ディレクティブ内を行ってください。

1. 取得、 [ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/)プロパティ値し、プラットフォーム固有にキャスト`NativeViewWrapper`型です。
1. 取得、`NativeViewWrapper.NativeElement`プロパティであり、ネイティブのビューの種類にキャストします。

ネイティブの API は、目的の操作を実行するネイティブのビューで呼び出すことができます。 このアプローチも利点は、さまざまなプラットフォームの XAML ネイティブの複数のビューが同一の子を指定できます[ `ContentView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)です。 次のコード例では、この手法を示します。

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

上記の例では、各プラットフォームのネイティブのビューの子[ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) 、コントロールで、`x:Name`属性値を取得するために使用されている、`ContentView`分離コード。

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

[ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/)プロパティがプラットフォーム固有の仕様としてラップされたネイティブ ビューを取得するアクセス`NativeViewWrapper`インスタンス。 `NativeViewWrapper.NativeElement`ネイティブ型としてネイティブ ビューを取得するプロパティがアクセスされます。 ネイティブ ビューの API は、目的の操作を実行し、呼び出されます。

IOS および Android のネイティブ ボタンが同じ`OnButtonTap`イベント ハンドラーでは、各ネイティブのボタンを消費するため、`EventHandler`タッチ イベントに応答を委任します。 ただし、ユニバーサル Windows プラットフォーム (UWP) を使用、個別`RoutedEventHandler`、さらに消費する、`OnButtonTap`この例ではイベント ハンドラー。 したがって、ときに、ネイティブ ボタンをクリックして、`OnButtonTap`イベント ハンドラーを実行するスケールを設定し、内に含まれるネイティブ コントロールを回転、 [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)という`contentViewTextParent`です。 次のスクリーン ショットでは、各プラットフォームで発生しているこのを示しています。

![](xaml-images/contentview.png "ネイティブのコントロールを含む ContentView")

<a name="subclassing" />

## <a name="subclassing-native-views"></a>ネイティブのビューをサブクラス化

多くの iOS と Android のネイティブ ビューは、コントロールを設定するプロパティではなく、メソッドを使用するために、XAML でインスタンス化するのに適したではありません。 この問題を解決では、ネイティブのビューのラッパーのプロパティを使用して、コントロールをセットアップして、プラットフォームに依存しないイベントを使用する複数の XAML フレンドリな API の定義にサブクラスです。 ラップされたネイティブ ビューし共有アセット プロジェクト (SAP) に配置していると、条件付きコンパイル ディレクティブで囲まれまたはプラットフォーム固有のプロジェクトに配置してポータブル クラス ライブラリ (PCL) プロジェクトでは、XAML から参照されています。

次のコード例では、Xamarin.Forms ページを使用するには、ネイティブのビューがサブクラス化を示しています。

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

このページには、 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)ネイティブ コントロールから、ユーザーが選択した成果物を表示します。 `Label`にバインドされて、`SubclassedNativeControlsPageViewModel.SelectedFruit`プロパティです。 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)ページの新しいインスタンスに設定されて、 `SubclassedNativeControlsPageViewModel` ViewModel クラスの実装に、分離コード ファイル内のクラス、`INotifyPropertyChanged`インターフェイスです。

ページには、各プラットフォームのネイティブ ピッカー ビューも含まれています。 各ネイティブのビューがバインドして果物のコレクションを表示、`ItemSource`プロパティを`SubclassedNativeControlsPageViewModel.Fruits`コレクション。 これにより、次のスクリーン ショットに示すように、成果物を取得するユーザー。

![](xaml-images/sub-classed.png "サブクラス ネイティブ ビュー")

IOS および Android では、ネイティブ ピッカーは、コントロールを設定するのにメソッドを使用します。 そのため、これらの選択は、XAML フレンドリなようにプロパティを公開するサブクラス化する必要があります。 ユニバーサル Windows プラットフォーム (UWP) に、 `ComboBox` XAML フレンドリなであるためを必要し、しないサブクラス化します。

### <a name="ios"></a>iOS

IOS 実装サブクラス、 [ `UIPickerView` ](https://developer.xamarin.com/api/type/UIKit.UIPickerView/)ビュー、およびプロパティを公開し、XAML から容易に利用できるイベント。

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

`MyUIPickerView`クラスが公開`ItemsSource`と`SelectedItem`プロパティ、および`SelectedItemChanged`イベント。 A [ `UIPickerView` ](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) 、基になる必要があります[ `UIPickerViewModel` ](https://developer.xamarin.com/api/type/UIKit.UIPickerViewModel/)によってアクセスされるデータ モデル、`MyUIPickerView`プロパティとイベント。 `UIPickerViewModel`データ モデルによって提供されます、`PickerModel`クラス。

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

`PickerModel`クラスを基になる記憶域を提供する、`MyUIPickerView`クラスを使用して、`Items`プロパティです。 たびにで選択された項目、 `MyUIPickerView` 、変更、 [ `Selected` ](https://developer.xamarin.com/api/member/UIKit.UIPickerViewModel.Selected/)メソッドを実行すると、更新される選択されたインデックスと起動、`ItemChanged`イベント。 これにより、`SelectedItem`プロパティは常に、ユーザーによって取得された最後の項目を返します。 さらに、`PickerModel`をセットアップするために使用するメソッドを上書きするクラス、`MyUIPickerView`インスタンス。

### <a name="android"></a>Android

Android 実装サブクラス、 [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/)ビュー、およびプロパティを公開し、XAML から容易に利用できるイベント。

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

`MySpinner`クラスが公開`ItemsSource`と`SelectedObject`プロパティ、および`ItemSelected`イベント。 によって表示される項目、`MySpinner`クラスは、によって提供される、 [ `Adapter` ](https://developer.xamarin.com/api/type/Android.Widget.Adapter/) 、ビューに関連付けられているし、アイテムに設定されます、`Adapter`ときに、`ItemsSource`プロパティを最初に設定します。 たびにで選択された項目、`MySpinner`クラスの変更、`OnBindableSpinnerItemSelected`イベント ハンドラーの更新プログラム、`SelectedObject`プロパティです。

## <a name="summary"></a>まとめ

この記事では、Xamarin.Forms の XAML ファイルからのネイティブのビューを使用する方法を示しました。 プロパティとイベント ハンドラーは、ネイティブのビューを設定でき、Xamarin.Forms のビューとやり取りすることができます。


## <a name="related-links"></a>関連リンク

- [NativeSwitch (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/NativeSwitch/)
- [Forms2Native (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Forms2Native/)
- [NativeViewInsideContentView (sample)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/NativeViewInsideContentView/)
- [SubclassedNativeControls (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/SubclassedNativeControls/)
- [ネイティブ フォーム](~/xamarin-forms/platform/native-forms.md)
- [XAML で引数の受け渡し](~/xamarin-forms/xaml/passing-arguments.md)
