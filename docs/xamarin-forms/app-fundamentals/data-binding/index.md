---
title: Xamarin.Forms のデータ バインディング
description: データ バインディングは、2 つのオブジェクトのプロパティをリンクして、片方のプロパティへの変更が自動的にもう片方のプロパティに反映されるようにする手法です。 データ バインディングは、Model-View-ViewModel (MVVM) アプリケーション アーキテクチャにとって不可欠の部分です。
ms.prod: xamarin
ms.assetid: 938E85C8-521D-43B9-92CB-D591A06D98A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/23/2018
ms.openlocfilehash: c607cecf6c7044fa4c8d0270a5b8d1471d3f9227
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53059062"
---
# <a name="xamarinforms-data-binding"></a>Xamarin.Forms のデータ バインディング

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)

_データ バインディングは、2 つのオブジェクトのプロパティをリンクして、片方のプロパティへの変更が自動的にもう片方のプロパティに反映されるようにする手法です。データ バインディングは、Model-View-ViewModel (MVVM) アプリケーション アーキテクチャにとって不可欠の部分です。_

## <a name="the-data-linking-problem"></a>データのリンクに関する問題

Xamarin.Forms アプリケーションは 1 つ以上のページで構成され、通常はそのそれぞれに "*ビュー*" と呼ばれるユーザー インターフェイス オブジェクトが複数含まれています。 プログラムの主なタスクの 1 つは、各ビューを同期された状態に保ち、それらが表すさまざまな値や選択を追跡することです。 多くの場合、ビューは基になるデータ ソースの値を表していて、ユーザーが各ビューを操作してそのデータを変更します。 ビューが変更された場合は、基になるデータにその変更が反映される必要があり、また同様に、基になるデータが変更された場合は、その変更がビューに反映される必要があります。

このジョブを適切に処理するために、各ビューや基になるデータへの変更をプログラムに通知する必要があります。 一般的なソリューションは、変更が発生したときに通知されるイベントを定義することです。 次に、これらの変更について通知されるイベント ハンドラーを導入することができます。 これは、1 つのオブジェクトからのデータを別のものに転送することで応答します。 しかし、ビューが多数存在している場合、イベント ハンドラーも多数必要となり、たくさんのコードが関与します。

## <a name="the-data-binding-solution"></a>データ バインディングのソリューション

データ バインディングによってこのジョブが自動化され、イベント ハンドラーが不要になります。 (ただし、イベントはまだ必要です。データ バインディングのインフラストラクチャで使われるためです。)データ バインディングは、コードまたは XAML のどちらでも実装できますが、分離コード ファイルのサイズを小さくできる XAML での方がはるかに一般的です。 イベント ハンドラーにおける手続き型コードを宣言型コードまたはマークアップに置き換えることで、アプリケーションが簡略化されてわかりやすくなります。

データ バインディングに関与する 2 つのオブジェクトのうちの 1 つは、ほとんど常に、`View` から派生してページのビジュアル インターフェイスの一部を形成している要素となります。 もう一方のオブジェクトは次のいずれかです。

- 通常は同じページ上にある、`View` の別の派生物。
- コード ファイル内のオブジェクト。

[**DataBindingDemos**](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) サンプルにあるようなデモ プログラムでは、`View` の 2 つの派生物間のデータ バインディングは、多くの場合、わかりやすさと簡潔さのために示されます。 ただし、`View` と別のオブジェクト間のデータ バインディングにも、同じ原則を適用できます。 アプリケーションが Model-View-ViewModel (MVVM) アーキテクチャを使って構築されている場合、基になるデータを使うクラスは多くの場合 ViewModel と呼ばれます。

データ バインディングついては、次の一連の記事で説明されています。

## <a name="basic-bindingsbasic-bindingsmd"></a>[基本的なバインディング](basic-bindings.md)

データ バインディングのターゲットとソースの違いについて学習し、コードと XAML における単純なデータ バインディングを確認します。

## <a name="binding-modebinding-modemd"></a>[バインディング モード](binding-mode.md)

バインディング モードによって 2 つのオブジェクト間のデータ フローを制御する方法について説明します。

## <a name="string-formattingstring-formattingmd"></a>[文字列の形式設定](string-formatting.md)

データ バインディングを使って、オブジェクトを書式設定して文字列として表示させます。

## <a name="binding-pathbinding-pathmd"></a>[バインディング パス](binding-path.md)

データ バインディングの `Path` プロパティについて詳しく調べ、サブプロパティとコレクションのメンバーにアクセスします。

## <a name="binding-value-convertersconvertersmd"></a>[値コンバーターのバインディング](converters.md)

値コンバーターのバインディングを使って、データ バインディング内の値を変更します。

## <a name="binding-fallbacksbinding-fallbacksmd"></a>[フォールバックのバインディング](binding-fallbacks.md)

バインディングのプロセスが失敗した場合に使うフォールバック値を定義することにより、より堅牢なデータ バインディングを実現します。

## <a name="the-command-interfacecommandingmd"></a>[コマンド インターフェイス](commanding.md)

データ バインディングを使って `Command` プロパティを実装します。

## <a name="compiled-bindingscompiled-bindingsmd"></a>[コンパイル済みのバインディング](compiled-bindings.md)

コンパイル済みのバインディングを使ってデータ バインディングのパフォーマンスを向上させます。

## <a name="related-links"></a>関連リンク

- [データ バインディングのデモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms 書籍のデータ バインディングに関する章](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
- [XAML マークアップ拡張](~/xamarin-forms/xaml/markup-extensions/index.md)
