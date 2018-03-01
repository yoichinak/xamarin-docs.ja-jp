---
title: "コンテンツ プロバイダーの作業"
ms.topic: article
ms.prod: xamarin
ms.assetid: B9E2EF89-7EBE-45F5-1ED9-7D2C70BE792C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 142ef16606bbf47de073122791fa2509ed6b6353
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="how-content-providers-work"></a>コンテンツ プロバイダーの作業

2 つのクラスに含まれている、`ContentProvider`相互作用します。

- **ContentProvider** &ndash;標準的な方法でデータのセットを公開する API を実装します。 主な方法は、クエリ、挿入、更新および削除されます。

- **ContentResolver** &ndash;と通信するための静的プロキシ、`ContentProvider`または別のアプリケーションから同じアプリケーション内のいずれかから、そのデータにアクセスします。

コンテンツ プロバイダーは通常、SQLite データベースでバックアップが、API は、コードを実行する必要はありません、基になる SQL に関する知識がまったくを意味します。 (基になるデータ構造体への依存関係を減らす) に列名を参照する定数を使用する Uri を使用して行う、クエリと`ICursor`を繰り返し処理するコンシューマー コードが返されます。

<a name="Consuming_a_ContentProvider" />

## <a name="consuming-a-contentprovider"></a>ContentProvider の使用

`ContentProviders` 登録されている Uri を介して機能を公開、 **AndroidManifest.xml**のデータを発行するアプリケーション。 規則、Uri とは、公開されるデータ列される必要のあるデータにバインドするが簡単に定数として使用できます。 Android の組み込み`ContentProviders`内のデータ構造を参照する定数を使用して利便性のためのクラスを提供すべて、 [ `Android.Providers` ](https://developer.xamarin.com/api/namespace/Android.Provider/)名前空間。


<a name="Built-In_Providers" />

### <a name="built-in-providers"></a>組み込みプロバイダー

Android のシステムとユーザー データを使用して広範なへのアクセスを提供する`ContentProviders`:

- *ブラウザー* &ndash;ブックマークとブラウザーの履歴 (権限が必要です`READ_HISTORY_BOOKMARKS`や`WRITE_HISTORY_BOOKMARKS`)。

- *CallLog* &ndash;最近使用した呼び出しが行われたまたはデバイスを受信しました。

- *連絡先*&ndash;人、携帯電話、写真、およびグループを含む、ユーザーの連絡先リストからの情報を詳しく説明します。

- *MediaStore* &ndash;ユーザーのデバイスの内容: (アルバム、アーティスト、ジャンル、再生リスト) オーディオ、画像 (縮小表示を含む)、およびビデオ。

- *設定*&ndash;システム全体のデバイスの設定と設定します。

- *UserDictionary* &ndash;予測テキスト入力に使用されるユーザー定義のディクショナリのコンテンツ。

- *ボイスメール*&ndash;ボイス メール メッセージの履歴。


<a name="Classes_Overview" />

## <a name="classes-overview"></a>クラスの概要

使用する場合に使用される主要なクラス、`ContentProvider`を挙げています。

[![コンテンツ プロバイダーのアプリケーションとがかかりますアプリケーションの相互作用のクラス図](how-it-works-images/classdiagram1.png)](how-it-works-images/classdiagram1.png)

この図で、`ContentProvider`クエリを実装し、その他のアプリケーション データを検索に使用する URI を登録します。 `ContentResolver`を 'proxy' として機能、 `ContentProvider` (クエリ、挿入、更新、およびメソッドを削除) します。 `SQLiteOpenHelper`によって使用されるデータが含まれています、`ContentProvider`は、アプリを使用する直接公開されていません。
`CursorAdapter`によって返されるカーソルを渡す、`ContentResolver`に表示する、`ListView`です。 `UriMatcher`クエリを処理するときに、Uri を解析するヘルパー クラスです。

各クラスの目的は、次のとおりです。

- **ContentProvider** &ndash;データを公開するこの抽象クラスのメソッドを実装します。 API は他のクラスとクラス定義に追加される Uri 属性を使用してアプリケーションに提供されます。

- **SQLiteOpenHelper** &ndash;により実装によって公開される SQLite データストア、`ContentProvider`です。

- **UriMatcher** &ndash;使用`UriMatcher`で、`ContentProvider`コンテンツをクエリに使用される Uri を管理するために実装します。

- **ContentResolver** &ndash;使用コードを使用して、`ContentResolver`にアクセスする、`ContentProvider`インスタンス。 2 つのクラスは、一緒に注意してデータをアプリケーション間で簡単に共有できるように、プロセス間通信の問題の。 使用コードを作成しない、`ContentProvider`明示的; のクラスによって公開されている Uri に基づいてカーソルを作成することで、データにアクセスする代わりに、`ContentProvider`アプリケーションです。

- **CursorAdapter** &ndash;使用`CursorAdapter`または`SimpleCursorAdapter`経由でアクセスされるデータを表示する、`ContentProvider`です。

`ContentProvider` API により、コンシューマーなど、さまざまなデータに対して操作を実行します。

-  リストまたは個別のレコードを返すデータのクエリを実行します。
-  個々 のレコードを変更します。
-  新しいレコードを追加します。
-  レコードを削除します。

このドキュメントには、システム指定を使用する例が含まれています。 `ContentProvider`、単純な読み取り専用例、カスタムを実装するだけでなく`ContentProvider`です。

