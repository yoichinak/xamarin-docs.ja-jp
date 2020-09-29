---
title: Xamarin. Android および磨耗 OS での画面サイズの操作
ms.prod: xamarin
ms.assetid: 77831169-C663-4D42-B742-B8B556B1DA4B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/25/2018
ms.openlocfilehash: 1747601596cd1772210d9a66755d7aa98ca14052
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457030"
---
# <a name="working-with-screen-sizes"></a>画面サイズの操作

Android の磨耗デバイスは、四角形または丸いディスプレイを持つことができます。サイズも異なる場合があります。

![四角形と丸い磨耗の画面のスクリーンショット](screen-sizes-images/moyeu-wear.png)

## <a name="identifying-screen-type"></a>画面の種類の識別

磨耗サポートライブラリには、やなど、さまざまな画面図形の検出と調整に役立つコントロールが用意されて `WatchViewStub` `BoxInsetLayout` います。

他のサポートライブラリコントロール (など) の一部は `GridViewPager` *自動的に* スクリーンシェイプを検出し、以下で説明するコントロールの子として追加することはできないことに注意してください。

### <a name="watchviewstub"></a>WatchViewStub

画面の種類を検出し、種類ごとに異なるレイアウトを表示する方法については、 [WatchViewStub](/samples/xamarin/monodroid-samples/wear-watchviewstub) サンプルを参照してください。

メインレイアウトファイルには、 `android.support.wearable.view.WatchViewStub` 属性と属性を使用して、四角形と丸い画面のさまざまなレイアウトを参照するが含まれてい `app:rectLayout` `app:roundLayout` ます。

```xml
<android.support.wearable.view.WatchViewStub
    xmlns:app="http://schemas.android.com/apk/res-auto"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  android:id="@+id/stub"
  app:rectLayout="@layout/rect_layout"
  app:roundLayout="@layout/round_layout" />
```

ソリューションには、実行時に選択されるスタイルごとに異なるレイアウトが含まれています。

![[リソース/レイアウト] の下に表示されるファイル](screen-sizes-images/solution.png)

### <a name="boxinsetlayout"></a>BoxInsetLayout

画面の種類ごとに異なるレイアウトを作成するのではなく、四角形または丸い画面に適応する1つのビューを作成することもできます。

この [Google の例](https://developer.android.com/training/wearables/ui/layouts.html#same-layout) では、を使用して、 `BoxInsetLayout` 四角形と丸い両方の画面で同じレイアウトを使用する方法を示します。

## <a name="wear-ui-designer"></a>摩耗 UI デザイナー

Xamarin Android Designer は、四角形と丸い両方の画面をサポートしています。

![Xamarin Android Designer の [Android の磨耗の四角形] 画面を選択する](screen-sizes-images/design-screen-type.png)

四角形のスタイルのデザイン画面を次に示します。

![四角形スタイルのデザインサーフェイス](screen-sizes-images/design-rect.png) 

次のように、ラウンドスタイルのデザイン画面が表示されます。

![デザインサーフェイスのラウンドスタイル](screen-sizes-images/design-round.png)

## <a name="wear-simulator"></a>磨耗シミュレーター

**Google Emulator Manager**には、両方の画面の種類のデバイス定義が含まれています。 アプリケーションをテストするために、四角形と丸いエミュレーターを作成できます。

![Google Emulator Manager に表示される磨耗デバイスの定義](screen-sizes-images/emulator-devices.png)

エミュレーターは、次のように四角形の画面に表示されます。

![四角形の画面のエミュレーターレンダリング](screen-sizes-images/recipe-2.png) 

次のような画面が表示されます。

![エミュレーターレンダリングのラウンドスクリーン](screen-sizes-images/recipe-2-round.png)

## <a name="video"></a>ビデオ

[Developers.google.com](https://www.youtube.com/channel/UC_x5XG1OV2P6uZZ5FSM9Ttw)から[の Android 磨耗用の全画面アプリ](https://www.youtube.com/watch?v=naf_WbtFAlY)。