---
title: Xamarin.Forms の StackLayout
description: この記事では、Xamarin.Forms StackLayout クラスを使用して、1 つのディメンション上のビューのコレクションを表示する方法について説明します。
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: 28a4fc6527a8c6990c47cdbbdc3163fc9350db83
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70770274"
---
# <a name="xamarinforms-stacklayout"></a>Xamarin.Forms の StackLayout

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

[Stacklayout](xref:Xamarin.Forms.StackLayout)は、水平方向または垂直方向の1次元線 ("スタック") でビューを整理します。 ビューで、`StackLayout`レイアウト オプションを使用して、レイアウト領域に基づくサイズ設定することができます。 配置は、ビューは、レイアウトとビューのレイアウト オプションに追加された順序によって決まります。

[![](stack-layout-images/layouts-sml.png "Xamarin.Forms のレイアウト")](stack-layout-images/layouts.png#lightbox "Xamarin.Forms のレイアウト")

## <a name="purpose"></a>目的

`StackLayout` 他のビューよりも簡単です。 ビューを追加するだけで線形の単純なインターフェイスを作成することができます、 `StackLayout`、および入れ子にして作成されたより複雑なインターフェイスです。

## <a name="usage--behavior"></a>使用状況と動作

### <a name="spacing"></a>スペース

既定では、`StackLayout`ビュー間 6px 余白が追加されます。 制御または設定の余白がないように設定するには、この、 `Spacing` StackLayout プロパティ。 次に、空白文字とスペースのさまざまなオプションの効果を設定する方法を示しています。

で XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.StackLayoutDemo"
Title="StackLayout Demo">
  <ContentPage.Content>
    <StackLayout Spacing="10" x:Name="layout">
      <Button Text="StackLayout" VerticalOptions="Start"
        HorizontalOptions="FillAndExpand" />
      <BoxView Color="Yellow" VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView Color="Green" VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView HeightRequest="75" Color="Blue" VerticalOptions="End"
        HorizontalOptions="FillAndExpand" />
    </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

C# の場合:

```csharp
public class StackLayoutCode : ContentPage
{
  public StackLayoutCode ()
  {
    var layout = new StackLayout ();
    var button = new Button { Text = "StackLayout", VerticalOptions = LayoutOptions.Start,
     HorizontalOptions = LayoutOptions.FillAndExpand };
    var yellowBox = new BoxView { Color = Color.Yellow, VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var greenBox = new BoxView { Color = Color.Green, VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var blueBox = new BoxView { Color = Color.Blue, VerticalOptions = LayoutOptions.FillAndExpand,
      HorizontalOptions = LayoutOptions.FillAndExpand, HeightRequest = 75 };

    layout.Children.Add(button);
    layout.Children.Add(yellowBox);
    layout.Children.Add(greenBox);
    layout.Children.Add(blueBox);
    layout.Spacing = 10;
    Content = layout;
  }
}
```

間隔 = 0。

![](stack-layout-images/spacing-zero.png "StackLayout を間隔 = 0")

10 個の空白文字:

![](stack-layout-images/spacing-ten.png "StackLayout 間隔で 10 を =")

### <a name="sizing"></a>サイズ変更

StackLayout 内でビューのサイズは、高さと幅の要求とレイアウト オプションの両方に依存します。 `StackLayout` 埋め込みが適用されます。 次`LayoutOption`s がレイアウトから使用可能な限り多くの領域を占有するビューになります。

- **CenterAndExpand** &ndash;レイアウト内でビューの中心し、レイアウトが指定されます限り多くの領域を占有に展開されます。
- **EndAndExpand** &ndash;レイアウト (下部または右端の境界) の最後に、ビューを配置し、レイアウトが指定されます限り多くの領域を占有に展開します。
- **FillAndExpand** &ndash;余白がありませんし、レイアウトが指定されます限り多くの領域を占有するように、ビューを配置します。
- **StartAndExpand** &ndash;レイアウトの開始時、ビューを配置し、親は、限り多くの領域を占有します。

詳細については、次を参照してください。[拡張](~/xamarin-forms/user-interface/layouts/layout-options.md#expansion)します。

### <a name="positioning"></a>配置

StackLayout 内でビューが配置できを使用してサイズ`LayoutOptions`します。 それぞれのビューを得ることができます`VerticalOptions`と`HorizontalOptions`ビューは配置方法自体、レイアウトを基準を定義します。 次の定義済み`LayoutOptions`利用できます。

- **Center** &ndash;レイアウト内でビューの中心です。
- **終了**&ndash;レイアウト (下部または右端の境界) の最後に、ビューを配置します。
- **入力**&ndash;パディングを持たないように、ビューを配置します。
- **開始**&ndash;レイアウトの開始時、ビューを配置します。

次のコードでは、レイアウト オプションの設定を示しています。

で XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.StackLayoutDemo"
Title="StackLayout Demo">
  <ContentPage.Content>
    <StackLayout x:Name="layout">
      <Button VerticalOptions="Start"
        HorizontalOptions="FillAndExpand" />
      <BoxView VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView HeightRequest="75" VerticalOptions="End"
        HorizontalOptions="FillAndExpand" />
    </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

C# の場合:

```csharp
public class StackLayoutCode : ContentPage
{
  public StackLayoutCode ()
  {
    var layout = new StackLayout ();
    var button = new Button { VerticalOptions = LayoutOptions.Start,
     HorizontalOptions = LayoutOptions.FillAndExpand };
    var oneBox = new BoxView { VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var twoBox = new BoxView {  VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var threeBox = new BoxView {  VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };

    layout.Children.Add(button);
    layout.Children.Add(oneBox);
    layout.Children.Add(twoBox);
    layout.Children.Add(threeBox);
    Content = layout;
  }
}
```

詳細については、次を参照してください。[配置](~/xamarin-forms/user-interface/layouts/layout-options.md#alignment)します。

## <a name="exploring-a-complex-layout"></a>複雑なレイアウトの調査

レイアウトの各は、特定のレイアウトを作成する長所と短所があります。 この一連のレイアウトの記事では、全体で同じページ レイアウトを次の 3 つの異なるレイアウトを使用して実装されているサンプル アプリが作成されました。

次の XAML を検討してください。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TheBusinessTumble.StackLayoutPage"
BackgroundColor="Maroon"
Title="StackLayouts">
    <ContentPage.Content>
    <ScrollView>
      <StackLayout Spacing="0" Padding="0" BackgroundColor="Maroon">
        <BoxView HorizontalOptions="FillAndExpand" HeightRequest="100"
          VerticalOptions="Start" Color="Gray" />
        <Button BorderRadius="30" HeightRequest="60" WidthRequest="60"
          BackgroundColor="Red" HorizontalOptions="Center" VerticalOptions="Start" />
        <StackLayout HeightRequest="100" VerticalOptions="Start" HorizontalOptions="FillAndExpand" Spacing="20" BackgroundColor="Maroon">
          <Label Text="User Name" FontSize="28" HorizontalOptions="Center"
            VerticalOptions="Center" FontAttributes="Bold" />
          <Entry Text="Bio + Hashtags" TextColor="White"
            BackgroundColor="Maroon" HorizontalOptions="FillAndExpand" VerticalOptions="CenterAndExpand" />
        </StackLayout>
        <StackLayout Orientation="Horizontal" HeightRequest="50" BackgroundColor="White" Padding="5">
          <StackLayout Spacing="0" BackgroundColor="White" Orientation="Horizontal" HorizontalOptions="Start">
            <BoxView BackgroundColor="Black" WidthRequest="40" HeightRequest="40"  HorizontalOptions="StartAndExpand" VerticalOptions="Center" />
            <Label FontSize="14" TextColor="Black" Text="Accent Color" HorizontalOptions="StartAndExpand" VerticalOptions="Center" />
          </StackLayout>
          <StackLayout Spacing="0" BackgroundColor="White" Orientation="Horizontal" HorizontalOptions="EndAndExpand">
            <BoxView BackgroundColor="Maroon" WidthRequest="40" HeightRequest="40" HorizontalOptions="Start" VerticalOptions="Center" />
            <Label FontSize="14" TextColor="Black" Text="Primary Color" HorizontalOptions="StartAndExpand" VerticalOptions="Center" />
          </StackLayout>
        </StackLayout>
        <StackLayout Orientation="Horizontal">
          <Label FontSize="14" Text="Age:" TextColor="White" HorizontalOptions="Start" VerticalOptions="Center" WidthRequest="100" />
          <Entry HorizontalOptions="FillAndExpand" Text="35" TextColor="White" BackgroundColor="Maroon" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
          <Label FontSize="14" Text="Interests:" TextColor="White"
          HorizontalOptions="Start" VerticalOptions="Center" WidthRequest="100" />
          <Entry HorizontalOptions="FillAndExpand" Text="Xamarin.Forms" TextColor="White" BackgroundColor="Maroon" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
          <Label FontSize="14" Text="Ask me about:" TextColor="White"
          HorizontalOptions="Start" VerticalOptions="Center" WidthRequest="100"/>
          <Entry HorizontalOptions="FillAndExpand" Text="Xamarin, C#, .NET, Mono..." TextColor="White" BackgroundColor="Maroon" />
        </StackLayout>
      </StackLayout>
    </ScrollView>
    </ContentPage.Content>
</ContentPage>

```

上記のコードは、次のレイアウトが得られます。

![](stack-layout-images/stack.png "複雑な StackLayout")

が入れ子に`StackLayouts`なっていることに注意してください。入れ子になったレイアウトは、同じレイアウト内のすべての要素を表示するよりも簡単な場合があるためです。 またため、いることを確認`StackLayout`ページがいくつかのレイアウト盛り込んでいますを見つけた他のレイアウトのページで、アイテムの重なりをサポートしません。

## <a name="related-links"></a>関連リンク

- [LayoutOptions](~/xamarin-forms/user-interface/layouts/layout-options.md)
- [レイアウト (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [BusinessTumble 例 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
