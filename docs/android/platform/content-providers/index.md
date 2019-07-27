---
title: ContentProviders の概要
description: Android オペレーティングシステムでは、コンテンツプロバイダーを使用して、メディアファイル、連絡先、予定表情報などの共有データへのアクセスを容易にします。 この記事では、ContentProvider クラスについて説明し、その使用方法の2つの例を示します。
ms.prod: xamarin
ms.assetid: 6E1810AA-EB70-9AD0-1B32-D9418908CC97
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 1e62dc32e9764667cb8737167a49bcc9a4516f0f
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510633"
---
# <a name="intro-to-contentproviders"></a>ContentProviders の概要

_Android オペレーティングシステムでは、コンテンツプロバイダーを使用して、メディアファイル、連絡先、予定表情報などの共有データへのアクセスを容易にします。この記事では、ContentProvider クラスについて説明し、その使用方法の2つの例を示します。_


## <a name="content-providers-overview"></a>コンテンツプロバイダーの概要

*Contentprovider*は、データリポジトリをカプセル化し、それにアクセスするための API を提供します。 プロバイダーは、通常、データを表示または管理するための UI も提供する Android アプリケーションの一部として存在します。 コンテンツプロバイダーを使用する主な利点は、他のアプリケーションがプロバイダークライアントオブジェクト ( *Contentresolver*と呼ばれます) を使用して、カプセル化されたデータに簡単にアクセスできるようにすることです。 また、コンテンツプロバイダーとコンテンツリゾルバーは、簡単に構築して使用できるデータアクセスのための一貫したアプリケーション間 API を提供します。 アプリケーションでは、を使用`ContentProviders`してデータを内部で管理したり、他のアプリケーションに公開したりすることもできます。

また`ContentProvider` 、は、アプリケーションがカスタムの検索候補を提供するために必要な場合や、アプリケーションから複雑なデータをコピーして他のアプリケーションに貼り付けることができるようにする場合にも必要です。 このドキュメントでは、Xamarin Android で`ContentProviders`にアクセスしてビルドする方法について説明します。

このセクションの構造は次のとおりです。

- **しくみ**の設計と動作の`ContentProvider`概要について説明します。 &ndash;

- **コンテンツプロバイダーの**使用&ndash;連絡先リストにアクセスする例。

- **ContentProvider を使用してデータを共有する**同じアプリケーションでの`ContentProvider`の書き込みと使用。 &ndash;

`ContentProviders`また、データを操作するカーソルは、ListViews を設定するために使用されることがよくあります。 これらのクラスの使用方法の詳細については、「 [Listviews And Adapters」ガイド](~/android/user-interface/layouts/list-view/index.md)を参照してください。

`ContentProviders`Android (または他のアプリケーション) によって公開されているので、アプリケーションの他のソースからのデータを簡単に含めることができます。 これらの機能を使用すると、アプリケーション内から連絡先リスト、写真、カレンダーイベントなどのデータにアクセスして表示し、ユーザーがそのデータを操作できるようになります。

カスタム`ContentProviders`は、独自のアプリ内で使用するデータをパッケージ化したり、他のアプリケーション (カスタム検索、コピー/貼り付けなどの特殊な用途を含む) で使用したりするための便利な方法です。

このセクションのトピックでは、コードの使用と記述`ContentProvider`の簡単な例をいくつか紹介します。



## <a name="related-links"></a>関連リンク

- [ContactsAdapter Demo (サンプル)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ContactsAdapterDemo/)
- [SimpleContentProvider (サンプル)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SimpleContentProvider)
- [コンテンツプロバイダー開発者ガイド](https://developer.android.com/guide/topics/providers/content-providers.html)
- [ContentProvider クラスのリファレンス](xref:Android.Content.ContentProvider)
- [ContentResolver クラスのリファレンス](xref:Android.Content.ContentResolver)
- [ListView クラスの参照](xref:Android.Widget.ListView)
- [カーソルクラス参照のカーソル](xref:Android.Widget.CursorAdapter)
- [UriMatcher クラスの参照](xref:Android.Content.UriMatcher)
- [Android.Provider](xref:Android.Provider)
- [ContactsContract クラスの参照](xref:Android.Provider.ContactsContract)
