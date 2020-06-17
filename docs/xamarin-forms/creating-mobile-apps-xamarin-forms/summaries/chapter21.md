---
title:"第 21 章の概要: 変換" description:"Xamarin.Forms でモバイル アプリを作成する: 第 21 章の概要: 変換" ms.prod: xamarin ms.technology: xamarin-forms ms.assetid:3642F112-C7FA-4A74-9000-F9087BA89AD9 author: davidbritch ms.author: dabritch ms.date:11/07/2017 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="summary-of-chapter-21-transforms"></a>第 21 章の概要: 変換

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21)

Xamarin.Forms ビューは、通常は `Layout` または `Layout<View>` の派生クラスであるその親によって決定された場所とサイズで、画面に表示されます。 "*変換*" は、該当の場所、サイズ、または向きさえも変更できる Xamarin.Forms の機能です。

Xamarin.Forms では、次の 3 種類の基本的な変換をサポートしています。

- "*移動*" &mdash; 要素を水平方向または垂直方向にシフトする
- "*スケーリング*" &mdash; 要素のサイズを変更する
- "*回転*" &mdash; 点または軸に従って要素を回転させる

Xamarin.Forms では、スケーリングは等方性であり、幅と高さに均一に影響を及ぼします。 回転は、画面の 2 次元平面と 3 次元空間の両方でサポートされています。 傾斜 (ほぼ垂直の) 変換および汎用化されたマトリックス変換はありません。

変換は、`VisualElement` クラスで定義された `double` 型の 8 つのプロパティによってサポートされています。

- [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX)
- [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY)
- [`Scale`](xref:Xamarin.Forms.VisualElement.Scale)
- [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)
- [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX)
- [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY)
- [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX)
- [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY)

