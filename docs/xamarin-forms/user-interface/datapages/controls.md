---
title: DataPages コントロールのリファレンス
description: この記事では、Xamarin.Forms DataPages NuGet パッケージで利用できるコントロールについて説明します。
ms.prod: xamarin
ms.assetid: 891615D0-E8BD-4ACC-A7F0-4C3725FBCC31
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: c907d55f09d334e167c831a19f9d0edc4c97732f
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38866523"
---
# <a name="datapages-controls-reference"></a>DataPages コントロールのリファレンス

![](~/media/shared/preview.png "この API は現在プレビュー段階")

> [!IMPORTANT]
> DataPages が必要です、 [Xamarin.Forms テーマ](~/xamarin-forms/user-interface/themes/index.md)レンダリングへの参照。


Xamarin.Forms の DataPages Nuget には、さまざまなデータ ソースのバインドの利用できるコントロールが含まれています。

これらのコントロールを XAML で使用する名前空間が含まれていることを確認、例を参照してください、`xmlns:pages`宣言。

```xaml
<ContentPage
    xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:pages="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
    x:Class="DataPagesDemo.Detail">
```

次の例は、`DynamicResource`参照は操作するプロジェクトのリソース ディクショナリに存在する必要があります。 構築する方法の例はまた、[カスタム コントロール](#custom)

## <a name="built-in-controls"></a>組み込みのコントロール

* [HeroImage](#heroimage)
* [ListItem](#listitem)

<a name="heroimage" />

### <a name="heroimage"></a>HeroImage

`HeroImage`コントロールが 4 つのプロパティ。

* テキスト
* 詳細
* ImageSource
* 側面

```xaml
<pages:HeroImage
    ImageSource="{ DynamicResource HeroImageImage }"
    Text="Keith Ballinger"
    Detail="Xamarin"
/>
```

**Android**

![](controls-images/heroimage-light-android.png "Android で HeroImage コントロール") ![](controls-images/heroimage-dark-android.png "android HeroImage コントロール")

**iOS**

![](controls-images/heroimage-light-ios.png "IOS で HeroImage コントロール") ![](controls-images/heroimage-dark-ios.png "ios HeroImage コントロール")


<a name="listitem" />

### <a name="listitem"></a>ListItem

`ListItem`コントロールのレイアウトはネイティブの iOS に似ており、Android のリストまたはテーブル行、ただしこの属性は通常のビューとしてにも使用できます。 例では、その下のコードが内部でホストされている表示されます、`StackLayout`がデータ バインド scolling リスト コントロールで使用することもできます。

5 つのプロパティがあります。

* Title
* 詳細
* ImageSource
* PlaceholdImageSource
* 側面

```xaml
<StackLayout Spacing="0">
    <pages:ListItemControl
        Detail="Xamarin"
        ImageSource="{ DynamicResource UserImage }"
        Title="Miguel de Icaza"
        PlaceholdImageSource="{ DynamicResource IconImage }"
    />
```

これらのスクリーン ショットを示して、 `ListItem` iOS および Android プラットフォームの両方、ライト テーマと暗いテーマを使用してで。

**Android**

![](controls-images/listitem-light-android.png "Android で ListItem コントロール") ![](controls-images/listitem-dark-android.png "android ListItem コントロール")

**iOS**

![](controls-images/listitem-light-ios.png "IOS で ListItem コントロール") ![](controls-images/listitem-dark-ios.png "ios ListItem コントロール")


## <a name="custom-control-example"></a>カスタム コントロールの例

このカスタムの目的は、`CardView`コントロールはネイティブ Android CardView のようになります。

3 つのプロパティが含まれます。

* テキスト
* 詳細
* ImageSource

目標は、カスタム コントロールを次のコードのようになります (なお、カスタム`xmlns:local`が必要です、現在のアセンブリを参照する)。

```xaml
<local:CardView
  ImageSource="{ DynamicResource CardViewImage }"
  Text="CardView Text"
  Detail="CardView Detail"
/>
```

組み込みのライトとダーク テーマに対応する色を使用して以下のスクリーン ショットのようになります。

**Android**

![](controls-images/cardview-light-android.png "Android で CardView カスタム コントロール") ![](controls-images/cardview-dark-android.png "android CardView カスタム コントロール")

**iOS**

![](controls-images/cardview-light-ios.png "IOS でのカスタム コントロールの CardView") ![](controls-images/cardview-dark-ios.png "ios CardView カスタム コントロール")

<a name="custom" />

### <a name="building-the-custom-cardview"></a>カスタム CardView の構築

1. [DataView のサブクラス](#1)
2. [フォント、レイアウト、および余白を定義します。](#2)
3. [スタイルのコントロールの子を作成します。](#3)
4. [コントロールのレイアウト テンプレートを作成します。](#4)
5. [テーマ固有のリソースを追加します。](#5)
6. [CardView クラスの ControlTemplate に設定します。](#6)
7. [ページにコントロールを追加します。](#7)

<a name="1" />

#### <a name="1-dataview-subclass"></a>1.DataView のサブクラス

C# サブクラス`DataView`コントロールのバインド可能なプロパティを定義します。

```csharp
public class CardView : DataView
{
    public static readonly BindableProperty TextProperty =
        BindableProperty.Create ("Text", typeof (string), typeof (CardView), null, BindingMode.TwoWay);

    public string Text
    {
        get { return (string)GetValue (TextProperty); }
        set { SetValue (TextProperty, value); }
    }

    public static readonly BindableProperty DetailProperty =
        BindableProperty.Create ("Detail", typeof (string), typeof (CardView), null, BindingMode.TwoWay);

    public string Detail
    {
        get { return (string)GetValue (DetailProperty); }
        set { SetValue (DetailProperty, value); }
    }

    public static readonly BindableProperty ImageSourceProperty =
        BindableProperty.Create ("ImageSource", typeof (ImageSource), typeof (CardView), null, BindingMode.TwoWay);

    public ImageSource ImageSource
    {
        get { return (ImageSource)GetValue (ImageSourceProperty); }
        set { SetValue (ImageSourceProperty, value); }
    }

    public CardView()
    {
    }
}
```

<a name="2" />

#### <a name="2-define-font-layout-and-margins"></a>2.フォント、レイアウト、および余白を定義します。

コントロール デザイナーは、カスタム コントロールのユーザー インターフェイス設計の一環として、これらの値を算出します。 プラットフォーム固有の仕様が必要ですが、`OnPlatform`要素を使用します。

いくつかの値を参照するメモ`StaticResource`s – これらで定義される[手順 5](#5)します。

```xml
<!-- CARDVIEW FONT SIZES -->
<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewTextFontSize">
        <On Platform="iOS, Android" Value="15" />
</OnPlatform>

<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewDetailFontSize">
        <On Platform="iOS, Android" Value="13" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color"    x:Key="CardViewTextTextColor">
        <On Platform="iOS" Value="{StaticResource iOSCardViewTextTextColor}" />
        <On Platform="Android" Value="{StaticResource AndroidCardViewTextTextColor}" />
</OnPlatform>

<OnPlatform x:TypeArguments="Thickness"    x:Key="CardViewTextlMargin">
        <On Platform="iOS" Value="12,10,12,4" />
        <On Platform="Android" Value="20,0,20,5" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color"    x:Key="CardViewDetailTextColor">
        <On Platform="iOS" Value="{StaticResource iOSCardViewDetailTextColor}" />
        <On Platform="Android" Value="{StaticResource AndroidCardViewDetailTextColor}" />
</OnPlatform>

<OnPlatform x:TypeArguments="Thickness"    x:Key="CardViewDetailMargin">
        <On Platform="iOS" Value="12,0,10,12" />
        <On Platform="Android" Value="20,0,20,20" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color"    x:Key="CardViewBackgroundColor">
        <On Platform="iOS" Value="{StaticResource iOSCardViewBackgroundColor}" />
        <On Platform="Android" Value="{StaticResource AndroidCardViewBackgroundColor}" />
</OnPlatform>

<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewShadowSize">
        <On Platform="iOS" Value="2" />
        <On Platform="Android" Value="5" />
</OnPlatform>

<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewCornerRadius">
        <On Platform="iOS" Value="0" />
        <On Platform="Android" Value="4" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color"    x:Key="CardViewShadowColor">
        <On Platform="iOS, Android" Value="#CDCDD1" />
</OnPlatform>
```

<a name="3" />

#### <a name="3-create-styles-for-the-controls-children"></a>3.スタイルのコントロールの子を作成します。

カスタム コントロールで使用される子を作成しようと定義されているすべての要素を参照します。

```xml
<!-- EXPLICIT STYLES (will be Classes) -->
<Style TargetType="Label" x:Key="CardViewTextStyle">
    <Setter Property="FontSize" Value="{ StaticResource CardViewTextFontSize }" />
    <Setter Property="TextColor" Value="{ StaticResource CardViewTextTextColor }" />
    <Setter Property="HorizontalOptions" Value="Start" />
    <Setter Property="Margin" Value="{ StaticResource CardViewTextlMargin }" />
    <Setter Property="HorizontalTextAlignment" Value="Start" />
</Style>

<Style TargetType="Label" x:Key="CardViewDetailStyle">
    <Setter Property="HorizontalTextAlignment" Value="Start" />
    <Setter Property="TextColor" Value="{ StaticResource CardViewDetailTextColor }" />
    <Setter Property="FontSize" Value="{ StaticResource CardViewDetailFontSize }" />
    <Setter Property="HorizontalOptions" Value="Start" />
    <Setter Property="Margin" Value="{ StaticResource CardViewDetailMargin }" />
</Style>

<Style TargetType="Image" x:Key="CardViewImageImageStyle">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="Center" />
    <Setter Property="WidthRequest" Value="220"/>
    <Setter Property="HeightRequest" Value="165"/>
</Style>
```

<a name="4" />

#### <a name="4-create-the-control-layout-template"></a>4.コントロールのレイアウト テンプレートを作成します。

カスタム コントロールのビジュアル デ ザインは上記で定義されたリソースを使用して、コントロール テンプレートで明示的に宣言します。

```xml
<!--- CARDVIEW -->
<ControlTemplate x:Key="CardViewControlControlTemplate">
  <StackLayout
    Spacing="0"
    BackgroundColor="{ TemplateBinding BackgroundColor }"
  >

    <!-- CARDVIEW IMAGE -->
    <Image
      Source="{ TemplateBinding ImageSource }"
      HorizontalOptions="FillAndExpand"
      VerticalOptions="StartAndExpand"
      Aspect="AspectFill"
      Style="{ StaticResource CardViewImageImageStyle }"
    />

    <!-- CARDVIEW TEXT -->
    <Label
      Text="{ TemplateBinding Text }"
      LineBreakMode="WordWrap"
      VerticalOptions="End"
      Style="{ StaticResource CardViewTextStyle }"
    />


    <!-- CARDVIEW DETAIL -->
    <Label
      Text="{ TemplateBinding Detail }"
      LineBreakMode="WordWrap"
      VerticalOptions="End"
      Style="{ StaticResource CardViewDetailStyle }" />

  </StackLayout>

</ControlTemplate>
```

<a name="5" />

#### <a name="5-add-the-theme-specific-resources"></a>5.テーマ固有のリソースを追加します。

これは、カスタム コントロールであるため、リソース ディクショナリを使用しているテーマに一致するリソースを追加します。

##### <a name="light-theme-colors"></a>ライト テーマの色

```xaml
<Color x:Key="iOSCardViewBackgroundColor">#FFFFFF</Color>
<Color x:Key="AndroidCardViewBackgroundColor">#FFFFFF</Color>

<Color x:Key="AndroidCardViewTextTextColor">#030303</Color>
<Color x:Key="iOSCardViewTextTextColor">#030303</Color>

<Color x:Key="AndroidCardViewDetailTextColor">#8F8E94</Color>
<Color x:Key="iOSCardViewDetailTextColor">#8F8E94</Color>
```

##### <a name="dark-theme-colors"></a>ダーク テーマの色

```xaml
<!-- CARD VIEW COLORS -->
            <Color x:Key="iOSCardViewBackgroundColor">#404040</Color>
            <Color x:Key="AndroidCardViewBackgroundColor">#404040</Color>

            <Color x:Key="AndroidCardViewTextTextColor">#FFFFFF</Color>
            <Color x:Key="iOSCardViewTextTextColor">#FFFFFF</Color>

            <Color x:Key="AndroidCardViewDetailTextColor">#B5B4B9</Color>
            <Color x:Key="iOSCardViewDetailTextColor">#B5B4B9</Color>
```

<a name="6" />

#### <a name="6-set-the-controltemplate-for-the-cardview-class"></a>6.CardView クラスの ControlTemplate に設定します。

最後に、c# クラスで作成されたことを確認[手順 1.](#1)で定義されているコントロール テンプレートを使用して[手順 4](#4)を使用して、 `Style` `Setter`要素

```xml
<Style TargetType="local:CardView">
    <Setter Property="ControlTemplate" Value="{ StaticResource CardViewControlControlTemplate }" />
  ... some custom styling omitted
  <Setter Property="BackgroundColor" Value="{ StaticResource CardViewBackgroundColor }" />
</Style>
```

<a name="7" />

#### <a name="7-add-the-control-to-a-page"></a>7.ページにコントロールを追加します。

`CardView`コントロールをページに追加できるようになりました。 次の例を示しますでホストされていること、 `StackLayout`:

```xaml
<StackLayout Spacing="0">
  <local:CardView
    Margin="12,6"
    ImageSource="{ DynamicResource CardViewImage }"
    Text="CardView Text"
    Detail="CardView Detail"
  />
</StackLayout>

```
