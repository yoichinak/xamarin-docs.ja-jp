---
title: 32/64 ビット プラットフォームの考慮事項
description: アプリケーションの 32 ビットおよび 64 ビットのアーキテクチャを対象とする場合の考慮事項
ms.prod: xamarin
ms.assetid: F7126340-04B2-4A10-B14D-394E23527C1A
ms.technology: xamarin-cross-platform
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: 06e65b8a639345af65c6e5d3bbe57d8bb3feffa1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="3264-bit-platform-considerations"></a>32/64 ビット プラットフォームの考慮事項

IOS や macOS がこれまでサポートされている 32 と 64 ビットの両方のアプリ、Apple が徐々 に 32 ビットをサポートする推奨されなくなりました。

11、iOS の時点で 32 ビット アプリは起動されなく、および[アプリ ストアへの送信は、64 ビットをサポートする必要があります](https://developer.apple.com/news/?id=06282017b)です。

以降では、年 2018年 1 月[Mac App Store に送信された新しいアプリは、64 ビットをサポートする必要があります](https://developer.apple.com/news/?id=06282017a)、年 2018年 6 月で既存のアプリを更新する必要があります。

Xamarin の Classic API (`XamMac.dll`と`monotouch.dll`) のみの 32 ビット アプリケーションをサポートします。 ただし、新しい Xamarin.Mac、Xamarin.iOS およびアプリケーションを使用して、 [Unified API](~/cross-platform/macios/unified/index.md) (`Xamarin.iOS`と`Xamarin.Mac`) 既定とそのため、32 と 64 ビット、必要に応じて、ターゲットを実行できます。

## <a name="ios"></a>iOS

<a name="enable-64" />

### <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Xamarin.iOS アプリのビルド時に 64 ビットの有効化

> [!WARNING]
> このセクションでは、履歴上の理由、および Unified API に古い Xamarin.iOS プロジェクトを移動し、64 ビットをサポートするために含まれます。 すべての新しい Xamarin.iOS プロジェクトでは、既定では、Unified API と 64 ビットのターゲットが使用されます。

Xamarin.iOS モバイル アプリケーションでは、Unified API に変換されたの開発者は 64 ビットのターゲットをビルド設定を手動で更新する必要があります。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **ソリューション パッド**、ダブルクリックして、アプリのプロジェクトを開くには、**プロジェクト オプション**ウィンドウです。
2. 選択**iOS ビルド**です。
3. IPhone シミュレーター用で、**サポートされているアーキテクチャ**ドロップダウン リストで、いずれかを選択**x86\_64**または**i386 + x86\_64**:

   [![サポートされているアーキテクチャを x86 に設定\_64 または i386 + x86\_64](Images/Image01.png "Setting Supported architectures to x86\_64 or i386 + x86\_64")](Images/Image01-large.png#lightbox) 

4. 物理デバイスの場合、使用可能なのいずれかを選択**ARM64**の組み合わせ。

   [![サポートされているアーキテクチャを ARM64 の組み合わせのいずれかに設定](Images/Image02.png "ARM64 組み合わせのいずれかに設定がサポートされているアーキテクチャ")](Images/Image02-large.png#lightbox)

5. **[OK]**をクリックします。
6. クリーン ビルドを実行します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **ソリューション エクスプ ローラー**をアプリのプロジェクトを右クリックし、**プロパティ**です。
2. 選択**iOS ビルド**です。
3. Iphone シミュレーターでは、次のように設定します**サポートされているアーキテクチャ**いずれかに**x86\_64**または**i386 + x86\_64**:。 

   [![サポートされているアーキテクチャを x86_64 または i386 + x86 に設定\_64](Images/VS02.png "Setting Supported architectures to x86_64 or i386 + x86\_64")](Images/VS02-large.png#lightbox)

4. 物理デバイスの場合、使用可能なのいずれかを選択**ARM64**の組み合わせ。
    
   [![サポートされているアーキテクチャを ARM64 の組み合わせのいずれかに設定](Images/VS01.png "ARM64 組み合わせのいずれかに設定がサポートされているアーキテクチャ")](Images/VS01-large.png#lightbox)

5. 変更内容を保存します。
6. クリーン ビルドを実行します。

-----

ARMv7s は、iPhone 5 (またはそれ以上) に含まれる A6 プロセッサでのみサポートされます。 常に ARMv7 コードは高速であり、ARMv6 よりも小さい、のみ iPhone 3GS 以降の動作および iPad または iOS の最小バージョン 5.0 の対象とする場合に、Apple が必要です。 ARMv6 では、すべてのデバイスでも動作しますが、Xcode 4.5 以降に付属するコンパイラではサポートされなくです。 

ARM64 は iPhone 6 またはその他の 64 ビットのデバイスで iOS 8 をサポートするために必要なし、新規または iTunes App Store でアプリケーションを終了して更新を送信するときに Apple によって必要があります。

さまざまな iOS デバイスの機能の包括的なについては、チェックは、Apple のアウト[デバイス互換性](https://developer.apple.com/library/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/DeviceCompatibilityMatrix/DeviceCompatibilityMatrix.html)ドキュメント。

### <a name="64-bit-and-binary-size-increases"></a>64 ビット版とバイナリのサイズを増加します。

32 ビットから 64 ビットへの iOS から Apple の遷移中には、アプリは、32 ビットおよび 64 ビットの両方のハードウェア上で実行する必要があります。 このためは、Xamarin の Unified API は、両方を対象とする開発者をできます。

32 ビットおよび 64 ビット アーキテクチャの両方を対象とすると、アプリケーションのサイズが大幅に向上します。 ただし、そのためは新しいデバイスを許可する古いデバイスをサポートしながら、最適化されたコードを実行します。

> [!IMPORTANT]
> 場合は、iTunes App Store に iOS アプリケーションを送信するときに、次のメッセージが表示される_"警告 ITMS 9000: 64 ビットのサポートがありません。アプリ ストアにアップロードされたアプリでは 2015 年 2 月 1 日の新しい iOS を開始、64 ビット サポートを含める必要があります、8 SDK、Xcode 6 以降が含まれている ios ビルドします。有効にする 64 ビットのプロジェクトで使用をお勧め Xcode ビルド「標準的なアーキテクチャ」の設定の既定値を 1 つのバイナリを 32 ビットおよび 64 ビットの両方のコードをビルドします"。_ サポートされているアーキテクチャを使用可能なのいずれかに切り替える必要がある**ARM64**組み合わせ (上記のように)、再コンパイルおよび再実行してください。

## <a name="mac"></a>Mac

> [!IMPORTANT]
> 年 2018年 1 月以降、Mac App Store に送信されたすべての新しい Mac アプリケーションは 64 ビットをサポートする必要があります。 既存の Mac App Store アプリ、その更新プログラムは年 2018年 6 月で 64 ビットの起動をサポートする必要があります。 参照してください[Apple のアナウンス](https://developer.apple.com/news/?id=06282017a)と[を 64 ビット Xamarin.Mac アプリを更新する方法について説明するガイド](~/cross-platform/macios/32-and-64/mac-64-bit.md)です。

最新の Mac コンピューターでは、32 ビットと 64 ビットの両方のアプリケーションをサポートします。   MacOS 10.6 (ユキヒョウ) は、32 ビット システムで実行する最後のオペレーティング システムをでした。   ほとんどの mac コンピューターのリリース以降に 2010 で両方のシステムはサポートします。

異なり、iOS の macOS の最近のバージョンで導入された新しいフレームワークの多くは、のみサポート (CloudKit、EventKit GameController、LocalAuthentication、MediaLibrary、MultipeerConnectivity、NotificationCenter、GLKit、SpriteKit、ソーシャル、64 ビット モードでその他の MapKit)。

Unified API により、開発者を生成するアプリケーションの種類を選択します。 32 ビットまたは 64 ビット。

**32 ビット アプリケーション**32 ビットおよび 64 ビットの両方の Mac コンピューターで実行は、32 ビットに制限されるアドレス空間を持っておよびすべてのライブラリが 32 ビットである必要があります。

小さい、ダウンロードする場合、64 ビット モードで実行されている 32 ビットの依存関係がある場合、または 64 ビットへ移行するパフォーマンス上の利点がない場合は、通常このモードを使用するされます。

このモードが macOS Mavericks および macOS Yosemite で使用できる多くのフレームワークを使用することはできませんとを制限します。

**64 ビット アプリケーション**64 ビット Mac のデバイスでのみ実行されます。

Mac、これは、推奨されるモードの操作の使用中のほとんどの Mac が今日 64 ビット モードをサポートし、Apple によって提供されるフレームワークの完全なセットへのアクセスがあるようです。

### <a name="enabling-64-bit-builds-of-xamarinmac-apps"></a>Xamarin.Mac アプリのビルド時に 64 ビットの有効化

Xamarin.Mac を使用して 64 ビット アプリケーションを構築する方法の詳細についてを参照してください、[を 64 ビット アプリケーションの更新 Xamarin.Mac Unified](~/cross-platform/macios/32-and-64/mac-64-bit.md)ガイドです。

## <a name="related-links"></a>関連リンク

- [従来の vs Unified API の相違点](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
