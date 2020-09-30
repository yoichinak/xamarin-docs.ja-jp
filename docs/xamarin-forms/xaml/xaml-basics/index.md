---
title: Xamarin.Forms XAML の基礎
description: このガイドでは、モバイルデバイス用のクロスプラットフォーム XAML の使用を開始する方法について説明します。 XAML を使用すると、開発者は Xamarin.Forms コードではなくマークアップを使用して、アプリケーションのユーザーインターフェイスを定義できます。
ms.prod: xamarin
ms.custom: video
ms.assetid: 67CC2CD6-D10A-4B14-9696-1D3A410EFFBF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3d2620014c9554712c746b98de3b9a0eeaa20864
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91562757"
---
# <a name="no-locxamarinforms-xaml-basics"></a>Xamarin.Forms XAML の基礎

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

拡張可能なアプリケーションマークアップ言語 (XAML) は、オブジェクトをインスタンス化および初期化し、それらのオブジェクトを親子階層で整理するためのプログラミングコードの代わりに、Microsoft によって作成された XML ベースの言語です。 XAML は .NET framework 内のいくつかのテクノロジに応用されていますが、Windows Presentation Foundation (WPF)、Silverlight、Windows ランタイム、およびユニバーサル Windows プラットフォーム (UWP) 内のユーザーインターフェイスのレイアウトを定義するのに最も多くのユーティリティが見つかりました。

XAML を使用すると、開発者は Xamarin.Forms コードではなくマークアップを使用して、アプリケーションのユーザーインターフェイスを定義できます。 XAML は、プログラムでは必要ありませんが、多くの場合、同等のコードや、場合によって Xamarin.Forms は、より簡潔で視覚的に一貫性があります。 Xaml は、一般的な MVVM (モデルビューモデル) アプリケーションアーキテクチャでの使用に適しています。 xaml は、xaml ベースのデータバインディングを通じて、ビューモデルコードにリンクされたビューを定義します。

XAML ファイル内では、 Xamarin.Forms 開発者は、すべての Xamarin.Forms ビュー、レイアウト、ページ、およびカスタムクラスを使用してユーザーインターフェイスを定義できます。 XAML ファイルは、コンパイルするか、実行可能ファイルに埋め込むことができます。 どちらの方法でも、XAML 情報はビルド時に解析され、名前付きオブジェクトを検索して、実行時にオブジェクトをインスタンス化および初期化したり、これらのオブジェクトとプログラミングコード間のリンクを確立したりします。

XAML には、同等のコードよりもいくつかの利点があります。

- XAML は、多くの場合、同等のコードよりも簡潔で読みやすくなります。
- XML に固有の親子階層を使用すると、XAML は、ユーザーインターフェイスオブジェクトの親子階層をより視覚的に明確にすることができます。
- XAML はプログラマが簡単に記述できますが、ビジュアルデザインツールによってツール可能で生成されることもあります。

また、主にマークアップ言語に固有の制限事項に関連する欠点もあります。

- XAML にコードを含めることはできません。 すべてのイベントハンドラーは、コードファイルで定義する必要があります。
- XAML に繰り返し処理するためのループを含めることはできません。 (ただし、いくつかの Xamarin.Forms ビジュアルオブジェクト (特に) は、  [`ListView`](xref:Xamarin.Forms.ListView) コレクション内のオブジェクトに基づいて複数の子を生成でき `ItemsSource` ます)。
- XAML に条件付き処理を含めることはできません (ただし、データバインディングでは、一部の条件付き処理を効果的に許可するコードベースのバインディングコンバーターを参照できます)。
- 一般に、パラメーターなしのコンストラクターを定義しないクラスを XAML でインスタンス化することはできません。 (ただし、この制限を回避する方法があります)。
- XAML は、一般にメソッドを呼び出すことはできません。 (この場合も、この制限が克服される可能性があります)。

アプリケーションで XAML を生成するためのビジュアルデザイナーはまだありません Xamarin.Forms 。 すべての XAML は手動で記述する必要がありますが、 [Xaml プレビューアー](~/xamarin-forms/xaml/xaml-previewer/index.md)があります。 XAML を初めて使用するプログラマは、アプリケーションを頻繁にビルドして実行することが必要になる場合があります (特に、適切でない場合)。 XAML で多くの経験を持つ開発者であっても、実験は向上しています。

XAML は基本的に XML ですが、XAML にはいくつかの固有の構文機能があります。 最も重要なものは次のとおりです。

- Property 要素
- 添付プロパティ
- マークアップ拡張機能

これらの機能は XML 拡張機能では *ありません* 。 XAML は完全に有効な XML です。 ただし、これらの XAML 構文機能では、一意の方法で XML を使用します。 これらの詳細については、以下の記事で詳しく説明します。これについては、MVVM を実装するための XAML の使用の概要について説明します。

## <a name="requirements"></a>要件

この記事では、に関する知識があることを前提としてい Xamarin.Forms ます。 また、この記事では、xml 名前空間宣言の使用方法、 *要素*、 *タグ*、 *属性*などの xml について理解していることも前提としています。

と XML に慣れている場合 Xamarin.Forms は、パート1の読み取りを開始 [します。XAML ではじめに](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)します。

## <a name="related-links"></a>関連リンク

- [XamlSamples](/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [Mobile Apps book の作成](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)
- [Xamarin.Forms サンプル](/samples/browse/?products=xamarin&term=Xamarin.Forms)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Series/Xamarin-101/XamarinForms-UI-with-XAML-5-of-11/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]