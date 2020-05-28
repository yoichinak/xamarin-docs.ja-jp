---
title: Xamarin.FormsDataPages
description: この記事では Xamarin.Forms 、データソースを事前構築されたビューにすばやく簡単にバインドするための API を提供する DataPages について説明します。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7d99870dd975d0996ffcd05d4aef153f3515ec9e
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84134317"
---
# <a name="xamarinforms-datapages"></a>Xamarin.FormsDataPages

![](~/media/shared/preview.png "This API is currently in preview")

> [!IMPORTANT]
> DataPages Xamarin.Forms を表示するには、テーマ参照が必要です。 これには、のインストールが含ま[ Xamarin.Forms れます。Theme。](https://www.nuget.org/packages/Xamarin.Forms.Theme.Base/)プロジェクトの基本となる NuGet パッケージの後に、[のいずれかが続きます。 Xamarin.FormsTheme](https://www.nuget.org/packages/Xamarin.Forms.Theme.Light/)または[ Xamarin.Forms 。Theme. ダーク](https://www.nuget.org/packages/Xamarin.Forms.Theme.Dark/)NuGet パッケージ。

Xamarin.FormsDataPages は2016進化に発表され、お客様がフィードバックをお寄せいただくためのプレビューとしてご利用いただけます。

DataPages は、事前に構築されたビューにデータソースをすばやく簡単にバインドするための API を提供します。 リスト項目と詳細ページは、データを自動的に表示し、テーマを使用してカスタマイズできます。

基調講演のデモのしくみを確認するには、[ファーストステップガイド](get-started.md)を参照してください。

[![](images/demo-sml.png "DataPages Sample Application")](images/demo.png#lightbox "DataPages Sample Application")

## <a name="introduction"></a>はじめに

データソースと関連付けられたデータページを使用すると、開発者はサポートされているデータソースをすばやく簡単に使用し、テーマでカスタマイズできる組み込みの UI スキャフォールディングを使用してレンダリングできます。

DataPages は Xamarin.Forms 、を含めることによってアプリケーションに追加され** Xamarin.Forms ます。ページ**NuGet パッケージ。

### <a name="data-sources"></a>データ ソース

プレビューでは、いくつかの事前に構築されたデータソースを使用できます。

* **JsonDataSource**
* **Azuredatasource** (個別の NuGet)
* **AzureEasyTableDataSource** (個別の NuGet)

の使用例については、[ファーストステップガイド](get-started.md)を参照してください `JsonDataSource` 。

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

### <a name="under-the-hood"></a>しくみ

Xamarin.Formsデータソースはインターフェイスに準拠して `IDataSource` います。

インフラストラクチャは、 Xamarin.Forms 次のプロパティを使用してデータソースと対話します。

* `Data`–表示できるデータ項目の読み取り専用の一覧。
* `IsLoading`–データが読み込まれ、表示に使用できるかどうかを示すブール値。
* `[key]`–要素を取得するためのインデクサー。

`MaskKey` `UnmaskKey` データ項目のプロパティを非表示 (または表示) するには、との2つのメソッドを使用できます。 表示されないようにします)。
キーは、データ項目オブジェクトの名前付きプロパティに対応します。
