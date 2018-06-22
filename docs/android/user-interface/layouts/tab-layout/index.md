---
title: タブ付きレイアウト
description: Android でのタブ付きレイアウトの概要
ms.prod: xamarin
ms.assetid: 1CFF590A-AC86-C3B3-36CA-A70248BC7F97
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/08/2017
ms.openlocfilehash: 53ed5f91583d43839e96388194aea8c0d6ac5315
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/12/2018
ms.locfileid: "34149261"
---
# <a name="tabbed-layouts"></a>タブ付きレイアウト


## <a name="overview"></a>概要

タブは、その簡潔さと使いやすさのためのモバイル アプリケーションで人気のあるユーザー インターフェイス パターンです。 アプリケーションでのさまざまな画面間を移動する一貫性のある、簡単な方法を提供します。 Android では、タブ付きインターフェイスのいくつかの API があります。 

-   **アクションバー** &ndash;です一貫性を提供する目的で Android 3.0 (API レベル 11) で導入された API の新しいセットの一部のナビゲーションとビューの切り替えのインターフェイスです。 Android 2.2 (API レベル 8) に戻す移植されましたが、 [Android のサポート ライブラリ v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)です。 

-   **PagerTabStrip** &ndash;の現在、次と前のページを示す、`ViewPager`です。 `ViewPager` によってのみが使用可能[Android のサポート ライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)です。
     詳細については`PagerTabStrip`を参照してください[ViewPager](~/android/user-interface/controls/view-pager/index.md)です。

-   **ツールバー** &ndash; `Toolbar`を置き換える新しいおよびより柔軟なアクション バー コンポーネントは、`ActionBar`です。 `Toolbar` Android 5.0 ロリポップで使用できる以降も使用して Android の古いバージョンの使用と、 [Android のサポート ライブラリ v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet パッケージです。 
    `Toolbar` 現在は、Android アプリを使用して推奨される操作バー コンポーネント。
    詳細については、次を参照してください。[ツールバー](~/android/user-interface/controls/tool-bar/index.md)です。 



## <a name="related-links"></a>関連リンク

- [素材のデザインのタブ](https://material.io/guidelines/components/tabs.html)- [アクションバー](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Android のサポート ライブラリ v7 AppCompat NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [v7 appcompat ライブラリ](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
