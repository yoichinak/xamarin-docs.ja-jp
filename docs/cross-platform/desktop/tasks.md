---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: 一般的なタスクの比較
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: b4d45ae61c5d7a7093d51c4440d11dd883c157a4
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/10/2018
---
# <a name="common-tasks-comparison"></a>一般的なタスクの比較

| タスク | WPF | Xamarin.Forms |
|--- |--- |--- |
|スクリーン ボタンにメッセージを表示します。|`MessageBox`|`Page.DisplayAlert`|
|タイマーを作成します。|`DispatcherTimer` クラス|`Device.StartTimer` 静的メソッド|
|既定のフォント サイズを取得します。|`SystemFonts` 静的クラス|`Device.GetNamedSize` 静的メソッド|
|URI の URL/を開く|`Process.Start`|`Device.OpenUri`|
|アクションを表示します。 (ボタンの一覧)|N/A|`Page.DisplayActionSheet`|
