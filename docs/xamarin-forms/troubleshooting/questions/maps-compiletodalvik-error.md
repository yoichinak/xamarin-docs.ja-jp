---
title: COMPILETODALVIK Android プロジェクトが予期しないトップレベルエラーで失敗するのはなぜですか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C0251EB1-F509-47AD-98D6-846AF46425E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: efa21d3547167996e1a7dcc533caa00e0b1262e6
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69529033"
---
# <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik-unexpected-top-level-error"></a>COMPILETODALVIK Android プロジェクトが予期しないトップレベルエラーで失敗するのはなぜですか。

このエラーは、Visual Studio for Mac のエラーパッドまたは Visual Studio のビルド出力ウィンドウに表示されることがあります。Android プロジェクトでは、Xamarin を使用します。

これは、Xamarin Android プロジェクトの Java ヒープサイズを増やすことによって最も一般的に解決されます。 ヒープサイズを増やすには、次の手順に従います。

## <a name="visual-studio"></a>Visual Studio

1. Android プロジェクト & 右クリックして、[プロジェクトオプション] を開きます。
2. **Android オプションにアクセス-> 詳細設定**
3. [Java ヒープのサイズ] テキストボックスに「1G」と入力します。
4. プロジェクトをリビルドします。

![Visual Studio プロジェクトオプションのスクリーンショット](maps-compiletodalvik-error-images/vsjavaheap.png "Visual Studio の Android ビルドオプション")

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1. Android プロジェクト & 右クリックして、[プロジェクトオプション] を開きます。
2. **ビルド-> Android ビルド-> 詳細設定**にアクセス
3. [Java ヒープのサイズ] テキストボックスに「1G」と入力します。
4. プロジェクトをリビルドします。  

![Visual Studio for Mac プロジェクトオプションのスクリーンショット](maps-compiletodalvik-error-images/xsjavaheap.png "Visual Studio for Mac の Android ビルドオプション")

