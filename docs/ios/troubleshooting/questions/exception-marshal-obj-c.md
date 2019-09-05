---
title: 'iOS 9 アプリが System.Exception: Failed to marshal the Objective-C object(System.Exception: Objective C オブジェクトのマーシャリングが失敗しました) で失敗するのはなぜですか。'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8805ABEC-48D4-4CCB-A226-3A5B2ECE4BF0
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 04/03/2018
ms.openlocfilehash: 62a63dc5156d1acf9ad6ca15029978131c151726
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290468"
---
# <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-object"></a>iOS 9 アプリが System.Exception: Failed to marshal the Objective-C object(System.Exception: Objective C オブジェクトのマーシャリングが失敗しました) で失敗するのはなぜですか。

次のような形式のエラーが表示されることがあります。

> System.Exception: 目的の C オブジェクトをマーシャリングできませんでした...このオブジェクトの既存のマネージドインスタンスが見つかりませんでした...

IOS 9 での API の変更では、基になる API が想定しているように、アンマネージコードを呼び出すときにコールバックコンストラクターを使用する必要があります。 次の行を使用して、コールバックコンストラクターをクラスに追加します。 

`public foo (IntPtr handle) : base (handle)` 

### <a name="next-steps"></a>次の手順

詳細については、お問い合わせください。または、上記の情報を利用した後もこの問題が発生する場合は、「 [Xamarin で使用できるサポートオプション](~/cross-platform/troubleshooting/support-options.md)」を参照してください。連絡先オプション、提案、および必要に応じて新しいバグをファイルに登録する方法については、こちらを参照してください. 
