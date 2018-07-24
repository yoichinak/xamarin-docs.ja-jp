---
title: 理由は Xamarin.Forms.Maps Android プロジェクト COMPILETODALVIK 予期しない最上位のエラーで失敗しますか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C0251EB1-F509-47AD-98D6-846AF46425E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 9df9e348440b9dd4b18b3859d64cbe47bd05b24c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30790666"
---
# <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik-unexpected-top-level-error"></a>理由は Xamarin.Forms.Maps Android プロジェクト COMPILETODALVIK 予期しない最上位のエラーで失敗しますか。

Mac 用または Visual Studio 以外のビルド出力 ウィンドウで Visual Studio のエラー パッドでこのエラーが発生する可能性があります。で Android プロジェクト Xamarin.Forms.Maps を使用します。

これが最もよく Xamarin.Android プロジェクト用 Java のヒープ サイズを増やすことで解決します。 ヒープ サイズを大きくこれらの手順に従います。

## <a name="visual-studio"></a>Visual Studio

1. Android プロジェクトを右クリックし、プロジェクトのオプションを開きます。
2. 移動して**Android オプション]-> [詳細設定**
3. Java では、ヒープ サイズ テキスト ボックスは、1 G を入力します。
4. プロジェクトをリビルドします。

![Visual Studio プロジェクトのオプションのスクリーン ショット](maps-compiletodalvik-error-images/vsjavaheap.png "Android ビルド Visual Studio でのオプション")

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1.  Android プロジェクトを右クリックし、プロジェクトのオプションを開きます。
2.  移動して **-> Android ビルドのビルド]-> [詳細設定**
3.  Java では、ヒープ サイズ テキスト ボックスは、1 G を入力します。
4.  プロジェクトをリビルドします。  

![Mac プロジェクト オプションの Visual Studio のスクリーン ショット](maps-compiletodalvik-error-images/xsjavaheap.png "Android ビルド Mac を Visual Studio でのオプション")

