---
title: Xamarin.Forms DataPages
description: この記事では、簡単にするための API を提供し、簡単に構築済みのビューにデータ ソースをバインド Xamarin.Forms DataPages について説明します。
ms.prod: xamarin
ms.assetid: DF16EAEE-DB78-42CA-9C59-51D9D6CB6B95
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 2a74b636a41a72b26776157a774f0a33ef45a075
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61407676"
---
# <a name="xamarinforms-datapages"></a>Xamarin.Forms DataPages

![](~/media/shared/preview.png "この API は現在プレビュー段階")

> [!IMPORTANT]
> DataPages が必要です、 [Xamarin.Forms テーマ](~/xamarin-forms/user-interface/themes/index.md)レンダリングへの参照。

Xamarin.Forms DataPages 進化 2016年で発表されたおよびフィードバックを提供するお客様向けのプレビューとして利用できます。

DataPages の迅速かつ簡単に構築済みのビューにデータ ソースにバインドする API を提供します。 リスト項目および詳細ページは、データを自動的にレンダリングし、テーマを使用してカスタマイズできます。

進化の keynote デモの動作を確認するチェック アウト、[ファースト ステップ ガイド](get-started.md)します。

[![](images/demo-sml.png "DataPages サンプル アプリケーション")](images/demo.png#lightbox "DataPages サンプル アプリケーション")

## <a name="introduction"></a>はじめに

データ ソースと関連付けられているデータ ページは、開発者はすばやく簡単にサポートされているデータ ソースを使用してテーマを使用して組み込みスキャフォールディング UI をカスタマイズすることができますを使用してレンダリングを許可します。

含めることで、Xamarin.Forms アプリケーションに追加 DataPages、 **Xamarin.Forms.Pages** Nuget パッケージ。

### <a name="data-sources"></a>Data Sources

プレビューには、使用可能な一部の事前構築済みのデータ ソースがあります。

* **JsonDataSource**
* **AzureDataSource** (Nuget の分離)
* **AzureEasyTableDataSource** (Nuget の分離)

参照してください、[ファースト ステップ ガイド](get-started.md)使用例について、`JsonDataSource`します。


### <a name="pages--controls"></a>ページとコントロール

次のページおよびコントロールは、提供されたデータ ソースに簡単にバインドできるようにするためには。

* **ListDataPage** – を参照してください、[作業の開始例](get-started.md)します。
* **DirectoryPage** – 有効になっているグループ化一覧。
* **PersonDetailPage** – 1 つのデータ項目の特定のオブジェクト型 (連絡先) 向けにカスタマイズされたビュー。
* **DataView** – 一般的な方法でソースからデータを公開するためのビュー。
* **CardView** – イメージ、タイトルのテキストと説明テキストを含むビューのスタイルを設定します。
* **HeroImage** – 画像レンダリング、表示します。
* **ListItem** –、既成のネイティブ iOS と Android のリスト アイテムのようなレイアウトで表示します。

参照してください、 [DataPages コントロールのリファレンス](controls.md)例についてはします。



### <a name="under-the-hood"></a>内部的には

Xamarin.Forms のデータ ソースに準拠する、`IDataSource`インターフェイス。

Xamarin.Forms インフラストラクチャ、次のプロパティによってデータ ソースと対話します。

* `Data` – 表示できるデータ項目の読み取り専用のリスト。
* `IsLoading` – データが読み込まれ、レンダリングに使用できるかどうかを示すブール値。
* `[key]` – 要素を取得するためのインデクサー。

2 つのメソッドがある`MaskKey`と`UnmaskKey`(非表示にする) データ項目のプロパティ (使用できます。 表示されないように) します。
対応する、キー、データ項目オブジェクトの名前付きプロパティ。
