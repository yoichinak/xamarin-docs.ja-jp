---
title: ContentProviders の概要
description: Android オペレーティングシステムでは、コンテンツプロバイダーを使用して、メディアファイル、連絡先、予定表情報などの共有データへのアクセスを容易にします。 この記事では、ContentProvider クラスについて説明し、その使用方法の2つの例を示します。
ms.prod: xamarin
ms.assetid: 6E1810AA-EB70-9AD0-1B32-D9418908CC97
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 496e5c092c79f4f71bddaad30bea6acd1d58d375
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027541"
---
# <a name="intro-to-contentproviders"></a>ContentProviders の概要

_Android オペレーティングシステムでは、コンテンツプロバイダーを使用して、メディアファイル、連絡先、予定表情報などの共有データへのアクセスを容易にします。この記事では、ContentProvider クラスについて説明し、その使用方法の2つの例を示します。_

## <a name="content-providers-overview"></a>コンテンツプロバイダーの概要

*Contentprovider*は、データリポジトリをカプセル化し、それにアクセスするための API を提供します。 プロバイダーは、通常、データを表示または管理するための UI も提供する Android アプリケーションの一部として存在します。 コンテンツプロバイダーを使用する主な利点は、他のアプリケーションがプロバイダークライアントオブジェクト ( *Contentresolver*と呼ばれます) を使用して、カプセル化されたデータに簡単にアクセスできるようにすることです。 また、コンテンツプロバイダーとコンテンツリゾルバーは、簡単に構築して使用できるデータアクセスのための一貫したアプリケーション間 API を提供します。 アプリケーションでは、`ContentProviders` を使用して内部でデータを管理したり、他のアプリケーションに公開したりすることもできます。

また、アプリケーションでカスタムの検索候補を提供するために `ContentProvider` が必要な場合や、複雑なデータをアプリケーションからコピーして他のアプリケーションに貼り付けることができるようにする場合もあります。 このドキュメントでは、Xamarin Android で `ContentProviders` にアクセスしてビルドする方法について説明します。

このセクションの構造は次のとおりです。

- **そのしくみ**&ndash;、`ContentProvider` の設計と動作の概要について説明します。

- **コンテンツプロバイダー**の使用 &ndash;、連絡先リストにアクセスする例を示します。

- **ContentProvider を使用してデータを共有**し、同じアプリケーションで `ContentProvider` を作成して使用 &ndash; ます。

`ContentProviders` と、そのデータを操作するカーソルは、ListViews を設定するためによく使用されます。 これらのクラスの使用方法の詳細については、「 [Listviews And Adapters」ガイド](~/android/user-interface/layouts/list-view/index.md)を参照してください。

Android (または他のアプリケーション) によって公開される `ContentProviders` は、アプリケーションの他のソースからのデータを簡単に含めることができます。 これらの機能を使用すると、アプリケーション内から連絡先リスト、写真、カレンダーイベントなどのデータにアクセスして表示し、ユーザーがそのデータを操作できるようになります。

カスタム `ContentProviders` は、独自のアプリ内で使用するデータをパッケージ化したり、他のアプリケーション (カスタム検索やコピー/貼り付けなどの特別な用途を含む) で使用したりするための便利な方法です。

このセクションのトピックでは、`ContentProvider` コードの使用と書き込みの簡単な例を示します。

## <a name="related-links"></a>関連リンク

- [ContactsAdapter Demo (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-contactsadapterdemo)
- [SimpleContentProvider (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-simplecontentprovider)
- [コンテンツプロバイダー開発者ガイド](https://developer.android.com/guide/topics/providers/content-providers.html)
- [ContentProvider クラスのリファレンス](xref:Android.Content.ContentProvider)
- [ContentResolver クラスのリファレンス](xref:Android.Content.ContentResolver)
- [ListView クラスの参照](xref:Android.Widget.ListView)
- [カーソルクラス参照のカーソル](xref:Android.Widget.CursorAdapter)
- [UriMatcher クラスの参照](xref:Android.Content.UriMatcher)
- [Android. プロバイダー](xref:Android.Provider)
- [ContactsContract クラスの参照](xref:Android.Provider.ContactsContract)
