---
title: IOS 用の Xamarin デザイナーに自動レイアウト
description: このガイドでは、自動レイアウトを iOS および iOS 用の Xamarin デザイナーで使用できる新しい制約のワークフローが導入されています。
ms.prod: xamarin
ms.assetid: CAC7A715-55BB-45E2-BB6D-2168D36D428F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 2023483f817c365d2cfad6945b281d630317693b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="auto-layout-with-the-xamarin-designer-for-ios"></a>IOS 用の Xamarin デザイナーに自動レイアウト

_このガイドでは、自動レイアウトを iOS および iOS 用の Xamarin デザイナーで使用できる新しい制約のワークフローが導入されています。_

自動レイアウト (「アダプティブ レイアウト」とも呼ばれます) は、レスポンシブ デザイン方法です。 各要素の場所は、画面上の点にハードコード、過渡期のレイアウト システムとは異なり、自動レイアウトがに関するは*リレーションシップ*-デザイン サーフェイス上の他の要素に対して要素の位置。 自動レイアウトの中核には、制約または画面の他の要素のコンテキストで要素の配置または要素のセットを定義するルールの設定を勧めします。 要素が画面上の特定位置に関連付けられていないために、制約はさまざまな画面サイズと向きがデバイスには良く見えるアダプティブ レイアウトの作成に役立ちます。

このガイドでは、制約、および Xamarin iOS デザイナー内で操作する方法を紹介します。 このガイドでは、制約の使用をプログラムでは説明しません。 プログラムによる自動レイアウトの使用方法の詳細についてを参照してください、 [Apple のドキュメント](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html)です。

## <a name="requirements"></a>要件

IOS 用 Xamarin デザイナーは、Windows 上の Mac に Visual Studio 2015 および 2017 での Visual Studio で使用します。

この前提として、デザイナーのコンポーネントからのナレッジ、 [iOS デザイナーの概要](~/ios/user-interface/designer/introduction.md)ガイドです。

## <a name="introduction-to-constraints"></a>制約の概要

制約は、画面上の 2 つの要素間の関係の数学的表現です。 UI を表す、数学的なリレーションシップとしての要素の位置は、ハード コーディングする UI 要素の場所に関連付けられているいくつかの問題を解決します。 たとえば、縦向きモードで、画面の下部からボタン 20 px に配置する場合、ボタンの位置は横モードで画面をオフします。 これを回避するには、ビューの下部からボタン 20 px の下端を配置するための制約を設定おでした。 としてのボタンのエッジ位置を計算し、 *button.bottom = view.bottom - 20 px*ビューの下部からボタン 20 px を縦長と横長の両方のモードで配置します。 数学的な関係に基づく配置を計算する機能は、UI の設計で制約を非常に便利な新機能です。

作成する制約を設定したときに、`NSLayoutConstraint`を制約が適用されるオブジェクトおよびプロパティ、引数として受け取るオブジェクトまたは*属性*で動作する制約です。 IOS デザイナーで、属性などのエッジ、*左*、*右*、*上部*、および*下部*要素のです。 含まれてサイズ属性など*高さ*と*幅*、およびポイントの場所、センター *centerX*と*centerY*です。 たとえば、2 つのボタンの左側の境界の位置に制約を追加する際、デザイナーは内部で次のコード生成しています。

```csharp
View.AddConstraint (NSLayoutConstraint.Create (Button1, NSLayoutAttribute.Left, NSLayoutRelation.Equal, Button2, NSLayoutAttribute.Left, 1, 10));
```

次のセクションでは、デザイナーを有効にすると、自動レイアウトを無効にして制約ツールバーを使用するなど、iOS を使用して、制約の扱いについて説明します。

## <a name="enable-auto-layout"></a>自動レイアウトを有効にします。

既定 iOS デザイナー構成では制約モードを有効にします。 ただし、必要を有効にするか、手動で無効にする 2 つの手順で行うことができます。

1.  デザイン サーフェイス上の空き領域をクリックします。 これにより、任意の要素を解除し、ストーリー ボードのドキュメントのプロパティを表示します。
1.  オンまたはオフに、**使用 Autolayout**プロパティ パネルのチェック ボックス。

    ![](designer-auto-layout-images/image01.png "プロパティ パネルで使用する自動レイアウト チェック ボックス")


既定では、制約がないサーフェイスに表示または作成します。 代わりに、コンパイル時にフレーム情報から、自動的に推論がされます。 制約を追加するには、デザイン サーフェイスに要素を選択し、制約を追加する必要があります。 使用して行うことができます、**制約ツールバー**です。

