---
title: ピンチ ジェスチャ認識エンジンの追加
description: この記事では、ピンチ ジェスチャを使用して、ピンチ場所の画像に対して対話型のズームを実行する方法について説明します。
ms.prod: xamarin
ms.assetid: 832F7810-F0CF-441A-B04A-3975F3FB8B29
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: be7a145e93aa4720b38921efc895ca3f3f33edb3
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656028"
---
# <a name="adding-a-pinch-gesture-recognizer"></a>ピンチ ジェスチャ認識エンジンの追加

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-pinchgesture)

"_ピンチ ジェスチャは対話型のズームを実行するために使用され、PinchGestureRecognizer クラスを使用して実装されます。ピンチ ジェスチャの一般的なシナリオは、ピンチ場所の画像に対して対話型のズームを実行することです。これは、ビューポートのコンテンツを拡大縮小することによって実現され、この記事でその方法について説明します。_ "

ユーザー インターフェース要素をピンチ ジェスチャを使用してズームできるようにするには、[`PinchGestureRecognizer`](xref:Xamarin.Forms.PinchGestureRecognizer) インスタンスを作成し、[`PinchUpdated`](xref:Xamarin.Forms.PinchGestureRecognizer.PinchUpdated) イベントを処理し、新しいジェスチャ認識エンジンをユーザー インターフェイス要素の [`GestureRecognizers`](xref:Xamarin.Forms.View.GestureRecognizers) コレクションに追加します。 次に示すのは、[`Image`](xref:Xamarin.Forms.Image) 要素に関連付けられている `PinchGestureRecognizer` のコード例です。

```csharp
var pinchGesture = new PinchGestureRecognizer();
pinchGesture.PinchUpdated += (s, e) => {
  // Handle the pinch
};
image.GestureRecognizers.Add(pinchGesture);
```

次のコード例に示すように、XAML でもこれを実現できます。

```xaml
<Image Source="waterfront.jpg">
  <Image.GestureRecognizers>
    <PinchGestureRecognizer PinchUpdated="OnPinchUpdated" />
  </Image.GestureRecognizers>
</Image>
```

`OnPinchUpdated` イベント ハンドラー用のコードが分離コード ファイルに追加されます。

```csharp
void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
{
  // Handle the pinch
}
```

## <a name="creating-a-pinchtozoom-container"></a>PinchToZoom コンテナーの作成

ズーム操作を実行するためのピンチ ジェスチャの処理では、ユーザー インターフェイスを変換するための多少の計算が必要です。 このセクションには、この計算を実行するための汎用化されたヘルパー クラスが含まれています。それを使用して、任意のユーザー インターフェイス要素を対話的にズームできます。 次に示すのは、`PinchToZoomContainer` クラスのコード例です。

```csharp
public class PinchToZoomContainer : ContentView
{
  ...

  public PinchToZoomContainer ()
  {
    var pinchGesture = new PinchGestureRecognizer ();
    pinchGesture.PinchUpdated += OnPinchUpdated;
    GestureRecognizers.Add (pinchGesture);
  }

  void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
  {
    ...
  }
}
```

このクラスでユーザー インターフェイス要素をラップして、ラップされたユーザー インターフェイス要素がピンチ ジェスチャでズームされるようにすることができます。 次に示すのは、[`Image`](xref:Xamarin.Forms.Image) 要素をラップしている `PinchToZoomContainer` の XAML のコード例です。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:PinchGesture;assembly=PinchGesture"
             x:Class="PinchGesture.HomePage">
    <ContentPage.Content>
        <Grid Padding="20">
            <local:PinchToZoomContainer>
                <local:PinchToZoomContainer.Content>
                    <Image Source="waterfront.jpg" />
                </local:PinchToZoomContainer.Content>
            </local:PinchToZoomContainer>
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

次に示すのは、`PinchToZoomContainer` で C# ページ内の [`Image`](xref:Xamarin.Forms.Image) 要素をラップする方法のコード例です。

```csharp
public class HomePageCS : ContentPage
{
  public HomePageCS ()
  {
    Content = new Grid {
      Padding = new Thickness (20),
      Children = {
        new PinchToZoomContainer {
          Content = new Image { Source = ImageSource.FromFile ("waterfront.jpg") }
        }
      }
    };
  }
}
```

[`Image`](xref:Xamarin.Forms.Image) 要素がピンチ ジェスチャを受け取ると、表示されている画像が拡大または縮小されます。次のコード例に示すように、`PinchZoomContainer.OnPinchUpdated` メソッドによってズームが実行されます。

```csharp
void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
{
  if (e.Status == GestureStatus.Started) {
    // Store the current scale factor applied to the wrapped user interface element,
    // and zero the components for the center point of the translate transform.
    startScale = Content.Scale;
    Content.AnchorX = 0;
    Content.AnchorY = 0;
  }
  if (e.Status == GestureStatus.Running) {
    // Calculate the scale factor to be applied.
    currentScale += (e.Scale - 1) * startScale;
    currentScale = Math.Max (1, currentScale);

    // The ScaleOrigin is in relative coordinates to the wrapped user interface element,
    // so get the X pixel coordinate.
    double renderedX = Content.X + xOffset;
    double deltaX = renderedX / Width;
    double deltaWidth = Width / (Content.Width * startScale);
    double originX = (e.ScaleOrigin.X - deltaX) * deltaWidth;

    // The ScaleOrigin is in relative coordinates to the wrapped user interface element,
    // so get the Y pixel coordinate.
    double renderedY = Content.Y + yOffset;
    double deltaY = renderedY / Height;
    double deltaHeight = Height / (Content.Height * startScale);
    double originY = (e.ScaleOrigin.Y - deltaY) * deltaHeight;

    // Calculate the transformed element pixel coordinates.
    double targetX = xOffset - (originX * Content.Width) * (currentScale - startScale);
    double targetY = yOffset - (originY * Content.Height) * (currentScale - startScale);

    // Apply translation based on the change in origin.
    Content.TranslationX = targetX.Clamp (-Content.Width * (currentScale - 1), 0);
    Content.TranslationY = targetY.Clamp (-Content.Height * (currentScale - 1), 0);

    // Apply scale factor.
    Content.Scale = currentScale;
  }
  if (e.Status == GestureStatus.Completed) {
    // Store the translation delta's of the wrapped user interface element.
    xOffset = Content.TranslationX;
    yOffset = Content.TranslationY;
  }
}
```

このメソッドにより、ユーザーのピンチ ジェスチャに基づいて、ラップされたユーザー インターフェイス要素のズーム レベルが更新されます。 これは、[`PinchGestureUpdatedEventArgs`](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs) インスタンスの [`Scale`](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.Scale)、[`ScaleOrigin`](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.ScaleOrigin)、および [`Status`](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.Status) の各プロパティを使用して、ピンチ ジェスチャの原点に適用される倍率を計算することで実現されます。 [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX)、[`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) および [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) の各プロパティが計算済みの値に設定されることで、ピンチ ジェスチャの原点にあるラップされたユーザー要素がズームされます。

## <a name="related-links"></a>関連リンク

- [PinchGesture (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-pinchgesture)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [PinchGestureRecognizer](xref:Xamarin.Forms.PinchGestureRecognizer)
