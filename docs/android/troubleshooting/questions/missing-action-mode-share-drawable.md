---
title: "Android.Support.v7.AppCompat - 指定した名前と一致するリソースが見つかりません: ' android: actionModeShareDrawable' attr"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5814069C-FC43-41DE-B5A5-024D05E59929
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: fea681ac3b99abed09d3d3e745bd4bf6015970df
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112417"
---
# <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawable"></a>Android.Support.v7.AppCompat - 指定した名前と一致するリソースが見つかりません: ' android: actionModeShareDrawable' attr

1. 最新の他の機能と、Android 5.0 (API 21)、Android SDK Manager を使用して SDK をダウンロードすることを確認します。

2. CompileSdkVersion 21 に設定を使って、アプリケーションをコンパイルすることを確認します。 21 のように、[targetsdkversion] オプションで設定できます。

3. API 19 などの以前のバージョンを必要とする場合は、Nuget のページにそれぞれのバージョンをダウンロードしてください。

[https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)

*注*: を手動でパッケージ マネージャー コンソールを使用してこれをインストールする場合も Xamarin.Android.Support.v4 の同じバージョンをインストールすることを確認してください

[https://www.nuget.org/packages/Xamarin.Android.Support.v4/](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)

スタック オーバーフロー リファレンス: [http://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro](http://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro)

## <a name="see-also"></a>関連項目

- [インストールする必要がある Android SDK パッケージを教えてください](~/android/troubleshooting/questions/install-android-sdk-packages.md)

