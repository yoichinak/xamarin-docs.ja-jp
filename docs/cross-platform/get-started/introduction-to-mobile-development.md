---
title: モバイル開発の概要
description: このドキュメントでは、モバイル開発の概要を示し、Xamarin とそのしくみ、および出力されるアプリケーションについて説明します。
ms.prod: xamarin
ms.assetid: 33C83E13-F3E5-17B4-6512-207F3D3C5AB6
author: asb3993
ms.author: amburns
ms.date: 03/28/2017
ms.openlocfilehash: 3b75ef6b0937248a43aa2e2ff3fc13a578d25d3c
ms.sourcegitcommit: 5f48dbd99a33acbb376a1703485c7b659df2111b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/28/2019
ms.locfileid: "67467851"
---
# <a name="introduction-to-mobile-development"></a>モバイル開発の概要

モバイル アプリの構築は、IDE を起動し、アプリの記述とテストを行い、App Store に公開すれば済んでしまうほど簡単に行えます。すべてが半日で終わります。 さもなければ、事前のデザインを正確に行い、ユーザビリティをテストし、何千ものデバイスで QA テストを行い、ベータ ライフサイクルを十分に経て、何通りもの方法で配置することになるでしょう。

このドキュメントでは、Xamarin プラットフォームの概要を説明します。 モバイル アプリケーションを構築する "*プロセス*" を設計からテストまで詳しく学習する場合は、「[モバイル ソフトウェア開発ライフサイクルの概要](~/cross-platform/get-started/introduction-to-mobile-sdlc.md)」をご覧ください。

