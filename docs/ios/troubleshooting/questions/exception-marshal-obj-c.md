---
title: 'IOS 9 アプリが次で失敗するのはなぜですか: System.exception: 目的の C オブジェクトをマーシャリングできませんでしたか?'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8805ABEC-48D4-4CCB-A226-3A5B2ECE4BF0
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: 1bb67eaa884e523e96ef1015daaa6b959ea1512d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031104"
---
# <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-object"></a>IOS 9 アプリが次で失敗するのはなぜですか: System.exception: 目的の C オブジェクトをマーシャリングできませんでしたか?

次のような形式のエラーが表示されることがあります。

> System.exception: 目的の C オブジェクトをマーシャリングできませんでした...このオブジェクトの既存のマネージドインスタンスが見つかりませんでした...

IOS 9 での API の変更では、基になる API が想定しているように、アンマネージコードを呼び出すときにコールバックコンストラクターを使用する必要があります。 次の行を使用して、コールバックコンストラクターをクラスに追加します。 

`public foo (IntPtr handle) : base (handle)` 

### <a name="next-steps"></a>次のステップ

詳細については、お問い合わせください。または、上記の情報を利用した後もこの問題が発生する場合は、「 [Xamarin で使用できるサポートオプション](~/cross-platform/troubleshooting/support-options.md)」を参照してください。連絡先オプション、提案、および必要に応じて新しいバグをファイルに登録する方法については、こちらを参照してください. 
