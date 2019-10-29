---
title: Xamarin. Mac でのストーリーボードの概要
description: この記事では、Xamarin. Mac アプリでストーリーボードを操作する方法について説明します。 ストーリーボードと Xcode の Interface Builder を使用したアプリの UI の作成と維持管理に関する内容が含まれています。
ms.prod: xamarin
ms.assetid: F37BA503-0B25-489F-80A8-58C493291A55
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: b27a8d65ebaca6009d8310931b9dac3a4d7e12f3
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73026148"
---
# <a name="introduction-to-storyboards-in-xamarinmac"></a>Xamarin. Mac でのストーリーボードの概要

_この記事では、Xamarin. Mac アプリでストーリーボードを操作する方法について説明します。この記事では、ストーリーボードと Xcode の Interface Builder を使用したアプリの UI の作成と保守について説明します。_

ストーリーボードを使用すると、ウィンドウの定義とコントロールだけでなく、さまざまなウィンドウ (セグエ via) とビューステートの間のリンクも含まれている Xamarin. Mac アプリ用のユーザーインターフェイスを開発できます。

[![](images/intro01.png "A sample UI in Xcode")](images/intro01.png#lightbox)

この記事では、ストーリーボードを使用して Xamarin. Mac アプリのユーザーインターフェイスを定義する方法について説明します。

<a name="What-are-Storyboards" />

## <a name="what-are-storyboards"></a>ストーリーボードとは

ストーリーボードを使用すると、個々の要素とユーザーインターフェイスの間のすべてのナビゲーションを使用して、すべての Xamarin アプリの UI を1つの場所に定義できます。 Xamarin. Mac のストーリーボードは、Xamarin. iOS のストーリーボードと非常によく似た方法で動作します。 ただし、インターフェイスの表現方法が異なるため、異なる_セグエ型_のセットが含まれています。

<a name="Working-with-Scenes" />

### <a name="working-with-scenes"></a>シーンの操作

前述のように、ストーリーボードは、特定のアプリのすべての UI を、その_ビューコントローラー_の機能の概要に分割して定義します。 Xcode の Interface Builder では、各コントローラーが独自の_シーン_に存在します。

[![](images/intro02.png "An example view controller")](images/intro02.png#lightbox)

各シーンは、特定のビューとビューコントローラーのペアを表します。これは、UI 内の各シーンを接続する一連の行 (セグエと呼ばれます) で、それらのリレーションシップを示します。 一部のセグエでは、1つのビューコントローラーに1つ以上の子ビューまたはビューコントローラーがどのように含まれるかを定義します。 その他のセグエ、ビューコントローラー間の遷移を定義します (segue やダイアログボックスの表示など)。 

[![](images/intro03.png "A sample segue")](images/intro03.png#lightbox)

注意する必要がある最も重要な点は、各セグエは、アプリの UI の特定の要素間にある何らかの形式のデータのフローを表すことです。

<a name="Working-with-View-Controllers" />

### <a name="working-with-view-controllers"></a>ビューコントローラーの操作

ビューコントローラーは、Mac アプリ内の情報の特定のビューと、その情報を提供するデータモデルとの間の関係を定義します。 ストーリーボードの最上位レベルの各シーンは、Xamarin. Mac アプリのコード内の1つのビューコントローラーを表します。

[![](images/intro04.png "An example slips view controller")](images/intro04.png#lightbox)

このようにして、各ビューコントローラーは、情報の視覚的表現 (ビュー) とその情報を提示および制御するロジックの両方の自己完結型で再利用可能な組み合わせです。

特定のシーン内では、通常、個々の `.xib` ファイルによって処理されるすべての処理を実行できます。 

- サブビューとコントロール (ボタンやテキストボックスなど) を配置します。
- 要素の位置と自動レイアウトの制約を定義します。
- UI 要素をコードに公開するためのワイヤアップアクションとコンセント。

<a name="Working-with-Segues" />

### <a name="working-with-segues"></a>セグエの操作

前述のように、セグエは、アプリの UI を定義するすべてのシーン間の関係を提供します。 IOS のストーリーボードでの作業に慣れている場合は、通常、セグエ for iOS が全画面表示の切り替え効果を定義していることがわかります。 これは macOS とは異なり、セグエでは通常、"含有" (1 つのシーンは親シーンの子) を定義します。

MacOS では、ほとんどのアプリは、分割ビューやタブなどの UI 要素を使用して、同じウィンドウ内にビューをグループ化する傾向があります。 IOS とは異なり、物理的な表示領域が限られているために、ビューをオンまたはオフに切り替える必要がある場合。

MacOS の傾向に対しては、モーダルウィンドウ、シートビュー、Popovers などの_プレゼンテーションの_が使用される場合があります。

Presentation セグエを使用する場合は、表示する親ビューコントローラーの `PrepareForSegue` メソッドをオーバーライドして、変数を初期化し、表示されるビューコントローラーにデータを提供することができます。

<a name="Design-and-Run-Times" />

### <a name="design-and-run-times"></a>設計と実行時間

デザイン時 (Xcode の Interface Builder の UI をレイアウトするとき) に、アプリの UI の各要素が構成項目に分割されます。

- **シーン**: 次の要素で構成されます。
  - **ビューコントローラー** -ビューとそれらをサポートするデータの間のリレーションシップを定義します。
  - **ビューとサブビュー** -ユーザーインターフェイスを構成する実際の要素。
  - **コンテインメントセグエ**-シーン間の親子リレーションシップを定義します。
- **プレゼンテーションセグエ**-個々のプレゼンテーションモードを定義します。 

各要素をこのように定義することによって、実行時に必要な場合にのみ、各要素の遅延読み込みを行うことができます。 MacOS では、プロセス全体が、開発者が作業を行うために最小限のバックアップコードを必要とする複雑で柔軟なユーザーインターフェイスを作成できるように設計されています。また、可能な限りシステムリソースを効率的に使用できます。

<a name="Storyboard-Quick-Start" />

## <a name="storyboard-quick-start"></a>ストーリーボードクイックスタート

[ストーリーボードクイックスタート](~/mac/platform/storyboards/quickstart.md)ガイドでは、ストーリーボードを使用してユーザーインターフェイスを作成するための主要な概念を紹介する単純な Xamarin. Mac アプリを作成します。 このサンプルアプリは、_コンテンツ領域_と_インスペクター領域_を含む、Spilt 含まれるビューで構成され、簡単な基本設定ダイアログウィンドウが表示されます。 セグエを使用して、すべてのユーザーインターフェイス要素を連結します。

<a name="Working-with-Storyboards" />

## <a name="working-with-storyboards"></a>ストーリーボードの使用

このセクションでは、Xamarin. Mac アプリでの[ストーリーボードの操作](~/mac/platform/storyboards/indepth.md)の詳細について説明します。 シーンの詳細と、ビューコントローラーとビューで構成される方法について詳しく説明します。 次に、セグエと共にシーンがどのように関連付けられているかを見ていきます。 最後に、カスタムのセグエ型の使用について説明します。 

<a name="Complex-Storyboard-Example" />

## <a name="complex-storyboard-example"></a>複雑なストーリーボードの例

Xamarin. Mac アプリでストーリーボードを操作する複雑な例の例については、 [Sourcewriter サンプルアプリ](https://docs.microsoft.com/samples/xamarin/mac-samples/sourcewriter)を参照してください。 SourceWriter は、コードの完了とシンプルな構文の強調表示をサポートするシンプルなソース コード エディターです。

SourceWriter コード全体に詳細なコメントが付いていて、可能な場合は、重要な技術やメソッド、Xamarin.Mac ガイド ドキュメントの関連情報へのリンクが示されます。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、Xamarin. Mac アプリでストーリーボードを操作する方法について簡単に説明しました。 ストーリーボードを使用して新しいアプリを作成する方法と、ユーザーインターフェイスを定義する方法を説明しました。 また、セグエを使用して、さまざまなウィンドウ間を移動し、状態を表示する方法についても説明しました。

## <a name="related-links"></a>関連リンク

- [Hello Mac (サンプル)](https://docs.microsoft.com/samples/xamarin/mac-samples/hello-mac)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [Windows の操作](~/mac/user-interface/window.md)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows の概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
