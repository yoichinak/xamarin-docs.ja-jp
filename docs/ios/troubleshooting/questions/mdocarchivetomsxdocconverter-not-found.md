---
title: MDocArchiveToMsxDocConverter.exe した rver が見つかりません。BaseCommand.OnRequest
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5AC6AC4-0E7C-4746-A7CF-872F0E75AFF4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 6d7fe8f48d22c6b5d445fa8356e52f924549f6e0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30775677"
---
# <a name="mdocarchivetomsxdocconverterexe-not-found-rverbasecommandonrequest"></a>MDocArchiveToMsxDocConverter.exe した rver が見つかりません。BaseCommand.OnRequest

> [!IMPORTANT]
> Xamarin の最近のバージョンでは、この問題を解決されています。 ただし、ソフトウェアの最新のバージョンで問題が発生した場合を送信してください、[新しいバグ](~/cross-platform/troubleshooting/questions/howto-file-bug.md)すべてのバージョン管理情報と完全のビルド ログ出力します。


## <a name="error-message"></a>エラー メッセージ

このエラーに表示される可能性があります、 *Mac サーバー ログ*Visual Studio で。

```
Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found
 rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context, System.Object commandRequestState) [0x00000] in <filename unknown>:0
  at Mtb.Server.Listener.OnRequest (System.Object state) [0x00000] in <filename unknown>:0
```

このメッセージには、2 つの個別の問題があります。

1.  `Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found`

    このエラーは、問題ありませんも誤解を招きます。 これは、[削除予定](https://bugzilla.xamarin.com/show_bug.cgi?id=21667)将来のリリースでします。

2.  `rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context …`

    このエラーは、実際の問題です。 残念ながら、ために、[制限](https://bugzilla.xamarin.com/show_bug.cgi?id=22080)この例外のスタック トレースは*不完全な*します。 Mac サーバー ログに次のように、不完全なスタック トレースを確認する場合は、確認、 `~/Library/Logs/Xamarin/MonoTouchVS/mtbserver.log` Mac ビルド ホスト ファイルの完全なスタック トレースが見つかります。
