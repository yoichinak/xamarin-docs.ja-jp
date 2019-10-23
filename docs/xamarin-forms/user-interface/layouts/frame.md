---
title: Xamarin. Forms Frame
description: Xamarin Frame クラスは、色、影、およびその他のオプションを使用して構成できる境界線を持つビューまたはレイアウトをラップするために使用されるレイアウトです。
ms.prod: xamarin
ms.assetId: 4E074714-0928-41C8-A468-B60E23236A8C
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 08/06/2019
ms.openlocfilehash: 619b29a9d65594b1badd805c3361fe1a174d7174
ms.sourcegitcommit: dad4dfcd194b63ec9e903363351b6d9e543d4888
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/18/2019
ms.locfileid: "69976495"
---
# <a name="xamarinforms-frame"></a>Xamarin. Forms Frame

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-frame/)

Xamarin [`Frame`](xref:Xamarin.Forms.Frame)クラスは、色、影、およびその他のオプションを使用して構成できる境界線を持つビューをラップするために使用されるレイアウトです。 フレームは、通常、コントロールの周囲に境界線を作成するために使用されますが、より複雑な UI を作成するために使用できます。 詳細については、「[高度なフレームの使用](#advanced-frame-usage)」を参照してください。

次のスクリーンショットは、iOS と Android の `Frame` コントロールを示しています。

[![ "iOS と Android でのフレームの例"](frame-images/frame-cropped.png)](frame-images/frame-full.png#lightbox "IOS と Android のフレームの例")

@No__t_0 クラスは、次のプロパティを定義します。

* [`BorderColor`](xref:Xamarin.Forms.Frame.BorderColor)は、`Frame` 境界線の色を決定する `Color` 値です。
* [`CornerRadius`](xref:Xamarin.Forms.Frame.CornerRadius)は、角の丸みの半径を決定する `float` 値です。
* [`HasShadow`](xref:Xamarin.Forms.Frame.HasShadow)は、フレームにドロップシャドウがあるかどうかを判断する `bool` 値です。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)のオブジェクトによってサポートされています。つまり、`Frame` をデータバインディングのターゲットにすることができます。

> [!NOTE]
> @No__t_0 プロパティの動作は、プラットフォームに依存します。 既定値は、すべてのプラットフォームで `true` ます。 ただし、UWP ドロップシャドウはレンダリングされません。 ドロップシャドウは Android と iOS の両方でレンダリングされますが、iOS のドロップシャドウは暗いため、より多くの領域を占有します。

## <a name="create-a-frame"></a>フレームを作成する

@No__t_0 は、XAML でインスタンス化できます。 既定の `Frame` オブジェクトには、白色の背景、ドロップシャドウ、および境界線がありません。 通常、`Frame` オブジェクトは別のコントロールをラップします。 次の例は、`Label` オブジェクトをラップする既定の `Frame` を示しています。

```xaml
<Frame>
  <Label Text="Example" />
</Frame>
```

コードでは、`Frame` を作成することもできます。

```csharp
Frame defaultFrame = new Frame
{
    Content = new Label { Text = "Example" }
};
```

XAML でプロパティを設定することにより、角が丸く、色分けされた境界線、ドロップシャドウを使用して、`Frame` オブジェクトをカスタマイズできます。 次の例は、カスタマイズされた `Frame` オブジェクトを示しています。

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

@No__t_0 クラスは `ContentView` から継承されます。これは、`Layout` オブジェクトを含む任意の型の `View` オブジェクトを含むことができることを意味します。 この機能により、`Frame` を使用して、カードなどの複雑な UI オブジェクトを作成できます。

### <a name="create-a-card-with-a-frame"></a>フレームを含むカードを作成する

@No__t_0 オブジェクトを `StackLayout` オブジェクトなどの `Layout` オブジェクトと組み合わせることで、より複雑な UI を作成できます。 次のスクリーンショットは、`Frame` オブジェクトを使用して作成されたカードの例を示しています。

[![ "フレームで作成されたカードのスクリーンショット"](frame-images/frame-card-cropped.png)](frame-images/frame-full.png#lightbox "フレームで作成されたカードのスクリーンショット")

次の XAML は、`Frame` クラスを使用してカードを作成する方法を示しています。

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

@No__t_1 コントロールの `CornerRadius` プロパティを使用して、円イメージを作成できます。 次のスクリーンショットは、`Frame` オブジェクトを使用して作成された丸い画像の例を示しています。

[![ "フレームで作成された円イメージのスクリーンショット"](frame-images/circle-image-cropped.png)](frame-images/frame-full.png#lightbox "フレームを使用して作成された円形画像のスクリーンショット")

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

各プラットフォームプロジェクトには、**アウトドア .jpg**イメージを追加する必要があります。また、プラットフォームによってどのように実現されるかが異なります。 詳細については、「 [Xamarin. Forms のイメージ](~/xamarin-forms/user-interface/images.md)」を参照してください。

> [!NOTE]
> 角を丸くすると、プラットフォームによって動作が若干異なります。 @No__t_0 オブジェクトの `Margin` は、イメージの幅と親フレームの幅の差の半分にする必要があります。また、`Frame` オブジェクト内でイメージを均等に中央揃えにするには、負の値にする必要があります。 ただし、要求された幅と高さは保証されないため、`Margin`、`HeightRequest`、および `WidthRequest` の各プロパティは、イメージのサイズやその他のレイアウトの選択に応じて変更する必要がある場合があります。

## <a name="related-links"></a>関連リンク

* [フレームのデモ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-frame/)
* [Xamarin 形式の画像](~/xamarin-forms/user-interface/images.md)