これらのプロパティはすべて、バインド可能なプロパティによってサポートされています。 また、データ バインディングとスタイル設定のターゲットにすることができます。 「[**第 22 章: アニメーション**](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter22.md)」では、これらのプロパティをアニメーション化する方法を示していますが、この章のいくつかのサンプルでは、Xamarin.Forms の[タイマー](~/xamarin-forms/platform/device.md#devicestarttimer)を使ってアニメーション化する方法を示しています。

変換のプロパティは、要素のレンダリング方法のみに影響を与え、要素がレイアウト内で認識される方法に影響を与えることは "*ありません*"。

## <a name="the-translation-transform"></a>移動による変換

[`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) および [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) プロパティに 0 以外の値を設定すると、水平または垂直に要素がシフトされます。

[**TranslationDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TranslationDemo) プログラムでは、`Frame` の `TranslationX` および `TranslationY` プロパティを制御する 2 つの `Slider` 要素を利用して、これらのプロパティを試すことができます。 また、変換によって、該当の `Frame` のすべての子も影響を受けます。

### <a name="text-effects"></a>テキストの文字飾り

移動プロパティの 1 つの一般的な用途は、テキストのレンダリングをわずかにオフセットすることです。 このことは、以下のように、[**TextOffsets**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TextOffsets) サンプルに示されています。

[![Text Offsets のトリプル スクリーンショット](images/ch21fg03-small.png "テキスト オフセット")](images/ch21fg03-large.png#lightbox "テキスト オフセット")

[**BlockText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BlockText) サンプルに示されているように、もう 1 つのエフェクトは、`Label` の複数のコピーを 3 次元ブロックのようにレンダリングすることです。

### <a name="jumps-and-animations"></a>ジャンプとアニメーション

[**ButtonJump**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonJump) サンプルでは、移動を使ってタップされるたびに `Button` を動かしますが、その主な目的は、`Button` ではボタンがレンダリングされた場所でユーザー入力を受け取ることを示すことです。

[**ButtonGlide**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonGlide) サンプルもよく似ていますが、タイマーを使用してある点から別の点へ `Button` をアニメーション化します。

## <a name="the-scale-transform"></a>スケールによる変換

[`Scale`](xref:Xamarin.Forms.VisualElement.Scale) 変換では、レンダリングされる要素のサイズを拡大縮小できます。 既定値は 1 です。 値 0 を指定すると、要素は非表示になります。 負の値を指定すると、要素は 180 度回転して表示されます。 `Scale` プロパティは、要素の `Width` または `Height` プロパティには影響を与えません。 それらの値は同じままです。

[**SimpleScaleDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/SimpleScaleDemo) サンプルを使用して、`Scale` プロパティを試すことができます。

[**ButtonScaler**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonScaler) サンプルでは、`Button` の `Scale` プロパティのアニメーション化と `FontSize` プロパティのアニメーション化の相違点を示しています。 `FontSize` プロパティは、`Button` がレイアウト内で認識される方法に影響を与えますが、`Scale` プロパティは影響を与えません。

[**ScaleToSize**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ScaleToSize) サンプルでは、`Label` 要素に適用される `Scale` プロパティを計算して、引き続きページ内に収めたまま、可能な限り拡大します。

### <a name="anchoring-the-scale"></a>スケールの固定

前の 3 つのサンプルでスケーリングされた要素のサイズはすべて、要素の中心を基準にして拡大または縮小されています。 言い換えると、要素は、全方向で同じサイズに拡大または縮小されます。 スケーリング時には、要素の中心にある点だけが同じ場所に保持されます。

スケーリングの中心を変更するには、[`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX) および [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY) プロパティを設定します。 これらのプロパティは、要素自体に対して相対的です。 `AnchorX` では、値 0 は要素の左側を表し、1 は右側を表します。 同様に、`AnchorY` では、0 が一番上、1 が一番下になります。 どちらのプロパティも、既定値は中央である 0.5 です。

[**AnchoredScaleDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/AnchoredScaleDemo) サンプルでは、`AnchorX` および `AnchorY` プロパティと共に、`Scale` プロパティを試すことができます。

iOS では、`AnchorX` および `AnchorY` プロパティの既定値以外の値を使用しても、通常は、スマートフォンの向きの変化には対応できません。

## <a name="the-rotation-transform"></a>回転による変換

[`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) プロパティは角度で指定され、`AnchorX` および `AnchorY` によって定義された要素の点を中心とする時計回りの回転を指示します。 [**PlaneRotationDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/PlaneRotationDemo) では、これら 3 つのプロパティを試すことができます。

### <a name="rotated-text-effects"></a>回転したテキストの文字飾り

[**BoxViewCircle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewCircle) サンプルでは、64 個の回転された小さな`BoxView` 要素を使用して円を描画するために必要な計算を示しています。

[**RotatedText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/RotatedText) サンプルでは、スポークのように見せるために、回転された同じテキスト文字列を含む複数の `Label` 要素を表示しています。

[**CircularText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/CircularText) サンプルでは、テキスト文字列が円の中で折り返して見えるように表示しています。

### <a name="an-analog-clock"></a>アナログ時計

[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ライブラリには、時計に対応させて角度を計算する [`AnalogClockViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnalogClockViewModel.cs) クラスがあります。 クラスでは、ViewModel でのプラットフォームの依存関係を回避するために、新しい `DateTime` 値の検索にタイマーではなく `Task.Delay` が使用されます。

また、**Xamarin.FormsBook.Toolkit** には、`IValueConverter` を実装して 1 秒の角度を最も近い秒に丸めて提示する [`SecondTickConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondTickConverter.cs) クラスもあります。

[**MinimalBoxViewClock**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/MinimalBoxViewClock) では、3 つの回転する `BoxView` 要素を使用して、アナログ時計を描画します。

[**BoxViewClock**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewClock) では 、時計のフェイスを囲む目盛りや、端からわずかな距離を回転して進む秒針など、より拡張されたグラフックスのために `BoxView` が使用されます。

[![BoxView Clock のトリプル スクリーンショット](images/ch21fg17-small.png "アナログ時計のフェイス")](images/ch21fg17-large.png#lightbox "アナログ時計のフェイス")

加えて、**Xamarin.FormsBook.Toolkit** の [`SecondBackEaseConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondBackEaseConverter.cs) クラスでは、先へ進む前にわずかに引き戻してから適切な位置に移動するように、秒針を表示します。

### <a name="vertical-sliders"></a>垂直のスライダーになるか

[**VerticalSliders**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/VerticalSliders) サンプルでは、`Slider` 要素が 90 度回転されても機能できることを示しています。 ただし、回転されたこれらの `Slider` 要素は、レイアウトでは引き続き水平に表示されるので、配置するのは難しいです。

## <a name="3d-ish-rotations"></a>3 次元のような回転

[`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX) プロパティでは、要素の上端と下端がビューアーとの間で移動して見えるように、3 次元の X 軸を中心に要素を回転して表示します。 同様に、[`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY) プロパティでは、要素の左端と右端がビューアーとの間で移動して見えるように、3 次元の Y 軸を中心に要素を回転して見せています。

`AnchorX` プロパティは `RotationY` に影響を与えますが、`RotationX` には影響を与えません。 `AnchorY` プロパティは `RotationX` に影響を与えますが、`RotationY` には影響を与えません。 [**ThreeDeeRotationDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ThreeDeeRotationDemo) サンプルを試して、これらのプロパティの相互作用を確認できます。

Xamarin.Forms によって暗黙的に示される 3 次元の座標系は、左手に対応しています。 左手の人差し指で X 座標の正の方向 (右) を指し、中指で Y 座標の負の方向を (下) を指すと、親指は Z 座標の正の方向 (画面外) を指します。

また、3 つの軸のいずれかに対して、左手の親指で値の正の方向を指すと、残りの指の曲がり方によって、正の回転角度に対する回転方向が示されます。

## <a name="related-links"></a>関連リンク

- [第 21 章の全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch21-Apr2016.pdf)
- [第 21 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21)
