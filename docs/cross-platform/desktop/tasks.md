---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: 一般的なタスクの比較
description: このドキュメントでは、WPF と Xamarin.Forms のさまざまな一般的なタスクを実行する方法を比較します。 ボタン、タイマー、フォント サイズを URI を開くと、アクション シートを表示するのには次になります。
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 2991e74ec59e3c43c665941706c2d9ac94bfb9ad
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61269596"
---
# <a name="common-tasks-comparison"></a>一般的なタスクの比較

| タスク | WPF | Xamarin.Forms |
|--- |--- |--- |
|ボタンが画面にメッセージを表示します。|`MessageBox`|`Page.DisplayAlert`|
|タイマーを作成します。|`DispatcherTimer` クラス|`Device.StartTimer` 静的メソッド|
|既定のフォント サイズを取得します。|`SystemFonts` 静的クラス|`Device.GetNamedSize` 静的メソッド|
|URI または URL を開く|`Process.Start`|`Device.OpenUri`|
|アクション シート (ボタンの一覧) を表示します。|適用なし|`Page.DisplayActionSheet`|
