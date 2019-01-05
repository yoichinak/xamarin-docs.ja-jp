---
title: モバイル開発の概要
description: このドキュメントでは、モバイル開発の概要を示し、Xamarin とそのしくみ、および出力されるアプリケーションについて説明します。
ms.prod: xamarin
ms.assetid: 33C83E13-F3E5-17B4-6512-207F3D3C5AB6
author: asb3993
ms.author: amburns
ms.date: 03/28/2017
ms.openlocfilehash: cefcc7084b2abab4af61f07ef1f33a4f4c363f69
ms.sourcegitcommit: 6e84adf7358dc05f4d888ab2674de70d88214090
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/31/2018
ms.locfileid: "53815205"
---
# <a name="introduction-to-mobile-development"></a>モバイル開発の概要

IDE を起動して何かをまとめてスローし、ちょっとしたテストを行って App Store に公開するのと同じくらい、モバイル アプリケーションのビルドが簡単になります。すべてが半日で終わります。 さもなければ、事前のデザインを正確に行い、ユーザビリティをテストし、何千ものデバイスで QA テストを行い、ベータ ライフサイクルを十分に経て、何通りもの方法で配置することになるでしょう。

このドキュメントは、Xamarin プラットフォームを導入することを目的としています。 モバイル アプリケーションの設計からテストまでの構築*プロセス*の詳細については、「[Introduction to the Mobile Software Development Lifecycle](~/cross-platform/get-started/introduction-to-mobile-sdlc.md)」(モバイル ソフトウェア開発ライフサイクルの概要) ドキュメントを参照してください。

