---
title: Objective-C のバインド
description: このドキュメントでは、開発者が Xamarin アプリケーションで既製C#のライブラリを使用できるようにするために、目的 C コードへのバインディングを作成する方法について説明しているさまざまなガイドへのリンクを示します。
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
author: conceptdev
ms.author: crdun
ms.date: 01/25/2016
ms.openlocfilehash: d48245ac6939a7b1a1528a7b42ec4a701f062a95
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70765766"
---
# <a name="binding-objective-c"></a>Objective-C のバインド

このセクションには、目的 C ライブラリへのバインドの作成に関するさまざまなドキュメントが含まれているC#ため、xamarin または Xamarin. Mac で作成したアプリケーションから呼び出すことができます。

## <a name="overviewcross-platformmaciosbindingoverviewmd"></a>[概要](~/cross-platform/macios/binding/overview.md)

このドキュメントには、バインディングのしくみの内部構造の一部が含まれています。 詳細なドキュメントであり、技術情報が含まれています。

## <a name="binding-objective-c-librariescross-platformmaciosbindingobjective-c-librariesmd"></a>[Objective-C ライブラリのバインド](~/cross-platform/macios/binding/objective-c-libraries.md)

このドキュメントでは、目的 C Api C#のバインディングを作成するために使用されるプロセスと、.net で使用される表現に対して、目的 c の表現がどのようにマップされるかについて説明します。
C Api のみをバインドする場合は、P/Invoke フレームワークの標準の .NET 機構を使用する必要があります。

## <a name="binding-definition-reference-guidecross-platformmaciosbindingbinding-types-referencemd"></a>[バインディング定義のリファレンスガイド](~/cross-platform/macios/binding/binding-types-reference.md)

これは、バインディングの生成プロセスを実行するために作成者が使用できるすべての属性について説明するリファレンスガイドです。

## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

目標マジックペンは、バインドの最初のパスをブートストラップするためのコマンドラインツールです。 これは、ネイティブライブラリのヘッダーファイルを解析して、パブリック API を[バインド定義](~/cross-platform/macios/binding/objective-c-libraries.md)(手動でも実行可能なプロセス) にマップすることによって機能します。

## <a name="ios"></a>iOS

[IOS バインドページ](~/ios/platform/binding-objective-c/index.md)は、次の例に加えて、これらの共通バインディングリソースにリンクします。

### <a name="walkthrough-binding-an-objective-c-libraryiosplatformbinding-objective-cwalkthroughmd"></a>[チュートリアル: 目的 C ライブラリのバインド](~/ios/platform/binding-objective-c/walkthrough.md)

この記事では、オープンソースの[Infcolorpicker](https://github.com/InfinitApps/InfColorPicker)目標 C プロジェクトを例として使用して、バインドプロジェクトを作成する手順について説明します。 InfColorPicker ライブラリには、ユーザーが HSB 表現に基づいて色を選択できるようにする再利用可能なビューコントローラーが用意されており、色の選択がよりわかりやすくなっています。 目標マジックペンは、バインディングプロセスを支援するために使用されます。

### <a name="binding-sampleshttpsgithubcommonomonotouch-bindings"></a>[バインディングのサンプル](https://github.com/mono/monotouch-bindings)

新しいバインドプロジェクトを作成するときに参照として使用できるサードパーティのバインドのコレクション。

## <a name="mac"></a>Mac

以前の[Mac バインド](~/mac/platform/binding.md)は、非常に手動のプロセスでした。 現在、今後のリリースの Visual Studio for Mac では、Mac バインドプロジェクトのサポートのダウンロード可能な[プレビュー](https://forums.xamarin.com/discussion/59760/xamarin-mac-binding-project-preview)があります。

## <a name="related-links"></a>関連リンク

- [iOS のバインド](~/ios/platform/binding-objective-c/index.md)
- [Mac バインド](~/mac/platform/binding.md)
