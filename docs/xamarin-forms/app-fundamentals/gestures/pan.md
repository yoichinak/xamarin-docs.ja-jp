---
title: パン ジェスチャ認識エンジンを追加します。
description: この記事では、イメージのサイズより小さいビューポートに表示されているすべてのイメージ コンテンツ表示できるように、水平方向にパン ジェスチャを使用して、イメージを垂直方向にパンする方法が説明します。
ms.prod: xamarin
ms.assetid: 42CBD2CF-432D-4F19-A05E-D569BB7F8713
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: 59e9f4c61bda86faa5a55d70ef91411adb14da6d
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/18/2018
ms.locfileid: "38996807"
---
# <a name="adding-a-pan-gesture-recognizer"></a>パン ジェスチャ認識エンジンを追加します。

_パン ジェスチャが画面の周りの指の動きを検出し、コンテンツを移動操作を適用するためし、は実装されて、`PanGestureRecognizer`クラス。パン ジェスチャの一般的なシナリオは、イメージのサイズより小さいビューポートに表示されているすべてのイメージ コンテンツ表示できるように、イメージを水平および垂直にパンします。これは、ビューポート内のイメージを移動することによって実現されますであり、この記事で説明が。_

ユーザー インターフェイス要素をパン ジェスチャの移動を可能にするために、作成、 [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer)インスタンス、処理、 [ `PanUpdated` ](xref:Xamarin.Forms.PanGestureRecognizer.PanUpdated)イベント、する新しいジェスチャ認識エンジンを追加し、 [`GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers)ユーザー インターフェイスの要素のコレクション。 次のコード例は、`PanGestureRecognizer`にアタッチされている、 [ `Image` ](xref:Xamarin.Forms.Image)要素。

```csharp
var panGesture = new PanGestureRecognizer();
panGesture.PanUpdated += (s, e) => {
  // Handle the pan
};
image.GestureRecognizers.Add(panGesture);
```

これ行うこともできます、XAML で次のコード例に示すようにします。

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
> Android で正しいパンが必要です、 [Xamarin.Forms 2.1.0-pre1 NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Forms/2.1.0.6501-pre1)には、少なくともします。

## <a name="creating-a-pan-container"></a>パン コンテナーを作成します。

このセクションには通常、イメージまたはマップ内を移動するために適していますが、自由形式パンを実行する汎用化されたヘルパー クラスが含まれています。 この操作を実行するパン ジェスチャの処理には、ユーザー インターフェイスを変換するいくつかの計算が必要です。 この数式は、ラップされたユーザー インターフェイス要素の境界内のみのパン操作に使用されます。 次に示すのは、`PanContainer` クラスのコード例です。

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

このクラスは、ジェスチャは、ラップされたユーザー インターフェイス要素をパンできるように、ユーザー インターフェイス要素を囲むラップできます。 次の XAML コード例は、`PanContainer`の折り返し、 [ `Image` ](xref:Xamarin.Forms.Image)要素。

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

次のコード例に示す方法、`PanContainer`ラップ、 [ `Image` ](xref:Xamarin.Forms.Image) c# ページ内の要素。

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

どちらの例で、 [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest)と[ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest)プロパティが表示されるイメージの幅と高さの値に設定されます。

ときに、 [ `Image` ](xref:Xamarin.Forms.Image)要素は、パン ジェスチャを受け取り、表示されるイメージが方向のパンします。 パンが実行される、`PanContainer.OnPanUpdated`メソッドは、次のコード例に示されています。

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

このメソッドは、ユーザーのパン ジェスチャに基づき、ラップされたユーザー インターフェイス要素の表示可能なコンテンツを更新します。 値を使用してこれは、 [ `TotalX` ](xref:Xamarin.Forms.PanUpdatedEventArgs.TotalX)と[ `TotalY` ](xref:Xamarin.Forms.PanUpdatedEventArgs.TotalY)のプロパティ、 [ `PanUpdatedEventArgs` ](xref:Xamarin.Forms.PanUpdatedEventArgs)方向を計算するインスタンスとパンの距離です。 `App.ScreenWidth`と`App.ScreenHeight`プロパティは、ビューポートの幅と高さを提供し、それぞれのプラットフォーム固有プロジェクトで、画面の幅とデバイスの画面の高さの値に設定されます。 ラップされたユーザーの要素が設定でパンしその[ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX)と[ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY)プロパティの値を計算します。

ビューポートの幅と高さを要素から取得できますと全画面表示を占有しない要素のコンテンツをパン[ `Height` ](xref:Xamarin.Forms.VisualElement.Height)と[ `Width` ](xref:Xamarin.Forms.VisualElement.Width)プロパティ。

> [!NOTE]
> 高解像度のイメージを表示すると、アプリのメモリ フット プリントが大幅に向上します。 そのため、する必要がありますのみ作成する必要が必要し、不要になったアプリで必要とすぐに解放する必要があります。 詳細については、「[イメージ リソースを最適化する](~/xamarin-forms/deploy-test/performance.md#optimizeimages)」を参照してください。

## <a name="related-links"></a>関連リンク

- [PanGesture (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PanGesture/)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [PanGestureRecognizer](xref:Xamarin.Forms.PanGestureRecognizer)
