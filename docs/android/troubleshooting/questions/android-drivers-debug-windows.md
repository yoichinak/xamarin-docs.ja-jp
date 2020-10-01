---
title: Windows 上で Android をデバッグするために必要な USB ドライバーを教えてください
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 36EC7341-A2A4-409C-BD4F-330BAC505123
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/22/2018
ms.openlocfilehash: c07f9099f3b76ed86a235883ce335ce19a426b99
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457888"
---
# <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>Windows 上で Android をデバッグするために必要な USB ドライバーを教えてください

## <a name="finding-usb-drivers"></a>USB ドライバーを探す

Windows で開発するときに Android デバイスでデバッグするには、互換性のある USB ドライバーをインストールする必要があります。 Android SDK マネージャーには、既定で "Google USB ドライバー" が含まれています。これにより、[https://developer.android.com/sdk/win-usb.html](https://developer.android.com/sdk/win-usb.html) で説明されているように Nexus デバイスのサポートが追加されます。

それ以外のデバイスの場合は、デバイスの製造元によって個別に公開されている USB ドライバーが必要です。 最も一般的な製造元へのリンクが、このガイド ([https://developer.android.com/tools/extras/oem-usb.html](https://developer.android.com/tools/extras/oem-usb.html)) に含まれています。

## <a name="alternatives"></a>代替

製造元よっては、必要な USB ドライバーを正確に追跡するのが困難な場合があります。 Windows で開発された Android アプリをテストするための代替手段としては、Android エミュレーターの使用や、外部テスト サービスの使用などがあります。 その例には次のものがあります。

- [App Center テスト](/appcenter/test-cloud/) - 多数の実際の Android デバイスで実行されるクラウド テスト サービス。

- [Visual Studio Emulator for Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/)

- [Android Emulator でのデバッグ](~/android/deploy-test/debugging/debug-on-emulator.md)