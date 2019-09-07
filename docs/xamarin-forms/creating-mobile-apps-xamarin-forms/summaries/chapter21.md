---
title: 第 21 章の概要です。 変換
description: Xamarin を使用した Mobile Apps の作成:第 21 章の概要です。 変換
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3642F112-C7FA-4A74-9000-F9087BA89AD9
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 40c091d0c5042d172108709f89774e41e9339d4b
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760577"
---
# <a name="summary-of-chapter-21-transforms"></a>第 21 章の概要です。 変換

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21)

Xamarin.Forms のビューは、場所とこれは一般に、その親によって決定されるサイズで画面に表示、`Layout`または`Layout<View>`から派生します。 *変換*Xamarin.Forms 機能であり、その場所、サイズ、または偶数の向きを変更できます。

Xamarin.Forms には、次の 3 つの基本的な種類の変換がサポートされています。

- *翻訳*&mdash;要素を水平方向または垂直方向にシフト
- *スケール*&mdash;要素のサイズを変更します。
- *回転*&mdash;ポイントまたは軸の周りの要素を有効にします。

Xamarin.Forms では、スケーリング アイソトロ ピック; は幅と高さを均等に影響がします。 両方の画面とは、2 次元表面に 3 次元空間で、回転はサポートされています。 傾斜 (または膨大な) 変換となしの汎用化された行列変換がありません。

型の 8 つのプロパティでは、変換がサポートされて`double`によって定義された、`VisualElement`クラス。

- [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX)
- [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY)
- [`Scale`](xref:Xamarin.Forms.VisualElement.Scale)
- [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)
- [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX)
- [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY)
- [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX)
- [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY)

