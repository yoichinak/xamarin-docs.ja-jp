---
title: "ユーザーの所在地が 8、iOS で動作していません"
ms.topic: article
ms.prod: xamarin
ms.assetid: 9BE92C99-C9C5-427E-ADE4-789DF258BACE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 4bb8d204d961e386b970cf206c9fa3fa06b80b3b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="user-location-not-working-in-ios-8"></a>ユーザーの所在地が 8、iOS で動作していません

テキスト エディター内で: を開く、Info.plist の設定し、次を追加します。

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>You are about to use location!</string>

<key>NSLocationAlwaysUsageDescription</key>
<string>This will be called if location is used behind the scenes</string>
```

MainViewController.cs 内で、以下を呼び出す必要があります。

```csharp
iPhoneLocationManager.RequestWhenInUseAuthorization ();
```

例:

```cs
if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
    iPhoneLocationManager.RequestWhenInUseAuthorization ();
    iPhoneLocationManager.LocationsUpdated += (object sender, CLLocationsUpdatedEventArgs e) => {
        UpdateLocation (mainScreen, e.Locations [e.Locations.Length - 1]);
        };
} else {
    // this won't be called on iOS 6 (deprecated)
    iPhoneLocationManager.UpdatedLocation += (object sender, CLLocationUpdatedEventArgs e) => {
        UpdateLocation (mainScreen, e.NewLocation);
        };
}
```
