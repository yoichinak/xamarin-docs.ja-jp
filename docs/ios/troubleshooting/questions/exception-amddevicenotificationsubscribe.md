---
title: System.Exception AMDeviceNotificationSubscribe returned ... (System.Exception AMDeviceNotificationSubscribe が ... を返しました)
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7E4ACC7E-F4FB-46C1-8837-C7FBAAFB2DC7
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: b6d2931168132630233112b9515071ac7fd07843
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769745"
---
# <a name="systemexception-amdevicenotificationsubscribe-returned-"></a>System.Exception AMDeviceNotificationSubscribe returned ... (System.Exception AMDeviceNotificationSubscribe が ... を返しました)

> [!IMPORTANT]
> この問題は、最新バージョンの Xamarin で解決されました。 ただし、最新バージョンのソフトウェアで問題が発生した場合は、完全なバージョン管理情報と完全ビルドログ出力を使用して[新しいバグを作成](~/cross-platform/troubleshooting/questions/howto-file-bug.md)してください。

## <a name="fix"></a>Fix

1. プロセスを`usbmuxd`強制終了して、システムが再起動するようにします。

    ```csharp
    sudo killall -QUIT usbmuxd
    ```

2. それでも問題が解決しない場合は、Mac を再起動します。

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

このメッセージは、Visual Studio for Mac を最初に起動したときにエラーダイアログに表示`mtbserver.log`される場合があります。または、xamarin. ios ビルドホストアプリのファイル (xamarin のビルドホスト **> ビュービルドホストログ**) で表示されます。

これは珍しい問題ではないことに注意してください。 Visual Studio が Mac ビルドホストへの接続に問題がある場合は、その他のエラーが`mtbserver.log`ファイルに表示される可能性が高くなります。

### <a name="errors-in-systemlog"></a>System .log のエラー

場合によっては、次の2つのエラーメッセージ`/var/log/system.log`が繰り返し表示されることがあります。

```csharp
17:17:11.369 usbmuxd[55040]: dnssd_clientstub ConnectToServer: socket failed 24 Too many open files
17:17:11.369 com.apple.usbmuxd[55040]: _BrowseReplyReceivedCallback Error doing DNSServiceResolve(): -65539
```

## <a name="additional-information"></a>追加情報

エラーの根本的な原因の1つとして、iOS デバイスとシミュレーターの情報を報告する OS X システムサービスが、まれに予期しない状態になることがあります。 Xamarin は、この状態のシステムサービスと正常に通信できません。 コンピューターを再起動すると、システムサービスが再起動され、問題が解決されます。

この問題が Bonjour ( `system.log` `mDNSResponder`) に関連している可能性があるというエラーが表示されます。 異なる WiFi ネットワーク間で変更を行うと、問題が発生する可能性が高くなります。

## <a name="references"></a>リファレンス

* [バグ 11789-Monotouch.dialog. MobileDevice. MobileDeviceException:返される AMDeviceNotificationSubscribe:0xe8000063 [解決済み NORESPONSE]](https://bugzilla.xamarin.com/show_bug.cgi?id=11789)
