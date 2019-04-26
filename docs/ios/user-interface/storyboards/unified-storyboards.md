---
title: Xamarin.iOS で統一されたストーリー ボード
description: このドキュメントでは、Xamarin.iOS で統一されたストーリー ボードについて説明します。 統合ストーリー ボードには、1 つのインターフェイス定義を持つ複数の画面サイズをサポートするために開発者ができるようにします。
ms.prod: xamarin
ms.assetid: F6F70374-FC2A-4401-A712-A16D0F9B340F
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: 26aeaa3d230a5c104014edd899b8d9231ced31e9
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61430502"
---
# <a name="unified-storyboards-in-xamarinios"></a>Xamarin.iOS で統一されたストーリー ボード

iOS 8 ユーザー インターフェイスを作成するため、簡単に使用する新しいメカニズムが含まれています-統合されたストーリー ボード。 別のハードウェアの画面サイズのすべてをカバーする 1 つにストーリー ボードは、高速で応答性のビューを作成することができます、"設計-1 回、使用して多"のスタイル。

開発者は、iPhone および iPad デバイスの独立した、特定のストーリー ボードを作成する必要がなくなりと共通インターフェイスを備えたアプリケーションを設計し、そのインターフェイスの別のサイズ クラスをカスタマイズする柔軟性があります。 これにより、アプリケーションは、各フォーム ファクターの長所に適応させることができ、最適なエクスペリエンスを提供する各ユーザー インターフェイスを調整できます。

<a name="size-classes" />

## <a name="size-classes"></a>サイズ クラス

IOS 8 の場合は、前に、開発者が使用される`UIInterfaceOrientation`と`UIInterfaceIdiom`縦長と横長モードと iPhone および iPad デバイス間で区別するためにします。 印刷の向きとデバイスの決定、iOS8 でを使用して、*サイズ クラス*します。

デバイスは垂直および水平方向の両方の軸でサイズのクラスによって定義され、iOS 8 でのサイズ クラスの 2 種類があります。

