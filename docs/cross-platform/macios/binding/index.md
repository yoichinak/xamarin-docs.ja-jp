---
title: Objective-C のバインド
description: このドキュメントでは、c# へのバインド Objective C コードでは、Xamarin アプリケーションで市販のライブラリを使用する開発者を作成する方法を説明するさまざまなガイドにリンクを提供します。
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: 3f1e1ce324e849c0c939d936eb6ee1470cf24a3b
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855157"
---
# <a name="binding-objective-c"></a>Objective-C のバインド

このセクションには、さまざまな Xamarin.iOS または Xamarin.Mac で作成された c# アプリケーションから呼び出せるように、Objective C のライブラリを作成するバインドをカバーするドキュメントが含まれています。

##  <a name="overviewcross-platformmaciosbindingoverviewmd"></a>[概要](~/cross-platform/macios/binding/overview.md)

このドキュメントには、バインディングが行われる方法の内部の一部が含まれます。 技術情報についてはいくつかの高度なドキュメントです。

##  <a name="binding-objective-c-librariescross-platformmaciosbindingobjective-c-librariesmd"></a>[Objective-C ライブラリのバインド](~/cross-platform/macios/binding/objective-c-libraries.md)

このドキュメントでは、c# の Objective C Api と .NET で使用される手法に Objective C での用法をマップする方法のバインドを作成するために使用するプロセスについて説明します。
C Api だけをバインドする場合は、これは、P/invoke framework 標準 .NET メカニズムを使用する必要があります。

##  <a name="binding-definition-reference-guidecross-platformmaciosbindingbinding-types-referencemd"></a>[バインドの定義のリファレンス ガイド](~/cross-platform/macios/binding/binding-types-reference.md)

これは、すべてのバインドの生成プロセスを促進するバインディングの作成者に使用可能な属性を説明するリファレンス ガイドです。


## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

油性の目標は、バインディングの最初のパスをブートス トラップするコマンド ライン ツールです。 パブリック API をマップするネイティブ ライブラリのヘッダー ファイルを解析することによって動作、[バインディング定義](~/cross-platform/macios/binding/objective-c-libraries.md)(プロセスを手動で行うこともできます)。

## <a name="ios"></a>iOS

[IOS バインド ページ](~/ios/platform/binding-objective-c/index.md)にリンク バックこれらの一般的なバインド リソース、さらに次の例にします。

### <a name="walkthrough-binding-an-objective-c-libraryiosplatformbinding-objective-cwalkthroughmd"></a>[チュートリアル: OBJECTIVE-C ライブラリのバインド](~/ios/platform/binding-objective-c/walkthrough.md)

この記事では、オープン ソースを使用してバインド プロジェクトの作成のステップ バイ ステップ チュートリアル[InfColorPicker](https://github.com/InfinitApps/InfColorPicker)例として Objective C のプロジェクト。 InfColorPicker ライブラリは、ユーザーよりユーザー フレンドリな色の選択を行う、HSB 形式に基づいて色を選択できる再利用可能なビュー コント ローラーを提供します。 油性の目標は、バインディング プロセスを支援するために使用されます。

### <a name="binding-sampleshttpsgithubcommonomonotouch-bindings"></a>[バインディング サンプル](https://github.com/mono/monotouch-bindings)

可能性のあるサード パーティのバインドのコレクションでは、新しいプロジェクトのバインドを作成するときの参照が使用されます。

## <a name="mac"></a>Mac

これまで[Mac バインド](~/mac/platform/binding.md)非常に手動のプロセスにされました。 現時点では、[ダウンロード可能なプレビュー](https://forums.xamarin.com/discussion/59760/xamarin-mac-binding-project-preview)の将来のリリースの Visual Studio for mac Mac バインド プロジェクト サポート。



## <a name="related-links"></a>関連リンク

- [iOS バインド](~/ios/platform/binding-objective-c/index.md)
- [Mac のバインド](~/mac/platform/binding.md)
- [Xamarin University のコース: OBJECTIVE-C のバインド ライブラリをビルドします。](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University のコース: 目標油性、OBJECTIVE-C のバインド ライブラリをビルドします。](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
