---
title: System.Exception AMDeviceNotificationSubscribe returned ... (System.Exception AMDeviceNotificationSubscribe が ... を返しました)
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7E4ACC7E-F4FB-46C1-8837-C7FBAAFB2DC7
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 4fb0712366422e8810a2db60d40c3b85d9f4cd82
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61421945"
---
# <a name="systemexception-amdevicenotificationsubscribe-returned-"></a>System.Exception AMDeviceNotificationSubscribe returned ... (System.Exception AMDeviceNotificationSubscribe が ... を返しました)

> [!IMPORTANT]
> Xamarin の最近のバージョンでは、この問題を解決されています。 ただし、ソフトウェアの最新バージョンで問題が発生した場合を提出してください、[新しいバグ](~/cross-platform/troubleshooting/questions/howto-file-bug.md)完全なバージョン管理情報と完全のビルド ログ出力します。


## <a name="fix"></a>修正

1.  強制終了、`usbmuxd`処理システムが再起動されます。

    ```csharp
    sudo killall -QUIT usbmuxd
    ```

2.  問題が解決しない、mac を再起動します。

## <a name="error-message"></a>エラー メッセージ

```csharp
Exception: Exception type: System.Exception
AMDeviceNotificationSubscribe returned: 3892314211
  at Xamarin.MacDev.IPhoneDeviceManager.SubscribeToNotifications () [0x00000] in <filename unknown="">:0
  at Xamarin.MacDev.IPhoneDeviceManager.StartWatching (Boolean useOwnRunloop) [0x00000] in <filename unknown="">:0
  at Mtb.Server.DeviceListener.Start () [0x00000] in <filename unknown="">:0
  at Mtb.Application.MainClass.RunDeviceListener () [0x00000] in <filename unknown="">:0
  at Mtb.Application.MainClass.Main (System.String[] args) [0x00000] in <filename unknown="">:0
```

For Mac、または初めて Visual Studio を起動すると、このメッセージがエラー ダイアログに表示されることができます、 `mtbserver.log` Xamarin.iOS Build Host アプリ内のファイル (**Xamarin.iOS Build Host > ビルド ホスト ログの表示**)。

これは、一般的でない問題であることに注意してください。 表示される可能性が高いその他のエラーがある場合、Visual Studio には、Mac ビルド ホストへの接続に問題がある、`mtbserver.log`ファイル。

### <a name="errors-in-systemlog"></a>System.log 内のエラー

いくつか次の 2 つの場合のエラー メッセージも表示されますで繰り返し`/var/log/system.log`:

```csharp
17:17:11.369 usbmuxd[55040]: dnssd_clientstub ConnectToServer: socket failed 24 Too many open files
17:17:11.369 com.apple.usbmuxd[55040]: _BrowseReplyReceivedCallback Error doing DNSServiceResolve(): -65539
```

## <a name="additional-information"></a>追加情報

エラーの根本原因の 1 つの推定値は、OS X がまれなケースでシステム サービスを iOS デバイスとシミュレーターの情報を報告することができますが、予期しない状態を入力します。 Xamarin は、この状態でシステム サービスと正しく対話ことはできません。 コンピューターを再起動では、システム サービスを再起動し、問題を解決します。

エラーに基づいて`system.log`Bonjour にこの問題を関連する可能性がありますが表示されます (`mDNSResponder`)。 別の WiFi ネットワーク間の変更の問題に達する確率を高めるようです。

## <a name="references"></a>参照

*   [バグ 11789 - MonoTouch.MobileDevice.MobileDeviceException:AMDeviceNotificationSubscribe が返されます。0XE8000063 [解決済み既定値は 20]](https://bugzilla.xamarin.com/show_bug.cgi?id=11789)
