---
title: Xamarin の watchOS Image コントロール
description: このドキュメントでは、Xamarin でビルドされた watchOS アプリケーションでイメージコントロールを使用する方法について説明します。 WKInterfaceImage コントロール、SetImage メソッド、watch 拡張機能へのイメージの追加、アニメーションなどについて説明します。
ms.prod: xamarin
ms.assetid: B741C207-3427-46F3-9C90-A52BF8933FA4
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: f9367eda7651ca61a8a3cb0928ad11cb320faab6
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "70769957"
---
# <a name="watchos-image-controls-in-xamarin"></a>Xamarin の watchOS Image コントロール

watchOS には、イメージと単純なアニメーションを表示するための[`WKInterfaceImage`](xref:WatchKit.WKInterfaceImage)コントロールが用意されています。 一部のコントロールには、背景イメージ (ボタン、グループ、インターフェイスコントローラーなど) を含めることもできます。

![](image-images/image-walkway.png "画像を表示する Apple Watch") ![](image-images/image-animation.png "単純なアニメーションを使用した Apple Watch")
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

アセットカタログ画像を使用して、ウォッチキットアプリに画像を追加します。
すべてのウォッチデバイスに Retina が表示されるため、必要なバージョンは **@2x** のみです。

![](image-images/asset-universal-sml.png "Only 2x versions are required, since all watch devices have Retina displays")

画像自体が、ウォッチ画面の正しいサイズであることを確認することをお勧めします。 サイズの正しくないイメージ (特に大きなもの) を使用*せず*に、拡大縮小して、ウォッチに表示します。

アセットカタログイメージで Watch Kit のサイズ (38mm と 42 mm) を使用して、表示サイズごとに異なるイメージを指定できます。

![](image-images/asset-watch-sml.png "You can use the Watch Kit sizes 38mm and 42mm in an asset catalog image to specify different images for each display size")

## <a name="images-on-the-watch"></a>ウォッチの画像

画像を表示する最も効率的な方法は、*それらを watch アプリプロジェクトに含め*、`SetImage(string imageName)` メソッドを使用して表示することです。

たとえば、 [WatchKitCatalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog/)サンプルには、watch アプリプロジェクトのアセットカタログにいくつかのイメージが追加されています。

![](image-images/asset-whale-sml.png "The WatchKitCatalog sample has a number of images added to an asset catalog in the watch app project")

これらの値は、文字列名パラメーターを指定して `SetImage` を使用して、監視に効率的に読み込むことができます。

```csharp
myImageControl.SetImage("Whale");
myOtherImageControl.SetImage("Worry");
```

### <a name="background-images"></a>背景画像

@No__t_1、`Group`、および `InterfaceController` クラスの `SetBackgroundImage (string imageName)` に対しても同じロジックが適用されます。 最適なパフォーマンスを得るには、画像を watch アプリ自体に格納します。

## <a name="images-in-the-watch-extension"></a>Watch 拡張機能の画像

Watch アプリ自体に格納されているイメージを読み込むだけでなく、拡張機能バンドルのイメージを watch アプリに送信して表示することもできます (または、リモートの場所からイメージをダウンロードして表示することもできます)。

Watch 拡張機能からイメージを読み込むには `UIImage` インスタンスを作成し、`UIImage` オブジェクトを使用して `SetImage` を呼び出します。

たとえば、 [WatchKitCatalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)サンプルには、watch 拡張機能プロジェクトの**Bumblebee**という名前のイメージがあります。

![](image-images/asset-bumblebee-sml.png "The WatchKitCatalog sample has an image named Bumblebee in the watch extension project")

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

[WatchKitCatalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)サンプルには、 **Bus**プレフィックスを持つ watch アプリプロジェクトに一連の番号付き画像が含まれています。

![](image-images/asset-bus-animation-sml.png "The WatchKitCatalog sample has a series of numbered images in the watch app project with the Bus prefix")

これらのイメージをアニメーションとして表示するには、まず、`SetImage` を使用してプレフィックス名を使用してイメージを読み込み、次に `StartAnimating` を呼び出します。

```csharp
animatedImage.SetImage ("Bus");
animatedImage.StartAnimating ();
```

イメージコントロールの `StopAnimating` を呼び出して、アニメーションのループを停止します。

```csharp
animatedImage.StopAnimating ();
```

<a name="cache" />

## <a name="appendix-caching-images-watchos-1"></a>付録: イメージのキャッシュ (watchOS 1)

> [!IMPORTANT]
> watchOS 3 アプリはデバイス上で完全に実行されます。 次の情報は、watchOS 1 アプリのみを対象としています。

アプリケーションが拡張機能に格納されているイメージを繰り返し使用している場合 (またはダウンロードされている場合)、ウォッチのストレージにイメージをキャッシュして、後続の表示のパフォーマンスを向上させることができます。

@No__t_0s `AddCachedImage` メソッドを使用してイメージをウォッチに転送し、image name パラメーターと共に `SetImage` を文字列として使用して表示します。

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

@No__t_0 を使用して、コード内のイメージキャッシュの内容を照会できます。

### <a name="managing-the-cache"></a>キャッシュの管理

キャッシュのサイズは約 20 MB です。 アプリの再起動間に保持されます。また、`WKInterfaceDevice.CurrentDevice` オブジェクトで `RemoveCachedImage` または `RemoveAllCachedImages` メソッドを使用してファイルをクリアする必要があります。

## <a name="related-links"></a>関連リンク

- [WatchKitCatalog (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Apple のイメージドキュメント](https://developer.apple.com/documentation/watchkit/wkinterfaceimage)
