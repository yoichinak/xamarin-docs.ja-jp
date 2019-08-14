---
title: Xamarin. Forms Frame
description: Xamarin Frame クラスは、色、影、およびその他のオプションを使用して構成できる境界線を持つビューまたはレイアウトをラップするために使用されるレイアウトです。
ms.prod: xamarin
ms.assetId: 4E074714-0928-41C8-A468-B60E23236A8C
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 08/06/2019
ms.openlocfilehash: 329edf455a4778efa5f6fbd1ec88cfb274c5cb49
ms.sourcegitcommit: 41a029c69925e3a9d2de883751ebfd649e8747cd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/13/2019
ms.locfileid: "68984424"
---
# <a name="xamarinforms-frame"></a>Xamarin. Forms Frame

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/en-us/samples/xamarin/xamarin-forms-samples/userinterface-frame/)

Xamarin [`Frame`](xref:Xamarin.Forms.Frame)クラスは、色、影、およびその他のオプションを使用して構成できる境界線を持つビューをラップするために使用されるレイアウトです。 フレームは、通常、コントロールの周囲に境界線を作成するために使用されますが、より複雑な UI を作成するために使用できます。 詳細については、「[高度なフレームの使用](#advanced-frame-usage)」を参照してください。

次のスクリーンショット`Frame`は、iOS と Android のコントロールを示しています。

Ios と(frame-images/frame-cropped.png)](frame-images/frame-full.png#lightbox "android での") ["ios と android のフレームの例" フレームの例![]

クラス`Frame`は、次のプロパティを定義します。

* [`BorderColor`](xref:Xamarin.Forms.Frame.BorderColor)境界線の色を決定する値です。`Color` `Frame`
* [`CornerRadius`](xref:Xamarin.Forms.Frame.CornerRadius)角の丸みの半径を決定する値です。`float`
* [`HasShadow`](xref:Xamarin.Forms.Frame.HasShadow)フレームにドロップシャドウがあるかどうかを決定する値です。`bool`

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトによって支え`Frame`られています。つまり、はデータバインディングのターゲットにすることができます。

> [!NOTE]
> プロパティ`HasShadow`の動作は、プラットフォームに依存します。 既定値は`true` 、すべてのプラットフォーム上にあります。 ただし、UWP ドロップシャドウはレンダリングされません。 ドロップシャドウは Android と iOS の両方でレンダリングされますが、iOS のドロップシャドウは暗いため、より多くの領域を占有します。

## <a name="create-a-frame"></a>フレームを作成する

は`Frame` 、XAML でインスタンス化できます。 既定`Frame`のオブジェクトには白色の背景、ドロップシャドウ、および境界線がありません。 通常`Frame` 、オブジェクトは別のコントロールをラップします。 次の例は、オブジェクト`Frame`を`Label`ラップする既定のを示しています。

```xaml
<Frame>
  <Label Text="Example" />
</Frame>
```

は`Frame` 、コードで作成することもできます。

```csharp
Frame defaultFrame = new Frame
{
    Content = new Label { Text = "Example" }
};
```

`Frame`XAML でプロパティを設定することにより、角が丸く、色分けされた境界、ドロップシャドウでオブジェクトをカスタマイズできます。 次の例は、カスタマイズ`Frame`されたオブジェクトを示しています。

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

クラス`Frame`は、から`ContentView`継承されます。これは、オブジェクト`View`を含む`Layout`任意の型のオブジェクトを含むことができることを意味します。 この機能により`Frame` 、を使用して、カードなどの複雑な UI オブジェクトを作成できます。

### <a name="create-a-card-with-a-frame"></a>フレームを含むカードを作成する

オブジェクトをオブジェクトなどの`Layout`オブジェクト`StackLayout`と組み合わせることで、より複雑な UI を作成できます。 `Frame` 次のスクリーンショットは、 `Frame`オブジェクトを使用して作成されたカードの例を示しています。

["フレームで作成されたカードのスクリーンショット" フレームで作成されたカードのスクリーンショット![](frame-images/frame-card-cropped.png)](frame-images/frame-full.png#lightbox "")

次の XAML は、クラスを使用し`Frame`てカードを作成する方法を示しています。

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

コントロールのプロパティを使用して`CornerRadius` 、円イメージを作成できます。 `Frame` 次のスクリーンショットは、 `Frame`オブジェクトを使用して作成された丸い画像の例を示しています。

["フレームで作成された円イメージのスクリーンショット" フレームで作成された円イメージのスクリーンショット![](frame-images/circle-image-cropped.png)](frame-images/frame-full.png#lightbox "")

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
> 角を丸くすると、プラットフォームによって動作が若干異なります。 オブジェクトの`Margin`は、イメージの幅と親フレームの幅の差の半分である必要があります。また、 `Frame`オブジェクト内でイメージを均等に中央揃えにするには、負の値にする必要があります。 `Image` ただし、要求された幅と高さは保証され`Margin`ない`HeightRequest`ため`WidthRequest` 、、、およびの各プロパティは、イメージのサイズやその他のレイアウトの選択肢に基づいて変更する必要があります。

## <a name="related-links"></a>関連リンク

* [フレームのデモ](https://docs.microsoft.com/en-us/samples/xamarin/xamarin-forms-samples/userinterface-frame/)
* [Xamarin 形式の画像](~/xamarin-forms/user-interface/images.md)
