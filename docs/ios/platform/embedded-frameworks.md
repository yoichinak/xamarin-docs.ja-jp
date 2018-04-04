---
title: 埋め込みのフレームワーク
description: このドキュメントでは、アプリケーション開発者がそれぞれのアプリでユーザーのフレームワークを埋め込むことができる方法について説明します。
ms.prod: xamarin
ms.assetid: F8C61020-4106-46F1-AECB-B56C909F42CB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: f223d8ef6e89cc44822b8a831dbba3cf71d727c9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="embedded-frameworks"></a>埋め込みのフレームワーク

_このドキュメントでは、アプリケーション開発者がそれぞれのアプリでユーザーのフレームワークを埋め込むことができる方法について説明します。_

8.0 Apple は iOS で行ったアプリ拡張機能と Xcode でメインのアプリ間でコードを共有するために埋め込みのフレームワークを作成することです。

Xamarin.iOS 9.0 では、Xamarin.iOS アプリでこれらの組み込みフレームワーク (Xcode で作成) を使用するためのサポートを追加します。 *これは**いない**Xamarin.iOS プロジェクトの任意の型から組み込みフレームワークを作成するには、既存のネイティブ (OBJECTIVE-C) フレームワークのみを使用できるようになります。*

Xamarin.iOS のフレームワークを使用する 2 つの方法があります。

- ツールに渡すために、フレームワーク、mtouch、プロジェクトの追加 mtouch 引数に、次を追加することによって**iOS ビルド**オプション。

  ```csharp
  --framework:/Path/To/My.Framework
  ```

  これは、各プロジェクト構成に設定します。

- コンテキスト メニューからのネイティブ参照を追加します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

ネイティブ参照を追加するには、プロジェクトと参照を右クリックします。

![](embedded-frameworks-images/xam-native-refs.png "Mac 用 Visual Studio でのネイティブ参照の追加を選択します。")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

ネイティブ参照を追加するには、プロジェクトと参照を右クリックします。

![](embedded-frameworks-images/vs-native-refs.png "Visual Studio でのネイティブ参照の追加を選択します。")

-----

  これは、すべての構成で機能します。

Mac 用の Visual Studio と Xamarin Tools for Visual Studio の今後のバージョンでは (プロジェクト ファイルを手動で編集するには) なし、IDE 内からのフレームワークを使用することができます。

いくつかのサンプル プロジェクトにある  [github](https://github.com/rolfbjarne/embedded-frameworks)

## <a name="limitations"></a>制限事項

- 埋め込みのフレームワークでのみサポート[統合](~/cross-platform/macios/unified/index.md)プロジェクト。
- 埋め込みのフレームワークでのみサポート プロジェクトの配置ターゲットを少なくとも iOS 8.0 です。
- 拡張機能は、埋め込みのフレームワークを必要とする場合、コンテナー アプリケーションも必要 framework への参照は、それ以外の場合、フレームワークは、アプリのバンドルに含まれません。

## <a name="the-mono-runtime"></a>モノのランタイム

内部的に Xamarin.iOS 活用モノのランタイムをそれぞれの拡張機能とコンテナーのアプリに静的にリンクの代わりに、フレームワークとしてモノのランタイムとリンクするには、この機能します。

これにより、コンテナー アプリは統合アプリ、拡張機能が含まれているし、対象の展開は iOS 8.0 以上である場合は自動的に行います。

拡張子のないアプリは引き続きモノのランタイムと静的にリンクがサイズ ペナルティを参照しているアプリの 1 つだけがある場合は、フレームワークを使用しているためです。

この動作は、プロジェクトの iOS のビルド オプションで追加 mtouch 引数として、次を追加することで、アプリ開発者によってオーバーライドできます。

- `--mono:static`: を使用してリンク モノラル ランタイム静的にします。
- `--mono:framework`: Mono フレームワークとしてランタイムにリンクしています。

拡張子のないアプリであってもフレームワークとしてモノのランタイムとのリンクを 1 つのシナリオは、Apple の強制実行可能ファイルのサイズ制限を克服する、実行可能ファイルのサイズを小さくです。 リファレンスについては、Mono ランタイムは、(に応じて Xamarin.iOS 8.12 のただし、彼のリリースの間とアプリ間であっても) アーキテクチャごとに約 1.7 MB を追加します。 Mono フレームワーク 2.3 つにつき約 MB アーキテクチャは、アプリについては、単一アーキテクチャの拡張機能がないことを意味、アプリのリンクを追加するモノのランタイムと、フレームワークは ~1.7MB で実行可能ファイルの圧縮が、~2.3MB フレームワークを追加、結果として得られるで ~0.6MB より大きなアプリ alltogether です。

