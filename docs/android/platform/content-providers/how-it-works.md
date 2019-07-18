---
title: コンテンツ プロバイダーの作業
ms.prod: xamarin
ms.assetid: B9E2EF89-7EBE-45F5-1ED9-7D2C70BE792C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: df4c2e10e34c308e4fadb44fba9c6a14714ae1b9
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "60952864"
---
# <a name="how-content-providers-work"></a>コンテンツ プロバイダーの作業

2 つのクラス、`ContentProvider`相互作用します。

- **ContentProvider** &ndash;標準的な方法でデータのセットを公開する API を実装します。 主な方法は、クエリ、挿入、更新および削除します。

- **ContentResolver** &ndash;と通信するための静的プロキシ、`ContentProvider`または別のアプリケーションから、同じアプリケーション内のいずれかから、そのデータにアクセスします。

コンテンツ プロバイダーは通常、SQLite データベースでバックアップしますが、API では、基になる SQL について何も知るコードを実行しない必要がないことを意味します。 クエリが Uri を使用して行われます (を基になるデータ構造体への依存関係を減らすため)、列名を参照する定数を使用して、`ICursor`使用側コードを反復処理する場合に返されます。


## <a name="consuming-a-contentprovider"></a>ContentProvider の使用

`ContentProviders` 登録されている Uri を通じて機能を公開、 **AndroidManifest.xml**のデータを発行するアプリケーション。 Uri と公開されているデータ列ができる場所データにバインドしやすくための定数として、規則があります。 Android の組み込み`ContentProviders`利便性のためのクラス内のデータ構造を参照する定数を使用して提供、 [ `Android.Providers` ](https://developer.xamarin.com/api/namespace/Android.Provider/)名前空間。



### <a name="built-in-providers"></a>組み込みのプロバイダー

さまざまなシステムとを使用してユーザー データへのアクセスを提供する android `ContentProviders`:

- *ブラウザー* &ndash;ブックマークとブラウザーの履歴 (権限が必要です`READ_HISTORY_BOOKMARKS`や`WRITE_HISTORY_BOOKMARKS`)。

- *CallLog* &ndash;最近の呼び出しが行われたまたは、デバイスで受信しました。

- *連絡先*&ndash;人、携帯電話、写真、およびグループを含む、ユーザーの連絡先リストからの情報を詳しく説明します。

- *MediaStore* &ndash;ユーザーのデバイスの内容: オーディオ (アルバム、アーティスト、ジャンル、再生リスト)、(縮小表示を含む) の画像とビデオ。

- *設定*&ndash;システム全体のデバイスの設定と設定します。

- *UserDictionary* &ndash;予測テキスト入力に使用されるユーザー定義のディクショナリの内容。

- *ボイスメール*&ndash;ボイスメール メッセージの履歴。



## <a name="classes-overview"></a>クラスの概要

使用する場合に使用されるプライマリ クラス、`ContentProvider`を次に示します。

[![コンテンツ プロバイダーのアプリケーションとアプリケーションの相互作用がかかりますクラス ダイアグラム](how-it-works-images/classdiagram1.png)](how-it-works-images/classdiagram1.png#lightbox)

この図で、`ContentProvider`クエリを実装し、その他のアプリケーションのデータを検索に使用する URI を登録します。 `ContentResolver`を 'proxy' として機能、 `ContentProvider` (照会、挿入、更新、および Delete の各メソッド)。 `SQLiteOpenHelper`によって使用されるデータが含まれています、`ContentProvider`は、アプリを使用する直接公開されていません。
`CursorAdapter`によって返されるカーソルを渡す、`ContentResolver`に表示する、`ListView`します。 `UriMatcher`はクエリを処理するときに、Uri を解析するヘルパー クラスです。

各クラスの目的は、次に示します。

- **ContentProvider** &ndash;データを公開するこの抽象クラスのメソッドを実装します。 API は他のクラスとクラス定義に追加される Uri 属性を使用してアプリケーションに提供されます。

- **SQLiteOpenHelper** &ndash;により実装によって公開される SQLite データストア、`ContentProvider`します。

- **UriMatcher** &ndash;使用`UriMatcher`で、`ContentProvider`コンテンツをクエリに使用される Uri を管理するために実装します。

- **ContentResolver** &ndash;コードを使用して、`ContentResolver`にアクセスする、`ContentProvider`インスタンス。 まとめて 2 つのクラスをデータをアプリケーション間で簡単に共有できるように、プロセス間通信の問題の処理します。 作成し、コードを実行しません、`ContentProvider`明示的; クラスによって公開されている Uri に基づいてカーソルを作成して、データにアクセスする代わりに、`ContentProvider`アプリケーション。

- **CursorAdapter** &ndash;使用`CursorAdapter`または`SimpleCursorAdapter`経由でアクセスされるデータを表示する、`ContentProvider`します。

`ContentProvider` API などのさまざまなデータの操作を実行するコンシューマーを使用できます。

-  リストまたは個別のレコードを返すデータのクエリを実行します。
-  個々 のレコードを変更します。
-  新しいレコードを追加します。
-  レコードを削除します。

このドキュメントには、システム指定の使用例が含まれています。 `ContentProvider`、カスタムを実装する単純な読み取り専用例と`ContentProvider`します。

