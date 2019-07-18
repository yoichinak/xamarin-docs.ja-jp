---
title: MDocArchiveToMsxDocConverter.exe not found rver.BaseCommand.OnRequest (MDocArchiveToMsxDocConverter.exe が rver.BaseCommand.OnRequest に見つかりません)
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5AC6AC4-0E7C-4746-A7CF-872F0E75AFF4
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 0746174857f66843ef9a09429b6286f2efca90d6
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61420538"
---
# <a name="mdocarchivetomsxdocconverterexe-not-found-rverbasecommandonrequest"></a>MDocArchiveToMsxDocConverter.exe not found rver.BaseCommand.OnRequest (MDocArchiveToMsxDocConverter.exe が rver.BaseCommand.OnRequest に見つかりません)

> [!IMPORTANT]
> Xamarin の最近のバージョンでは、この問題を解決されています。 ただし、ソフトウェアの最新バージョンで問題が発生した場合を提出してください、[新しいバグ](~/cross-platform/troubleshooting/questions/howto-file-bug.md)完全なバージョン管理情報と完全のビルド ログ出力します。


## <a name="error-message"></a>エラー メッセージ

このエラーが含まれる、 *Mac サーバー ログ*Visual Studio で。

```
Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found
 rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context, System.Object commandRequestState) [0x00000] in <filename unknown>:0
  at Mtb.Server.Listener.OnRequest (System.Object state) [0x00000] in <filename unknown>:0
```

このメッセージには、2 つの個別の問題があります。

1.  `Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found`

    このエラーは問題がも誤解を招きます。 これは、[削除予定](https://bugzilla.xamarin.com/show_bug.cgi?id=21667)将来のリリースでします。

2.  `rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context …`

    このエラーは、実際の問題です。 残念ながら、ために、[制限](https://bugzilla.xamarin.com/show_bug.cgi?id=22080)この例外のスタック トレースは*不完全な*します。 Mac サーバー ログに次のように、不完全なスタック トレースを確認する場合は、確認、`~/Library/Logs/Xamarin/MonoTouchVS/mtbserver.log`完全なスタック トレースを検索する Mac ビルド ホスト上のファイル。
