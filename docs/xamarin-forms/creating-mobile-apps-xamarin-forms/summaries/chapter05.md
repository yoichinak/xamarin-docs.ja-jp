---
title: "第 5 章の概要です。 サイズを処理します。"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 486800E9-C09F-4B95-9AC2-C0F8FE563BCF
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 1df1751c55c6a031bf9f26d774b739f4ca83fa91
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-5-dealing-with-sizes"></a>第 5 章の概要です。 サイズを処理します。

Xamarin.Forms でいくつかのサイズがこれまでに発生しました。

- IOS のステータス バーの高さは 20 です。
- `BoxView`には、既定の幅と高さ 40
- 既定値`Padding`で、`Frame`は 20 です
- 既定値`Spacing`上、 `StackLayout` 6 は、
- `Device.GetNamedSize`メソッド数値のフォント サイズを返します

これらのサイズがピクセルです。 これらはむしろデバイス非依存単位を個別に各プラットフォームで認識されません。

## <a name="pixels-points-dps-dips-and-dius"></a>ピクセル、ポイント、dp、Dip、および DIUs

Apple Mac および Microsoft Windows の履歴の早い段階では、プログラマはピクセル単位で作業します。 ただし、高解像度ディスプレイの出現には、画面座標に複数仮想化され、抽象アプローチが必要です。 単位で世界では、Mac、プログラマが動作していた*ポイント*、Windows 開発者が使用中に、1/72 インチ従来*デバイス非依存単位*1/96 インチに基づいて (DIUs)。

ただし、モバイル デバイスの表面によく似ては一般に、保持、デスクトップより高い解像度画面で、ピクセル密度の大きい値を許容できることを意味します。

Apple の iPhone および iPad デバイスを対象とするプログラマがの単位で作業を続行*ポイント*がこれらのポイント 1 インチの 160 です。 デバイスによってある可能性があります 1、2、または 3 のピクセルをポイントします。

Android と似ています。 プログラマが作業の単位で*密度非依存ピクセル*(dp)、ピクセルと dp 間のリレーションシップは 1 インチ 160 dp に基づいています。

Windows ランタイムでは、1 インチの 160 デバイス非依存単位に近いものを示唆するスケール ファクターは確立もいます。

要約するでスマート フォンやタブレットを対象とする Xamarin.Forms プログラマはすべての販売単位が次の条件に基づいていると見なすことができます。

- 1 インチに相当する 160 単位
- 64、センチメートル単位

読み取り専用[ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/)と[ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/)によって定義されたプロパティ`VisualElement`「模擬表示」の値 & #x 2013 です。 既定値がある 1 です。 要素がサイズし、レイアウトで対応されている場合にのみこれらのプロパティはデバイスに依存しない単位内の要素の実際のサイズが反映されます。 このサイズが含まれる`Padding`要素で設定がない、`Margin`です。

視覚的要素が発生、 [ `SizeChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.SizeChanged/)イベントとその`Width`または`Height`が変更されました。 [ **WhatSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/WhatSize)サンプルでは、このイベントを使用して、プログラムの画面のサイズを表示します。

## <a name="metrical-sizes"></a>Metrical サイズ

[ **MetricalBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/MetricalBoxView)使用[ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/)と[ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/)を表示する、 `BoxView` 1 インチ高さと 1 つセンチメートルの幅。

## <a name="estimated-font-sizes"></a>推定のフォント サイズ

[**フォント サイズ**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FontSizes)サンプル 160 単位-インチにルールを使用して、フォント サイズをポイント単位で指定する方法を示しています。 この手法を使用してプラットフォーム間で視覚的な一貫性がよりも良い`Device.GetNamedSize`します。

## <a name="fitting-text-to-available-size"></a>利用可能なサイズに収まらない

計算することで、特定の四角形内のテキストのブロックに合わせて可能であれば、`FontSize`の`Label`次の条件を使用します。

- 行間とは、フォント サイズ (Windows プラットフォームで 130%) の 120% です。
- 平均の文字幅は、フォント サイズの 50% です。

[ **EstimatedFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EstimatedFontSize)サンプルは、この手法を示します。 このプログラムが以前に作成された、 [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/)を使うようにプロパティが、使用可能な[ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)で、 [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/)をシミュレートするには設定、マージンです。

[![推定のフォント サイズの 3 倍のスクリーン ショット](images/ch05fg07-small.png "テキストが利用可能なサイズに合わせる")](images/ch05fg07-large.png#lightbox "テキストが利用可能なサイズに合わせる")

## <a name="a-fit-to-size-clock"></a>サイズに合わせる-クロック

[ **FitToSizeClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FitToSizeClock)サンプルの使用例[ `Device.StartTimer` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.StartTimer/p/System.TimeSpan/System.Func%7BSystem.Boolean%7D/)は時計を更新する際に、アプリケーションを定期的に通知するタイマーを開始します。 フォント サイズは、表示可能な限り大きくに 1 つの 6 番目のページの幅に設定されます。

## <a name="accessibility-issues"></a>アクセシビリティの問題

**EstimatedFontSize**プログラム、および**FitToSizeClock**両方のプログラムに不備が含まれている: ユーザーが Android または Windows 10 Mobile、不要になったプログラムでスマート フォンのユーザー補助の設定を変更するかどうか見積もることができます大きさ描画されるテキスト フォントのサイズに基づきます。 [ **AccessibilityTest** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/AccessibilityTest)サンプルは、この問題を示します。

## <a name="empirically-fitting-text"></a>調整テキスト経験的

四角形にテキストに合わせての別の方法では、経験的表示されるテキストのサイズを計算し、上下を調整することです。 書籍の呼び出しで、プログラム[ `GetSizeRequest` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.GetSizeRequest/p/System.Double/System.Double/)で視覚的要素を要素の目的のサイズを取得します。 メソッドは廃止されて、そのプログラムを呼び出す代わりにする必要があります [`Measure`] (/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/)。

`Label`、最初の引数は、(折り返しを許可) コンテナーの幅を使用する必要がありますが、2 番目の引数を設定する必要がありますに`Double.PositiveInfinity`制約なしを高さにします。 [ **EmpiricalFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EmpiricalFontSize)サンプルは、この手法を示します。



## <a name="related-links"></a>関連リンク

- [第 5 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch05-Apr2016.pdf)
- [第 5 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05)
- [第 5 章 f# のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FS)
