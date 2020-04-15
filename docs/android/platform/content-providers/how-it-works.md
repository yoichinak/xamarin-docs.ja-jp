---
title: コンテンツ プロバイダーのしくみ
ms.prod: xamarin
ms.assetid: B9E2EF89-7EBE-45F5-1ED9-7D2C70BE792C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: e61be6f0189eb825c15fd75764a16706e588ebc9
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "73020516"
---
# <a name="how-content-providers-work"></a>コンテンツ プロバイダーのしくみ

`ContentProvider` の対話処理には、次の 2 つのクラスが関係します。

- **ContentProvider** &ndash; 一連のデータを標準的な方法で公開する API を実装します。 クエリ、挿入、更新、および削除のメイン メソッドです。

- **ContentResolver** &ndash; 同じアプリケーション内または別のアプリケーションからデータにアクセスするために `ContentProvider` と通信する静的プロキシです。

通常、コンテンツ プロバイダーは SQLite データベースによってサポートされますが、この API は、使用側コードでは基になる SQL について何も知る必要がないことを意味しています。 クエリでは、定数を使用する URI 経由で列名が参照され (基になるデータ構造に対する依存関係を減らすため)、使用側コードで反復処理するために `ICursor` が返されます。

## <a name="consuming-a-contentprovider"></a>ContentProvider の使用

`ContentProviders` の機能は、データを発行するアプリケーションの **AndroidManifest.xml** に登録されている URI を通して公開されます。 データに簡単にバインドできるように、公開される URI とデータ列を定数として使用できるようにするための規則があります。 Android に組み込まれている `ContentProviders` には、そのすべてに、[`Android.Providers`](xref:Android.Provider) 名前空間でデータ構造を参照する定数を使用する便利なクラスが提供されています。

### <a name="built-in-providers"></a>組み込みプロバイダー

Android では、`ContentProviders` を使用した幅広いシステム データとユーザー データへのアクセス方法が提供されています。

- "*ブラウザー*" &ndash; ブックマークとブラウザーの履歴 (アクセス許可 `READ_HISTORY_BOOKMARKS` または `WRITE_HISTORY_BOOKMARKS`、あるいはその両方が必要です)。

- "*通話記録*" &ndash; デバイスで行われた最近の発信または着信。

- "*連絡先*" &ndash; ユーザーの連絡先リストの詳細情報 (ユーザー、電話番号、写真、グループなど)。

- "*メディア ストア*" &ndash; ユーザーのデバイスの内容: オーディオ (アルバム、アーティスト、ジャンル、再生リスト)、画像 (サムネイルを含む)、および動画。

- "*設定*" &ndash; システム全体のデバイス設定とユーザー設定。

- "*ユーザー辞書*" &ndash; 予測テキスト入力に使用されるユーザー定義辞書の内容。

- "*ボイスメール*" &ndash; ボイスメール メッセージの履歴。

## <a name="classes-overview"></a>クラスの概要

`ContentProvider` を操作するときに使用される主なクラスを次に示します。

[![コンテンツ プロバイダー アプリケーションと使用側アプリケーションの対話処理のクラス ダイアグラム](how-it-works-images/classdiagram1.png)](how-it-works-images/classdiagram1.png#lightbox)

この図では、`ContentProvider` によってクエリが実装され、他のアプリケーションがデータを検索するために使用する URI が登録されます。 `ContentResolver` は、`ContentProvider` (クエリ、挿入、更新、および削除の各メソッド) に対する "プロキシ" として機能します。 `SQLiteOpenHelper` には `ContentProvider` によって使用されるデータが含まれますが、それが使用側アプリに直接公開されることはありません。
`CursorAdapter` は、`ListView` に表示するために `ContentResolver` によって返されたカーソルを渡します。 `UriMatcher` は、クエリの処理時に URI を解析するヘルパー クラスです。

各クラスの目的を以下で説明します。

- **ContentProvider** &ndash; この抽象クラスのメソッドを実装してデータを公開します。 クラス定義に追加された URI 属性経由で他のクラスとアプリケーションがこの API を使用できるようにします。

- **SQLiteOpenHelper** &ndash; `ContentProvider` によって公開された SQLite データストアの実装を支援します。

- **UriMatcher** &ndash; お使いの `ContentProvider` の実装の中で `UriMatcher` を使用して、コンテンツをクエリするために使用される URI の管理を支援します。

- **ContentResolver** &ndash; 使用側コードで `ContentResolver` を使用して、`ContentProvider` インスタンスにアクセスします。 この 2 つのクラスがまとまってプロセス間通信の問題が処理され、アプリケーション間でデータを簡単に共有できるようにします。 使用側コードによって `ContentProvider` クラスが明示的が作成されることはありません。代わりに、データへのアクセスは、`ContentProvider` アプリケーションによって公開された URI に基づいてカーソルを作成することで行われます。

- **CursorAdapter** &ndash; `CursorAdapter` または `SimpleCursorAdapter` を使用して、`ContentProvider` 経由でアクセスされたデータを表示します。

`ContentProvider` API によって、使用側は、データに対して次のようなさまざまな操作を実行できます。

- データをクエリして、リストまたは個々のレコードを返します。
- 個々のレコードを変更します。
- 新しいレコードを追加します。
- レコードを削除します。

このドキュメントには、システムによって提供される `ContentProvider` を使用する例と、カスタム `ContentProvider` を実装する単純な読み取り専用の例が含まれています。