[システム要件](~/cross-platform/get-started/requirements.md#macos-requirements)を参照して、Xamarin をインストールできることを確認してください。

## <a name="introduction-to-xamarin"></a>Xamarin の概要

iOS および Android アプリケーションの構築方法について検討する場合、多くの人はネイティブ言語、Objective-C、Swift、Java しか選択肢にないと考えます。 しかし、ここ数年で、モバイル アプリケーションを構築するためのプラットフォームにまったく新しいエコシステムが生まれています。

Xamarin は iOS、Android、Windows Phone の 3 つのすべてのモバイル プラットフォームで動作する単一の言語 C#、クラス ライブラリ、そしてランタイムを提供する点で、この分野では独自です (Windows Phone のネイティブ言語は既に C# です)。それでいて、パフォーマンスが求められるゲームでも十分な威力を発揮するネイティブ (非解釈の) アプリケーションもコンパイルします。

これらのプラットフォームにはそれぞれ異なる機能セットが含まれています。また、ネイティブ コードにコンパイルされ、基になる Java サブシステムとスムーズに動作するネイティブ アプリケーションの記述に関して異なる機能を持ちます。 たとえば、一部のプラットフォームではアプリを HTML と JavaScript でしか構築できず、別のプラットフォームでは非常に低レベルで C/C++ コードしか許可されません。 一部のプラットフォームではネイティブ コントロール ツールキットさえも利用しません。

Xamarin はネイティブ プラットフォームの威力がすべて組み合わされていて、次のような多数の強力な機能を追加する点で独自です。

1.   **基になる SDK の完全なバインディング** – Xamarin には iOS と Android の両方の基になるプラットフォーム SDK のほぼ全体を対象としたバインディングが含まれています。 さらに、これらのバインディングは厳密に型指定されています。つまり、ナビゲーションや使用が簡単で、開発時に堅牢なコンパイル時の型チェックが行われます。 これはランタイム エラーの削減とアプリケーションの品質向上につながります。
1.   **Objective-C、Java、C、および C++ の相互運用** – Xamarin は Objective-C、Java、C、C++ ライブラリを直接呼び出す機能を提供しているため、既に作成されているサード パーティ製の豊富なコードを使用できます。 これにより、Objective-C、Java、C/C++ で記述された既存の iOS および Android のライブラリを活用できるようになります。 さらに、Xamarin は宣言型の構文を使用してネイティブの Objective-C および Java のライブラリを簡単にバインドできるバインド プロジェクトも提供しています。
1.   **最新の言語構造** – Xamarin アプリケーションは、現代的な言語である C# で記述されています。C# には、*動的言語機能*、*Lambda*、*LINQ* などの*機能の構造体*、*並列プログラミング*機能、高度な*ジェネリック*などの、Objective-C と Java に対する大幅な改善が含まれています。
1.   **卓越した基本クラス ライブラリ (BCL)** – Xamarin アプリケーションは .NET BCL を使用します。これはクラスの大規模なコレクションで、強力な XML、データベース、シリアル化、IO、文字列、ネットワーキングのサポートなどの包括的かつ簡素化された機能が含まれています。 さらに、既存の C# コードはアプリケーションで使用するためにコンパイルすることができます。これにより、膨大な数のライブラリにアクセスして BCL でまだ対応していないことも実行できるようになります。
1.   **最新の統合開発環境 (IDE)** – Xamarin は Mac OS X では Visual Studio for Mac を、Windows では Visual Studio を使用します。 どちらも最新の IDE で、コードのオート コンプリート、高度なプロジェクトとソリューションの管理システム、包括的なプロジェクト テンプレート ライブラリ、統合ソース管理などの機能が含まれます。
1.   **モバイルのクロス プラットフォーム サポート** – Xamarin は iOS、Android、Windows Phone の 3 つの主要なモバイル プラットフォーム向けに高度なクロス プラット フォーム サポートを提供しています。 記述したアプリケーションは最大 90% のコードを共有することができ、Xamarin.Mobile ライブラリでは 3 つのすべてのプラットフォームに共通するリソースにアクセスするための Unified API を提供しています。 これにより、特に一般的な 3 つのモバイル プラットフォームを対象とするモバイル開発者の開発コストと市場投入までの時間の両方が大幅に削減されます。


Xamarin には強力で包括的な機能セットがあるため、クロス プラットフォームのモバイル アプリケーションの開発に最新の言語とプラットフォームを使用したいと考えるアプリケーション開発者の要望に応えることができます。


> [!NOTE]
> この「作業の開始」のシリーズでは、iOS および Android アプリケーションの構築の開始に焦点を当てます。 Microsoft は、タブレットおよびデスクトップ用の[ユニバーサル Windows プラットフォーム (UWP) の開発](https://docs.microsoft.com/windows/uwp/develop/)に関する情報を提供しています。 (Windows 用の UWP アプリを含む) Xamarin を使用したクロスプラットフォーム開発の詳細については、「[Building Cross-Platform Applications](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)」(クロスプラットフォーム アプリケーションの構築) ガイドを参照してください。



## <a name="how-does-xamarin-work"></a>Xamarin のしくみ

Xamarin は、Xamarin.iOS および Xamarin.Android という 2 つの製品を提供しています。 どちらも、公開済みの .NET ECMA 規格に基づいたオープン ソース版の .NET Framework である *Mono* の上に構築されています。 Mono は .NET Framework 自体とほぼ同じで、Linux、Unix、FreeBSD、Mac OS X などの想定されるほぼすべてのプラットフォームで実行されます。

iOS では、Xamarin の *Ahead-of-Time* (*AOT*) コンパイラによって Xamarin.iOS アプリケーションが直接ネイティブの ARM アセンブリ コードにコンパイルされます。 Android では、Xamarin のコンパイラによって*中間言語* (*IL*) にコンパイルされてから、アプリケーションの起動時にネイティブのアセンブリに *Just-in-Time* (*JIT*) コンパイルされます。

どちらの場合も、Xamarin アプリケーションはランタイムを活用するため、メモリ割り当て、ガベージ コレクション、基になるプラットフォームの相互運用などが自動的に処理されます。



### <a name="xamariniosdll-and-monoandroiddll"></a>Xamarin.iOS.dll と Mono.Android.dll

Xamarin アプリケーションは、Xamarin モバイル プロファイルとして知られる .NET BCL のサブセットに対して構築されます。 このプロファイルはモバイル アプリケーションに特化して作成されており、MonoTouch.dll と Mono.Android.dll (それぞれ iOS と Android 向け) にパッケージ化されています。 これは Silverlight (および Moonlight) アプリケーションが Silverlight/Moonlight .NET プロファイルに対して構築される場合と非常に似ています。 実際、Xamarin モバイル プロファイルは BCL クラス群が追加されている Silverlight 4.0 プロファイルと同等です。

使用可能なアセンブリとクラスの完全な一覧については、「[Xamarin.iOS アセンブリ一覧](~/cross-platform/internals/available-assemblies.md?context=xamarin/ios)」と「[Xamarin.Android アセンブリ一覧](~/cross-platform/internals/available-assemblies.md?context=xamarin/android)」を参照してください

これらの .dll には、BCL に加えて、基になる SDK API を C# から直接呼び出せるようにする iOS SDK と Android SDK のほぼすべてに対するラッパーが含まれています。



### <a name="application-output"></a>アプリケーションの出力

Xamarin アプリケーションがコンパイルされると、iOS には .app ファイルの、Android には .apk ファイルのアプリケーション パッケージが生成されます。 これらのファイルはプラットフォームの既定の IDE で構築されたアプリケーション パッケージと区別がつかず、まったく同じ方法で展開することができます。



## <a name="getting-started"></a>作業の開始

Xamarin のしくみの概要について説明したので、さらに詳しい話をしましょう。

次の手順は、以下のガイドのいずれかを使用したアプリ構築の開始方法です。

* [**Hello, iOS**](~/ios/get-started/hello-ios/index.md)

![](introduction-to-mobile-development-images/ios.png "Hello, iOS")


* [**Android へようこそ**](~/android/get-started/hello-android/index.md)

![](introduction-to-mobile-development-images/android.png "Hello, Android")


* [**Xamarin.Forms の概要**](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)





## <a name="summary"></a>まとめ

このドキュメントは、Xamarin プラットフォームの紹介にすぎません。 本当に面白くなるのは、初めてアプリを使い始めてからです。 「[Hello, iOS](~/ios/get-started/hello-ios/index.md)」、「[Android へようこそ](~/android/get-started/hello-android/index.md)」、「[Introduction to Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)」(Xamarin.Forms の概要) を参照して、始めましょう。


## <a name="related-links"></a>関連リンク

- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [Hello Android](~/android/get-started/hello-android/index.md)
