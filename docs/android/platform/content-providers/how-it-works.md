---
title: コンテンツプロバイダーのしくみ
ms.prod: xamarin
ms.assetid: B9E2EF89-7EBE-45F5-1ED9-7D2C70BE792C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 64a12f4f797630ad37e5821cd04a14a9d561c53e
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510679"
---
# <a name="how-content-providers-work"></a>コンテンツプロバイダーのしくみ

相互作用には、次の`ContentProvider` 2 つのクラスが関係します。

- **Contentprovider**&ndash;データのセットを標準の方法で公開する API を実装します。 主な方法は、クエリ、挿入、更新、および削除です。

- **Contentresolver**と通信して、同じアプリケーション内または別のアプリケーションからデータにアクセスする静的プロキシ。`ContentProvider` &ndash;

通常、コンテンツプロバイダーは SQLite データベースによってサポートされますが、API は、基になる SQL について、コードを使用する必要がないことを意味します。 クエリは、列名を参照するために定数を使用する Uri (基になるデータ構造に対する依存関係`ICursor`を減らすため) を介して実行されます。また、を使用するコードを反復処理するためにが返されます。


## <a name="consuming-a-contentprovider"></a>ContentProvider の使用

`ContentProviders`データを発行するアプリケーションの**Androidmanifest .xml**に登録されている Uri を使用して機能を公開します。 データに簡単にバインドできるように、公開されている Uri とデータ列を定数として使用できるようにするための規則があります。 Android の組み込みのすべて`ContentProviders`に、 [`Android.Providers`](xref:Android.Provider)名前空間のデータ構造を参照する定数を持つ便利なクラスが用意されています。



### <a name="built-in-providers"></a>組み込みプロバイダー

Android では、次のものを使用して`ContentProviders`広範なシステムおよびユーザーデータにアクセスできます。

- *ブラウザー*ブックマークとブラウザーの履歴 (アクセス`READ_HISTORY_BOOKMARKS`許可と/ `WRITE_HISTORY_BOOKMARKS`またはが必要です)。 &ndash;

- *Calllog*&ndash;デバイスで行われた最近の呼び出し。

- *連絡先*&ndash;ユーザーの連絡先リストに含まれる詳細情報 (ユーザー、電話、写真 & グループなど)。

- *Mediastore*&ndash;ユーザーのデバイスのコンテンツ: オーディオ (アルバム、アーティスト、ジャンル、再生リスト)、画像 (サムネイルを含む) & ビデオ。

- *設定*&ndash;システム全体のデバイス設定と設定。

- *Userdictionary*&ndash;予測テキスト入力に使用されるユーザー定義の辞書の内容。

- *ボイスメール*&ndash;ボイスメールメッセージの履歴。



## <a name="classes-overview"></a>クラスの概要

を`ContentProvider`操作するときに使用される主なクラスを次に示します。

[![コンテンツプロバイダーアプリケーションとアプリケーションの対話処理のクラス図](how-it-works-images/classdiagram1.png)](how-it-works-images/classdiagram1.png#lightbox)

この図では、 `ContentProvider`はクエリを実装し、他のアプリケーションがデータを検索するために使用する URI を登録します。 は`ContentResolver` 、`ContentProvider`に対する "プロキシ" として機能します (クエリ、挿入、更新、および削除の各メソッド)。 には、 `ContentProvider`によって使用されるデータが含まれていますが、アプリの消費に直接公開されることはありません。`SQLiteOpenHelper`
は、に`ContentResolver`よって返されたカーソルをに`ListView` `CursorAdapter`渡して、に表示します。 は`UriMatcher` 、クエリの処理時に uri を解析するヘルパークラスです。

各クラスの目的は次のとおりです。

- **Contentprovider**&ndash;この抽象クラスのメソッドを実装して、データを公開します。 API は、クラス定義に追加された Uri 属性を使用して、他のクラスおよびアプリケーションで使用できるようになります。

- **SQLiteOpenHelper**によって公開される SQLite データストアを実装するのに役立ちます。`ContentProvider` &ndash;

- **UriMatcher**コンテンツのクエリ`ContentProvider`に使用される uri を管理するために、の実装でを使用`UriMatcher`します。 &ndash;

- **Contentresolver**コードを使用する`ContentResolver`には、 `ContentProvider`を使用してインスタンスにアクセスします。 &ndash; 2つのクラスは、プロセス間の通信の問題を処理し、アプリケーション間でデータを簡単に共有できるようにします。 コードを使用する場合`ContentProvider`は、明示的クラスを作成するのではなく、 `ContentProvider`アプリケーションによって公開された Uri に基づいてカーソルを作成することによって、データにアクセスします。

- **カーソルアダプター**またはを`SimpleCursorAdapter`使用`CursorAdapter`して、 `ContentProvider`でアクセスされるデータを表示します。 &ndash;

API `ContentProvider`を使用すると、コンシューマーは次のようなデータに対してさまざまな操作を実行できます。

-  データをクエリして、リストまたは個々のレコードを返します。
-  個々のレコードを変更します。
-  新しいレコードを追加します。
-  レコードを削除します。

このドキュメントには、システム提供`ContentProvider`のを使用する例と、カスタム`ContentProvider`を実装する単純な読み取り専用の例が含まれています。

