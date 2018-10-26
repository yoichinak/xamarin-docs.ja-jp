---
title: タブ付きレイ アウト
description: Android でのタブ付きレイ アウトの概要
ms.prod: xamarin
ms.assetid: 1CFF590A-AC86-C3B3-36CA-A70248BC7F97
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/08/2017
ms.openlocfilehash: 5d88ffb44d12ee142314c74ca8e749164cbfe3b9
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105962"
---
# <a name="tabbed-layouts"></a>タブ付きレイ アウト


## <a name="overview"></a>概要

タブは、その簡潔さと使いやすさのためのモバイル アプリケーションの一般的なユーザー インターフェイス パターンです。 アプリケーションのさまざまな画面間を移動する一貫性のある簡単な方法を提供します。 Android では、タブ付きインターフェイスのいくつかの API があります。 

-   **ActionBar** &ndash;これは、一貫性を提供するという目的で Android 3.0 (API レベル 11) で導入された API の新しいセットの一部のナビゲーションとビューの切り替えのインターフェイス。 Android 2.2 (API レベル 8) に戻る移植されましたが、 [Android サポート ライブラリ v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)します。 

-   **PagerTabStrip** &ndash;の現在、次と前のページを示します、`ViewPager`します。 `ViewPager` のみ利用可能は[Android サポート ライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)します。
     詳細については`PagerTabStrip`を参照してください[ViewPager](~/android/user-interface/controls/view-pager/index.md)します。

-   **ツールバー** &ndash; `Toolbar`を置き換える新しいより柔軟なアクション バー コンポーネント`ActionBar`します。 `Toolbar` Android 5.0 Lollipop で使用できる以降も古いバージョンを使用して Android の使用と、 [Android サポート ライブラリ v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet パッケージ。 
    `Toolbar` Android アプリで使用する推奨される操作バー コンポーネントは現在です。
    詳細については、次を参照してください。[ツールバー](~/android/user-interface/controls/tool-bar/index.md)します。 



## <a name="related-links"></a>関連リンク

- [素材のデザインのタブ](https://material.io/guidelines/components/tabs.html)- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Android サポート ライブラリ v7 AppCompat NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [v7 appcompat ライブラリ](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
