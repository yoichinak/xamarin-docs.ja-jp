---
title: Instruments を使用した Xamarin.iOS アプリケーションのプロファイリング
description: このドキュメントでは、Apple の Instruments アプリを使用して、デバイスやシミュレーターにインストールされている Xamarin.iOS アプリケーションをプロファイリングする方法について説明します。
ms.prod: xamarin
ms.assetid: 70A8CAC8-20C2-655B-37C3-ACF9EA7874D8
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/19/2017
ms.openlocfilehash: 866ebd2a66557cda5372c5bc3d7a909e18c762d7
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70756090"
---
# <a name="profiling-xamarinios-applications-with-instruments"></a>Instruments を使用した Xamarin.iOS アプリケーションのプロファイリング

Xcode **Instruments** は、デバイスまたはシミュレーターで Xamarin.iOS アプリをプロファイリングするために使用できるツールです。 Mono はその Just-in-Time モデルを使用してコードをコンパイルし、Instruments はこの種のデータをうまく解釈できません。そのため、Instruments を使用するシミュレーター ベースのアプリケーションからの出力を使用するのが困難な場合があります。
この問題のため、このガイドでは、このドキュメントで Instruments の出力を解釈するための開発者アプリの使用法を重点的に説明します。

## <a name="requirements"></a>必要条件

Xcode Instruments は Mac でのみ実行されます。

## <a name="opening-the-instruments-app"></a>Instruments アプリを開く

デバイスを選択し、Instruments アプリを実行します。

1. Visual Studio for Mac で Xamarin.iOS プロジェクトを開きます。
2. **[Debug|iPhone]\(デバッグ|iPhone\)** 構成を選択します。
3. iOS デバイスをコンピューターに接続します。
4. **[実行]** メニューで **[デバイスにアップロード]** を選択します。 これでアプリケーションがビルドされ、デバイスにアップロードされます。
5. **[ツール]** メニューで、**[Instruments の起動]** を選択します。

Instruments が開き、次のダイアログが表示されます。

 [![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments1.png "プロファイリング テンプレートを選択する")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments1.png#lightbox)

**[Allocations]\(割り当て\)** テンプレートをクリックして選択します。 他のテンプレートも有効ですが、この記事では **Allocations** プロファイル テンプレートについてのみ説明します。

次に、ウィンドウの上部にあるメニューを使用して、デバイスとアプリケーションを選択します。

[![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments2.png "デバイスとアプリケーションを選択する")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments2.png#lightbox)

ウィンドウの上部にあるメニューで iOS デバイスを選択し、その横でプロファイルするアプリケーションを選択します (上記のスクリーン ショットの [**MemoryDemo**])。

メニューにデバイスが一覧表示されない場合は、Visual Studio for Mac の**コンソール** で、アプリがデバイスに展開されたときに表示された可能性のあるエラー メッセージを確認します。 また、Xcode オーガナイザーによってデバイスが開発用にプロビジョニングされていることを確認します。

**[選択]** ボタンをクリックすると、次の画面が表示されます。

[![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments3.png "プロファイリング インターフェイス")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments3.png#lightbox)

レコード ボタン (左上の赤い円) をクリックしてプロファイリングを開始します。

次のスクリーンショットは、**Instruments** を使用したプロファイリングの例を示しています。

[![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments4.png "Instruments によるプロファイリング例")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments4.png#lightbox)

## <a name="summary"></a>まとめ

このガイドでは、Xcode Instruments を開始して、Visual Studio for Mac 内から iOS アプリを監視する方法を説明しました。 Instruments を使用してメモリの問題を診断する方法の例については、[Instruments のチュートリアル](~/ios/deploy-test/walkthrough-apples-instrument.md)に進みます。

## <a name="related-links"></a>関連リンク

- [Instruments のチュートリアル](~/ios/deploy-test/walkthrough-apples-instrument.md)
- [Xamarin.iOS ガベージ コレクション (ブログ記事)](http://c-sharx.net/2015-04-27-xamarin-ios-the-garbage-collector-and-me/)
