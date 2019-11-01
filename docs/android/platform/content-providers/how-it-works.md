---
title: コンテンツプロバイダーのしくみ
ms.prod: xamarin
ms.assetid: B9E2EF89-7EBE-45F5-1ED9-7D2C70BE792C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: e61be6f0189eb825c15fd75764a16706e588ebc9
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020516"
---
# <a name="how-content-providers-work"></a>コンテンツプロバイダーのしくみ

`ContentProvider` の相互作用には、次の2つのクラスが関係します。

- **Contentprovider** &ndash; は、一連のデータを標準的な方法で公開する API を実装します。 主な方法は、クエリ、挿入、更新、および削除です。

- **Contentresolver** &ndash;、同じアプリケーション内または別のアプリケーションからデータにアクセスするために、`ContentProvider` と通信する静的プロキシです。

通常、コンテンツプロバイダーは SQLite データベースによってサポートされますが、API は、基になる SQL について、コードを使用する必要がないことを意味します。 クエリは、列名を参照するために定数を使用する Uri (基になるデータ構造に対する依存関係を減らすため) を介して実行され、処理コードが反復処理するために `ICursor` が返されます。

## <a name="consuming-a-contentprovider"></a>ContentProvider の使用

`ContentProviders` は、データを発行するアプリケーションの**Androidmanifest .xml**に登録されている Uri を通じて機能を公開します。 データに簡単にバインドできるように、公開されている Uri とデータ列を定数として使用できるようにするための規則があります。 Android の組み込み `ContentProviders` には、 [`Android.Providers`](xref:Android.Provider)名前空間のデータ構造を参照する定数を使用した便利なクラスが用意されています。

### <a name="built-in-providers"></a>組み込みプロバイダー

Android では、`ContentProviders`を使用して、さまざまなシステムデータとユーザーデータにアクセスできます。

- *ブラウザー* &ndash; ブックマークとブラウザーの履歴 (アクセス許可 `READ_HISTORY_BOOKMARKS` と `WRITE_HISTORY_BOOKMARKS`) が必要です。

- *Calllog* &ndash; 最近の呼び出しがデバイスで行われたか、受信されました。

- *連絡先*には、ユーザーの連絡先リストから、ユーザー、電話、写真 & グループなどの詳細情報 &ndash; ます。

- *Mediastore*では、ユーザーのデバイスのコンテンツ &ndash; ます。オーディオ (アルバム、アーティスト、ジャンル、再生リスト)、画像 (サムネイルを含む) はビデオ & ます。

- *設定*&ndash;、システム全体のデバイス設定と設定を行います。

- *Userdictionary*は、予測テキスト入力に使用されるユーザー定義の辞書の内容を &ndash; します。

- *ボイスメール &ndash; ボイス*メールメッセージの履歴。

## <a name="classes-overview"></a>クラスの概要

`ContentProvider` を操作するときに使用される主なクラスを次に示します。

[コンテンツプロバイダーアプリケーションとアプリケーションの相互作用を使用する![クラス図](how-it-works-images/classdiagram1.png)](how-it-works-images/classdiagram1.png#lightbox)

この図では、`ContentProvider` によってクエリが実装され、他のアプリケーションがデータを検索するために使用する URI が登録されます。 `ContentResolver` は、`ContentProvider` (クエリ、挿入、更新、および削除の各メソッド) の ' プロキシ ' として機能します。 `SQLiteOpenHelper` には、`ContentProvider`によって使用されるデータが含まれていますが、アプリの消費に直接公開されることはありません。
`CursorAdapter` は、`ContentResolver` によって返されたカーソルを `ListView`に表示します。 `UriMatcher` は、クエリの処理時に Uri を解析するヘルパークラスです。

各クラスの目的は次のとおりです。

- **Contentprovider** &ndash; この抽象クラスのメソッドを実装してデータを公開します。 API は、クラス定義に追加された Uri 属性を使用して、他のクラスおよびアプリケーションで使用できるようになります。

- **SQLiteOpenHelper** &ndash; は、`ContentProvider`によって公開される SQLite データストアを実装するのに役立ちます。

- **UriMatcher** &ndash;、`ContentProvider` の実装で `UriMatcher` を使用して、コンテンツのクエリに使用される uri を管理するのに役立ちます。

- **Contentresolver** &ndash; コードを使用する場合は、`ContentResolver` を使用して `ContentProvider` インスタンスにアクセスします。 2つのクラスは、プロセス間の通信の問題を処理し、アプリケーション間でデータを簡単に共有できるようにします。 コードを使用しても `ContentProvider` クラス明示的が作成されることはありません。代わりに、`ContentProvider` アプリケーションによって公開された Uri に基づいてカーソルを作成することによって、データにアクセスします。

- **カーソル**&ndash; `CursorAdapter` または `SimpleCursorAdapter` を使用して、`ContentProvider`経由でアクセスされるデータを表示します。

`ContentProvider` API を使用すると、コンシューマーは次のようなデータに対してさまざまな操作を実行できます。

- データをクエリして、リストまたは個々のレコードを返します。
- 個々のレコードを変更します。
- 新しいレコードを追加します。
- レコードを削除します。

このドキュメントには、システムによって提供される `ContentProvider`を使用する例と、カスタム `ContentProvider`を実装する単純な読み取り専用の例が含まれています。
