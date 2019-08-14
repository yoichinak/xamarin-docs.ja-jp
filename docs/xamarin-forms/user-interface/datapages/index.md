---
title: DataPages
description: この記事では、データソースを事前構築されたビューにすばやく簡単にバインドするための API を提供する DataPages について説明します。
ms.prod: xamarin
ms.assetid: DF16EAEE-DB78-42CA-9C59-51D9D6CB6B95
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 9dc34f412549c4be6099b373ddae7cbd2e8d21c8
ms.sourcegitcommit: 41a029c69925e3a9d2de883751ebfd649e8747cd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/13/2019
ms.locfileid: "68980770"
---
# <a name="xamarinforms-datapages"></a>DataPages

![](~/media/shared/preview.png "この API は現在プレビュー段階")

> [!IMPORTANT]
> DataPages を表示するには、Xamarin. Forms Theme リファレンスが必要です。 これには、プロジェクトに[xamarin. theme. Base](https://www.nuget.org/packages/Xamarin.Forms.Theme.Base/) nuget パッケージをインストールし、その後に、 [xamarin. theme](https://www.nuget.org/packages/Xamarin.Forms.Theme.Light/)パッケージまたは[xamarin](https://www.nuget.org/packages/Xamarin.Forms.Theme.Dark/) . theme. theme パッケージのいずれかをインストールする必要があります。

Xamarin DataPages は2016進化に発表され、お客様がフィードバックをお寄せいただくためのプレビューとしてご利用いただけるようになりました。

DataPages の迅速かつ簡単に構築済みのビューにデータ ソースにバインドする API を提供します。 リスト項目と詳細ページは、データを自動的に表示し、テーマを使用してカスタマイズできます。

基調講演のデモのしくみを確認するには、[ファーストステップガイド](get-started.md)を参照してください。

[![](images/demo-sml.png "DataPages サンプル アプリケーション")](images/demo.png#lightbox "DataPages サンプル アプリケーション")

## <a name="introduction"></a>概要

データソースと関連付けられたデータページを使用すると、開発者はサポートされているデータソースをすばやく簡単に使用し、テーマでカスタマイズできる組み込みの UI スキャフォールディングを使用してレンダリングできます。

DataPages Nuget パッケージをインクルードすることにより、Xamarin. forms アプリケーションに追加されます。

### <a name="data-sources"></a>Data Sources

プレビューでは、いくつかの事前に構築されたデータソースを使用できます。

* **JsonDataSource**
* **Azuredatasource**(個別の Nuget)
* **AzureEasyTableDataSource**(個別の Nuget)

の使用`JsonDataSource`例については、[ファーストステップガイド](get-started.md)を参照してください。


### <a name="pages--controls"></a>ページ & コントロール

指定されたデータソースに簡単にバインドできるように、次のページとコントロールが含まれています。

* **ListDataPage** –作業の[開始の例](get-started.md)を参照してください。
* **DirectoryPage** –グループ化が有効になっているリスト。
* [**個人情報] ページ**-特定のオブジェクトの種類 (連絡先エントリ) 用にカスタマイズされた単一のデータ項目ビュー。
* **DataView** –汎用的な方法でソースからデータを公開するためのビュー。
* **CardView** –イメージ、タイトルテキスト、および説明テキストを含むスタイル付きビュー。
* **HeroImage** –イメージレンダリングビューです。
* **ListItem** : ネイティブの IOS および Android のリスト項目と同様のレイアウトを備えた、あらかじめ構築されたビューです。

例については、「 [DataPages controls reference](controls.md) 」を参照してください。



### <a name="under-the-hood"></a>内部

Xamarin データソースは、インターフェイスに準拠し`IDataSource`ています。

Xamarin. Forms インフラストラクチャは、次のプロパティを使用してデータソースと対話します。

* `Data`–表示できるデータ項目の読み取り専用の一覧。
* `IsLoading`–データが読み込まれ、表示に使用できるかどうかを示すブール値。
* `[key]`–要素を取得するためのインデクサー。

データ項目のプロパティ`MaskKey`を`UnmaskKey`非表示 (または表示) するには、との2つのメソッドを使用できます。 表示されないようにします)。
キーは、データ項目オブジェクトの名前付きプロパティに対応します。
