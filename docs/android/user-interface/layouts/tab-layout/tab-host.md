---
title: "TabHost タブ レイアウト"
description: "この記事では、高レベルの概要を説明します。、Xamarin.Android アプリケーションでタブ付きレイアウトを作成するため、TabHost、古い API を使用します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1CFF590A-AC86-C3B3-36CA-A70248BC7F97
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 10/25/2017
ms.openlocfilehash: ff61ca0a2bca466da3e33c93a17944915328b70c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="tab-layout-with-tabhost"></a>TabHost タブ レイアウト

_この記事では、高レベルの概要を説明します。、Xamarin.Android アプリケーションでタブ付きレイアウトを作成するため、TabHost、古い API を使用します。_

<a name="Overview" />

## <a name="overview"></a>概要

> [!NOTE]
> **注:** `TabHost`は Google によって推奨されていませんする古い API です。 使用してタブ付きのアプリケーションを構築する開発者は、[アクションバー](~/android/user-interface/controls/action-bar.md)です。 `ActionBar`は Android のすべてのバージョンで使用できます。 Android 3.0 (API レベル 11) で初めて導入されました、Android 2.2 (API レベル 8) および Android 2.3 (API レベル 10) に移植された戻る、 [V7 AppCompat ライブラリ](http://developer.android.com/tools/support-library/features.html#v7-appcompat)、を介して Xamarin.Android に利用できる、 [XamarinAndroid のサポート ライブラリ - V7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)パッケージです。

`TabHost`が Android 2.2 および Android 2.3 をサポートする必要がありますを使用することはできません Xamarin.Android アプリケーションに最適なはタブ付きユーザー interfacesIt を作成する古い、元の API **ActionBarSherlock**です。
次の 5 つのコンポーネントはすべてに関連する、 `TabHost` API:

-  **TabActivity** &ndash;これは、特殊なビューのコンテナーとして機能する、 `TabHost` (下記参照)。

-  **TabHost** &ndash;これは、タブ付きの ui コンテナー。 2 つの子をホストしている: タブのラベルの一覧と`FrameLayout`タブのコンテンツを表示します。

-  **TabWidget** &ndash;このコントロールの各タブに 1 つずつタブ ラベルの一覧を表示する、`TabHost`です。 各タブに、`TabHost`によって表される、`TabHost.TabSpec`オブジェクト。 このオブジェクトには、それぞれのタブのメタ データが含まれています。ユーザーは、タブを選択したときに、`TabHost`適切なタブを表示することによって、選択範囲に応答します。

-  **FrameLayout** &ndash;タブ付きの UI があります、`FrameLayout`の内側に含まれる、`TabHost`です。 タブの内容を表示する使用されます。

-  **アクティビティまたはビュー** &ndash;アクティビティまたはビューの 1 つのいずれかが表示されます、タブを選択すると、`FrameLayout`です。

次の図は、これらすべてのコンポーネントとの関係一緒に示しています。

![TabHost 内 TabWidget 内のフレームのレイアウトを示すダイアグラム](tab-host-images/image03.png)

アクティビティまたはビューのいずれかのタブのコンテンツがあります。 ビューは、比較的軽量で単純なものが、多数のアクティビティに関連付けられていないコード co-habitating されない可能性があります。 これは、分離の問題と保守が困難である肥大化クラスが不完全になります。 これに対し、活動はシステム リソースを必要としますが、それ自身の別のクラスにカプセル化されたそれぞれのタブのロジックを使用するよりモジュール式のアプローチを許可します。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事には、旧バージョンの高レベルのコンポーネントが説明されている`TabHost`for Android およびすべての関係が一緒に API です。



## <a name="related-links"></a>関連リンク

- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [TabHost](https://developer.xamarin.com/api/type/Android.Widget.TabHost/)
- [TabHost.TabSpec](https://developer.xamarin.com/api/type/Android.Widget.TabHost+TabSpec/)
- [TabWidget](https://developer.xamarin.com/api/type/Android.Widget.TabWidget/)
- [TabActivity](https://developer.xamarin.com/api/type/Android.App.TabActivity/)
- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Android のサポート ライブラリ v7 AppCompat NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [v7 appcompat ライブラリ](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
