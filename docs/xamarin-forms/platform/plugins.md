---
title: プラグイン
description: ネイティブ機能を Xamarin.Forms アプリに簡単に追加します。
ms.prod: xamarin
ms.assetid: 8A06A420-A9D0-4BCB-B9AF-3AEA6A648A8B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/07/2016
ms.openlocfilehash: 5770d13c46998872752820b7a0cbb222a04c3ff8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="plugins"></a>プラグイン

すべてのプラットフォームにわたって存在するネイティブ プラットフォーム機能が多数ありますが、若干異なる Api があります。 開発者は、他のユーザーと共有することもできます。 その機能の抽象クロスプラット フォーム インターフェイスを作成するプラグインを記述します。

これらの機能が含まれます。 バッテリの状態、コンパス、センサー、地理的位置情報、音声合成、および、もっとです。 プラグインは、Xamarin.Forms のアプリケーションで簡単にアクセスできるこれらの機能を許可します。

## <a name="finding-and-adding-plugins"></a>検索とプラグインの追加

Xamarin コミュニティが作成多くのプラグインのクロスプラット フォーム - Xamarin.Forms を使用した互換性のある大規模なコレクションにあります。

[**Xamarin のプラグイン**](https://github.com/xamarin/plugins)

NuGet パッケージをプロジェクトに追加する場合に、ガイド用のチュートリアルを参照してください。 [、プロジェクトの NuGet パッケージを含む](/visualstudio/mac/nuget-walkthrough/)です。


## <a name="creating-plugins"></a>プラグインを作成します。

作成して Nuget パッケージ (、および Xamarin コンポーネント) として独自のプラグインを発行することもできます。 多くの既存のプラグインはオープン ソース writtern をされている方法を理解するためのコードを確認することができます。

たとえば、次のプラグインの一覧は、すべてのオープン ソースとサンプルに対応する、 [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md)セクション。

- **音声合成**James Montemagno によって&ndash; [GitHub](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/TextToSpeech)と[NuGet](https://www.nuget.org/packages/Xam.Plugin.Battery)
- **バッテリの状態**James Montemagno によって&ndash; [GitHub](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/Battery)と[NuGet](https://www.nuget.org/packages/Xam.Plugins.TextToSpeech/)

Github、これらのプロジェクトに次の手順と同様に、独自のクロスプラット フォームのプラグインを作成するための適切な開始点を提供できます[Xamarin 用のプラグインを作成する](https://github.com/xamarin/plugins#create-a-plugin-for-xamarin)です。

### <a name="structuring-cross-platform-plugin-projects"></a>クロス プラットフォームのプラグインのプロジェクトを構成します。

NuGet パッケージをデザインするための特定の要件はありませんが、クロスプラット フォーム アプリのパッケージを作成するためのガイドラインがあります。

クロスプラット フォームのプラグインは必要があります、通常、次のコンポーネントで構成されます。

- API は、プラグインを表すインターフェイスを使用して PCL
- iOS、Android、および Windows はクラス、インターフェイスの実装を持つライブラリです。

読み取り James Montemagno[ブログの投稿](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms/)Xamarin 用のプラグインを作成する手順について説明します。

お勧め、Xamarin.Forms をプラグインから直接参照しないようにします。
これにより、他の開発者が、プラグインを使用しようとしています。 バージョン競合の問題が作成することができます。 代わりに Xamarin または .NET アプリケーションが使用できるように、API をデザインしてください。

### <a name="publishing-nuget-packages"></a>NuGet パッケージの発行

NuGet パッケージが、 **nuspec**ファイル、プロジェクトのどの部分がパッケージに公開されるを定義する xml ファイルです。 **Nuspec**ファイルには、id、タイトル、作成者など、パッケージに関する情報も含まれています。

参照してください[NuGet のドキュメント](http://docs.nuget.org/create/creating-and-publishing-a-package)を作成して、NuGet パッケージを発行の詳細についてはします。


## <a name="related-links"></a>関連リンク

- [Xamarin.Forms の再利用可能なプラグインを作成します。](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms)
- [Xamarin (ビデオ) 用にプラグインを使用すると開発](https://university.xamarin.com/guestlectures/using-developing-plugins-for-xamarin)
