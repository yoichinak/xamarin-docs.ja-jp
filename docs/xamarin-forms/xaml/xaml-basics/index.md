---
title: Xamarin.Forms XAML Basics
description: モバイル デバイス用のクロスプラット フォームのマークアップの概要
ms.prod: xamarin
ms.assetid: 67CC2CD6-D10A-4B14-9696-1D3A410EFFBF
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: 991d928c2c58f05098a41c84aba295a31636ab96
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinforms-xaml-basics"></a>Xamarin.Forms XAML Basics

XAML (eXtensible Application Markup Language) を使用すると、開発者は Xamarin.Forms アプリケーションでコードではなくマークアップを使用してユーザー インターフェイスを定義できます。 プログラムでは、Xamarin.Forms、XAML が必要なことはありませんが、詳細は簡潔なと同等のコードより視覚的に一貫性のある潜在的に使いやすい。 XAML は、人気のある MVVM (モデル-ビュー-ViewModel) アプリケーションのアーキテクチャで使用する場合に特に適して: XAML を XAML ベースのデータ バインディングによって ViewModel コードにリンクされているビューを定義します。

## <a name="xaml-basics-contents"></a>XAML の基礎の内容

* [概要](#Overview)
* [第 1 部XAML の概要](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
* [第 2 部基本的な XAML 構文](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
* [第 3 部XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
* [第 4 部データ バインディングの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
* [第 5 部MVVM へのデータ バインディング](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)

これらの XAML の基礎記事だけでなく、本の章をダウンロードできます[Xamarin.Forms を使用したモバイル アプリを作成する](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md):

[![](images/cover-sml.png "表紙")](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)

XAML 内容は、書籍の多くの章で詳細を含みます。

<table style="border:0px; box-shadow:0 0px 0px" cellpadding="0" cellspacing="2" border="0" width="85%">
<tr style="background:#ecf0f1">
  <td style="border:0px;">
    <h4>第 7 章します。 XAML とします。コード</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf">PDF をダウンロードします。</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md">まとめ</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>第 8 章します。 コードと調和で XAML</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf">PDF をダウンロードします。</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter08.md">まとめ</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>章 10 です。 XAML マークアップ拡張機能</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf">PDF をダウンロードします。</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md">まとめ</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>第 18 章します。 MVVM</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf">PDF をダウンロードします。</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md">まとめ</a></td></tr>
</table>

これらの章を指定できます[無料でダウンロード](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)です。

<a name="Overview" />

## <a name="overview"></a>概要

XAML は、インスタンス化して、オブジェクトの初期化および親子階層内のオブジェクトを整理するためのプログラミング コードの代わりとして、Microsoft によって作成された XML に基づく言語です。 .NET framework 内で複数のテクノロジに XAML を適応させるが、Windows Presentation Foundation (WPF)、Silverlight、Windows ランタイムおよびユニバーサル Windows 内のユーザー インターフェイスのレイアウトを定義する、最大のユーティリティが検出プラットフォーム (UWP) です。

XAML は、Xamarin.Forms、クロスプラット フォーム ネイティブ ベースのプログラミング インターフェイスの iOS、Android、および UWP の一部のモバイル デバイス。 XAML ファイル内では、Xamarin.Forms 開発者はすべて、Xamarin.Forms ビュー、レイアウト、およびページを使用して、カスタム クラスとしてユーザー インターフェイスを定義できます。 XAML ファイルは、いずれかをコンパイルまたは実行可能ファイルに埋め込まれています。 どちらにしても、名前付きのオブジェクトを特定のビルド時ともう一度実行時にインスタンス化し、オブジェクトを初期化し、これらのオブジェクトとプログラミング コード間のリンクを確立するために、XAML の情報が解析されます。

XAML では、同等のコードをいくつかの利点があります。

-  XAML は、多くの場合はより簡潔なと同等のコードよりも読みやすくします。
-  XML に特有の親子階層は、大きい visual わかりやすくするためのユーザー インターフェイス オブジェクトの親子階層を模倣するために XAML を許可します。
-  XAML では、簡単に手動で記述したプログラマにできますが、使いやすいビジュアル デ ザイン ツールによって生成されるようにするためにも適しています。

もちろん、ほとんどの場合は、マークアップ言語に固有の制限に関連する欠点も。

-  XAML では、コードを含めることはできません。 すべてのイベント ハンドラーは、コード ファイルで定義する必要があります。
-  XAML では、ループの反復処理を含めることはできません。 (ただし、いくつかの Xamarin.Forms visual オブジェクト — 最も顕著な[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) — 内のオブジェクトに基づく複数の子を生成することができます、`ItemsSource`コレクション)。
-  XAML は、条件付きの処理 (ただし、データ バインディングを参照できますいくつかの条件付き処理を効果的にできるコード ベースのバインディング コンバーター。) を含めることはできません。
-  一般に XAML することは、パラメーターなしのコンス トラクターを定義していないクラス インスタンス化できません。 (ただし、存在がこの制限を回避する方法です。)
-  XAML は一般に、メソッドを呼び出すことはできません。 (ここでも、この制限も解決できます。)

いないビジュアルなデザイナー Xamarin.Forms アプリケーションで XAML を生成するためです。 すべての XAML は、手動で作成する必要がありますが、 [XAML プレビューアー](~/xamarin-forms/xaml/xaml-previewer.md)です。 プログラマは、新しい XAML 可能性があります頻繁に作成する明らかに正しくできない可能性がある何も特に後に、アプリケーションを実行します。 多くの XAML での経験によって、開発者は偶数では、実験が効率的になっていることを知っています。

XAML は XML で、基本的に XAML に固有の構文の一部の機能があるが、します。 最も重要なは。

- プロパティ要素
- 添付プロパティ
- マークアップ拡張機能

これらの機能は*いない*XML 拡張。 XAML は、完全に有効な XML です。 これらの XAML 構文機能は、独自の方法で XML を使用します。 説明して、以下の記事で詳しく MVVM を実装するための XAML の使用の概要ではこれで完了します。

## <a name="requirements"></a>要件

この記事は、Xamarin.Forms を使用した作業に関する知識を前提とします。 読み取り[An Introduction to Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)を強くお勧めします。

この記事には、XML 名前空間の宣言と用語を使用する方法を含む XML を理解しても前提としています*要素*、*タグ*、および*属性*です。

Xamarin.Forms および XML を理解できたら、読み取りを開始[パート 1 です。XAML の概要](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)です。



## <a name="related-links"></a>関連リンク

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Xamarin.Forms の概要](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [モバイル アプリのブックを作成します。](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)
- [Xamarin.Forms のサンプル](https://developer.xamarin.com/samples/xamarin-forms/all/)
