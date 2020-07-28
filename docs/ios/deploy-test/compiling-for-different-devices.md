---
title: Xamarin.iOS の各種デバイス向けコンパイル
description: このドキュメントでは、各種デバイス用に Xamarin.iOS ビルドをカスタマイズするために使用できる、さまざまなビルド構成オプションについて説明します。
ms.prod: xamarin
ms.assetid: 3B259248-887E-3E4F-E09C-7AD28C2A8CEE
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 2f70dd3b18c36d478548672bb78d329cb2a4c9ab
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938776"
---
# <a name="compiling-for-different-devices-in-xamarinios"></a>Xamarin.iOS の各種デバイス向けコンパイル

実行可能ファイルのビルド プロパティは、プロジェクトの **iOS ビルド**のプロパティ ページから設定できます。このページは、プロジェクト名を右クリックし、 **[オプション]、[iOS ビルド]** の順に移動するか (Visual Studio for Mac)、 **[プロパティ]** に移動します (Visual Studio)。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

[![プロジェクト iOS ビルドのプロパティ ページ](compiling-for-different-devices-images/image1.png)](compiling-for-different-devices-images/image1.png#lightbox) 

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![プロジェクト iOS ビルドのプロパティ ページ](compiling-for-different-devices-images/image1a.png)](compiling-for-different-devices-images/image1a.png#lightbox)

-----

UI で利用できる構成オプションに加え、独自のコマンド ライン オプション セットを [Xamarin.iOS ビルド ツール (mtouch)](~/ios/deploy-test/mtouch.md) に渡すこともできます。

## <a name="sdk-options"></a>SDK オプション

Visual Studio for Mac では、SDK に関連する 2 つの重要なプロパティを構成できます。ソフトウェアのビルドに利用される iOS SDK バージョンと配置ターゲット (または必要な最小 iOS バージョン) です。

iOS **SDK バージョン** オプションでは、Apple が公開している SDK をさまざまなバージョンで利用できます。これはビルド中に参照すべきコンパイラ、リンカー、ライブラリを Xamarin.iOS に指示します。 プロジェクトを右クリックして **[オプション]** を選択し、オプション ウィンドウで **[iOS ビルド]** を選択します。

[![オプション ウィンドウで SDK バージョンを選択する](compiling-for-different-devices-images/sdk-version-sml.png)](compiling-for-different-devices-images/sdk-version.png#lightbox)

**配置ターゲット**設定では、アプリケーションを実行するオペレーティング システムの必要な最小バージョンを選択します。 これはプロジェクトの **Info.plist** ファイルで設定されます。 アプリケーションを実行するために必要なすべての API が含まれる最小のバージョンを選択してください。

[![Info.plist ファイルで配置ターゲットを設定する](compiling-for-different-devices-images/deployment-target-sml.png)](compiling-for-different-devices-images/deployment-target.png#lightbox)

通常、Xamarin.iOS API は最新版の SDK で利用できるすべてのメソッドを公開します。Microsoft では、必要に応じて、実行時に機能が利用できるかどうかを検出する便利なプロパティを提供しています (たとえば、`UIDevice.UserInterfaceIdiom` と `UIDevice.IsMultitaskingSupported` は常に Xamarin.iOS で動作し、Microsoft はバックグラウンドのあらゆる仕事を行っています)。

## <a name="linking"></a>リンク

Microsoft の[リンカー](~/ios/deploy-test/linker.md)専用ページをご覧ください。実行可能ファイルのサイズを減らし、実行可能ファイルを効果的に使用するためにリンカーがいかに役立つかをご確認いただけます。

## <a name="code-generation-engine"></a>コード生成エンジン

Xamarin.iOS 4.0 以降、2 つのコード生成バックエンドが Xamarin.iOS に加わりました。 通常の Mono コード生成エンジンと LLVM 最適化コンパイラを基盤とするコード生成エンジンです。 各エンジンには長所と短所があります。

通常、開発プロセスにおいては、イテレーションの速い Mono コード生成エンジンを使用することが多いです。 リリース ビルドと AppStore 配置の場合、LLVM コード生成エンジンに切り替えることがあります。

LLVM 最適化バックエンド エンジンは、Mono エンジンより簡潔なコードを短時間で生成できますが、コンパイル時間が長くなります。

生成エンジンは、Visual Studio for Mac または Visual Studio の iOS ビルド オプションから有効にすることができます。

[![LLVM を有効にする](compiling-for-different-devices-images/image2.png)](compiling-for-different-devices-images/image2.png#lightbox)

[![LLVM を有効にする](compiling-for-different-devices-images/image2a.png)](compiling-for-different-devices-images/image2a.png#lightbox)

## <a name="architecture-support"></a>アーキテクチャ サポート

### <a name="armv6-xamarinios-discontinued-support-for-armv6-with-v810"></a>ARMv6 (Xamarin.iOS では、v8.10 で ARMv6 のサポートが終了しました)

- iPhone (オリジナル)、3G
- iPod 第 1、2 世代

### <a name="armv7"></a>ARMv7

- iPhone 3GS、4、4S
- iPad 1、2、3、Mini
- iPod 第 3、4、5 世代

### <a name="armv7s"></a>ARMv7s

- iPhone 5
- iPhone 5c
- iPad 4

ARMv7s プロセッサのみを対象とする場合、わずかに速いコードが生成されますが、パッケージに複数の実行可能ファイルが含まれる FAT バイナリをコンパイルしない限り、ARMv7 または ARMv6 システムでは実行されません。

### <a name="arm64-xamarinios-started-supporting-arm64-in-v86"></a>ARM64 (Xamarin.iOS では、v8.6 で ARM64 のサポートを開始しました)

- iPhone 5s
- iPhone SE
- iPhone 6、6 Plus
- iPhone 6s、6s Plus
- iPhone 7、7 Plus
- iPhone 8、8 Plus
- iPhone X
- iPad Air
- iPad Air 2
- iPad Mini 2、3、4
- iPad Pro (すべて)

App Store に提出するビルドには、64 ビット サポートを含める必要があります。これは [Apple](https://developer.apple.com/news/?id=12172014b) が定めた要件です。 また、iOS 11 は 64 ビット アプリケーションにのみ対応しています。

### <a name="arm-thumb-2-support"></a>ARM Thumb-2 サポート

Thumb は、ARM プロセッサで利用される、より簡潔な命令セットです。 Thumb サポートを有効にすれば、実行時間が長くなりますが、実行可能ファイルのサイズを減らすことができます。 Thumb は ARMv7 と ARMv7s でサポートされています。

## <a name="conditional-framework-usage"></a>フレームワークの条件付き利用

プロジェクトで最新の iOS リリースの機能の一部を活用したい場合は、条件付きで新しいフレームワークを利用する必要があります。 この典型的な例は、iOS 4.0 以降で iAd を利用し、3.x デバイスにも対応するというものです。 これを達成するには、iAd フレームワークに対して "弱く" リンクする必要があることを Xamarin.iOS に認識させる必要があります。 結合が弱いことで、フレームワークからのクラスが初めて必要になったときにだけフレームワークが要求に応じて読み込まれます。

これを行うには、次の手順を実行する必要があります。

- **[プロジェクト オプション]** を開き、 **[iOS ビルド]** ウィンドウに移動します。
- 弱くリンクする構成ごとに **[追加オプション]** に `'-gcc_flags "-weak_framework iAd"'` を追加します。

[![その他のオプション](compiling-for-different-devices-images/image3.png)](compiling-for-different-devices-images/image3.png#lightbox)

これに加え、型の処理が旧バージョンの iOS で実行されないようにする必要があります。その型が旧バージョンにはない可能性があります。 これを行う方法はいくつかありますが、その 1 つは `UIDevice.CurrentDevice.SystemVersion` を解析することです。

## <a name="related-links"></a>関連リンク

- [リンカー](~/ios/deploy-test/linker.md)
