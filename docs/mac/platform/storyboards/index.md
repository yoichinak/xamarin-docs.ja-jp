---
title: Xamarin.Mac 内のストーリー ボードの概要
description: この記事では、ストーリー ボード Xamarin.Mac アプリでの操作に概要を示します。 ストーリーボードと Xcode の Interface Builder を使用したアプリの UI の作成と維持管理に関する内容が含まれています。
ms.prod: xamarin
ms.assetid: F37BA503-0B25-489F-80A8-58C493291A55
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 027998d6aff8aba4e5621b1cde51a24e18821ff9
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792666"
---
# <a name="introduction-to-storyboards-in-xamarinmac"></a>Xamarin.Mac 内のストーリー ボードの概要

_この記事では、ストーリー ボード Xamarin.Mac アプリでの操作に概要を示します。作成してストーリー ボードと Xcode のインターフェイスのビルダーを使用して、アプリの UI を維持する方法を説明します。_

Xamarin.Mac アプリだけでなく、ウィンドウの定義と、コントロールが含まれていますが、さまざまなウィンドウの間のリンクを含むものユーザー インターフェイスを開発することはストーリー ボード (を介して segues) および状態を表示します。

[![](images/intro01.png "Xcode で UI の例")](images/intro01.png#lightbox)

この記事では、ストーリー ボードを使用して Xamarin.Mac アプリのユーザー インターフェイスを定義する概要を提供します。

<a name="What-are-Storyboards" />

## <a name="what-are-storyboards"></a>ストーリー ボードとは

ストーリー ボードを使用するは、すべて Xamarin.Mac アプリの UI はすべて、個々 の要素およびユーザー インターフェイスの間のナビゲーションの 1 つの場所で定義できます。 Xamarin.Mac のストーリー ボードは、Xamarin.iOS for ストーリー ボードによく似た方法で機能します。 ただし、別のセットを含まれる_話題型_別のインターフェイスの表現方法が原因です。

<a name="Working-with-Scenes" />

### <a name="working-with-scenes"></a>シーンの操作

前述のように、ストーリー ボードを定義のすべての機能の概要に分割する特定のアプリの UI の_コント ローラーの表示_です。 Xcode のインターフェイス ビルダーでは、これらのコント ローラーの各に住んでいる独自_シーン_です。

[![](images/intro02.png "例ビュー コント ローラー")](images/intro02.png#lightbox)

各シーンでは、一連の線 (Segues と呼ばれる) UI では、それらの関係を示すためには、各シーンを接続すると、特定のビューとビュー コント ローラーのペアを表します。 いくつか Segues ビュー コント ローラーの 1 つの方法を定義する 1 つまたは複数の子ビューまたはコント ローラーの表示が含まれています。 その他の Segues (重なってまたはダイアログ ボックスを表示する) などのビューのコント ローラー間の遷移を定義します。 

[![](images/intro03.png "サンプル segue")](images/intro03.png#lightbox)

最も重要な注目するは各 Segue が何らかの形式、アプリの UI の特定の要素間のデータのフローを表します。

<a name="Working-with-View-Controllers" />

### <a name="working-with-view-controllers"></a>コント ローラーの表示の操作

コント ローラーの表示は、Mac アプリ内の情報の特定のビューと、その情報を提供するデータ モデル間のリレーションシップを定義します。 ストーリー ボードの各トップ レベル シーンでは、Xamarin.Mac アプリのコード内の 1 つのビュー コント ローラーを表します。

[![](images/intro04.png "明細ビュー コント ローラーの例")](images/intro04.png#lightbox)

これにより、各ビューのコント ローラーは、再利用可能な自己完結型情報のビジュアル表現 (ビュー) と存在し、その情報を制御するためのロジックの両方の組み合わせです。

所定のシーン内で、すべての個人によって処理された通常は、処理を実行できます`.xib`ファイル。 

 - 場所は subviews し、(ボタンのテキスト ボックスなど) を制御します。
 - 要素の位置と自動レイアウトの制約を定義します。
 - ワイヤ アップ アクションとコンセントとコードの UI 要素を公開します。

<a name="Working-with-Segues" />

### <a name="working-with-segues"></a>扱い Segues します。

前述のように、Segues はすべて、アプリの UI を定義するシーンの間のリレーションシップを提供します。 Ios ストーリー ボードでの作業に慣れている場合がわかっている Segues 用、iOS は、通常、全画面ビュー間の遷移を定義します。 これは、プロセスは Segues は通常「含有」(1 つのシーンは、親シーンの子) を定義するときに macOS などとは異なります。

MacOS、ほとんどのアプリを分割ビューやタブなどの UI 要素を使用して、同じウィンドウ内で同時に、ビューをグループ化する傾向があります。 IOS、上と画面をオフに遷移するビューが必要があるとは異なり制限物理により領域を表示できます。

場合によっては包含に向かって macOS の傾向を指定するには、ここで_プレゼンテーション Segues_モーダル ウィンドウ、シート ビューおよび Popover などが使用されます。

オーバーライドできますプレゼンテーション Segues を使用する場合、`PrepareForSegue`および変数の初期化にプレゼンテーション親ビュー コント ローラーのメソッド、表示されているビュー コント ローラーに任意のデータを提供します。

<a name="Design-and-Run-Times" />

### <a name="design-and-run-times"></a>デザインし、実行時間

デザイン時に (ときに Xcode のインターフェイスのビルダーで UI をレイアウト)、アプリの UI の各要素が構成項目に分割します。

- **シーン**で、これで構成されます。
    - **コント ローラーを表示**-ビューとそれらをサポートするデータの間のリレーションシップを定義します。
    - **ビューおよびサブビュー** -ユーザー インターフェイスを構成する実際の要素。
    - **コンテインメント Segues** -シーン間の親子リレーションシップを定義します。
- **プレゼンテーション Segues** -個々 のプレゼンテーション モードを定義します。 

この方法で各要素を定義してこれにより各要素の遅延読み込みの実行時に必要なときにのみできます。 MacOS などのプロセス全体の設計を開発者が必要な最小限のさせるために、コードのバックアップを必要とする、複雑で柔軟性のユーザー インターフェイスを作成できるようにしながら、可能なシステム リソースと、効率的なされています。

<a name="Storyboard-Quick-Start" />

## <a name="storyboard-quick-start"></a>ストーリー ボードのクイック スタート

[ストーリー ボードのクイック スタート](~/mac/platform/storyboards/quickstart.md)ガイドは、アプリケーションを作成した、単純な Xamarin.Mac をユーザー インターフェイスを作成するには、ストーリー ボードなどの操作の主要な概念が導入されています。 サンプル アプリでは、ビューの分割を含む、_コンテンツ エリア_と_インスペクター領域_し、シンプルな環境設定 ダイアログ ウィンドウが表示されます。 使用する Segues 結び付け、すべてのユーザー インターフェイス要素にします。

<a name="Working-with-Storyboards" />

## <a name="working-with-storyboards"></a>ストーリー ボードの使用

ここでは、詳細[ストーリー ボードを](~/mac/platform/storyboards/indepth.md)Xamarin.Mac アプリでします。 シーンとコント ローラーの表示、およびビューの構成されている方法を詳しく説明を取得します。 その後、Segues と共にこれらのシーンが関連付けられている方法を見てをみましょう。 最後に、カスタムの Segue 型の使用を見てをみましょう。 

<a name="Complex-Storyboard-Example" />

## <a name="complex-storyboard-example"></a>複雑なストーリー ボードの使用例

ストーリー ボードで Xamarin.Mac アプリでの作業の複雑な例の例を参照してください、 [SourceWriter サンプル アプリ](https://developer.xamarin.com/samples/mac/SourceWriter/)です。 SourceWriter は、コードの完了とシンプルな構文の強調表示をサポートするシンプルなソース コード エディターです。

SourceWriter コード全体に詳細なコメントが付いていて、可能な場合は、重要な技術やメソッド、Xamarin.Mac ガイド ドキュメントの関連情報へのリンクが示されます。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事の内容が Xamarin.Mac アプリでストーリー ボードを操作する概要を確認しています。 ユーザー インターフェイスを定義する方法とストーリー ボードを使用して新しいアプリを作成する方法を説明しました。 また別のウィンドウ間を移動する方法を説明しましたしを使用してビュー ステート segues します。


## <a name="related-links"></a>関連リンク

- [Hello Mac (サンプル)](https://developer.xamarin.com/samples/mac/Hello_Mac/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [ウィンドウの操作](~/mac/user-interface/window.md)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows の概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
