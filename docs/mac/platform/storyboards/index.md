---
title: Xamarin.Mac でストーリー ボードの概要
description: この記事では、Xamarin.Mac アプリでストーリー ボードの使用の概要を提供します。 ストーリーボードと Xcode の Interface Builder を使用したアプリの UI の作成と維持管理に関する内容が含まれています。
ms.prod: xamarin
ms.assetid: F37BA503-0B25-489F-80A8-58C493291A55
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 166a50021c22aa09be3eecdb8b745a70e75c3d51
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61031485"
---
# <a name="introduction-to-storyboards-in-xamarinmac"></a>Xamarin.Mac でストーリー ボードの概要

_この記事では、Xamarin.Mac アプリでストーリー ボードの使用の概要を提供します。作成および保守のストーリー ボードと Xcode の Interface Builder を使用して、アプリの UI について説明します。_

ストーリー ボードを使用するだけでなく、ウィンドウの定義と、コントロールが含まれますが、別のウィンドウ間のリンクも含まれます Xamarin.Mac アプリのユーザー インターフェイスを開発できます (を使用してセグエ) および状態を表示します。

[![](images/intro01.png "Xcode で UI の例")](images/intro01.png#lightbox)

この記事では、Xamarin.Mac アプリのユーザー インターフェイスを定義するストーリー ボードの使用の概要が説明されます。

<a name="What-are-Storyboards" />

## <a name="what-are-storyboards"></a>ストーリー ボードとは

ストーリー ボードを使用するは、すべての Xamarin.Mac アプリの UI はすべてその個々 の要素およびユーザー インターフェイスの間のナビゲーションの 1 つの場所で定義できます。 Xamarin.Mac のストーリー ボードは、Xamarin.iOS 用のストーリー ボードによく似た方法で動作します。 別のセットが含まれている、_セグエの種類_別のインターフェイスの表現方法が原因です。

<a name="Working-with-Scenes" />

### <a name="working-with-scenes"></a>シーンの操作

ストーリー ボードに、特定のアプリの機能の概要に分割するための UI を定義します前述のように、その_ビュー コント ローラー_します。 Xcode の Interface Builder でこれらのコント ローラーの各在住独自_シーン_します。

[![](images/intro02.png "例のビュー コント ローラー")](images/intro02.png#lightbox)

各シーンは、一連の (Segues と呼ばれます) の関係を示すため、UI の各シーンを接続する線の特定のビューとビュー コント ローラーのペアを表します。 いくつかの Segues 方法の 1 つのビュー コント ローラーを定義する 1 つまたは複数の子ビューまたはコント ローラーの表示が含まれています。 その他の Segues (ポップ オーバーまたはダイアログ ボックスを表示する) などのビュー コント ローラー間の遷移を定義します。 

[![](images/intro03.png "サンプルのセグエ")](images/intro03.png#lightbox)

最も重要なことに注意してくださいは、各セグエが何らかの形式のアプリの UI の特定の要素の間でデータのフローを表します。

<a name="Working-with-View-Controllers" />

### <a name="working-with-view-controllers"></a>ビュー コント ローラーの操作

ビュー コント ローラーは、Mac アプリ内の情報の指定されたビューとその情報を提供するデータ モデルの間のリレーションシップを定義します。 ストーリー ボードの各最上位レベルのシーンは、Xamarin.Mac アプリのコード内の 1 つのビュー コント ローラーを表します。

[![](images/intro04.png "明細ビュー コント ローラーの例")](images/intro04.png#lightbox)

これにより、各ビュー コント ローラーは、再利用可能な自己完結型情報のビジュアル表現 (ビュー) とその情報を表示および管理ロジックの両方の組み合わせ。

所定のシーン内で、すべての個人によって処理された通常は、操作を実行できます`.xib`ファイル。 

 - Subviews し、(ボタンやテキスト ボックス) を制御します。
 - 要素の位置と自動レイアウトの制約を定義します。
 - ワイヤ アップのアクションとアウトレットをコードに UI 要素を公開します。

<a name="Working-with-Segues" />

### <a name="working-with-segues"></a>操作をセグエします。

前述のように、Segues はすべて、アプリの UI を定義するシーンの間のリレーションシップを提供します。 Ios ストーリー ボードでの作業について熟知する場合は、あるとわかっているセグエの iOS は、通常、全画面表示ビュー間の遷移を定義します。 これは、プロセスは Segues 通常「含有」(1 つのシーンはシーンの親の子) を定義するときに、macOS とは異なります。

MacOS、ほとんどのアプリを分割ビュー タブなどの UI 要素を使用して、同じウィンドウ内のビューをグループ化する傾向があります。 ビューが画面外に移行する必要がある、iOS とは異なりの限られた物理により領域を表示します。

包含への macOS の傾向を指定するには、状況がある場所_プレゼンテーション セグエ_モーダル Windows、シート ビュー Popovers などを使用します。

プレゼンテーションのセグエを使用する場合をオーバーライドできます、`PrepareForSegue`親ビュー コント ローラーのメソッドをおよび変数の初期化に提示する、表示されているビュー コント ローラーへのデータを提供します。

<a name="Design-and-Run-Times" />

### <a name="design-and-run-times"></a>設計と実行時間

デザイン時に (ときに Xcode の Interface Builder では、UI をレイアウト)、アプリの UI の各要素は、構成項目に分割されます。

- **シーン**で、これで構成されます。
    - **コント ローラー表示**-ビューとそれらをサポートするデータの間のリレーションシップを定義します。
    - **ビューとサブビュー** -ユーザー インターフェイスを構成する実際の要素。
    - **包含 Segues**のシーン間の親子リレーションシップを定義します。
- **プレゼンテーションの Segues** -個々 のプレゼンテーション モードを定義します。 

この方法で各要素を定義してが各要素の遅延読み込みの実行時に必要な場合にのみします。 MacOS、プロセス全体を開発者が必要な最小限のコードを動作させるためのバックアップを必要とする、複雑で柔軟性の高いユーザー インターフェイスを作成できるように設計された同時に、可能なシステム リソースを効率化されています。

<a name="Storyboard-Quick-Start" />

## <a name="storyboard-quick-start"></a>ストーリー ボードのクイック スタート

[ストーリー ボードのクイック スタート](~/mac/platform/storyboards/quickstart.md)ガイド、ユーザー インターフェイスを作成するストーリー ボードの使用の主要な概念を導入する単純な Xamarin.Mac アプリを作成します。 サンプル アプリでは、別々 のビューを含む、_コンテンツ領域_と_インスペクター領域_とシンプルなユーザー設定 ダイアログ ウィンドウが表示されます。 使用する Segues をすべてのユーザー インターフェイス要素を関連付けます。

<a name="Working-with-Storyboards" />

## <a name="working-with-storyboards"></a>ストーリーボードの使用

ここでは、詳細の[ストーリー ボードの使用](~/mac/platform/storyboards/indepth.md)Xamarin.Mac アプリでします。 話をシーンとのビュー コント ローラーとビューで構成されている方法を詳しく説明します。 次に、シーンが Segues と関連付けられている方法を見てをみましょう。 最後に、カスタムのセグエの種類の操作を見てをみましょう。 

<a name="Complex-Storyboard-Example" />

## <a name="complex-storyboard-example"></a>複雑なストーリー ボードの例

Xamarin.Mac アプリでストーリー ボードの使用の複雑な例の例を参照してください、 [SourceWriter サンプル アプリ](https://developer.xamarin.com/samples/mac/SourceWriter/)します。 SourceWriter は、コードの完了とシンプルな構文の強調表示をサポートするシンプルなソース コード エディターです。

SourceWriter コード全体に詳細なコメントが付いていて、可能な場合は、重要な技術やメソッド、Xamarin.Mac ガイド ドキュメントの関連情報へのリンクが示されます。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、Xamarin.Mac アプリでストーリー ボードの使用の概要を取得しました。 ストーリー ボードを使用して新しいアプリを作成する方法と、ユーザー インターフェイスを定義する方法を説明しました。 また別のウィンドウ間を移動する方法を説明しましたしを使用して、ビュー ステートのセグエします。


## <a name="related-links"></a>関連リンク

- [Hello Mac (サンプル)](https://developer.xamarin.com/samples/mac/Hello_Mac/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [Windows の操作](~/mac/user-interface/window.md)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows の概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
