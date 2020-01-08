---
title: 新機能Xamarinとは?
description: この記事では、Xamarin と関連するライブラリについて説明します。
ms.prod: xamarin
ms.assetid: 33C83E13-F3E5-17B4-6512-207F3D3C5AB6
ms.custom: video
author: profexorgeek
ms.author: jusjohns
ms.date: 09/16/2019
no-loc:
- Xamarin
- Xamarin.Forms
- Xamarin.Android
- Xamarin.Essentials
- Xamarin.iOS
- Xamarin.Mac
ms.openlocfilehash: d4abb59cac117314239c669df454a786a3720ff5
ms.sourcegitcommit: 6f09bc2b760e76a61a854f55d6a87c4f421ac6c8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/02/2020
ms.locfileid: "75607881"
---
# <a name="what-is-opno-locxamarin"></a>新機能Xamarinとは?

[![スクリーンショット (例) [!ファンド.IOS および Android でのアプリケーションなし (Xamarin)](what-is-xamarin-images/xamarin-app-cropped.png)](what-is-xamarin-images/xamarin-app.png#lightbox)

Xamarin は、iOS、Android、Windows 用の最新で高性能なアプリケーションを .NET で構築するためのオープンソースのプラットフォームです。 Xamarin は、基になるプラットフォームコードと共有コードの通信を管理する抽象化レイヤーです。 Xamarin は、メモリの割り当てやガベージコレクションなどの便利なを提供するマネージ環境で実行されます。

Xamarin を使用すると、開発者はプラットフォーム間でアプリケーションの90% の平均を共有できます。 このパターンを使用すると、開発者はすべてのビジネスロジックを1つの言語で記述でき (または既存のアプリケーションコードを再利用できます)、各プラットフォームでネイティブのパフォーマンス、ルックアンドフィールを実現できます。

Xamarin アプリケーションは、PC または Mac 上で記述し、ネイティブアプリケーションパッケージ (Android 上の**apk**ファイルなど)、または iOS 上の**ipa**ファイルとしてコンパイルできます。

> [!NOTE]
> 現在、iOS 用のアプリケーションをコンパイルして展開するには、MacOS コンピューターが必要です。 開発要件の詳細については、「[システム要件](~/cross-platform/get-started/requirements.md#macos-requirements)」を参照してください。

## <a name="who-opno-locxamarin-is-for"></a>Xamarin の対象

Xamarin は、次の目標を持つ開発者を対象としています。

- プラットフォーム間でコード、テスト、ビジネスロジックを共有します。
- Visual Studio を使用してC# 、でクロスプラットフォームアプリケーションを作成します。

## <a name="how-opno-locxamarin-works"></a>Xamarin のしくみ

![[!ファンド.NO LOC (Xamarin)] アーキテクチャ](what-is-xamarin-images/xamarin-architecture.png)

この図は、クロスプラットフォームの Xamarin アプリケーションの全体的なアーキテクチャを示しています。 Xamarin を使用すると、各プラットフォームでネイティブ UI を作成し、 C#プラットフォーム間で共有されるのビジネスロジックを記述できます。 ほとんどの場合、アプリケーションコードの80% は Xamarinを使用して共有できます。

Xamarin は、.NET ECMA 標準に基づく .NET Framework のオープンソースバージョンである**Mono**の上に構築されています。 Mono は .NET Framework 自体とほぼ同じくらいに存在し、Linux、Unix、FreeBSD、macOS などのほとんどのプラットフォームで実行されます。 Mono 実行環境では、メモリの割り当て、ガベージコレクション、基になるプラットフォームとの相互運用性などのタスクが自動的に処理されます。

プラットフォーム固有のアーキテクチャの詳細については、「Xamarin」を参照してください[。Android](#xamarinandroid)と[Xamarin。](#xamarinios)

### <a name="added-features"></a>追加された機能

Xamarin は、ネイティブプラットフォームの機能を統合し、次のような多数の機能を追加します。

1. **基になる sdk の完全なバインド**– Xamarin は、IOS と Android の両方で、基になるプラットフォーム sdk のほぼすべてのバインドを含みます。 さらに、これらのバインディングは厳密に型指定されています。つまり、ナビゲーションや使用が簡単で、開発時に堅牢なコンパイル時の型チェックが行われます。 厳密に型指定されたバインディングによって、実行時エラーや品質の高いアプリケーションが減少します。
1. **目的-c、Java、C、およびC++ Interop** – Xamarin は、目的の c、java、c、およびC++ライブラリを直接呼び出す機能を提供しており、サードパーティ製の幅広いコードを使用する機能を提供します。 この機能により、目標 C、Java、または C/C++で記述された既存の IOS および Android ライブラリを使用できるようになります。 また、Xamarin には、宣言型の構文を使用してネイティブの目的 C と Java のライブラリをバインドできるバインドプロジェクトが用意されています。
1. **最新の言語構築**– Xamarin アプリケーションは、 C#で記述されています。これは、動的言語機能、ラムダ、LINQ、並列プログラミング、ジェネリックなどの機能の構成要素など、目的の C と Java よりも大幅に改善された最新の言語です。
1. **堅牢な基底クラスライブラリ (BCL)** – Xamarin アプリケーションは、強力な XML、データベース、シリアル化、IO、文字列、ネットワークのサポートなどの包括的で合理化された機能を持つクラスの大規模なコレクションである .net BCL を使用します。 既存C#のコードをコンパイルしてアプリで使用することができます。これにより、BCL を超える機能を追加する数千のライブラリにアクセスできます。
1. **最新の統合開発環境 (IDE)** – Xamarin は、コードのオートコンプリート、洗練されたプロジェクトとソリューションの管理システム、包括的なプロジェクトテンプレートライブラリ、統合ソース管理などの機能を備えた最新の ide である Visual Studio を使用します。
1. **モバイルクロスプラットフォームサポート**– Xamarin は、IOS、Android、Windows の3つの主要なプラットフォームに対して、高度なクロスプラットフォームサポートを提供します。 アプリケーションは、コードの最大90% と Xamarinを共有するように記述できます。Essentials は、3つのすべてのプラットフォームで共通のリソースにアクセスするための統一された API を提供します。 共有コードを使用すると、開発コストと、モバイル開発者の製品化までの時間を大幅に削減できます。

### <a name="opno-locxamarinandroid"></a>Xamarin。Android

[![[!ファンド.なし (Xamarin)]。Android アーキテクチャの図](what-is-xamarin-images/android-architecture-cropped.png)](what-is-xamarin-images/android-architecture.png#lightbox)

Xamarin。Android アプリケーションはからC# **中間言語 (IL)** にコンパイルされます。これは、アプリケーションの起動時に**just-in-time (JIT) で**ネイティブアセンブリにコンパイルされます。 Xamarin。Android アプリケーションは、Mono 実行環境内で Android Runtime (アート) 仮想マシンとサイドバイサイドで実行されます。 Xamarin には、Android. * と Java. * 名前空間への .NET バインドが用意されています。 Mono 実行環境は、**マネージ呼び出し可能ラッパー (MCW)** を使用してこれらの名前空間を呼び出し、 **Android 呼び出し可能ラッパー (ACW)** をアートに提供して、両方の環境が相互にコードを呼び出すことができるようにします。

詳細については、「Xamarin」を参照してください[。Android アーキテクチャ](~/android/internals/architecture.md)。

### <a name="opno-locxamarinios"></a>Xamarin. iOS

[![[!ファンド.NO LOC (Xamarin)]。 iOS アーキテクチャダイアグラム](what-is-xamarin-images/ios-architecture-cropped.png)](what-is-xamarin-images/ios-architecture.png#lightbox)

Xamarin。 iOS アプリケーションは、からC#ネイティブ ARM アセンブリコードに完全に事前にコンパイルされます **(AOT)** 。 Xamarin は**セレクター**を使用して、目標 c C#をマネージコードと**レジストラー**に公開し、マネージC#コードを目的の c に公開します。 セレクターとレジストラーは、総称して "バインド" と呼ばれ、 C# C との間の通信を許可します。

詳細については、「Xamarin」を参照してください[。](~/ios/internals/architecture.md)

### <a name="opno-locxamarinessentials"></a>Xamarin。基本

Xamarin。Essentials は、ネイティブデバイス機能用のクロスプラットフォーム Api を提供するライブラリです。 Xamarin 自体と同様に、Xamarinます。Essentials は、ネイティブ機能へのアクセスプロセスを簡略化する抽象化です。 Xamarinによって提供される機能の例をいくつか紹介します。要点は次のとおりです。

- デバイス情報
- ファイル システム
- 加速度計
- ダイヤラー
- 音声合成
- 画面のロック

詳細については、「Xamarin」を参照してください[。要点](~/essentials/index.md)。

### <a name="opno-locxamarinforms"></a>Xamarin。形態

Xamarin。フォームは、オープンソースの UI フレームワークです。 Xamarin。フォームを使用すると、開発者は1つの共有コードベースから iOS、Android、および Windows アプリケーションをビルドできます。 Xamarin。フォームを使用すると、開発者はのC#分離コードを使用して XAML でユーザーインターフェイスを作成できます。 これらのユーザーインターフェイスは、各プラットフォームでパフォーマンスの高いネイティブコントロールとしてレンダリングされます。 Xamarinによって提供される機能の例をいくつか紹介します。フォームには次のものがあります。

- XAML ユーザーインターフェイス言語
- データ バインド
- ジェスチャ
- 効果
- スタイル

詳細については、「Xamarin」を参照してください[。フォーム](~/xamarin-forms/index.yml)。

## <a name="get-started"></a>作業開始

次のガイドでは、Xamarinを使用して初めてのアプリを作成する方法について説明します。

- [Xamarinの使用を開始します。形態](~/xamarin-forms/index.yml)
- [Xamarinの使用を開始します。Android](~/android/index.yml)
- [Xamarinを使ってみる](~/ios/index.yml)
- [Xamarinの使用を開始します。Mac](~/mac/index.yml)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Series/Xamarin-101/What-is-Xamarin-1-of-11/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
