---
title: Xamarin とは
description: この記事では、Xamarin プラットフォームと関連ライブラリについて説明します。
ms.prod: xamarin
ms.assetid: 33C83E13-F3E5-17B4-6512-207F3D3C5AB6
ms.custom: video
author: profexorgeek
ms.author: jusjohns
ms.date: 05/28/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 708a310ea015f9e678d534898fde18abc3848120
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84198328"
---
# <a name="what-is-xamarin"></a>Xamarin とは

[![iOS および Android での Xamarin サンプル アプリケーションのスクリーンショット](what-is-xamarin-images/xamarin-app-cropped.png)](what-is-xamarin-images/xamarin-app.png#lightbox)

Xamarin は、.NET を使用して、iOS、Android、Windows 向けの最新で高性能なアプリケーションをビルドするためのオープンソースのプラットフォームです。 Xamarin は、基になるプラットフォーム コードと共有コードの通信を管理する抽象化レイヤーです。 Xamarin は、メモリ割り当てやガベージ コレクションなどの利便性を提供するマネージド環境で実行されます。

Xamarin を使用すると、開発者はプラットフォーム間でアプリケーションの 90% (平均) を共有できます。 このパターンにより、開発者はすべてのビジネス ロジックを 1 つの言語で記述 (または既存のアプリケーション コードを再利用) して、各プラットフォームでネイティブのパフォーマンスとルック アンド フィールを実現することができます。

Xamarin アプリケーションを PC または Mac で記述し、ネイティブ アプリケーション パッケージ (Android 上の **.apk** ファイルや、iOS 上の **.ipa** ファイルなど) にコンパイルできます。

> [!NOTE]
> 現在、iOS 用のアプリケーションをコンパイルして展開するには、MacOS マシンが必要です。 開発要件の詳細については、[システム要件](~/cross-platform/get-started/requirements.md#macos-requirements)を参照してください。

## <a name="who-xamarin-is-for"></a>Xamarin の対象者

Xamarin は、次のような目標を持つ開発者を対象としています。

- プラットフォーム間でコード、テスト、ビジネス ロジックを共有する。
- Visual Studio を使用して C# でクロスプラットフォーム アプリケーションを作成する。

## <a name="how-xamarin-works"></a>Xamarin のしくみ

![Xamarin アーキテクチャの図](what-is-xamarin-images/xamarin-architecture.png)

この図は、クロスプラットフォーム Xamarin アプリケーションのアーキテクチャ全体を示しています。 Xamarin を使用すると、プラットフォームごとにネイティブ UI を作成し、プラットフォーム間で共有されるビジネス ロジックを C# で記述できます。 ほとんどの場合、アプリケーション コードの 80% は Xamarin を使用して共有できます。

Xamarin は .NET 上に構築されています。このため、メモリの割り当て、ガベージ コレクション、基になるプラットフォームとの相互運用性などのタスクが自動的に処理されます。

プラットフォーム固有のアーキテクチャの詳細については、「[Xamarin.Android](#xamarinandroid)」および「[Xamarin.iOS](#xamarinios)」を参照してください。

### <a name="added-features"></a>追加された機能

Xamarin には、ネイティブ プラットフォームの機能が組み合わされたうえ、次のような機能が多数追加されています。

1. **基になる SDK の完全なバインディング** – Xamarin には iOS と Android の両方の基になるプラットフォーム SDK のほぼ全体を対象としたバインディングが含まれています。 さらに、これらのバインディングは厳密に型指定されています。つまり、ナビゲーションや使用が簡単で、開発時に堅牢なコンパイル時の型チェックが行われます。 厳密に型指定されたバインディングは、ランタイム エラーの削減とアプリケーションの品質向上につながります。
1. **Objective-C、Java、C、および C++ の相互運用** – Xamarin は Objective-C、Java、C、C++ ライブラリを直接呼び出す機能を提供しているため、サード パーティ製の豊富なコードを使用できます。 この機能により、Objective-C、Java、C/C++ で記述された既存の iOS および Android のライブラリを使用できるようになります。 さらに、Xamarin は宣言型の構文を使用してネイティブの Objective-C および Java のライブラリをバインドできるバインド プロジェクトも提供しています。
1. **最新の言語構造** – Xamarin アプリケーションは、現代的な言語である C# で記述されています。これには、動的言語機能や、ラムダ、LINQ などの関数コンストラクト、並列プログラミング機能、ジェネリックなど、Objective-C と Java を超える大幅な改善が含まれています。
1. **堅牢な基本クラス ライブラリ (BCL)** – Xamarin アプリケーションでは .NET BCL が使用されます。これはクラスの大規模なコレクションで、強力な XML、データベース、シリアル化、IO、文字列、ネットワークのサポートなどの包括的かつ簡素化された機能が含まれています。 既存の C# コードをコンパイルしてアプリで使用することができます。これにより、BCL を超える機能を追加する何千個ものライブラリにアクセスできるようになります。
1. **最新の統合開発環境 (IDE)** – Xamarin では最新の IDE である Visual Studio が使用されています。これには、コードのオート コンプリート、高度なプロジェクトとソリューションの管理システム、包括的なプロジェクト テンプレート ライブラリ、統合ソース管理などの機能が含まれます。
1. **モバイルのクロス プラットフォーム サポート** – Xamarin には、iOS、Android、Windows という 3 つの主要なプラットフォーム向けの高度なクロス プラット フォーム サポートが用意されています。 記述したアプリケーションは最大 90% のコードを共有することができ、Xamarin.Essentials では、3 つのすべてのプラットフォームに共通するリソースにアクセスするための Unified API が提供されます。 共有コードにより、モバイル開発者の開発コストと市場投入までの時間の両方が大幅に削減されます。

### <a name="xamarinandroid"></a>Xamarin.Android

[![Xamarin.Android アーキテクチャ ダイアグラム](what-is-xamarin-images/android-architecture-cropped.png)](what-is-xamarin-images/android-architecture.png#lightbox)

Xamarin.Android アプリケーションでは、アプリケーションの起動時に、C# が **中間言語 (IL)** にコンパイルされてから、ネイティブのアセンブリに **Just-in-Time (JIT)** コンパイルされます。 Xamarin.Android アプリケーションは、Mono 実行環境内で Android Runtime (ART) 仮想マシンとサイド バイ サイドで実行されます。 Xamarin には、Android.* と Java.* 名前空間への .NET バインドが用意されています。 Mono 実行環境では、**Managed Callable Wrappers (MCW)** 経由でこれらの名前空間を呼び出し、**Android Callable Wrappers (ACW)** を ART に提供して、両方の環境で相互にコードを呼び出せるようにします。

詳細については、[Xamarin.Android のアーキテクチャ](~/android/internals/architecture.md)に関するページをご覧ください。

### <a name="xamarinios"></a>Xamarin.iOS

[![Xamarin.iOS アーキテクチャ ダイアグラム](what-is-xamarin-images/ios-architecture-cropped.png)](what-is-xamarin-images/ios-architecture.png#lightbox)

Xamarin.iOS アプリケーションは、C# からネイティブ ARM アセンブリ コードに完全に **Ahead-of-Time (AOT)** コンパイルされます。 Xamarin では、**セレクター**を使用して Objective-C がマネージド C# に公開され、**レジストラー**を使用してマネージド C# コードが Objective-C に公開されます。 セレクターとレジストラーは、総称して "バインド" と呼ばれ、Objective-C と C# 間の通信を可能にします。

詳細については、[Xamarin.iOS のアーキテクチャ](~/ios/internals/architecture.md)に関するページをご覧ください。

### Xamarin.Essentials

Xamarin.Essentials は、ネイティブ デバイスの機能にクロスプラットフォーム API を提供するライブラリです。 Xamarin 自体と同様に、Xamarin.Essentials は、ネイティブ機能へのアクセス プロセスを簡略化する抽象化です。 以下に、Xamarin.Essentials で提供される機能の例の一部を示します。

- デバイス情報
- ファイル システム
- 加速度計
- ダイヤラー
- 音声合成
- 画面のロック

詳細については、「[Xamarin.Essentials](~/essentials/index.md)」を参照してください。

### Xamarin.Forms

Xamarin.Forms は、オープンソースの UI フレームワークです。 Xamarin.Forms を使用すると、開発者は 1 つの共有コードベースから Xamarin.iOS、Xamarin.Android、Windows のアプリケーションを作成できます。 Xamarin.Forms を使用すると、開発者は C# でコードビハインドを使用して、ユーザー インターフェイスを XAML で作成できます。 これらのユーザー インターフェイスは、各プラットフォームでパフォーマンスの高いネイティブ コントロールとしてレンダリングされます。 Xamarin.Forms で提供される機能の例には、次のようなものがあります。

- XAML ユーザー インターフェイス言語
- データ バインド
- ジェスチャ
- エフェクト
- スタイル

詳細については、「[Xamarin.Forms](~/xamarin-forms/index.yml)」を参照してください。

## <a name="get-started"></a>作業開始

次のガイドは、Xamarin を使用して初めてのアプリをビルドする際に役立ちます。

- [Xamarin.Forms の概要](~/xamarin-forms/index.yml)
- [Xamarin.Android の概要](~/android/index.yml)
- [Xamarin.iOS の概要](~/ios/index.yml)
- [Xamarin.Mac の概要](~/mac/index.yml)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Series/Xamarin-101/What-is-Xamarin-1-of-11/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
