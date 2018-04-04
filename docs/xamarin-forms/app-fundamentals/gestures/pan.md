---
title: パン ジェスチャ レコグナイザーを追加します。
description: パン ジェスチャでは、ドラッグすることを検出するために使用され、PanGestureRecognizer クラスに実装されます。 パン ジェスチャの一般的なシナリオは水平方向および垂直方向にドラッグ イメージようにするイメージの大きさよりも小さいビューポートに表示されているときに、すべてのイメージのコンテンツ表示できます。 これは、ビューポート内の画像を移動することによって実現し、は、この記事で説明します。
ms.prod: xamarin
ms.assetid: 42CBD2CF-432D-4F19-A05E-D569BB7F8713
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: ed38f7ace9e11b009aae768cda2d4af0f36c337e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="adding-a-pan-gesture-recognizer"></a>パン ジェスチャ レコグナイザーを追加します。

_パン ジェスチャでは、ドラッグすることを検出するために使用され、PanGestureRecognizer クラスに実装されます。パン ジェスチャの一般的なシナリオは水平方向および垂直方向にドラッグ イメージようにするイメージの大きさよりも小さいビューポートに表示されているときに、すべてのイメージのコンテンツ表示できます。これは、ビューポート内の画像を移動することによって実現し、は、この記事で説明します。_

## <a name="overview"></a>概要

ユーザー インターフェイス要素をパン ジェスチャにドラッグするためには、作成、 [ `PanGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PanGestureRecognizer/)インスタンス、処理、 [ `PanUpdated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.PanGestureRecognizer.PanUpdated/)イベントを新しいジェスチャ レコグナイザーを追加し、 [`GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/)ユーザー インターフェイス要素のコレクション。 次のコード例は、`PanGestureRecognizer`にアタッチされている、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)要素。

```csharp
var panGesture = new PanGestureRecognizer();
panGesture.PanUpdated += (s, e) => {
  // Handle the pan
};
image.GestureRecognizers.Add(panGesture);
```

またこれを行う XAML では、次のコード例に示すようにします。

```xaml
<Image Source="MonoMonkey.jpg">
  <Image.GestureRecognizers>
    <PanGestureRecognizer PanUpdated="OnPanUpdated" />
  </Image.GestureRecognizers>
</Image>
```

コードを`OnPanUpdated`イベント ハンドラーは、分離コード ファイルに追加されます。

```csharp
void OnPanUpdated (object sender, PanUpdatedEventArgs e)
{
  // Handle the pan
}
```

> [!NOTE]
> Android で正しいパン必要があります、 [Xamarin.Forms 2.1.0-pre1 NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Forms/2.1.0.6501-pre1)最低限です。

## <a name="creating-a-pan-container"></a>パン コンテナーを作成します。

このセクションには、通常に適したイメージまたはマップ内でのナビゲート自由形式のパンを実行する汎用化されたヘルパー クラスが含まれています。 ドラッグ操作を実行するパン ジェスチャの処理には、いくつかの数学、ユーザー インターフェイスを変換する必要があります。 この数式を使用して、ラップされたユーザー インターフェイス要素の境界内でのみをドラッグします。 次に示すのは、`PanContainer` クラスのコード例です。

```csharp
public class PanContainer : ContentView
{
  double x, y;

  public PanContainer ()
  {
    // Set PanGestureRecognizer.TouchPoints to control the
    // number of touch points needed to pan
    var panGesture = new PanGestureRecognizer ();
    panGesture.PanUpdated += OnPanUpdated;
    GestureRecognizers.Add (panGesture);
  }

  void OnPanUpdated (object sender, PanUpdatedEventArgs e)
  {
    ...
  }
}
```

このクラスは、パン ジェスチャは、ラップされたユーザー インターフェイス要素をドラッグできるように、ユーザー インターフェイス要素を囲むラップできます。 XAML コード例を次に、`PanContainer`ラッピング、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)要素。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:PanGesture"
             x:Class="PanGesture.HomePage">
    <ContentPage.Content>
        <AbsoluteLayout>
            <local:PanContainer>
                <Image Source="MonoMonkey.jpg" WidthRequest="1024" HeightRequest="768" />
            </local:PanContainer>
        </AbsoluteLayout>
    </ContentPage.Content>
</ContentPage>
```

次のコード例に示す方法、`PanContainer`ラップ、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) c# ページ内の要素。

```csharp
public class HomePageCS : ContentPage
{
  public HomePageCS ()
  {
    Content = new AbsoluteLayout {
      Padding = new Thickness (20),
      Children = {
        new PanContainer {
          Content = new Image {
            Source = ImageSource.FromFile ("MonoMonkey.jpg"),
            WidthRequest = 1024,
            HeightRequest = 768
          }
        }
      }
    };
  }
}
```

どちらの例で、 [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/)と[ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/)プロパティが表示されるイメージの幅と高さの値に設定されます。

ときに、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)要素は、パン ジェスチャを受け取り、表示されるイメージをドラッグします。 によって、ドラッグを実行、`PanContainer.OnPanUpdated`メソッドは、次のコード例に示されています。

```csharp
void OnPanUpdated (object sender, PanUpdatedEventArgs e)
{
  switch (e.StatusType) {
  case GestureStatus.Running:
    // Translate and ensure we don't pan beyond the wrapped user interface element bounds.
    Content.TranslationX =
      Math.Max (Math.Min (0, x + e.TotalX), -Math.Abs (Content.Width - App.ScreenWidth));
    Content.TranslationY =
      Math.Max (Math.Min (0, y + e.TotalY), -Math.Abs (Content.Height - App.ScreenHeight));
    break;

  case GestureStatus.Completed:
    // Store the translation applied during the pan
    x = Content.TranslationX;
    y = Content.TranslationY;
    break;
  }
}
```

このメソッドは、ユーザーのパン ジェスチャに基づく、ラップされたユーザー インターフェイス要素の表示可能なコンテンツを更新します。 値を使用して、これは、 [ `TotalX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PanUpdatedEventArgs.TotalX/)と[ `TotalY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PanUpdatedEventArgs.TotalY/)のプロパティ、 [ `PanUpdatedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PanUpdatedEventArgs/)方向を計算するインスタンスとパンの距離です。 `App.ScreenWidth`と`App.ScreenHeight`プロパティが、ビューポートの幅と高さを指定し、それぞれのプラットフォーム固有のプロジェクトで、画面の幅とデバイスの画面の高さの値に設定されています。 設定して、ラップされたユーザー要素をドラッグし、その[ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/)と[ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/)プロパティを計算される値。

ビューポートの幅と高さを要素から取得できますと全画面表示を占有しない要素のコンテンツをパン、 [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/)と[ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/)プロパティです。

> [!NOTE]
> 高解像度のイメージを表示すると、アプリのメモリ使用量が大幅に向上できます。 そのため、必要がありますのみ作成する必要が必須であり、不要になったアプリで必要とすぐに解放する必要があるとします。 詳細については、「[イメージ リソースを最適化する](~/xamarin-forms/deploy-test/performance.md#optimizeimages)」を参照してください。

## <a name="summary"></a>まとめ

パン ジェスチャをドラッグすることを検出するために使用し、使用して実装されて、 [ `PanGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PanGestureRecognizer/)クラスです。



## <a name="related-links"></a>関連リンク

- [PanGesture (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PanGesture/)
- [GestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/)
- [PanGestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.PanGestureRecognizer/)
