---
title: Xamarin.Forms XAML の基礎
description: このガイドでは、モバイル デバイス用のクロスプラット フォームでの XAML を使用する方法について説明します。 XAML では、コードではなく、マークアップを使用して、Xamarin.Forms アプリケーションのユーザー インターフェイスを定義できます。
ms.prod: xamarin
ms.assetid: 67CC2CD6-D10A-4B14-9696-1D3A410EFFBF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
ms.openlocfilehash: d8b522ce75b2b594242dca167242ad0362f6cbfc
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69528307"
---
# <a name="xamarinforms-xaml-basics"></a>Xamarin.Forms XAML の基礎

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

XAML (eXtensible Application Markup Language) を使用すると、開発者は Xamarin.Forms アプリケーションでコードではなくマークアップを使用してユーザー インターフェイスを定義できます。 Xamarin.Forms のプログラムでは、XAML が必要なことはありませんが、方が簡潔なと同等のコードより視覚的に一貫性のある使いやすい可能性があります。 XAML は、一般的によく使用される MVVM (モデルビューモデル) アプリケーションアーキテクチャでの使用に適しています。Xaml は、XAML ベースのデータバインディングを通じて、ビューモデルコードにリンクされたビューを定義します。

## <a name="xaml-basics-contents"></a>XAML の基礎の内容

* [概要](#Overview)
* [第 1 部XAML の概要](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
* [第 2 部基本的な XAML 構文](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
* [第 3 部XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
* [第 4 部データ バインディングの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
* [第 5 部MVVM へのデータ バインディングから](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)

書籍の章をダウンロードするだけでなく、次の XAML の基礎記事[を Xamarin.Forms での Mobile Apps の作成](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md):

[![](images/cover-sml.png "本の表紙")](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)

XAML 内容は、書籍の多数の章で詳しくなど。


| 」の章 | ダウンロード | Summary |
|---------|---------|---------|
| 第 7 章 XAML 対コード | [PDF のダウンロード](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf) | [まとめ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md) |
| 第 8 章 コードと調和で XAML | [PDF のダウンロード](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf) | [まとめ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter08.md) |
| 第10章 XAML マークアップ拡張機能 | [PDF のダウンロード](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf) | [まとめ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md) |
| 第 18 章 MVVM | [PDF のダウンロード](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf) | [概要](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md) |

これらの章は、[無料でダウンロード](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)します。

<a name="Overview" />

## <a name="overview"></a>概要

XAML は、プログラミング コードをインスタンス化して、オブジェクトの初期化および親子階層内の各オブジェクトを整理するための代替として、Microsoft によって作成された XML ベースの言語です。 XAML が、.NET framework 内でいくつかのテクノロジに適応させるが、Windows Presentation Foundation (WPF)、Silverlight、Windows ランタイム、およびユニバーサル Windows 内のユーザー インターフェイスのレイアウトを定義する際に、最大のユーティリティが検出プラットフォーム (UWP)。

XAML は Xamarin.Forms、クロス プラットフォーム ネイティブ ベースのプログラミング インターフェイス、iOS、Android、および UWP の一部でもモバイル デバイス。 XAML ファイル内では、Xamarin.Forms 開発者はすべて、Xamarin.Forms のビュー、レイアウト、およびページを使用して、カスタム クラスとしてユーザー インターフェイスを定義できます。 XAML ファイルは、いずれかのコンパイルまたは実行可能ファイルに埋め込まれています。 いずれにしても、XAML 情報は、名前付きのオブジェクトを検索するビルド時に、もう一度インスタンス化し、オブジェクトを初期化して、これらのオブジェクトとプログラミング コード間のリンクを確立するために、実行時に解析されます。

XAML では、同等のコードをいくつかの利点があります。

- XAML は、多くの場合は詳細は、簡潔で同等のコードよりも読みやすくします。
- 親子階層の XML に固有では、visual できるようにするためのユーザー インターフェイス オブジェクト、親子階層を模倣するために XAML をできます。
- XAML では、プログラマ、簡単に手記述できますが、理解できる使いやすいビジュアル デ ザイン ツールによって生成されるようにするためにも適しています。

もちろん、ほとんどの場合は、マークアップ言語に固有の制限に関連するデメリットも。

- XAML では、コードを含めることはできません。 すべてのイベント ハンドラーは、コード ファイルで定義する必要があります。
- XAML では、反復処理のループを含めることはできません。 (ただし、いくつかの Xamarin.Forms のビジュアル オブジェクト-最も顕著な[ `ListView` ](xref:Xamarin.Forms.ListView) -内のオブジェクトに基づく複数の子要素を生成することができます、`ItemsSource`コレクションです)。
- XAML は、条件付きの処理 (ただし、データ バインディングを参照できますいくつかの条件付き処理を効率的にできるコード ベースのバインディング コンバーターです。) を含めることはできません。
- 一般に XAML することは、パラメーターなしのコンストラクターを定義しないクラス インスタンス化できません。 (ただし、存在がこの制限を回避する方法です。)
- XAML は一般に、メソッドを呼び出すことはできません。 (ここでも、この制限も解決できます。)

ない、ビジュアル デザイナー Xamarin.Forms アプリケーションで XAML を生成するためです。 すべての XAML は、手作業で記述された、する必要がありますが、ある、 [XAML プレビューアー](~/xamarin-forms/xaml/xaml-previewer/index.md)します。 プログラマは新しい XAML を頻繁にビルドして、特に後何も明らかに誤ってできない可能性がある、アプリケーションを実行する可能性があります。 多くの XAML での経験もの開発者は、実験が報いることを知っています。

XAML は、XML では基本的に XAML が固有の構文の一部の機能です。 最も重要なは。

- プロパティ要素
- 添付プロパティ
- マークアップ拡張機能

これらの機能は*いない*XML 拡張機能。 XAML は、完全に有効な XML です。 これらの XAML 構文の機能は、独自の方法で XML を使用します。 これらは詳しく説明、以下の記事で最後に、MVVM を実装するための XAML を使用しています。

## <a name="requirements"></a>必要条件

この記事では、Xamarin.Forms を熟知して作業を想定しています。 この記事には、いくつかの XML 名前空間の宣言、および用語を使用する方法を含む XML に関する知識も前提としています*要素*、*タグ*、および*属性*します。

Xamarin.Forms と XML について理解したら、読み取りを開始[第 1 部です。XAML の概要](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)します。

## <a name="related-links"></a>関連リンク

- [XamlSamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [Mobile Apps のブックを作成します。](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)
- [Xamarin.Forms のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
