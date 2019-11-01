---
title: GridViewPager
ms.prod: xamarin
ms.assetid: A1CDD5F0-049B-4DFA-A268-8A875D26A675
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/02/2018
ms.openlocfilehash: 77c5eb65601e5bfdac2cbaad2e196a8ae2813981
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030322"
---
# <a name="gridviewpager"></a>GridViewPager

[GridViewPager](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-gridviewpager)サンプルでは、Android 劣化の2d ピッカーナビゲーションパターンを実装する方法を示します。

![正方形ディスプレイの GridViewPager のスクリーンショットの例](gridviewpager-images/gridviewpager.png)

まず、 [Xamarin Android 磨耗サポート](https://www.nuget.org/packages/Xamarin.Android.Wear/)NuGet パッケージをプロジェクトに追加します。

レイアウト XML は次のようになります。

```xml
<android.support.wearable.view.GridViewPager xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/pager"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:keepScreenOn="true" />
```

[`GridPagerAdapter`](https://developer.android.com/reference/android/support/wearable/view/GridPagerAdapter.html)を作成する
(または[`FragmentGridPagerAdapter`](https://developer.android.com/reference/android/support/wearable/view/FragmentGridPagerAdapter.html)のようなサブクラス
ユーザーが移動したときに表示するビューを指定します。

[サンプルアダプター](https://github.com/xamarin/monodroid-samples/blob/master/wear/GridViewPager/GridViewPager/SimpleGridPagerAdapter.cs)は、`RowCount`、`GetColumnCount`、`GetBackground`、`GetFragment` およびのオーバーライドなど、必要なメソッドを実装する方法を示しています。

次に示すように、アダプターを接続します。

```csharp
pager.Adapter = new SimpleGridPagerAdapter (this, FragmentManager);
```

## <a name="related-links"></a>関連リンク

- [Google の2D ピッカードキュメント](https://developer.android.com/training/wearables/ui/2d-picker.html)
- [android. ウェアラブルドキュメント](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
- [GridViewPager (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-gridviewpager)
