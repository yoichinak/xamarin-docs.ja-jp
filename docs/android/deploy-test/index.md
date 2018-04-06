---
title: 配置とテスト
description: このセクションには、アプリケーションをテストしたり、アプリケーションのパフォーマンスを最適化したり、リリース用にそれを準備したり、証明書で署名したり、それをアプリ ストアに公開する方法のガイドが含まれています
ms.prod: xamarin
ms.assetid: 568C0B85-EFF3-AF6F-5605-95055193D367
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: d7453af3721cbdb6b93d884322cee1cd5115c317
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="deployment-and-testing"></a>配置とテスト

このセクションには、アプリケーションをテストしたり、アプリケーションのパフォーマンスを最適化したり、リリース用にそれを準備したり、証明書で署名したり、それをアプリ ストアに公開する方法のガイドが含まれています。


##  <a name="application-package-sizesapp-package-sizemd"></a>[アプリケーション パッケージのサイズ](app-package-size.md)

この記事では Xamarin.Android アプリケーション パッケージの構成部分と、開発のデバッグおよびリリース段階で効果的なパッケージ開発を行うために使用できる関連の戦略について検証します。

##  <a name="building-appsbuilding-appsindexmd"></a>[アプリのビルド](building-apps/index.md)

このセクションでは、ビルド プロセスのしくみと、ABI 固有の APK のビルド方法について説明します。

##  <a name="command-line-emulatorcommand-line-emulatormd"></a>[コマンド ライン エミュレーター](command-line-emulator.md)

この記事ではコマンド ラインを使用したエミュレーターの起動について簡単に説明します。

## <a name="debuggingandroiddeploy-testdebuggingindexmd"></a>[デバッグ](~/android/deploy-test/debugging/index.md)

このセクションのガイドは、Android エミュレーター、実際の Android デバイス、およびデバッグ ログを使用してアプリをデバッグするときに役立ちます。

##  <a name="setting-the-debuggable-attributeandroiddeploy-testdebuggable-attributemd"></a>[デバッグできる属性の設定](~/android/deploy-test/debuggable-attribute.md)

この記事では `adb` などのツールが JVM と通信できるように、デバッグできる属性を設定する方法について説明します。

##  <a name="environmentenvironmentmd"></a>[環境](environment.md)

この記事では Xamarin.Android の実行環境と、プログラムの実行に影響を与える Android システムのプロパティについて説明します。

##  <a name="gdbgdbmd"></a>[GDB](gdb.md)

この記事では、`gdb` を使用して Xamarin.Android アプリケーションをデバッグする方法について説明します。

##  <a name="installing-a-system-appinstall-system-appmd"></a>[システム アプリのインストール](install-system-app.md)

このガイドでは、システム アプリケーションとして Android デバイスまたはカスタム ROM の一部として Xamarin.Android アプリをインストールする方法について説明します。

##  <a name="linking-on-androidlinkermd"></a>[Android でのリンク](linker.md)

この記事ではアプリケーションの最終的なサイズを削減するために Xamarin.Android で使用されるリンク プロセスについて説明します。 実行できるさまざまなレベルのリンクについて説明し、リンカーの使用が原因のエラーを軽減するためのガイダンスやトラブルシューティングのアドバイスを提供します。

## <a name="xamarinandroid-performanceandroiddeploy-testperformancemd"></a>[Xamarin.Android パフォーマンス](~/android/deploy-test/performance.md)

Xamarin.Android でビルドされたアプリケーションのパフォーマンスを高めるための方法は多数あります。 これらの手法をすべて使用することで、CPU で実行される作業量や、アプリケーションで消費されるメモリ量を大幅に減らすことができます。

## <a name="preparing-an-application-for-releaseandroiddeploy-testrelease-prepindexmd"></a>[リリースに向けてアプリケーションを準備する](~/android/deploy-test/release-prep/index.md)

アプリケーションがコード化され、テストされたら、配信のためにパッケージを用意する必要があります。 このパッケージ準備における最初の作業は、リリース用のアプリケーションをビルドすることです。中心的な作業は、いくつかのアプリケーション属性を設定することです。

## <a name="signing-the-android-application-packageandroiddeploy-testsigningindexmd"></a>[Android アプリケーション パッケージに署名する](~/android/deploy-test/signing/index.md)

Android の署名 ID を作成する方法、Android アプリケーション用の新しい署名証明書を作成する方法、署名証明書を使用してアプリケーションに署名する方法を学習します。 さらに、*アドホック*配布用にアプリをディスクにエクスポートする方法をこのトピックでは説明します。 結果として得られる APK は、アプリ ストアを経由せずに Android デバイスにサイドロードすることができます。

## <a name="publishing-an-applicationandroiddeploy-testpublishingindexmd"></a>[アプリケーションの発行](~/android/deploy-test/publishing/index.md)

この一連の記事では、Xamarin.Android で作成されたアプリケーションをパブリック配布する手順について説明します。 配布は、電子メール、プライベートの Web サーバー、Google Play、Amazon Android アプリ ストアなどのチャネルを経由して行うことができます。
