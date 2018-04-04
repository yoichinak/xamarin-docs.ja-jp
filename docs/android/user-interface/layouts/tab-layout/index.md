---
title: タブ付きレイアウト
description: Android でのタブ付きレイアウトの概要
ms.prod: xamarin
ms.assetid: 1CFF590A-AC86-C3B3-36CA-A70248BC7F97
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/23/2017
ms.openlocfilehash: 4095944bb630637a2e761af18796dacdef17785c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="tabbed-layouts"></a>タブ付きレイアウト


## <a name="overview"></a>概要

タブは、その簡潔さと使いやすさのためのモバイル アプリケーションで人気のあるユーザー インターフェイス パターンです。 アプリケーションでのさまざまな画面間を移動する一貫性のある、簡単な方法を提供します。 Android では、タブ付きインターフェイスのいくつかの API があります。 

-   **TabHost** &ndash;タブ付きのユーザー インターフェイスを作成するため、元の API です。 A`TabHost`ウィジェットがレイアウトに追加され、タブ付きビューのコンテナーとして機能します。 この API は廃止されて以降との使用はお勧めします。 

-   **アクションバー** &ndash;です一貫性を提供する目的で Android 3.0 (API レベル 11) で導入された API の新しいセットの一部のナビゲーションとビューの切り替えのインターフェイスです。 Android 2.2 (API レベル 8) に戻す移植されましたが、 [Android のサポート ライブラリ v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)です。 

-   **PagerTabStrip** &ndash;の現在、次と前のページを示す、`ViewPager`です。 `ViewPager` によってのみが使用可能[Android のサポート ライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)です。
     詳細については`PagerTabStrip`を参照してください[ViewPager](~/android/user-interface/controls/view-pager/index.md)です。

-   **ツールバー** &ndash; `Toolbar`を置き換える新しいおよびより柔軟なアクション バー コンポーネントは、`ActionBar`です。 `Toolbar` Android 5.0 ロリポップで使用できる以降も使用して Android の古いバージョンの使用と、 [Android のサポート ライブラリ v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet パッケージです。 
    `Toolbar` 現在は、Android アプリを使用して推奨される操作バー コンポーネント。
    詳細については、次を参照してください。[ツールバー](~/android/user-interface/controls/tool-bar/index.md)です。 


これらの API が視覚的に非常に異なっているし、は相互に互換性がありません。 次の図の画面`TabHost`と`ActionBar`サイド バイ サイド。 

![スクリーン ショットの TabHost 左側と右側のアクションバー](images/image01.png)

互換性のないこれら API は、Android 3.0 (API レベル 11) 以降は、重要な UI が変更されたのため存在します。 これらの UI の変更の 1 つが、[デザイン パターン バー アクション](http://www.androidpatterns.com/uap_pattern/action-bar)アプリケーションでのナビゲーションとキーの機能に簡単にアクセスするためのパターン。 `ActionBar`このパターンをサポートする API が導入されました。 

`ActionBar` API および過言ユーザー エクスペリエンスを改善を実現できる簡単です。 Android 2.2 に戻る移植されたし、Xamarin.Android アプリケーションをお勧めします。 

`TabHost` API と互換性が Android のすべてのバージョンでは使用する多くの労力が必要です、現在の整合性がありません[Android の UI ガイドライン](http://developer.android.com/design/index.html)です。 開発者は、この API の使用を避け、Xamarin.Android アプリケーションに対して新しいアクションバーを種類別する必要があります。 



## <a name="actionbarsherlock"></a>ActionBarSherlock

Android 2.2、アクションバー API の新しいルック アンド フィールは、サード パーティ製ライブラリを使用する開発者には移植アクションバー API の前に[ActionBarSherlock](http://actionbarsherlock.com)です。 ActionBarSherlock は、Android のサポート ライブラリの拡張機能で移植する Android 2.x アクション バーのデザイン パターンように設計します。 ネイティブ Android 3.0 以上を実行する場合、自動的に ActionBarSherlock を使用`ActionBar`Android によって提供される API。 Android の古いバージョンが、新しいのルック アンド フィールを模倣するカスタムの実装を使用して`ActionBar`API です。 [ActionBarSherlock コンポーネント](https://www.nuget.org/packages/xamstore-XamarinActionBarSherlock/)ActionBarSherlock を Xamarin.Android アプリケーションに追加するが容易です。 



## <a name="related-links"></a>関連リンク

- [TabHost の概要](tab-host.md)
- [TabHost チュートリアル](~/android/user-interface/layouts/tab-layout/creating-a-tabbed-ui.md)
- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Android のサポート ライブラリ v7 AppCompat NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [v7 appcompat ライブラリ](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
