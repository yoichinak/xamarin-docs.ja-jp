---
title: "統一されたストーリー ボード"
description: "統一されたストーリー ボードには、iOS デバイスの画面サイズの範囲が拡張をカバーする、複数のストーリー ボードではなく、1 つのストーリー ボードで、ユーザー インターフェイスを作成する開発者ができるようにします。 この記事は、Xamarin.iOS 内で統一されたストーリー ボードの操作に詳細な概要を説明する設計されています。"
ms.topic: article
ms.prod: xamarin
ms.assetid: F6F70374-FC2A-4401-A712-A16D0F9B340F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 60b2e6fa65226631fe2d2c847a56852ac9ae63d2
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="unified-storyboards"></a>統一されたストーリー ボード

_統一されたストーリー ボードには、iOS デバイスの画面サイズの範囲が拡張をカバーする、複数のストーリー ボードではなく、1 つのストーリー ボードで、ユーザー インターフェイスを作成する開発者ができるようにします。この記事は、Xamarin.iOS 内で統一されたストーリー ボードの操作に詳細な概要を説明する設計されています。_

iOS 8 には、ユーザー インターフェイスを作成するための新しい、使用する単純なメカニズムが含まれており、統一されたストーリー ボードです。 すべてのハードウェアのさまざまな画面サイズに対応する 1 つにストーリー ボードは、や応答性の高速のビューを作成することができます、"設計-1 回のみ、使用する多"のスタイル。

ように、開発者は、iPhone および iPad デバイス用に独立した特定のストーリー ボードを作成する必要がなくなり、共通のインターフェイスを持つアプリケーションを設計し、サイズの異なるクラスのインターフェイスをカスタマイズする柔軟性があります。 これにより、アプリケーションは、各フォーム ファクターの長所に合わせて調整することができ、最適なエクスペリエンスを提供する各ユーザー インターフェイスをチューニングすることができます。

<a name="size-classes" />

## <a name="size-classes"></a>サイズ クラス

IOS 8 の場合は、前に、開発者が使用される`UIInterfaceOrientation`と`UIInterfaceIdiom`縦長と横長のモード間で、iPhone および iPad デバイス間で区別するためにします。 印刷の向きとデバイスの決定、iOS8 でを使用して、*サイズ クラス*です。

デバイスは、垂直および水平方向の両方の軸で、サイズのクラスによって定義および iOS 8 のサイズ クラスの 2 種類があります。