これらすべてのプロパティは、バインド可能なプロパティでサポートされます。 データ バインディングのターゲットにする、スタイルを設定します。 [**第 22 章です。アニメーション**](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter22.md)どのようにこれらのプロパティをアニメーション化できるを示しますが、この章では一部のサンプルの表示、Xamarin.Forms を使用してアニメーション化[タイマー](~/xamarin-forms/platform/device.md#devicestarttimer)します。

プロパティは、要素が表示され、は方法だけで変換*いない*レイアウトで要素を認識する方法に影響します。

## <a name="the-translation-transform"></a>移動変換

値が 0 以外の場合、 [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX)と[ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY)プロパティは、水平方向または垂直方向に要素をシフトします。

[ **TranslationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TranslationDemo)プログラムでは、これらの 2 つのプロパティを実験できます`Slider`を制御する要素、`TranslationX`と`TranslationY`プロパティ、の`Frame`. 変換にも影響するすべての子`Frame`します。

### <a name="text-effects"></a>テキスト効果

変換のプロパティの 1 つの一般的な用途は、少しテキストのレンダリングをオフセットします。 これは、方法については、 [ **TextOffsets** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TextOffsets)サンプル。

[![テキストのオフセットの 3 倍になるスクリーン ショット](images/ch21fg03-small.png "テキスト オフセット")](images/ch21fg03-large.png#lightbox "テキスト オフセット")

別の効果はの複数のコピーを表示するためには、 `Label` 3D ブロックと同じように示すように、 [ **BlockText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BlockText)サンプル。

### <a name="jumps-and-animations"></a>ジャンプとアニメーション

[ **ButtonJump** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonJump)サンプルでは、変換を使用して、移動、`Button`タップすると、主な目的は、デモンストレーションをするときに、`Button`場所にユーザー入力を受け取る場所ボタンが表示されます。

[ **ButtonGlide** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonGlide)サンプルは似ていますが、タイマーを使用してアニメーション化する、`Button`を別の 1 つのポイントから。

## <a name="the-scale-transform"></a>スケール変換

[ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale)変換は増やすことも、レンダリングされる要素のサイズを減らします。 既定値は 1 です。 値 0 は、表示する要素です。 負の値では、180 度回転表示する要素が発生します。 `Scale`プロパティには影響しません、`Width`または`Height`要素のプロパティ。 これらの値は変わりません。

試すことができます、`Scale`プロパティを使用して、 [ **SimpleScaleDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/SimpleScaleDemo)サンプル。

[ **ButtonScaler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonScaler)サンプルではアニメーション化の違い、`Scale`のプロパティを`Button`アニメーション化して、`FontSize`プロパティ。 `FontSize`プロパティに影響する方法、`Button`レイアウトで認識される場合は、`Scale`プロパティはありません。

[ **ScaleToSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ScaleToSize)サンプルを計算、`Scale`プロパティに適用される、`Label`要素をページ内に収まる中に可能な限り大きくなります。

### <a name="anchoring-the-scale"></a>アンカーのスケール

前の 3 つのサンプルでスケール要素がすべて増加または減少サイズ要素の中央を基準にします。 つまり、要素が増加またはすべての方向に同じサイズが減少します。 要素の中心点のみが同じ場所に、スケーリング中に残ります。

設定でスケーリングの中心を変更することができます、 [ `AnchorX` ](xref:Xamarin.Forms.VisualElement.AnchorX)と[ `AnchorY` ](xref:Xamarin.Forms.VisualElement.AnchorY)プロパティ。 これらのプロパティでは、要素自体に関連します。 `AnchorX`1 は右側にある、0 の値は、要素の左側にあるを参照します。 同様に`AnchorY`0 は、上部、および、1 は、下部にあります。 両方のプロパティでは、中心である 0.5 の既定値を指定します。

[ **AnchoredScaleDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/AnchoredScaleDemo)を実験することができます、`AnchorX`と`AnchorY`プロパティだけでなく`Scale`プロパティ。

Ios での既定以外の値を使用して`AnchorX`と`AnchorY`プロパティは、電話の向きの変更と互換性のある一般的に、します。

## <a name="the-rotation-transform"></a>回転変換

[ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation)プロパティが度で指定され、によって定義される要素の点を中心に時計回りの回転を示す`AnchorX`と`AnchorY`します。 [ **PlaneRotationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/PlaneRotationDemo)これら 3 つのプロパティを実験することができます。

### <a name="rotated-text-effects"></a>回転のテキスト効果

[ **BoxViewCircle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewCircle)サンプルでは回転小さな 64 を使用して円を描画するために必要な数学`BoxView`要素。

[ **RotatedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/RotatedText)サンプル表示複数`Label`に同じテキスト文字列を持つ要素を回転してスポークのように表示されます。

[ **CircularText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/CircularText)サンプルには、円でラップする表示されるテキスト文字列が表示されます。

### <a name="an-analog-clock"></a>アナログ時計

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリが含まれています、 [ `AnalogClockViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnalogClockViewModel.cs)時計の針の角度を計算するクラス。 クラスは、ViewModel でプラットフォームの依存関係を回避するために`Task.Delay`新しいを検索するためのタイマーではなく`DateTime`値。

含まれます**Xamarin.FormsBook.Toolkit**は、 [ `SecondTickConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondTickConverter.cs)を実装するクラス`IValueConverter`に最も近い 2 番目の 2 つ目の角度を丸める機能します。

[ **MinimalBoxViewClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/MinimalBoxViewClock) 3 つの回転を使用して`BoxView`アナログ時計を描画する要素。

[ **BoxViewClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewClock)使用`BoxView`より広範な画像の目盛りを含む、クロックの表面の周りをマークし、それぞれの端からほとんど距離を回転を渡しません。

[![BoxView クロックのスクリーン ショットをトリプル](images/ch21fg17-small.png "アナログ クロック表面")](images/ch21fg17-large.png#lightbox "アナログ時計の表面")

さらに、 [ `SecondBackEaseConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondBackEaseConverter.cs)クラス**Xamarin.FormsBook.Toolkit**を引き出せなかった少し、先に進む前に表示される 2 番目の手札を発生し、正しい位置に戻す。

### <a name="vertical-sliders"></a>垂直スライダーですか。

[ **VerticalSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/VerticalSliders)サンプルを`Slider`要素が 90 度回転できるし、も引き続き機能します。 ただし、回転したこれらの配置が難しいは`Slider`要素のレイアウトで、ままであるために表示を水平方向。

## <a name="3d-ish-rotations"></a>3D らしく回転

[ `RotationX` ](xref:Xamarin.Forms.VisualElement.RotationX)要素の上下方向にまたはビューアーから離れた場所に移動すると思われるように、3 D の x 軸の周りの要素を回転するプロパティが表示されます。 同様に、 [ `RotationY` ](xref:Xamarin.Forms.VisualElement.RotationY)に向かってまたはビューアーから離れた場所に移動すると思われる左辺と要素の右辺を y 軸の周りの要素を回転させるようです。

`AnchorX`プロパティに影響`RotationY`なく`RotationX`します。 `AnchorY`プロパティに影響`RotationX`なく`RotationY`します。 試すことができます、 [ **ThreeDeeRotationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ThreeDeeRotationDemo)これらのプロパティの相互作用を調査するためのサンプルです。

Xamarin.Forms で暗黙的に指定する 3D 座標系は、左手座標系です。 ポイントした場合は、(右) に増加の X 方向に左手の人差し指を調整し、中指を Y の増加方向には、(ダウン)、Z 座標 (画面) 外を増加させる方向で、thumb ポイントし、調整します。

また、3 つの軸のいずれかの値を増加させる方向で、左側の親指をポイントする場合、指の曲線方向を示します正の回転角度の回転の。

## <a name="related-links"></a>関連リンク

- [第 21 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch21-Apr2016.pdf)
- [第 21 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21)