[システム要件](~/cross-platform/get-started/requirements.md#macos-requirements)を参照してご自分のシステムを確認してください。

## <a name="introduction-to-xamarin"></a>Xamarin の概要

iOS および Android アプリケーションの構築方法について検討する場合、多くの人はネイティブ言語、Objective-C、Swift、Java しか選択肢にないと考えます。 しかし、ここ数年で、モバイル アプリケーションを構築するためのプラットフォームにまったく新しいエコシステムが生まれています。

Xamarin は iOS、Android、Windows Phone の 3 つのすべてのモバイル プラットフォームで動作する単一の言語 C#、クラス ライブラリ、そしてランタイムを提供する点で、この分野では独自です (Windows Phone のネイティブ言語は既に C# です)。それでいて、パフォーマンスが求められるゲームでも十分な威力を発揮するネイティブ (非解釈の) アプリケーションもコンパイルします。

これらのプラットフォームにはそれぞれ異なる機能セットが含まれています。また、ネイティブ コードにコンパイルされ、基になる Java サブシステムとスムーズに動作するネイティブ アプリケーションの記述に関して異なる機能を持ちます。 たとえば、あるプラットフォームではアプリを HTML と JavaScript でしか構築できず、別の低レベルなものでは C/C++ コードしか許可されません。 一部のプラットフォームではネイティブ コントロール ツールキットさえも利用しません。

Xamarin はネイティブ プラットフォームの威力がすべて組み合わされていて、次のような多数の強力な機能を追加する点で独自です。

1.   **基になる SDK の完全なバインディング** – Xamarin には iOS と Android の両方の基になるプラットフォーム SDK のほぼ全体を対象としたバインディングが含まれています。 さらに、これらのバインディングは厳密に型指定されています。つまり、ナビゲーションや使用が簡単で、開発時に堅牢なコンパイル時の型チェックが行われます。 これはランタイム エラーの削減とアプリの品質向上につながります。
1.   **Objective-C、Java、C、および C++ の相互運用** – Xamarin は Objective-C、Java、C、C++ ライブラリを直接呼び出す機能を提供しているため、既に作成されているサード パーティ製の豊富なコードを使用できます。 これにより、Objective-C、Java、C/C++ で記述された既存の iOS および Android のライブラリを活用できるようになります。 さらに、Xamarin は宣言型の構文を使用してネイティブの Objective-C および Java のライブラリを簡単にバインドできるバインド プロジェクトも提供しています。
1.   **最新の言語構造** – Xamarin アプリケーションは、現代的な言語である C# で記述されています。これには、*動的言語機能や、*Lambda、*LINQ などの "*関数コンストラクト*"、"*並列プログラミング*" 機能、高度な *ジェネリックなど、Objective-C と Java を超える大幅な改善が含まれています。
1.   **卓越した基本クラス ライブラリ (BCL)** – Xamarin アプリケーションでは .NET BCL が使用されます。これはクラスの大規模なコレクションで、強力な XML、データベース、シリアル化、IO、文字列、ネットワークのサポートなどの包括的かつ簡素化された機能が含まれています。 既存の C# コードをコンパイルしてアプリで使用することができます。これにより、何千個ものライブラリにアクセスして、BCL では対応していないことを実行できるようになります。
1.   **最新の統合開発環境 (IDE)** – Xamarin では、macOS では Visual Studio for Mac が、Windows では Visual Studio が使用されます。 どちらも最新の IDE で、コードのオート コンプリート、高度なプロジェクトとソリューションの管理システム、包括的なプロジェクト テンプレート ライブラリ、統合ソース管理などの機能が含まれます。
1.   **モバイルのクロス プラットフォーム サポート** – Xamarin は iOS、Android、Windows Phone の 3 つの主要なモバイル プラットフォーム向けに高度なクロス プラット フォーム サポートを提供しています。 記述したアプリケーションは最大 90% のコードを共有することができ、Xamarin.Mobile ライブラリでは 3 つのすべてのプラットフォームに共通するリソースにアクセスするための Unified API を提供しています。 これにより、特に一般的な 3 つのモバイル プラットフォームを対象とするモバイル開発者の開発コストと市場投入までの時間の両方が大幅に削減されます。

Xamarin には強力で包括的な機能セットがあるため、クロス プラットフォームのモバイル アプリケーションの開発に最新の言語とプラットフォームを使用したいと考えるアプリケーション開発者の要望に応えることができます。

> [!NOTE]
> この「作業の開始」のシリーズでは、iOS および Android アプリケーションの構築の開始に焦点を当てます。 Microsoft は、タブレットおよびデスクトップ用の[ユニバーサル Windows プラットフォーム (UWP) の開発](https://docs.microsoft.com/windows/uwp/develop/)に関する情報を提供しています。 (Windows 用の UWP アプリを含む) Xamarin を使用したクロスプラットフォーム開発の詳細については、「[Building Cross-Platform Applications](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)」(クロスプラットフォーム アプリケーションの構築) ガイドを参照してください。

## <a name="how-does-xamarin-work"></a>Xamarin のしくみ

Xamarin は、Xamarin.iOS および Xamarin.Android という 2 つの製品を提供しています。 どちらも、公開済みの .NET ECMA 規格に基づいたオープン ソース版の .NET Framework である *Mono* の上に構築されています。 Mono は .NET Framework 自体と同じくらい前から存在していて、Linux、Unix、FreeBSD、macOS などの想定されるほぼすべてのプラットフォーム上で実行できます。

iOS では、Xamarin の *Ahead-of-Time* (*AOT*) コンパイラによって Xamarin.iOS アプリケーションが直接ネイティブの ARM アセンブリ コードにコンパイルされます。 Android では、Xamarin のコンパイラによって*中間言語* (*IL*) にコンパイルされてから、アプリケーションの起動時にネイティブのアセンブリに *Just-in-Time* (*JIT*) コンパイルされます。

どちらの場合も、Xamarin アプリケーションはランタイムを活用するため、メモリ割り当て、ガベージ コレクション、基になるプラットフォームの相互運用などが自動的に処理されます。

### <a name="xamariniosdll-and-monoandroiddll"></a>Xamarin.iOS.dll と Mono.Android.dll

Xamarin アプリケーションは、Xamarin モバイル プロファイルとして知られる .NET BCL のサブセットに対して構築されます。 このプロファイルはモバイル アプリケーションに特化して作成されており、MonoTouch.dll と Mono.Android.dll (それぞれ iOS と Android 向け) にパッケージ化されています。 これは Silverlight (および Moonlight) アプリケーションが Silverlight/Moonlight .NET プロファイルに対して構築される場合と非常に似ています。 実際、Xamarin モバイル プロファイルは BCL クラス群が追加されている Silverlight 4.0 プロファイルと同等です。

使用可能なアセンブリとクラスの完全な一覧については、「[Xamarin.iOS アセンブリ一覧](~/cross-platform/internals/available-assemblies.md?context=xamarin/ios)」と「[Xamarin.Android アセンブリ一覧](~/cross-platform/internals/available-assemblies.md?context=xamarin/android)」を参照してください

これらの .dll には、BCL に加えて、基になる SDK API を C# から直接呼び出せるようにする iOS SDK と Android SDK のほぼすべてに対するラッパーが含まれています。

### <a name="application-output"></a>アプリケーションの出力

Xamarin アプリケーションがコンパイルされると、iOS には .app ファイルの、Android には .apk ファイルのアプリケーション パッケージが生成されます。 これらのファイルはプラットフォームの既定の IDE で構築されたアプリケーション パッケージと区別がつかず、まったく同じ方法で展開することができます。

## <a name="next-steps"></a>次の手順

Xamarin の動作方法についてある程度わかったので、次に以下のガイドのいずれかを使用してアプリの構築を開始します。

- [**Xamarin.Forms の概要**](~/get-started/index.yml)
- [**Xamarin.iOS の概要**](~/ios/get-started/hello-ios/index.md)
- [**Xamarin.Android の概要**](~/android/get-started/hello-android/index.md)
- [**Xamarin.Mac の概要**](~/mac/get-started/hello-mac.md)

