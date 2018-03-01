---
title: "21 章の概要です。 変換"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 378ce3fb39cfb5c42d5ec7611415f5420146a9cc
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-21-transforms"></a>21 章の概要です。 変換

Xamarin.Forms ビューの位置と、その親では、通常、これによって決定されるサイズ、画面に表示されます、`Layout`または`Layout<View>`から派生します。 *変換*Xamarin.Forms 機能であり、その場所、サイズ、または偶数方向に変更できます。

Xamarin.Forms は、次の 3 つの基本的な種類の変換をサポートします。

- *翻訳*& #x 2014; 要素を水平方向または垂直方向にシフト
- *スケール*& #x 2014; 要素のサイズの変更
- *回転*& #x 2014; ポイントまたは軸の周りの要素を有効にします。

Xamarin.Forms でスケーリングはアイソトロ ピックです。影響の幅と高さ一様に分布します。 回転には、両方で 2 次元表面画面の 3D スペースではサポートされています。 ないずれ (または膨大な) 変換、汎用化された行列変換されません。

型の 8 つのプロパティでサポートされている変換`double`によって定義された、`VisualElement`クラス。

- [`TranslationX`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/)
- [`TranslationY`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/)
- [`Scale`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/)
- [`Rotation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/)
- [`RotationX`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationX/)
- [`RotationY`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationY/)
- [`AnchorX`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/)
- [`AnchorY`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/)

これらすべてのプロパティは、バインド可能なプロパティでサポートされます。 データ バインディングのターゲットにすることができます、スタイルを設定します。 [**章 22 です。アニメーション**](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter22.md)これらのプロパティをアニメーション化できる方法を示しますが、この章で一部のサンプルを表示、Xamarin.Forms を使用してアニメーション化[タイマー](~/xamarin-forms/platform/device.md#Device_StartTimer)です。

変換方法のみ、要素が含まれ、操作を行いますのプロパティは、*いない*レイアウトで要素を認識する方法に影響します。

## <a name="the-translation-transform"></a>変換の変換

0 以外の値の[ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/)と[ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/)プロパティは、水平方向または垂直方向に要素をシフトします。

[ **TranslationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TranslationDemo)プログラムでは、これらのプロパティを 2 つを実験する`Slider`を制御する要素、`TranslationX`と`TranslationY`のプロパティ`Frame`. 変換にも影響するすべての子`Frame`です。

### <a name="text-effects"></a>テキスト効果

変換のプロパティの 1 つの一般的な用途では、テキストのレンダリングでオフセットわずかです。 これに示されている、 [ **TextOffsets** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TextOffsets)サンプル。

[![テキストのオフセットのトリプル スクリーン ショット](images/ch21fg03-small.png "テキスト オフセット")](images/ch21fg03-large.png "テキスト オフセット")

別の効果はの複数のコピーを表示するためには、`Label`に示されているなど、3 D のブロックと同じように、 [ **BlockText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BlockText)サンプルです。

### <a name="jumps-and-animations"></a>ジャンプとアニメーション

[ **ButtonJump** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonJump)サンプルでは、翻訳を使用して、移動、`Button`タップすると、主な目的は、デモンストレーションをするときに、`Button`位置にユーザー入力を受け取り、ボタンが表示されます。

[ **ButtonGlide** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonGlide)サンプルと似ていますが、タイマーを使用して、アニメーション、`Button`を別の 1 つのポイントから。

## <a name="the-scale-transform"></a>スケールの変換

[ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/)トランス フォームは増やすことも、レンダリングされる要素のサイズを減らします。 既定値は 1 です。 値 0 は、表示する要素です。 負の値には、要素を 180 度回転する表示が発生します。 `Scale`プロパティには影響しません、`Width`または`Height`要素のプロパティです。 これらの値は変わりません。

試すことができます、`Scale`プロパティを使用して、 [ **SimpleScaleDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/SimpleScaleDemo)サンプルです。

[ **ButtonScaler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonScaler)サンプルでは、アニメーションの違い、`Scale`のプロパティ、`Button`をアニメーション化して、`FontSize`プロパティです。 `FontSize`プロパティに影響する方法、`Button`レイアウトでは、見かけ上の`Scale`プロパティはありません。

[ **ScaleToSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ScaleToSize)サンプルを計算、`Scale`プロパティに適用される、`Label`要素をページ内に収まる中に可能な限り大きくなります。

### <a name="anchoring-the-scale"></a>固定小数点以下桁数

前の 3 つのサンプルにスケーリング要素がすべて増加または要素の中心を基準としてサイズでは減少します。 つまり、要素が増加またはすべての方向に同じサイズ減少します。 要素の中央に点だけでは、スケーリング中に同じ場所に残ります。

設定でスケーリングの中心を変更することができます、 [ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/)と[ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/)プロパティです。 これらのプロパティは、要素自体です。 `AnchorX`0 の値を指す要素の左側にある、および 1 は、右側にあるを参照します。 同様に`AnchorY`0 は、top、および 1 は、下部にあります。 両方のプロパティでは、中央は 0.5 の既定値を設定します。

[ **AnchoredScaleDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/AnchoredScaleDemo)サンプルを試すことができます、`AnchorX`と`AnchorY`プロパティだけでなく`Scale`プロパティです。

Ios の場合の既定以外の値を使用して`AnchorX`と`AnchorY`プロパティは、電話の向きの変更と互換性のある一般的にします。

## <a name="the-rotation-transform"></a>回転変換

[ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/)プロパティ度で指定され、によって定義された要素の点を中心に時計回りの回転を示す`AnchorX`と`AnchorY`です。 [ **PlaneRotationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/PlaneRotationDemo)これら 3 つのプロパティをテストすることができます。

### <a name="rotated-text-effects"></a>回転したテキスト効果

[ **BoxViewCircle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewCircle)サンプルでは回転小さな 64 を使用して円を描画するために必要な数学`BoxView`要素。

[ **RotatedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/RotatedText)サンプルには、複数が表示され`Label`スポークと同じように同じテキスト文字列を持つ要素を回転します。

[ **CircularText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/CircularText)サンプルは、円で囲んだをラップする表示されるテキスト文字列を表示します。

### <a name="an-analog-clock"></a>アナログ時計

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリに含まれる、 [ `AnalogClockViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnalogClockViewModel.cs)時計の針の角度を計算するクラス。 ViewModel、クラスの使用のプラットフォームの依存関係を回避する`Task.Delay`新しいを検索するためのタイマーではなく`DateTime`値。

