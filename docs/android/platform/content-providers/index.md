---
title: "ContentProviders の概要"
description: "Android オペレーティング システムでは、コンテンツ プロバイダーを使用して、メディア ファイル、連絡先や予定表の情報などの共有データへのアクセスを容易にします。 この記事では、ContentProvider クラスを紹介し、その使用方法の 2 つの例を示します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6E1810AA-EB70-9AD0-1B32-D9418908CC97
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.openlocfilehash: 42a20f4f0bc16cf42aa54fd0758db8a5db7c9048
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="intro-to-contentproviders"></a>ContentProviders の概要

_Android オペレーティング システムでは、コンテンツ プロバイダーを使用して、メディア ファイル、連絡先や予定表の情報などの共有データへのアクセスを容易にします。この記事では、ContentProvider クラスを紹介し、その使用方法の 2 つの例を示します。_


## <a name="content-providers-overview"></a>コンテンツ プロバイダーの概要

A *ContentProvider*データ リポジトリをカプセル化し、それにアクセスする API を提供します。 プロバイダーは、通常、データを表示する/管理するための UI も提供する Android アプリケーションの一部として存在します。 コンテンツ プロバイダーを使用する主な利点が簡単にプロバイダーのクライアント オブジェクトを使用してカプセル化されたデータにアクセスするには、他のアプリケーションを有効にする (と呼ばれる、 *ContentResolver*)。 同時に、コンテンツ プロバイダーとコンテンツ リゾルバーは、ビルドし、使用が簡単にデータ アクセスのための一貫性のあるアプリケーション間 API を提供します。 任意のアプリケーションが使用を選択できます`ContentProviders`内部的にデータを管理し、他のアプリケーションを公開します。

A`ContentProvider`カスタム検索候補を提供するアプリケーションにも必要、またはその他のアプリケーションに貼り付けるアプリケーションから複雑なデータをコピーする機能を提供する場合。 このドキュメントにアクセスしてビルドする方法を示しています。 `ContentProviders` Xamarin.Android とします。

このセクションの構造は次のとおりです。

- **そのしくみ**&ndash;新機能の概要については、`ContentProvider`しくみとよう設計されています。

- **コンテンツ プロバイダーを消費して**&ndash;連絡先リストにアクセスする例です。

- **ContentProvider を使用してデータを共有する**&ndash;手書き入力やがかかり、`ContentProvider`同じアプリケーションでします。

`ContentProviders` そのデータを操作するカーソルは Listview の設定によく使用されます。 参照してください、[の Listview およびアダプター ガイド](~/android/user-interface/layouts/list-view/index.md)これらのクラスを使用する方法の詳細。

`ContentProviders` によって公開されている Android (またはその他のアプリケーション) は、アプリケーションで他のソースからデータを含める簡単な方法です。 これらのアクセスと連絡先の一覧、写真、または、アプリケーション内から予定表イベントなどのデータを表示する、し、ユーザーがそのデータと対話できるようにできます。

カスタム`ContentProviders`(カスタムの検索、コピー/貼り付けなどの特別な用途を含む) 他のアプリケーションで使用したり、独自のアプリケーション内で使用するデータをパッケージ化する便利な手段です。

このセクションのトピックでは、簡単な例を使用し、書き込みを提供`ContentProvider`コード。



## <a name="related-links"></a>関連リンク

- [ContactsAdapter デモ (サンプル)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ContactsAdapterDemo/)
- [SimpleContentProvider (sample)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SimpleContentProvider)
- [コンテンツ プロバイダー開発者ガイド](http://developer.android.com/guide/topics/providers/content-providers.html)
- [ContentProvider クラスのリファレンス](https://developer.xamarin.com/api/type/Android.Content.ContentProvider/)
- [ContentResolver クラスのリファレンス](https://developer.xamarin.com/api/type/Android.Content.ContentResolver/)
- [ListView クラスのリファレンス](https://developer.xamarin.com/api/type/Android.Widget.ListView/)
- [CursorAdapter クラスのリファレンス](https://developer.xamarin.com/api/type/Android.Widget.CursorAdapter/)
- [UriMatcher クラスのリファレンス](https://developer.xamarin.com/api/type/Android.Content.UriMatcher/)
- [Android.Provider](https://developer.xamarin.com/api/namespace/Android.Provider/)
- [ContactsContract クラスのリファレンス](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract/)
