---
title: Xamarin.Forms フレーム
description: Xamarin.FormsFrame クラスは、色、影、およびその他のオプションを使用して構成できる境界線を持つビューまたはレイアウトをラップするために使用されるレイアウトです。
ms.prod: xamarin
ms.assetId: 4E074714-0928-41C8-A468-B60E23236A8C
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 08/06/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ba5fd2e8488f1f28f6bdc02b85c8e41fa212be32
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373745"
---
# <a name="no-locxamarinforms-frame"></a>Xamarin.Forms フレーム

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-frame/)

クラスは、 Xamarin.Forms [`Frame`](xref:Xamarin.Forms.Frame) 色、影、およびその他のオプションを使用して構成できる境界線を持つビューをラップするために使用されるレイアウトです。 フレームは、通常、コントロールの周囲に境界線を作成するために使用されますが、より複雑な UI を作成するために使用できます。 詳細については、「 [高度なフレームの使用](#advanced-frame-usage)」を参照してください。

次のスクリーンショットは、 `Frame` iOS と Android のコントロールを示しています。

[!["IOS と Android のフレームの例"](frame-images/frame-cropped.png)](frame-images/frame-full.png#lightbox "IOS と Android のフレームの例")

`Frame` クラスでは、次のプロパティが定義されます。

* [`BorderColor`](xref:Xamarin.Forms.Frame.BorderColor)`Color`境界線の色を決定する値です `Frame` 。
* [`CornerRadius`](xref:Xamarin.Forms.Frame.CornerRadius)`float`角の丸みの半径を決定する値です。
* [`HasShadow`](xref:Xamarin.Forms.Frame.HasShadow) フレームに `bool` ドロップシャドウがあるかどうかを決定する値です。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。つまり、は `Frame` データバインディングのターゲットにすることができます。

> [!NOTE]
> `HasShadow`プロパティの動作は、プラットフォームに依存します。 既定値は、 `true` すべてのプラットフォーム上にあります。 ただし、UWP ドロップシャドウはレンダリングされません。 ドロップシャドウは Android と iOS の両方でレンダリングされますが、iOS のドロップシャドウは暗いため、より多くの領域を占有します。

## <a name="create-a-frame"></a>フレームを作成する

は、 `Frame` XAML でインスタンス化できます。 既定の `Frame` オブジェクトには白色の背景、ドロップシャドウ、および境界線がありません。 `Frame`通常、オブジェクトは別のコントロールをラップします。 次の例は、オブジェクトをラップする既定のを示してい `Frame` `Label` ます。

```xaml
<Frame>
  <Label Text="Example" />
</Frame>
```

は、 `Frame` コードで作成することもできます。

```csharp
Frame defaultFrame = new Frame
{
    Content = new Label { Text = "Example" }
};
```

`Frame` XAML でプロパティを設定することにより、角が丸く、色分けされた境界、ドロップシャドウでオブジェクトをカスタマイズできます。 次の例は、カスタマイズされたオブジェクトを示してい `Frame` ます。

```xaml
<Frame BorderColor="Orange"
       CornerRadius="10"
       HasShadow="True">
  <Label Text="Example" />
</Frame>
```

これらのインスタンスプロパティは、コードで設定することもできます。

```csharp
Frame frame = new Frame
{
    BorderColor = Color.Orange,
    CornerRadius = 10,
    HasShadow = true,
    Content = new Label { Text = "Example" }
};
```

## <a name="advanced-frame-usage"></a>高度なフレームの使用方法

`Frame`クラスは、から継承されます。これは、オブジェクトを `ContentView` 含む任意の型のオブジェクトを含むことができることを意味し `View` `Layout` ます。 この機能により、 `Frame` を使用して、カードなどの複雑な UI オブジェクトを作成できます。

### <a name="create-a-card-with-a-frame"></a>フレームを含むカードを作成する

オブジェクトをオブジェクトなどの `Frame` オブジェクトと組み合わせる `Layout` `StackLayout` ことで、より複雑な UI を作成できます。 次のスクリーンショットは、オブジェクトを使用して作成されたカードの例を示してい `Frame` ます。

[!["フレームで作成されたカードのスクリーンショット"](frame-images/frame-card-cropped.png)](frame-images/frame-full.png#lightbox "フレームで作成されたカードのスクリーンショット")

次の XAML は、クラスを使用してカードを作成する方法を示してい `Frame` ます。

```xaml
<Frame BorderColor="Gray"
       CornerRadius="5"
       Padding="8">
  <StackLayout>
    <Label Text="Card Example"
           FontSize="Medium"
           FontAttributes="Bold" />
    <BoxView Color="Gray"
             HeightRequest="2"
             HorizontalOptions="Fill" />
    <Label Text="Frames can wrap more complex layouts to create more complex UI components, such as this card!"/>
  </StackLayout>
</Frame>
```

また、コードでカードを作成することもできます。

```csharp
Frame cardFrame = new Frame
{
    BorderColor = Color.Gray,
    CornerRadius = 5,
    Padding = 8,
    Content = new StackLayout
    {
        Children =
        {
            new Label
            {
                Text = "Card Example",
                FontSize = Device.GetNamedSize(NamedSize.Medium, typeof(Label)),
                FontAttributes = FontAttributes.Bold
            },
            new BoxView
            {
                Color = Color.Gray,
                HeightRequest = 2,
                HorizontalOptions = LayoutOptions.Fill
            },
            new Label
            {
                Text = "Frames can wrap more complex layouts to create more complex UI components, such as this card!"
            }
        }
    }
};
```

### <a name="round-elements"></a>Round 要素

`CornerRadius`コントロールのプロパティを使用して、 `Frame` 円イメージを作成できます。 次のスクリーンショットは、オブジェクトを使用して作成された丸い画像の例を示してい `Frame` ます。

[!["フレームで作成された円イメージのスクリーンショット"](frame-images/circle-image-cropped.png)](frame-images/frame-full.png#lightbox "フレームを使用して作成された円形画像のスクリーンショット")

次の XAML は、XAML で円イメージを作成する方法を示しています。

```xaml
<Frame Margin="10"
       BorderColor="Black"
       CornerRadius="50"
       HeightRequest="60"
       WidthRequest="60"
       IsClippedToBounds="True"
       HorizontalOptions="Center"
       VerticalOptions="Center">
  <Image Source="outdoors.jpg"
         Aspect="AspectFill"
         Margin="-20"
         HeightRequest="100"
         WidthRequest="100" />
</Frame>
```

コードでは、円のイメージを作成することもできます。

```csharp
Frame circleImageFrame = new Frame
{
    Margin = 10,
    BorderColor = Color.Black,
    CornerRadius = 50,
    HeightRequest = 60,
    WidthRequest = 60,
    IsClippedToBounds = true,
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center,
    Content = new Image
    {
        Source = ImageSource.FromFile("outdoors.jpg"),
        Aspect = Aspect.AspectFill,
        Margin = -20,
        HeightRequest = 100,
        WidthRequest = 100
    }
};
```

**outdoors.jpg** イメージは、各プラットフォームプロジェクトに追加する必要があります。また、プラットフォームによってどのように実現されるかが異なります。 詳細については、「 [」の Xamarin.Forms 「画像](~/xamarin-forms/user-interface/images.md)」を参照してください。

> [!NOTE]
> 角を丸くすると、プラットフォームによって動作が若干異なります。 `Image`オブジェクトのは、 `Margin` イメージの幅と親フレームの幅の差の半分である必要があります。また、オブジェクト内でイメージを均等に中央揃えにするには、負の値にする必要があり `Frame` ます。 ただし、要求された幅と高さは保証されないため、 `Margin` 、、 `HeightRequest` およびの各プロパティは、イメージの `WidthRequest` サイズやその他のレイアウトの選択肢に基づいて変更する必要があります。

## <a name="related-links"></a>関連リンク

* [フレームのデモ](/samples/xamarin/xamarin-forms-samples/userinterface-frame/)
* [画像 Xamarin.Forms](~/xamarin-forms/user-interface/images.md)