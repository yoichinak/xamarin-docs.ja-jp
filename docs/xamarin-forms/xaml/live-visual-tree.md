---
title: Xamarin.Forms ライブビジュアルツリー
description: デバッグ中にアプリのランタイム UI 階層を表示します。
ms.prod: xamarin
ms.assetid: 29c45b85-21b1-40ab-941f-4d9e57770c46
ms.technology: xamarin-forms
author: BretJohnson
ms.author: bretjohn
ms.date: 03/25/2021
no-loc:
- Xamarin.Forms
ms.openlocfilehash: 66ee5c28d3c6f35bec8e30e409a38430d41d8c68
ms.sourcegitcommit: 3aa9bdcaaedca74ab5175cb2338a1df122300243
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/03/2021
ms.locfileid: "101751625"
---
# <a name="xamarinforms-live-visual-tree"></a>Xamarin.Forms ライブビジュアルツリー

**ライブビジュアルツリー** を使用して、実行中の XAML コードのリアルタイムビューを受け取ることができます。 実行中のアプリケーションの UI 要素のツリービューが表示さ Xamarin.Forms れます。

## <a name="requirements"></a>要件

* Xamarin.Forms5.0 以降を使用します。
* 変更されるのは、ホットリロードが有効になっている (既定で有効になっている) *だけ* です。

## <a name="usage"></a>使用法

要件を満たしていれば、[アプリのデバッグ] ウィンドウと [ライブビジュアルツリー] ウィンドウにアプリのランタイム UI 階層が表示されます。

  * **Windows**: 既定では、IDE の左側に表示されます。 表示されない場合は、 **デバッグ > Windows > ライブビジュアルツリー** を使用して表示します。
  * **Mac**: 既定では、IDE の右側に表示されます。 表示されない場合は、 **Windows > ライブビジュアルツリーの表示 > デバッグ** ] を使用して表示します。

ツリービューを使用すると、アプリのランタイム UI 階層を調べることができます。ノードを展開したり折りたたんだりすることで、UI の特定の部分に焦点を当てることができます。

## <a name="live-visual-tree-toolbar"></a>ライブビジュアルツリーツールバー 

XAML 要素のビューは、既定では **[マイ xaml** ] 機能のみを使用して簡略化されます。 [ライブビジュアルツリー] ツールバーの右端にある **[マイ XAML のみ表示** ] ボタンを切り替えると、すべての UI 要素が表示されます。 必要に応じて、[常にすべての XAML 要素を表示する] オプションで [この設定を無効](/visualstudio/debugger/general-debugging-options-dialog-box.md) にすることができます。

> [!NOTE]
> Visual Studio for Mac は、現在、" **マイ XAML** " 機能のみをサポートしていません。

XAML の構造には多くの要素が含まれていますが、これにはあまり関心がない可能性があります。コードウェルがわからない場合は、ツリー内を移動して探しているものを見つけるのに時間がかかることがあります。 したがって、 **ライブビジュアルツリー** には複数の方法があり、アプリケーションの UI を使用して、確認する要素を見つけるのに役立ちます。

**実行中のアプリケーションの要素を選択** します (現在、UWP アプリでのみサポートされています)。 **[Live Visual Tree]** ツール バーの左端のボタンを選択すると、このモードを有効にすることができます。 このモードをオンにすると、アプリケーションで UI 要素を選択できます。また、 **ライブビジュアルツリー** が自動的に更新され、その要素に対応するツリー内のノードとそのプロパティが表示されます。 

**実行中のアプリケーションでレイアウトガイドを表示** します (現在、UWP アプリでのみサポートされています)。 選択を有効にするためのボタンのすぐ右にあるボタンを選択すると、このモードを有効にすることができます。 **レイアウトの装飾の表示** がオンのときは、アプリケーション ウィンドウには選択されたオブジェクトの境界に沿って水平と垂直の線が表示され、何に揃えて配置されているかが確認できます。さらに、余白を示すための四角形も表示されます。

**選択のプレビュー**。 このモードを有効にするには、Visual Tree ツールバーで左端から 3 番目のボタンを選択します。 このモードは、アプリケーションのソース コードにアクセスできる場合に、要素が宣言されている XAML を示します。

## <a name="related-links"></a>関連リンク

- [XAML ホットリロードを使用して実行中の XAML コードを記述およびデバッグする](hot-reload.md)
