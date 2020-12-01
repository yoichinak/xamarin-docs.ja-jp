---
title: Xamarin Designer for iOS を使用した自動レイアウト
description: このガイドでは、iOS の自動レイアウトについて説明し、Xamarin Designer for iOS を使用して、制約を使用してレイアウトを作成および編集する方法について説明します。 また、コード内の制約の変更、制約の変更のアニメーション化などについても説明します。
ms.prod: xamarin
ms.assetid: CAC7A715-55BB-45E2-BB6D-2168D36D428F
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 37d90bc42e843dd3b3c8f07689e0e229225ff57d
ms.sourcegitcommit: d1f0e0a9100548cfe0960ed2225b979cc1d7c28f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/01/2020
ms.locfileid: "96439461"
---
# <a name="auto-layout-with-the-xamarin-designer-for-ios"></a>Xamarin Designer for iOS を使用した自動レイアウト

> [!WARNING]
> IOS Designer は、Visual Studio 2019 バージョン16.8 と Visual Studio 2019 for Mac バージョン8.8 で段階的に廃止される予定です。
> IOS ユーザーインターフェイスを構築する場合は、Xcode を実行している Mac 上で直接作成することをお勧めします。 詳細については、「 [Xcode を使用したユーザーインターフェイスの設計](../storyboards/index.md)」を参照してください。 

自動レイアウト ("アダプティブレイアウト" とも呼ばれます) は、応答性の高いデザインアプローチです。 移動レイアウトシステムとは異なり、各要素の位置は画面上のポイントにハードコーディングされています。 "自動レイアウト" は、 *リレーションシップ* (デザインサーフェイス上の他の要素に対する要素の相対的な位置) に関するものです。 自動レイアウトの中核となるのは、画面上の他の要素のコンテキストにおける要素または要素のセットの配置を定義する制約または規則の概念です。 要素は画面上の特定の位置に関連付けられていないため、制約は、さまざまな画面サイズとデバイスの向きに適したアダプティブレイアウトを作成するのに役立ちます。

このガイドでは、Xamarin iOS Designer で制約とその操作を行う方法について説明します。 このガイドでは、制約の使用方法については説明しません。 プログラムによる自動レイアウトの使用の詳細については、 [Apple のドキュメント](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html)を参照してください。

## <a name="requirements"></a>必要条件

Xamarin Designer for iOS は、Visual Studio 2017 以降の Windows で Visual Studio for Mac で使用できます。

このガイドでは、「 [IOS designer の概要](~/ios/user-interface/designer/introduction.md) 」ガイドのデザイナーのコンポーネントに関する知識を前提としています。

## <a name="introduction-to-constraints"></a>制約の概要

制約とは、画面上の2つの要素間のリレーションシップを数学的に表現したものです。 UI 要素の位置を数学的関係として表すと、UI 要素の場所のハードコーディングに関連するいくつかの問題が解決されます。 たとえば、画面の下部にあるボタン20px を縦モードで配置する場合、ボタンの位置は横モードで画面から外れます。 これを回避するには、ボタンの下端をビューの下部から20px に設定する制約を設定します。 ボタンの端の位置は、ボタンとして計算さ *れます。 bottom = 20px* の場合は、ビューの一番下から縦モードと横モードの両方でボタン20px が配置されます。 数学的な関係に基づいて配置を計算する機能は、UI デザインで制約を使用すると便利です。

制約を設定するときに、制約 `NSLayoutConstraint` を適用するオブジェクトの引数として使用するオブジェクトと、制約が動作するプロパティ ( *属性*) を作成します。 IOS デザイナーでは、属性に要素の *左*、 *右*、 *上*、 *下* などのエッジが含まれます。 *高さ* や *幅* などのサイズ属性や、中心点の位置、 *system.windows.media.rotatetransform.centerx* 、およびセンター *y* も含まれます。 たとえば、2つのボタンの左側の境界の位置に制約を追加すると、デザイナーはその部分の下に次のコードを生成します。

