---
title: Windows 上の Android をデバッグする必要がある USB ドライバーの種類をでしょうか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 36EC7341-A2A4-409C-BD4F-330BAC505123
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/22/2018
ms.openlocfilehash: 8b996a1cc89acedc47c7169ec579dfb99dae788f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114536"
---
# <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>Windows 上の Android をデバッグする必要がある USB ドライバーの種類をでしょうか。

## <a name="finding-usb-drivers"></a>USB ドライバーを見つける

Windows で開発するときに、Android デバイスでデバッグするには互換性のある USB ドライバーをインストールする必要があります。 Android SDK Manager には、既定では、ここの説明に従って、Nexus デバイスのサポートを追加する"Google USB Driver"が含まれています。 [http://developer.android.com/sdk/win-usb.html](http://developer.android.com/sdk/win-usb.html)

その他のデバイスには、具体的には、デバイスの製造元によって発行された USB ドライバーが必要です。 このガイドでは、最も一般的な製造元のいくつかのリンクが含まれています。 [http://developer.android.com/tools/extras/oem-usb.html](http://developer.android.com/tools/extras/oem-usb.html)

## <a name="alternatives"></a>代替手段

Manfacturer によっては難しいために必要な正確な USB ドライバーを追跡することができます。 Android アプリをテストするためのいくつかの代替は、Windows、Android エミュレーターを使用して、外部のテスト サービスの使用などで開発します。 その例には次のものがあります。

- [App Center テスト](https://docs.microsoft.com/appcenter/test-cloud/)- クラウド サービスを何百もの実際の Android デバイスで実行をテストします。

- [Visual Studio Emulator for Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/)

- [Android Emulator でのデバッグ](~/android/deploy-test/debugging/debug-on-emulator.md)

