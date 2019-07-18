---
title: Xamarin.Mac 統合アプリケーションを 64 ビットの更新
description: このガイドでは、64 ビットをターゲットに Xamarin.Mac アプリケーションを更新する方法について説明します。 この変更を行うときに発生する可能性があるエラーの種類の例も提供します。
ms.prod: xamarin
ms.assetid: C3810A74-539C-4FFB-B47F-68CA5F7BCDAD
author: conceptdev
ms.author: crdun
ms.date: 02/22/2018
ms.openlocfilehash: 9bd70fec5d6d3bbbc4855980e1542bd4e486acaa
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61266733"
---
# <a name="updating-xamarinmac-unified-applications-to-64-bit"></a>Xamarin.Mac 統合アプリケーションを 64 ビットの更新

2018 年 1 月の時点で Apple は、要求を新しい[Mac アプリ ストアへの送信対象の 64 ビット](https://developer.apple.com/news/?id=06282017a)します。 Mac App Store で既に利用可能なアプリは、2018 年 6 月によって 64 ビットのターゲットを更新する必要があります。

**ファイル** > **新規**最近作成されたすべてのアプリが 64 ビット互換であるし、変更する必要はありませんので、Xamarin.Mac プロジェクト テンプレートが既定では、64 ビット アプリケーションを作成します。

## <a name="targeting-64-bit"></a>64 ビットを対象とします。

1. 開く、**プロジェクト オプション**Xamarin.Mac アプリのウィンドウ。

   ![プロジェクトのコンテキスト メニュー](mac-64-bit-images/1-contextual_menu-vsmac.png "プロジェクトのコンテキスト メニュー")

2. 選択**Mac ビルド**設定と**サポートされているアーキテクチャ**に**x86\_64**:

   [![サポートされているアーキテクチャに x86_64 設定](mac-64-bit-images/2-project_options-vsmac.png "x86_64 にサポートされているアーキテクチャの設定")](mac-64-bit-images/2-project_options-vsmac-large.png#lightbox)

3. アプリにネイティブ参照またはバインド プロジェクトなどの外部依存関係がある場合は、64 ビットのターゲットを更新します。

### <a name="errors"></a>エラー

最初にビルドまたは 64 ビット サポートにより、アプリケーションを実行する clang やランタイムの問題からリンク エラーが発生する可能性があります。 サード パーティ製の場合、これらのエラーが発生する可能性が依存関係-ネイティブ参照など、Xamarin.Mac またはバインド プロジェクト、またはシステム全体のフレームワークを手動で読み込む-64 ビットに更新されていません。

> [!TIP]
> プロジェクトを 64 ビットに変換する大きな変更し、さまざまなプログラミング エラーが直接明らかです。 特にの p/invoke シグネチャと、プロジェクトにリンクされたネイティブ コードに影響するデータの構造体の配置とサイズを変更こと可能性があります。 すべてのビルド警告を確認してください、アプリケーションを徹底的にテストして潜在的な問題を検出するには、その後です。

#### <a name="example-error-resulting-from-a-dynamically-linked-third-party-dependency-that-does-not-target-64-bit"></a>64 ビットを使用しない動的リンクのサードパーティの依存関係の結果エラーの例:

```console
ld : warning : ignoring file PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary, 
file was built for i386 which is not the architecture being linked (x86_64): 
PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary 
```

実行時にこのエラーの後にでした`dlopen`返す`IntPtr.Zero`ハンドルが予想される代わりにします。

#### <a name="example-error-resulting-from-a-statically-linked-third-party-dependency-that-does-not-target-64-bit"></a>64 ビットを使用しない静的リンクのサードパーティの依存関係の結果エラーの例:

```console
Undefined symbols for architecture x86_64:
  "_LibraryFunction", referenced from:
     -u command line option
ld: symbol(s) not found for architecture x86_64 
```

ビルドが正常に実行し、するには、これらの依存関係を 64 ビットに更新し、アプリを再コンパイルします。

