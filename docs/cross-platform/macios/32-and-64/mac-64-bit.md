---
title: Xamarin. Mac 統合アプリケーションを64ビットに更新する
description: このガイドでは、64ビットをターゲットとするように Xamarin. Mac アプリケーションを更新する方法について説明します。 また、この変更を行うときに発生する可能性があるエラーの種類の例も示します。
ms.prod: xamarin
ms.assetid: C3810A74-539C-4FFB-B47F-68CA5F7BCDAD
author: conceptdev
ms.author: crdun
ms.date: 02/22/2018
ms.openlocfilehash: 5539bab417c5efc0064cd1753cb74c7524463ee5
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "70765922"
---
# <a name="updating-xamarinmac-unified-applications-to-64-bit"></a>Xamarin. Mac 統合アプリケーションを64ビットに更新する

2018年1月の時点で、Apple は新しい[Mac App Store の送信ターゲットを64ビット](https://developer.apple.com/news/?id=06282017a)にする必要があります。 Mac App Store で既に提供されているアプリは、64 2018 年6月のビット版に更新する必要があります。

**新しい**Xamarin. Mac プロジェクトテンプレート  > **ファイル**は既定で64ビットアプリケーションを作成するため、最近作成されたアプリは既に64ビット互換性があるため、変更は必要ありません。

## <a name="targeting-64-bit"></a>64ビットをターゲットにする

1. Xamarin. Mac アプリの **[プロジェクトオプション]** ウィンドウを開きます。

   ![プロジェクトのコンテキストメニュー](mac-64-bit-images/1-contextual_menu-vsmac.png "プロジェクトのコンテキストメニュー")

2. **Mac ビルド**を選択し、**サポートされているアーキテクチャ**を**x86 \_64**に設定します。

   [![サポートされているアーキテクチャを x86_64 に設定する](mac-64-bit-images/2-project_options-vsmac.png "サポートされているアーキテクチャを x86_64 に設定する")](mac-64-bit-images/2-project_options-vsmac-large.png#lightbox)

3. アプリにネイティブ参照やバインドプロジェクトなどの外部依存関係がある場合は、それをターゲット64ビットに更新します。

### <a name="errors"></a>エラー

64ビットのサポートで初めてアプリケーションをビルドまたは実行すると、clang またはランタイムの問題からのリンクエラーが発生することがあります。 これらのエラーは、サードパーティの依存関係 (たとえば、Xamarin. Mac またはバインドプロジェクト内のネイティブ参照、システム全体のフレームワークの手動読み込みなど) が64ビットに更新されていない場合に発生する可能性があります。

> [!TIP]
> プロジェクトを64ビットに変換することは大きな変更であり、さまざまなプログラミングエラーを間接的に発見することがあります。 特に、データ構造のサイズとアラインメントを変更することがあります。これは、プロジェクトにリンクされた p/invoke シグネチャとネイティブコードに影響します。 発生する可能性のある問題を検出するには、指定されたビルドの警告を確認し、その後でアプリケーションを十分にテストしてください。

#### <a name="example-error-resulting-from-a-dynamically-linked-third-party-dependency-that-does-not-target-64-bit"></a>64ビットを対象としない動的にリンクされたサードパーティの依存関係によって発生するエラーの例を次に示します。

```console
ld : warning : ignoring file PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary, 
file was built for i386 which is not the architecture being linked (x86_64): 
PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary 
```

このエラーは、予期されるハンドルではなく `IntPtr.Zero` を返す `dlopen` ことによって実行時に発生する可能性があります。

#### <a name="example-error-resulting-from-a-statically-linked-third-party-dependency-that-does-not-target-64-bit"></a>64ビットを対象としない、静的にリンクされたサードパーティの依存関係が発生した場合のエラーの例を次に示します。

```console
Undefined symbols for architecture x86_64:
  "_LibraryFunction", referenced from:
     -u command line option
ld: symbol(s) not found for architecture x86_64 
```

ビルドして正常に実行するには、これらの依存関係を64ビットに更新し、アプリを再コンパイルします。
