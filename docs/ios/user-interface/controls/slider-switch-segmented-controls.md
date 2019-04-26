---
title: スライダー、スイッチ、および Xamarin.iOS でセグメント付きコントロール
description: このドキュメントでは、スライド、スイッチ、およびプログラムと iOS デザイナーの両方に、それらを操作する方法を説明する Xamarin.iOS のセグメント化されたコントロールについて説明します。
ms.prod: xamarin
ms.assetid: 85BF0EC8-E581-49CD-B9E7-98BE4C5A0F6B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 2ed14752cc5906b68d277b4f492875f7e281b053
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61029976"
---
# <a name="sliders-switches-and-segmented-controls-in-xamarinios"></a>スライダー、スイッチ、および Xamarin.iOS でセグメント付きコントロール

<a name="Sliders" />

## <a name="sliders"></a>スライダー

スライダー コントロールには、数値の範囲内の単純な選択ができるようにします。 コントロールの既定値は 0 ~ 1 の値が、これらの制限をカスタマイズできます。

 [![](slider-switch-segmented-controls-images/image25a.png "スライダー")](slider-switch-segmented-controls-images/image25a.png#lightbox)

次のスクリーン ショットでは、デザイナーで編集できるプロパティを示します。

 [![](slider-switch-segmented-controls-images/image26a.png "スライダーのプロパティ")](slider-switch-segmented-controls-images/image25a.png#lightbox)

次に示す、現在選択されている値を表示するハンドラーを配線を含め、コードでこれらの値を設定できます、`UILabel`コントロール。

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

カスタマイズしたスライダーのようになります。

 [![](slider-switch-segmented-controls-images/image27a.png "カスタムのスライダー")](slider-switch-segmented-controls-images/image28a.png#lightbox)

> [!IMPORTANT]
> 現時点では、[バグ](https://stackoverflow.com/a/19496179)原因、`ThumbTint`期待どおりに実行時に表示されません。 次のコード行を追加する**する前に**回避策としては、上記のコード。 [[ソース](https://stackoverflow.com/a/21396794)]。
>
> `slider1.SetThumbImage(UIImage.FromBundle("thumb.png"),UIControlState.Normal);`
> 
> 任意のイメージを使用するには、無効になり、これが配置されているかどうかを確認するよう_で_Resources ディレクトリとは、コードで呼び出されます。

<a name="Switch" />

## <a name="switch"></a>切り替え

iOS を使用して、`UISwitch`ブール値の入力で他のプラットフォームでラジオ ボタンを表すことができます。 ユーザーは移動することによって、コントロールを操作することができます、 *thumb*間、**オン/オフ**位置。

 [![](slider-switch-segmented-controls-images/image28a.png "スイッチ")](slider-switch-segmented-controls-images/image28a.png#lightbox)

スイッチの外観をカスタマイズすることができます、 **Properties Pad**既定の状態を制御することが、デザイナーの**オン/オフの濃淡**色と**On/Off イメージ**. これは、次の図に示します。

 [![](slider-switch-segmented-controls-images/image29a.png "スイッチのプロパティ")](slider-switch-segmented-controls-images/image29a.png#lightbox)

スイッチのプロパティは、コードで設定することもできます、次のコードは、スイッチの既定値を表示する例`On`:

```csharp
switch1.On = true;
```

 <a name="Segmented_Controls" />


## <a name="segmented-controls"></a>セグメント付きコントロール

セグメント化されたコントロールは、オプションの数が少ない対話をユーザーに許可する適切な手段です。 水平方向に配置し、各セグメントは、個別のボタンとして機能します。 デザイナーを使用する場合、セグメント化されたコントロールは、「**ツールボックス > コントロール**、し、次の図のようになります。

 [![](slider-switch-segmented-controls-images/segmentedcontrol.png "セグメント化されたコントロール")](slider-switch-segmented-controls-images/segmentedcontrol.png#lightbox)

次のようにデザイン サーフェイスでは、個別に選択するのには、各セグメントのデザイナーの一意の機能を使用できます。

 [![](slider-switch-segmented-controls-images/segmentedcontrolselection.png "セグメント化されたコントロール")](slider-switch-segmented-controls-images/segmentedcontrolselection.png#lightbox)

これにより、各セグメントのプロパティをより正確に制御を使用するプロパティ パッドができます。 次のスクリーン ショットの編集可能なプロパティを確認できます。

 [![](slider-switch-segmented-controls-images/segmentedcontrolproperties.png "セグメント化されたコントロール")](slider-switch-segmented-controls-images/segmentedcontrolproperties.png#lightbox)

セグメント化されたコントロールのスタイルは iOS7、非推奨し、そのため、iOS7 アプリケーションでこのオプションを調整する効果はありませんに注意してください。

## <a name="related-links"></a>関連リンク

- [コントロール (サンプル)](https://developer.xamarin.com/samples/Controls/)
- [アラートのコント ローラー](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller)
