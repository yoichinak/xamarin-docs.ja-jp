---
title: GridViewPager
ms.prod: xamarin
ms.assetid: A1CDD5F0-049B-4DFA-A268-8A875D26A675
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/02/2018
ms.openlocfilehash: 3a0b1ec9359b1c6067c253b4d04126dbdd726cc5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="gridviewpager"></a>GridViewPager

[GridViewPager](https://developer.xamarin.com/samples/GridViewPager/)サンプルは Android 着用の 2D ピッカー ナビゲーション パターンを実装する方法を示します。

![正方形のディスプレイに GridViewPager の例のスクリーン ショット](gridviewpager-images/gridviewpager.png)

最初に追加、 [Xamarin Android 着用サポート](http://www.nuget.org/packages/Xamarin.Android.Wear/)NuGet パッケージをプロジェクトにします。

XML は次のようにレイアウト。

```xml
<android.support.wearable.view.GridViewPager xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/pager"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:keepScreenOn="true" />
```

作成、 [ `GridPagerAdapter` ](http://developer.android.com/reference/android/support/wearable/view/GridPagerAdapter.html) (またはなどサブクラス[ `FragmentGridPagerAdapter` ](http://developer.android.com/reference/android/support/wearable/view/FragmentGridPagerAdapter.html)をユーザーが移動したときに表示するビューを指定します。

[サンプル アダプター](https://github.com/xamarin/monodroid-samples/blob/master/wear/GridViewPager/GridViewPager/SimpleGridPagerAdapter.cs)の上書きを含め、必要なメソッドを実装する方法を示します`RowCount`、 `GetColumnCount`、 `GetBackground`、および `GetFragment`

アダプターをネットワーク上で示すようにします。

```csharp
pager.Adapter = new SimpleGridPagerAdapter (this, FragmentManager);
```



## <a name="related-links"></a>関連リンク

- [Google の 2D ピッカー ドキュメント](https://developer.android.com/training/wearables/ui/2d-picker.html)
- [android.support.wearable docs](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
- [GridViewPager (sample)](https://developer.xamarin.com/samples/GridViewPager/)
