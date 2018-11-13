---
title: 使用し、Xamarin.Forms のプラグインを作成します。
description: この記事では、使用し、Xamarin.Forms のプラグインを作成する方法について説明します。 プラグインは、ネイティブ プラットフォームの機能を簡単に公開する通常使用されます。
ms.prod: xamarin
ms.assetid: 8A06A420-A9D0-4BCB-B9AF-3AEA6A648A8B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/05/2018
ms.openlocfilehash: ac8e5323a2a2e05ac03294bb6919e8dfadc93655
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/11/2018
ms.locfileid: "51526521"
---
# <a name="consuming-and-creating-xamarinforms-plugins"></a>使用し、Xamarin.Forms のプラグインを作成します。

すべてのプラットフォーム間に存在する多くのネイティブ プラットフォーム機能がありますが、若干異なる Api があります。 これらの機能を使用する開発者の 1 つの方法は、抽象のクロス プラットフォームのインターフェイスを作成して、さまざまなプラットフォームでそのインターフェイスを実装することです。 次に、Xamarin.Forms アプリケーションにアクセスを使用してこれらのプラットフォーム実装[ `DependencyService`](~/xamarin-forms/app-fundamentals/dependency-service/index.md)します。

開発者が記述することでこの作業を共有できます、_プラグイン_NuGet に公開するとします。

> [!NOTE]
> 以前、プラグインによってのみ使用可能な多くのクロス プラットフォーム機能、オープン ソースの一部になった**[Xamarin.Essentials](~/essentials/index.md)** ライブラリ。 これらの機能が含まれます: バッテリの状態、コンパス、モーション センサー、位置情報、音声合成、およびそうですね。 今後、 **Xamarin.Essentials** Xamarin.Forms アプリケーションのクロス プラットフォームの機能の主要なソースになります。 開発者を作成してプラグインを発行できますが、検討に貢献する**Xamarin.Essentials**します。

## <a name="finding-and-adding-plugins"></a>検索とプラグインの追加

Xamarin コミュニティには多くのクロスプラット フォーム対応のプラグインが Xamarin.Forms と互換性のある作成します。 大規模なコレクションはあることができます。

[**Xamarin プラグイン**](https://github.com/xamarin/XamarinComponents)

NuGet パッケージをプロジェクトに追加する場合にガイドについてで、チュートリアルを参照してください。 [NuGet パッケージをプロジェクトに含める](/visualstudio/mac/nuget-walkthrough/)します。

## <a name="creating-plugins"></a>プラグインを作成します。

作成して、独自のプラグインを Nuget パッケージ (および Xamarin コンポーネント) として発行することもできます。 多くの既存のプラグインはオープン ソース writtern をされている方法を理解するには、そのコードを確認することができます。

たとえば、プラグインは次の一覧は、すべてのオープン ソースとのいくつかのサンプルに対応する、 [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md)セクション。

- **音声合成**James montemagno &ndash; [GitHub](https://github.com/jamesmontemagno/TextToSpeechPlugin)と[NuGet  ](https://www.nuget.org/packages/Xam.Plugins.TextToSpeech)
- **バッテリの状態**James montemagno &ndash; [GitHub](https://github.com/jamesmontemagno/BatteryPlugin)と[NuGet](https://www.nuget.org/packages/Xam.Plugin.Battery)

これらのプロジェクトを Github は、次の手順と同様に、独自のクロスプラット フォーム対応のプラグインを作成するための出発点として適していますを提供できます[for Xamarin プラグインを作成する](https://github.com/xamarin/XamarinComponents#create-a-plugin-for-xamarin)します。

### <a name="structuring-cross-platform-plugin-projects"></a>クロスプラット フォーム対応のプラグインのプロジェクトの構成

NuGet パッケージを設計するための特定の要件はありませんが、クロス プラットフォーム アプリのパッケージを作成するためのガイドラインがあります。

以前は、クロスプラット フォーム対応のプラグインは、一般に、次のコンポーネントのとおりです。

- プラグイン用の API を表すインターフェイスを使用して PCL
- iOS、Android、およびユニバーサル Windows プラットフォーム (UWP) はクラス、インターフェイスの実装とライブラリです。

読み取り James Montemagno 氏[ブログの投稿](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms/)Xamarin プラグインを作成するための手順について説明します。

最近では、プラグインは、1 つのターゲットが複数のプラットフォームで作成できます。 このアプローチは、James Montemagno 氏で説明[ブログの投稿](https://montemagno.com/converting-xamarin-libraries-to-sdk-style-multi-targeted-projects/)します。 この方法は、上のリンクから James Montemagno 氏のプラグインで使用して、形式にも使用**Xamarin.Essentials**します。

Xamarin.Forms をプラグインから直接参照しないようにする方が望ましいです。
バージョン競合の問題は、他の開発者が、プラグインを使用しようとしています。 ときにこの作成できます。 代わりに、任意の Xamarin または .NET アプリケーションで使用できるように、API を設計するみてください。

### <a name="publishing-nuget-packages"></a>NuGet パッケージを公開します。

NuGet パッケージが、 **nuspec**ファイルで、プロジェクトのどの部分が、パッケージで公開されるを定義する xml ファイルです。 **Nuspec**ファイルには、id、タイトル、作成者など、パッケージに関する情報も含まれています。

参照してください[NuGet のドキュメント](/nuget/create-packages/creating-a-package.md)の作成と NuGet パッケージの公開の詳細についてはします。

## <a name="related-links"></a>関連リンク

- [Xamarin.Forms 用の再利用可能なプラグインの作成](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms)
- [Xamarin (ビデオ) の使用と開発のプラグイン](https://university.xamarin.com/guestlectures/using-developing-plugins-for-xamarin)
