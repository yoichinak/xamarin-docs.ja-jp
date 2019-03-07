---
title: Windows に Xamarin.iOS をインストールする
description: このドキュメントでは、Windows コンピューターを設定する方法、Mac ビルド ホストを設定する方法、Xamarin.iOS 開発のために Windows を Mac にペアリングする方法について説明しています。
ms.prod: xamarin
ms.assetid: abf85d3e-a365-44a2-b1a4-6c572c7f76dd
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/16/2018
---

# <a name="installing-xamarinios-on-windows"></a>Windows に Xamarin.iOS をインストールする

_この記事では、Xamarin.iOS の開発用に Windows コンピューターおよび Mac ビルド ホストをセットアップする方法について説明します。_

## <a name="overview"></a>概要

Windows 上で Visual Studio 2017 を使って Xamarin.iOS アプリを構築するには、次のものが必要です。
 
-  Visual Studio 2017 がインストールされている Windows コンピューター。 物理コンピューターでも仮想マシンでもかまいません。
    - [Windows のシステム要件](~/cross-platform/get-started/requirements.md#windows-requirements)
    
-  Apple のビルド ツールと Xamarin.iOS でセットアップされたネットワーク アクセス可能な Mac。 Visual Studio 2017 は、ネットワーク接続経由でこのマシンにアクセスして Apple のビルド ツールを使用します。ネイティブ iOS アプリケーションをコンパイルするために必要です。 
    - [Mac のシステム要件](~/cross-platform/get-started/requirements.md#macos-requirements)

## <a name="setup"></a>セットアップ

Visual Studio 2017 を Xamarin.iOS 開発用にセットアップするには、以下の手順のようにします。

1. Windows をセットアップする (Visual Studio 2017 をインストールする)

    Xamarin.iOS は、スタンドアロン マシンまたは仮想マシン上の Visual Studio 2017 Community、Professional、Enterprise エディションで動作します。
    
    - [Visual Studio 2017 をインストールします](~/get-started/installation/windows.md)。

2. Mac をセットアップする (Xcode と Visual Studio for Mac をインストールする)

    iOS アプリケーションを配布用にビルド、デバッグ、署名するには、Apple の開発者ツール (Xcode) と Xamarin.iOS の両方で構成された Mac ビルド ホストに、Visual Studio 2017 がネットワーク アクセスできる必要があります。

    - [Mac App Store から Xcode をダウンロードしてインストールします](https://itunes.apple.com/us/app/xcode/id497799835?mt=12)。 
    - [Visual Studio for Mac をインストールします](https://docs.microsoft.com/visualstudio/mac/installation)。Xamarin.iOS もインストールされます。

    > [!NOTE] 
    > Visual Studio for Mac をインストールしない方がよい場合は、[Visual Studio 2017 バージョン 15.6](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes#automatic-macos-provisioning) 以降では、Visual Studio 2017 は Mac ビルド ホストを Xamarin.iOS アプリケーションのビルドに必要なソフトウェアで自動的に構成できます。 詳しくは、「[Mac の自動プロビジョニング](~/ios/get-started/installation/windows/connecting-to-mac/index.md#automatic-mac-provisioning)」をご覧ください。

3. Mac とペアリング (Visual Studio 2017 を Mac に接続する)

    Visual Studio 2017 で Mac 上の iOS ビルド ツールを使用するには、2 台のマシンがネットワークで接続している必要があります。

    - 「[Mac とペアリング](~/ios/get-started/installation/windows/connecting-to-mac/index.md)」ガイドをお読みください。

## <a name="summary"></a>まとめ

この記事では、Xamarin.iOS の開発用に Windows コンピューターおよびそれに関連付けられた Mac ビルド ホストをセットアップする方法について説明しました。

## <a name="next-steps"></a>次の手順

- [Xamarin.iOS for Visual Studio の概要](introduction-to-xamarin-ios-for-visual-studio.md)
- [Visual Studio 2017 の構成](config-options.md)
- [デバイスのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)
