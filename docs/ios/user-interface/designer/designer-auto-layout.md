---
title: IOS 用の Xamarin のデザイナーを使用した自動レイアウト
description: このガイドでは、iOS 自動レイアウトを紹介し、iOS 用の Xamarin のデザイナーを使用して、作成および編集する制約を使用してレイアウトする方法について説明します。 制約の変更、アニメーション化するコードの変更の制約についても説明します。
ms.prod: xamarin
ms.assetid: CAC7A715-55BB-45E2-BB6D-2168D36D428F
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 41a9ec90b4b734dde7a982ac3d4b2e7b2082321c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61413181"
---
# <a name="auto-layout-with-the-xamarin-designer-for-ios"></a>IOS 用の Xamarin のデザイナーを使用した自動レイアウト

自動レイアウトの (「アダプティブ レイアウト」とも呼ばれます) は、レスポンシブ デザイン手法です。 各要素の場所は、画面上のポイントにハード コードされた、過渡期のレイアウト システムとは異なり、自動レイアウトがの詳細については*リレーションシップ*-デザイン サーフェイス上の他の要素を基準として要素の位置。 自動レイアウトの中核が制約または画面上の他の要素のコンテキストで要素の配置または要素のセットを定義するルールの考え方です。 要素が画面上の特定位置に関連付けられていないため、制約はさまざまな画面サイズと向きがデバイス上で優れた外観アダプティブのレイアウトの作成に役立ちます。

このガイドでは、制約、および Xamarin iOS Designer に処理する方法を紹介します。 このガイドでは、プログラムで制約の使用は含まれません。 プログラムによる自動レイアウトの使用方法の詳細についてを参照してください、 [Apple ドキュメント](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html)します。

## <a name="requirements"></a>必要条件

IOS 用 Xamarin デザイナーは、Visual Studio for Mac で Visual Studio 2017 と、後で Windows で使用します。

このガイドでは、デザイナーのコンポーネントからのナレッジ、 [iOS Designer の概要](~/ios/user-interface/designer/introduction.md)ガイド。

## <a name="introduction-to-constraints"></a>制約の概要

制約は、画面上の 2 つの要素間の関係の数学的表現です。 UI を表す、数学的なリレーションシップとしての要素の位置は、ハード コーディング UI 要素の場所に関連付けられているいくつかの問題を解決します。 たとえば、画面の下部にあるボタン 20px を縦向きモードで配置する場合、ボタンの位置は横モードで画面をオフになります。 これを回避するには、ビューの下部にあるボタン 20px の下端に配置する制約を設定しますでした。 ボタンのエッジの位置として計算されますが*button.bottom = view.bottom - 20 px*縦長と横長の両方のモードで、ビューの下部にあるボタン 20px を配置するとします。 数学的な関係に基づく配置を計算する機能は、UI 設計に制約は非常に役立つ、何です。

制約を設定すると、作成、`NSLayoutConstraint`制約が適用されるオブジェクトとプロパティを引数として受け取るオブジェクトまたは*属性*制約の対象となるをします。 IOS デザイナーで属性には、エッジなど、*左*、*右*、*上部*、および*下部*要素の。 サイズ属性はなど、それらも*高さ*と*幅*、およびポイントの場所、センター *centerX*と*centerY*。 たとえば、2 つのボタンの左側の境界の位置に制約を追加する際、デザイナーは内部では、次のコード生成しています。

```csharp
View.AddConstraint (NSLayoutConstraint.Create (Button1, NSLayoutAttribute.Left, NSLayoutRelation.Equal, Button2, NSLayoutAttribute.Left, 1, 10));
```

次のセクションでは、iOS デザイナーを有効にすると、自動レイアウトを無効にして制約ツールバーを使用してなどを使用して制約の使用について説明します。

## <a name="enable-auto-layout"></a>自動レイアウトを有効にします。

IOS Designer の既定の構成が、制約モードを有効にします。 ただしに必要となる有効または無効に手動で、2 つの手順で行うことができます。

1.  デザイン画面で空の領域をクリックします。 これにより、任意の要素を解除し、ストーリー ボード ドキュメントのプロパティが表示されます。
1.  オンまたはオフに、**使用オート**プロパティ パネルでチェック ボックスをオンします。

    ![](designer-auto-layout-images/image01.png "プロパティ パネルで使用して [自動レイアウト] チェック ボックス")


既定では、制約がない作成された、または画面に表示します。 代わりに、コンパイル時にフレーム情報から、自動的に推論がされます。 制約を追加するには、デザイン画面で要素を選択し、制約を追加する必要があります。 使用するため、**制約ツールバー**します。

