---
title: 32/64 ビットのプラットフォームに関する考慮事項
description: このドキュメントでは、Xamarin.iOS または Xamarin.Mac アプリケーションの 32 ビットおよび 64 ビットのアーキテクチャを対象とする場合に留意するさまざまな考慮事項について説明します。
ms.prod: xamarin
ms.assetid: F7126340-04B2-4A10-B14D-394E23527C1A
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 31eb0bfae58ecdca40548e46d1d9d95828be67b4
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120100"
---
# <a name="3264-bit-platform-considerations"></a>32/64 ビットのプラットフォームに関する考慮事項

IOS および macOS では、32 と 64 ビットの両方のアプリをサポートすることがこれまで、中に、Apple が徐々 に 32 ビットのサポートが非推奨です。

IOS 11、時点では 32 ビット アプリは起動されなく、および[App Store への送信が 64 ビットをサポートする必要があります](https://developer.apple.com/news/?id=06282017b)します。

2018 年 1 月以降[Mac App Store に送信される新しいアプリが 64 ビットをサポートする必要があります](https://developer.apple.com/news/?id=06282017a)2018 年 6 月で既存のアプリを更新する必要があります。

Xamarin のクラシック API (`XamMac.dll`と`monotouch.dll`) のみの 32 ビット アプリケーションをサポートします。 ただし、新しい Xamarin.iOS および Xamarin.Mac アプリケーションを使用して、 [Unified API](~/cross-platform/macios/unified/index.md) (`Xamarin.iOS`と`Xamarin.Mac`) 既定では、したがって 32 と 64 ビット、必要に応じて、ターゲットにできます。

## <a name="ios"></a>iOS

<a name="enable-64" />

### <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Xamarin.iOS アプリのビルドを 64 ビットの有効化

> [!WARNING]
> このセクションでは、歴史的な理由から、および Unified API に旧バージョンの Xamarin.iOS プロジェクトを移動すると、64 ビットのサポートに含まれます。 すべての新しい Xamarin.iOS プロジェクトでは、Unified API とターゲット 64 ビットを既定で使用されます。

Unified API に変換された Xamarin.iOS モバイル アプリケーション、開発者はビルド設定を 64 ビットをターゲットに手動で更新する必要があります。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Solution Pad**、アプリのプロジェクトを開く をダブルクリック、**プロジェクト オプション**ウィンドウ。
2. 選択**iOS ビルド**します。
3. IPhone シミュレーター用で、**サポートされているアーキテクチャ**ドロップダウンで、いずれかを選択**x86\_64**または**i386 + x86\_64**:

   [![サポートされているアーキテクチャを x86 に設定\_64 または i386 + x86\_64](Images/Image01.png "Setting Supported architectures to x86\_64 or i386 + x86\_64")](Images/Image01-large.png#lightbox) 

4. 物理デバイスは、使用可能ないずれかを選択**ARM64**の組み合わせ。

   [![サポートされているアーキテクチャを ARM64 の組み合わせのいずれかに設定する](Images/Image02.png "ARM64 の組み合わせのいずれかに設定がサポートされているアーキテクチャ")](Images/Image02-large.png#lightbox)

5. **[OK]** をクリックします。
6. クリーン ビルドを実行します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **ソリューション エクスプ ローラー**をアプリのプロジェクトを右クリックし、**プロパティ**します。
2. 選択**iOS ビルド**します。
3. IPhone シミュレーターでは、設定**サポートされているアーキテクチャ**いずれかに**x86\_64**または**i386 + x86\_64**: 

   [![サポートされているアーキテクチャを x86_64 または i386 + x86 に設定\_64](Images/VS02.png "Setting Supported architectures to x86_64 or i386 + x86\_64")](Images/VS02-large.png#lightbox)

4. 物理デバイスは、使用可能ないずれかを選択**ARM64**の組み合わせ。
    
   [![サポートされているアーキテクチャを ARM64 の組み合わせのいずれかに設定する](Images/VS01.png "ARM64 の組み合わせのいずれかに設定がサポートされているアーキテクチャ")](Images/VS01-large.png#lightbox)

5. 変更内容を保存します。
6. クリーン ビルドを実行します。

-----

ARMv7s は、iPhone 5 (またはそれ以上) に含まれる A6 プロセッサでのみサポートされます。 ARMv7 コードは、ARMv6 より小さくて高速です、のみ iPhone 3GS 以降の機能および iPad または最低限の iOS バージョン 5.0 の対象とする場合に、Apple が必要です。 ARMv6 では、すべてのデバイスで機能しますが、Xcode 4.5 以降に付属するコンパイラではサポートされなく。 

ARM64 は iPhone 6、またはその他の 64 ビットのデバイスで iOS 8 をサポートするために必要なし、新規または iTunes App Store でアプリケーションを終了して更新を送信するときに、Apple で必要になります。

Apple の確認、さまざまな iOS デバイスの機能の包括的なについては、[デバイスとの互換性](https://developer.apple.com/library/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/DeviceCompatibilityMatrix/DeviceCompatibilityMatrix.html)ドキュメント。

### <a name="64-bit-and-binary-size-increases"></a>64 ビット版とバイナリのサイズが増加

32 ビットから 64 ビット iOS Apple の遷移中には、アプリは、32 ビットと 64 ビットの両方のハードウェア上で実行する必要があります。 このためは、Xamarin の Unified API には、両方を対象とする開発者が使用できます。

32 ビットと 64 ビットの両方のアーキテクチャを対象とすると、アプリケーションのサイズが大幅に向上します。 ただし、そのため、古いデバイスをサポートしながら、最適化されたコードを実行する新しいデバイスできます。

> [!IMPORTANT]
> ITunes App Store に iOS アプリケーションを送信するときに、次のメッセージが表示される場合 _"警告 ITMS 9000: 64 ビットのサポートがありません。2015 年 2 月 1 日の新しい iOS の開始、App Store にアップロードされたアプリは 64 ビットのサポートを含める必要があり、8 SDK、Xcode 6 以降に含まれる、iOS のビルドです。有効にするプロジェクトで、64 ビットをお勧め既定の Xcode ビルドの「標準的なアーキテクチャ」の設定を使用して 1 つのバイナリを 32 ビットと 64 ビットの両方のコードをビルドする"。_ サポートされているアーキテクチャを利用可能ないずれかに切り替える必要がある**ARM64**組み合わせ (前述のように)、再コンパイルと再送信します。

## <a name="mac"></a>Mac

> [!IMPORTANT]
> 2018 年 1 月以降、Mac App Store に送信されたすべての新しい Mac アプリは 64 ビットをサポートする必要があります。 既存の Mac App Store アプリ、その更新プログラムは 2018 年 6 月で 64 ビットの起動をサポートする必要があります。 参照してください[Apple の発表](https://developer.apple.com/news/?id=06282017a)と[を 64 ビット、Xamarin.Mac アプリを更新する方法を説明するガイド](~/cross-platform/macios/32-and-64/mac-64-bit.md)します。

ほとんどの最新の Mac コンピューターでは、32 ビットと 64 ビットの両方のアプリケーションをサポートします。   MacOS 10.6 (雪ヒョウ柄) は、32 ビット システムで実行する最後のオペレーティング システムをでした。   ほとんどの Mac 2010 両方のシステムをサポートするためにリリースします。

異なり iOS、macOS の最新バージョンで導入された新しいフレームワークの多くがのみサポート (CloudKit、EventKit GameController、LocalAuthentication、MediaLibrary、MultipeerConnectivity、NotificationCenter、GLKit、SpriteKit、ソーシャル、64 ビット モードでおよび 他のユーザーの間での MapKit) で変更します。

Unified API を生成するアプリケーションの種類を選択する開発者を許可する: 32 ビットまたは 64 ビット。

**32 ビット アプリケーション**は 32 ビットと 64 ビットの両方の Mac コンピューターで実行、アドレス空間を 32 ビットに制限していて、すべてのライブラリが 32 ビットである必要があります。

64 ビット モードで実行されている 32 ビットの依存関係がある場合より小さいダウンロードする場合、または 64 ビットに移行する際にパフォーマンス上の利点がない場合に通常、このモードを使用します。

このモードは、macOS Mavericks および macOS Yosemite で使用できる多くのフレームワークを使用することはできません。 が制限されます。

**64 ビット アプリケーション**は 64 ビット Mac デバイスでのみ実行します。

For Mac では、これは、優先モード操作の現在サポートして 64 ビット モードでは、使用中のほとんどの Mac と Apple によって提供されるフレームワークの完全なセットへのアクセスがあります。

### <a name="enabling-64-bit-builds-of-xamarinmac-apps"></a>Xamarin.Mac アプリのビルドを 64 ビットの有効化

Xamarin.Mac を使用して 64 ビット アプリケーションを構築する方法の詳細についてを参照してください、[を 64 ビット アプリケーションの更新 Xamarin.Mac Unified](~/cross-platform/macios/32-and-64/mac-64-bit.md)ガイド。

## <a name="related-links"></a>関連リンク

- [クラシックと Unified API の相違点](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
