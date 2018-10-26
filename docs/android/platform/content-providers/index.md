---
title: ContentProviders の概要
description: Android オペレーティング システムでは、コンテンツ プロバイダーを使用して、メディア ファイル、連絡先や予定表の情報などの共有データへのアクセスを容易にします。 この記事を ContentProvider クラスについて説明し、その使用方法の 2 つの例を示します。
ms.prod: xamarin
ms.assetid: 6E1810AA-EB70-9AD0-1B32-D9418908CC97
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 4105200c48e41b142fc71e3a524023790b683cdb
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105988"
---
# <a name="intro-to-contentproviders"></a>ContentProviders の概要

_Android オペレーティング システムでは、コンテンツ プロバイダーを使用して、メディア ファイル、連絡先や予定表の情報などの共有データへのアクセスを容易にします。この記事を ContentProvider クラスについて説明し、その使用方法の 2 つの例を示します。_


## <a name="content-providers-overview"></a>コンテンツ プロバイダーの概要

A *ContentProvider*データ リポジトリをカプセル化し、それにアクセスする API を提供します。 プロバイダーは、通常はも、データの表示/管理するための UI を提供する Android アプリケーションの一部として存在します。 コンテンツ プロバイダーを使用して主なメリットがプロバイダーのクライアント オブジェクトを使用してカプセル化されたデータを簡単にアクセスするには、他のアプリケーションを有効にする (と呼ばれる、 *ContentResolver*)。 同時に、コンテンツ プロバイダーとコンテンツの競合回避モジュールは、データへのアクセスが簡単にビルドおよび使用の一貫性のあるアプリケーション間の API を提供します。 任意のアプリケーションを使用することできます`ContentProviders`内部的にデータを管理し、他のアプリケーションに公開します。

A`ContentProvider`カスタム検索の候補を提供するアプリケーションにも必要または他のアプリケーションに貼り付けるアプリケーションから複雑なデータをコピーする機能を提供する場合。 このドキュメントにアクセスしてビルドする方法を示しています。 `ContentProviders` Xamarin.Android でします。

このセクションの構造は次のとおりです。

- **そのしくみ**&ndash;内容の概要、`ContentProvider`とどのように機能は設計されています。

- **コンテンツ プロバイダーを使用する**&ndash;連絡先リストにアクセスする例。

- **ContentProvider を使用してデータを共有する**&ndash;がかかると、`ContentProvider`同じアプリケーション内で。

`ContentProviders` データを操作するカーソルを Listview の設定に使用する多くの場合。 参照してください、 [Listview と Adapter ガイド](~/android/user-interface/layouts/list-view/index.md)それらのクラスを使用する方法の詳細。

`ContentProviders` によって公開されている Android (またはその他のアプリケーション) は、アプリケーションで他のソースからデータを含める簡単な方法です。 アクセスと連絡先の一覧、写真や、アプリケーション内からの予定表イベントなどのデータの表示を許可するされ、ユーザーがそのデータ操作します。

カスタム`ContentProviders`(カスタム検索、コピー/貼り付けなどの特別な用途を含め) その他のアプリケーションで使用したり、独自のアプリ内で使用できるデータをパッケージ化するのに便利です。

このセクションのトピックで使用して、書き込みの簡単な例を提供する`ContentProvider`コード。



## <a name="related-links"></a>関連リンク

- [ContactsAdapter デモ (サンプル)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ContactsAdapterDemo/)
- [SimpleContentProvider (サンプル)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SimpleContentProvider)
- [コンテンツ プロバイダーの開発者ガイド](http://developer.android.com/guide/topics/providers/content-providers.html)
- [ContentProvider クラスのリファレンス](https://developer.xamarin.com/api/type/Android.Content.ContentProvider/)
- [ContentResolver クラスのリファレンス](https://developer.xamarin.com/api/type/Android.Content.ContentResolver/)
- [ListView クラスのリファレンス](https://developer.xamarin.com/api/type/Android.Widget.ListView/)
- [CursorAdapter クラスのリファレンス](https://developer.xamarin.com/api/type/Android.Widget.CursorAdapter/)
- [UriMatcher クラスのリファレンス](https://developer.xamarin.com/api/type/Android.Content.UriMatcher/)
- [Android.Provider](https://developer.xamarin.com/api/namespace/Android.Provider/)
- [ContactsContract クラスのリファレンス](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract/)
