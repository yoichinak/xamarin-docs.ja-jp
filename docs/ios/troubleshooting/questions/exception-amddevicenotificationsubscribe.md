---
title: "System.Exception AMDeviceNotificationSubscribe が返されます。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7E4ACC7E-F4FB-46C1-8837-C7FBAAFB2DC7
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 14b138138f3d6297ff587aacc9ea5cde9df54381
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="systemexception-amdevicenotificationsubscribe-returned-"></a>System.Exception AMDeviceNotificationSubscribe が返されます。

> [!IMPORTANT]
> Xamarin の最近のバージョンでは、この問題を解決されています。 ただし、ソフトウェアの最新のバージョンで問題が発生した場合を送信してください、[新しいバグ](~/cross-platform/troubleshooting/questions/howto-file-bug.md)すべてのバージョン管理情報と完全のビルド ログ出力します。


## <a name="fix"></a>修正

1.  Kill、`usbmuxd`処理できるように、システムが再起動されます。

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

For Mac またはで初めて Visual Studio を起動するときに、このメッセージがエラー ダイアログに表示されることができます、 `mtbserver.log` Xamarin.iOS Build Host アプリ内のファイル (**Xamarin.iOS Build Host > ビルド ホスト ログの表示**)。

これは、一般的でない問題であることに注意してください。 表示される可能性が高いその他のエラーがある場合は、Visual Studio には、Mac ビルド ホストへの接続に問題があることを`mtbserver.log`ファイル。

### <a name="errors-in-systemlog"></a>必ず内のエラー

いくつか次の 2 つのケースのエラー メッセージも表示されます繰り返しの`/var/log/system.log`:

```csharp
17:17:11.369 usbmuxd[55040]: dnssd_clientstub ConnectToServer: socket failed 24 Too many open files
17:17:11.369 com.apple.usbmuxd[55040]: _BrowseReplyReceivedCallback Error doing DNSServiceResolve(): -65539
```

## <a name="additional-information"></a>追加情報

エラーの根本原因の 1 つの推定値は、OS X がまれなケースでシステム サービスに iOS デバイスおよびシミュレーターでの情報をレポート作成を担当することができますが、予期しない状態を入力します。 Xamarin は、この状態でシステム サービスと正しく対話ことはできません。 コンピューターを再起動すると、システム サービスを再起動し、問題が解決します。

エラーに基づいて`system.log`Bonjour にこの問題が関連する表示されます (`mDNSResponder`)。 別の WiFi ネットワーク間の切り替え、問題のヒットの確率を高めるようです。

## <a name="references"></a>参照

*   [バグ 11789 - MonoTouch.MobileDevice.MobileDeviceException: AMDeviceNotificationSubscribe が返されます 0xe8000063 [解決された既定値は 20]。](https://bugzilla.xamarin.com/show_bug.cgi?id=11789)
