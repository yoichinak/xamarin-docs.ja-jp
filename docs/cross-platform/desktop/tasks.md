---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: 一般的なタスクの比較
description: このドキュメントでは、WPF と Xamarin. Forms で各種の一般的なタスクを実行する方法を比較します。 ボタン、タイマー、フォントサイズ、URI を開き、操作シートを表示します。
author: davidortinau
ms.author: daortin
ms.date: 04/26/2017
ms.openlocfilehash: d1f1430d8044f94c17a19d747334b5ed8a1441cf
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016415"
---
# <a name="common-tasks-comparison"></a>一般的なタスクの比較

| タスク | WPF | Xamarin.Forms |
|--- |--- |--- |
|ボタンが表示された画面にメッセージを表示する|`MessageBox`|`Page.DisplayAlert`|
|タイマーを作成する|`DispatcherTimer` クラス|`Device.StartTimer` の静的メソッド|
|既定のフォントサイズを取得する|`SystemFonts` 静的クラス|`Device.GetNamedSize` の静的メソッド|
|URI/URL を開く|`Process.Start`|`Device.OpenUri`|
|操作シートの表示 (ボタンの一覧)|N/A|`Page.DisplayActionSheet`|