-  **通常**– これは、(iPad) などの大きな画面サイズ、または大きなサイズのという印象を与えるガジェット (など、 `UIScrollView`
-  **Compact** – これは比較的小さなデバイス (iPhone) などの。 このサイズでは、デバイスの向きが考慮されます。


2 つの概念をまとめて使用する場合、結果は次の図に示すように、両方、異なる向きで使用できるさまざまな可能なサイズを定義する 2 x 2 グリッドに。

 [![](unified-storyboards-images/sizeclassgrid.png "標準モードと最適化の向きで使用できるさまざまな可能なサイズを定義する 2 x 2 グリッド")](unified-storyboards-images/sizeclassgrid.png#lightbox)

開発者は、4 つのこと (上の図に表示) と、さまざまなレイアウトになる可能性のいずれかを使用するビュー コント ローラーを作成できます。

### <a name="ipad-size-classes"></a>iPad サイズ クラス

サイズが多いため、iPad が、**正規**クラスの両方向のサイズ。

 [![](unified-storyboards-images/image1.png "iPad サイズ クラス")](unified-storyboards-images/image1.png#lightbox)


### <a name="iphone-size-classes"></a>iPhone サイズ クラス

IPhone では、デバイスの向きに基づいてさまざまなサイズ クラスがあります。

 [![](unified-storyboards-images/iphonesizeclasses.png "iPhone サイズ クラス")](unified-storyboards-images/iphonesizeclasses.png#lightbox)

-  デバイスは、縦向きモードが、画面に、 **compact**水平方向にクラスと**正規**垂直方向に
-  横モードでは、画面のクラスは、縦モードから取り消されます。

### <a name="iphone-6-plus-size-classes"></a>iPhone 6 Plus サイズ クラス

サイズでは、以前の Iphone 縦向きが横で別々 の場合と同じです。

[![](unified-storyboards-images/iphone6sizeclasses.png "iPhone 6 Plus サイズ クラス")](unified-storyboards-images/iphone6sizeclasses.png#lightbox)

IPhone 6 Plus が十分な大きさの画面を横モードで正規の幅のサイズ クラスを用意することができます。

### <a name="support-for-a-new-screen-scale"></a>新しい画面のスケールのサポート

IPhone 6 Plus で画面のスケール ファクター 3.0 (3 回元 iPhone 画面の解像度) の新しい HD の Retina ディスプレイを使用します。 これらのデバイスで最高のエクスペリエンスを提供するには、この画面のスケール用に設計された新しいアートワークを含めます。 Xcode 6 以降で資産カタログが 1 倍、2 x、および 3 x サイズ; でイメージを含めることができます。単に追加、新しいイメージの資産および iOS は、iPhone 6 で実行されている場合に、適切な資産に選択されますとします。

IOS での動作も読み込む画像認識、`@3x`イメージ ファイルのサフィックス。 たとえば、開発者が、次のファイル名と、アプリケーションのバンドルに (さまざまな解像度) でのイメージ アセットに含まれています: `MonkeyIcon.png`、 `MonkeyIcon@2x.png`、および`MonkeyIcon@3x.png`します。 Iphone 6 Plus、`MonkeyIcon@3x.png`開発者は、次のコードを使用してイメージを読み込む場合、イメージを自動的に使用されます。

```csharp
UIImage icon = UIImage.FromFile("MonkeyImage.png");
```
場合は、iOS を使用して UI 要素に、イメージを割り当てる、またはデザイナーとして`MonkeyIcon.png`、`MonkeyIcon@3x.png`で使用される、自動的に再 iPhone 6 Plus です。

<a name="dynamic-launch-screens" />

### <a name="dynamic-launch-screens"></a>動的な起動画面

アプリが実際に開始をユーザーにフィードバックを提供する iOS アプリケーションを起動するときに、スプラッシュ スクリーンとして起動画面のファイルが表示されます。 IOS 8 の場合は、前に、開発者が 複数を含める必要があります`Default.png`のイメージ アセットをアプリケーションで実行されるデバイスの種類、方向、および画面解像度ごと。

新しい ios 8 の場合は、開発者が 1 つのアトミックを作成できます`.xib`自動レイアウトとサイズ クラスを使用して作成する Xcode でファイルを*動的な起動画面*は、すべてのデバイス、解像度、向きの動作をします。 これだけでなく、開発者を作成し、すべての必要なイメージ資産の管理の必要な作業の量が減少が、アプリケーションのインストールされているバンドルのサイズが削減されます。

## <a name="traits"></a>Traits

特徴は、その環境の変更、レイアウトの変更を特定するのに使用できるプロパティです。 プロパティのセットで構成されます (、`HorizontalSizeClass`と`VerticalSizeClass`に基づいて`UIUserInterfaceSizeClass`)、インターフェイスの表現形式と ( `UIUserInterfaceIdiom`) と表示スケールします。

Apple が特徴であるコレクションとして参照するコンテナーでまとめ、上記の状態のすべて ( `UITraitCollection`)、だけでなく、プロパティもその値が含まれています。

## <a name="trait-environment"></a>特徴である環境

特徴の環境は iOS 8 の新しいインターフェイスとは、次のオブジェクトの特徴であるコレクションを返すことができません。

-  画面 ( `UIScreens` )。
-  Windows ( `UIWindows` )。
-  コント ローラーの表示 ( `UIViewController` )。
-  ビュー ( `UIView` )。
-  プレゼンテーションのコント ローラー ( `UIPresentationController` )。


開発者は、ユーザー インターフェイスのレイアウト方法を決定するのに特徴環境によって返される特徴であるコレクションを使用します。

階層は、次の図に示すようすべての特徴である環境のこと。

 [![](unified-storyboards-images/viewhierarchy.png "特徴である環境階層図")](unified-storyboards-images/viewhierarchy.png#lightbox)

属性がコレクション子環境を親からの既定で、フローは、上記の特徴である環境があること。

特徴である環境には、現在の特徴であるコレクションを取得するには、だけでなく、`TraitCollectionDidChange`メソッドで、ビューまたはビュー コント ローラーのサブクラスで上書きできます。 開発者は、これらの特徴が変更されたときに、特徴に依存している UI 要素のいずれかを変更するのに、このメソッドを使用できます。

## <a name="typical-trait-collections"></a>一般的な特徴であるコレクション

このセクションでは、ユーザーが iOS 8 を使用する場合に発生する特徴であるコレクションの一般的な種類を説明します。

開発者は、iPhone で表示される一般的な特徴であるコレクションを次に示します。

|プロパティ|[値]|
|--- |--- |
|`HorizontalSizeClass`|Compact|
|`VerticalSizeClass`|Regular|
|`UserInterfaceIdom`|電話番号|
|`DisplayScale`|2.0|

すべての特徴であるプロパティの値があるために、上記のセットは、完全修飾の特徴であるコレクションを表します。

その値の一部が不足している特徴であるコレクションが存在することも (Apple として参照する*未指定*)。

|プロパティ|[値]|
|--- |--- |
|`HorizontalSizeClass`|Compact|
|`VerticalSizeClass`|指定されていません。|
|`UserInterfaceIdom`|指定されていません。|
|`DisplayScale`|指定されていません。|

一般に、ただし、開発者では、その特徴であるコレクションの特徴である環境が表示されたらがコレクションを返す完全修飾の上記の例に示す。

現在のビュー階層内で (ビューやビュー コント ローラー) などの特徴である環境でない場合、開発者可能性があります値を取得戻る指定されていないの特徴であるプロパティの 1 つ以上の。

など、Apple によって提供される作成方法のいずれかを使用している場合、開発者は部分的に修飾の特徴であるコレクションを取得または`UITraitCollection.FromHorizontalSizeClass`、新しいコレクションを作成します。

複数の特徴であるコレクションに対して実行できる 1 つの操作と比較する、互いにもう 1 つが含まれていない 1 つの特徴であるコレクションを確認する必要があります。 意味*コンテインメント*、2 番目のコレクションで指定されたすべての特徴の値必要がありますと一致最初のコレクション内の値と完全にします。

テストする 2 つの特徴を使用して、`Contains`のメソッド、`UITraitCollection`をテストする特徴の値を渡します。

開発者が決定するコードで、比較を手動で実行できるレイアウト ビューまたはコント ローラーの表示方法。 ただし、`UIKit`など、外観プロキシのように、その機能の一部を提供するこのメソッドを内部的に使用します。

## <a name="appearance-proxy"></a>外観のプロキシ

外観プロキシは、開発者は、ビューのプロパティのカスタマイズを許可する iOS の以前のバージョンで導入されました。 Ios 8 の特徴であるコレクションをサポートするために拡張されました。

外観のプロキシが、新しいメソッドが追加されました`AppearanceForTraitCollection`に渡された、指定されたコレクションの特徴の外観の新しいプロキシを返します。 開発者を実行するカスタマイズ外観プロキシのみ有効になる準拠しているビュー、指定された特徴のコレクション。

開発者が部分的に指定された属性がコレクションに渡すは一般に、`AppearanceForTraitCollection`直前に指定した水平方向のサイズ クラスのコンパクトなコンパクトな水平方向には、アプリケーション内の任意のビューをカスタマイズすることができるようにするものなどのメソッド。

## <a name="uiimage"></a>UIImage

Apple が特徴であるコレクションに追加する別のクラス`UIImage`します。 過去に、開発者を指定する必要がある、@1Xと@2x(アイコン) などのアプリケーションに組み込む予定がある任意のビットマップ グラフィック資産のバージョン。

iOS 8 は、開発者の特徴であるコレクションに基づくイメージ カタログ内の複数バージョンのイメージを含めるように拡張されています。 たとえば、開発者は、コンパクトな特徴クラスとその他のコレクションの完全な画像のサイズの小さい画像を含めることが。

内部で使用される、イメージの 1 つ、`UIImageView`クラス、イメージのビューがその特徴であるコレクションの正しいバージョンのイメージを自動的に表示します。 します。 (横向きに縦向きからデバイスを切り替えるユーザー) などの特徴である環境が変更された場合のイメージ ビューが自動的に新しい特徴のコレクションと一致し、イメージの中の現在のバージョンの一致するように、サイズを変更する新しいイメージのサイズを選択します。表示されます。

## <a name="uiimageasset"></a>UIImageAsset

Apple がという iOS 8 を新しいクラスを追加`UIImageAsset`イメージの選択をさらに多くの制御を開発者に提供します。

イメージ アセットには、イメージのさまざまなバージョンのすべてをラップし、により、開発者に渡された特徴であるコレクションに一致する特定のイメージを要求します。 イメージを追加または削除、イメージの資産からは、稼働中です。

イメージの資産の詳細については、Apple を参照してください。 [UIImageAsset](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset)ドキュメント。

## <a name="combining-trait-collections"></a>結合の特徴であるコレクション

開発者が特徴であるコレクションで実行できる別の関数は、追加する 2 つ同時がの結果として 1 つのコレクションから指定されていない値は、2 つ目で指定された値を置き換え、結合されたコレクション。 これを使用して、`FromTraitsFromCollections`のメソッド、`UITraitCollection`クラス。

前述のよう、特徴のいずれかの特徴であるコレクションのいずれかで指定されていない場合は、別の指定した場合、値は指定されたバージョンに設定されます。 ただし、複数のバージョンの指定された特定の値がある場合は、過去の値の特徴であるコレクション値になります、ために使用されます。

## <a name="adaptive-view-controllers"></a>アダプティブ ビュー コント ローラー

このセクションでは、iOS ビューとビュー コント ローラーが自動的に、開発者のアプリケーションで適応性を特徴とサイズ クラスの概念を採用する方法の詳細を説明します。

### <a name="split-view-controller"></a>分割ビュー コント ローラー

IOS 8 では、最も変更されたビュー コント ローラー クラスの 1 つは、`UISplitViewController`クラス。 以前は、開発者は、多くの場合、分割ビュー コント ローラーを使用して iPad のバージョンのアプリケーションでと iPhone バージョンのアプリのビュー階層の完全に別のバージョンを提供するが、します。

IOS 8 で、`UISplitViewController`クラスは、両方のプラットフォーム (iPad および iPhone) で使用可能な開発者は、iPhone と iPad の両方の機能を 1 つのビュー コント ローラーの階層を作成します。

IPhone が横にあるときは、分割ビュー コント ローラーで、iPad に表示されている場合と同様にそのビュー サイド バイ サイドが表示されます。

### <a name="overriding-traits"></a>特徴のオーバーライド

分割ビュー コント ローラーを示す、横向きで iPad を次の図のように、子コンテナーまでの親コンテナーからの特徴である環境が重ねて表示します。

 [![](unified-storyboards-images/cascadingclasses01.png "横方向に iPad での分割ビュー コント ローラー")](unified-storyboards-images/cascadingclasses01.png#lightbox)

IPad では、水平および垂直方向の両方に通常のサイズ クラスが含まれるために、マスター/詳細の両方のビューが分割ビューに表示されます。

Iphone でサイズ クラスが両方向でコンパクトで、ある分割ビュー コント ローラーでは、次に示すように詳細の表示がのみ表示します。

 [![](unified-storyboards-images/cascadingclasses02.png "分割ビュー コント ローラーは、詳細の表示のみが表示されます。")](unified-storyboards-images/cascadingclasses02.png#lightbox)

開発者が横方向に iPhone でマスター/詳細の両方のビューを表示するアプリケーション、開発者が分割ビュー コント ローラーの親コンテナーに挿入し、特徴のコレクションをオーバーライドする必要があります。 次の図に示すようにします。

 [![](unified-storyboards-images/cascadingclasses03.png "開発者が分割ビュー コント ローラーの親コンテナーを挿入し、特徴であるコレクションをオーバーライドする必要があります。")](unified-storyboards-images/cascadingclasses03.png#lightbox)

A`UIView`が分割ビュー コント ローラーの親として設定し、`SetOverrideTraitCollection`メソッドは、ビューの新しい特徴のコレクションを渡すと、分割ビュー コント ローラーを対象とする呼び出されます。 新しい特徴コレクションよりも優先、`HorizontalSizeClass`に設定すると`Regular`分割ビュー コント ローラーは横方向に iPhone でマスター/詳細の両方のビューを表示するようにします。

なお、`VerticalSizeClass`に設定された`unspecified`がその結果、親での特徴であるコレクションに追加する新しい特徴のコレクションを許可、`Compact VerticalSizeClass`子分割ビュー コント ローラーの。

### <a name="trait-changes"></a>特徴の変更

このセクションでの特徴である環境が変更されたときの特徴であるコレクションの移行の詳細に見てになります。 たとえば、ときに、デバイスは縦から横に回転されます。

 [![](unified-storyboards-images/traittransitions01.png "特徴の変更の概要を横向きに縦向き")](unified-storyboards-images/traittransitions01.png#lightbox)

最初に、iOS 8 は、いくつかの設定を行う移行の準備をします。 次に、システムでは、遷移の状態がアニメーション化します。 最後に、iOS 8 クリーンアップの移行中に必要な一時的な状態です。

iOS 8 では、次の表に示すように、特徴の変更に参加する、開発者が使用できるいくつかのコールバックを提供します。

|Phase|コールバック|説明|
|--- |--- |--- |
|セットアップ|<ul><li>`WillTransitionToTraitCollection`</li><li>`TraitCollectionDidChange`</li></ul>|<ul><li>特徴であるコレクションは、その新しい値に設定を取得する前に、特徴の変更の先頭にこのメソッドが呼び出されます。</li><li>メソッドは、アニメーションの実行前に、特徴であるコレクションの値が変更されたときに呼び出されます。</li></ul>|
|アニメーション|`WillTransitionToTraitCollection`|このメソッドに渡される遷移のコーディネーターが、`AnimateAlongside`により、開発者は、既定のアニメーションと共に実行されるアニメーションを追加するプロパティ。|
|クリーンアップ|`WillTransitionToTraitCollection`|開発者は、移行が行われた後に、独自のクリーンアップ コードを含めるためのメソッドを提供します。|

`WillTransitionToTraitCollection`メソッドが特徴であるコレクションの変更と共にビュー コント ローラーをアニメーション化に適しています。 `WillTransitionToTraitCollection`メソッドは、ビュー コント ローラーの使用のみ ( `UIViewController`) とその他の特徴である環境ではなくなど`UIViews`します。

`TraitCollectionDidChange`操作に最適ですが、`UIView`クラス、開発者が、特徴を変更すると、UI を更新します。

### <a name="collapsing-the-split-view-controllers"></a>分割ビュー コント ローラーを折りたたむ

これで、みましょう take について詳しく見てどのようなは、分割ビュー コント ローラーが 2 つの列から 1 つの列のビューを縮小するときに発生します。 この変更の一環として、2 つのプロセスを実行する必要があります。

-  既定で分割ビュー コント ローラーは、折りたたみが行われた後、ビューとしてプライマリ ビュー コント ローラーが使用されます。 開発者は、オーバーライドすることでこの動作をオーバーライドできます、`GetPrimaryViewControllerForCollapsingSplitViewController`のメソッド、`UISplitViewControllerDelegate`と折りたたまれた状態で表示するビュー コント ローラーを提供します。
-  セカンダリのビュー コント ローラーでは、プライマリのビュー コント ローラーにマージされます。 一般に、開発者は、この手順では; の操作を行う必要はありません。分割ビュー コント ローラーには、このフェーズがハードウェア デバイスでの自動処理が含まれています。 ただし、場所、開発者が、この変更と対話するでしょう。 いくつかの特殊なケースがあります。 呼び出す、`CollapseSecondViewController`のメソッド、`UISplitViewControllerDelegate`詳細ビューではなく、折りたたみが発生したときに表示するマスター ビュー コント ローラーを使用します。


### <a name="expanding-the-split-view-controller"></a>分割ビュー コント ローラーを展開します。

これで、みましょう折りたたまれた状態の分割ビュー コント ローラーが展開されている場合を見て何が発生します。 もう一度は、発生する必要がある 2 つの段階があります。

-  まず、新しいプライマリ ビュー コント ローラーを定義します。 既定では、分割ビュー コント ローラーは、折りたたまれたビューからプライマリのビュー コント ローラーを自動的に使用されます。 ここでも、開発者を使用してこの動作をオーバーライドすることができます、`GetPrimaryViewControllerForExpandingSplitViewController`のメソッド、`UISplitViewControllerDelegate`します。
-  プライマリのビュー コント ローラーが選択されると、セカンダリのビュー コント ローラーを再作成する必要があります。 ここでも、分割ビュー コント ローラーには、このフェーズがハードウェア デバイスでの自動処理が含まれています。 開発者は呼び出すことでこの動作をオーバーライドすることができます、`SeparateSecondaryViewController`のメソッド、`UISplitViewControllerDelegate`します。


分割ビューのコント ローラーでプライマリ ビュー コント ローラーが両方の展開と実装することで、ビューの折りたたみで役割を果たします、`CollapseSecondViewController`と`SeparateSecondaryViewController`のメソッド、`UISplitViewControllerDelegate`します。 `UINavigationController` 自動的にプッシュして、セカンダリのビュー コント ローラーをポップするこれらのメソッドを実装します。

### <a name="showing-view-controllers"></a>ビュー コント ローラーの表示

Apple は iOS 8 に加えられたその他の変更では、開発者がビュー コント ローラーに表示されるようにします。 以前は、アプリケーションにはリーフのビュー コント ローラー (テーブル ビュー コント ローラーなど)、および開発者示しました別 (たとえば、応答でのセルにユーザーのタップに)、コント ローラーの階層を遡って、アプリケーションに到達する場合、ナビゲーション ビュー コント ローラーと呼び出し、`PushViewController`メソッドに対して新しいビューを表示するようにします。

これには、ナビゲーション コント ローラーおよび環境で実行されていたを非常に密接に結合が表示されます。 Ios 8、Apple がによって分離されているこの 2 つの新しいメソッドを指定できます。

-  `ShowViewController` – その環境に基づく新しいビュー コント ローラーの表示に適応します。 たとえば、`UINavigationController`スタックに新しいビューを単純にプッシュします。 分割ビュー コント ローラーでは、新しいビュー コント ローラーは、新しいプライマリ ビュー コント ローラーとしての左側にある表示されます。 コンテナーのビュー コント ローラーが存在しない場合は、モーダル ビュー コント ローラーとして、新しいビューが表示されます。
-  `ShowDetailViewController` – と同様の方法で動作します。 `ShowViewController`、新しいビュー コント ローラーに渡されると、詳細ビューを置換する分割ビュー コント ローラーに実装されます。 呼び出しにリダイレクトされます (iPhone アプリケーションで見られる) と分割ビュー コント ローラーが折りたたまれている場合、`ShowViewController`メソッド、および新しいビューは、プライマリのビュー コント ローラーとして表示されます。 ここでも、コンテナーのビュー コント ローラーが存在しない場合は、モーダル ビュー コント ローラーとして、新しいビューが表示されます。


これらのメソッドは、リーフのビュー コント ローラーを起動して動作し、新しいビューの表示を処理する適切なコンテナーのビュー コント ローラーが見つかるまで、階層を表示する方法について説明します。

開発者は実装できます`ShowViewController`と`ShowDetailViewController`専用のカスタム ビュー コント ローラーを取得する同じ機能の自動化を`UINavigationController`と`UISplitViewController`を提供します。

### <a name="how-it-works"></a>しくみ

このセクションではこれらのメソッドが iOS 8 で実際に実装される方法を参照してくださいになります。 最初、新しいを見てみましょう`GetTargetForAction`メソッド。

 [![](unified-storyboards-images/gettargetforaction.png "新しい GetTargetForAction メソッド")](unified-storyboards-images/gettargetforaction.png#lightbox)

このメソッドでは、適切なコンテナーのビュー コント ローラーが見つかるまで、階層チェーンについて説明します。 例:

1.  場合、`ShowViewController`メソッドが呼び出されると、このメソッドを実装するチェーンの最初のビュー コント ローラーにはナビゲーション コント ローラーがあるため、新しいビューの親として使用されます。
1.  場合、`ShowDetailViewController`メソッドが呼び出された代わりに、分割ビュー コント ローラーは、親として使用されているため、実装する最初のビュー コント ローラー。


`GetTargetForAction`メソッドは、特定のアクションを実装するビュー コント ローラーを検索して、そのアクションを受信する場合は、そのビュー コント ローラー。 このメソッドはパブリックであるために、開発者は、関数と同じように組み込み、独自のカスタム メソッドを作成できます`ShowViewController`と`ShowDetailViewController`メソッド。

## <a name="adaptive-presentation"></a>アダプティブ プレゼンテーション

Ios 8 で、Apple のポップ オーバー プレゼンテーションが行われて ( `UIPopoverPresentationController`) アダプティブもします。 ポップ オーバー プレゼンテーション ビュー コント ローラーは、通常のサイズ クラスで自動的に通常のポップ オーバー ビューが表示されますが、全画面表示で表示コンパクトな水平方向にサイズ クラス (など、iphone の場合)。

提示されたビュー コント ローラーを管理する統合されたストーリー ボード システム内で変更に合わせて、新しいコント ローラー オブジェクトが作成された —`UIPresentationController`します。 非表示にするまで、ビュー コント ローラーが表示される時刻からこのコント ローラーが作成されます。 管理クラスは、そのことができますと見なされるスーパー クラス ビュー コント ローラー経由でビュー コント ローラー (向き) など、プレゼンテーションのコント ローラーを制御するは、ビュー コント ローラーにフィードバックに影響するデバイスの変更に応答します。

ときに、開発者は、ビュー コント ローラーを使用して、`PresentViewController`プレゼンテーション プロセスの管理が渡されたメソッド、`UIKit`します。 UIKit 処理 (特に) が作成されているスタイルの適切なコント ローラー、ビュー コント ローラーに設定するスタイルをされている場合を除いて`UIModalPresentationCustom`します。 ここでは、アプリケーションが独自 PresentationController はなくを使用してを提供することができます、`UIKit`コント ローラー。

### <a name="custom-presentation-styles"></a>カスタム スタイル設定

開発者ではカスタムのプレゼンテーションの形式でカスタム プレゼンテーション コント ローラーを使用するオプションがあります。 同盟はそのビューの動作と外観を変更するのには、このカスタム コント ローラーを使用できます。

<a name="size-classes"/>

## <a name="working-with-size-classes"></a>サイズ クラスの使用

この記事に含まれているアダプティブ写真 Xamarin プロジェクトでは、iOS 8 Unified Interface アプリケーション サイズ クラスとアダプティブのビュー コント ローラーを使用する実際の例を示します。

アプリケーションに UI を完全に、コードから作成 IOS デザイナーを使用するのではなく、Unified ストーリー ボードを作成するには、同じ手法を適用します。 この記事の後半には、Unified ストーリー ボードおよび iOS の Xamarin アプリケーション デザイナーでサイズ クラスを使用する方法を紹介します。

今すぐアダプティブ写真プロジェクトがでどのように実装サイズ クラスの機能のいくつかの iOS 8 を適応型アプリケーションの作成について詳しく見てみましょう。

### <a name="adapting-to-trait-environment-changes"></a>特徴の環境の変化に合わせて調整

Iphone の写真の適応型アプリケーションを実行中、ユーザーが縦から横へのデバイスを回転したときに、マスターと詳細の両方のビューが分割ビュー コント ローラーに表示されます。

 [![](unified-storyboards-images/rotation.png "分割ビュー コント ローラーには、両方のマスターが表示され、詳細が以下のようにビュー")](unified-storyboards-images/rotation.png#lightbox)

これは、オーバーライドすることで実現、`UpdateConstraintsForTraitCollection`の値に基づいて、ビュー コント ローラーおよび制約を調整する方法、`VerticalSizeClass`します。 例えば:

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

### <a name="adding-transition-animations"></a>遷移のアニメーションを追加します。

オーバーライドすることで、既定のアニメーションに追加、分割ビュー コント ローラーからのアプリケーションがアダプティブの写真に折りたたむに展開されていると、アニメーション、`WillTransitionToTraitCollection`ビュー コント ローラーのメソッド。 例:

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

上記に示すようアダプティブ写真アプリケーションで iPhone デバイスが横長ビューである場合、両方の詳細を表示する分割ビュー コント ローラーおよびマスター ビューが強制的します。

これは、ビュー コント ローラーで次のコードを使用して行われました。

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

[次へ] 分割ビュー コント ローラーの展開および折りたたみの動作を Xamarin で実装された方法を見てみましょう。 `AppDelegate`分割ビュー コント ローラーが作成されたときに、これらの変更を処理するために、デリゲートが割り当てられます。

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

`SeparateSecondaryViewController`写真が表示されていると、その状態に基づいてアクションを実行するかどうかに表示するメソッドを調べます。 マスター ビュー コント ローラーが表示されるように表示されている写真がない場合にセカンダリのビュー コント ローラーを折りたたみます。

`CollapseSecondViewController`メソッドを使用分割ビュー コント ローラーを展開するときにそのビューに戻るため、折りたたまれている場合は、写真はすべてが、スタック上に存在かどうかを参照してください。

### <a name="moving-between-view-controllers"></a>ビュー コント ローラー間の移動

次に、ビュー コント ローラー間で、写真の適応型アプリケーションを移動する方法を見てみましょう。 `AAPLConversationViewController`クラスのユーザーが、テーブルのセルを選択、`ShowDetailViewController`詳細ビューを表示するメソッドが呼び出されます。

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

### <a name="displaying-disclosure-indicators"></a>漏えいのマークを表示します。

アダプティブ フォト アプリケーションでは、いくつかの場所の漏えいインジケーターが非表示または特徴環境への変更に基づいて表示できます。 これは、次のコードで処理されます。

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

これらを使用して実装、`GetTargetViewControllerForAction`メソッドの詳細は上記で説明します。

テーブル ビュー コント ローラーでは、データの表示が発生する、プッシュを移動するかどうかと、それに応じて漏えいインジケーターを表示または非表示にするかどうかを確認の上に実装されるメソッドを使用します。

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

Apple サイズ クラスと、分割ビュー コント ローラー内からの特徴である環境を操作するための新しい通知の種類が追加`ShowDetailTargetDidChangeNotification`します。 コント ローラーを展開または折りたたむ場合など、ターゲット分割ビュー コント ローラーの詳細の表示が変更されるたびに、この通知は送信を取得します。

アダプティブ写真アプリケーションでは、この通知を使用して、詳細ビュー コント ローラーの変更時に、漏えいインジケーターの状態を更新します。

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

アダプティブ写真アプリケーション サイズ クラスがすべての方法を詳しく見て、特徴のコレクションとアダプティブのビュー コント ローラーを使用して、Xamarin.iOS で簡単に統合されたアプリケーションを作成します。

## <a name="unified-storyboards"></a>統合ストーリーボード

新しい ios 8、Unified ストーリー ボードがサイズ クラスの複数ターゲットとする、iPhone と iPad の両方のデバイスで表示できるストーリー ボード ファイルをユニファイド開発者が、1 つを作成できるようにします。 Unified ストーリー ボードを使用すると、開発者は、小さい UI 固有のコードを記述しが 1 つだけのインターフェイスのデザインを作成および管理します。

Unified ストーリー ボードの主な利点は次のとおりです。

-  IPhone と iPad 用の同じストーリー ボード ファイルを使用します。
-  IOS 6 および iOS 7 を下位にデプロイします。
-  さまざまなデバイス、向き、および Xamarin iOS Designer 内からすべての OS バージョンのレイアウトをプレビューします。

この機能は完全にサポートでは、Visual Studio for Mac

<a name="enabling-size-classes" />

### <a name="enabling-size-classes"></a>サイズ クラスを有効にします。

新しい Xamarin.iOS プロジェクトでは、既定ではごサイズ クラス。 サイズ クラスとアダプティブ セグエ内、以前のプロジェクトからのストーリー ボードを使用するにはする必要がありますまず形式に変換、Xcode 6 Unified ストーリー ボードから、iOS デザイナー内。

IOS デザイナーとチェックに変換するストーリー ボードを開くこれを行う、**サイズ クラスを使用して** チェック ボックス。

 [![](unified-storyboards-images/sizeclass01.png "サイズ クラスを使用 チェック ボックス")](unified-storyboards-images/sizeclass01.png#lightbox)

IOS Designer は、開発者がサイズ クラスを使用してストーリー ボードの形式に変換することを確認します。

 [![](unified-storyboards-images/sizeclass02.png "サイズ クラスのアラートの使用")](unified-storyboards-images/sizeclass02.png#lightbox)

> [!IMPORTANT]
> 自動レイアウトは、正常に動作するサイズ クラスについても確認する必要があります。

### <a name="generic-device-types"></a>汎用デバイスの種類

サイズ クラスを使用して、ストーリー ボードを変換すると、それが再表示されます、デザイン画面で、**ビューとして**デバイスはジェネリックになります。

 [![](unified-storyboards-images/sizeclass03.png "汎用デバイスの種類として表示します。")](unified-storyboards-images/sizeclass03.png#lightbox)

汎用デバイスの種類を選択すると、すべてのビュー コント ローラーは 600 x 600 正方形にサイズ変更されます。 この四角形は、任意の幅と任意の高さのサイズを表します。 IOS デザイナーは、このモードでは、編集はサイズ クラスのすべてに適用されます。

開発者は、iPhone としてデザイン画面を表示するためのオプションもあります。

 [![](unified-storyboards-images/sizeclass04.png "IPhone としてデザイン画面を表示します。")](unified-storyboards-images/sizeclass04.png#lightbox)

または、iPad として表示する:

 [![](unified-storyboards-images/sizeclass05.png "IPad としてデザイン画面を表示します。")](unified-storyboards-images/sizeclass05.png#lightbox)

### <a name="select-a-size-class"></a>サイズ クラスを選択します。

サイズ クラス セレクター ボタンでは、ドロップダウン リストとして表示) の近くのデザイン画面の左上隅にあります。 サイズ クラスは現在編集中に選択する開発者が使用できます。

 [![](unified-storyboards-images/sizeclass06.png "サイズ クラスを選択します。")](unified-storyboards-images/sizeclass06.png#lightbox)

セレクターはサイズ クラスの選択を 3 x 3 グリッドとして表示します。 グリッドの正方形のそれぞれは、クラスを幅と高さのクラスの組み合わせを表します。 中央の正方形は、Any Width/Any 高さのサイズ クラス (つまり Unified ストーリー ボードの既定のビュー) を選択します。 この四角形を選択した場合、開発者が、その他のすべての構成によって継承される既定のレイアウトを編集します。

グリッドの左上隅の正方形は、Compact Compact 幅/高さのサイズ クラスを表します。

 [![](unified-storyboards-images/sizeclass07.png "Compact の幅/Compact 高さのサイズ クラス")](unified-storyboards-images/sizeclass07.png#lightbox)

このモードは横方向に iPhone に対応します。 グリッドの右下隅にある四角は、正規の幅/標準高さサイズのクラスを iPad を表すを表します。

 [![](unified-storyboards-images/sizeclass08.png "通常の幅/標準の高さのサイズ クラス")](unified-storyboards-images/sizeclass08.png#lightbox)

Iphone 縦向きの用紙のレイアウトを編集するには、左下隅で、正方形を選択します。 これは、Compact 通常幅/高さサイズ クラスを表します。

 [![](unified-storyboards-images/sizeclass09.png "Compact の幅/標準の高さのサイズ クラス")](unified-storyboards-images/sizeclass09.png#lightbox)

これを選択する四角形をクリックし、デザイン画面には、新しい選択範囲に一致するようにビュー コント ローラーのサイズが変わります。

 [![](unified-storyboards-images/sizeclass10.png "デザイン画面に示すように、新しい選択範囲を一致するようにビュー コント ローラーのサイズを変更します。")](unified-storyboards-images/sizeclass10.png#lightbox)

サイズ クラスおよび Iphone と Ipad のレイアウトへの影響についての詳細については、この記事のサイズ クラスを参照してください。

### <a name="adaptive-segue-types"></a>アダプティブ セグエの種類

かどうか、開発者が前に、ストーリー ボードを使用しの既存のセグエの種類に精通することが**プッシュ**、**モーダル**と**ポップ オーバー**します。 サイズ クラスは、Unified ストーリー ボード ファイルに有効な場合は、次アダプティブ セグエの種類 (上で説明した新しいビュー コント ローラー API に対応) が提供されます。**表示**と**詳細を表示する**します。

> [!IMPORTANT]
> サイズ クラスを有効にするをセグエ既存のすべての新しい型に変換します。

IOS の例では、マスター ビューで単純なゲームのナビゲーション メニューにある分割ビュー コント ローラーを Unified ストーリー ボードを使用している 8 アプリケーションを実行します。 ユーザーがメニュー ボタンをクリックすると、選択した項目のビュー コント ローラー表示するかどう分割ビュー コント ローラーの詳細セクションでは iPad で実行されている場合。 Iphone の場合、項目のビュー コント ローラーをナビゲーション スタックにプッシュする必要があります。

この効果を実現するために iOS Designer のコントロール ボタンをクリックし、線を表示するビュー コント ローラーにドラッグします。 マウス ボタンが解放されると、選択`Show Detail`セグエの種類のポップアップ メニューから。

 [![](unified-storyboards-images/segue01.png "セグエの種類のポップアップ メニューから詳細の表示 を選択します。")](unified-storyboards-images/segue01.png#lightbox)

新しいセグエは、ボタンとビュー コント ローラー間で作成されます。 IPhone シミュレーターでアプリケーションを実行し、メイン メニューが表示されます。

 [![](unified-storyboards-images/segue02.png "メイン メニュー")](unified-storyboards-images/segue02.png#lightbox)

をクリックして、**選択ゲーム**ボタンをクリックし、項目のビュー コント ローラーをナビゲーション スタックにプッシュされます。

 [![](unified-storyboards-images/segue03.png "示すようにその項目のビュー コント ローラーがナビゲーション スタックにプッシュされます。")](unified-storyboards-images/segue03.png#lightbox)

IPhone シミュレーターを停止し、iPad シミュレーターでアプリケーションを実行します。 横の向きをメイン スイッチ メニューが再び表示されます。

 [![](unified-storyboards-images/segue04.png "メイン メニューが表示されます。")](unified-storyboards-images/segue04.png#lightbox)

もう一度、をクリックして、**選択ゲーム**分割ビュー コント ローラーの [詳細] セクションで、ボタンと、項目のビュー コント ローラーが表示されます。

 [![](unified-storyboards-images/segue05.png "分割ビュー コント ローラーの詳細セクションに示すようにビュー コント ローラーの項目")](unified-storyboards-images/segue05.png#lightbox)

### <a name="excluding-an-element-from-a-size-class"></a>サイズ クラスからの要素の除外

指定された要素 (ビュー、コントロール、制約など) ができない特定のサイズ クラスの内部で必要な期間があります。 サイズ クラスから要素を除外するで除外する目的の項目を選択します。、**デザイン サーフェイス**します。 一番下までスクロール、**プロパティ エクスプ ローラー**  をクリックし、**歯車**ドロップダウン メニュー。 組み合わせを選択します**幅**と**高さ**から項目を除外します。

[![](unified-storyboards-images/exclude-a.png "幅と高さの組み合わせを選択します")](unified-storyboards-images/exclude-a.png#lightbox)

新しい*除外ケース*の下にある要素に追加されます、**プロパティ エクスプ ローラー**します。 次に、オフにして、**インストール済み**特定のサイズ クラスのチェック ボックス。

[![](unified-storyboards-images/exclude-b.png "インストールされているチェック ボックスをオフにします。")](unified-storyboards-images/exclude-b.png#lightbox)

幅と高さから除外された項目をデザイン画面を切り替えると、全体の UI デザインではない特定のサイズ クラスから削除されました。

 [![](unified-storyboards-images/exclude02.png "スイッチ幅と高さから除外された項目をデザイン画面")](unified-storyboards-images/exclude02.png#lightbox)

Any Width/Any 高さのサイズ クラスと、要素への切り替えは、引き続き有効では。

 [![](unified-storyboards-images/exclude03.png "Any Width/Any 高さのサイズ クラスへの切り替え")](unified-storyboards-images/exclude03.png#lightbox)

IPad シミュレーターでアプリケーションを実行すると、要素が表示されます。

 [![](unified-storyboards-images/exclude04.png "ときに表示される要素 iPad シミュレーターで実行中のアプリ")](unified-storyboards-images/exclude04.png#lightbox)

IPhone シミュレーターでは、アプリケーションが実行されるときに、要素がありません。

 [![](unified-storyboards-images/exclude05.png "要素が不足しているときに、iPhone シミュレーターで実行中のアプリ")](unified-storyboards-images/exclude05.png#lightbox)

要素から、除外のケースを削除する選択内の要素、**デザイン サーフェイス**の一番下までスクロール、**プロパティ エクスプ ローラー**  をクリックし、 **-** を削除する場合の横にあるボタンをクリックします。

Unified ストーリー ボードの実装を確認するを参照、`UnifiedStoryboard`サンプル Xamarin iOS 8 のアプリケーションがこのドキュメントにアタッチします。

## <a name="dynamic-launch-screens"></a>動的な起動画面

アプリが実際に開始をユーザーにフィードバックを提供する iOS アプリケーションを起動するときに、スプラッシュ スクリーンとして起動画面のファイルが表示されます。 IOS 8 の場合は、前に、開発者が 複数を含める必要があります`Default.png`のイメージ アセットをアプリケーションで実行されるデバイスの種類、方向、および画面解像度ごと。 たとえば、 `Default@2x.png`、 `Default-Landscape@2x~ipad.png`、`Default-Portrait@2x~ipad.png`など。

新しい iPhone 6 および iPhone 6 考慮とデバイス (および今後の Apple Watch)、すべての既存の iPhone と iPad デバイスで、これはさまざまなサイズ、向きと解像度の大きな配列を表します`Default.png`スタートアップ画面のイメージのアセットには、する必要があります作成および管理します。 さらに、これらのファイルは非常に大きくなることができは「膨張」成果物のアプリケーション バンドルは、iTunes App Store を (場合によって維持することが移動体通信ネットワーク経由で配信をできないように)、アプリケーションのダウンロードに必要な時間の量の増加エンドユーザーのデバイスで必要な記憶域の量の増加します。

新しい ios 8 の場合は、開発者が 1 つのアトミックを作成できます`.xib`自動レイアウトとサイズ クラスを使用して作成する Xcode でファイルを*動的な起動画面*は、すべてのデバイス、解像度、向きの動作をします。 これだけでなく、開発者を作成し、すべての必要なイメージ資産の管理の必要な作業の量が減少しますが、アプリケーションのインストールされているバンドルのサイズが大幅に削減します。


動的な起動画面は、次の制限事項と考慮事項があります。

 - のみを使用して`UIKit`クラス。
 - 単一のルート ビューを使用して、`UIView`または`UIViewController`オブジェクト。
 - アプリケーションのコードへの接続を作成しない (追加しないでください**アクション**または**Outlet**)。
 - 追加しない`UIWebView`オブジェクト。
 - 任意のカスタム クラスを使用しないでください。
 - ランタイムの属性を使用しないでください。

上記のガイドラインを考慮してでは、既存の Xamarin iOS 8 のプロジェクトに動的な起動画面の追加を見てみましょう。

次の手順で行います。

1. 開いている**Visual Studio for Mac**と負荷、**ソリューション**に動的な起動画面を追加します。
2. **ソリューション エクスプ ローラー**を右クリックし、`MainStoryboard.storyboard`ファイルおよび選択**プログラムから開く** > **Xcode の Interface Builder**:

    [![](unified-storyboards-images/dls01.png "Xcode インターフェイス ビルダーで開く")](unified-storyboards-images/dls01.png#lightbox)
3. Xcode で次のように選択します**ファイル** > **新規** > **ファイル...**:

    [![](unified-storyboards-images/dls02.png "ファイルを選択/新規")](unified-storyboards-images/dls02.png#lightbox)
4. 選択**iOS** > **ユーザー インターフェイス** > **起動画面** をクリックし、**次**ボタン。

    [![](unified-storyboards-images/dls03.png "IOS の選択/ユーザー インターフェイス/起動画面")](unified-storyboards-images/dls03.png#lightbox)
5. ファイルに名前を`LaunchScreen.xib` をクリックし、**作成**ボタン。

    [![](unified-storyboards-images/dls04.png "LaunchScreen.xib ファイルを名前します。")](unified-storyboards-images/dls04.png#lightbox)
6. グラフィック要素を追加し、レイアウトの制約を使用して特定のデバイス、向き、および画面サイズに配置するには、起動画面のデザインを編集します。

    [![](unified-storyboards-images/dls05.png "起動画面のデザインの編集")](unified-storyboards-images/dls05.png#lightbox)
7. 変更を保存`LaunchScreen.xib`します。
8. 選択、**アプリケーション ターゲット**と**全般** タブ。

    [![](unified-storyboards-images/dls06.png "ターゲットのアプリケーションと、[全般] タブを選択します。")](unified-storyboards-images/dls06.png#lightbox)
9. をクリックして、**選択 Info.plist**ボタンを選択、`Info.plist`の Xamarin アプリをクリック、**選択**ボタン。

    [![](unified-storyboards-images/dls07.png "Xamarin アプリの Info.plist を選択します。")](unified-storyboards-images/dls07.png#lightbox)
10. **アプリ アイコンと起動イメージ** セクションで、開いている、**スクリーン ファイルを起動**ドロップダウンを選択し、`LaunchScreen.xib`上記で作成しました。

    [![](unified-storyboards-images/dls08.png "選択、LaunchScreen.xib")](unified-storyboards-images/dls08.png#lightbox)
11. ファイルに変更を保存し、Visual studio for mac を返す
12. Visual Studio for Mac、Xcode で変更の同期を完了するまで待ちます。
13. **ソリューション エクスプ ローラー**を右クリックし、**リソース**フォルダーと選択**追加** > **ファイルを追加しています.**:

    [![](unified-storyboards-images/dls09.png "選択は、追加/ファイルを追加しています.")](unified-storyboards-images/dls09.png#lightbox)
14. 選択、`LaunchScreen.xib`上記で作成したファイルをクリックして、**オープン**ボタン。

    [![](unified-storyboards-images/dls10.png "LaunchScreen.xib ファイルを選択します。")](unified-storyboards-images/dls10.png#lightbox)
15. アプリケーションをビルドします。

### <a name="testing-the-dynamic-launch-screen"></a>動的な起動画面のテスト

Visual studio for Mac では、iPhone 4 の Retina シミュレーターを選択し、アプリケーションを実行します。 正しい形式と向きの動的な起動画面が表示されます。

[![](unified-storyboards-images/dls11.png "垂直方向に表示される動的な起動画面")](unified-storyboards-images/dls11.png#lightbox)

Visual studio for Mac のアプリケーションを停止し、iPad の iOS 8 デバイスを選択します。 アプリケーションを実行し、起動画面はこのデバイスの向きを正しくフォーマットされます。

[![](unified-storyboards-images/dls12.png "水平方向に表示される動的な起動画面")](unified-storyboards-images/dls12.png#lightbox)

Visual studio for Mac を返すし、アプリケーションの実行を停止します。

### <a name="working-with-ios-7"></a>IOS 7 の操作

IOS 7 との下位互換性を維持するために含めるだけです、通常`Default.png`のイメージ アセット iOS 8 アプリケーションで通常どおりです。 iOS は、以前の動作に戻るし、iOS 7 デバイスで実行されているときに、起動画面としてそれらのファイルを使用します。

Xamarin で動的起動画面の実装を確認するを参照、[動的起動画面](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/)サンプル iOS 8 アプリケーションがこのドキュメントにアタッチします。

## <a name="summary"></a>まとめ

この記事では、サイズのクラスと iPhone および iPad デバイスでのレイアウトへの影響を簡単に見てかかりました。 これは、統合インターフェイスを作成するサイズ クラスの特徴、特徴環境および特徴のコレクションを使用する方法について説明します。 アダプティブのビュー コント ローラーの概要と統一インターフェイスの内部サイズ クラスとそのしくみがかかりました。 サイズ クラスとから完全に統合されたインターフェイスの実装に見えましたC#Xamarin iOS 8 のアプリケーション内のコード。

最後に、この記事では、Xamarin iOS は iOS デバイスで機能するデザイナーの Unified ストーリー ボードの作成の基本をについて説明し、1 つの動的な起動画面を作成する、すべての iOS 8 デバイスでの起動画面として表示されます。

## <a name="related-links"></a>関連リンク

- [アダプティブ写真 (サンプル)](https://developer.xamarin.com/samples/monotouch/ios8/AdaptivePhotos/)
- [StoryboardIntro サンプル](https://developer.xamarin.com/samples/monotouch/StoryboardIntro/)
- [動的な起動画面 (サンプル)](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/)
- [iOS 8 の概要](~/ios/platform/introduction-to-ios8.md)
- [動的レイアウト iOS8 - Evolve 2014 (ビデオ)](http://youtu.be/f3mMGlS-lM4)
- [UIPresentationController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPresentationController_class/)
- [UIImageAsset](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset)
