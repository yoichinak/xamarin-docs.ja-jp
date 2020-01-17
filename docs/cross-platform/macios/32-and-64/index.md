---
title: 32/64 ビットプラットフォームに関する考慮事項
description: このドキュメントでは、Xamarin.iOS または Xamarin.Mac アプリケーションの32ビットアーキテクチャと64ビットアーキテクチャを対象とする場合に留意すべきさまざまな考慮事項について説明します。
ms.prod: xamarin
ms.assetid: F7126340-04B2-4A10-B14D-394E23527C1A
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: 5ba451de857444bc5b12b750ae479b62abdb75a3
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016331"
---
# <a name="3264-bit-platform-considerations"></a>32/64 ビットプラットフォームに関する考慮事項

iOS と macOS は、従来は 32 と 64 ビットの両方のアプリをサポートしていましたが、Apple は、非推奨の 32 ビットサポートを徐々に廃止しています。

iOS 11 以降では、32ビットのアプリは起動されなくなり、[App Store へのすべての送信は64ビットをサポートする必要があり](https://developer.apple.com/news/?id=06282017b)ます。

2018年1月以降、 [Mac App Store に送信された新しいアプリは64ビットをサポートする必要があり](https://developer.apple.com/news/?id=06282017a)、既存のアプリは2018年の6月までに更新する必要があります。

Xamarin の Classic API (`XamMac.dll` と `monotouch.dll`) では、32ビットアプリケーションのみがサポートされていました。 ただし、新しい Xamarin.iOS および Xamarin.Mac アプリケーションでは、既定で [Unified API](~/cross-platform/macios/unified/index.md) (`Xamarin.iOS` と `Xamarin.Mac`) が使用されるため、必要に応じて32と64ビットの両方をターゲットにすることができます。

## <a name="ios"></a>iOS

<a name="enable-64" />

### <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Xamarin iOS アプリの64ビットビルドを有効にする

> [!WARNING]
> このセクションは、歴史的な理由のために含まれており、古い Xamarin. iOS プロジェクトを Unified API に移動し、64ビットをサポートします。 すべての新しい Xamarin. iOS プロジェクトでは、既定で Unified API とターゲット64が使用されます。

Unified API に変換された Xamarin の iOS モバイルアプリケーションでは、開発者はビルド設定を64ビットのターゲットに手動で更新する必要があります。

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Solution Pad**で、アプリのプロジェクトをダブルクリックして、 **[プロジェクトオプション]** ウィンドウを開きます。
2. **[IOS ビルド]** を選択します。
3. IPhone シミュレーターの **[サポートされているアーキテクチャ]** ドロップダウンで、 **x86\_64**または**i386 + x86\_64**を選択します。

   [![サポートされているアーキテクチャを x86\_64 または i386 + x86\_64 に設定する](Images/Image01.png "Setting Supported architectures to x86\_64 or i386 + x86\_64")](Images/Image01-large.png#lightbox) 

4. 物理デバイスの場合は、使用可能な**ARM64**の組み合わせのいずれかを選択します。

   [![サポートされているアーキテクチャを ARM64 の組み合わせのいずれかに設定する](Images/Image02.png "サポートされているアーキテクチャを ARM64 の組み合わせのいずれかに設定する")](Images/Image02-large.png#lightbox)

5. **[OK]** をクリックします。
6. クリーンビルドを実行します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **ソリューションエクスプローラー**で、アプリのプロジェクトを右クリックし、 **[プロパティ]** を選択します。
2. **[IOS ビルド]** を選択します。
3. IPhone シミュレーターでは、**サポートされているアーキテクチャ**を**x86\_64**または**i386 + x86\_64**のいずれかに設定します。 

   [![サポートされているアーキテクチャを x86_64 または i386 + x86\_64 に設定する](Images/VS02.png "Setting Supported architectures to x86_64 or i386 + x86\_64")](Images/VS02-large.png#lightbox)

4. 物理デバイスの場合は、使用可能な**ARM64**の組み合わせのいずれかを選択します。
    
   [![サポートされているアーキテクチャを ARM64 の組み合わせのいずれかに設定する](Images/VS01.png "サポートされているアーキテクチャを ARM64 の組み合わせのいずれかに設定する")](Images/VS01-large.png#lightbox)

5. 変更内容を保存します。
6. クリーンビルドを実行します。

-----

ARMv7s は、iPhone 5 (またはそれ以降) に含まれている A6 プロセッサでのみサポートされています。 ARMv7 コードは、ARMv6 よりも高速で小さく、iPhone 3GS 以降でのみ動作し、iPad または最小の iOS バージョン5.0 を対象とする場合に Apple で必要とされます。 ARMv6 はすべてのデバイスで動作しますが、Xcode 4.5 以降に付属するコンパイラではサポートされなくなりました。 

ARM64 は、iPhone 6 またはその他の64ビットデバイスで iOS 8 をサポートするために必要であり、iTunes App Store で新規または既存のアプリケーションを更新するときに Apple によって必要になります。

さまざまな iOS デバイスの機能を包括的に確認するには、Apple の[デバイスの互換性](https://developer.apple.com/library/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/DeviceCompatibilityMatrix/DeviceCompatibilityMatrix.html)に関するドキュメントを参照してください。

### <a name="64-bit-and-binary-size-increases"></a>64-ビットサイズとバイナリサイズの増加

Apple の32ビットから64ビットへの移行中は、iOS アプリを32ビットハードウェアと64ビットハードウェアの両方で実行する必要があります。 このため、Xamarin の Unified API により、開発者は両方をターゲットにすることができます。

32ビットアーキテクチャと64ビットアーキテクチャの両方をターゲットにすると、アプリケーションのサイズが大幅に増加します。 ただし、これにより、古いデバイスをサポートしながら、より新しいデバイスで最適化されたコードを実行できるようになります。

> [!IMPORTANT]
> IOS アプリケーションを iTunes App Store に送信するときに次のメッセージが表示された場合は、 _"WARNING ITMS-9000:64 ビットのサポートがありません。2015年2月1日以降、アプリストアにアップロードされた新しい iOS アプリには64ビットのサポートが含まれている必要があります。また、Xcode 6 以降に含まれる iOS 8 SDK を使用してビルドする必要があります。プロジェクトで64ビットを有効にするには、"Standard アーキテクチャ" の既定の Xcode ビルド設定を使用して、32ビットと64ビットの両方のコードで1つのバイナリを構築することをお勧めします。_ サポートされているアーキテクチャを、(上記のように) 使用可能な**ARM64**の組み合わせのいずれかに切り替える必要があります。再コンパイルして再送信します。

## <a name="mac"></a>Mac

> [!IMPORTANT]
> 2018年1月以降、Mac App Store に送信されたすべての新しい Mac アプリが64ビットをサポートしている必要があります。 既存の Mac App Store アプリとその更新プログラムは、2018年6月以降、64ビット版をサポートする必要があります。 [Apple の発表](https://developer.apple.com/news/?id=06282017a)と、 [Xamarin アプリを64ビットに更新する方法について説明しているガイド](~/cross-platform/macios/32-and-64/mac-64-bit.md)を参照してください。

最新の Mac コンピューターのほとんどは、32ビットアプリケーションと64ビットアプリケーションの両方をサポートしています。   MacOS 10.6 (雪 Leopard) は、32ビットシステムで実行する最後のオペレーティングシステムでした。   2010以降にリリースされたほとんどの Mac では、両方のシステムがサポートされます。

IOS とは異なり、macOS の最近のバージョンで導入された新しいフレームワークの多くは、64ビットモード (CloudKit、EventKit、GameController、LocalAuthentication、MediaLibrary、MultipeerConnectivity、NotificationCenter、GLKit、SpriteKit、ソーシャル) でのみサポートされています。と MapKit など)。

Unified API を使用すると、開発者が生成するアプリケーションの種類 (32 ビットまたは64ビット) を選択できます。

**32 ビットアプリケーション**は、32ビットと64ビットの Mac コンピューターの両方で実行され、アドレス空間は32ビットに制限されており、すべてのライブラリが32ビットである必要があります。

このモードは、通常、64ビットモードでは実行されない32ビットの依存関係がある場合、ダウンロードを小さくする場合、または64ビットへの移行にパフォーマンス上の利点がない場合に使用します。

このモードは、macOS Mavericks と macOS で利用できる多くのフレームワークを使用できないため、制限されています。

**64 ビットアプリケーション**は、64ビットの Mac デバイスでのみ実行されます。

Mac では、現在使用されているほとんどの Mac が64ビットモードをサポートしており、Apple によって提供されるすべてのフレームワークのセットにアクセスできるため、これは推奨される操作モードです。

### <a name="enabling-64-bit-builds-of-xamarinmac-apps"></a>Xamarin. Mac アプリの64ビットビルドを有効にする

Xamarin. Mac を使用した64ビットアプリのビルドの詳細については、「 [xamarin. Mac 統合アプリケーションを64ビットに更新する](~/cross-platform/macios/32-and-64/mac-64-bit.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [クラシックと Unified API の違い](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
