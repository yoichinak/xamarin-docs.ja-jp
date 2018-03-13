---
title: DataPages
ms.topic: article
ms.prod: xamarin
ms.assetid: DF16EAEE-DB78-42CA-9C59-51D9D6CB6B95
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 60973068b56ea4160c3e5ae53d387b063c601afe
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="datapages"></a>DataPages

![](~/media/shared/preview.png "この API は現在プレビュー中")

> [!IMPORTANT]
> DataPages が必要です、 [Xamarin.Forms テーマ](~/xamarin-forms/user-interface/themes/index.md)レンダリングへの参照。

Xamarin.Forms DataPages する 2016年で発表されたおよびフィードバックを提供する顧客用のプレビューとして利用できます。

DataPages は、迅速かつ簡単に構築済みのビューにデータ ソースにバインドするために API を提供します。 リスト項目および詳細ページ、データを自動的にレンダリングし、テーマを使用してカスタマイズすることができます。

チェック アウトする keynote デモのしくみを表示する、[ファースト ステップ ガイド](get-started.md)です。

[![](images/demo-sml.png "DataPages サンプル アプリケーション")](images/demo.png#lightbox "DataPages サンプル アプリケーション")

## <a name="introduction"></a>はじめに

データ ソースと関連付けられているデータ ページには、迅速かつ簡単にサポートされているデータ ソースを使用して、スキャフォールディング UI をカスタマイズする組み込みを使用してテーマを使用してを描画する開発者ができるようにします。

DataPages はなどにより Xamarin.Forms アプリケーションに追加される、 **Xamarin.Forms.Pages** Nuget パッケージです。

### <a name="data-sources"></a>Data Sources

プレビューでは、使用可能な一部のビルド済みのデータ ソースがあります。

* **JsonDataSource**
* **AzureDataSource** (Nuget の分離)
* **AzureEasyTableDataSource** (Nuget の分離)

参照してください、[ファースト ステップ ガイド](get-started.md)使用例については、`JsonDataSource`です。


### <a name="pages--controls"></a>ページとコントロール

次のページおよびコントロールは、提供されたデータ ソースを簡単にバインドできるように説明します。

* **ListDataPage** – を参照してください、[作業の開始の例](get-started.md)です。
* **DirectoryPage** – 有効になっているグループ化されたリストです。
* **PersonDetailPage** – 1 つのデータ項目のビューの特定のオブジェクトの種類 (連絡先) をカスタマイズします。
* **DataView** –、汎用的なやり方でソースからデータを公開するビュー。
* **CardView** –、画像、タイトルのテキスト、および説明のテキストを含むビューのスタイルを設定します。
* **HeroImage** – image 表示ビュー。
* **ListItem** – 既成のネイティブの iOS および Android のリスト項目のようなレイアウトで表示します。

参照してください、 [DataPages コントロールのリファレンス](controls.md)例についてはします。



### <a name="under-the-hood"></a>実際には

準拠して Xamarin.Forms データ ソース、`IDataSource`インターフェイスです。

Xamarin.Forms インフラストラクチャは、以下のプロパティを通じてデータ ソースと対話します。

* `Data` – 表示できるデータ項目の読み取り専用のリスト。
* `IsLoading` – データが読み込まれ、レンダリングに使用できるかどうかを示すブール値。
* `[key]` – 要素を取得するためのインデクサーです。

2 つのメソッドがある`MaskKey`と`UnmaskKey`(非表示に) データ項目のプロパティ (ie 使用できます。 表示されないように) します。
対応する、キー、名前付きオブジェクトのプロパティをデータ項目。

