---
title: "Android.Support.v7.AppCompat - 指定された名前: 属性 'android:actionModeShareDrawable' と一致するリソースが見つかりません"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5814069C-FC43-41DE-B5A5-024D05E59929
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 3bce50169919df83ba1ab76c29d1ecc5c1da0bd6
ms.sourcegitcommit: 13e43f510da37ad55f1c2f5de1913fb0aede6362
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/16/2019
ms.locfileid: "71021117"
---
# <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawable"></a>Android.Support.v7.AppCompat - 指定された名前: 属性 'android:actionModeShareDrawable' と一致するリソースが見つかりません

1. Android SDK Manager を使用して、最新のエクストラと Android 5.0 (API 21) SDK をダウンロードしてください。

2. Compilesdkversion を21に設定してアプリケーションをコンパイルしていることを確認します。 必要に応じて、targetSdkVersion を21に設定することもできます。

3. API 19 などの以前のバージョンが必要な場合は、Nuget のページに記載されているバージョンをダウンロードしてください。

[https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)

> [!NOTE]
> パッケージマネージャーコンソールを使用して手動でインストールした場合は、同じバージョンの Xamarin. Android. Android. サポートをインストールする必要があります。

[https://www.nuget.org/packages/Xamarin.Android.Support.v4/](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)

Stack Overflow リファレンス:[https://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro](https://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro)

## <a name="see-also"></a>関連項目

- [インストールする必要がある Android SDK パッケージを教えてください](~/android/troubleshooting/questions/install-android-sdk-packages.md)
