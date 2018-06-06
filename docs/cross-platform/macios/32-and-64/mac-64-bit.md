---
title: 64 ビットに Xamarin.Mac 統合アプリケーションの更新
description: このガイドでは、64 ビットのターゲットに Xamarin.Mac アプリケーションを更新する方法について説明します。 この変更を行うときに検出される可能性があるエラーの種類の例も提供します。
ms.prod: xamarin
ms.assetid: C3810A74-539C-4FFB-B47F-68CA5F7BCDAD
author: bradumbaugh
ms.author: brumbaug
ms.date: 02/22/2018
ms.openlocfilehash: aa97f9a68ea4acc4234233a22d10c99cde3e6d6c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780699"
---
# <a name="updating-xamarinmac-unified-applications-to-64-bit"></a>64 ビットに Xamarin.Mac 統合アプリケーションの更新

年 2018年 1 月時点で Apple が必要です新しい[Mac アプリ ストアへの送信が 64 ビットを対象](https://developer.apple.com/news/?id=06282017a)です。 Mac App Store で既に使用可能なアプリは、年 2018年 6 月によって 64 ビットのターゲットを更新する必要があります。

**ファイル** > **新規**のための最後に作成されたすべてのアプリが 64 ビット互換では既にされ、変更する必要はありません、Xamarin.Mac プロジェクト テンプレートが既定では、64 ビット アプリケーションを作成します。

## <a name="targeting-64-bit"></a>64 ビットを対象とします。

1. 開く、**プロジェクト オプション**ウィンドウいる Xamarin.Mac アプリ。

   ![プロジェクトのコンテキスト メニュー](mac-64-bit-images/1-contextual_menu-vsmac.png "プロジェクトのコンテキスト メニュー")

2. 選択**Mac をビルド**設定と**サポートされているアーキテクチャ**に**x86\_64**:

   [![サポートされているアーキテクチャに x86_64 設定](mac-64-bit-images/2-project_options-vsmac.png "x86_64 にサポートされているアーキテクチャの設定")](mac-64-bit-images/2-project_options-vsmac-large.png#lightbox)

3. アプリにネイティブ参照、またはバインド プロジェクトなど、外部の依存関係がある場合は、64 ビットのターゲットを更新します。

### <a name="errors"></a>エラー

初めてビルドまたは 64 ビット サポートで、アプリケーションを実行する clang または実行時に問題からのリンク エラーが発生する可能性があります。 サード パーティ製の場合、これらのエラーが発生する可能性が依存関係: たとえば、ネイティブ参照、Xamarin.Mac バインド プロジェクト、またはシステム全体のフレームワークを手動で読み込む — 64 ビットに更新されていません。

> [!TIP]
> プロジェクトを 64 ビットに変換する大きな変更は、さまざまなプログラミング エラーが直接明らかです。 具体的には、サイズとデータ構造体に影響を与える p/呼び出す署名と、プロジェクトのリンクされたネイティブ コードの配置には変更できます。 指定されたすべてのビルド警告を確認することを検討してください、アプリケーションとテストかもしれない後で潜在的な問題を検出します。

#### <a name="example-error-resulting-from-a-dynamically-linked-third-party-dependency-that-does-not-target-64-bit"></a>動的にリンクされているサードパーティの依存関係が 64 ビットを対象としないによるエラーの例:

```console
ld : warning : ignoring file PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary, 
file was built for i386 which is not the architecture being linked (x86_64): 
PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary 
```

によって実行時にこのエラーを続けてでした`dlopen`を返す`IntPtr.Zero`予想されるハンドルの代わりにします。

#### <a name="example-error-resulting-from-a-statically-linked-third-party-dependency-that-does-not-target-64-bit"></a>静的にリンクされているサードパーティの依存関係が 64 ビットを対象としないによるエラーの例:

```console
Undefined symbols for architecture x86_64:
  "_LibraryFunction", referenced from:
     -u command line option
ld: symbol(s) not found for architecture x86_64 
```

ビルドが正常に実行し、するには、64 ビットにこれらの依存関係を更新し、アプリを再コンパイルします。