含まれます**Xamarin.FormsBook.Toolkit**は、 [ `SecondTickConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondTickConverter.cs)を実装するクラス`IValueConverter`を最も近い秒を 2 つ目の角度を丸めるに機能します。

[ **MinimalBoxViewClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/MinimalBoxViewClock) 3 つの回転を使用して`BoxView`アナログ時計を描画する要素。

[ **BoxViewClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewClock)使用`BoxView`より広範な画像のティックを含む、クロックの顔周りをマークし、それぞれの端からほとんど距離を回転を渡しません。

[![BoxView クロックのスクリーン ショットをトリプル](images/ch21fg17-small.png "アナログ時計の表面")](images/ch21fg17-large.png "アナログ時計の表面")

さらに、 [ `SecondBackEaseConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondBackEaseConverter.cs)クラス**Xamarin.FormsBook.Toolkit**秒針を引き出せなかった少し先、見る前に表示すると、し、正しい位置に戻します。

### <a name="vertical-sliders"></a>垂直スライダーしますか。

[ **VerticalSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/VerticalSliders)サンプルを`Slider`要素が 90 度回転でき、機能します。 ただし、回転これらの配置が難しいは`Slider`要素のレイアウトでれているためにと思われる水平方向です。

## <a name="3d-ish-rotations"></a>3D らしく回転

[ `RotationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationX/)に向かってまたはビューアーから離れた場所に移動する要素の上下に見えるため、3 D の x 軸の周りの要素を回転させたときにプロパティが表示されます。 同様に、 [ `RotationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationY/)に向かってまたはビューアーから離れた場所に移動するよう左および要素の右側に y 軸の周りの要素を回転するように見えます。

`AnchorX`プロパティに影響を与えます`RotationY`ではなく`RotationX`です。 `AnchorY`プロパティに影響を与えます`RotationX`ではなく`RotationY`です。 試すことができます、 [ **ThreeDeeRotationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ThreeDeeRotationDemo)をこれらのプロパティの相互作用を調査するサンプルです。

Xamarin.Forms で暗黙的に指定 3D 座標系では、左利きします。 ポイントする場合は、(右側) に増加する X の方向で、左側の人差し指を調整し、(ダウン)、Z 座標 (画面) 外を増加させる方向で、thumb ポイントし、増加する Y の方向に中央指を調整します。

またの 3 つの軸のいずれか、値を増加させる方向で、左側のつまみをポイントする場合、指の曲線方向を示す回転の角度に正の回転の。



## <a name="related-links"></a>関連リンク

- [21 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch21-Apr2016.pdf)
- [21 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21)
