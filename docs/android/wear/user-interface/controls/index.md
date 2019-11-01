---
title: Android の磨耗の制御
ms.prod: xamarin
ms.assetid: 5B62A5F8-5E55-4B3C-BFC4-E21CDB27C08B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 54ef09af1da484305eecc2107d0245426d11a646
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030305"
---
# <a name="android-wear-controls"></a>Android の磨耗の制御

Android の磨耗アプリでは、通常の Android アプリに既に使用されている多くのコントロールを使用できます。これには、`Button`、`TextView`、イメージの引き出しが可能です。 `ScrollView`、`LinearLayout`、`RelativateLayout` を含むレイアウトコントロールを使用することもできます。

このページでは、[ウェアラブルサポート](https://www.nuget.org/packages/Xamarin.Android.Wear/)NuGet パッケージを使用して、Xamarin プロジェクトで使用可能な[ウェアラブル UI ライブラリ](https://developer.android.com/training/wearables/apps/layouts.html#UiLibrary)から、Android の磨耗に固有のコントロールにリンクします。 これらのコントロールには、次のものが含まれます。

- **GridViewPager** &ndash; 2 次元のナビゲーションインターフェイスを作成します。このインターフェイスでは、ユーザーは次にスクロールして選択を行います (詳細については、 [GridViewPager](~/android/wear/user-interface/controls/gridviewpager.md)を参照してください)。

    ![GridViewPager のスクリーンショットの例](images/gridviewpager.png)

磨耗アプリのその他の重要な制御は次のとおりです。

- `BoxInsetLayout` (「[画面サイズの操作](~/android/wear/screen-sizes.md)」を参照)、

- `WatchViewStub` (「[画面サイズの操作](~/android/wear/screen-sizes.md)」を参照)、

- `CardFrame` (「 [Android カードを作成する](https://developer.android.com/training/wearables/ui/cards.html)」を参照してください)。

- `CardScrollView` (「 [Android カードを作成する](https://developer.android.com/training/wearables/ui/cards.html)」を参照してください)。

- `WearableListView` (「 [Android 作成リスト](https://developer.android.com/training/wearables/ui/lists.html)」を参照してください)。

## <a name="related-links"></a>関連リンク

- [Android. ウェアラブルドキュメント](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
