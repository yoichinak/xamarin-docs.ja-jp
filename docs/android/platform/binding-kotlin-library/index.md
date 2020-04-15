---
title: Android Kotlin ライブラリをバインドする
description: このドキュメントでは、Kotlin コードへの C# バインドを作成して、Xamarin.Android アプリケーションでネイティブ ライブラリを使用できるようにする方法について説明します。
ms.prod: xamarin
ms.assetid: AB03A6C4-5A5A-4EAD-AD51-D887B20A3551
ms.technology: xamarin-android
author: alexeystrakh
ms.author: alstrakh
ms.date: 02/11/2020
ms.openlocfilehash: ec7d154b0d7fcb055bd398089e142fe8b1d9f60e
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "77497966"
---
# <a name="bind-android-kotlin-libraries"></a>Android Kotlin ライブラリをバインドする

Android プラットフォームは、そのネイティブ言語とツールと共に常に進化しており、最新のオファリングを使用して開発されたサード パーティ製のライブラリが多数あります。 コードとコンポーネントを再利用して最大限に活用することは、クロスプラットフォーム開発の主な目標の 1 つです。 Kotlin でビルドされたコンポーネントを再利用する機能は、開発者の間で人気が高まり続けているため、Xamarin 開発者にとってますます重要になっています。 お客様は、通常の [Java](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/) ライブラリをバインドするプロセスについて、既に理解されているかもしれません。 [Kotlin ライブラリをバインドする](walkthrough.md)プロセスについて説明する追加のドキュメントを利用できるようになったため、Xamarin アプリケーションでも同じ方法で使用できます。 このドキュメントの目的は、Xamarin 用の Kotlin バインドを作成するための上位レベルのアプローチについて説明することです。

## <a name="high-level-approach"></a>上位レベルのアプローチ

Xamarin を使用すると、任意のサード パーティ製のネイティブ ライブラリを、Xamarin アプリケーションで使用できるようにバインドできます。 Kotlin は新しい言語であり、この言語でビルドされたライブラリに対してバインドを作成するには、いくつかの追加の手順とツールが必要です。 このアプローチには、次の 4 つの手順が含まれます。

1. ネイティブ ライブラリをビルドする
1. Xamarin メタデータを準備する。これにより、Xamarin ツールで C# クラスを生成できるようになります
1. ネイティブ ライブラリとメタデータを使用して Xamarin バインド ライブラリをビルドする
1. Xamarin アプリケーションで Xamarin バインド ライブラリを使用する

以下のセクションでは、これらの手順の概要を追加の詳細情報と共に説明します。

### <a name="build-the-native-library"></a>ネイティブ ライブラリをビルドする

最初の手順は、ネイティブ Kotlin ライブラリ (Android アーカイブである AAR パッケージ) を取得することです。 ベンダーから直接要求するか、自分でビルドするかのいずれかを行うことができます。

### <a name="prepare-the-xamarin-metadata"></a>Xamarin メタデータを準備する

2 番目の手順では、メタデータ変換ファイルを準備します。これは、それぞれの C# クラスを生成するために Xamarin ツールによって使用されます。 最適なケース シナリオでは、すべてのクラスが Xamarin ツールによって検出および生成される場合、このファイルは空になる可能性があります。 場合によっては、正しいコードまたは目的の C# コード、あるいは両方を生成するために、メタデータ変換を適用する必要があります。 多くの場合、[Java デコンパイラ (JD)](http://java-decompiler.github.io/) などの AAR 逆アセンブラーを使用して、生成される C# クラスの最終的なリストから除外する、非表示の依存関係や不要なクラスを識別する必要があります。 最終的なメタデータは、参照する Xamarin.Android アプリケーションがやりとりするパブリック インターフェイスを表す必要があります。

### <a name="build-a-xamarinandroid-binding-library"></a>Xamarin.Android バインド ライブラリをビルドする

3 番目の手順では、特別なプロジェクトを作成します (Xamarin.Android バインド ライブラリ)。 これには、ネイティブ参照としての Kotlin ライブラリと、前の手順で定義したメタデータ変換が含まれます。 作成時には、参照する AAR パッケージごとに、個別の Android バインド ライブラリ プロジェクトが必要です。 バインド ライブラリでは、Kotlin 標準ライブラリをサポートするために、[Xamarin.Kotlin.StdLib](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/) パッケージを追加する必要があります。

### <a name="consume-the-xamarin-binding-library"></a>Xamarin バインド ライブラリを使用する

4 番目の最後の手順は、Xamarin.Android アプリケーションでバインド ライブラリを参照することです。 Xamarin.Android バインド ライブラリへの参照を追加すると、Xamarin アプリケーションで、公開されている Kotlin クラスをそのパッケージ内から使用できるようになります。

## <a name="walkthrough"></a>チュートリアル

上記のアプローチでは、Xamarin 用の Kotlin バインドを作成するために必要な手順の概要について説明しています。 これらのバインドを実際に準備する際には、ネイティブのツールと言語の変更に合わせるなど、多くの下位レベルの手順が必要になり、さらに詳細な考慮事項があります。 ここでの目的は、この概念と、このプロセスに関連する手順の概要をより深く理解できるようにすることです。 詳細なステップ バイ ステップ ガイドについては、[Xamarin Kotlin バインドのチュートリアル](walkthrough.md)に関するドキュメントを参照してください。

## <a name="related-links"></a>関連リンク

- [Android Studio](https://developer.android.com/studio)
- [Gradle のインストール](https://gradle.org/install/)
- [Visual Studio for Mac](https://visualstudio.microsoft.com/downloads)
- [Java デコンパイラ](http://java-decompiler.github.io/)
- [BubblePicker Kotlin ライブラリ](https://github.com/igalata/Bubble-Picker)
- [Java ライブラリのバインド](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/)
- [XPath](https://www.w3.org/TR/xpath/)
- [Java バインド メタデータ](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata)
- [Xamarin.Kotlin.StdLib NuGet](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/)
- [サンプル プロジェクト リポジトリ](https://github.com/xamcat/xamarin-binding-kotlin-framework)
