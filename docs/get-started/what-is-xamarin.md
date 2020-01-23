---
title: Xamarin とは
description: この記事では、Xamarin と関連ライブラリについて説明します。
ms.prod: xamarin
ms.assetid: 33C83E13-F3E5-17B4-6512-207F3D3C5AB6
ms.custom: video
author: profexorgeek
ms.author: jusjohns
ms.date: 09/16/2019
ms.openlocfilehash: 34763804e9833224721ea32f9c7e6200dd5faba7
ms.sourcegitcommit: ba83c107c87b015dbcc9db13964fe111a0573dca
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2020
ms.locfileid: "75607881"
---
# <a name="what-is-xamarin"></a>Xamarin とは

[iOS および Android での Xamarin アプリケーションの例の ![スクリーンショット](what-is-xamarin-images/xamarin-app-cropped.png)](what-is-xamarin-images/xamarin-app.png#lightbox)

Xamarin は、iOS、Android、Windows 向けの最新で高性能なアプリケーションを .NET で構築するためのオープンソースのプラットフォームです。 Xamarin は、基になるプラットフォームコードと共有コードの通信を管理する抽象化レイヤーです。 Xamarin は、メモリ割り当てやガベージコレクションなどの便利なを提供するマネージ環境で実行されます。

Xamarin を使用すると、開発者はプラットフォーム間でアプリケーションの90% の平均を共有できます。 このパターンを使用すると、開発者はすべてのビジネスロジックを1つの言語で記述でき (または既存のアプリケーションコードを再利用できます)、各プラットフォームでネイティブのパフォーマンス、ルックアンドフィールを実現できます。

Xamarin アプリケーションは、PC または Mac 上で記述し、ネイティブアプリケーションパッケージ (Android 上の**apk**ファイルなど)、または iOS 上の**ipa**ファイルとしてコンパイルできます。

> [!NOTE]
> 現在、iOS 用のアプリケーションをコンパイルして展開するには、MacOS コンピューターが必要です。 開発要件の詳細については、「[システム要件](~/cross-platform/get-started/requirements.md#macos-requirements)」を参照してください。

## <a name="who-xamarin-is-for"></a>Xamarin の対象ユーザー

Xamarin は、次のような目標を持つ開発者を対象としています。

- プラットフォーム間でコード、テスト、ビジネスロジックを共有します。
- Visual Studio を使用してC# 、でクロスプラットフォームアプリケーションを作成します。

## <a name="how-xamarin-works"></a>Xamarin のしくみ

![Xamarin アーキテクチャの図](what-is-xamarin-images/xamarin-architecture.png)

この図は、クロスプラットフォーム Xamarin アプリケーションの全体的なアーキテクチャを示しています。 Xamarin では、プラットフォームごとにネイティブ UI を作成し、プラットフォーム間C#で共有されるのビジネスロジックを記述できます。 ほとんどの場合、アプリケーションコードの80% は Xamarin を使用して共有できます。

Xamarin は、 **Mono**の上に構築されており、.net ECMA 標準に基づく .NET Framework のオープンソースバージョンです。 Mono は .NET Framework 自体とほぼ同じくらいに存在し、Linux、Unix、FreeBSD、macOS などのほとんどのプラットフォームで実行されます。 Mono 実行環境では、メモリの割り当て、ガベージコレクション、基になるプラットフォームとの相互運用性などのタスクが自動的に処理されます。

プラットフォーム固有のアーキテクチャの詳細については、「 [Xamarin Android](#xamarinandroid)と[xamarin. iOS](#xamarinios)」を参照してください。

### <a name="added-features"></a>追加された機能

Xamarin は、ネイティブプラットフォームの機能を統合し、次のような多数の機能を追加します。

1. **基になる sdk の完全なバインド**– Xamarin には、IOS と Android の両方で、基になるプラットフォーム sdk のほぼすべてのバインドが含まれています。 さらに、これらのバインディングは厳密に型指定されています。つまり、ナビゲーションや使用が簡単で、開発時に堅牢なコンパイル時の型チェックが行われます。 厳密に型指定されたバインディングによって、実行時エラーや品質の高いアプリケーションが減少します。
1. **目的-c、Java、C、およびC++相互運用**-Xamarin には、目的の c、java、c、およびC++ライブラリを直接呼び出す機能が用意されており、サードパーティ製のさまざまなコードを使用することができます。 この機能により、目標 C、Java、または C/C++で記述された既存の IOS および Android ライブラリを使用できるようになります。 さらに、Xamarin には、宣言型の構文を使用してネイティブの目的 C と Java のライブラリをバインドできるバインドプロジェクトが用意されています。
1. **最新の言語構成**– Xamarin アプリケーションは、 C#で記述されています。これは、動的言語機能、ラムダ、LINQ、並列プログラミング、ジェネリックなどの機能の構成要素など、目的の C と Java よりも大幅に改善された最新の言語です。
1. **堅牢な基底クラスライブラリ (BCL)** – Xamarin アプリケーションは、.net BCL を使用します。これは、強力な XML、データベース、シリアル化、IO、文字列、ネットワークのサポートなど、包括的で合理化された機能を持つクラスの大規模なコレクションです。 既存C#のコードをコンパイルしてアプリで使用することができます。これにより、BCL を超える機能を追加する数千のライブラリにアクセスできます。
1. **最新の統合開発環境 (IDE)** – Xamarin は、コードのオートコンプリート、洗練されたプロジェクトとソリューションの管理システム、包括的なプロジェクトテンプレートライブラリ、統合ソース管理などの機能を備えた最新の ide である Visual Studio を使用します。
1. **モバイルクロスプラットフォームサポート**– Xamarin では、IOS、Android、Windows の3つの主要なプラットフォームに対して、高度なクロスプラットフォームサポートを提供しています。 アプリケーションは、コードの最大90% を共有するように記述できます。また、Xamarin では、3つのすべてのプラットフォームで共通のリソースにアクセスするための統一された API を提供します。 共有コードを使用すると、開発コストと、モバイル開発者の製品化までの時間を大幅に削減できます。

### <a name="xamarinandroid"></a>Xamarin.Android

[![Xamarin. Android アーキテクチャの図](what-is-xamarin-images/android-architecture-cropped.png)](what-is-xamarin-images/android-architecture.png#lightbox)

Xamarin Android アプリケーションはからC#中間言語 (IL) にコンパイルされます。**中間言語 (IL)** は、アプリケーションの起動時にネイティブアセンブリにコンパイルされます。 Xamarin Android アプリケーションは、Mono 実行環境内で Android Runtime (アート) 仮想マシンとサイドバイサイドで実行されます。 Xamarin には、Android. * と Java. * 名前空間への .NET バインドが用意されています。 Mono 実行環境は、**マネージ呼び出し可能ラッパー (MCW)** を使用してこれらの名前空間を呼び出し、 **Android 呼び出し可能ラッパー (ACW)** をアートに提供して、両方の環境が相互にコードを呼び出すことができるようにします。

詳細については、「 [Xamarin のアーキテクチャ](~/android/internals/architecture.md)」を参照してください。

### <a name="xamarinios"></a>Xamarin.iOS

[![Xamarin. iOS アーキテクチャダイアグラム](what-is-xamarin-images/ios-architecture-cropped.png)](what-is-xamarin-images/ios-architecture.png#lightbox)

Xamarin iOS アプリケーションは、からC#ネイティブ ARM アセンブリコードに完全に事前にコンパイルされます **(AOT)** 。 Xamarin では、**セレクター**を使用して目的C#の c をマネージおよびC# **レジストラー**に公開し、マネージコードを目的の c に公開します。 セレクターとレジストラーは、総称して "バインド" と呼ばれ、 C# C との間の通信を許可します。

詳細については、「 [Xamarin. iOS アーキテクチャ](~/ios/internals/architecture.md)」を参照してください。

### <a name="xamarinessentials"></a>Xamarin.Essentials

Xamarin. Essentials は、ネイティブデバイス機能用のクロスプラットフォーム Api を提供するライブラリです。 Xamarin 自体と同様に、Xamarin. Essentials は、ネイティブ機能へのアクセスプロセスを簡略化する抽象化です。 Xamarin. Essentials で提供される機能の例には、次のようなものがあります。

- デバイス情報
- ファイル システム
- 加速度計
- ダイヤラー
- 音声合成
- 画面のロック

詳細については、「 [Xamarin. Essentials](~/essentials/index.md)」を参照してください。

### <a name="xamarinforms"></a>Xamarin.Forms

Xamarin. Forms はオープンソースの UI フレームワークです。 Xamarin を使用すると、開発者は1つの共有コードベースから iOS、Android、および Windows アプリケーションを作成できます。 Xamarin を使用すると、開発者はのC#分離コードを使用して XAML でユーザーインターフェイスを作成できます。 これらのユーザーインターフェイスは、各プラットフォームでパフォーマンスの高いネイティブコントロールとしてレンダリングされます。 Xamarin に用意されている機能の例を次に示します。

- XAML ユーザーインターフェイス言語
- データ バインド
- ジェスチャ
- 効果
- スタイル

詳細については、「 [Xamarin. フォーム](~/xamarin-forms/index.yml)」を参照してください。

## <a name="get-started"></a>作業開始

次のガイドは、Xamarin を使用して初めてのアプリを構築する際に役立ちます。

- [Xamarin を使ってみる](~/xamarin-forms/index.yml)
- [Xamarin Android を使ってみる](~/android/index.yml)
- [Xamarin を使ってみる](~/ios/index.yml)
- [Xamarin. Mac を使ってみる](~/mac/index.yml)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Series/Xamarin-101/What-is-Xamarin-1-of-11/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
