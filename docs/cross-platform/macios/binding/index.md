---
title: Objective-C のバインド
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: fef2826f536042dc9be830a4c0dc358658c359d9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="binding-objective-c"></a>Objective-C のバインド

このセクションには、さまざまな c# Xamarin.iOS または Xamarin.Mac で作成されたアプリケーションから呼び出せるように Objective C ライブラリを作成するバインディングに対応するドキュメントが含まれています。

##  <a name="overviewcross-platformmaciosbindingoverviewmd"></a>[概要](~/cross-platform/macios/binding/overview.md)

このドキュメントでは、バインディングが行われる方法の内部コンポーネントのいくつか含まれています。 これは、いくつかの技術的な情報と共に高度なドキュメントです。

##  <a name="binding-objective-c-librariescross-platformmaciosbindingobjective-c-librariesmd"></a>[Objective-C ライブラリのバインド](~/cross-platform/macios/binding/objective-c-libraries.md)

このドキュメントでは、c# Objective C Api および Objective C の表現方法を .NET で使用されている表現形式にマップする方法のバインドを作成するために使用するプロセスについて説明します。
C Api のみをバインドする場合は、これは、P/invoke framework 標準 .NET メカニズムを使用する必要があります。

##  <a name="binding-definition-reference-guidecross-platformmaciosbindingbinding-types-referencemd"></a>[バインディングの定義のリファレンス ガイド](~/cross-platform/macios/binding/binding-types-reference.md)

これは、すべてのバインドの生成処理をドライブにするバインディングの作成者に利用可能な属性を説明するリファレンス ガイドです。


## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

目標ペンを使わずは、ブートス トラップ バインディングの最初のパスを支援するコマンド ライン ツールです。 パブリック API をマップするネイティブ ライブラリのヘッダー ファイルを解析して、それと、[バインディング定義](~/cross-platform/macios/binding/objective-c-libraries.md)(この処理は手動で行うこともできます)。

## <a name="ios"></a>iOS

[IOS バインディング ページ](~/ios/platform/binding-objective-c/index.md)にリンク バック共通バインディング リソース、さらに次の例にします。

### <a name="walkthrough-binding-an-objective-c-libraryiosplatformbinding-objective-cwalkthroughmd"></a>[チュートリアル: バインド Objective C ライブラリ](~/ios/platform/binding-objective-c/walkthrough.md)

この記事は、オープン ソースを使用してバインド プロジェクトの作成の詳細なチュートリアル[InfColorPicker](https://github.com/InfinitApps/InfColorPicker)例として Objective C のプロジェクトです。 InfColorPicker ライブラリでは、ユーザーが、HSB 表現をよりわかりやすい色の選択を行ったに基づいて色を選択できる再利用可能なビュー コント ローラーを提供します。 目標ペンを使わずは、バインディング プロセスを支援するために使用されます。

### <a name="binding-sampleshttpsgithubcommonomonotouch-bindings"></a>[バインディングのサンプル](https://github.com/mono/monotouch-bindings)

使用可能なサード パーティのバインドのコレクションでは、新しいプロジェクトのバインドを作成するときの参照が使用されます。

## <a name="mac"></a>Mac

これまで[Mac バインディング](~/mac/platform/binding.md)非常に手動での処理されました。 現在、[プレビューのダウンロード可能な](https://forums.xamarin.com/discussion/59760/xamarin-mac-binding-project-preview)Mac バインド プロジェクトが、for mac、Visual Studio の今後のリリースのサポートの



## <a name="related-links"></a>関連リンク

- [iOS Binding](~/ios/platform/binding-objective-c/index.md)
- [Mac のバインド](~/mac/platform/binding.md)