```csharp
View.AddConstraint (NSLayoutConstraint.Create (Button1, NSLayoutAttribute.Left, NSLayoutRelation.Equal, Button2, NSLayoutAttribute.Left, 1, 10));
```

次のセクションでは、自動レイアウトの有効化と無効化、制約ツールバーの使用など、iOS Designer を使用した制約の操作について説明します。

## <a name="enable-auto-layout"></a>自動レイアウトを有効にする

既定の iOS デザイナー構成では、制約モードが有効になっています。 ただし、手動で有効または無効にする必要がある場合は、次の2つの手順で行うことができます。

1. デザイン画面の空の領域をクリックします。 これにより、すべての要素が選択解除され、ストーリーボードドキュメントのプロパティが表示されます。
1. プロパティパネルで、[ **オートレイアウトを使用する** ] チェックボックスをオンまたはオフにします。

    ![プロパティパネルの [オートレイアウトを使用する] チェックボックス](designer-auto-layout-images/image01.png)

既定では、画面上に制約が作成または表示されません。 代わりに、コンパイル時にフレーム情報から自動的に推論されます。 制約を追加するには、デザイン画面で要素を選択し、制約を追加する必要があります。 これは、[ **制約] ツールバー** を使用して行うことができます。

## <a name="constraints-toolbar"></a>制約ツールバー

 [![コンテキストメニューのコマンド](designer-auto-layout-images/toolbarnew.png)](designer-auto-layout-images/toolbarnew.png#lightbox)

[制約] ツールバーが更新され、次の2つの主要部分で構成されるようになりました。

- **制約モードボタンのトグル**: 以前は、デザイン画面で選択したビューでもう一度クリックして、制約モードを入力しました。 次に、このトグルボタンを制約バーで使用します。

  ![制約モードの切り替え](designer-auto-layout-images/constraints.png)

- **[制約の更新] ボタン:** 制約編集モードになっているかどうかによって変わることに注意する必要があります。
  - 制約編集モードでは、このボタンによって、要素フレームに合わせて制約が調整されます。
  - フレーム編集モードでは、このボタンは、制約が定義されている位置と一致するように要素フレームを調整します。

## <a name="constraints-editing-popover"></a>制約の編集 segue

[制約エディター] ポップアップを使用すると、選択ビューに対して一度に複数の制約を追加および更新できます。 2つのビューの左端にビューを配置するなど、複数の間隔、縦横比、および配置の制約を作成できます。

選択したビューの制約を編集するには、省略記号をクリックして segue: ![ constraints 編集 segue を表示します。](designer-auto-layout-images/constraints-popup.png)

制約を開くと、segue は、ビューに対する事前設定された制約を表示します。 右上隅にあるコンボボックスからすべての **辺** を選択し、[ **すべてクリア** ] を選択して削除します。

**W** は幅を設定し、 **H** は高さの制約を設定します。 **縦横比** を確認すると、ビューの高さと幅がさまざまな画面サイズで制御されます。ビューの幅は、その比率の分子として、および高さを分母として使用されます。

![制約の間隔](designer-auto-layout-images/constraints-spacing.png)

スペーシング制約の4つのコンボボックスには、制約を固定するための隣接するビューが一覧表示されます。

## <a name="surface-based-constraint-editing"></a>Surface-Based の制約の編集

より細かく調整された制約を編集するために、デザインサーフェイスで直接制約を操作できます。 このセクションでは、ピン間隔コントロール、領域のドロップ、さまざまな種類の制約の操作など、サーフェスベースの制約編集の基本について説明します。

### <a name="creating-constraints"></a>制約の作成

IOS デザイナーツールには、デザインサーフェイス上の要素を操作するためのコントロールが2種類用意されています。 次の図に示すように、コントロールと *ピン間隔コントロール* を *ドラッグ* します。

![コントロールの表示](designer-auto-layout-images/controls.png)

これらは、制約バーの [制約モード] ボタンを選択することによって切り替えられます。

要素の両側にある4つの T 形ハンドルによって、制約の要素の *上*、 *右*、 *下*、および *左端* が定義されます。 要素の右側と下部にある2つの I 形ハンドルは、それぞれ *高さ* と *幅* の制約を定義します。 中央の四角形は、 *system.windows.media.rotatetransform.centerx* 制約と *センター y* 制約の両方を処理します。

制約を作成するには、ハンドルを選択し、デザインサーフェイス上の任意の場所にドラッグします。 ドラッグを開始すると、一連の緑色の線/ボックスが画面に表示され、制約できるものが示されます。 たとえば、次のスクリーンショットでは、中央のボタンの上側を制限しています。

 [![中央のボタンの上側を制限する](designer-auto-layout-images/image07.png)](designer-auto-layout-images/image07.png#lightbox)

他の2つのボタンの間にある3つの点線の緑色の線に注意してください。 緑色の線は、 *ドロップ領域*、または制限できる他の要素の属性を示します。 上のスクリーンショットでは、その他の2つのボタンは、ボタンを制限するための垂直方向の3つのドロップエリア ( *下部*、 *中央の y*、 *上*) を提供しています。 ビューの上部にある緑色の点線は、ビューコントローラーがビューの一番上に制約を提供することを意味します。緑色の緑色の場合は、ビューコントローラーがトップレイアウトガイドの下に制約を提供することを意味します。

> [!IMPORTANT]
> レイアウトガイドは、さまざまな種類の制約ターゲットで、ステータスバーやツールバーなどのシステムバーの存在を考慮して、上下の制約を作成できます。 主な用途の1つは、iOS 6 と iOS 7 の間でアプリとの互換性を持たせることです。これは、最新バージョンでは、コンテナービューがステータスバーの下に拡張されるためです。 上部のレイアウトガイドの詳細については、 [Apple のドキュメント](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/transitionguide/AppearanceCustomization.html#//apple_ref/doc/uid/TP40013174-CH15-SW2)を参照してください。

次の3つのセクションでは、さまざまな種類の制約の使用について説明します。

### <a name="size-constraints"></a>サイズ制約

サイズの制約がある場合 ( *高さ* と *幅* )、2つのオプションがあります。 最初のオプションでは、上の例で示すように、ハンドルをドラッグして近隣の要素サイズを制限します。 もう1つのオプションは、ハンドルをダブルクリックして自己制約を作成することです。 これにより、次のスクリーンショットに示すように、定数サイズ値を指定できます。

 [![次に示すように、ハンドルをドラッグして近隣の要素サイズに制限します。](designer-auto-layout-images/sizec.png)](designer-auto-layout-images/sizec.png#lightbox)

### <a name="center-constraints"></a>中央の制約

Square ハンドルは、コンテキストに応じて、 *system.windows.media.rotatetransform.centerx* または *センター y* 制約を作成します。 四角形ハンドルをドラッグすると、次のスクリーンショットに示すように、垂直方向と水平方向の両方のドロップ領域を提供するために、他の要素が淡色表示されます。

 [![中央の制約](designer-auto-layout-images/centerc.png)](designer-auto-layout-images/centerc.png#lightbox)

垂直のドロップエリアを選択すると、 *中央の y* 制約が作成されます。 水平方向のドロップ領域を選択した場合、制約は *system.windows.media.rotatetransform.centerx* に基づいて作成されます。

### <a name="combinational-constraints"></a>主要制約の連結

2つの要素間のアラインメントとサイズの等価性の両方の制約を作成するには、次のスクリーンショットに示すように、上部のツールバーから項目を選択して、水平方向の配置、垂直方向の配置、およびサイズの等号を指定できます。

 [![主要制約の連結](designer-auto-layout-images/image06.png)](designer-auto-layout-images/image06.png#lightbox)

### <a name="visualizing-and-editing-constraints"></a>制約の視覚化と編集

制約を追加すると、項目を選択したときに、デザインサーフェイスに青い線として表示されます。

 [![視覚化 (制約を)](designer-auto-layout-images/image09.png)](designer-auto-layout-images/image09.png#lightbox)

制約を選択するには、青い線をクリックし、[プロパティ] パネルで直接制約値を編集します。 または、青い線をダブルクリックすると、デザインサーフェイス上で値を直接編集できる segue が表示されます。

 [![制約の編集](designer-auto-layout-images/image08.png)](designer-auto-layout-images/image08.png#lightbox)

## <a name="constraint-issues"></a>制約の問題

制約を使用すると、いくつかの種類の問題が発生する可能性があります。

- **制約が競合** しています。これは、複数の制約によって要素の属性の値が競合し、制約エンジンがその属性を調整できない場合に発生します。
- **Underconstrained items** —要素のプロパティ (location + size) は、制約のセットと有効な固有のサイズによって完全にカバーされている必要があります。 これらの値があいまいである場合、項目は underconstrained と呼ばれます。
- **Frame misplacement** -これは、要素のフレームとその制約のセットで2つの異なる結果の四角形が定義されている場合に発生します。

このセクションでは、上記の3つの問題について説明し、その処理方法の詳細を示します。詳述

### <a name="conflicting-constraints"></a>競合する制約

競合する制約は赤色でマークされ、警告記号が付きます。 警告シンボルをポイントすると、segue に競合に関する情報が表示されます。

 [![競合する制約の警告](designer-auto-layout-images/image11.png)](designer-auto-layout-images/image11.png#lightbox)

### <a name="underconstrained-items"></a>Underconstrained Items

Underconstrained items はオレンジ色で表示され、ビューコントローラーオブジェクトバーにオレンジ色のマーカーアイコンが表示されます。

 [![Underconstrained の項目はオレンジ色で表示されます](designer-auto-layout-images/image02.png)](designer-auto-layout-images/image02.png#lightbox)

そのマーカーアイコンをクリックすると、次のスクリーンショットに示すように、シーン内の underconstrained アイテムに関する情報を取得し、問題を解決することができます。

 [![Underconstrained 項目の修正](designer-auto-layout-images/image10.png)](designer-auto-layout-images/image10.png#lightbox)

### <a name="frame-misplacement"></a>フレーム Misplacement

Frame misplacement は、underconstrained items と同じカラーコードを使用します。 項目は常にネイティブフレームを使用してサーフェイス上に表示されますが、フレーム misplacement の場合は、次のスクリーンショットに示すように、アプリケーションの実行時に項目が終了する位置が赤い四角形でマークされます。

 [![サンプルフレーム Misplacement ビュー](designer-auto-layout-images/image05.png)](designer-auto-layout-images/image05.png#lightbox)

フレーム misplacement エラーを解決するには、[制約] ツールバーの [ **制約に基づいてフレームを更新** する] ボタン (右端のボタン) を選択します。

 [![[制約に基づいてフレームを更新する] ツールバーボタン](designer-auto-layout-images/image03.png)](designer-auto-layout-images/image03.png#lightbox)

これにより、コントロールによって定義された位置と一致するように、要素フレームが自動的に調整されます。

<a name="modifying-in-code"></a>

## <a name="modifying-constraints-in-code"></a>コード内の制約の変更

アプリの要件に基づいて、コードで制約を変更することが必要になる場合があります。 たとえば、制約が関連付けられているビューのサイズ変更や位置変更を行うには、制約の優先順位を変更するか、制約を完全に非アクティブ化します。

コード内の制約にアクセスするには、まず次の手順を実行して、その制約を iOS デザイナーで公開する必要があります。

1. (上記のいずれかの方法を使用して) 通常どおり制約を作成します。
2. **ドキュメントアウトラインエクスプローラー** で、目的の制約を見つけて選択します。

    [![ドキュメントアウトラインエクスプローラー](designer-auto-layout-images/modify01.png)](designer-auto-layout-images/modify01.png#lightbox)
3. 次に、**プロパティエクスプローラー** の [**ウィジェット**] タブで、制約に **名前** を割り当てます。

    [![[ウィジェット] タブ](designer-auto-layout-images/modify02.png)](designer-auto-layout-images/modify02.png#lightbox)
4. 変更を保存します。

上記の変更を適用したら、コードで制約にアクセスし、そのプロパティを変更できます。 たとえば、次のコードを使用して、添付ビューの高さをゼロに設定できます。

```csharp
ViewInfoHeight.Constant = 0;
```

IOS デザイナーでは、次のような制約が設定されています。

[![プロパティエクスプローラーでの制約の編集](designer-auto-layout-images/modify03.png)](designer-auto-layout-images/modify03.png#lightbox)

### <a name="the-deferred-layout-pass"></a>遅延レイアウトパス

自動レイアウトエンジンは、制約の変更に応じて添付ビューをすぐに更新するのではなく、近い将来の _遅延レイアウトパス_ をスケジュールします。 この遅延パスでは、特定のビューの制約が更新されただけでなく、階層内のすべてのビューに対する制約が再計算され、新しいレイアウトに合わせて更新されます。

任意の時点で、 `SetNeedsLayout` 親ビューのメソッドまたはメソッドを呼び出すことによって、独自の遅延レイアウトパスをスケジュールすることができ `SetNeedsUpdateConstraints` ます。

遅延レイアウトパスは、ビュー階層を介して2つの一意のパスで構成されます。

- このパスの **更新パス** では、自動レイアウトエンジンがビュー階層を走査し、すべてのビューコントローラーでメソッドを呼び出し、 `UpdateViewConstraints` すべての `UpdateConstraints` ビューに対してメソッドを呼び出します。
- **レイアウトが再度渡され** ます。自動レイアウトエンジンはビュー階層を走査しますが、今回はすべてのビューコントローラーでメソッドを呼び出し、 `ViewWillLayoutSubviews` すべての `LayoutSubviews` ビューに対してメソッドを呼び出します。 メソッドは、 `LayoutSubviews` `Frame` 自動レイアウトエンジンによって計算された四角形を使用して、各サブビューのプロパティを更新します。

### <a name="animating-constraint-changes"></a>制約変更のアニメーション化

制約プロパティを変更するだけでなく、コアアニメーションを使用して、ビューの制約に対する変更をアニメーション化することもできます。 次に例を示します。

```csharp
UIView.BeginAnimations("OpenInfo");
UIView.SetAnimationDuration(1.0f);
ViewInfoHeight.Constant = 237;
View.LayoutIfNeeded();

//Execute Animation
UIView.CommitAnimations();
```

ここで重要なのは、 `LayoutIfNeeded` アニメーションブロック内の親ビューのメソッドを呼び出すことです。 これにより、アニメーションの位置またはサイズの変更の各フレームを描画するようビューに指示します。 この行を使用しない場合、ビューはアニメーション化せずに最終バージョンにスナップするだけです。

## <a name="summary"></a>まとめ

このガイドでは、iOS の自動 (アダプティブ) レイアウトと、デザインサーフェイス上の要素間のリレーションシップの数学的表現としての制約の概念を紹介しました。 ここでは、iOS デザイナーで自動レイアウトを有効にする方法、 **制約ツールバー** を使用する方法、デザインサーフェイスで個別に制約を編集する方法について説明します。 次に、3つの一般的な制約の問題をトラブルシューティングする方法について説明しました。 最後に、コードで制約を変更する方法を示しました。

## <a name="related-links"></a>関連リンク

- [ストーリーボードの概要](~/ios/user-interface/storyboards/index.md)
- [iOS のデザイン可能コントロールのチュートリアル](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Android Designer の概要](~/android/user-interface/android-designer/index.md)
- [プログラムによる制約](~/ios/user-interface/programmatic-layout-constraints.md)
