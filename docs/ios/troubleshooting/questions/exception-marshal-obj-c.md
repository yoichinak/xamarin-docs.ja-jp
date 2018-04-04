---
title: '理由が iOS 9 を探すアプリで異常終了: System.Exception: OBJECTIVE-C オブジェクトをマーシャ リングできませんでした。'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8805ABEC-48D4-4CCB-A226-3A5B2ECE4BF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: f7382ac963249a3f3646a917d8700e3a12873ec9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-object"></a>理由が iOS 9 を探すアプリで異常終了: System.Exception: OBJECTIVE-C オブジェクトをマーシャ リングできませんでした。

このフォームのエラーを確認できます。

> Objective C オブジェクトをマーシャ リング System.Exception: に失敗しました.このオブジェクトの既存のマネージ インスタンスを見つけることができませんでした.

IOS 9 の API の変更は、基になる API を今すぐとして、アンマネージ コードを呼び出すことが必要とするときにコールバック コンス トラクターを使用することが必要です。 クラスにコールバック コンス トラクターを追加するのにには、次の行を使用します。 

`public foo (IntPtr handle) : base (handle) ` 

### <a name="next-steps"></a>次の手順

詳細については、問い合わせ先、または、上記の情報を使用した後もこの問題が残っている場合を参照してください[どのようなサポート オプションは、Xamarin を使用しますか?](~/cross-platform/troubleshooting/support-options.md)推奨事項は、の連絡先のオプションについて方法だけでなく必要な場合は、新しいバグをファイルです。 
