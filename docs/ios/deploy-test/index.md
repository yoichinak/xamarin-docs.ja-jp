---
title: Xamarin.iOS アプリの展開とテスト
description: このドキュメントはさまざまなガイドにリンクしています。これらのガイドでは、Xamarin.iOS アプリケーションの展開とテストに関連するトピックについて説明します。 たとえば、アプリの配布、.ipa ファイル、プロビジョニング、ワイヤレス展開、TestFlight、およびデバッグについてです。
ms.prod: xamarin
ms.assetid: 2DBF3BF9-79E7-4E24-AF26-E34C972B0169
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/19/2017
ms.openlocfilehash: 8ce601bff478cfc75d209b0d3e6ec3f6a48dbeee
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70288964"
---
# <a name="deploying-and-testing-xamarinios-apps"></a>Xamarin.iOS アプリの展開とテスト

このセクションには、アプリケーションのテストに使用されるトピックとアプリケーションを配布する方法が含まれます。 以下のトピックでは、デバッグに使用されるツール、テスターへの展開、App Store にアプリケーションを発行する方法などを説明しています。

## <a name="app-distributioniosdeploy-testapp-distributionindexmd"></a>[アプリの配布](~/ios/deploy-test/app-distribution/index.md)

この記事では、以下のようなさまざまな手段で配布する Xamarin.iOS アプリケーションの構成、構築、発行の各方法について説明しています。

- [App Store の配布](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [社内 (エンタープライズ) 配布](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [アドホック配布](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)

## <a name="ipa-deploymentiosdeploy-testapp-distributionipa-supportmd"></a>[IPA 展開](~/ios/deploy-test/app-distribution/ipa-support.md)

開発者はアドホック展開とエンタープライズ展開を使用して、テスト目的または社内ユーザー向けに配布するパッケージを作成できます。 このドキュメントでは、iTunes を使用して iOS デバイスと同期できる IPA を作成する方法について説明します。

## <a name="provisioningprovisioningindexmd"></a>[プロビジョニング](provisioning/index.md)

この一連のガイドでは、プロパティの一覧やアプリケーション サービス用アプリのプロビジョニング方法など、コード署名とプロビジョニングの要点を取り上げます。 

## <a name="wireless-deploymentwireless-deploymentmd"></a>[ワイヤレス展開](wireless-deployment.md)

 Xcode 9 では、ネットワークを介して iOS デバイスや Apple TV に展開するオプションが導入されました。アプリを展開およびデバッグするたびにデバイスを配線する必要はありません。 この機能は現在プレビュー段階です。

## <a name="testflightiosdeploy-testtestflightmd"></a>[TestFlight](~/ios/deploy-test/testflight.md)

Apple が所有するようになった TestFlight は、Xamarin.iOS アプリのベータ テストを行う主要な方法です。 この記事では、アプリのアップロードから iTunes Connect での使用まで、TestFlight プロセスのすべての手順について説明します。

## <a name="debugging-in-xamariniosiosdeploy-testdebugging-in-xamarin-iosmd"></a>[Xamarin.iOS でのデバッグ](~/ios/deploy-test/debugging-in-xamarin-ios.md)

Visual Studio と Visual Studio for Mac IDE のどちらにも、iOS シミュレーターと iOS デバイスの両方で Xamarin.iOS アプリケーションをデバッグできるサポートが含まれています。 この記事では、デバッガーを使用する方法と、サポートする多様なオプションを構成する方法について説明します。

## <a name="touchunitiosdeploy-testtouchunitmd"></a>[Touch.Unit](~/ios/deploy-test/touch.unit.md)

このドキュメントでは、Xamarin.iOS プロジェクト用の単体テストを作成する方法について説明します。
Xamarin.iOS での単体テストは、Touch.Unit フレームワークを使用して行います。このフレームワークには、iOS テスト ランナーと、単体テストの書き込みのために使い慣れた一連の API を提供する [NUnitLite](http://www.nunitlite.com/) フレームワークの変更バージョンの両方が含まれます。

## <a name="using-instruments-to-detect-native-leaks-using-markheapiosdeploy-testusing-instruments-to-detect-native-leaks-using-markheapmd"></a>[Instruments で MarkHeap を使用してネイティブ リークを検出する](~/ios/deploy-test/using-instruments-to-detect-native-leaks-using-markheap.md)

この記事では、iOS デバイスと Xamarin.iOS アプリケーションで Instruments を使用する方法について説明します。 また、シミュレーターでアプリケーションをプロファイルする方法についても説明します。

## <a name="walkthrough---using-apples-instrument-tooliosdeploy-testwalkthrough-apples-instrumentmd"></a>[チュートリアル - Apple の Instruments ツールの使用](~/ios/deploy-test/walkthrough-apples-instrument.md)

この記事では、Apple の Instruments ツールを使用して、Xamarin でビルドされた iOS アプリケーションのメモリ問題を診断する方法について説明します。 Instruments を起動し、ヒープ スナップショットを取得してメモリの増加を分析する方法を示します。 また、Instruments を使用して、メモリの問題を引き起こしている正確なコード行を表示および特定する方法も示します。

## <a name="linking-on-ioslinkermd"></a>[iOS でのリンク](linker.md)

リンカーが最小限のアプリケーション パッケージを実現するためのしくみと、その設定と用途の変更方法について説明します。

## <a name="xamarinios-performanceperformancemd"></a>[Xamarin.iOS のパフォーマンス](performance.md)

Xamarin.iOS でビルドされたアプリケーションのパフォーマンスを高めるための方法は多数あります。 これらの手法をすべて使用することで、CPU で実行される作業量や、アプリケーションで消費されるメモリ量を大幅に減らすことができます。

## <a name="mtouchmtouchmd"></a>[mtouch](mtouch.md)

iOS で使用できるアプリケーションにプロジェクトをビルドするコマンド ライン ツール mtouch.exe のノートと情報。

## <a name="ios-build-mechanicsios-build-mechanicsmd"></a>[iOS ビルドのしくみ](ios-build-mechanics.md)

このガイドではアプリのタイミング設定の方法と、すべてのビルド構成で迅速なビルドを行うために採用できる方法の使い方について説明します。
