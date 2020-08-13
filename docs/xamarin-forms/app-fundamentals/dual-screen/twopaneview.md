---
title: Xamarin.Forms のデュアル画面のレイアウト
description: このガイドでは、Xamarin.Forms の TwoPaneView を使用して Surface Duo や Surface Neo などのデュアル画面デバイスのアプリ エクスペリエンスを最適化する方法について説明します。
ms.prod: xamarin
ms.assetid: 17ee8afa-5e7c-4a4f-a9b6-2aca03f30fe3
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 734dea456af56f4103691e0368ae72202bce9556
ms.sourcegitcommit: 08290d004d1a7e7ac579bf1f96abf8437921dc70
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/07/2020
ms.locfileid: "87918241"
---
# <a name="no-locxamarinforms-twopaneview-layout"></a>Xamarin.Forms TwoPaneView レイアウト

![プレリリース API](~/media/shared/preview.png)

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-dualscreendemos/)

`TwoPaneView` クラスはコンテンツのサイズを変更し、左右または上下の使用可能な領域に配置する 2 つのビューを持つコンテナーを表しています。 `TwoPaneView` は `Grid` から継承するため、これらのプロパティについてはグリッドに適用されているかのように考える方法が最も簡単です。

## <a name="set-up-twopaneview"></a>TwoPaneView を設定する

次の手順に従って、アプリにデュアル画面レイアウトを作成します。

1. [概要](index.md)手順に従って NuGet を追加し、Android `MainActivity` クラスを構成します。
1. 次の XAML を使用し、基本的な `TwoPaneView` で開始します。

    ```xaml
    <ContentPage
        xmlns:dualScreen="clr-namespace:Xamarin.Forms.DualScreen;assembly=Xamarin.Forms.DualScreen">
        <dualScreen:TwoPaneView>
            <dualScreen:TwoPaneView.Pane1>
                <StackLayout>
                    <Label Text="Pane1 Content" />
                </StackLayout>
            </dualScreen:TwoPaneView.Pane1>
            <dualScreen:TwoPaneView.Pane2>
                <StackLayout>
                    <Label Text="Pane2 Content" />
                </StackLayout>
            </dualScreen:TwoPaneView.Pane2>
        </dualScreen:TwoPaneView>
    </ContentPage>
    ```

> [!TIP]
> 上の XAML では、`ContentPage` 要素から多くの共通属性が除外されています。 アプリに `TwoPaneView` を追加するとき、画像のように `xmlns:dualScreen` 名前空間を忘れずに宣言してください。

## <a name="understand-twopaneview-modes"></a>TwoPaneView モードについて

アクティブにできるのは、次のモードのうち 1 つだけです。

- `SinglePane` 同時に表示できるペインは 1 つだけです。
- `Wide` 2 つのペインが水平方向にレイアウトされます。 左側に 1 つのペインが表示され、もう 1 つが右側に表示されます。 2 画面で、デバイスが縦向きのときはこのモードになります。
- `Tall` 2 つのペインが垂直方向にレイアウトされます。 1 つのペインが上に表示され、もう 1 つが下に表示されます。 2 画面で、デバイスが横向きのときはこのモードになります。

## <a name="control-twopaneview-when-its-only-on-one-screen"></a>1 画面しかないときに TwoPaneView を制御する

`TwoPaneView` が 1 つの画面を占有する場合は、次のプロパティが適用されます。

- `MinTallModeHeight` は縦長モードにするためにコントロールが必要な高さの最小値を示します。
- `MinWideModeWidth` は横長モードにするためにコントロールが必要な幅の最小値を示します。
- `Pane1Length` では横長モードで Pane1 の幅が、縦長モードで Pane1 の高さが設定され、SinglePane モードでは効果がありません。
- `Pane2Length` では横長モードで Pane2 の幅が、縦長モードで Pane2 の高さが設定され、SinglePane モードでは効果がありません。

> [!IMPORTANT]
> `TwoPaneView` が 2 画面にまたがっている場合、これらのプロパティは効果がありません。

## <a name="properties-that-apply-when-on-one-screen-or-two"></a>1 画面または 2 画面の場合に適用されるプロパティ

`TwoPaneView` が 1 つまたは 2 つの画面を占有する場合は、次のプロパティが適用されます。

- `TallModeConfiguration` は、縦長モードの場合の上/下の配置を示します。または、TwoPaneViewPriority で定義されているように 1 つのペインだけを表示したい場合です。
- `WideModeConfiguration` は、横長モードの場合の左/右の配置を示します。または、TwoPaneViewPriority で定義されているように 1 つのペインだけを表示したい場合です。
- `PanePriority` は SinglePane モードの場合に Pane1 または Pane2 を表示するかどうかを決定します。

## <a name="related-links"></a>関連リンク

- [DualScreen (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-dualscreendemos/)
