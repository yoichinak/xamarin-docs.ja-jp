---
title: ContentProviders の概要
description: Android オペレーティング システムでは、コンテンツ プロバイダーを使用して、メディア ファイル、連絡先、予定表情報などの共有データへのアクセスを容易にします。 この記事では、ContentProvider クラスについて説明し、その使用方法の例を 2 つ示します。
ms.prod: xamarin
ms.assetid: 6E1810AA-EB70-9AD0-1B32-D9418908CC97
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 496e5c092c79f4f71bddaad30bea6acd1d58d375
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027541"
---
# <a name="intro-to-contentproviders"></a>ContentProviders の概要

"_Android オペレーティング システムでは、コンテンツ プロバイダーを使用して、メディア ファイル、連絡先、予定表情報などの共有データへのアクセスを容易にします。この記事では、ContentProvider クラスについて説明し、その使用方法の例を 2 つ示します。_ "

## <a name="content-providers-overview"></a>コンテンツ プロバイダーの概要

*ContentProvider* は、データ リポジトリをカプセル化し、そこにアクセスするための API を提供します。 プロバイダーは、Android アプリケーションの一部として存在します。これは通常、データを表示または管理するための UI も提供します。 コンテンツ プロバイダーを使用する主な利点は、プロバイダー クライアント オブジェクト (*ContentResolver*) を使用して、カプセル化されたデータに他のアプリケーションが簡単にアクセスできることです。 また、コンテンツ プロバイダーおよびコンテンツ リゾルバーでは、簡単にビルドして使用できるデータ アクセスのための一貫したアプリケーション間 API も提供します。 アプリケーションでは、`ContentProviders` を使用して内部でデータを管理したり、他のアプリケーションに公開したりすることもできます。

また、`ContentProvider` は、アプリケーションでカスタムの検索候補を提供するため、または、複雑なデータをアプリケーションからコピーして他のアプリケーションに貼り付ける機能を提供する場合にも必要です。 このドキュメントでは、Xamarin Android で `ContentProviders` にアクセスしてビルドする方法について示します。

このセクションの構成は次のとおりです。

- **しくみ** &ndash; `ContentProvider` の設計目的と動作方法の概要。

- **コンテンツ プロバイダーの使用** &ndash; 連絡先リストにアクセスする例。

- **ContentProvider を使用してデータを共有** &ndash; 同じアプリケーションでの `ContentProvider` の作成と使用。

`ContentProviders` およびデータを操作するカーソルは、ListViews を作成するために使用されることがよくあります。 これらのクラスの詳細情報と使用方法については、[ListViews と Adapters のガイド](~/android/user-interface/layouts/list-view/index.md)に関するページを参照してください。

Android (または他のアプリケーション) によって公開されている `ContentProviders` では、アプリケーションの他のソースからのデータを簡単に含めることができます。 これらを使用すると、アプリケーション内から連絡先リスト、写真、またはカレンダー イベントなどのデータにアクセスして表示し、ユーザーがそのデータを操作できるようになります。

カスタムの `ContentProviders` は、独自のアプリ内で使用したり、他のアプリケーションで使用したりする (カスタム検索やコピーまたは貼り付けなどの特別な用途を含む) ために、データをパッケージ化するのに便利な方法です。

このセクションのトピックでは、`ContentProvider` コードの使用と作成のシンプルな例をいくつか示します。

## <a name="related-links"></a>関連リンク

- [ContactsAdapter デモ (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-contactsadapterdemo)
- [SimpleContentProvider (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-simplecontentprovider)
- [コンテンツ プロバイダー開発者ガイド](https://developer.android.com/guide/topics/providers/content-providers.html)
- [ContentProvider クラスのリファレンス](xref:Android.Content.ContentProvider)
- [ContentResolver クラスのリファレンス](xref:Android.Content.ContentResolver)
- [ListView クラスのリファレンス](xref:Android.Widget.ListView)
- [CursorAdapter クラスのリファレンス](xref:Android.Widget.CursorAdapter)
- [UriMatcher クラスのリファレンス](xref:Android.Content.UriMatcher)
- [Android.Provider](xref:Android.Provider)
- [ContactsContract クラスのリファレンス](xref:Android.Provider.ContactsContract)
