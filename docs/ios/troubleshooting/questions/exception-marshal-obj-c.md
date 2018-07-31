---
title: 'IOS 9 アプリが失敗する理由と: System.Exception: OBJECTIVE-C オブジェクトをマーシャ リングできませんでしたか?'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8805ABEC-48D4-4CCB-A226-3A5B2ECE4BF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 93666fcb39f0cd717c14eb07e6407801e9f0642e
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350600"
---
# <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-object"></a>IOS 9 アプリが失敗する理由と: System.Exception: OBJECTIVE-C オブジェクトをマーシャ リングできませんでしたか?

このフォームのエラーが表示することがあります。

> System.Exception: OBJECTIVE-C オブジェクトをマーシャ リングできませんでした.このオブジェクトの既存のマネージ インスタンスが見つかりませんでした.

IOS 9 の API の変更は、基になる API を今すぐとしてアンマネージ コードを呼び出すことが期待したときにコールバック コンス トラクターを使用することが必要です。 クラスにコールバック コンス トラクターを追加するのにには、次の行を使用します。 

`public foo (IntPtr handle) : base (handle) ` 

### <a name="next-steps"></a>次の手順

問い合わせ、または上記の情報を使用した後でもこの問題が残っている場合を参照してください、詳細については[Xamarin のどのようなサポート オプションを使用しますか?](~/cross-platform/troubleshooting/support-options.md)連絡先オプション、推奨事項は、についてする方法についても必要な場合は、新しいバグをファイルします。 
