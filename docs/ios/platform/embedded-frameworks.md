---
title: Xamarin.iOS で埋め込みフレームワーク
description: このドキュメントでは、Xamarin.iOS アプリケーションでの埋め込みのフレームワークとコードを共有する方法について説明します。 これは、mtouch ツールまたはネイティブ参照のいずれかで実行できます。
ms.prod: xamarin
ms.assetid: F8C61020-4106-46F1-AECB-B56C909F42CB
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/05/2018
ms.openlocfilehash: b59fd7c1a9e5f528878b90e1a76fabe5a79bab81
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "60946825"
---
# <a name="embedded-frameworks-in-xamarinios"></a>Xamarin.iOS で埋め込みフレームワーク

_このドキュメントでは、アプリケーション開発者が自分のアプリでユーザーのフレームワークを埋め込むことができる方法について説明します。_

8.0 Apple は iOS で行ったアプリ拡張機能と Xcode でメイン アプリケーションの間のコードを共有する埋め込みのフレームワークを作成することです。

Xamarin.iOS 9.0 では、Xamarin.iOS アプリでこれらの組み込みフレームワーク (Xcode で作成された) を使用するためのサポートを追加します。 **いない** *Xamarin.iOS プロジェクトの任意の型から埋め込みフレームワークを作成するには、既存のネイティブ (OBJECTIVE-C) フレームワークにのみ使用できます*。

Xamarin.iOS でフレームワークを使用する 2 つの方法はあります。

- プロジェクトの追加 mtouch 引数に、次を追加することで、mtouch ツールのフレームワークを渡す**iOS ビルド**オプション。

  ```csharp
  --framework:/Path/To/My.Framework
  ```

  これは、各プロジェクト構成に設定します。

- コンテキスト メニューからのネイティブ参照を追加します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

ネイティブ参照を追加するには、プロジェクトと参照を右クリックします。

![](embedded-frameworks-images/xam-native-refs.png "Visual studio for Mac ネイティブ参照の追加を選択します")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

ネイティブ参照を追加するには、プロジェクトと参照を右クリックします。

![](embedded-frameworks-images/vs-native-refs.png "Visual Studio でネイティブ参照の追加を選択します。")

-----

  これにより、すべての構成は機能します。

将来のバージョンの Visual Studio for Mac と Visual Studio の Xamarin ツールでは (プロジェクト ファイルを手動で編集するには) なし、IDE 内からフレームワークを使用することができます。

いくつかのサンプル プロジェクトが見つかりません[github](https://github.com/rolfbjarne/embedded-frameworks)

## <a name="limitations"></a>制限事項

- 埋め込みフレームワークでのみサポート[統合](~/cross-platform/macios/unified/index.md)プロジェクト。
- 埋め込みフレームワークはのみサポートでのデプロイ ターゲットでのプロジェクトには少なくとも iOS 8.0。
- 拡張機能は、埋め込みのフレームワークを必要とする場合、コンテナー アプリでは、フレームワーク、それ以外の場合、フレームワークは、アプリ バンドルに含まれませんへの参照が必要もあります。

## <a name="the-mono-runtime"></a>Mono ランタイム

内部的に Xamarin.iOS は Mono ランタイムを各拡張機能と、アプリをコンテナーに静的にリンクの代わりに、フレームワークとして、Mono ランタイムとリンクするには、この機能はします。

これにより、コンテナー アプリが統合アプリ、拡張機能が含まれているし、対象の展開は iOS 8.0 以上である場合は自動的に行います。

拡張子のないアプリは引き続き Mono ランタイムとを静的にリンクを参照している 1 つだけのアプリがある場合は、フレームワークを使用して、サイズの低下があるためです。

この動作は、プロジェクトの iOS ビルド オプションでその他の mtouch 引数として、次を追加することで、アプリ開発者によってオーバーライドできます。

- `--mono:static`:Mono ランタイムと静的にリンクします。
- `--mono:framework`:Mono ランタイム、フレームワークとしてのリンクを示します。

拡張子のないアプリの場合でもフレームワークとして、Mono ランタイムとのリンクの 1 つのシナリオは、Apple が強制実行可能ファイルのサイズ制限を克服するために、実行可能ファイルのサイズを小さくです。 リファレンスについては、Mono ランタイムは、(Xamarin.iOS 8.12 のしかし彼は異なります、リリース間とアプリ間であっても)、アーキテクチャごとに約 1.7 MB を追加します。 Mono フレームワークは、約 2.3 MB アーキテクチャは、任意の拡張子のない単一アーキテクチャ アプリのことを意味、アプリのリンクを作成を追加します Mono ランタイムとフレームワークは ~1.7MB、で実行可能ファイルの圧縮が、~2.3MB フレームワークを追加、がその結果。で ~0.6MB 大きなアプリか。

