---
title: watchOS Xamarin でのイメージ コントロール
description: このドキュメントでは、Xamarin でビルドされた watchOS アプリケーション イメージ コントロールを使用する方法について説明します。 SetImage メソッドは、watch extension、アニメーション、画像の追加、WKInterfaceImage コントロールについて説明します。
ms.prod: xamarin
ms.assetid: B741C207-3427-46F3-9C90-A52BF8933FA4
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 6a2b8c99156963ae167aecd29a618d0feeffbdc7
ms.sourcegitcommit: 2713f2c1d74e3582704c3d0ca65b6651119ed489
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/15/2019
ms.locfileid: "56321130"
---
# <a name="watchos-image-controls-in-xamarin"></a>watchOS Xamarin でのイメージ コントロール

watchOS の提供、 [`WKInterfaceImage`](xref:WatchKit.WKInterfaceImage)イメージや単純なアニメーションを表示するコントロール。 一部のコントロール (ボタン、グループ、およびインターフェイス コント ローラー) などの背景画像をこともできます。

![](image-images/image-walkway.png "Apple Watch の画像の表示") ![](image-images/image-animation.png "単純なアニメーションを使用して、Apple Watch")
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

資産カタログのイメージを使用すると、ウォッチ キット アプリにイメージを追加できます。
のみ **@2x** Retina ディスプレイ デバイスをすべて見るために、バージョンが必要です。 

![](image-images/asset-universal-sml.png "Retina ディスプレイ デバイスをすべて見るために 2 つだけの x バージョンが必要です。")

イメージ自体がウォッチの適切なサイズであることを確認することをお勧めします。 *回避*サイズが不適切イメージ (特に大規模なもの) を使用して、ウォッチ上に表示するのにスケーリングします。

資産カタログ イメージでウォッチ キット サイズ (38 mm および 42 mm) を使用すると、表示サイズごとに異なるイメージを指定します。

![](image-images/asset-watch-sml.png "資産カタログ イメージで 38 mm および 42 mm ウォッチ キット サイズを使用する表示サイズごとに異なるイメージを指定するには")


## <a name="images-on-the-watch"></a>Watch でのイメージ

イメージを表示する最も効率的な方法は、する*watch アプリのプロジェクトに含める*し、表示を使用して、`SetImage(string imageName)`メソッド。

たとえば、 [WatchKitCatalog](https://developer.xamarin.com/samples/WatchKitCatalog/)サンプルでは、多数のイメージは、watch アプリ プロジェクト内のアセット カタログに追加します。

![](image-images/asset-whale-sml.png "WatchKitCatalog サンプルには、多数のイメージは、watch アプリ プロジェクト内のアセット カタログに追加")

これらは効率的に読み込まれ、watch を使用して、表示される`SetImage`文字列名パラメーターを使用。

```csharp
myImageControl.SetImage("Whale");
myOtherImageControl.SetImage("Worry");
```

### <a name="background-images"></a>背景画像

同じロジックが適用されます、`SetBackgroundImage (string imageName)`上、 `Button`、 `Group`、および`InterfaceController`クラス。 最適なパフォーマンスは、watch アプリ自体で、イメージを格納することによって実現されます。


## <a name="images-in-the-watch-extension"></a>イメージ ウォッチ拡張機能

に加えて、watch アプリ自体に格納されているイメージを読み込み、表示用の watch アプリに拡張機能のバンドルからイメージを送信することができます (または、リモートの場所からイメージをダウンロードし、それらを表示する可能性があります)。

作成するイメージ ウォッチ拡張機能からロードされ、`UIImage`をインスタンス化し、呼び出す`SetImage`で、`UIImage`オブジェクト。

たとえば、 [WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)サンプルという名前のイメージでは、 **Bumblebee**ウォッチ拡張プロジェクトで。

![](image-images/asset-bumblebee-sml.png "WatchKitCatalog サンプルには、ウォッチ拡張機能プロジェクトで Bumblebee をという名前のイメージ")

次のコードが発生します。

- メモリに読み込まれているイメージと
- watch で表示されます。

```csharp
using (var image = UIImage.FromBundle ("Bumblebee")) {
    myImageControl.SetImage (image);
}
```


## <a name="animations"></a>Animations

イメージのセットをアニメーション化するには、同じプレフィックスで始まるすべてと数値のサフィックスが付いてする必要があります。

[WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)サンプル watch アプリのプロジェクトに一連の番号付きのイメージには、 **Bus**プレフィックス。

![](image-images/asset-bus-animation-sml.png "WatchKitCatalog サンプルでは、バスのプレフィックスを持つ、watch アプリ プロジェクトの一連の番号付きのイメージ")

これらのイメージとしてアニメーションを表示する最初に読み込むイメージを使用して、`SetImage`し呼び出しとプレフィックスの名前で`StartAnimating`:

```csharp
animatedImage.SetImage ("Bus");
animatedImage.StartAnimating ();
```

呼び出す`StopAnimating`アニメーションのループを停止するイメージ コントロール。

```csharp
animatedImage.StopAnimating ();
```


<a name="cache" />

## <a name="appendix-caching-images-watchos-1"></a>付録:イメージ (watchOS 1) のキャッシュ

> [!IMPORTANT]
> watchOS 3 のアプリをデバイスに完全に実行します。 次の情報は、watchOS 1 アプリのみです。

アプリケーションでは、イメージ、拡張機能に格納されます (またはダウンロードされた) を繰り返し使用する場合は、後続の表示のパフォーマンスを向上させる、ウォッチのストレージにイメージをキャッシュすること勧めします。

使用して、 `WKInterfaceDevice`s `AddCachedImage` watch、イメージを転送し、使用してメソッド`SetImage`に表示される、文字列としてイメージの name パラメーターで。

```csharp
var device = WKInterfaceDevice.CurrentDevice;
using (var image = UIImage.FromBundle ("Bumblebee")) {
    if (!device.AddCachedImage (image, "Bumblebee")) {
            Console.WriteLine ("Image cache full.");
        } else {
            cachedImage.SetImage ("Bumblebee");
        }
    }
}
```

コードを使用して、イメージ キャッシュの内容を照会できます`WKInterfaceDevice.CurrentDevice.WeakCachedImages`します。


### <a name="managing-the-cache"></a>キャッシュを管理します。

キャッシュ サイズは約 20 MB です。 アプリの再起動の間で維持、およびを使用してファイルをクリアする必要がなくなることと`RemoveCachedImage`または`RemoveAllCachedImages`メソッド、`WKInterfaceDevice.CurrentDevice`オブジェクト。



## <a name="related-links"></a>関連リンク

- [WatchKitCatalog (サンプル)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Apple のイメージのドキュメント](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Images.html)
