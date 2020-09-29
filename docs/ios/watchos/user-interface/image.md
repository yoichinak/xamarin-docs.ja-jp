---
title: Xamarin の watchOS Image コントロール
description: このドキュメントでは、Xamarin でビルドされた watchOS アプリケーションでイメージコントロールを使用する方法について説明します。 WKInterfaceImage コントロール、SetImage メソッド、watch 拡張機能へのイメージの追加、アニメーションなどについて説明します。
ms.prod: xamarin
ms.assetid: B741C207-3427-46F3-9C90-A52BF8933FA4
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: f1c21d64c8e1e271043e7d0b918f6033e21daac7
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91432993"
---
# <a name="watchos-image-controls-in-xamarin"></a>Xamarin の watchOS Image コントロール

watchOS には、 [`WKInterfaceImage`](xref:WatchKit.WKInterfaceImage) イメージと単純なアニメーションを表示するためのコントロールが用意されています。 一部のコントロールには、背景イメージ (ボタン、グループ、インターフェイスコントローラーなど) を含めることもできます。

![](image-images/image-walkway.png) ![ 単純なアニメーションで画像 Apple Watch を表示する Apple Watch](image-images/image-animation.png)
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

アセットカタログ画像を使用して、ウォッチキットアプリに画像を追加します。
**@2x**すべてのウォッチデバイスに Retina が表示されるため、バージョンのみが必要です。

![すべてのウォッチデバイスに Retina が表示されるため、必要なバージョンは2倍です。](image-images/asset-universal-sml.png)

画像自体が、ウォッチ画面の正しいサイズであることを確認することをお勧めします。 サイズの正しくないイメージ (特に大きなもの) を使用*せず*に、拡大縮小して、ウォッチに表示します。

アセットカタログイメージで Watch Kit のサイズ (38mm と 42 mm) を使用して、表示サイズごとに異なるイメージを指定できます。

![アセットカタログイメージで Watch Kit のサイズ38mm と 42 mm を使用して、表示サイズごとに異なるイメージを指定できます。](image-images/asset-watch-sml.png)

## <a name="images-on-the-watch"></a>ウォッチの画像

画像を表示する最も効率的な方法は、 *それらを watch アプリプロジェクトに含め* 、メソッドを使用して表示することです `SetImage(string imageName)` 。

たとえば、 [WatchKitCatalog](/samples/xamarin/ios-samples/watchos-watchkitcatalog/) サンプルには、watch アプリプロジェクトのアセットカタログにいくつかのイメージが追加されています。

![WatchKitCatalog サンプルには、watch アプリプロジェクトのアセットカタログに多数のイメージが追加されています。](image-images/asset-whale-sml.png)

これらは、文字列 name パラメーターを使用して、監視に効率的に読み込んで表示でき `SetImage` ます。

```csharp
myImageControl.SetImage("Whale");
myOtherImageControl.SetImage("Worry");
```

### <a name="background-images"></a>背景画像

、、およびクラスのに対しても同じロジックが適用され `SetBackgroundImage (string imageName)` `Button` `Group` `InterfaceController` ます。 最適なパフォーマンスを得るには、画像を watch アプリ自体に格納します。

## <a name="images-in-the-watch-extension"></a>Watch 拡張機能の画像

Watch アプリ自体に格納されているイメージを読み込むだけでなく、拡張機能バンドルのイメージを watch アプリに送信して表示することもできます (または、リモートの場所からイメージをダウンロードして表示することもできます)。

Watch 拡張機能からイメージを読み込むには、 `UIImage` インスタンスを作成し、オブジェクトを使用してを呼び出し `SetImage` `UIImage` ます。

たとえば、 [WatchKitCatalog](/samples/xamarin/ios-samples/watchos-watchkitcatalog) サンプルには、watch 拡張機能プロジェクトの **Bumblebee** という名前のイメージがあります。

![WatchKitCatalog サンプルには、watch 拡張機能プロジェクトの Bumblebee という名前のイメージがあります。](image-images/asset-bumblebee-sml.png)

次のコードでは、が発生します。

- メモリに読み込まれているイメージ。
- ウォッチに表示されます。

```csharp
using (var image = UIImage.FromBundle ("Bumblebee")) {
    myImageControl.SetImage (image);
}
```

## <a name="animations"></a>Animations

一連のイメージをアニメーション化するには、すべてが同じプレフィックスで始まり、数字のサフィックスを持つ必要があります。

[WatchKitCatalog](/samples/xamarin/ios-samples/watchos-watchkitcatalog)サンプルには、 **Bus**プレフィックスを持つ watch アプリプロジェクトに一連の番号付き画像が含まれています。

![WatchKitCatalog サンプルには、Bus プレフィックスを使用した watch アプリプロジェクト内の一連の番号付き画像が含まれています。](image-images/asset-bus-animation-sml.png)

これらのイメージをアニメーションとして表示するには、まずプレフィックス名を使用してイメージを読み込み、次のように `SetImage` 呼び出し `StartAnimating` ます。

```csharp
animatedImage.SetImage ("Bus");
animatedImage.StartAnimating ();
```

`StopAnimating`アニメーションのループを停止するには、イメージコントロールでを呼び出します。

```csharp
animatedImage.StopAnimating ();
```

<a name="cache"></a>

## <a name="appendix-caching-images-watchos-1"></a>付録: イメージのキャッシュ (watchOS 1)

> [!IMPORTANT]
> watchOS 3 アプリはデバイス上で完全に実行されます。 次の情報は、watchOS 1 アプリのみを対象としています。

アプリケーションが拡張機能に格納されているイメージを繰り返し使用している場合 (またはダウンロードされている場合)、ウォッチのストレージにイメージをキャッシュして、後続の表示のパフォーマンスを向上させることができます。

S メソッドを使用して `WKInterfaceDevice` `AddCachedImage` イメージをウォッチに転送し、 `SetImage` image name パラメーターと共にを文字列として使用して表示します。

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

を使用して、コード内のイメージキャッシュの内容を照会でき `WKInterfaceDevice.CurrentDevice.WeakCachedImages` ます。

### <a name="managing-the-cache"></a>キャッシュの管理

キャッシュのサイズは約 20 MB です。 アプリの再起動間に保持され `RemoveCachedImage` ます。また、オブジェクトに対してメソッドまたはメソッドを使用してファイルをクリアする必要があり `RemoveAllCachedImages` `WKInterfaceDevice.CurrentDevice` ます。

## <a name="related-links"></a>関連リンク

- [WatchKitCatalog (サンプル)](/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Apple のイメージドキュメント](https://developer.apple.com/documentation/watchkit/wkinterfaceimage)