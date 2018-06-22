---
title: "Android.Support.v7.AppCompat - 指定した名前と一致するリソースが見つかりません: attr ' android: actionModeShareDrawable'"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5814069C-FC43-41DE-B5A5-024D05E59929
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 07655587642c3e1aa94d035e76f6f6758340546d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30774897"
---
# <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawable"></a>Android.Support.v7.AppCompat - 指定した名前と一致するリソースが見つかりません: attr ' android: actionModeShareDrawable'

1. 最新の他の機能だけでなく、Android 5.0 (API 21)、Android SDK Manager を使用して SDK をダウンロードすることを確認してください。

2. CompileSdkVersion に 21 に設定を使って、アプリケーションをコンパイルすることを確認します。 21 同様に、targetSdkVersion を設定することができます。

3. API 19 などの以前のバージョンを必要とする場合は、それぞれのバージョンの Nuget ページをダウンロードしてください。

[https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)

*注*: を手動でパッケージ マネージャー コンソールを使用してこれをインストールする場合は、インストールすることも、同じバージョンの Xamarin.Android.Support.v4 を確認してください

[https://www.nuget.org/packages/Xamarin.Android.Support.v4/](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)

スタック オーバーフローのリファレンス: [http://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro](http://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro)

## <a name="see-also"></a>関連項目

- [インストールする必要がある Android SDK パッケージを教えてください](~/android/troubleshooting/questions/install-android-sdk-packages.md)

