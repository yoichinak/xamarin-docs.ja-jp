---
title: 'iOS 9 アプリが System.Exception: Failed to marshal the Objective-C object(System.Exception: Objective C オブジェクトのマーシャリングが失敗しました) で失敗するのはなぜですか。'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8805ABEC-48D4-4CCB-A226-3A5B2ECE4BF0
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/03/2018
ms.openlocfilehash: 3dbb4d9132d5d94e4533704730e95002b5aec0be
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832268"
---
# <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-object"></a>iOS 9 アプリが System.Exception: Failed to marshal the Objective-C object(System.Exception: Objective C オブジェクトのマーシャリングが失敗しました) で失敗するのはなぜですか。

このフォームのエラーが表示することがあります。

> System.Exception: Objective C のオブジェクトをマーシャ リングできませんでした.このオブジェクトの既存のマネージ インスタンスが見つかりませんでした.

IOS 9 の API の変更は、基になる API を今すぐとしてアンマネージ コードを呼び出すことが期待したときにコールバック コンス トラクターを使用することが必要です。 クラスにコールバック コンス トラクターを追加するのにには、次の行を使用します。 

`public foo (IntPtr handle) : base (handle)` 

### <a name="next-steps"></a>次の手順

問い合わせ、または上記の情報を使用した後でもこの問題が残っている場合を参照してください、詳細については[Xamarin のどのようなサポート オプションを使用しますか?](~/cross-platform/troubleshooting/support-options.md)連絡先オプション、推奨事項は、についてする方法についても必要な場合は、新しいバグをファイルします。 
