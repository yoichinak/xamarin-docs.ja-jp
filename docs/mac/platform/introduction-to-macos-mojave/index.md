---
title: macOS Mojave の概要
description: このドキュメントでは、macOS Mojave の新機能と更新された機能の概要について説明します。
ms.prod: xamarin
ms.assetid: 4A41CD85-C807-44C9-85AB-B5441B145A73
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 10/05/2018
ms.openlocfilehash: 8c296e32fd0d1491c83ea3cb16e0921c0f0c5f12
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73017043"
---
# <a name="introduction-to-macos-mojave"></a>macOS Mojave の概要

このドキュメントでは、macOS Mojave の新機能と更新された機能の概要について説明します。

Xamarin を使用した macOS Mojave アプリの構築を開始するには、 [xamarin. Mac 5.0](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/mac/xamarin.mac_5/xamarin.mac_5.0.md)の[ファーストステップガイド](~/mac/platform/introduction-to-macos-mojave/get-started.md)を参照してください。

## <a name="dark-mode"></a>ダーク モード

ダークモードは、macOS Mojave のシステム全体にわたるダークテーマで、動的なダークグレーの配色を使用してユーザーインターフェイス要素を表示します。 また、ユーザーの色の設定に関係なく、サードパーティのアプリの外観を向上させるために、新しいアクセントカラー、色効果、およびコンテンツ濃淡の色が導入されています。

## <a name="user-notifications-framework"></a>ユーザー通知フレームワーク

ユーザー通知フレームワークは macOS Mojave に含まれており、Mac アプリがユーザー通知を操作するために使用する Api を変更します。

## <a name="natural-language-framework"></a>自然言語フレームワーク

自然言語フレームワークを使用すると、アプリケーションはさまざまな種類の言語分析を実行できます。 たとえば、音声の一部を識別し、テキストブロックで表される言語を特定するために使用できます。

## <a name="vision-framework"></a>ビジョンフレームワーク

ビジョンフレームワークには、さまざまな向きで顔を検出できる改善された顔検出機能が含まれています。 また、要求のリビジョンを使用して、特定のビジョンフレームワークアルゴリズムリビジョンを選択できるようになりました。

## <a name="network-framework"></a>ネットワークフレームワーク

Network framework は、iOS アプリケーションで一般的に使用される `URLSession` Api の基礎となるネットワークスタックをスタンドアロンフレームワークとして使用できるようになり、TCP、UDP、TLS、IPv4/IPv6 などを簡単に操作できるようになりました。

## <a name="deprecations"></a>廃止

MacOS Mojave では、Apple は OpenGL ES と OpenCL を非推奨にし、[開発者](https://developer.apple.com/macos/whats-new/)が金属および金属のパフォーマンスシェーダーを採用するようにしています。

## <a name="related-links"></a>関連リンク

- [Xamarin. Mac サンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Mac)
- [macOS – Apple Developer](https://developer.apple.com/macos/)
- [Xamarin. Mac 5.0 リリースノート](https://docs.microsoft.com/xamarin/mac/release-notes/5/5.0/)
