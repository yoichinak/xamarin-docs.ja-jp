---
title: Xamarin.Android と Wear の OS の画面サイズの使用
ms.prod: xamarin
ms.assetid: 77831169-C663-4D42-B742-B8B556B1DA4B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 9fc22a3c08b60a8474b006f1c9225155b9705507
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113119"
---
# <a name="working-with-screen-sizes"></a>画面サイズの使用

四角形またはラウンドの表示、さまざまなサイズにすることもできますが、android Wear デバイスを持つことができます。

![四角形と円形 Wear のスクリーン ショットを表示します](screen-sizes-images/moyeu-wear.png)

## <a name="identifying-screen-type"></a>画面の種類を識別します。

Wear サポート ライブラリを提供する際に役立ついくつかのコントロールを検出およびなど、さまざまな画面の図形に適応`WatchViewStub`と`BoxInsetLayout`します。

ライブラリ コントロールをサポートしてその他のことに注意してください (など`GridViewPager`)*自動的に*自体の画面の図形を検出し、コントロールの子が以下に示すように追加することはできません。

### <a name="watchviewstub"></a>WatchViewStub

参照してください、 [WatchViewStub](https://developer.xamarin.com/samples/WatchViewStub/)サンプルを画面の種類を検出し、種類ごとに異なるレイアウトを表示する方法を参照してください。

メインのレイアウト ファイルが含まれています、`android.support.wearable.view.WatchViewStub`を使用して四角形と円形の画面のさまざまなレイアウトを参照する、`app:rectLayout`と`app:roundLayout`属性。

```xml
<android.support.wearable.view.WatchViewStub
    xmlns:app="http://schemas.android.com/apk/res-auto"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  android:id="@+id/stub"
  app:rectLayout="@layout/rect_layout"
  app:roundLayout="@layout/round_layout" />
```

ソリューションには、実行時に選択されますが、各スタイルのさまざまなレイアウトが含まれています。

![ファイル リソース/レイアウトの下に表示](screen-sizes-images/solution.png)


### <a name="boxinsetlayout"></a>BoxInsetLayout

各画面の種類ごとに異なるレイアウトを構築するのではなく四角形またはラウンド画面に適応できる単一のビューも作成できます。

これは、 [Google 例](https://developer.android.com/training/wearables/ui/layouts.html#same-layout)を使用する方法を示しています、`BoxInsetLayout`四角形と円形の両方の画面で、同じレイアウトを使用します。


## <a name="wear-ui-designer"></a>Wear UI デザイナー

Xamarin Android Designer には、四角形と円形の両方の画面がサポートされています。

![Xamarin Android Designer で Android Wear の正方形の画面を選択します。](screen-sizes-images/design-screen-type.png)

四角形のスタイルでデザイン画面を次に示します。

![四角形のスタイルでデザイン画面](screen-sizes-images/design-rect.png) 

Round スタイルでデザイン画面を次に示します。

![Round スタイルでデザイン画面](screen-sizes-images/design-round.png)


## <a name="wear-simulator"></a>Wear シミュレーター

**Google エミュレーター マネージャー**画面の種類の両方のデバイスの定義が含まれています。 アプリをテストする四角形と円形のエミュレーターを作成することができます。

![Wear デバイス定義の Google エミュレーター マネージャーで表示されます。](screen-sizes-images/emulator-devices.png)

エミュレーターは、次のように四角形の画面にレンダリングされます。

![エミュレーターの四角形の画面の表示](screen-sizes-images/recipe-2.png) 

このようなラウンド画面にレンダリングされます。

![エミュレーター ラウンド画面の表示](screen-sizes-images/recipe-2-round.png)

## <a name="video"></a>ビデオ

[Android Wear 用アプリを全画面表示](https://www.youtube.com/watch?v=naf_WbtFAlY)から[developers.google.com](https://www.youtube.com/channel/UC_x5XG1OV2P6uZZ5FSM9Ttw)します。

