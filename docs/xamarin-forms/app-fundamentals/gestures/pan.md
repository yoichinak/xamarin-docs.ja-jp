---
title: パン ジェスチャ認識エンジンを追加する
description: この記事では、パン ジェスチャを使用してイメージを水平方向や垂直方向にパンし、イメージをそのイメージのディメンションより小さいビューポートで表示するときに、そのコンテンツがすべて表示されるようにする方法について説明します。
ms.prod: xamarin
ms.assetid: 42CBD2CF-432D-4F19-A05E-D569BB7F8713
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8c9968ac7de45f3e6e239f82ba43b53abd05a647
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91558519"
---
# <a name="add-a-pan-gesture-recognizer"></a>パン ジェスチャ認識エンジンを追加する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-pangesture)

_パン ジェスチャは、画面周辺の指の動きを検出し、その動きをコンテンツに適用するために使用され、`PanGestureRecognizer` クラスを使用して実装されます。パン ジェスチャの一般的なシナリオは、イメージを水平方向や垂直方向にパンし、イメージをそのイメージのディメンションより小さいビューポートで表示するときに、そのコンテンツがすべて表示されるようにすることです。これは、イメージをビューポート内で移動することによって実現され、この記事でその方法について説明します。_

ユーザー インターフェース要素をパン ジェスチャを使用して移動できるようにするには、[`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer) インスタンスを作成し、[`PanUpdated`](xref:Xamarin.Forms.PanGestureRecognizer.PanUpdated) イベントを処理し、新しいジェスチャ認識エンジンをユーザー インターフェイス要素の [`GestureRecognizers`](xref:Xamarin.Forms.View.GestureRecognizers) コレクションに追加します。 次に示すのは、[`Image`](xref:Xamarin.Forms.Image) 要素に関連付けられている `PanGestureRecognizer` のコード例です。

```csharp
var panGesture = new PanGestureRecognizer();
panGesture.PanUpdated += (s, e) => {
  // Handle the pan
};
image.GestureRecognizers.Add(panGesture);
```

次のコード例に示すように、XAML でもこれを実現できます。

```xaml
<Image Source="MonoMonkey.jpg">
  <Image.GestureRecognizers>
    <PanGestureRecognizer PanUpdated="OnPanUpdated" />
  </Image.GestureRecognizers>
</Image>
```

`OnPanUpdated` イベント ハンドラー用のコードが分離コード ファイルに追加されます。

```csharp
void OnPanUpdated (object sender, PanUpdatedEventArgs e)
{
  // Handle the pan
}
```

## <a name="creating-a-pan-container"></a>パン コンテナーの作成

このセクションにはフリーフォームのパンを実行する汎用のへルパー メソッドが含まれており、一般的に画像内やマップ内の移動に適しています。 この操作を実行するためのパン ジェスチャの処理では、ユーザー インターフェイスを変換するための多少の計算が必要です。 この計算はラップされたユーザー インターフェイス要素の境界内のパンにのみ使用されます。 次に示すのは、`PanContainer` クラスのコード例です。

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

このクラスでユーザー インターフェイス要素をラップして、ラップされたユーザー インターフェイス要素がそのジェスチャでパンされるようにすることができます。 次に示すのは、[`Image`](xref:Xamarin.Forms.Image) 要素をラップしている `PanContainer` の XAML のコード例です。

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

次に示すのは、`PanContainer` で C# ページ内の [`Image`](xref:Xamarin.Forms.Image) 要素をラップする方法のコード例です。

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

両方の例で、[`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) と [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) の各プロパティは表示されるイメージの高さと幅の値に設定されます。

[`Image`](xref:Xamarin.Forms.Image) 要素がパン ジェスチャを受け取ると、表示されているイメージがパンされます。 次のコード例に示すように、`PanContainer.OnPanUpdated` メソッドによってパンが実行されます。

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

このメソッドにより、ユーザーのパン ジェスチャに基づいて、ラップされたユーザー インターフェイス要素の表示可能コンテンツが更新されます。 これは、[`PanUpdatedEventArgs`](xref:Xamarin.Forms.PanUpdatedEventArgs) インスタンスの [`TotalX`](xref:Xamarin.Forms.PanUpdatedEventArgs.TotalX) と [`TotalY`](xref:Xamarin.Forms.PanUpdatedEventArgs.TotalY) の各プロパティを使用して、パンの方向と距離を計算します。 `App.ScreenWidth` と `App.ScreenHeight` の各プロパティはそのビューポートの高さと幅を提供し、プラットフォーム固有の各プロジェクトによってそのデバイスの画面の幅と高さの値に設定されます。 [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) と [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) の各プロパティが計算済みの値に設定されることで、ラップされたユーザー要素がパンされます。

画面全体を占拠しない要素内でコンテンツをパンすると、そのビューポートの高さと幅はその要素の [`Height`](xref:Xamarin.Forms.VisualElement.Height) と [`Width`](xref:Xamarin.Forms.VisualElement.Width) の各プロパティから取得できます。

> [!NOTE]
> 高解像度のイメージを表示すると、アプリのメモリの占有領域が大幅に増える場合があります。 そのため、必要な場合にのみ作成し、アプリで不要になったらすぐに解放する必要があります。 詳細については、「[イメージ リソースを最適化する](~/xamarin-forms/deploy-test/performance.md#optimize-image-resources)」を参照してください。

## <a name="related-links"></a>関連リンク

- [PanGesture (サンプル)](/samples/xamarin/xamarin-forms-samples/workingwithgestures-pangesture)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [PanGestureRecognizer](xref:Xamarin.Forms.PanGestureRecognizer)