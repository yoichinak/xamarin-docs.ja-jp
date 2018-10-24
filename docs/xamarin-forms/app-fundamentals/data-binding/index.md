---
title: Xamarin.Forms のデータ バインディング
description: データ バインディングは、1 つのプロパティの変更は、その他のプロパティに自動的に反映されるように、2 つのオブジェクトのプロパティをリンクする方法です。 データ バインディングは、モデル-ビュー-ビューモデル (MVVM) アプリケーションのアーキテクチャの不可欠な部分です。
ms.prod: xamarin
ms.assetid: 938E85C8-521D-43B9-92CB-D591A06D98A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/16/2018
ms.openlocfilehash: 60bf0bdc5f1a4472dfd12424c3cc0375d3f34c24
ms.sourcegitcommit: 11c1df7594064e4e141470e092e55cc50c138068
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/23/2018
ms.locfileid: "35240354"
---
# <a name="xamarinforms-data-binding"></a>Xamarin.Forms のデータ バインディング

_データ バインディングは、1 つのプロパティの変更は、その他のプロパティに自動的に反映されるように、2 つのオブジェクトのプロパティをリンクする方法です。データ バインディングは、モデル-ビュー-ビューモデル (MVVM) アプリケーションのアーキテクチャの不可欠な部分です。_

## <a name="the-data-linking-problem"></a>データ リンクの問題

Xamarin.Forms アプリケーションの通常と呼ばれる複数のユーザー インターフェイス オブジェクトが含まれています、1 つまたは複数のページから成る*ビュー*します。 プログラムの主な作業の 1 つは、同期すると、これらのビューを保持し、さまざまな値やそれに対応する選択を追跡します。 多くの場合、ビューは基になるデータ ソースから値を表すし、ユーザーがそのデータを変更するこれらのビューを操作します。 基になるデータが、その変更を反映する必要があります、ビューが変更されたときと同様に、基になるデータが変更されたときにその変更を反映する必要があるビューにします。

このジョブを正常に処理するために、プログラムは、これらのビューまたは基になるデータの変更の通知する必要があります。 一般的な解決策では、変更が発生したときに、シグナル イベントを定義します。 イベント ハンドラーはこれらの変更の通知は、インストールします。 1 つのオブジェクトからデータを転送して応答します。 ただし、多くのビューが存在する場合、多くのイベント ハンドラーがありますが、多くのコードが関与します。

## <a name="the-data-binding-solution"></a>データ バインディングのソリューション

データ バインディングは、このジョブを自動化し、不要なイベント ハンドラーをレンダリングします。 (イベントはまだ必要に応じて、ただし、データ バインド インフラストラクチャを使用するため) です。コードまたは XAML では、データ バインディングを実装することができますが、XAML 分離コード ファイルのサイズを小さくための場所でさらに一般的です。 宣言型コードまたはマークアップと手続き型コードでイベント ハンドラーを置き換えることで、アプリケーションが簡素化しを明記しました。

データ バインディングに関連する 2 つのオブジェクトのいずれかから派生した要素は、ほぼ常に`View`し、ページのビジュアル インターフェイスの一部を形成します。 その他のオブジェクトは、いずれかです。

- もう 1 つ`View`同じページには、通常、派生します。
- コード ファイル内のオブジェクト。

デモ プログラムにあるように、 [ **DataBindingDemos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)サンプリングには、2 つのデータ バインド`View`導関数がわかりやすくするため、簡潔さのためによく表示されます。 同じ原則を適用して、間にデータ バインドするただし、`View`およびその他のオブジェクト。 モデル-ビュー-ビューモデル (MVVM) アーキテクチャを使用してアプリケーションをビルドするとき、基になるデータ クラスには多くの場合、ViewModel が呼び出されます。

次の一連の記事では、データ バインドがについて説明します。

## <a name="basic-bindingsbasic-bindingsmd"></a>[基本的なバインディング](basic-bindings.md)

データ バインディングのターゲットとソースの違いについて説明し、コードと XAML で単純なデータ バインドを参照してください。

## <a name="binding-modebinding-modemd"></a>[バインディング モード](binding-mode.md)

バインド モードが 2 つのオブジェクト間のデータ フローを制御する方法を説明します。

## <a name="string-formattingstring-formattingmd"></a>[文字列の形式設定](string-formatting.md)

書式設定し、オブジェクトを文字列として表示するには、データ バインディングを使用します。

## <a name="binding-pathbinding-pathmd"></a>[バインディング パス](binding-path.md)

詳しく、`Path`サブプロパティとコレクションのメンバーにアクセスするデータ バインディングのプロパティ。

## <a name="binding-value-convertersconvertersmd"></a>[値コンバーターのバインディング](converters.md)

値コンバーターのバインディングを使用すると、データ バインディング内の値を変更できます。

## <a name="binding-fallbacksbinding-fallbacksmd"></a>[バインドのフォールバック](binding-fallbacks.md)

バインド プロセスが失敗した場合に使用するフォールバック値を定義することより堅牢なデータ バインドを行います。

## <a name="the-command-interfacecommandingmd"></a>[コマンド インターフェイス](commanding.md)

実装、`Command`データ バインドを持つプロパティです。

## <a name="related-links"></a>関連リンク

- [データ バインディング デモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms book からデータ バインド」の章](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
- [XAML マークアップ拡張](~/xamarin-forms/xaml/markup-extensions/index.md)