## <a name="constraints-toolbar"></a>制約のツールバー

 [![](designer-auto-layout-images/toolbarnew.png "コンテキスト メニュー コマンド")](designer-auto-layout-images/toolbarnew.png#lightbox)

制約ツールバーが更新されており、ここでは 2 つの主要な部分で構成されます。

- **制約モード ボタン トグル**: 以前は、デザイン画面で選択したビューをもう一度クリックして制約モードを入力します。 これで、制約バーでこのトグル ボタンを使用する必要があります。

  ![制約のモードを切り替える](designer-auto-layout-images/constraints.png)

- **「更新制約」ボタン:**が場合に応じて変更している編集モードの制約に注意してください。
  - 制約の編集モードでは、このボタンは、要素のフレームを一致するように制約を調整します。
  - フレームが編集モードでは、このボタンは、制約を定義する位置に一致する要素のフレームを調整します。


## <a name="surface-based-constraint-editing"></a>画面に基づく制約の編集

前のセクションでは、既定の制約を追加および制約ツールバーを使用して制約を削除するについて説明しました。 微調整制約の詳細の編集、デザイン画面に直接制約で操作することができます。 このセクションでは、画面ベースの制約を編集、暗証番号 (pin) 間隔コントロール、ドロップ領域の制約のさまざまな種類の操作などの基本について説明します。

### <a name="creating-constraints"></a>制約を作成します。

IOS デザイナー ツールには、デザイン サーフェイス上の要素を操作するためのコントロールの 2 つの種類が用意されています。 *コントロールをドラッグ*と*暗証番号 (pin) 間隔コントロール*次の図に示すように。

![ビュー コントロール](designer-auto-layout-images/controls.png)

これらの切り替えを制約バーに制約モード ボタンを選択します。

要素の各辺に 4 T の形のハンドルを定義、*上部*、*右*、*下部*、および*左*の要素のエッジ、制約です。 右と要素の下にある 2 つの I の形をしたハンドルを定義する*高さ*と*幅*制約それぞれします。 中間四角いハンドルは、両方とも*centerX*と*centerY*制約。

制約を作成するには、ハンドルを選択し、デザイン画面上の任意の場所にドラッグします。 新機能を示す画面で、一連の緑の線/ボックスが表示されます、ドラッグを開始するときに制約することができます。 たとえば、次のスクリーン ショットの中央ボタンの上にあるを制約お。

 [![](designer-auto-layout-images/image07.png "中央のボタンの上側の制約")](designer-auto-layout-images/image07.png#lightbox)

他の 2 つのボタンの間で、次の 3 つ緑破線に注意してください。 緑の線を示す*ドロップ エリア*、またはおを制限するその他の要素の属性です。 他の 2 つのボタンが 3 の垂直方向のドロップ領域を提供する上記のスクリーン ショット (*下部*、 *centerY*、*上部*) ボタンを制限します。 ビューの上部にある緑の点線は、ビュー コント ローラーには、ビューの上部にある、制約と純色の緑色のボックスでは、ビュー コント ローラーには、最上位レイアウト ガイドの下の制約は意味を意味します。

> [!IMPORTANT]
> レイアウト ガイドは、特別な種類の上部を作成することを許可する制約のターゲットとステータス バーまたはツールバーなどのシステムのバーの有無を考慮する下部制約です。 最新バージョンには、コンテナーのビュー、ステータス バーの下を拡張するため、アプリを iOS 6、iOS 7 の間で互換性がある主な用途の 1 つです。 最上位レイアウト ガイドの詳細についてを参照してください、 [Apple のドキュメント](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/transitionguide/AppearanceCustomization.html#//apple_ref/doc/uid/TP40013174-CH15-SW2)です。



次の 3 つのセクションでは、さまざまな種類の制約の動作を紹介します。

### <a name="size-constraints"></a>サイズの制限

サイズ制約 -*高さ*と*幅*-2 つのオプションがあります。 最初のオプションでは、上記の例で示すように、近隣ノードと要素のサイズを制限するハンドルをドラッグします。 その他のオプションでは、自己制約を作成するハンドルをダブルクリックします。 これにより、定数のサイズの値を指定する次のスクリーン ショットに示すようにします。

 [![](designer-auto-layout-images/sizec.png "次に示すように、近隣ノードと要素のサイズを制限するハンドルをドラッグします。")](designer-auto-layout-images/sizec.png#lightbox)

### <a name="center-constraints"></a>Center の制約

四角形のハンドルが作成されます、 *centerX*または*centerY*コンテキストに応じて、制約。 四角形のハンドルをドラッグすることが点灯で次のスクリーン ショットに示すように両方の垂直および水平方向のドロップ領域を提供するその他の要素。

 [![](designer-auto-layout-images/centerc.png "Center の制約")](designer-auto-layout-images/centerc.png#lightbox)

垂直方向のドロップ領域を選択した場合、 *centerY*制約が作成されます。 水平方向のドロップ領域を選択した場合、制約は、に基づいて*centerX*です。

### <a name="combinational-constraints"></a>Combinational 制約

アラインメントと 2 つの要素間の等値制約のサイズの両方を作成するには、次のスクリーン ショットに示すように、上部のツールバーの水平方向の配置、垂直方向の配置およびサイズの等号の順序で指定するから項目を選択できます。

 [![](designer-auto-layout-images/image06.png "Combinational 制約")](designer-auto-layout-images/image06.png#lightbox)

### <a name="visualizing-and-editing-constraints"></a>視覚表示や制約を編集

制約を追加するときにそれが表示されますデザイン サーフェイスに青い線として項目を選択するときに。

 [![](designer-auto-layout-images/image09.png "制約を視覚化します。")](designer-auto-layout-images/image09.png#lightbox)

青い線をクリックし、[プロパティ] パネルで直接制約条件の値を編集する制約を選択します。 代わりに、青い線をダブルクリックするとは、デザイン画面に直接値を編集できる重なってが表示されます。

 [![](designer-auto-layout-images/image08.png "制約の編集")](designer-auto-layout-images/image08.png#lightbox)

## <a name="constraint-issues"></a>制約の問題

問題のいくつかの種類は、制約を使用する場合に発生します。

-  **競合する制約**— 複数の制約を強制的にその要素に属性の値と競合しておよび制約エンジンが、それらを調整することがないときに発生します。
-  **アイテムは不完全拘束**— (場所とサイズ) の要素のプロパティは、一連の制約と、制約を有効にするための組み込みのサイズによって完全カバーする必要があります。 これらの値があいまいな場合は、アイテムは不完全拘束と呼ばれます。
-  **フレームの存在**: この要素のフレームとその制約のセットが異なる 2 つの結果として得られる四角形を定義する場合に発生します。


このセクションでは、上記 3 つの問題について詳しく説明し、それらを処理する方法について詳しく説明します。

### <a name="conflicting-constraints"></a>競合する制約

競合する制約は赤でマークされに警告シンボルがあります。 競合に関する情報を含む重なって警告記号の上にマウスが表示されます。

 [![](designer-auto-layout-images/image11.png "競合する制約の警告")](designer-auto-layout-images/image11.png#lightbox)

### <a name="underconstrained-items"></a>不完全拘束項目

不完全拘束項目は、オレンジ色で表示され、ビュー コント ローラー オブジェクトのバーに、オレンジ色のマーカーのアイコンの外観をトリガーします。

 [![](designer-auto-layout-images/image02.png "不完全拘束項目がオレンジ色を表示します。")](designer-auto-layout-images/image02.png#lightbox)

マーカー アイコンをクリックした場合は、シーンの不完全拘束項目に関する情報を取得および次のスクリーン ショットに示すように、またはそれらの制約を削除することでいずれか完全に制限することでそれらの問題を解決できます。

 [![](designer-auto-layout-images/image10.png "不完全拘束項目を修正します。")](designer-auto-layout-images/image10.png#lightbox)

### <a name="frame-misplacement"></a>フレームの存在

フレームの存在は、不完全拘束項目として、同じ色コードを使用します。 ネイティブ フレームを使用して、画面には、アイテムが常に表示されますが、フレームの存在の場合、赤色の四角形としてマークされますがどこアイテムで終了アプリケーションの実行時に次のスクリーン ショットに示すように。

 [![](designer-auto-layout-images/image05.png "サンプルのフレームの存在の表示")](designer-auto-layout-images/image05.png#lightbox)

フレーム存在エラーを解決するには、選択、**制約に基づいてフレームを更新**(一番右のボタン) は、制約ツールバーからボタンをクリックします。

 [![](designer-auto-layout-images/image03.png "ツール バー ボタンの制約に基づくフレームを更新します。")](designer-auto-layout-images/image03.png#lightbox)

これにより、コントロールで定義されている位置に一致する要素のフレームは自動的に調整します。

<a name="modifying-in-code" />

## <a name="modifying-constraints-in-code"></a>コード内の制約を変更します。

アプリの要件に基づき、ありますコードで制約を変更する必要がある場合。 たとえば、サイズまたはビューの位置を制約を適用、制約の優先順位を変更または制約を完全に非アクティブにします。

コードの制約にアクセスするには、まず iOS デザイナーで、次の手順で公開する必要があります。

1. (上記のいずれかを使用) が通常どおり、制約を作成します。
2. **ドキュメント アウトラインのエクスプ ローラー**目的の制約を見つけて、それを選択します。

    [![](designer-auto-layout-images/modify01.png "ドキュメント アウトラインのエクスプ ローラー")](designer-auto-layout-images/modify01.png#lightbox)
3. 次に、割り当て、**名前**で、この制約に、**ウィジェット**のタブ、**プロパティ エクスプ ローラー**:

    [![](designer-auto-layout-images/modify02.png "ウィジェット タブ")](designer-auto-layout-images/modify02.png#lightbox)
4. 変更内容を保存します。

場所で上記の変更、コード内の制約にアクセスでき、そのプロパティを変更できます。 たとえば、次を使用して、0 に接続されているビューの高さを設定することができます。

```csharp
ViewInfoHeight.Constant = 0;
```

IOS デザイナーで制約の次の設定を指定します。

[![](designer-auto-layout-images/modify03.png "制約プロパティ エクスプ ローラーでの編集")](designer-auto-layout-images/modify03.png#lightbox)

### <a name="the-deferred-layout-pass"></a>遅延のレイアウト パス

制約の変更に応じて接続されているビューを即座に更新するには、代わりに、自動レイアウト エンジンをスケジュールする_延期レイアウト パス_近い将来にします。 この遅延パス中にだけでなくは指定されたビューの制約が更新されると、制約、階層内のすべてのビューが再計算され、更新、新しいレイアウトを調整するのです。

任意の時点では、呼び出すことによって、独自の延期レイアウト パスをスケジュールすることができます、`SetNeedsLayout`または`SetNeedsUpdateConstraints`親ビューのメソッドです。 

延期レイアウト パスは、次の 2 つの一意な通過ビュー階層で構成されます。

- **更新パス**-このパスで、自動レイアウト エンジンがビューの階層構造を走査しを呼び出す、`UpdateViewConstraints`ビューのすべてのコント ローラーのメソッドと`UpdateConstraints`のすべてのビューのメソッドです。
- **レイアウト パス**- 自動レイアウト エンジンは、階層の表示を通過する時間が、この時間を呼び出す、`ViewWillLayoutSubviews`ビューのすべてのコント ローラーのメソッドと`LayoutSubviews`のすべてのビューのメソッド。 `LayoutSubviews`メソッドを更新、`Frame`各サブビュー四角形のプロパティは、自動レイアウト エンジンで計算します。

### <a name="animating-constraint-changes"></a>制約の変更をアニメーション化

制約のプロパティを変更するには、だけでなく、ビューの制約に加えた変更をアニメーション化するのにコア アニメーションを使用できます。 例えば:

```csharp
UIView.BeginAnimations("OpenInfo");
UIView.SetAnimationDuration(1.0f);
ViewInfoHeight.Constant = 237;
View.LayoutIfNeeded();

//Execute Animation
UIView.CommitAnimations();
```

ここでキーを呼び出して、`LayoutIfNeeded`アニメーション ブロック内で親ビューのメソッドです。 これは、アニメーション化された場所またはサイズ変更の各"frame"を描画するビューを示しています。 この行がない、ビューは単にスナップ最終バージョンをアニメーション化することがなくです。

## <a name="summary"></a>まとめ

このガイドに iOS 自動 (または"adaptive") が導入されたレイアウトと制約をデザイン サーフェイス上の要素間の関係の数学的表現としての概念です。 デザイナーで、iOS の使用に自動レイアウトを有効にする方法が説明されている、**制約ツールバー**、およびデザイン サーフェイスに個別に制約を編集します。 次に、制約の 3 つの一般的な問題をトラブルシューティングする方法を説明します。 最後に、コード内の制約を変更する方法を示しました。

## <a name="related-links"></a>関連リンク

- [ストーリーボードの概要](~/ios/user-interface/storyboards/index.md)
- [iOS 設計可能なコントロールのチュートリアル](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Android デザイナーの概要](~/android/user-interface/android-designer/index.md)
- [プログラムによる制約](~/ios/user-interface/programmatic-layout-constraints.md)
- [Apple の自動レイアウト ガイド](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/AutolayoutPG/Introduction/Introduction.html#/apple_ref/doc/uid/TP40010853-CH13-SW1)
