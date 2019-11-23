---
title: XAML のネイティブ ビュー
description: IOS、Android、およびユニバーサル Windows プラットフォームからのネイティブ ビューは、Xamarin.Forms XAML ファイルから直接参照できます。 プロパティとイベント ハンドラーは、ネイティブのビューを設定でき、Xamarin.Forms のビューとやり取りすることができます。 この記事では、Xamarin.Forms XAML ファイルからのネイティブ ビューを使用する方法を示します。
ms.prod: xamarin
ms.assetid: 7A856D31-B300-409E-9AEB-F8A4DB99B37E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/24/2016
ms.openlocfilehash: 3c4fa085c9fdf17cdc256d9710c23911bb60d584
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70770642"
---
# <a name="native-views-in-xaml"></a>XAML のネイティブ ビュー

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeswitch)

_IOS、Android、およびユニバーサル Windows プラットフォームからのネイティブビューは、Xamarin の XAML ファイルから直接参照できます。プロパティとイベントハンドラーは、ネイティブビューで設定でき、Xamarin のビューと対話できます。この記事では、Xamarin の XAML ファイルからネイティブビューを使用する方法について説明します。_

この記事では、次のトピックについて説明します。

- [ネイティブ ビューの使用](#consuming)– XAML からのネイティブ ビューを使用するためのプロセス。
- [ネイティブのバインドを使用して](#native_bindings)– データとの間のネイティブ ビューのプロパティをバインドします。
- [ネイティブ ビューに引数を渡す](#passing_arguments)– ネイティブ ビュー コンス トラクターに引数を渡すと、ネイティブのビュー ファクトリ メソッドを呼び出すことです。
- [コードからネイティブ ビューを参照する](#native_view_code)– その分離コード ファイルからの XAML ファイルで宣言されているネイティブ ビューのインスタンスを取得します。
- [ネイティブ ビューのサブクラス化](#subclassing)– XAML 使いやすい API を定義するネイティブ ビューのサブクラス化します。  

<a name="overview" />

## <a name="overview"></a>概要

Xamarin.Forms XAML ファイルにネイティブのビューを埋め込むには。

1. 追加、`xmlns`ネイティブ ビューを含む名前空間の XAML ファイルの名前空間宣言。
1. XAML ファイルでのネイティブ ビューのインスタンスを作成します。

> [!IMPORTANT]
> ネイティブビューを使用するすべての XAML ページでは、コンパイルされた XAML を無効にする必要があります。 これは、XAML ページの分離コードクラスを `[XamlCompilation(XamlCompilationOptions.Skip)]` 属性で修飾することによって実現できます。 XAML コンパイルの詳細については、「 [Xamarin. Forms での xaml のコンパイル](~/xamarin-forms/xaml/xamlc.md)」を参照してください。

分離コード ファイルからのネイティブ ビューを参照するには、共有資産プロジェクト (SAP) を使用して、条件付きコンパイル ディレクティブを使用してプラットフォーム固有のコードをラップする必要があります。 詳細については、次を参照してください。[コードからネイティブ ビューを参照する](#native_view_code)します。

<a name="consuming" />

## <a name="consuming-native-views"></a>ネイティブ ビューの使用

次のコード例は、Xamarin.Forms の各プラットフォームのネイティブ ビューの使用を示します[ `ContentPage` ](xref:Xamarin.Forms.ContentPage):

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

指定すると、`clr-namespace`と`assembly`ネイティブ ビューの名前空間で、`targetPlatform`も指定する必要があります。 これは、値のいずれかに設定する必要があります、 [ `TargetPlatform` ](xref:Xamarin.Forms.TargetPlatform)列挙体を通常に設定して`iOS`、 `Android`、または`Windows`します。 XAML パーサーが XML 名前空間プレフィックスを持つを無視、実行時に、`targetPlatform`アプリケーションが実行されているプラットフォームと一致しません。

指定した名前空間から任意のクラスまたは構造体を参照する各名前空間宣言を使用できます。 たとえば、`ios`名前空間の宣言は、iOS から任意のクラスまたは構造体を参照するために使用できます`UIKit`名前空間。 ネイティブ ビューのプロパティは、XAML で設定できますが、プロパティとオブジェクトの型が一致する必要があります。 たとえば、`UILabel.TextColor`プロパティに設定されて`UIColor.Red`を使用して、`x:Static`マークアップ拡張機能と`ios`名前空間。

バインド可能なプロパティおよび添付のバインド可能なプロパティも設定できますのネイティブ ビューを使用して、`Class.BindableProperty="value"`構文。 それぞれのネイティブ ビューは、プラットフォーム固有にラップ`NativeViewWrapper`から派生したインスタンス、 [ `Xamarin.Forms.View` ](xref:Xamarin.Forms.View)クラス。 ラッパーにプロパティの値を転送するネイティブのビューにバインド可能なプロパティまたは添付のバインド可能なプロパティを設定します。 中央揃えの水平レイアウトを指定を設定するなど、`View.HorizontalOptions="Center"`ネイティブ ビュー。

> [!NOTE]
> スタイルのみ対象にできるためでは、提供されるプロパティは、ネイティブのビューのスタイルを使用できないことに注意してください`BindableProperty`オブジェクト。

Android のウィジェットのコンス トラクターは通常、Android を必要と`Context`オブジェクト引数、およびこれが利用できる静的プロパティを通じて、`MainActivity`クラス。 そのため、XAML で Android のウィジェットを作成するときに、`Context`オブジェクトは、ウィジェットのコンス トラクターに渡す一般的にする必要がありますを使用して、`x:Arguments`属性、`x:Static`マークアップ拡張機能。 詳細については、次を参照してください。[をネイティブのビューに渡す引数](#passing_arguments)します。

> [!NOTE]
> ネイティブ ビューの名前を付けることに注意してください。 `x:Name` .NET Standard ライブラリ プロジェクトまたは共有資産プロジェクト (SAP) のいずれかのことはできません。 そうと、コンパイル エラーの原因は、ネイティブの型の変数が生成されます。 ただし、ネイティブのビューにラップできます`ContentView`インスタンスし、SAP が使用されていること、分離コード ファイルで取得します。 詳細については、次を参照してください。[コードからネイティブのビューを参照する](#native_view_code)します。

<a name="native_bindings" />

## <a name="native-bindings"></a>ネイティブのバインド

データ バインディングは、そのデータ ソースと UI を同期するために使用し、Xamarin.Forms アプリケーションの表示し、そのデータをやり取りするを合理化します。 ソース オブジェクトが実装されている、`INotifyPropertyChanged`インターフェイスでの変更、*ソース*オブジェクトに自動的にプッシュする、*ターゲット*バインディング フレームワーク、および、の変更によってオブジェクト*ターゲット*にオブジェクトをプッシュできます必要に応じて、*ソース*オブジェクト。

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

このページには、 [ `Entry` ](xref:Xamarin.Forms.Entry)が[ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled)プロパティにバインドされて、`NativeSwitchPageViewModel.IsSwitchOn`プロパティ。 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)のページの新しいインスタンスに設定されて、 `NativeSwitchPageViewModel` 、ViewModel クラスを実装すると、分離コード ファイル内のクラス、`INotifyPropertyChanged`インターフェイス。

ページには、各プラットフォームのネイティブのスイッチも含まれています。 各ネイティブ スイッチを使用して、 [ `TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay)バインディングの値を更新する、`NativeSwitchPageViewModel.IsSwitchOn`プロパティ。 スイッチがオフがときにそのため、`Entry`は無効になりますがある場合、スイッチ、および、`Entry`を有効にします。 次のスクリーン ショットは、各プラットフォームでこの機能を示します。

![](xaml-images/native-switch-disabled.png "ネイティブのスイッチを無効になっている")
![](xaml-images/native-switch-enabled.png "ネイティブ スイッチが有効になっています。")

ネイティブのプロパティが実装されている、双方向のバインディングは自動的にサポートされている`INotifyPropertyChanged`、または ios では、キーと値を観察し (KVO) をサポートしていますが、 `DependencyProperty` UWP の。 ただし、多くのネイティブ ビューでは、プロパティの変更通知をサポートしません。 これらのビューを指定できます、 [ `UpdateSourceEventName` ](xref:Xamarin.Forms.Binding.UpdateSourceEventName)バインディング式の一部としてプロパティ値。 このプロパティは、ターゲット プロパティが変更されたときに通知するネイティブ ビュー内のイベントの名前に設定する必要があります。 次に、ネイティブ スイッチの値が変更されたとき、`Binding`クラスに、ユーザーのスイッチの値が変更されたことを通知し、`NativeSwitchPageViewModel.IsSwitchOn`プロパティの値を更新します。

<a name="passing_arguments" />

## <a name="passing-arguments-to-native-views"></a>ネイティブ ビューに引数を渡す

コンス トラクターの引数を使用してネイティブのビューに渡すことができます、`x:Arguments`属性、`x:Static`マークアップ拡張機能。 さらに、ファクトリ メソッドのネイティブ ビュー (`public static`オブジェクトまたはクラスまたはメソッドを定義する構造体と同じ型の値を返すメソッドを)、メソッドの指定することによって呼び出すことが名前を使用して、`x:FactoryMethod`属性、およびその引数使用して、`x:Arguments`属性。

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

[ `UIFont.FromName` ](xref:UIKit.UIFont.FromName*)ファクトリ メソッドを設定するため、 [ `UILabel.Font` ](xref:UIKit.UILabel.Font)プロパティを新しい[ `UIFont` ](xref:UIKit.UIFont) iOS でします。 `UIFont`名とサイズの子であるメソッドの引数で指定された、`x:Arguments`属性。

[ `Typeface.Create` ](xref:Android.Graphics.Typeface.Create*)ファクトリ メソッドを設定するため、 [ `TextView.Typeface` ](xref:Android.Widget.TextView.Typeface)プロパティを新しい[ `Typeface` ](xref:Android.Graphics.Typeface) Android で。 `Typeface`ファミリ名とスタイルの子であるメソッドの引数によって指定されます、`x:Arguments`属性。

[ `FontFamily` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.fontfamily)コンス トラクターは、設定に使用される、 [ `TextBlock.FontFamily` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.fontfamily)プロパティを新しい`FontFamily`ユニバーサル Windows プラットフォーム (UWP) にします。 `FontFamily`の子であるメソッドの引数で指定された名前、`x:Arguments`属性。

> [!NOTE]
> 引数は、コンス トラクターまたはファクトリ メソッドで必要な型と一致する必要があります。

次のスクリーン ショットでは、別のネイティブ ビューのフォントを設定するファクトリ メソッドとコンス トラクターの引数を指定して結果を表示します。

![](xaml-images/passing-arguments.png "ネイティブ ビューのフォントの設定")

XAML で引数の受け渡しの詳細については、次を参照してください。 [XAML で引数を渡す](~/xamarin-forms/xaml/passing-arguments.md)します。

<a name="native_view_code" />

## <a name="referring-to-native-views-from-code"></a>コードからネイティブ ビューを参照します。

ネイティブ ビューの名前を付けることはできませんが、`x:Name`ネイティブ ビューが、の子へのアクセス、共有プロジェクトでは、その分離コードファイルからXAMLファイルで宣言されているネイティブビューのインスタンスを取得することは、属性、[ `ContentView` ](xref:Xamarin.Forms.ContentView)を指定する、`x:Name`属性の値。 次に、分離コード ファイルで条件付きコンパイル ディレクティブ内で行う必要があります。

1. 取得、 [ `ContentView.Content` ](xref:Xamarin.Forms.ContentView.Content)プロパティ値し、プラットフォーム固有にキャスト`NativeViewWrapper`型。
1. 取得、`NativeViewWrapper.NativeElement`プロパティとビューのネイティブ型にキャストします。

ネイティブの API は、目的の操作を実行するネイティブのビューを起動できます。 このアプローチも利点がさまざまなプラットフォーム向け XAML ネイティブの複数のビューは、同じの子であること[ `ContentView`](xref:Xamarin.Forms.ContentView)します。 次のコード例では、この手法を示します。

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

上記の例での子である各プラットフォームのネイティブ ビュー [ `ContentView` ](xref:Xamarin.Forms.ContentView)コントロールで、`x:Name`属性の値を取得するために使用される、`ContentView`分離コードで。

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

[ `ContentView.Content` ](xref:Xamarin.Forms.ContentView.Content)をプラットフォーム固有のラップされたネイティブ ビューを取得するプロパティにアクセス`NativeViewWrapper`インスタンス。 `NativeViewWrapper.NativeElement`ネイティブ型としてネイティブ ビューを取得するプロパティにアクセスします。 ネイティブ ビューの API は、目的の操作を実行し、呼び出されます。

IOS と Android のネイティブ ボタンが共有して同じ`OnButtonTap`イベント ハンドラーでは、ネイティブの各ボタンが消費されるので、`EventHandler`タッチ イベントに応答を委任します。 ただし、別の使用、ユニバーサル Windows プラットフォーム (UWP) `RoutedEventHandler`、さらに消費する、`OnButtonTap`この例では、イベント ハンドラー。 そのため、ネイティブのボタンがクリックされたとき、`OnButtonTap`イベント ハンドラーを実行するスケールおよびに含まれるネイティブ コントロールを回転する、 [ `ContentView` ](xref:Xamarin.Forms.ContentView)という`contentViewTextParent`します。 次のスクリーン ショットは、各プラットフォームで発生しているこのを示しています。

![](xaml-images/contentview.png "ネイティブ コントロールを含む ContentView")

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

このページには、 [ `Label` ](xref:Xamarin.Forms.Label)ネイティブ コントロールから、ユーザーが選択した成果物を表示します。 `Label`にバインドする、`SubclassedNativeControlsPageViewModel.SelectedFruit`プロパティ。 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)のページの新しいインスタンスに設定されて、 `SubclassedNativeControlsPageViewModel` 、ViewModel クラスを実装すると、分離コード ファイル内のクラス、`INotifyPropertyChanged`インターフェイス。

ページには、各プラットフォームのネイティブ ピッカー ビューも含まれています。 各ネイティブの表示には、バインド、果物のコレクションが表示されます、`ItemSource`プロパティを`SubclassedNativeControlsPageViewModel.Fruits`コレクション。 これにより、次のスクリーン ショットに示すように、ユーザー、フルーツを選択します。

![](xaml-images/sub-classed.png "サブクラス化ネイティブビュー")

IOS と Android では、ネイティブの選択は、コントロールを設定するのにメソッドを使用します。 そのため、これらの選択は、XAML に対応できるようにするプロパティを公開するサブクラス化する必要があります。 ユニバーサル Windows プラットフォーム (UWP) で、`ComboBox`は既に XAML への対応、およびそのため、サブクラス化を必要としません。

### <a name="ios"></a>iOS

IOS の実装サブクラス、 [ `UIPickerView` ](xref:UIKit.UIPickerView)と XAML から簡単に使用できるイベントの表示、およびプロパティを公開します。

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

`MyUIPickerView`クラスでは`ItemsSource`と`SelectedItem`プロパティ、および`SelectedItemChanged`イベント。 A [ `UIPickerView` ](xref:UIKit.UIPickerView)基になる必要があります[ `UIPickerViewModel` ](xref:UIKit.UIPickerViewModel)によってアクセスされるデータ モデル、`MyUIPickerView`プロパティとイベント。 `UIPickerViewModel`データ モデルがによって提供される、`PickerModel`クラス。

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

`PickerModel`クラスを基になるストレージを提供する、`MyUIPickerView`クラスを使用して、`Items`プロパティ。 たびにで選択された項目、 `MyUIPickerView` 、変更、 [ `Selected` ](xref:UIKit.UIPickerViewModel.Selected*)メソッドを実行すると、更新される、選択されたインデックスと起動、`ItemChanged`イベント。 これにより、`SelectedItem`プロパティは常に、ユーザーによって選択された最後の項目を返します。 さらに、`PickerModel`クラスは、セットアップに使用されるメソッドをオーバーライド、`MyUIPickerView`インスタンス。

### <a name="android"></a>Android

Android の実装サブクラス、 [ `Spinner` ](xref:Android.Widget.Spinner)と XAML から簡単に使用できるイベントの表示、およびプロパティを公開します。

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

`MySpinner`クラスでは`ItemsSource`と`SelectedObject`プロパティ、および`ItemSelected`イベント。 によって表示される項目、`MySpinner`クラスは、によって提供される、 [ `Adapter` ](xref:Android.Widget.Adapter) 、ビューに関連付けられたおよびに項目が表示されます、`Adapter`ときに、`ItemsSource`プロパティが最初に設定します。 たびにで選択された項目、`MySpinner`クラスの変更、`OnBindableSpinnerItemSelected`イベント ハンドラーの更新プログラム、`SelectedObject`プロパティ。

## <a name="summary"></a>概要

この記事では、Xamarin.Forms XAML ファイルからのネイティブ ビューを使用する方法を示しました。 プロパティとイベント ハンドラーは、ネイティブのビューを設定でき、Xamarin.Forms のビューとやり取りすることができます。

## <a name="related-links"></a>関連リンク

- [NativeSwitch (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeswitch)
- [Forms2Native (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/forms2native)
- [NativeViewInsideContentView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeviewinsidecontentview)
- [SubclassedNativeControls (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-subclassednativecontrols)
- [ネイティブ フォーム](~/xamarin-forms/platform/native-forms.md)
- [XAML で引数の受け渡し](~/xamarin-forms/xaml/passing-arguments.md)
