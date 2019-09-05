---
title: Xamarin の watchOS Image コントロール
description: このドキュメントでは、Xamarin でビルドされた watchOS アプリケーションでイメージコントロールを使用する方法について説明します。 WKInterfaceImage コントロール、SetImage メソッド、watch 拡張機能へのイメージの追加、アニメーションなどについて説明します。
ms.prod: xamarin
ms.assetid: B741C207-3427-46F3-9C90-A52BF8933FA4
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: 18e7873eede87e9bb81c1c0b304bfc87c317c27a
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291510"
---
# <a name="watchos-image-controls-in-xamarin"></a>Xamarin の watchOS Image コントロール

watchOS の提供、 [`WKInterfaceImage`](xref:WatchKit.WKInterfaceImage)イメージや単純なアニメーションを表示するコントロール。 一部のコントロールには、背景イメージ (ボタン、グループ、インターフェイスコントローラーなど) を含めることもできます。

![](image-images/image-walkway.png "Apple Watch を示す図") ![](image-images/image-animation.png "単純なアニメーションを使用して Apple Watch")
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

アセットカタログ画像を使用して、ウォッチキットアプリに画像を追加します。
のみ **@2x** Retina ディスプレイ デバイスをすべて見るために、バージョンが必要です。

![](image-images/asset-universal-sml.png "すべてのウォッチデバイスに Retina が表示されるため、必要なバージョンは2倍です。")

画像自体が、ウォッチ画面の正しいサイズであることを確認することをお勧めします。 サイズの正しくないイメージ (特に大きなもの) を使用*せず*に、拡大縮小して、ウォッチに表示します。

アセットカタログイメージで Watch Kit のサイズ (38mm と 42 mm) を使用して、表示サイズごとに異なるイメージを指定できます。

![](image-images/asset-watch-sml.png "アセットカタログイメージで Watch Kit のサイズ38mm と 42 mm を使用して、表示サイズごとに異なるイメージを指定できます。")


## <a name="images-on-the-watch"></a>ウォッチの画像

画像を表示する最も効率的な方法は、*それらを watch アプリプロジェクトに含め*、 `SetImage(string imageName)`メソッドを使用して表示することです。

たとえば、 [WatchKitCatalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog/)サンプルには、watch アプリプロジェクトのアセットカタログにいくつかのイメージが追加されています。

![](image-images/asset-whale-sml.png "WatchKitCatalog サンプルには、watch アプリプロジェクトのアセットカタログに多数のイメージが追加されています。")

これらは、文字列 name パラメーターを使用して`SetImage` 、監視に効率的に読み込んで表示できます。

```csharp
myImageControl.SetImage("Whale");
myOtherImageControl.SetImage("Worry");
```

### <a name="background-images"></a>背景画像

、、および`SetBackgroundImage (string imageName)` `Group` `Button`クラスのに対しても同じロジックが適用されます。`InterfaceController` 最適なパフォーマンスを得るには、画像を watch アプリ自体に格納します。


## <a name="images-in-the-watch-extension"></a>Watch 拡張機能の画像

Watch アプリ自体に格納されているイメージを読み込むだけでなく、拡張機能バンドルのイメージを watch アプリに送信して表示することもできます (または、リモートの場所からイメージをダウンロードして表示することもできます)。

Watch 拡張機能からイメージを読み込むには`UIImage` 、インスタンスを作成`SetImage`し、 `UIImage`オブジェクトを使用してを呼び出します。

たとえば、 [WatchKitCatalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)サンプルには、watch 拡張機能プロジェクトの**Bumblebee**という名前のイメージがあります。

![](image-images/asset-bumblebee-sml.png "WatchKitCatalog サンプルには、watch 拡張機能プロジェクトの Bumblebee という名前のイメージがあります。")

次のコードでは、が発生します。

- メモリに読み込まれているイメージ。
- ウォッチに表示されます。

```csharp
using (var image = UIImage.FromBundle ("Bumblebee")) {
    myImageControl.SetImage (image);
}
```


## <a name="animations"></a>アニメーション

一連のイメージをアニメーション化するには、すべてが同じプレフィックスで始まり、数字のサフィックスを持つ必要があります。

[WatchKitCatalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)サンプルには、 **Bus**プレフィックスを持つ watch アプリプロジェクトに一連の番号付き画像が含まれています。

![](image-images/asset-bus-animation-sml.png "WatchKitCatalog サンプルには、Bus プレフィックスを使用した watch アプリプロジェクト内の一連の番号付き画像が含まれています。")

これらのイメージをアニメーションとして表示するには、 `SetImage`まずプレフィックス名を使用して`StartAnimating`イメージを読み込み、次のように呼び出します。

```csharp
animatedImage.SetImage ("Bus");
animatedImage.StartAnimating ();
```

アニメーション`StopAnimating`のループを停止するには、イメージコントロールでを呼び出します。

```csharp
animatedImage.StopAnimating ();
```


<a name="cache" />

## <a name="appendix-caching-images-watchos-1"></a>付録:イメージのキャッシュ (watchOS 1)

> [!IMPORTANT]
> watchOS 3 アプリはデバイス上で完全に実行されます。 次の情報は、watchOS 1 アプリのみを対象としています。

アプリケーションが拡張機能に格納されているイメージを繰り返し使用している場合 (またはダウンロードされている場合)、ウォッチのストレージにイメージをキャッシュして、後続の表示のパフォーマンスを向上させることができます。

S メソッドを使用してイメージをウォッチに転送し、image name `SetImage`パラメーターと共にを文字列として使用して表示します。 `AddCachedImage` `WKInterfaceDevice`

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

を使用して`WKInterfaceDevice.CurrentDevice.WeakCachedImages`、コード内のイメージキャッシュの内容を照会できます。


### <a name="managing-the-cache"></a>キャッシュの管理

キャッシュのサイズは約 20 MB です。 アプリの再起動間に保持されます。また、 `RemoveCachedImage` `WKInterfaceDevice.CurrentDevice`オブジェクトに対してメソッドまたは`RemoveAllCachedImages`メソッドを使用してファイルをクリアする必要があります。



## <a name="related-links"></a>関連リンク

- [WatchKitCatalog (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Apple のイメージドキュメント](https://developer.apple.com/documentation/watchkit/wkinterfaceimage)