## <a name="constraints-toolbar"></a>制約ツールバー

 [![](designer-auto-layout-images/toolbarnew.png "コンテキスト メニュー コマンド")](designer-auto-layout-images/toolbarnew.png#lightbox)

制約ツールバーが更新され、2 つの主要な部分から成るようになりました。

- **制約モード ボタンの切り替え**:以前は、デザイン画面で選択したビューをもう一度クリックして制約モードを入力します。 制約バーにこのトグル ボタンを使用する必要がありますようになりました。

  ![制約モードの切り替え](designer-auto-layout-images/constraints.png)

- **「更新制約」のボタンをクリックします。** 制約の編集モードではするかにより、変更に注意して重要です。
  - 制約の編集モードでは、このボタンは、要素のフレームを一致するように制約を調整します。
  - フレームが編集モードでは、このボタンは、制約を定義する位置に一致する要素のフレームを調整します。


## <a name="surface-based-constraint-editing"></a>画面に基づく制約の編集

前のセクションでは、既定の制約を追加したり、制約ツールバーを使用して制約を削除について説明しました。 微調整の制約の詳細の編集、デザイン サーフェイスで直接制約と対話できます。 このセクションでは、画面ベースの制約の編集 (暗証番号 (pin) 間隔コントロール、ドロップ領域では、さまざまな種類の制約の使用など) の基礎について説明します。

### <a name="creating-constraints"></a>制約を作成します。

IOS のデザイナー ツールは、2 種類のデザイン サーフェイス上の要素を操作するためのコントロールを提供します。 *コントロールをドラッグ*と*pin 間隔コントロール*次の図のように、します。

![ビュー コントロール](designer-auto-layout-images/controls.png)

これらは、制約のバーで、制約モード ボタンを選択して切り替えること。

4 要素の両側 T 型ハンドルを定義、*上部*、*右*、*下部*、および*左*の要素の端を制約です。 要素の下、右にある 2 つは消すハンドル定義*高さ*と*幅*制約それぞれします。 中央の正方形では、両方を処理*centerX*と*centerY*制約。

制約を作成するには、ハンドルを選択し、デザイン サーフェイス上のどこかにドラッグします。 一連の緑の線/ボックスが何を伝える画面に表示されますが、ドラッグを開始するときに制限することができます。 たとえば、次のスクリーン ショットで中央ボタンの上にあるを制約します。

 [![](designer-auto-layout-images/image07.png "中央のボタンの上の制約")](designer-auto-layout-images/image07.png#lightbox)

その他の 2 つのボタンの間では、次の 3 つの破線緑の線に注意してください。 緑の線を示す*ドロップ エリア*、または制限できますが、他の要素の属性。 その他の 2 つのボタンが 3 つの垂直方向のドロップ領域を提供する上記のスクリーン ショット (*下部*、 *centerY*、*上部*) ボタンを制限します。 ビューの上部にある緑の破線は、ビュー コント ローラー、ビューの上部にある制約は、実線の緑色のボックスは、ビュー コント ローラー トップ レイアウト ガイドの下の制約を提供することを意味します。

> [!IMPORTANT]
> レイアウト ガイドは、特殊な種類の上部を作成することを許可する制約のターゲットと下部にある制約システム バー、ステータス バーやツールバーなどの存在を考慮します。 主な用途の 1 つは、最新のバージョンがあるコンテナーのビューのステータス バーの下に拡張するために iOS 6 および iOS 7 の間で互換性のあるアプリが必要です。 トップ レイアウト ガイドの詳細についてを参照してください、 [Apple ドキュメント](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/transitionguide/AppearanceCustomization.html#//apple_ref/doc/uid/TP40013174-CH15-SW2)します。



次の 3 つのセクションでは、さまざまな種類の制約の使用を紹介します。

### <a name="size-constraints"></a>サイズの制約

サイズの制約の*高さ*と*幅*-2 つのオプションがあります。 最初のオプションは、上記の例に示すように、近隣要素のサイズに制約を識別するハンドルをドラッグすることです。 その他のオプションでは、セルフ_サービスによる制約を作成するハンドルをダブルクリックします。 これにより、定数のサイズの値を指定する次のスクリーン ショットに示すようにします。

 [![](designer-auto-layout-images/sizec.png "次に示すように、近隣要素のサイズを制限するハンドルをドラッグします")](designer-auto-layout-images/sizec.png#lightbox)

### <a name="center-constraints"></a>Center の制約

四角形のハンドルが作成されます、 *centerX*または*centerY*コンテキストに応じて、制約。 次のスクリーン ショットに示すように、両方の縦と横のドロップダウン領域を提供するその他の要素のライトは四角形のハンドルをドラッグします。

 [![](designer-auto-layout-images/centerc.png "Center の制約")](designer-auto-layout-images/centerc.png#lightbox)

垂直方向のドロップ領域では、選択した場合、 *centerY*制約が作成されます。 水平方向のドロップ領域を選択した場合、制約がに基づいてする*centerX*します。

### <a name="combinational-constraints"></a>Combinational 制約

配置と 2 つの要素間の等値制約のサイズの両方を作成するには、次のスクリーン ショットに示すようから上部のツールバーの水平方向の配置、垂直方向の配置およびサイズの等号の順序で指定する項目を選択できます。

 [![](designer-auto-layout-images/image06.png "Combinational 制約")](designer-auto-layout-images/image06.png#lightbox)

### <a name="visualizing-and-editing-constraints"></a>視覚化および制約を編集します。

制約を追加するときが表示されます、デザイン サーフェイス青い線として項目を選択するとき。

 [![](designer-auto-layout-images/image09.png "制約を視覚化します。")](designer-auto-layout-images/image09.png#lightbox)

青い線をクリックし、[プロパティ] パネルで直接制約の値を編集する制約を選択します。 代わりに、青い線をダブルクリックすると表示されます、ポップ オーバー デザイン画面に直接値を編集することができます。

 [![](designer-auto-layout-images/image08.png "制約の編集")](designer-auto-layout-images/image08.png#lightbox)

## <a name="constraint-issues"></a>制約の問題

いくつかの種類の問題は、制約を使用する場合に発生します。

-  **競合する制約**— これ複数の制約は、属性の競合する値を持つ要素を強制し、制約エンジンがそれらを調整できる場合に発生します。
-  **アイテムは不完全拘束**— (場所とサイズ) の要素のプロパティを一連の制約と制約を有効にするための組み込みのサイズで完全対応する必要があります。 これらの値があいまいな場合は、拘束になり、項目と言います。
-  **フレームの存在**: この要素のフレームとその一連の制約が異なる 2 つの結果として得られる四角形を定義する場合に発生します。


このセクションは、上記 3 つの問題について詳しく説明し、それらを処理する方法の詳細を提供します。

### <a name="conflicting-constraints"></a>競合する制約

競合する制約は、赤色でマークされ、警告記号があります。 競合に関する情報をポップ オーバー ポインターを合わせると、警告シンボルが表示されます。

 [![](designer-auto-layout-images/image11.png "競合する制約の警告")](designer-auto-layout-images/image11.png#lightbox)

### <a name="underconstrained-items"></a>不完全拘束項目

不完全拘束項目は、オレンジ色で表示され、ビュー コント ローラーのオブジェクトのバーで、オレンジ色のマーカーのアイコンの外観をトリガーします。

 [![](designer-auto-layout-images/image02.png "オレンジ色で不完全拘束項目が表示されます。")](designer-auto-layout-images/image02.png#lightbox)

マーカー アイコンをクリックした場合は、シーンで不完全拘束項目に関する情報を取得し、次のスクリーン ショットに示すように制限することで、完全にまたはそれらの制約を削除することで、問題の解決。

 [![](designer-auto-layout-images/image10.png "不完全拘束項目を修正")](designer-auto-layout-images/image10.png#lightbox)

### <a name="frame-misplacement"></a>フレームの存在

フレームの存在は、不完全拘束項目として同じカラー コードを使用します。 項目は、ネイティブのフレームを使用して、画面に常に表示されますが、項目は最終的に、アプリケーションを実行すると次のスクリーン ショットに示すようにフレームの存在の場合、赤色の四角形としてマークされます。

 [![](designer-auto-layout-images/image05.png "サンプルのフレームの存在の表示")](designer-auto-layout-images/image05.png#lightbox)

フレーム存在エラーを解決するには、選択、**制約に基づいてフレームを更新**(一番右のボタン) 制約ツールバーからボタンをクリックします。

 [![](designer-auto-layout-images/image03.png "ツール バー ボタンの制約に基づくフレームを更新します。")](designer-auto-layout-images/image03.png#lightbox)

これにより、コントロールによって定義されている位置に一致する要素のフレームが自動的に調整します。

<a name="modifying-in-code" />

## <a name="modifying-constraints-in-code"></a>コード内の制約を変更します。

アプリの要件に基づき、ありますコードで制約を変更する必要がある場合。 たとえば、サイズを変更したり、ビューの位置を変更する制約にアタッチされます、制約の優先順位を変更または制約を完全に非アクティブ化します。

コードの制約にアクセスするには、まず次の手順に従って iOS Designer で公開する必要があります。

1. (上記の方法のいずれかを使用) が通常どおり、制約を作成します。
2. **ドキュメント アウトラインのエクスプ ローラー**目的の制約を見つけ、それを選択します。

    [![](designer-auto-layout-images/modify01.png "ドキュメント アウトラインのエクスプ ローラー")](designer-auto-layout-images/modify01.png#lightbox)
3. 次に、割り当て、**名前**の制約、**ウィジェット**のタブ、**プロパティ エクスプ ローラー**:

    [![](designer-auto-layout-images/modify02.png "[ウィジェット] タブ")](designer-auto-layout-images/modify02.png#lightbox)
4. 変更内容を保存します。

場所で上記の変更、コード内の制約にアクセスでき、そのプロパティを変更できます。 たとえば、0 に接続されているビューの高さを設定するのに、次を使用できます。

```csharp
ViewInfoHeight.Constant = 0;
```

IOS Designer の制約は次の設定を指定します。

[![](designer-auto-layout-images/modify03.png "プロパティ エクスプ ローラーでの制約の編集")](designer-auto-layout-images/modify03.png#lightbox)

### <a name="the-deferred-layout-pass"></a>遅延のレイアウト パス

制約の変更に応じて接続されているビューをすぐに更新するには、代わりに、自動レイアウト エンジンをスケジュール、_延期レイアウト パス_近い将来にします。 この遅延のパスではだけでなくは、指定されたビューの制約が更新されると、制約、階層内のすべてのビューが再計算され、新しいレイアウトを調整するように更新します。

任意の時点では、呼び出すことによって遅延レイアウト パスをスケジュールすることができます、`SetNeedsLayout`または`SetNeedsUpdateConstraints`親ビューのメソッド。 

段階的提供のレイアウト パスは、ビュー階層を 2 つの一意のパスで構成されます。

- **更新パス**-このパスで、自動レイアウト エンジン ビューの階層構造を走査し、呼び出します、`UpdateViewConstraints`すべてのビュー コント ローラー メソッドと`UpdateConstraints`メソッドすべてのビューをします。
- **レイアウト パス**- もう一度、自動レイアウト エンジンがビューの階層構造を走査が、この時点で呼び出す、`ViewWillLayoutSubviews`すべてのビュー コント ローラー メソッドと`LayoutSubviews`メソッドすべてのビューをします。 `LayoutSubviews`メソッドを更新、`Frame`自動レイアウト エンジンによって各サブビュー四角形のプロパティが計算されます。

### <a name="animating-constraint-changes"></a>制約の変更をアニメーション化

制約のプロパティを変更するだけでなく、ビューの制約に加えた変更をアニメーション化するのにコア アニメーションを使用できます。 例:

```csharp
UIView.BeginAnimations("OpenInfo");
UIView.SetAnimationDuration(1.0f);
ViewInfoHeight.Constant = 237;
View.LayoutIfNeeded();

//Execute Animation
UIView.CommitAnimations();
```

ここで重要を呼び出し、`LayoutIfNeeded`アニメーション ブロック内の親ビューのメソッド。 これは、アニメーション化された場所またはサイズ変更の各"frame"を描画するためにビューを示します。 、この行がない、ビューは単にスナップ、最終バージョンにアニメーション化することがなく。

## <a name="summary"></a>まとめ

このガイドは、iOS 自動 (または"adaptive") で導入されました。 レイアウトとデザイン サーフェイス上の要素間のリレーションシップの数学的表現としての制約の概念です。 IOS デザイナーの操作で自動レイアウトを有効にする方法を説明し、**制約ツールバー**、およびデザイン サーフェイスに個別に制約を編集します。 次に、制約の 3 つの一般的な問題をトラブルシューティングする方法を説明します。 最後に、コード内の制約を変更する方法を示しました。

## <a name="related-links"></a>関連リンク

- [ストーリーボードの概要](~/ios/user-interface/storyboards/index.md)
- [iOS デザイン可能なコントロールのチュートリアル](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Android デザイナーの概要](~/android/user-interface/android-designer/index.md)
- [プログラムによる制約](~/ios/user-interface/programmatic-layout-constraints.md)
- [Apple - 自動レイアウト ガイド](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/AutolayoutPG/Introduction/Introduction.html#/apple_ref/doc/uid/TP40010853-CH13-SW1)
