---
title: "Android.Support.v7.AppCompat - 指定された名前: 属性 'android:actionModeShareDrawable' と一致するリソースが見つかりません"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5814069C-FC43-41DE-B5A5-024D05E59929
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 4e544368d8fe2ab6316f9d3c79ff392f2bd83f09
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73026800"
---
# <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawable"></a>Android.Support.v7.AppCompat - 指定された名前: 属性 'android:actionModeShareDrawable' と一致するリソースが見つかりません

1. Android SDK Manager を使用して、最新のエクストラと Android 5.0 (API 21) SDK をダウンロードしてください。

2. Compilesdkversion を21に設定してアプリケーションをコンパイルしていることを確認します。 必要に応じて、targetSdkVersion を21に設定することもできます。

3. API 19 などの以前のバージョンが必要な場合は、Nuget のページに記載されているバージョンをダウンロードしてください。

[https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)

> [!NOTE]
> パッケージマネージャーコンソールを使用して手動でインストールした場合は、同じバージョンの Xamarin. Android. Android. サポートをインストールする必要があります。

[https://www.nuget.org/packages/Xamarin.Android.Support.v4/](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)

Stack Overflow リファレンス: [https://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro](https://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro)

## <a name="see-also"></a>参照

- [インストールする必要がある Android SDK パッケージを教えてください](~/android/troubleshooting/questions/install-android-sdk-packages.md)
