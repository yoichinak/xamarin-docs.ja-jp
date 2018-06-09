---
title: ピンチ ジェスチャ レコグナイザーを追加します。
description: この記事では、ピンチ ジェスチャを使用して、ピンチ操作での場所にイメージの対話のズームを実行する方法について説明します。
ms.prod: xamarin
ms.assetid: 832F7810-F0CF-441A-B04A-3975F3FB8B29
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: 3600a8bf059bf29429cce35a233cc6618daa4d79
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241778"
---
# <a name="adding-a-pinch-gesture-recognizer"></a>ピンチ ジェスチャ レコグナイザーを追加します。

_ピンチ ジェスチャでは、対話型のズームを実行するために使用され、PinchGestureRecognizer クラスに実装されます。ピンチ ジェスチャの一般的なシナリオでは、ピンチ操作での場所にイメージの対話のズームを実行します。これは、ビューポートのコンテンツをスケーリングして行われますであり、この記事で示されます。_

## <a name="overview"></a>概要

ユーザー インターフェイス要素ピンチ ジェスチャとズーム可能な作成、 [ `PinchGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureRecognizer/)インスタンス、処理、 [ `PinchUpdated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.PinchGestureRecognizer.PinchUpdated/)イベントを新しいジェスチャ レコグナイザーを追加し、 [`GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/)ユーザー インターフェイス要素のコレクション。 次のコード例は、`PinchGestureRecognizer`にアタッチされている、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)要素。

```csharp
var pinchGesture = new PinchGestureRecognizer();
pinchGesture.PinchUpdated += (s, e) => {
  // Handle the pinch
};
image.GestureRecognizers.Add(pinchGesture);
```

またこれを行う XAML では、次のコード例に示すようにします。

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

ズーム操作を実行するピンチ ジェスチャの処理には、いくつかの数学、ユーザー インターフェイスを変換する必要があります。 このセクションには、対話形式で任意のユーザー インターフェイス要素を拡大するために使用する数値演算を実行する汎用化されたヘルパー クラスが含まれています。 次に示すのは、`PinchToZoomContainer` クラスのコード例です。

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

このクラスは、ピンチ ジェスチャがラップされたユーザー インターフェイス要素を拡大表示できるように、ユーザー インターフェイス要素を囲むラップできます。 XAML コード例を次に、`PinchToZoomContainer`ラッピング、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)要素。

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

次のコード例に示す方法、`PinchToZoomContainer`ラップ、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) c# ページ内の要素。

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

ときに、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)要素はピンチ ジェスチャを受け取ると、表示されるイメージがあるズーム インまたは無効にします。ズームは、`PinchZoomContainer.OnPinchUpdated`メソッドは、次のコード例に示されています。

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

このメソッドは、ユーザーのピンチ ジェスチャに基づいてラップされたユーザー インターフェイス要素のズーム レベルを更新します。 値を使用して、これは、 [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PinchGestureUpdatedEventArgs.Scale/)、 [ `ScaleOrigin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PinchGestureUpdatedEventArgs.ScaleOrigin/)と[ `Status` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PinchGestureUpdatedEventArgs.Status/)のプロパティ、 [ `PinchGestureUpdatedEventArgs`](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureUpdatedEventArgs/)ピンチ ジェスチャの原点を適用するスケール ファクターを計算するインスタンス。 ラップされたユーザー要素が、設定してピンチ ジェスチャの原点を拡大表示されて、その[ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/)、 [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/)、および[ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/)計算結果の値プロパティです。

## <a name="summary"></a>まとめ

ピンチ ジェスチャは対話型のズームを実行するために使用し、使用して実装されて、 [ `PinchGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureRecognizer/)クラスです。


## <a name="related-links"></a>関連リンク

- [PinchGesture (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PinchGesture/)
- [GestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/)
- [PinchGestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureRecognizer/)
