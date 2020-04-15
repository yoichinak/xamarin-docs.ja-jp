---
title: "Android.Support.v7.AppCompat - 指定された名前: 属性 'android:actionModeShareDrawable' と一致するリソースが見つかりません"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5814069C-FC43-41DE-B5A5-024D05E59929
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: e688bd27d1116b2a77a12ccd6da29ea582053581
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "75728110"
---
# <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawable"></a>Android.Support.v7.AppCompat - 指定された名前: 属性 'android:actionModeShareDrawable' と一致するリソースが見つかりません

1. Android SDK マネージャーを使用して、最新のエクストラと Android 5.0 (API 21) SDK をダウンロードしてください。

2. compileSdkVersion を 21 に設定してアプリケーションをコンパイルしていることを確認してください。 必要に応じて、targetSdkVersion を 21 に設定することもできます。

3. API 19 などの以前のバージョンが必要な場合は、NuGet のページにある該当するバージョンをダウンロードしてください。

[https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)

> [!NOTE]
> パッケージ マネージャー コンソールを使用してこれを手動でインストールする場合は、必ず同じバージョンの Xamarin.Android.Support.v4 もインストールしてください

[https://www.nuget.org/packages/Xamarin.Android.Support.v4/](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)

スタック オーバーフロー リファレンス: [https://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro](https://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro)

## <a name="see-also"></a>関連項目

- [インストールする必要がある Android SDK パッケージを教えてください](~/android/troubleshooting/questions/install-android-sdk-packages.md)
