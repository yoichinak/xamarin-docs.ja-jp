---
title: Windows 上のアプリをデバッグする必要があります USB ドライバーの種類をしますか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 36EC7341-A2A4-409C-BD4F-330BAC505123
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: 8b49d02b9670e66d04060375e59b005905c41bf7
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935257"
---
# <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>Windows 上のアプリをデバッグする必要があります USB ドライバーの種類をしますか。

## <a name="finding-usb-drivers"></a>USB ドライバーを見つける

Windows; で開発するときに Android デバイスでデバッグするには互換性のある USB ドライバーをインストールする必要があります。 Android SDK Manager には、既定では、前述のように Nexus デバイスのサポートを追加する「Google USB ドライバー」が含まれています。 [http://developer.android.com/sdk/win-usb.html](http://developer.android.com/sdk/win-usb.html)

その他のデバイスでは、具体的には、デバイスの製造元によって発行された USB ドライバーが必要です。 このガイドでは、最も一般的な製造元の一部のリンクが含まれています。 [http://developer.android.com/tools/extras/oem-usb.html](http://developer.android.com/tools/extras/oem-usb.html)

## <a name="alternatives"></a>代替手段

Manfacturer によっては、必要に応じて、正確な USB ドライバーを追跡するが困難ができます。 Android エミュレーターを使用して、外部テスト サービスを使用するなどの Windows での Android アプリをテストする代替策を開発しました。 その例には次のものがあります。

- [アプリ Center Test](https://docs.microsoft.com/appcenter/test-cloud/) - クラウド サービスを数百の実際の Android デバイスで実行をテストします。

- [Visual Studio Emulator for Android](https://www.visualstudio.com/en-us/features/msft-android-emulator-vs.aspx)

- [Android エミュレーターでデバッグします。](~/android/deploy-test/debugging/debug-on-emulator.md)

