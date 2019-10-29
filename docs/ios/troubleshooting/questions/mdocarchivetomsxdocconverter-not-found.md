---
title: MDocArchiveToMsxDocConverter.exe not found rver.BaseCommand.OnRequest (MDocArchiveToMsxDocConverter.exe が rver.BaseCommand.OnRequest に見つかりません)
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5AC6AC4-0E7C-4746-A7CF-872F0E75AFF4
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 3d21dfdbf6c9be00fe6851bb288268faccd74308
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030970"
---
# <a name="mdocarchivetomsxdocconverterexe-not-found-rverbasecommandonrequest"></a>MDocArchiveToMsxDocConverter.exe not found rver.BaseCommand.OnRequest (MDocArchiveToMsxDocConverter.exe が rver.BaseCommand.OnRequest に見つかりません)

> [!IMPORTANT]
> この問題は、最新バージョンの Xamarin で解決されました。 ただし、最新バージョンのソフトウェアで問題が発生した場合は、完全なバージョン管理情報と完全ビルドログ出力を使用して[新しいバグを作成](~/cross-platform/troubleshooting/questions/howto-file-bug.md)してください。

## <a name="error-message"></a>エラー メッセージ

このエラーは、Visual Studio の*Mac サーバーログ*に表示される場合があります。

```
Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found
 rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context, System.Object commandRequestState) [0x00000] in <filename unknown>:0
  at Mtb.Server.Listener.OnRequest (System.Object state) [0x00000] in <filename unknown>:0
```

このメッセージには、次の2つの問題があります。

1. `Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found`

    このエラーは害を与えませんが、誤解を招くこともあります。 今後のリリースでは[削除される予定](https://bugzilla.xamarin.com/show_bug.cgi?id=21667)です。

2. `rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context …`

    このエラーは実際の問題です。 残念ながら、この例外スタックトレースは、[制限](https://bugzilla.xamarin.com/show_bug.cgi?id=22080)があるため、*不完全*です。 このような不完全なスタックトレースが Mac サーバーログに記録されている場合は、Mac ビルドホストの `~/Library/Logs/Xamarin/MonoTouchVS/mtbserver.log` ファイルをチェックして、完全なスタックトレースを見つけることができます。
