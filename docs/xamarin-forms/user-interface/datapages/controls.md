---
title: ''
description: この記事では、DataPages NuGet パッケージで使用できるコントロールについて説明 Xamarin.Forms します。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 093ef4b9b3ae7bde25da276330894bcf4e399145
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84134447"
---
# <a name="datapages-controls-reference"></a>DataPages コントロールのリファレンス

![](~/media/shared/preview.png "This API is currently in preview")

> [!IMPORTANT]
> DataPages Xamarin.Forms を表示するには、テーマ参照が必要です。 これには、のインストールが含ま[ Xamarin.Forms れます。Theme。](https://www.nuget.org/packages/Xamarin.Forms.Theme.Base/)プロジェクトの基本となる NuGet パッケージの後に、[のいずれかが続きます。 Xamarin.FormsTheme](https://www.nuget.org/packages/Xamarin.Forms.Theme.Light/)または[ Xamarin.Forms 。Theme. ダーク](https://www.nuget.org/packages/Xamarin.Forms.Theme.Dark/)NuGet パッケージ。

DataPages NuGet には、 Xamarin.Forms データソースバインドを利用できる多数のコントロールが含まれています。

これらのコントロールを XAML で使用するには、名前空間が含まれていることを確認します。たとえば、次の宣言を参照してください `xmlns:pages` 。

```xaml
<ContentPage
    xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:pages="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
    x:Class="DataPagesDemo.Detail">
```

次の例には、 `DynamicResource` プロジェクトの resources ディクショナリに存在しなければならない参照が含まれています。 [カスタムコントロール](#custom)を構築する方法の例もあります。

## <a name="built-in-controls"></a>組み込みコントロール

* [HeroImage](#heroimage)
* [ListItem](#listitem)

<a name="heroimage" />

### <a name="heroimage"></a>HeroImage

この `HeroImage` コントロールには、次の4つのプロパティがあります。

* テキスト
* 詳細
* ImageSource
* 特徴

```xaml
<pages:HeroImage
    ImageSource="{ DynamicResource HeroImageImage }"
    Text="Keith Ballinger"
    Detail="Xamarin"
/>
```

**Android**

![](controls-images/heroimage-light-android.png "Android の HeroImage コントロール") ![](controls-images/heroimage-dark-android.png "Android の HeroImage コントロール")

**iOS**

![](controls-images/heroimage-light-ios.png "IOS の HeroImage コントロール") ![](controls-images/heroimage-dark-ios.png "IOS の HeroImage コントロール")

<a name="listitem" />

### <a name="listitem"></a>ListItem

`ListItem`コントロールのレイアウトは、ネイティブの iOS および Android の一覧またはテーブルの行に似ていますが、通常のビューとしても使用できます。 下のコード例では、がの内部でホストされてい `StackLayout` ますが、データバインドされたリストコントロールで使用することもできます。

次の5つのプロパティがあります。

* タイトル
* 詳細
* ImageSource
* PlaceholdImageSource
* 特徴

```xaml
<StackLayout Spacing="0">
    <pages:ListItemControl
        Detail="Xamarin"
        ImageSource="{ DynamicResource UserImage }"
        Title="Miguel de Icaza"
        PlaceholdImageSource="{ DynamicResource IconImage }"
    />
```

これらのスクリーンショットは、次のよう `ListItem` に、明るいテーマとダークテーマの両方を使用した iOS および Android プラットフォームのを示しています。

**Android**

![](controls-images/listitem-light-android.png "Android での ListItem コントロール") ![](controls-images/listitem-dark-android.png "Android での ListItem コントロール")

**iOS**

![](controls-images/listitem-light-ios.png "IOS の ListItem コントロール") ![](controls-images/listitem-dark-ios.png "IOS の ListItem コントロール")

## <a name="custom-control-example"></a>カスタムコントロールの例

このカスタムコントロールの目的は、 `CardView` ネイティブの Android CardView に似ていることです。

次の3つのプロパティが含まれます。

* テキスト
* 詳細
* ImageSource

目標は、次のコードのように見えるカスタムコントロールです (現在のアセンブリを参照するカスタムが必要であることに注意して `xmlns:local` ください)。

```xaml
<local:CardView
  ImageSource="{ DynamicResource CardViewImage }"
  Text="CardView Text"
  Detail="CardView Detail"
/>
```

組み込みのライトとダークテーマに対応する色を使用して、次のスクリーンショットのようになります。

**Android**

![](controls-images/cardview-light-android.png "Android での CardView カスタムコントロール") ![](controls-images/cardview-dark-android.png "Android での CardView カスタムコントロール")

**iOS**

![](controls-images/cardview-light-ios.png "IOS での CardView カスタムコントロール") ![](controls-images/cardview-dark-ios.png "IOS での CardView カスタムコントロール")

<a name="custom" />

### <a name="building-the-custom-cardview"></a>カスタム CardView のビルド

1. [DataView サブクラス](#1)
2. [フォント、レイアウト、および余白を定義する](#2)
3. [コントロールの子のスタイルを作成する](#3)
4. [コントロールレイアウトテンプレートを作成する](#4)
5. [テーマ固有のリソースを追加する](#5)
6. [CardView クラスの ControlTemplate を設定します。](#6)
7. [コントロールをページに追加する](#7)

<a name="1" />

#### <a name="1-dataview-subclass"></a>1. DataView サブクラス

の C# サブクラスは、 `DataView` コントロールのバインド可能なプロパティを定義します。

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

#### <a name="2-define-font-layout-and-margins"></a>2. フォント、レイアウト、および余白を定義する

コントロールデザイナーは、カスタムコントロールのユーザーインターフェイスデザインの一部としてこれらの値を確認します。 プラットフォーム固有の仕様が必要な場合は、 `OnPlatform` 要素が使用されます。

が参照する値として `StaticResource` は、[手順 5](#5). で定義されているものがあります。

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

#### <a name="3-create-styles-for-the-controls-children"></a>3. コントロールの子のスタイルを作成する

に対して定義されているすべての要素を参照して、カスタムコントロールで使用される子を作成します。

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

#### <a name="4-create-the-control-layout-template"></a>4. コントロールレイアウトテンプレートを作成する

カスタムコントロールのビジュアルデザインは、上で定義したリソースを使用して、コントロールテンプレートで明示的に宣言されます。

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

#### <a name="5-add-the-theme-specific-resources"></a>5. テーマ固有のリソースを追加する

これはカスタムコントロールであるため、リソースディクショナリを使用しているテーマに一致するリソースを追加します。

##### <a name="light-theme-colors"></a>明るいテーマの色

```xaml
<Color x:Key="iOSCardViewBackgroundColor">#FFFFFF</Color>
<Color x:Key="AndroidCardViewBackgroundColor">#FFFFFF</Color>

<Color x:Key="AndroidCardViewTextTextColor">#030303</Color>
<Color x:Key="iOSCardViewTextTextColor">#030303</Color>

<Color x:Key="AndroidCardViewDetailTextColor">#8F8E94</Color>
<Color x:Key="iOSCardViewDetailTextColor">#8F8E94</Color>
```

##### <a name="dark-theme-colors"></a>ダークテーマの色

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

#### <a name="6-set-the-controltemplate-for-the-cardview-class"></a>6. CardView クラスの ControlTemplate を設定する

最後に、[手順 1](#1) . で作成した C# クラスが、要素を使用して、[手順 4.](#4)で定義したコントロールテンプレートを使用していることを確認します。 `Style` `Setter`

```xml
<Style TargetType="local:CardView">
    <Setter Property="ControlTemplate" Value="{ StaticResource CardViewControlControlTemplate }" />
  ... some custom styling omitted
  <Setter Property="BackgroundColor" Value="{ StaticResource CardViewBackgroundColor }" />
</Style>
```

<a name="7" />

#### <a name="7-add-the-control-to-a-page"></a>7. コントロールをページに追加する

`CardView`これで、コントロールをページに追加できるようになりました。 次の例は、でホストされていることを示してい `StackLayout` ます。

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
