---
title: ピンチ ジェスチャ認識エンジンを追加します。
description: この記事では、ピンチ ジェスチャを使用して、対話型ズームのピンチ場所にイメージを実行する方法について説明します。
ms.prod: xamarin
ms.assetid: 832F7810-F0CF-441A-B04A-3975F3FB8B29
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: 37befdcd4ccbcd49e3cebda92d55ae6f70da2ad6
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998703"
---
# <a name="adding-a-pinch-gesture-recognizer"></a>ピンチ ジェスチャ認識エンジンを追加します。

_ピンチ ジェスチャでは、対話型ズームを実行するために使用し、PinchGestureRecognizer クラスを使用して実装されます。ピンチ ジェスチャの一般的なシナリオでは、ピンチ場所にイメージの対話型ズームを実行します。これは、ビューポートのコンテンツをスケールすることによって実現され、、この記事で説明します。_

## <a name="overview"></a>概要

ピンチ ジェスチャとズーム可能なユーザー インターフェイス要素をように、作成、 [ `PinchGestureRecognizer` ](xref:Xamarin.Forms.PinchGestureRecognizer)インスタンスを処理、 [ `PinchUpdated` ](xref:Xamarin.Forms.PinchGestureRecognizer.PinchUpdated)イベントに新しいジェスチャ レコグナイザーを追加し、 [`GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers)ユーザー インターフェイスの要素のコレクション。 次のコード例は、`PinchGestureRecognizer`にアタッチされている、 [ `Image` ](xref:Xamarin.Forms.Image)要素。

```csharp
var pinchGesture = new PinchGestureRecognizer();
pinchGesture.PinchUpdated += (s, e) => {
  // Handle the pinch
};
image.GestureRecognizers.Add(pinchGesture);
```

これ行うこともできます、XAML で次のコード例に示すようにします。

```xaml
<Image Source="waterfront.jpg">
  <Image.GestureRecognizers>
    <PinchGestureRecognizer PinchUpdated="OnPinchUpdated" />
  </Image.GestureRecognizers>
</Image>
```

コードを`OnPinchUpdated`イベント ハンドラーは、分離コード ファイルに追加されます。

```csharp
void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
{
  // Handle the pinch
}
```

## <a name="creating-a-pinchtozoom-container"></a>PinchToZoom コンテナーを作成します。

ズーム操作を実行するピンチ ジェスチャの処理には、ユーザー インターフェイスを変換するいくつかの計算が必要です。 このセクションには、対話形式で任意のユーザー インターフェイス要素を拡大するために使用できる計算を実行する汎用化されたヘルパー クラスが含まれています。 次に示すのは、`PinchToZoomContainer` クラスのコード例です。

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

このクラスは、指をつまむジェスチャがラップされたユーザー インターフェイス要素を拡大表示するため、ユーザー インターフェイス要素を囲むラップできます。 次の XAML コード例は、`PinchToZoomContainer`の折り返し、 [ `Image` ](xref:Xamarin.Forms.Image)要素。

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

次のコード例に示す方法、`PinchToZoomContainer`ラップ、 [ `Image` ](xref:Xamarin.Forms.Image) c# ページ内の要素。

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

ときに、 [ `Image` ](xref:Xamarin.Forms.Image)要素は、指をつまむジェスチャを受け取る、表示されるイメージが拡大表示または無効です。ズームは、`PinchZoomContainer.OnPinchUpdated`メソッドは、次のコード例に示されています。

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

このメソッドは、ユーザーの指をつまむジェスチャに基づいてラップされたユーザー インターフェイス要素のズーム レベルを更新します。 値を使用してこれは、 [ `Scale` ](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.Scale)、 [ `ScaleOrigin` ](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.ScaleOrigin)と[ `Status` ](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.Status)のプロパティ、 [ `PinchGestureUpdatedEventArgs`](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs)ピンチ ジェスチャの原点に適用されるスケール ファクターを計算するインスタンス。 設定して、ラップされたユーザーの要素が拡大ピンチ ジェスチャの原点を注視し、その[ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX)、 [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY)、および[ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale)プロパティの計算される値。

## <a name="summary"></a>まとめ

ピンチ ジェスチャは対話型ズームを実行するために使用され、は実装されて、 [ `PinchGestureRecognizer` ](xref:Xamarin.Forms.PinchGestureRecognizer)クラス。


## <a name="related-links"></a>関連リンク

- [PinchGesture (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PinchGesture/)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [PinchGestureRecognizer](xref:Xamarin.Forms.PinchGestureRecognizer)
