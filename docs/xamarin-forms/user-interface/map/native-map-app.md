---
title: Xamarin からネイティブマップアプリを起動します。
description: 各プラットフォームのネイティブマップアプリは、Xamarin. Essentials ランチャークラスによって Xamarin. Forms アプリケーションから起動できます。
ms.prod: xamarin
ms.assetid: 5CF7CD67-3F20-4D80-B99E-D35A5FD1019A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/30/2019
ms.openlocfilehash: 54776d28bb75b152a6402e4d531d1baa4f724cba
ms.sourcegitcommit: 3ea19e3a51515b30349d03c70a5b3acd7eca7fe7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/01/2019
ms.locfileid: "73426325"
---
# <a name="launch-the-native-map-app-from-xamarinforms"></a>Xamarin からネイティブマップアプリを起動します。

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

各プラットフォームのネイティブマップアプリは、xamarin. Essentials `Launcher` クラスを使用して Xamarin アプリケーションから起動できます。 このクラスを使用すると、アプリケーションは、カスタム URI スキームを使用して別のアプリを開くことができます。 ランチャー機能は、`OpenAsync` メソッドを使用して呼び出すことができます。これを行うには、開くカスタム URL スキームを表す `string` または `Uri` 引数を渡します。 Xamarin の詳細については、「 [xamarin. essentials](~/essentials/index.md?context=xamarin/xamarin-forms)」を参照してください。

> [!NOTE]
> Xamarin `Launcher` クラスを使用する代わりに、`Map` クラスを使用することもできます。 詳細については、「 [Xamarin. Essentials: Map](~/essentials/maps.md?context=xamarin/xamarin-forms)」を参照してください。

各プラットフォームの maps アプリでは、一意のカスタム URI スキームが使用されます。 IOS での maps URI スキームの詳細については、「 [Map Links](https://developer.apple.com/library/archive/featuredarticles/iPhoneURLScheme_Reference/MapLinks/MapLinks.html) on developer.apple.com」を参照してください。 Android の maps URI スキームの詳細については、「 [Maps 開発者ガイド](https://developer.android.com/guide/components/intents-common.html#Maps)」および「developers.android.com で[の Android 用の Google maps インテント](https://developers.google.com/maps/documentation/urls/android-intents)」を参照してください。 ユニバーサル Windows プラットフォーム (UWP) のマップ URI スキームの詳細については、「 [Windows maps アプリを起動する](/windows/uwp/launch-resume/launch-maps-app)」を参照してください。

## <a name="launch-the-map-app-at-a-specific-location"></a>特定の場所でマップアプリを起動する

ネイティブ maps アプリ内の場所は、各マップアプリのカスタム URI スキームに適切なクエリパラメーターを追加することによって開くことができます。

```csharp
if (Device.RuntimePlatform == Device.iOS)
{
    // https://developer.apple.com/library/ios/featuredarticles/iPhoneURLScheme_Reference/MapLinks/MapLinks.html
    await Launcher.OpenAsync("http://maps.apple.com/?q=394+Pacific+Ave+San+Francisco+CA");
}
else if (Device.RuntimePlatform == Device.Android)
{
    // open the maps app directly
    await Launcher.OpenAsync("geo:0,0?q=394+Pacific+Ave+San+Francisco+CA");
}
else if (Device.RuntimePlatform == Device.UWP)
{
    await Launcher.OpenAsync("bingmaps:?where=394 Pacific Ave San Francisco CA");
}
```

このコード例では、各プラットフォームでネイティブマップアプリが起動され、指定された場所を表すピンの中央にマップが配置されています。

[![IOS と Android のネイティブマップアプリのスクリーンショット](native-map-app-images/location.png "ネイティブマップアプリ")](native-map-app-images/location-large.png#lightbox "ネイティブマップアプリ")

## <a name="launch-the-map-app-with-directions"></a>指示に従ってマップアプリを起動する

ネイティブ maps アプリを起動するには、各マップアプリのカスタム URI スキームに適切なクエリパラメーターを追加します。

```csharp
if (Device.RuntimePlatform == Device.iOS)
{
    // https://developer.apple.com/library/ios/featuredarticles/iPhoneURLScheme_Reference/MapLinks/MapLinks.html
    await Launcher.OpenAsync("http://maps.apple.com/?daddr=San+Francisco,+CA&saddr=cupertino");
}
else if (Device.RuntimePlatform == Device.Android)
{
    // opens the 'task chooser' so the user can pick Maps, Chrome or other mapping app
    await Launcher.OpenAsync("http://maps.google.com/?daddr=San+Francisco,+CA&saddr=Mountain+View");
}
else if (Device.RuntimePlatform == Device.UWP)
{
    await Launcher.OpenAsync("bingmaps:?rtp=adr.394 Pacific Ave San Francisco CA~adr.One Microsoft Way Redmond WA 98052");
}
```

このコード例では、各プラットフォームでネイティブマップアプリが起動され、指定された場所のルートの中央にマップが配置されています。

[![IOS と Android のネイティブマップアプリルートのスクリーンショット](native-map-app-images/directions.png "ネイティブマップアプリの方向")](native-map-app-images/directions-large.png#lightbox "ネイティブマップアプリの方向")

## <a name="related-links"></a>関連リンク

- [Maps サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Xamarin.Essentials](~/essentials/index.md?context=xamarin/xamarin-forms)
- [マップリンク](https://developer.apple.com/library/archive/featuredarticles/iPhoneURLScheme_Reference/MapLinks/MapLinks.html)
- [Maps 開発者ガイド](https://developer.android.com/guide/components/intents-common.html#Maps)
- [Android 向けの Google Maps インテント](https://developers.google.com/maps/documentation/)
- [Windows Maps アプリを起動する](/windows/uwp/launch-resume/launch-maps-app)
