---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: 一般的なタスクの比較
description: このドキュメントでは、WPF と Xamarin. Forms で各種の一般的なタスクを実行する方法を比較します。 ボタン、タイマー、フォントサイズ、URI を開き、操作シートを表示します。
author: conceptdev
ms.author: crdun
ms.date: 04/26/2017
ms.openlocfilehash: fecd8ed774adbacf69e3b2db514a4698e71711d8
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290330"
---
# <a name="common-tasks-comparison"></a>一般的なタスクの比較

| タスク | WPF | Xamarin.Forms |
|--- |--- |--- |
|ボタンが表示された画面にメッセージを表示する|`MessageBox`|`Page.DisplayAlert`|
|タイマーを作成する|`DispatcherTimer` クラス|`Device.StartTimer`静的メソッド|
|既定のフォントサイズを取得する|`SystemFonts`静的クラス|`Device.GetNamedSize`静的メソッド|
|URI/URL を開く|`Process.Start`|`Device.OpenUri`|
|操作シートの表示 (ボタンの一覧)|N/A|`Page.DisplayActionSheet`|
