---
title: watchOS Xamarin のイメージ コントロール
description: このドキュメントでは、Xamarin でビルドした watchOS アプリケーションでのイメージ コントロールを使用する方法について説明します。 これには、WKInterfaceImage コントロール、イメージに追加するウォッチ拡張機能、アニメーション、およびその他、SetImage メソッドについて説明します。
ms.prod: xamarin
ms.assetid: B741C207-3427-46F3-9C90-A52BF8933FA4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: eb58c587f737a5991a21f0efe9964353a8ab0399
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791252"
---
# <a name="watchos-image-controls-in-xamarin"></a>watchOS Xamarin のイメージ コントロール

watchOS 提供、 [ `WKInterfaceImage` ](https://developer.xamarin.com/api/type/WatchKit.WKInterfaceImage/)イメージや単純なアニメーションを表示するコントロール。 一部のコントロール (ボタン、グループ、およびコント ローラーのインターフェイス) などの背景画像をこともできます。

![](image-images/image-walkway.png "Apple Watch を示す図") ![ ](image-images/image-animation.png "単純なアニメーションを使用して Apple Watch")
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

ウォッチ キット アプリにイメージを追加するのにには、アセット カタログのイメージを使用します。
のみ**@2x** Retina ディスプレイを持つデバイスをすべて見るためのバージョンが必要です。

![](image-images/asset-universal-sml.png "Retina ディスプレイを持つデバイスを見るすべて以降にのみ 2 x バージョンが必要ですが、")

イメージ自体がウォッチ ディスプレイの適切なサイズであることを確認することをお勧めします。 *回避*サイズが不適切イメージ (特に大規模なもの) を使用して、ウォッチ上に表示するための拡張します。

資産カタログ イメージでウォッチ キット サイズ (38 mm および 42 mm) を使用すると、表示サイズごとに異なるイメージを指定します。

![](image-images/asset-watch-sml.png "表示サイズごとに異なるイメージを指定するのに資産カタログ イメージで 38 mm および 42 mm ウォッチ キット サイズを使用することができます。")


## <a name="images-on-the-watch"></a>ウォッチ上の画像

イメージを表示する最も効率的な方法は、する*watch アプリ プロジェクトに含める*し、表示を使用して、`SetImage(string imageName)`メソッドです。

たとえば、 [WatchKitCatalog](https://developer.xamarin.com/samples/WatchKitCatalog/)サンプルでは、さまざまなイメージ ウォッチ アプリ プロジェクト内のアセット カタログに追加します。

![](image-images/asset-whale-sml.png "WatchKitCatalog サンプルには、ウォッチ アプリケーション プロジェクト内の資産カタログを追加イメージの数")

これら効率的に読み込まれるし、表示できるウォッチを使用して、`SetImage`文字列名パラメーターを使用します。

```csharp
myImageControl.SetImage("Whale");
myOtherImageControl.SetImage("Worry");
```

### <a name="background-images"></a>背景画像

同じロジックが適用されます、`SetBackgroundImage (string imageName)`上、 `Button`、 `Group`、および`InterfaceController`クラスです。 最適なパフォーマンスは、ウォッチ アプリ自体にイメージを格納することによって実現されます。


## <a name="images-in-the-watch-extension"></a>イメージ ウォッチ拡張機能

Watch アプリ自体に保存されているイメージを読み込み、だけでなく、ウォッチ向けのアプリを表示拡張機能のバンドルからイメージを送信することができます (またはリモートの場所からイメージをダウンロードし、それらを表示する可能性があります)。

読み込むにはイメージ ウォッチ拡張機能から、次のように作成します。`UIImage`をインスタンス化し、呼び出す`SetImage`で、`UIImage`オブジェクト。

たとえば、 [WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)サンプルでは、という名前のイメージ**Bumblebee**ウォッチ拡張プロジェクトで。

![](image-images/asset-bumblebee-sml.png "WatchKitCatalog サンプル ウォッチ拡張プロジェクトで Bumblebee をという名前のイメージには")

次のコードが発生します。

- メモリに読み込まれているイメージと
- ウォッチに表示されます。

```csharp
using (var image = UIImage.FromBundle ("Bumblebee")) {
    myImageControl.SetImage (image);
}
```


## <a name="animations"></a>Animations

イメージのセットをアニメーション化するには、同じプレフィックスで始まるすべてと、数値のサフィックスが必要なです。

[WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)サンプル、ウォッチ アプリケーション プロジェクト内で一連の番号付きイメージには、**バス**プレフィックス。

![](image-images/asset-bus-animation-sml.png "WatchKitCatalog サンプル バス プレフィックスを持つ watch アプリ プロジェクトで一連の番号付きイメージには")

アニメーションとしてこれらのイメージを表示、まずロードを使用してイメージ`SetImage`プレフィックスの名前と、呼び出しが`StartAnimating`:

```csharp
animatedImage.SetImage ("Bus");
animatedImage.StartAnimating ();
```

呼び出す`StopAnimating`アニメーション ループを停止する、イメージ コントロールに。

```csharp
animatedImage.StopAnimating ();
```


<a name="cache" />

## <a name="appendix-caching-images-watchos-1"></a>付録: イメージ (watchOS 1) のキャッシュ

> [!IMPORTANT]
> watchOS 3 アプリは、デバイスに完全に実行されます。 次の情報は、watchOS 1 アプリのみです。

アプリケーションでは、イメージ、拡張機能に格納されます (またはダウンロードされた) を繰り返し使用する場合は、ウォッチの記憶域で、後続の表示のパフォーマンスを向上するイメージをキャッシュすることです。

使用して、 `WKInterfaceDevice`s `AddCachedImage` 、ウォッチにイメージを転送し、使用してメソッド`SetImage`パラメーターを持つ、イメージ名を文字列として表示するに。

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

使用してコード内のイメージのキャッシュの内容を照会する`WKInterfaceDevice.CurrentDevice.WeakCachedImages`です。


### <a name="managing-the-cache"></a>キャッシュを管理します。

キャッシュのサイズは約 20 MB です。 アプリの再起動前後で保持およびを使用してファイルをクリアする責任が不足すると`RemoveCachedImage`または`RemoveAllCachedImages`のメソッド、`WKInterfaceDevice.CurrentDevice`オブジェクト。



## <a name="related-links"></a>関連リンク

- [WatchKitCatalog (サンプル)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Apple のイメージのドキュメント](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Images.html)
