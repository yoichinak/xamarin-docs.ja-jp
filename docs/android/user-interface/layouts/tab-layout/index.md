---
title: タブ付きレイアウト
description: Android でのタブ付きレイアウトの概要
ms.prod: xamarin
ms.assetid: 1CFF590A-AC86-C3B3-36CA-A70248BC7F97
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/08/2017
ms.openlocfilehash: 1e0632eb921b25fef40b8f0483ab80d62c9e1235
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70762188"
---
# <a name="tabbed-layouts"></a>タブ付きレイアウト

## <a name="overview"></a>概要

タブは、単純さと使いやすさが理由で、モバイルアプリケーションの一般的なユーザーインターフェイスパターンです。 アプリケーションのさまざまな画面間を移動するための一貫した簡単な方法を提供します。 Android には、タブ付きインターフェイス用の API がいくつかあります。 

- **Actionbar**&ndash;これは、Android 3.0 (api レベル 11) で導入された新しい api のセットに含まれており、一貫性のあるナビゲーションとビュー切り替えインターフェイスを提供することを目標としています。 Android[サポートライブラリ v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)を使用して、android 2.2 (API レベル 8) に移植されています。 

- **Pagertabstrip**の現在、次、および前のページを示します。`ViewPager` &ndash; `ViewPager`は、 [Android サポートライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)を通じてのみ使用できます。
     の詳細`PagerTabStrip`については、「 [viewpager](~/android/user-interface/controls/view-pager/index.md)」を参照してください。

- **ツールバー**は、を置き換える`ActionBar`新しいより柔軟な操作バーコンポーネントです。 &ndash; `Toolbar` `Toolbar`は、android 5.0 のロリポップ以降で使用できます。また、android [Support Library v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet パッケージを使用して、以前のバージョンの android でも使用できます。 
    `Toolbar`は現在、Android アプリで使用するために推奨されるアクションバーコンポーネントです。
    詳細については、「[ツールバー](~/android/user-interface/controls/tool-bar/index.md)」を参照してください。 

## <a name="related-links"></a>関連リンク

- [素材のデザイン-タブ](https://material.io/guidelines/components/tabs.html)- の[actionbar](https://developer.android.com/guide/topics/ui/actionbar.html)
- [Android サポートライブラリ v7 AppCompat NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [v7 appcompat ライブラリ](https://developer.android.com/tools/support-library/features.html#v7-appcompat)
