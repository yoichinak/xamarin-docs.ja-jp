---
title: Xamarin.Forms.Maps Android プロジェクトが COMPILETODALVIK UNEXPECTED TOP-LEVEL ERROR で失敗は理由ですか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C0251EB1-F509-47AD-98D6-846AF46425E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 9df9e348440b9dd4b18b3859d64cbe47bd05b24c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61250492"
---
# <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik-unexpected-top-level-error"></a>Xamarin.Forms.Maps Android プロジェクトが COMPILETODALVIK UNEXPECTED TOP-LEVEL ERROR で失敗は理由ですか。

このエラーは for Mac または Visual Studio のビルド出力 ウィンドウで、Visual Studio のエラー パッドで発生する可能性があります。Xamarin.Forms.Maps を使用して Android のプロジェクト。

これは、最もよく、Xamarin.Android プロジェクト用の Java ヒープ サイズを増やすことで解決します。 ヒープ サイズを大きくこれらの手順に従います。

## <a name="visual-studio"></a>Visual Studio

1. Android プロジェクトを右クリックして & プロジェクト オプション を開きます。
2. 移動して**Android オプション]、[詳細設定**
3. Java ヒープ サイズ ボックスには、1 G を入力します。
4. プロジェクトをリビルドします。

![Visual Studio プロジェクトのオプションのスクリーン ショット](maps-compiletodalvik-error-images/vsjavaheap.png "Android ビルド Visual Studio でのオプション")

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1.  Android プロジェクトを右クリックして & プロジェクト オプション を開きます。
2.  移動して**ビルド Android のビルド]->-> [詳細設定**
3.  Java ヒープ サイズ ボックスには、1 G を入力します。
4.  プロジェクトをリビルドします。  

![Visual Studio for Mac プロジェクト オプションのスクリーン ショット](maps-compiletodalvik-error-images/xsjavaheap.png "Android のビルドでは、Visual Studio for Mac のオプション")

