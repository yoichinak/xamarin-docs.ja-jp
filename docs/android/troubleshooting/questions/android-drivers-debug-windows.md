---
title: Windows 上で Android をデバッグするために必要な USB ドライバーを教えてください
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 36EC7341-A2A4-409C-BD4F-330BAC505123
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/22/2018
ms.openlocfilehash: 21fd8eff64d374e52e64194524a8c096cdf4d90e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027035"
---
# <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>Windows 上で Android をデバッグするために必要な USB ドライバーを教えてください

## <a name="finding-usb-drivers"></a>USB ドライバーの検索

Windows で開発するときに Android デバイスでデバッグするには互換性のある USB ドライバーをインストールする必要があります。 Android SDK マネージャーには、既定で "Google USB ドライバー" が含まれています。これにより、次の説明に従って、デバイスのデバイスのサポートが追加されます: [https://developer.android.com/sdk/win-usb.html](https://developer.android.com/sdk/win-usb.html)

その他のデバイスには、デバイスの製造元によって特別に発行された USB ドライバーが必要です。 このガイドには、最も一般的な製造元向けのいくつかのリンクが含まれています: [https://developer.android.com/tools/extras/oem-usb.html](https://developer.android.com/tools/extras/oem-usb.html)

## <a name="alternatives"></a>選択肢

Manfacturer よっては、必要な USB ドライバーを正確に追跡するのが困難な場合があります。 Windows で開発された Android アプリをテストするための代替手段として、Android エミュレーターの使用や外部テストサービスの使用などがあります。 その例には次のものがあります。

- [App Center テスト](https://docs.microsoft.com/appcenter/test-cloud/)-クラウドテストサービスは、数百の実際の Android デバイスで実行されます。

- [Visual Studio Emulator for Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/)

- [Android Emulator でのデバッグ](~/android/deploy-test/debugging/debug-on-emulator.md)
