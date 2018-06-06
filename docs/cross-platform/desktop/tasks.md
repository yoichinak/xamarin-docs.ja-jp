---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: 一般的なタスクの比較
description: このドキュメントでは、WPF と Xamarin.Forms のさまざまな一般的なタスクを実行する方法を比較します。 ボタン、タイマー、フォント サイズを URI を開き、アクション シートを表示するようになります。
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 2991e74ec59e3c43c665941706c2d9ac94bfb9ad
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780477"
---
# <a name="common-tasks-comparison"></a>一般的なタスクの比較

| タスク | WPF | Xamarin.Forms |
|--- |--- |--- |
|スクリーン ボタンにメッセージを表示します。|`MessageBox`|`Page.DisplayAlert`|
|タイマーを作成します。|`DispatcherTimer` クラス|`Device.StartTimer` 静的メソッド|
|既定のフォント サイズを取得します。|`SystemFonts` 静的クラス|`Device.GetNamedSize` 静的メソッド|
|URI の URL/を開く|`Process.Start`|`Device.OpenUri`|
|アクションを表示します。 (ボタンの一覧)|N/A|`Page.DisplayActionSheet`|
