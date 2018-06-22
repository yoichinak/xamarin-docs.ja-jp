---
title: Xamarin.Android とウェア OS で画面のサイズの使用
ms.prod: xamarin
ms.assetid: 77831169-C663-4D42-B742-B8B556B1DA4B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/25/2018
ms.openlocfilehash: 40e7850ffe239b0ede43e4d0cd3c6da08bce3a40
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
ms.locfileid: "32436077"
---
# <a name="working-with-screen-sizes"></a>画面サイズの使用

Android の消耗デバイスには、四角形またはさまざまなサイズを指定できますもラウンドの表示のいずれかを持つことができます。

![四角形と円形の消耗のスクリーン ショットが表示されます。](screen-sizes-images/moyeu-wear.png)

## <a name="identifying-screen-type"></a>画面の種類を識別します。

消耗サポート ライブラリを提供するのに役立ついくつかのコントロールを検出しなどのさまざまな画面の図形に適応させる`WatchViewStub`と`BoxInsetLayout`です。

ライブラリのコントロールをサポートして、他のいくつかのことに注意してください (など`GridViewPager`)*自動的に*自体画面の図形を検出し、次のコントロールの子のように追加することはできません。

### <a name="watchviewstub"></a>WatchViewStub

参照してください、 [WatchViewStub](https://developer.xamarin.com/samples/WatchViewStub/)サンプルを画面の種類を検出し、種類ごとに異なるレイアウトを表示する方法を参照してください。

メインのレイアウト ファイルが含まれています、`android.support.wearable.view.WatchViewStub`を参照する各種レイアウトの四角形と円形の画面を使用して、`app:rectLayout`と`app:roundLayout`属性。

```xml
<android.support.wearable.view.WatchViewStub
    xmlns:app="http://schemas.android.com/apk/res-auto"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  android:id="@+id/stub"
  app:rectLayout="@layout/rect_layout"
  app:roundLayout="@layout/round_layout" />
```

ソリューションには、実行時に選択されますが、各スタイルに対してさまざまなレイアウトが含まれています。

![ファイル リソース/レイアウトの下に表示](screen-sizes-images/solution.png)


### <a name="boxinsetlayout"></a>BoxInsetLayout

各画面の種類ごとに異なるレイアウトを構築するのではなく、四角形または round の画面に合わせて調整する 1 つのビューを作成することもできます。

これは、 [Google 例](https://developer.android.com/training/wearables/ui/layouts.html#same-layout)を使用する方法を示します、`BoxInsetLayout`四角形と円形の両方の画面で、同じレイアウトを使用します。


## <a name="wear-ui-designer"></a>デザイナーの UI を着用します。

Xamarin Android デザイナーには、四角形と円形の両方の画面がサポートされています。

![Xamarin Android デザイナーで、Android 消耗正方形の画面を選択します。](screen-sizes-images/design-screen-type.png)

四角形のスタイルでデザイン画面を次に示します。

![四角形のスタイルでデザイン画面](screen-sizes-images/design-rect.png) 

Round スタイルでデザイン画面を次に示します。

![Round スタイルでデザイン画面](screen-sizes-images/design-round.png)


## <a name="wear-simulator"></a>シミュレーターを着用します。

**Google エミュレーター マネージャー**両方の画面の種類のデバイスの定義が含まれています。 アプリをテストする四角形と円形のエミュレーターを作成することができます。

![Google エミュレーター マネージャーでデバイスの定義を着用します。](screen-sizes-images/emulator-devices.png)

エミュレーターは、次のように、四角形の画面の描画されます。

![エミュレーター四角形の画面の表示](screen-sizes-images/recipe-2.png) 

これは、round、画面の次のように描画されます。

![エミュレーター round 画面の表示](screen-sizes-images/recipe-2-round.png)

## <a name="video"></a>ビデオ

[Android の消耗の全画面アプリ](https://www.youtube.com/watch?v=naf_WbtFAlY)から[developers.google.com](https://www.youtube.com/channel/UC_x5XG1OV2P6uZZ5FSM9Ttw)です。

