---
title: Windows 上で Android をデバッグするために必要な USB ドライバーを教えてください
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 36EC7341-A2A4-409C-BD4F-330BAC505123
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/22/2018
ms.openlocfilehash: 85045967f5c63eb39c45f917b957d2a393a3a068
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "60945563"
---
# <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>Windows 上で Android をデバッグするために必要な USB ドライバーを教えてください

## <a name="finding-usb-drivers"></a>USB ドライバーを見つける

Windows で開発するときに、Android デバイスでデバッグするには互換性のある USB ドライバーをインストールする必要があります。 Android SDK Manager には、既定では、ここの説明に従って、Nexus デバイスのサポートを追加する"Google USB Driver"が含まれています。 [https://developer.android.com/sdk/win-usb.html](https://developer.android.com/sdk/win-usb.html)

その他のデバイスには、具体的には、デバイスの製造元によって発行された USB ドライバーが必要です。 このガイドでは、最も一般的な製造元のいくつかのリンクが含まれています。 [https://developer.android.com/tools/extras/oem-usb.html](https://developer.android.com/tools/extras/oem-usb.html)

## <a name="alternatives"></a>代替手段

Manfacturer によっては難しいために必要な正確な USB ドライバーを追跡することができます。 Android アプリをテストするためのいくつかの代替は、Windows、Android エミュレーターを使用して、外部のテスト サービスの使用などで開発します。 その例には次のものがあります。

- [App Center テスト](https://docs.microsoft.com/appcenter/test-cloud/)- クラウド サービスを何百もの実際の Android デバイスで実行をテストします。

- [Visual Studio Emulator for Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/)

- [Android Emulator でのデバッグ](~/android/deploy-test/debugging/debug-on-emulator.md)

