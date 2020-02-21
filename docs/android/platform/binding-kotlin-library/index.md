---
title: Android Kotlin ライブラリをバインドする
description: このドキュメントでは、Kotlin C#コードへのバインドを作成して、Xamarin Android アプリケーションでネイティブライブラリを使用できるようにする方法について説明します。
ms.prod: xamarin
ms.assetid: AB03A6C4-5A5A-4EAD-AD51-D887B20A3551
ms.technology: xamarin-android
author: alexeystrakh
ms.author: alstrakh
ms.date: 02/11/2020
ms.openlocfilehash: ec7d154b0d7fcb055bd398089e142fe8b1d9f60e
ms.sourcegitcommit: b751605179bef8eee2df92cb484011a7dceb6fda
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/20/2020
ms.locfileid: "77497966"
---
# <a name="bind-android-kotlin-libraries"></a>Android Kotlin ライブラリをバインドする

Android プラットフォームとそのネイティブ言語およびツールは常に進化しており、最新のサービスを使用して開発されたサードパーティ製のライブラリが多数あります。 コードとコンポーネントの再利用を最大化することは、クロスプラットフォーム開発の主な目標の1つです。 Kotlin で構築されたコンポーネントを再利用する機能は、開発者にとって多くの開発者が成長し続けているため、Xamarin 開発者にとってますます重要になっています。 通常の[Java](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/)ライブラリをバインドするプロセスについては、既に理解している場合があります。 [Kotlin ライブラリをバインドする](walkthrough.md)プロセスを説明する追加のドキュメントが提供されるようになりました。そのため、Xamarin アプリケーションでも同じ方法で使用できます。 このドキュメントの目的は、Xamarin の Kotlin Binding を作成するための高レベルなアプローチについて説明することです。

## <a name="high-level-approach"></a>高レベルのアプローチ

Xamarin を使用すると、任意のサードパーティ製のネイティブライブラリを、Xamarin アプリケーションで使用できるようにバインドできます。 Kotlin は新しい言語であり、この言語でビルドされたライブラリのバインドを作成するには、いくつかの追加の手順とツールが必要です。 このアプローチには、次の4つの手順が含まれます。

1. ネイティブライブラリをビルドする
1. Xamarin のメタデータを準備して、Xamarin ツールC#でクラスを生成できるようにします
1. ネイティブライブラリとメタデータを使用して Xamarin バインドライブラリを構築する
1. Xamarin アプリケーションで Xamarin バインドライブラリを使用する

以下のセクションでは、これらの手順の概要を説明します。

### <a name="build-the-native-library"></a>ネイティブライブラリをビルドする

最初の手順は、ネイティブ Kotlin ライブラリ (Android アーカイブである AAR パッケージ) を取得することです。 ベンダーから直接要求することも、自分でビルドすることもできます。

### <a name="prepare-the-xamarin-metadata"></a>Xamarin メタデータを準備する

2番目の手順では、メタデータ変換ファイルを準備します。このファイルは、それぞれC#のクラスを生成するために Xamarin ツールによって使用されます。 ベストケースシナリオでは、すべてのクラスが Xamarin ツールによって検出および生成される場合、このファイルは空になる可能性があります。 場合によっては、正しいコードまたは必要C#なコードを生成するためにメタデータ変換を適用する必要があります。 多くの場合、 [Java デコンパイラ (JD)](http://java-decompiler.github.io/)などの AAR 逆アセンブラーを使用して、生成されるクラスの最終的な一覧から除外する非表示のC#依存関係や不要なクラスを識別する必要があります。 最終的なメタデータは、参照する Xamarin アプリケーションがと対話するパブリックインターフェイスを表す必要があります。

### <a name="build-a-xamarinandroid-binding-library"></a>Xamarin Android のバインドライブラリを構築する

3番目の手順では、特別なプロジェクトを作成します (Xamarin. Android バインドライブラリ)。 これには、ネイティブ参照としての Kotlin ライブラリと、前の手順で定義したメタデータ変換が含まれます。 作成時には、参照する AAR パッケージごとに個別の Android バインドライブラリプロジェクトが必要です。 バインドライブラリでは、Kotlin 標準ライブラリをサポートするために、 [Kotlin. stdlib.h>](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/)パッケージを追加する必要があります。

### <a name="consume-the-xamarin-binding-library"></a>Xamarin バインドライブラリを使用する

最後の手順は、Xamarin Android アプリケーションでバインドライブラリを参照することです。 Xamarin. Android バインドライブラリへの参照を追加すると、Xamarin アプリケーションで、公開されている Kotlin クラスをそのパッケージ内から使用できるようになります。

## <a name="walkthrough"></a>チュートリアル

このアプローチでは、Xamarin の Kotlin Binding を作成するために必要な手順の概要を説明します。 ネイティブのツールと言語の変更に合わせて、これらのバインディングを実際に準備する際には、多くの下位レベルの手順が必要になり、さらに詳細な考慮事項があります。 この目的は、この概念と、このプロセスに関連する大まかな手順を深く理解できるようにすることです。 詳細な手順ガイドについては、 [Xamarin Kotlin Binding チュートリアル](walkthrough.md)のドキュメントを参照してください。

## <a name="related-links"></a>関連リンク

- [Android Studio](https://developer.android.com/studio)
- [Gradle のインストール](https://gradle.org/install/)
- [Visual Studio for Mac](https://visualstudio.microsoft.com/downloads)
- [Java デコンパイラ](http://java-decompiler.github.io/)
- [BubblePicker Kotlin ライブラリ](https://github.com/igalata/Bubble-Picker)
- [Java ライブラリのバインド](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/)
- [XPath](https://www.w3.org/TR/xpath/)
- [Java バインドメタデータ](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata)
- [Kotlin. Stdlib.h> NuGet](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/)
- [サンプルプロジェクトリポジトリ](https://github.com/xamcat/xamarin-binding-kotlin-framework)
