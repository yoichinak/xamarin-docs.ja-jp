---
title: "Android.Support.v7.AppCompat - 指定された名前: 属性 'android:actionModeShareDrawable' と一致するリソースが見つかりません"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5814069C-FC43-41DE-B5A5-024D05E59929
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: ff2a586773e33a1f4cf78657c3c69c22e79ed047
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61153374"
---
# <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawable"></a>Android.Support.v7.AppCompat - 指定された名前: 属性 'android:actionModeShareDrawable' と一致するリソースが見つかりません

1. 最新の他の機能と、Android 5.0 (API 21)、Android SDK Manager を使用して SDK をダウンロードすることを確認します。

2. CompileSdkVersion 21 に設定を使って、アプリケーションをコンパイルすることを確認します。 21 のように、[targetsdkversion] オプションで設定できます。

3. API 19 などの以前のバージョンを必要とする場合は、Nuget のページにそれぞれのバージョンをダウンロードしてください。

[https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)

*注*:手動でパッケージ マネージャー コンソールを使用してこれをインストールする場合も Xamarin.Android.Support.v4 の同じバージョンをインストールすることを確認してください。

[https://www.nuget.org/packages/Xamarin.Android.Support.v4/](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)

スタック オーバーフロー リファレンス: [https://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro](https://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro)

## <a name="see-also"></a>関連項目

- [インストールする必要がある Android SDK パッケージを教えてください](~/android/troubleshooting/questions/install-android-sdk-packages.md)