-  **正規**– これは (iPad) などの大規模な画面サイズ、または大きなサイズの印象を提供するガジェットの (など、 `UIScrollView`
-  **Compact** – これは、小さいデバイス (iPhone) などです。 このサイズでは、デバイスの向きが考慮されます。


2 つの概念が一緒に使用されている場合、結果は次の図に示すように、両方のさまざまな向きで使用できるさまざまな可能なサイズを定義する 2 x 2 グリッドには。

 [![](unified-storyboards-images/sizeclassgrid.png "標準モードと最適化の向きで使用できるさまざまな可能なサイズを定義する 2 x 2 グリッド")](unified-storyboards-images/sizeclassgrid.png#lightbox)

開発者は、いずれかのようなさまざまなレイアウト (グラフィックスの上に表示) と 4 つの可能性を使用するビュー コント ローラーを作成できます。

### <a name="ipad-size-classes"></a>iPad サイズ クラス

サイズにより、iPad は、**正規**クラスの両方の向きのサイズ。

 [![](unified-storyboards-images/image1.png "iPad サイズ クラス")](unified-storyboards-images/image1.png#lightbox)


### <a name="iphone-size-classes"></a>iPhone サイズ クラス

IPhone では、デバイスの向きに基づくサイズの異なるクラスがあります。

 [![](unified-storyboards-images/iphonesizeclasses.png "iPhone サイズ クラス")](unified-storyboards-images/iphonesizeclasses.png#lightbox)

-  デバイスは、縦向きモードでは、画面が、 **compact**クラスの水平方向および**正規**垂直方向に
-  デバイスは、横モードでは、画面のクラスが縦モードから取り消されます。

### <a name="iphone-6-plus-size-classes"></a>iPhone 6 Plus サイズ クラス

サイズは、ランドス ケープの異なるが、縦向きで以前の Iphone と同じです。

[![](unified-storyboards-images/iphone6sizeclasses.png "iPhone 6 Plus サイズ クラス")](unified-storyboards-images/iphone6sizeclasses.png#lightbox)

IPhone 6 Plus は十分な大きさの画面、横置きモードの通常の幅サイズ クラスがあることができます。

### <a name="support-for-a-new-screen-scale"></a>新しい画面のスケールのサポート

IPhone 6 Plus で画面スケール ファクター 3.0 (3 回元 iPhone 画面の解像度) の新しい HD の Retina ディスプレイを使用します。 これらのデバイスで、最適なエクスペリエンスを提供するには、この画面スケール用に設計された新しいアートワークにはが含まれます。 Xcode 6 以降で、資産のカタログが 1、2 x、および 3 x サイズ; でイメージを含めることができます。新しいイメージの資産および iOS は、iPhone 6 で実行されている場合に、正しい資産に選択されますに追加するだけとします。

IOS での動作も読み込む画像認識、`@3x`イメージ ファイルにサフィックス。 たとえば、開発者には、次のファイル名、アプリケーションのバンドルに (別の解像度) のイメージ アセットが含まれています: `MonkeyIcon.png`、 `MonkeyIcon@2x.png`、および`MonkeyIcon@3x.png`です。 Iphone 6 Plus、`MonkeyIcon@3x.png`開発者が次のコードを使用してイメージを読み込む場合、イメージを自動的に使用されます。

```csharp
UIImage icon = UIImage.FromFile("MonkeyImage.png");
```
イメージを iOS を使用して、UI 要素に割り当てられる場合、またはデザイナーとして`MonkeyIcon.png`、`MonkeyIcon@3x.png`で使用される、自動的に再度 iPhone 6 Plus です。

<a name="dynamic-launch-screens" />

### <a name="dynamic-launch-screens"></a>動的な起動画面

起動画面のファイルは、アプリが実際に開始アップことをユーザーにフィードバックを提供する iOS アプリケーションを起動するときに、スプラッシュ スクリーンとして表示されます。 8、iOS の前に、開発者が、複数を含める`Default.png`イメージの資産を各デバイスの種類、方向、および画面解像度でアプリケーションを実行するとします。

新しい 8、iOS を開発者が 1 つのアトミックを作成できます`.xib`自動レイアウトおよびサイズ クラスの作成を使用した Xcode でファイル、*動的な起動画面*は、すべてのデバイス、解像度や向き動作をします。 これだけでなくを作成し、すべての必要なイメージ資産を保持する開発者の必要な作業の量が減少しますが、アプリケーションのインストールされているバンドルのサイズが軽減されます。

## <a name="traits"></a>Traits

特徴 (traits) は、その環境の変化に応じて、レイアウトがどのように変化するかを決定するために使用するプロパティです。 一連のプロパティで構成されます (、`HorizontalSizeClass`と`VerticalSizeClass`に基づいて`UIUserInterfaceSizeClass`)、インターフェイスの表現形式だけでなく ( `UIUserInterfaceIdiom`) と表示の小数点以下桁数です。

状態は、上記のすべての Apple では、特徴であるコレクションをコンテナーにラップされます ( `UITraitCollection`)、だけでなく、プロパティ、その値もが含まれています。

## <a name="trait-environment"></a>特徴である環境

特徴である環境では、iOS 8 の新しいインターフェイスし、は、次のオブジェクトの特徴であるコレクションを返すことができません。

-  画面 ( `UIScreens` )。
-  Windows ( `UIWindows` )。
-  コント ローラーを表示 ( `UIViewController` )。
-  ビュー ( `UIView` )。
-  プレゼンテーションのコント ローラー ( `UIPresentationController` )。


開発者は、ユーザー インターフェイスの配置方法を決定するのに特徴環境によって返される特徴であるコレクションを使用します。

階層は、次の図に示すようにすべての特徴である環境ください。

 [![](unified-storyboards-images/viewhierarchy.png "環境の特徴階層図")](unified-storyboards-images/viewhierarchy.png#lightbox)

特徴であるコレクションの上の特徴である環境があること、フロー、既定では、親、子環境からです。

現在の特徴であるコレクションを取得するには、ほかの特徴である環境にが、`TraitCollectionDidChange`メソッドで、ビューまたはビュー コント ローラーのサブクラスでオーバーライドされることができます。 開発者は、これらの特徴 (traits) が変更されたときに、特徴 (traits) に依存している UI 要素のいずれかを変更するのに、このメソッドを使用できます。

## <a name="typical-trait-collections"></a>一般的な特徴であるコレクション

このセクションでは、ユーザーが iOS 8 を使用する場合に発生するの特徴であるコレクションの一般的な種類を説明します。

以下は、開発者可能性があります、iPhone で表示される一般的な特徴であるコレクションです。

|プロパティ|[値]|
|--- |--- |
|`HorizontalSizeClass`|Compact|
|`VerticalSizeClass`|Regular|
|`UserInterfaceIdom`|電話番号|
|`DisplayScale`|2.0|

すべての特徴であるプロパティの値が、上記のセットでは、完全修飾の特徴であるコレクションを表します。

その値の一部が欠落している特徴であるコレクションが存在することも (Apple として参照*未指定*)。

|プロパティ|[値]|
|--- |--- |
|`HorizontalSizeClass`|Compact|
|`VerticalSizeClass`|指定されていません。|
|`UserInterfaceIdom`|指定されていません。|
|`DisplayScale`|指定されていません。|

一般に、ただし、問い合わせるときに、開発者の特徴である環境の特徴は、コレクションのがコレクションを返す完全修飾では、上記の例のようです。

(ビューまたはビュー コント ローラーの場合) などの特徴である環境が現在のビュー階層内でない場合は、開発者可能性があります戻る指定されていない値の特徴であるプロパティの 1 つ以上。

など、Apple によって提供される作成方法のいずれかを使用している場合、開発者は部分修飾の特徴であるコレクションを取得もは`UITraitCollection.FromHorizontalSizeClass`、新しいコレクションを作成します。

複数の特徴であるコレクションで実行できる 1 つの操作は比較する、互いにもう 1 つが含まれているかどうかは 1 つの特徴であるコレクションを確認する必要があります。 意味*コンテインメント*は、2 つ目のコレクションで指定されたすべての特徴の値と一致するが、最初のコレクション内の値と正確に一致します。

2 つの特徴 (traits) を使用してテストする、`Contains`のメソッド、`UITraitCollection`をテストする特徴の値を渡します。

開発者を決定するコードで、比較を手動で実行できるレイアウト ビューまたはコント ローラーの表示する方法です。 ただし、`UIKit`メソッドを使用してこの内部的にはたとえば、外観プロキシと同様に、その機能の一部を提供します。

## <a name="appearance-proxy"></a>外観プロキシ

外観プロキシは、開発者は、ビューのプロパティのカスタマイズを許可する iOS の以前のバージョンで導入されました。 Ios 8 の特徴であるコレクションをサポートするために拡張されました。

外観プロキシが新しいメソッドが追加されました`AppearanceForTraitCollection`に渡された特定の特徴であるコレクションの新しい外観プロキシを返します。 開発者を実行する任意のカスタマイズ外観プロキシのみ有効になります準拠しているビュー、指定された特徴であるコレクション。

部分的に指定した特徴であるコレクションに、開発者が渡す一般的に、`AppearanceForTraitCollection`直前に指定した水平方向サイズ クラスのコンパクトなコンパクトで水平方向には、アプリケーション内の任意のビューをカスタマイズすることができるようにするなどのメソッドです。

## <a name="uiimage"></a>UIImage

別のクラスの特徴であるコレクションには、Apple が追加した`UIImage`です。 過去に、開発者を指定する必要がある、@1Xと@2x(アイコン) などのアプリケーションに含める予定があるすべてのビットマップ グラフィック資産のバージョン。

iOS 8 は、開発者がカタログに含める複数のバージョンのイメージ、イメージの特徴であるコレクションに基づいてできるように拡張されました。 たとえば、開発者は、コンパクトな特徴クラスとその他のコレクションの完全なサイズ設定されたイメージを操作する小さいイメージを含めることができます。

内の使用される、イメージの 1 つ、`UIImageView`クラス、イメージのビューがその特徴であるコレクションの正しいバージョンのイメージを自動的に表示されます。 (など、ユーザーの簡易切り替え縦からデバイスを横向き) の特徴である環境が変更された場合、イメージ ビューが自動的に新しい特徴であるコレクションと一致し、中のイメージの現在のバージョンの一致するように、サイズ変更に新しい画像のサイズを選択します。表示されます。

## <a name="uiimageasset"></a>UIImageAsset

Apple のという iOS 8 に新しいクラスが追加`UIImageAsset`にイメージの選択より詳細に制御を開発者に付与します。

イメージ資産は、イメージの異なるバージョンのすべてをラップし、開発者がで渡されたの特徴であるコレクションに一致する特定のイメージを要求できるようにします。 イメージをイメージ資産から削除したり追加することができます、稼働中です。

イメージ資産の詳細については、Apple を参照してください。 [UIImageAsset](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset)ドキュメント。

## <a name="combining-trait-collections"></a>特徴であるコレクションを組み合わせること

特徴であるコレクションで、開発者が実行できる別の関数が合計する 2 つのことになりますが 1 つのコレクションから指定されていない値が 2 つ目の指定した値を置き換え、組み合わされたコレクションが発生します。 これを使用して、`FromTraitsFromCollections`のメソッド、`UITraitCollection`クラスです。

前述のよう、特徴であるコレクションの 1 つで指定されていませんが、特徴 (traits) のいずれかの別の指定した場合、値が指定されたバージョンに設定されます。 ただし、複数のバージョンの指定された特定の値がある場合は、最後の値の特徴であるコレクション値になります、ために使用されます。

## <a name="adaptive-view-controllers"></a>コント ローラーのアダプティブの表示

このセクションでは、ビューとコント ローラーの表示、iOS に特徴 (traits) およびサイズ クラスは、自動的に開発者向けのアプリケーションには適応性の概念を採用している方法の詳細を説明します。

### <a name="split-view-controller"></a>分割ビュー コント ローラー

IOS 8 で最もが変更されてビュー コント ローラー クラスの 1 つは、`UISplitViewController`クラスです。 以前は、開発者は多くの場合、分割ビュー コント ローラー iPad のバージョンのアプリケーションで実行してする必要があります、iPhone アプリのバージョンの用の階層の表示のまったく異なるバージョンを提供します。

8、ios、`UISplitViewController`クラスが使用可能 (iPad および iPhone)、両方のプラットフォームで、開発者は iPhone と iPad の両方の機能を 1 つのビュー コント ローラー階層を作成します。

IPhone が横にあるときは、分割ビュー コント ローラーで、iPad で表示されているときと同じようにそのビュー サイド バイ サイドが表示されます。

### <a name="overriding-traits"></a>特徴 (traits) をオーバーライドします。

分割ビュー コント ローラーを横方向に iPad に表示されている次の図のように、子コンテナーに親コンテナーから連鎖的に特徴である環境。

 [![](unified-storyboards-images/cascadingclasses01.png "横方向に iPad での分割ビュー コント ローラー")](unified-storyboards-images/cascadingclasses01.png#lightbox)

IPad では、水平および垂直の軸の方向に通常のサイズ クラスが含まれるために、マスター/詳細の両方のビューが分割ビューに表示されます。

場所、サイズ クラスは、両方の向きでコンパクトには、iPhone で分割ビュー コント ローラーのみが表示されます、詳細ビューでは、以下のように。

 [![](unified-storyboards-images/cascadingclasses02.png "分割ビュー コント ローラーは、詳細の表示のみが表示されます。")](unified-storyboards-images/cascadingclasses02.png#lightbox)

アプリケーションでは、開発者が横方向に iPhone でマスター/詳細の両方のビューを表示する場合で、開発者は分割ビュー コント ローラーの親コンテナーを挿入しの特徴であるコレクションをオーバーライドする必要があります。 次の図に表示します。

 [![](unified-storyboards-images/cascadingclasses03.png "開発者が分割ビュー コント ローラーの親コンテナーを挿入しの特徴であるコレクションをオーバーライドする必要があります。")](unified-storyboards-images/cascadingclasses03.png#lightbox)

A`UIView`分割ビュー コント ローラーの親として設定されていると、`SetOverrideTraitCollection`で新しい特徴であるコレクションに渡すことと、分割ビュー コント ローラーを対象とするビューのメソッドが呼び出されます。 新しい特徴であるコレクションよりも優先、`HorizontalSizeClass`に設定すると`Regular`分割ビュー コント ローラーは横方向に iPhone でマスター/詳細の両方のビューを表示するようにします。

注意してください、`VerticalSizeClass`に設定された`unspecified`、これにより、新しい特徴であるコレクションの親の特徴であるコレクションに追加する結果として、`Compact VerticalSizeClass`子分割ビュー コント ローラーのです。

### <a name="trait-changes"></a>特徴の変更

このセクションで詳細に、特徴環境が変更されたときの特徴であるコレクションの移行、外観になります。 たとえば、ときに、デバイスは縦から横に回転されます。

 [![](unified-storyboards-images/traittransitions01.png "特徴である変更の概要を横向きに縦")](unified-storyboards-images/traittransitions01.png#lightbox)

最初に、iOS 8 は、行わへの移行を準備するには、いくつか設定します。 次に、システムは、遷移の状態をアニメーション化します。 最後に、iOS 8 をクリーンアップの移行中に必要とされる、一時的な状態です。

iOS 8 では、次の表に示すように、特徴の変更に参加する、開発者が使用できるいくつかのコールバックを提供します。

|Phase|コールバック|説明|
|--- |--- |--- |
|セットアップ|<ul><li>`WillTransitionToTraitCollection`</li><li>`TraitCollectionDidChange`</li></ul>|<ul><li>特徴であるコレクションは、その新しい値に設定を取得する前に、特徴の変更の先頭にこのメソッドが呼び出されます。</li><li>特徴であるコレクションの値が変更されたときに、すべてのアニメーションが行われる前に、メソッドが呼び出されます。</li></ul>|
|アニメーション|`WillTransitionToTraitCollection`|このメソッドに渡された遷移コーディネーターは、`AnimateAlongside`により、開発者は、既定のアニメーションと共に実行されるアニメーションを追加するプロパティです。|
|クリーンアップ|`WillTransitionToTraitCollection`|開発者は、遷移が行われた後にクリーンアップ コードを含めるためのメソッドを提供します。|

`WillTransitionToTraitCollection`メソッドはの特徴であるコレクションの変更とコント ローラーの表示をアニメーション化するのに適しています。 `WillTransitionToTraitCollection`メソッドはコント ローラーの表示で使用できるのみ ( `UIViewController`) と、他の特徴である環境ではなくと同様に`UIViews`です。

`TraitCollectionDidChange`が使用するための優れた、`UIView`クラス、開発者が、特徴 (traits) が変更されるたびに UI を更新します。

### <a name="collapsing-the-split-view-controllers"></a>分割コント ローラーの表示を折りたたむ

これで、みましょう take、詳しく見てどのようなは、分割ビュー コント ローラーが 1 つの列ビューに 2 つの列から折りたたまれたときに発生します。 この変更の一環としては、2 つのプロセスを実行する必要があります。

-  既定では、分割ビュー コント ローラーは、折りたたみが発生した後、ビューとしてプライマリ ビュー コント ローラーが使用されます。 開発者はオーバーライドすることでこの動作をオーバーライドすることができます、`GetPrimaryViewControllerForCollapsingSplitViewController`のメソッド、`UISplitViewControllerDelegate`折りたたまれた状態で表示するビュー コント ローラーを提供するとします。
-  セカンダリ ビュー コント ローラーでは、プライマリ ビュー コント ローラーに結合します。 一般に、開発者は、この手順は何もする必要はありません。分割ビュー コント ローラーには、ハードウェア デバイスに基づくこのフェーズの自動処理が含まれています。 ただし、ここで、開発者と対話するこの変更によって特殊なケースがあります。 呼び出す、`CollapseSecondViewController`のメソッド、`UISplitViewControllerDelegate`詳細ビューではなくの折りたたみが発生したときに表示するマスター ビュー コント ローラーを使用します。


### <a name="expanding-the-split-view-controller"></a>分割ビュー コント ローラーを展開します。

これで、みましょう take、詳しく見てどのような分割ビュー コント ローラーが折りたたまれた状態から展開されているときに発生します。 もう一度は、発生する必要がある 2 つの段階があります。

-  最初に、新しいプライマリ ビュー コント ローラーを定義します。 既定では、分割ビュー コント ローラーは自動的に、プライマリ ビュー コント ローラーを使用、折りたたまれたビューからです。 ここでも、開発者を使用してこの動作をオーバーライドすることができます、`GetPrimaryViewControllerForExpandingSplitViewController`のメソッド、`UISplitViewControllerDelegate`です。
-  プライマリ ビュー コント ローラーを選択すると、セカンダリ ビュー コント ローラーを再作成する必要があります。 もう一度、分割ビュー コント ローラーには、ハードウェア デバイスに基づくこのフェーズの自動処理が含まれています。 開発者は呼び出すことによってこの動作をオーバーライドすることができます、`SeparateSecondaryViewController`のメソッド、`UISplitViewControllerDelegate`です。


分割ビューのコント ローラーでプライマリ ビュー コント ローラーは、展開して、実装することによって、ビューの折りたたみ両方で役割を果たします、`CollapseSecondViewController`と`SeparateSecondaryViewController`のメソッド、`UISplitViewControllerDelegate`です。 `UINavigationController` これらのメソッドを自動的にプッシュしてポップ セカンダリ ビュー コント ローラーを実装します。

### <a name="showing-view-controllers"></a>コント ローラーの表示の表示

Apple は iOS 8 に加えられたその他の変更は、開発者が、コント ローラーの表示を示しています。 以前は、アプリケーションには、リーフ ビュー コント ローラー (コント ローラーなどのテーブル ビュー)、および開発者示しました別 (たとえば、応答で、ユーザーをタップするセルを)、コント ローラーの階層を通じて、アプリケーションに到達した場合、ナビゲーション ビューのコント ローラーと呼び出し、`PushViewController`メソッドに対して、新しいビューを表示するようにします。

これには、ナビゲーション コント ローラーとで実行されていた、環境間の非常に密結合が表示されます。 Ios 8、Apple が切り離されてこの 2 つの新しいメソッドを提供することで。

-  `ShowViewController` – その環境に基づいて、新しいビューのコント ローラーを表示するを適合させます。 たとえば、`UINavigationController`をスタックに新しいビューを単にプッシュします。 分割ビュー コント ローラー、新しいビュー コント ローラーは、新しいプライマリ ビュー コント ローラーとしての左側に表示されます。 コンテナー ビュー コント ローラーが存在しない場合は、モーダル ビュー コント ローラーとして、新しいビューが表示されます。
-  `ShowDetailViewController` – と同様の方法で動作するか`ShowViewController`、分割ビュー コント ローラーに詳細ビューを置き換える、新しいビュー コント ローラーで渡されるで実装されます。 呼び出しにリダイレクトされます (iPhone アプリケーションで認識されます) と分割ビュー コント ローラーが折りたたまれている場合、`ShowViewController`メソッド、および、新しいビューは、プライマリ ビュー コント ローラーとして表示されます。 ここでも、コンテナーのビューのコント ローラーが存在しない場合は、モーダル ビュー コント ローラーとして、新しいビューが表示されます。


これらのメソッドは、リーフ ビュー コント ローラーでを起動して作業し、新しいビューの表示を処理する適切なコンテナー ビュー コント ローラーが見つかるまで、階層の表示をウォークします。

開発者が実装できます`ShowViewController`と`ShowDetailViewController`独自カスタム コント ローラーの表示を取得する同じ機能の自動化を`UINavigationController`と`UISplitViewController`を提供します。

### <a name="how-it-works"></a>しくみ

このセクションの内容をどのように、これらのメソッドを実際には 8、iOS で実装見ておになります。 最初、新しいを見てみましょう`GetTargetForAction`メソッド。

 [![](unified-storyboards-images/gettargetforaction.png "新しい GetTargetForAction メソッド")](unified-storyboards-images/gettargetforaction.png#lightbox)

このメソッドは、適切なコンテナー ビュー コント ローラーが見つかるまで、階層チェーンをについて説明します。 例えば:

1.  場合、`ShowViewController`メソッドが呼び出されると、新しいビューの親として使用されているために、このメソッドを実装するチェーンの最初のビュー コント ローラーは、ナビゲーションのコント ローラー。
1.  場合、`ShowDetailViewController`メソッドが呼び出された代わりに、分割ビュー コント ローラーは、親として使用されているため、実装する最初のビュー コント ローラー。


`GetTargetForAction`メソッドでは、特定のアクションを実装するビュー コント ローラーの検索して確認し、そのビューのコント ローラーは、そのアクションを受信する場合。 このメソッドはパブリックであるために、開発者は、関数と同じように、組み込みの独自のカスタム メソッドを作成できます`ShowViewController`と`ShowDetailViewController`メソッドです。

## <a name="adaptive-presentation"></a>アダプティブ プレゼンテーション

IOS 8、Apple が行われる重なってプレゼンテーション ( `UIPopoverPresentationController`) アダプティブもします。 重なってプレゼンテーション ビュー コント ローラー クラスでは、正規のサイズ、標準重なって表示が自動的に表示されますが、全画面表示コンパクトな水平方向にサイズ クラスで (など、iPhone で)。

提示されたコント ローラーの表示を管理する、統一されたストーリー ボード システム内の変更に合わせて、新しいコント ローラー オブジェクトが作成された —`UIPresentationController`です。 このコント ローラーは非表示になるまでビュー コント ローラーが表示される時刻から作成されます。 管理クラスは、ことができます検討する必要がスーパークラス ビュー コント ローラー上に影響を与える、ビュー コント ローラー (印刷の向き) などのプレゼンテーション コント ローラーを制御するは、フィードバック ビュー コント ローラーにデバイスの変更に応答するとき。

ビュー コント ローラーを使用して、、開発者が提示するときに、`PresentViewController`メソッド、プレゼンテーションのプロセスの管理に渡された`UIKit`です。 UIKit 処理 (特に) 作成したスタイルの正しいコント ローラー、ときにされている唯一の例外ビュー コント ローラーに設定されたスタイル`UIModalPresentationCustom`です。 ここでは、アプリケーションが独自 PresentationController はなくを使用してを提供することができます、`UIKit`コント ローラー。

### <a name="custom-presentation-styles"></a>カスタムの表示スタイル

開発者ではカスタムのプレゼンテーションの形式でカスタム プレゼンテーション コント ローラーを使用するオプションがあります。 同盟がそのビューの動作と外観を変更するのには、このカスタム コント ローラーを使用できます。

<a name="size-classes"/>

## <a name="working-with-size-classes"></a>サイズのクラスの扱い

この記事に含まれているアダプティブ写真 Xamarin プロジェクトでは、サイズのクラスとアダプティブ コント ローラーの表示を使用して、iOS 8 インターフェイスの統合アプリケーション内の実際の例を示します。

アプリケーションに UI を完全に、コードから作成 IOS デザイナーを使用してではなく、一元化されたストーリー ボードを作成するには、同じ手法を適用します。 この記事の後半は、Unified ストーリー ボードおよび iOS Xamarin アプリケーションのデザイナーでサイズのクラスを使用する方法を紹介します。

今すぐアダプティブ写真プロジェクトがでどのように実装サイズ クラス機能のいくつか iOS 8 アダプティブ アプリケーションの作成を詳しく見てみましょう。

### <a name="adapting-to-trait-environment-changes"></a>特徴である環境の変化に合わせて調整

IPhone でアダプティブ写真アプリケーションの実行中、ユーザーは、縦から横にデバイスを回転させると、分割ビュー コント ローラーには、マスターと詳細の両方のビューが表示されます。

 [![](unified-storyboards-images/rotation.png "分割ビュー コント ローラーには、両方のマスタが表示され、次に示すように詳細が表示")](unified-storyboards-images/rotation.png#lightbox)

オーバーライドすることでこれを行う、`UpdateConstraintsForTraitCollection`の値に基づいてビュー コント ローラーおよび制約の調整方法、`VerticalSizeClass`です。 例えば:

```csharp
public void UpdateConstraintsForTraitCollection (UITraitCollection collection)
{
    var views = NSDictionary.FromObjectsAndKeys (
        new object[] { TopLayoutGuide, ImageView, NameLabel, ConversationsLabel, PhotosLabel },
        new object[] { "topLayoutGuide", "imageView", "nameLabel", "conversationsLabel", "photosLabel" }
    );

    var newConstraints = new List<NSLayoutConstraint> ();
    if (collection.VerticalSizeClass == UIUserInterfaceSizeClass.Compact) {
        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|[imageView]-[nameLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("[imageView]-[conversationsLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("[imageView]-[photosLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:|[topLayoutGuide]-[nameLabel]-[conversationsLabel]-[photosLabel]",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:|[topLayoutGuide][imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.Add (NSLayoutConstraint.Create (ImageView, NSLayoutAttribute.Width, NSLayoutRelation.Equal,
            View, NSLayoutAttribute.Width, 0.5f, 0.0f));
    } else {
        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|[imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[nameLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[conversationsLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[photosLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:[topLayoutGuide]-[nameLabel]-[conversationsLabel]-[photosLabel]-20-[imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));
    }

    if (constraints != null)
        View.RemoveConstraints (constraints.ToArray ());

    constraints = newConstraints;
    View.AddConstraints (constraints.ToArray ());
}
```

### <a name="adding-transition-animations"></a>移行アニメーションを追加します。

アプリケーションがからアダプティブ写真で分割ビュー コント ローラーが展開に折りたたまれている場合でも、アニメーションはオーバーライドすることで既定アニメーションに追加されます、`WillTransitionToTraitCollection`ビュー コント ローラーのメソッドです。 例えば:

```csharp
public override void WillTransitionToTraitCollection (UITraitCollection traitCollection, IUIViewControllerTransitionCoordinator coordinator)
{
    base.WillTransitionToTraitCollection (traitCollection, coordinator);
    coordinator.AnimateAlongsideTransition ((UIViewControllerTransitionCoordinatorContext) => {
        UpdateConstraintsForTraitCollection (traitCollection);
        View.SetNeedsLayout ();
    }, (UIViewControllerTransitionCoordinatorContext) => {
    });
}
```

### <a name="overriding-the-trait-environment"></a>特徴である環境をオーバーライドします。

上に示すようアダプティブ写真アプリケーション強制的に両方の詳細を表示する分割ビュー コント ローラーとマスター ビューには、iPhone デバイスが横向きビューの場合。

これは、ビュー コント ローラーで、次のコードを使用して行われました。

```csharp
private UITraitCollection forcedTraitCollection = new UITraitCollection ();
...

public UITraitCollection ForcedTraitCollection {
    get {
        return forcedTraitCollection;
    }

    set {
        if (value != forcedTraitCollection) {
            forcedTraitCollection = value;
            UpdateForcedTraitCollection ();
        }
    }
}
...

public override void ViewWillTransitionToSize (SizeF toSize, IUIViewControllerTransitionCoordinator coordinator)
{
    ForcedTraitCollection = toSize.Width > 320.0f ?
         UITraitCollection.FromHorizontalSizeClass (UIUserInterfaceSizeClass.Regular) :
         new UITraitCollection ();

    base.ViewWillTransitionToSize (toSize, coordinator);
}

public void UpdateForcedTraitCollection ()
{
    SetOverrideTraitCollection (forcedTraitCollection, viewController);
}
```

### <a name="expanding-and-collapsing-the-split-view-controller"></a>展開と折りたたみ分割ビュー コント ローラー

[次へ] Xamarin で分割ビュー コント ローラーの展開と折りたたみの動作を実装する方法を調べてみましょう。 `AppDelegate`分割ビュー コント ローラーの作成時に、これらの変更を処理する、デリゲートが割り当てられます。

```csharp
public class SplitViewControllerDelegate : UISplitViewControllerDelegate
{
    public override bool CollapseSecondViewController (UISplitViewController splitViewController,
        UIViewController secondaryViewController, UIViewController primaryViewController)
    {
        AAPLPhoto photo = ((CustomViewController)secondaryViewController).Aapl_containedPhoto (null);
        if (photo == null) {
            return true;
        }

        if (primaryViewController.GetType () == typeof(CustomNavigationController)) {
            var viewControllers = new List<UIViewController> ();
            foreach (var controller in ((UINavigationController)primaryViewController).ViewControllers) {
                var type = controller.GetType ();
                MethodInfo method = type.GetMethod ("Aapl_containsPhoto");

                if ((bool)method.Invoke (controller, new object[] { null })) {
                    viewControllers.Add (controller);
                }
            }

            ((UINavigationController)primaryViewController).ViewControllers = viewControllers.ToArray<UIViewController> ();
        }

        return false;
    }

    public override UIViewController SeparateSecondaryViewController (UISplitViewController splitViewController,
        UIViewController primaryViewController)
    {
        if (primaryViewController.GetType () == typeof(CustomNavigationController)) {
            foreach (var controller in ((CustomNavigationController)primaryViewController).ViewControllers) {
                var type = controller.GetType ();
                MethodInfo method = type.GetMethod ("Aapl_containedPhoto");

                if (method.Invoke (controller, new object[] { null }) != null) {
                    return null;
                }
            }
        }

        return new AAPLEmptyViewController ();
    }
}
```

`SeparateSecondaryViewController`写真が表示されていると、その状態に基づいてアクションを実行するかどうかを確認するメソッドのテスト。 マスター ビュー コント ローラーが表示されないようにに表示されている写真がない場合にセカンダリ ビュー コント ローラーを折りたたみます。

`CollapseSecondViewController`メソッドは分割ビュー コント ローラーを展開するときに使用ように折りたたまれて、そのビューに戻る場合は、任意の写真が、スタック上に存在かどうかを参照してください。

### <a name="moving-between-view-controllers"></a>ビューのコント ローラー間での移動

次に、コント ローラーの表示との間でアダプティブ写真アプリケーションがどのように移動するのか見てみましょう。 `AAPLConversationViewController`クラスのユーザーが、そのテーブルからセルを選択したときに、`ShowDetailViewController`詳細ビューを表示するメソッドが呼び出されます。

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    var photo = PhotoForIndexPath (indexPath);
    var controller = new AAPLPhotoViewController ();
    controller.Photo = photo;

    int photoNumber = indexPath.Row + 1;
    int photoCount = (int)Conversation.Photos.Count;
    controller.Title = string.Format ("{0} of {1}", photoNumber, photoCount);
    ShowDetailViewController (controller, this);
}
```

### <a name="displaying-disclosure-indicators"></a>漏えいインジケーターを表示します。

アダプティブ フォト アプリケーションでは、漏えいインジケーターを非表示またはに基づいて表示の変更の特徴である環境に、いくつかの場所があります。 これは、次のコードで処理されます。

```csharp
public bool Aapl_willShowingViewControllerPushWithSender ()
{
    var selector = new Selector ("Aapl_willShowingViewControllerPushWithSender");
    var target = this.GetTargetViewControllerForAction (selector, this);

    if (target != null) {
        var type = target.GetType ();
        MethodInfo method = type.GetMethod ("Aapl_willShowingDetailViewControllerPushWithSender");
        return (bool)method.Invoke (target, new object[] { });
    } else {
        return false;
    }
}

public bool Aapl_willShowingDetailViewControllerPushWithSender ()
{
    var selector = new Selector ("Aapl_willShowingDetailViewControllerPushWithSender");
    var target = this.GetTargetViewControllerForAction (selector, this);

    if (target != null) {
        var type = target.GetType ();
        MethodInfo method = type.GetMethod ("Aapl_willShowingDetailViewControllerPushWithSender");
        return (bool)method.Invoke (target, new object[] { });
    } else {
        return false;
    }
}
```

使用して実装されて、`GetTargetViewControllerForAction`メソッド上で説明します。

テーブル ビュー コント ローラーでは、データの表示かどうか、プッシュが行われるか、およびそれに従って漏えいインジケーターを表示または非表示するかどうかを上に実装されたメソッドを使用します。

```csharp
public override void WillDisplay (UITableView tableView, UITableViewCell cell, NSIndexPath indexPath)
{
    bool pushes = ShouldShowConversationViewForIndexPath (indexPath) ?
         Aapl_willShowingViewControllerPushWithSender () :
         Aapl_willShowingDetailViewControllerPushWithSender ();

    cell.Accessory = pushes ? UITableViewCellAccessory.DisclosureIndicator : UITableViewCellAccessory.None;
    var conversation = ConversationForIndexPath (indexPath);
    cell.TextLabel.Text = conversation.Name;
}
```

### <a name="new-showdetailtargetdidchangenotification-type"></a>新しい`ShowDetailTargetDidChangeNotification`型

Apple では、新しい通知の種類はサイズ クラスと分割ビューのコント ローラー内からの特徴である環境の使用対象として追加`ShowDetailTargetDidChangeNotification`です。 コント ローラーを展開または折りたたむときなど、ターゲット分割ビュー コント ローラーの詳細の表示が変更されるたびに、この通知が送信されます。

アダプティブ写真アプリケーションでは、この通知を使用して、詳細ビュー コント ローラーが変更されたときに漏えいインジケーターの状態を更新します。

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    TableView.RegisterClassForCellReuse (typeof(UITableViewCell), AAPLListTableViewControllerCellIdentifier);
    NSNotificationCenter.DefaultCenter.AddObserver (this, new Selector ("showDetailTargetDidChange:"),
        UIViewController.ShowDetailTargetDidChangeNotification, null);
    ClearsSelectionOnViewWillAppear = false;
}
```

すべてを表示する方法のサイズのクラスがアダプティブ写真アプリケーションで詳しく見て、簡単にアプリケーションを作成する、一元化された Xamarin.iOS の特徴であるコレクションとアダプティブ コント ローラーの表示を使用できます。

## <a name="unified-storyboards"></a>統一されたストーリー ボード

新しいストーリー ボードの Unified 8、iOS してサイズの複数のクラスを対象として iPhone と iPad の両方のデバイスで表示できるストーリー ボード ファイルの統合された開発者が、1 つを作成できるようにします。 Unified ストーリー ボードを使用すると、開発者は小さい UI 固有のコードを書き込み、1 つだけのインターフェイスのデザインを作成および管理します。

ストーリー ボードの統合の主な利点は次のとおりです。

-  IPhone と iPad の同一のストーリー ボード ファイルを使用します。
-  IOS 6、iOS 7 に逆方向に展開します。
-  さまざまなデバイス、向き、および Xamarin iOS デザイナー内からすべての OS バージョンのレイアウトをプレビューします。

この機能が完全にサポートされる Visual Studio for Mac

<a name="enabling-size-classes" />

### <a name="enabling-size-classes"></a>サイズのクラスを有効にします。

既定では、新しい Xamarin.iOS プロジェクトはクラスのサイズを変更します。 サイズ クラスとアダプティブ Segues を使用して、古いプロジェクトからストーリー ボード内にする必要があります最初に変換する iOS デザイナー内から、Xcode 6 の一元化されたストーリー ボード形式。

IOS デザイナーおよびチェックに変換するストーリー ボードを開くこれを行うには、**サイズ クラスを使用** チェック ボックス。

 [![](unified-storyboards-images/sizeclass01.png "サイズのクラスを使用 チェック ボックス")](unified-storyboards-images/sizeclass01.png#lightbox)

IOS デザイナーは、開発者がサイズのクラスを使用して、ストーリー ボードの形式に変換することを確認します。

 [![](unified-storyboards-images/sizeclass02.png "アラートのサイズのクラスを使用します。")](unified-storyboards-images/sizeclass02.png#lightbox)

> [!IMPORTANT]
> 自動レイアウトは、正常に動作するサイズ クラスもチェックする必要があります。

### <a name="generic-device-types"></a>汎用的なデバイスの種類

サイズのクラスを使用して、ストーリー ボードを変換すると、その表示されるデザイン画面で、**ビューとして**デバイスはジェネリックにします。

 [![](unified-storyboards-images/sizeclass03.png "汎用的なデバイスの種類として表示します。")](unified-storyboards-images/sizeclass03.png#lightbox)

汎用的なデバイスの種類を選択すると、ビューのすべてのコント ローラーは 600 x 600 正方形にサイズ変更されます。 この四角形は、任意の幅と任意の高さのサイズを表します。 IOS デザイナーは、このモードでは、すべての編集がサイズ クラスのすべてに適用されます。

開発者は、iPhone としてデザイン画面を表示するためのオプションもあります。

 [![](unified-storyboards-images/sizeclass04.png "IPhone としてデザイン画面を表示します。")](unified-storyboards-images/sizeclass04.png#lightbox)

または iPad として表示します。

 [![](unified-storyboards-images/sizeclass05.png "IPad としてデザイン画面を表示します。")](unified-storyboards-images/sizeclass05.png#lightbox)

### <a name="select-a-size-class"></a>サイズ クラスを選択します。

サイズ クラス セレクター ボタンは、(近くにビューとしてドロップダウン) デザイン サーフェイスの左上隅には。 これにより、開発者はサイズにどのクラスは、現在編集中の選択。

 [![](unified-storyboards-images/sizeclass06.png "サイズ クラスを選択します。")](unified-storyboards-images/sizeclass06.png#lightbox)

セレクターは、3 x 3 グリッドとしてサイズ クラスの選択を表示します。 グリッドの正方形は、幅のクラスと高さの組み合わせを表します。 中央の四角形は、Any Width/Any 高さサイズ クラス (ある Unified ストーリー ボードの既定のビュー) を選択します。 この四角形を選択すると、開発者がその他のすべての構成によって継承される既定のレイアウトを編集します。

グリッドの左上隅にある四角では、Compact Compact 幅/高さサイズ クラスを表します。

 [![](unified-storyboards-images/sizeclass07.png "Compact 幅/Compact 高さサイズ クラス")](unified-storyboards-images/sizeclass07.png#lightbox)

このモードは横方向に iPhone に対応します。 グリッドの右下隅にある四角には、正規標準幅/高さのサイズを表すクラスを iPad を表しています。

 [![](unified-storyboards-images/sizeclass08.png "正規の幅/正規高さサイズ クラス")](unified-storyboards-images/sizeclass08.png#lightbox)

縦向きで iPhone のレイアウトを編集するには、左下隅で、正方形を選択します。 これは、コンパクトな標準幅/高さサイズ クラスを表します。

 [![](unified-storyboards-images/sizeclass09.png "Compact 幅/Regular 高さサイズ クラス")](unified-storyboards-images/sizeclass09.png#lightbox)

それを選択する四角形にクリックし、デザイン サーフェイスが新しい選択内容に合わせてコント ローラーの表示のサイズを変更します。

 [![](unified-storyboards-images/sizeclass10.png "デザイン画面に示すように、新しい選択を一致するコント ローラーの表示のサイズを変更します。")](unified-storyboards-images/sizeclass10.png#lightbox)

サイズのクラスおよび Iphone と Ipad のレイアウトへの影響についての詳細については、この記事のサイズのクラスを参照してください。

### <a name="adaptive-segue-types"></a>アダプティブ話題型

開発者が前に、ストーリー ボードを使用するかどうかは、既存の segue 種類の精通することが**プッシュ**、**モーダル**と**重なって**です。 一元化されたストーリー ボード ファイルには、サイズのクラスが有効な場合、次アダプティブ話題の種類 (上で説明した新しいビュー コント ローラーの API に対応) が利用可能な:**表示**と**詳細の表示**.

> [!IMPORTANT]
> Segues 既存サイズ クラスを有効にすると、新しい型に変換します。

IOS の例に単純なゲーム ナビゲーション メニューをマスター ビューの分割ビュー コント ローラーで、一元化されたストーリー ボードを使用するアプリケーションが 8 を実行します。 ユーザーは、メニュー ボタンをクリックすると、選択した項目のビューのコント ローラー時に表示される分割ビュー コント ローラーの [詳細] セクションで iPad で実行されています。 IPhone では、アイテムのビューのコント ローラーをナビゲーション スタックにプッシュされる必要があります。

この特殊効果を実現するために iOS デザイナーでコントロールのボタンをクリックしますし、行を表示するビュー コント ローラーにドラッグします。 マウス ボタンが離されたときに選択`Show Detail`話題型ポップアップ メニューから。

 [![](unified-storyboards-images/segue01.png "詳細の表示 メニューから選択型ポップアップの話題")](unified-storyboards-images/segue01.png#lightbox)

新しい segue は、ボタンとビューのコント ローラーの間で作成されます。 IPhone シミュレーターでアプリケーションを実行し、メイン メニューが表示されます。

 [![](unified-storyboards-images/segue02.png "メイン メニュー")](unified-storyboards-images/segue02.png#lightbox)

をクリックして、**選択ゲーム**ボタンをクリックし、項目のビューのコント ローラーのナビゲーション スタックにプッシュされます。

 [![](unified-storyboards-images/segue03.png "ようにその項目ビュー コント ローラーがナビゲーション スタックにプッシュされます。")](unified-storyboards-images/segue03.png#lightbox)

IPhone シミュレーターを停止し、iPad シミュレーターでアプリケーションを実行します。 横向きし、メイン スイッチをもう一度メニューが表示されます。

 [![](unified-storyboards-images/segue04.png "メイン メニューを表示")](unified-storyboards-images/segue04.png#lightbox)

もう一度、をクリックして、**選択ゲーム**分割ビュー コント ローラーの [詳細] セクションで、ボタンと、項目のビューのコント ローラーが表示されます。

 [![](unified-storyboards-images/segue05.png "分割ビュー コント ローラーの詳細セクションに示すようにビュー コント ローラーの項目")](unified-storyboards-images/segue05.png#lightbox)

### <a name="excluding-an-element-from-a-size-class"></a>サイズのクラスからの要素の除外

(ビュー、コントロール、制約など) の指定された要素がありません必要な場合、特定のサイズ クラスの内部でもあります。 要素サイズ クラスから、除外するで除外する目的の項目を選択、**デザイン サーフェイス**です。 一番下までスクロール、**プロパティ エクスプ ローラー**  をクリックし、**歯車**ドロップダウン メニュー。 組み合わせを選択**幅**と**高さ**から項目を除外します。

[![](unified-storyboards-images/exclude-a.png "幅と高さの組み合わせを選択します。")](unified-storyboards-images/exclude-a.png#lightbox)

新しい*除外ケース*の下部にある要素に追加されます、**プロパティ エクスプ ローラー**です。 次に、オフにして、**インストール**サイズの指定したクラスのチェック ボックス。

[![](unified-storyboards-images/exclude-b.png "インストールされているチェック ボックスをオフします。")](unified-storyboards-images/exclude-b.png#lightbox)

幅と高さから除外すると、アイテムを切り替える、デザイン画面、サイズの指定したクラスが、全体的な UI 設計されませんから削除されています。

 [![](unified-storyboards-images/exclude02.png "幅と高さから除外すると、アイテムを切り替える、デザイン画面")](unified-storyboards-images/exclude02.png#lightbox)

切り替え復帰 Any Width/Any 高さサイズ クラスと要素は、引き続き有効では。

 [![](unified-storyboards-images/exclude03.png "切り替え復帰 Any Width/Any 高さサイズ クラス")](unified-storyboards-images/exclude03.png#lightbox)

アプリケーションは、iPad シミュレーターで実行すると、要素が表示されます。

 [![](unified-storyboards-images/exclude04.png "時に表示要素 iPad シミュレーターで実行中のアプリ")](unified-storyboards-images/exclude04.png#lightbox)

および要素が不足しているアプリケーションは iPhone シミュレーターで実行されるときに。

 [![](unified-storyboards-images/exclude05.png "要素が見つからない場合に iPhone シミュレーターで実行中のアプリ")](unified-storyboards-images/exclude05.png#lightbox)

要素から除外ケースを削除する内の要素を選択するだけ、**デザイン サーフェイス**の一番下までスクロール、**プロパティ エクスプ ローラー**  をクリックし、  **-** を削除する場合の横にあるボタンをクリックします。

ストーリー ボードの統合の実装を表示するを見て、`UnifiedStoryboard`サンプル Xamarin iOS 8 のアプリケーションがこのドキュメントにアタッチされています。

## <a name="dynamic-launch-screens"></a>動的な起動画面

起動画面のファイルは、アプリが実際に開始アップことをユーザーにフィードバックを提供する iOS アプリケーションを起動するときに、スプラッシュ スクリーンとして表示されます。 8、iOS の前に、開発者が、複数を含める`Default.png`イメージの資産を各デバイスの種類、方向、および画面解像度でアプリケーションを実行するとします。 たとえば、 `Default@2x.png`、 `Default-Landscape@2x~ipad.png`、 `Default-Portrait@2x~ipad.png`, などです。

計算に入れて新しい iPhone 6 および iPhone 6 Plus デバイス (今後 Apple Watch) と、すべての既存の iPhone および iPad デバイス、さまざまなサイズや向きの解像度の大きな配列を表します`Default.png`スタートアップ画面のイメージの資産には、する必要があります作成および管理します。 さらに、これらのファイルは非常に大きくなることができは「膨張」、成果物のアプリケーション バンドルを itunes アプリ ストアで (おそらくおく移動体通信ネットワーク経由で配信することができません)、アプリケーションをダウンロードするために必要な時間の量を増やすおよびエンドユーザーのデバイスで必要な記憶域の量を増やします。

新しい 8、iOS を開発者が 1 つのアトミックを作成できます`.xib`自動レイアウトおよびサイズ クラスの作成を使用した Xcode でファイル、*動的な起動画面*は、すべてのデバイス、解像度や向き動作をします。 これだけでなくを作成し、すべての必要なイメージ資産を保持する開発者の必要な作業の量が減少しますが、アプリケーションのインストールされているバンドルのサイズが大幅に軽減されます。


動的な起動画面は、次の制限事項と考慮事項があります。

 - のみを使用して`UIKit`クラスです。
 - ある単一のルート ビューを使用して、`UIView`または`UIViewController`オブジェクト。
 - アプリケーションのコードへの接続を作成しない (コードを追加しないで**アクション**または**コンセント**)。
 - 追加しない`UIWebView`オブジェクト。
 - 任意のカスタム クラスを使用しないでください。
 - ランタイムの属性を使用しないでください。

注意上記のガイドラインには、既存の Xamarin iOS 8 プロジェクトへの動的な起動画面の追加を見てみましょう。

次の手順で行います。

1. 開いている**Visual Studio for Mac**と読み込み、**ソリューション**に動的な起動画面を追加します。
2. **ソリューション エクスプ ローラー**を右クリックし、`MainStoryboard.storyboard`ファイルおよび選択した**ファイルを開く** > **Xcode インターフェイス ビルダー**:

    [![](unified-storyboards-images/dls01.png "Xcode インターフェイス ビルダーで開く")](unified-storyboards-images/dls01.png#lightbox)
3. Xcode で次のように選択します**ファイル** > **新規** > **ファイル...**:

    [![](unified-storyboards-images/dls02.png "ファイルを選択/新規")](unified-storyboards-images/dls02.png#lightbox)
4. 選択**iOS** > **ユーザー インターフェイス** > **起動画面** をクリックし、**次**ボタン。

    [![](unified-storyboards-images/dls03.png "IOS を選択して、ユーザー インターフェイス//起動画面")](unified-storyboards-images/dls03.png#lightbox)
5. ファイルの名前を付けます`LaunchScreen.xib` をクリックし、**作成**ボタン。

    [![](unified-storyboards-images/dls04.png "LaunchScreen.xib ファイルを名前します。")](unified-storyboards-images/dls04.png#lightbox)
6. グラフィック要素を追加して、レイアウトの制約を使用して、指定されたデバイス、向きと画面サイズに配置して起動画面のデザインを編集します。

    [![](unified-storyboards-images/dls05.png "起動画面のデザインの編集")](unified-storyboards-images/dls05.png#lightbox)
7. 変更を保存`LaunchScreen.xib`です。
8. 選択、**アプリケーション ターゲット**と**全般** タブ。

    [![](unified-storyboards-images/dls06.png "ターゲットのアプリケーションと、[全般] タブを選択します。")](unified-storyboards-images/dls06.png#lightbox)
9. をクリックして、**選択 Info.plist**ボタン、、 `Info.plist` for Xamarin アプリをクリック、**選択**ボタン。

    [![](unified-storyboards-images/dls07.png "Xamarin アプリの Info.plist を選択します。")](unified-storyboards-images/dls07.png#lightbox)
10. **アプリのアイコンと [イメージの起動**] セクションで、開く、**スクリーン ファイルを起動**ドロップダウンを選択し、`LaunchScreen.xib`上記で作成されました。

    [![](unified-storyboards-images/dls08.png "選択、LaunchScreen.xib")](unified-storyboards-images/dls08.png#lightbox)
11. ファイルに変更を保存し、for mac を Visual Studio に戻る
12. Visual Studio for Mac Xcode での変更の同期を完了するまで待ちます。
13. **ソリューション エクスプ ローラー**を右クリックし、**リソース**フォルダーと選択**追加** > **ファイルを追加しています.**:

    [![](unified-storyboards-images/dls09.png "選択は、追加/ファイルを追加しています.")](unified-storyboards-images/dls09.png#lightbox)
14. 選択、`LaunchScreen.xib`上記で作成したファイルをクリック、**開く**ボタン。

    [![](unified-storyboards-images/dls10.png "LaunchScreen.xib ファイルを選択します。")](unified-storyboards-images/dls10.png#lightbox)
15. アプリケーションをビルドします。

### <a name="testing-the-dynamic-launch-screen"></a>動的な起動画面のテスト

Mac 用 Visual Studio で、iPhone 4 の Retina シミュレーターを選択し、アプリケーションを実行します。 動的な起動画面の向き、正しい形式で表示されます。

[![](unified-storyboards-images/dls11.png "垂直方向に表示される動的な起動画面")](unified-storyboards-images/dls11.png#lightbox)

Mac 用の Visual Studio でアプリケーションを停止し、iPad の iOS 8 デバイスを選択します。 アプリケーションを実行し、起動画面が正しく書式設定されるこのデバイスと印刷の向きの。

[![](unified-storyboards-images/dls12.png "水平方向に表示される動的な起動画面")](unified-storyboards-images/dls12.png#lightbox)

Mac 用 Visual Studio に戻るし、アプリケーションの実行を停止します。

### <a name="working-with-ios-7"></a>IOS 7 の操作

IOS 7 との下位互換性を維持するためにのみ含める通常`Default.png`iOS 8 アプリケーションで通常どおり資産のイメージです。 iOS は、以前の動作に戻るし、iOS 7 デバイスで実行されているときに、起動画面としてそれらのファイルを使用します。

Xamarin で動的起動画面の実装を表示するを見て、[動的な起動画面](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/)サンプル iOS 8 アプリケーションがこのドキュメントにアタッチされています。

## <a name="summary"></a>まとめ

この記事では、サイズのクラスと iPhone および iPad デバイスでのレイアウトへの影響を簡単に見て所要時間。 これは、統合インターフェイスを作成するサイズ クラス特徴 (traits)、特徴の環境およびの特徴であるコレクションを使用する方法について説明します。 これには、アダプティブ コント ローラーの表示について簡単に説明と統合されたインターフェイスの内部でサイズのクラスと動作がかかりました。 Xamarin iOS 8 アプリケーション内部の c# コードからサイズ クラスとインターフェイスの統合を完全に実装する、説明しました。

最後に、この記事は、Xamarin iOS は iOS デバイスで機能するデザイナーでの一元化されたストーリー ボードの作成の基本を説明し、1 つの動的な起動画面を作成する、すべての iOS 8 デバイスに起動画面として表示されます。

## <a name="related-links"></a>関連リンク

- [アダプティブ写真 (サンプル)](https://developer.xamarin.com/samples/monotouch/ios8/AdaptivePhotos/)
- [StoryboardIntro サンプル](https://developer.xamarin.com/samples/monotouch/StoryboardIntro/)
- [動的な起動画面 (サンプル)](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/)
- [iOS 8 の概要](~/ios/platform/introduction-to-ios8.md)
- [IOS8 - 進化 2014 (ビデオ) での動的なレイアウト](http://youtu.be/f3mMGlS-lM4)
- [UIPresentationController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPresentationController_class/)
- [UIImageAsset](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset)
