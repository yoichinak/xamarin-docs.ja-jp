---
title: GridViewPager
ms.prod: xamarin
ms.assetid: A1CDD5F0-049B-4DFA-A268-8A875D26A675
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/02/2018
ms.openlocfilehash: ff054b1bd9607dd0dade874453a6ddf99ea4fd77
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70758209"
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

を作成する[`GridPagerAdapter`](https://developer.android.com/reference/android/support/wearable/view/GridPagerAdapter.html)
(またはのようなサブクラスです。[`FragmentGridPagerAdapter`](https://developer.android.com/reference/android/support/wearable/view/FragmentGridPagerAdapter.html)
ユーザーが移動したときに表示するビューを指定します。

[サンプルアダプター](https://github.com/xamarin/monodroid-samples/blob/master/wear/GridViewPager/GridViewPager/SimpleGridPagerAdapter.cs)は、、 `RowCount` `GetColumnCount` `GetBackground`、、およびのオーバーライドなど、必要なメソッドを実装する方法を示しています。`GetFragment`

次に示すように、アダプターを接続します。

```csharp
pager.Adapter = new SimpleGridPagerAdapter (this, FragmentManager);
```

## <a name="related-links"></a>関連リンク

- [Google の2D ピッカードキュメント](https://developer.android.com/training/wearables/ui/2d-picker.html)
- [android. ウェアラブルドキュメント](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
- [GridViewPager (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-gridviewpager)
