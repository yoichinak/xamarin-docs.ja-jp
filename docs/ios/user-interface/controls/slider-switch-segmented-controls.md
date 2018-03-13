---
title: "スライダー、スイッチ、およびセグメント化されたコントロール"
ms.topic: article
ms.prod: xamarin
ms.assetid: 85BF0EC8-E581-49CD-B9E7-98BE4C5A0F6B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 282a4cb59545703c5172f8747cb5b633e7b648dc
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="sliders-switches-and-segmented-controls"></a>スライダー、スイッチ、およびセグメント化されたコントロール

<a name="Sliders" />


## <a name="sliders"></a>スライダー

スライダー コントロールでは、数値の範囲内の単純な選択ができます。 コントロールの既定値は 0 ~ 1 の値が、これらの制限をカスタマイズすることができます。

 [![](slider-switch-segmented-controls-images/image25a.png "スライダー")](slider-switch-segmented-controls-images/image25a.png#lightbox)

次のスクリーン ショットは、デザイナーで編集可能なプロパティを示します。

 [![](slider-switch-segmented-controls-images/image26a.png "スライダーのプロパティ")](slider-switch-segmented-controls-images/image25a.png#lightbox)

現在選択されている値を表示するハンドラーを配線を含め、次のように、コードでこれらの値を設定することができます、`UILabel`コントロール。

```csharp
slider1.MinValue = -1;
slider1.MaxValue = 2;
slider1.Value = 0.5f; // the current value
slider1.ValueChanged += (sender,e) => label1.Text = ((UISlider)sender).Value.ToString ();
```

設定によって、スライダーの外観をカスタマイズすることもできます。

```csharp
slider1.ThumbTintColor = UIColor.Blue;
slider1.MinimumTrackTintColor = UIColor.Gray;
slider1.MaximumTrackTintColor = UIColor.Green;
```

次のようなカスタマイズのスライダーが表示。

 [![](slider-switch-segmented-controls-images/image27a.png "カスタムのスライダー")](slider-switch-segmented-controls-images/image28a.png#lightbox)

> [!IMPORTANT]
> 現在、[バグ](http://stackoverflow.com/a/19496179)原因、`ThumbTint`に期待どおりに実行時に表示されません。 次のコード行を追加する**する前に**回避策としては、上記のコード。 [[ソース](http://stackoverflow.com/a/21396794)]。
>
> `slider1.SetThumbImage(UIImage.FromBundle("thumb.png"),UIControlState.Normal);`
> 
> 任意のイメージを使用するには、無効になり、いるものの、配置されていることを確認_で_Resources ディレクトリと、コード内と呼びます。

<a name="Switch" />

## <a name="switch"></a>切り替え

iOS を使用して、`UISwitch`ブール値の入力によって他のプラットフォームでラジオ ボタンを表すことができます。 ユーザーが移動することによって、コントロールを操作したり、 *thumb*間、**オン/オフ**位置。

 [![](slider-switch-segmented-controls-images/image28a.png "Switch")](slider-switch-segmented-controls-images/image28a.png#lightbox)

スイッチの外観をカスタマイズすることができます、**プロパティ パッド**既定の状態を制御することが、デザイナーの**オン/オフ濃淡**色と**On/Off イメージ**. これは次の図に示します。

 [![](slider-switch-segmented-controls-images/image29a.png "スイッチのプロパティ")](slider-switch-segmented-controls-images/image29a.png#lightbox)

コードで、スイッチのプロパティを設定することができますも、次のコードが既定値は、スイッチを表示する例を示します`On`:

```csharp
switch1.On = true;
```

 <a name="Segmented_Controls" />


## <a name="segmented-controls"></a>セグメント化されたコントロール

セグメント化されたコントロールは、オプションの数が少ない対話をユーザーに使用できるように整理された方法です。 レイアウト水平方向にし、各セグメントは、個別のボタンとして機能します。 デザイナーを使用する場合、セグメント化されたコントロールは「**ツールボックス > コントロール**、し、次の図のようになります。

 [![](slider-switch-segmented-controls-images/segmentedcontrol.png "セグメント化されたコントロール")](slider-switch-segmented-controls-images/segmentedcontrol.png#lightbox)

デザイナーの固有の機能は、以下に示すように、デザイン サーフェイスでは、個別に選択するのには、各セグメントのにより。

 [![](slider-switch-segmented-controls-images/segmentedcontrolselection.png "セグメント化されたコントロール")](slider-switch-segmented-controls-images/segmentedcontrolselection.png#lightbox)

これにより、各セグメントのプロパティをより正確に制御するために使用するプロパティのパッド。 次のスクリーン ショットの編集可能なプロパティを確認できます。

 [![](slider-switch-segmented-controls-images/segmentedcontrolproperties.png "セグメント化されたコントロール")](slider-switch-segmented-controls-images/segmentedcontrolproperties.png#lightbox)

セグメント化されたコントロールのスタイルは、ios 7 で廃止されており、そのため、ios 7 アプリケーションでこのオプションを調整することは効果がなくことに注意してください。

## <a name="related-links"></a>関連リンク

- [コントロール (サンプル)](https://developer.xamarin.com/samples/Controls/)
- [アラートのコント ローラー](https://developer.xamarin.com/recipes/ios/standard_controls/alertcontroller/)
