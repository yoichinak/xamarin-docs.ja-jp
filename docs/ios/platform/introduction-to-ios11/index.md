---
title: iOS 11 の概要
description: このドキュメントでは、ARKit、CoreML、MapKit、PDFKit、SiriKit、ビジョンフレームワークなど、iOS 11 の機能について説明するさまざまなガイドにリンクしています。
ms.prod: xamarin
ms.assetid: 22C38EA6-6DA9-4B92-B41B-814E589033F6
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/19/2017
ms.openlocfilehash: 7262e44fe862e7a5c71f0b1dcfa5d672932fd4ee
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032185"
---
# <a name="introduction-to-ios-11"></a>iOS 11 の概要

![ARKit の例](images/arkit.png) ![AR オブジェクトの配置](images/arkit2.png) ![CoreML の例](images/coreml.png) ![MapKit の例](images/mapkit.png) ![ビジョン四角形の例](images/vision1.png) ![ビジョンの例](images/vision2.png) ![ドラッグアンドドロップの例](images/drag-drop.png) ![ドラッグアンドドロップの例](images/drag-drop2.png) ![SiriKit の例](images/sirikit.png)

iOS 11 には、さまざまなフレームワークにわたる多くの新機能と機能強化が含まれています。

## <a name="preparing-your-app-for-ios-11updating-your-appindexmd"></a>[IOS 11 用アプリの準備](updating-your-app/index.md)

Apple は、iOS 11 用のアーキテクチャの更新、新しいビジュアルの変更、および更新された iTunes Connect プロセスを導入しました。 このガイドを使用して、新しいリリース用に Xamarin iOS アプリが準備されていることを確認してください。

## <a name="arkitarkitindexmd"></a>[ARKit](arkit/index.md)

ARKit は、iOS に拡張された現実をもたらし、ユーザーはデバイスのカメラを介して世界中と対話できるようにします。
Xamarin では、 [Arkit を UrhoSharp と共](arkit/urhosharp.md)に使用することもできます。

## <a name="coremlcoremlmd"></a>[CoreML](coreml.md)

Machine learning モデルは、CoreML で iOS 11 アプリに統合できます。 CoreML フレームワークには、既存のモデルをアプリプロジェクトに組み込むための単純な API が用意されているため、アプリで問題を分析することができます。

## <a name="corenfccorenfcmd"></a>[CoreNFC](corenfc.md)

iPhone 7 以降のデバイスでは、近距離無線通信 (NFC) タグを読み取ることができます。これにより、アプリはタグ付けされた製品、場所、または世界中の物を検出できます。

## <a name="drag-and-dropdrag-and-dropmd"></a>[ドラッグ アンド ドロップ](drag-and-drop.md)

ドラッグアンドドロップフレームワークを使用すると、タッチによってデータを移動するための iOS 全体のサポートが提供されます。 IPad では、さまざまなアプリの両方でドラッグできます。iPhone では、同じアプリ内でのみドラッグできます。 豊富なデータ型、アニメーション、マルチタッチジェスチャの処理など、さまざまな種類のカスタマイズがサポートされています。

## <a name="mapkitmapkitmd"></a>[MapKit](mapkit.md)

MapKit には、自動マーカーグループ化のサポートや、ビューへのコンパスの追加など、さまざまな機能強化が施されています。

## <a name="pdfkit"></a>PDFKit

IOS 11 で PDFKit が利用可能になり、アプリに PDF の作成と編集の機能が提供されるようになりました。

## <a name="sirikitsirikitmd"></a>[SiriKit](sirikit.md)

Siri では、リストやメモなど、さらに多くの相互作用がサポートされるようになりました。その他の拡張機能としては、別のアプリ名があります

## <a name="visionvisionmd"></a>[Vision](vision.md)

顔検出と認識、CoreML モデル、新しいバーコード検出 Api、テキストとホライズンの検出、および一般的なオブジェクトの検出と追跡を含む、さまざまなイメージ処理および分析機能を iOS に提供します。

## <a name="samples"></a>サンプル

使用を開始するためC#の[サンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS11)がいくつか用意されています。

- [ARKit サンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-arkitsample)
- [オブジェクトを配置する ARKit](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-arkitplacingobjects)
- [ARKit と UrhoSharp](arkit/urhosharp.md)
- [CoreML イメージ認識のサンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlimagerecognition)
- [Azure カスタムモデルを使用した CoreML](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlazuremodel)
- [CoreNFC タグリーダーのサンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-nfctagreader)
- [ドラッグ & ドロップテーブルビュー](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-draganddroptableview)
- [& ドロップコレクションビューをドラッグします](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-draganddropcollectionview)
- [ドラッグ & ドロップカスタムビュー](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-draganddropcustomview)
- [DragBoard ドラッグ & ドロップサンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-draganddropdragboard)
- [MapKit サンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-mapkitsample)
- [SiriKit のサンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-sirikitsample)
- [更新された Photos フレームワークのサンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-samplephotoapp)
- [ビジョン & CoreML サンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlvision)
- [視覚四角形の検出のサンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-visionrectangles/)
- [ビジョン検出のサンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-visionfaces)
- [PDKFit ウィジェットのサンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-pdfannotationwidgetsadvanced)
- [PDFKit 透かしサンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-pdfdocumentwatermark)

## <a name="related-links"></a>関連リンク

- [Xamarin iOS 11 サンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS11)
