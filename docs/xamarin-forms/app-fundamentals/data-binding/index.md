---
title: Xamarin.Forms のデータ バインディング
description: データ バインディングは、1 つのプロパティの変更は、その他のプロパティで自動的に反映されるように、2 つのオブジェクトのプロパティをリンクする方法です。 データ バインディングは、モデル View-viewmodel (MVVM) アプリケーションのアーキテクチャの不可欠な部分です。
ms.prod: xamarin
ms.assetid: 938E85C8-521D-43B9-92CB-D591A06D98A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: a5ea5dcb5b108da52634f131fd36a91ba82f7da4
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240354"
---
# <a name="xamarinforms-data-binding"></a>Xamarin.Forms のデータ バインディング

_データ バインディングは、1 つのプロパティの変更は、その他のプロパティで自動的に反映されるように、2 つのオブジェクトのプロパティをリンクする方法です。データ バインディングは、モデル View-viewmodel (MVVM) アプリケーションのアーキテクチャの不可欠な部分です。_

## <a name="the-data-linking-problem"></a>データ リンクの問題

Xamarin.Forms アプリケーションは、通常と呼ばれる複数のユーザー インターフェイス オブジェクトを含むそれぞれの 1 つまたは複数のページで構成される*ビュー*です。 プログラムの主要なタスクの 1 つは、同期されていれば、これらのビューを保持して、さまざまな値やそれに対応する選択項目を追跡するためです。 多くの場合、ビューは、基になるデータ ソースから値を表すし、ユーザーがそのデータを変更するこれらのビューを操作します。 基になるデータがその変更を反映する必要があります、ビューが変更されたときと同様に、基になるデータが変更されたときにその変更に反映するビュー。

このジョブを正常に処理するためにこれらのビューまたは基になるデータの変更のプログラムを通知する必要があります。 一般的なソリューションでは、変更が発生したときに通知するイベントを定義します。 イベント ハンドラーはこれらの変更通知されるしてインストールします。 1 つのオブジェクトを別のデータを転送することによって応答します。 ただし、多くのビューが存在する場合、多くのイベント ハンドラーがありますが、多くのコードが関与します。

## <a name="the-data-binding-solution"></a>データ バインディングのソリューション

データ バインディングは、このジョブを自動化し、不要なイベント ハンドラーを表示します。 (イベントは引き続き必要に応じて、ただし、データ バインド インフラストラクチャを使用しているため) です。コードまたは XAML では、データ バインディングを実装することができますが、分離コード ファイルのサイズを小さくできる XAML のほうが一般的です。 宣言コードまたはマークアップ イベント ハンドラーで手順のコードに置き換えて、アプリケーションが簡略化され、明確にされました。

データ バインディングに関連する 2 つのオブジェクトのいずれかから派生した要素ではほぼ`View`し、ページのビジュアル インターフェイスの一部を形成します。 その他のオブジェクトは、いずれか。

- 別`View`通常は同じページで、派生します。
- コード ファイル内のオブジェクト。

デモ プログラムなどで、 [ **DataBindingDemos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)サンプルは、2 つのデータ バインディング`View`明確さと簡潔さのために派生型が表示される多くの場合。 ただし、間のデータ バインディングを同じ基本原則を適用できます、`View`およびその他のオブジェクト。 モデル View-viewmodel (MVVM) アーキテクチャを使用してアプリケーションをビルドすると、基になるデータを使用してクラスには多くの場合、ViewModel が呼び出されます。

次の一連の記事では、データ バインディングがについて説明しました。

## <a name="basic-bindingsbasic-bindingsmd"></a>[基本的なバインディング](basic-bindings.md)

データ バインディングのターゲットとソースの違いを理解し、コードと XAML で単純なデータ バインディングを参照してください。

## <a name="binding-modebinding-modemd"></a>[バインディング モード](binding-mode.md)

バインド モードが 2 つのオブジェクト間のデータ フローを制御する方法を検出します。

## <a name="string-formattingstring-formattingmd"></a>[文字列の形式設定](string-formatting.md)

書式設定およびオブジェクトを文字列として表示するには、データ バインディングを使用します。

## <a name="binding-pathbinding-pathmd"></a>[バインディング パス](binding-path.md)

詳しく、`Path`サブ プロパティおよびコレクションのメンバーにアクセスするデータ バインドのプロパティです。

## <a name="binding-value-convertersconvertersmd"></a>[値コンバーターのバインディング](converters.md)

バインディングの値コンバーターを使用すると、データ バインディング内の値を変更できます。

## <a name="the-command-interfacecommandingmd"></a>[コマンド インターフェイス](commanding.md)

実装、`Command`データ バインディングを持つプロパティです。



## <a name="related-links"></a>関連リンク

- [データ バインディング デモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms 帳からのデータ バインディング章](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
- [XAML マークアップ拡張](~/xamarin-forms/xaml/markup-extensions/index.md)
