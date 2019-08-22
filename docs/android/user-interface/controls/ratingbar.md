---
title: Xamarin Android RatingBar
description: Android アクティビティに RatingBar ウィジェットを追加する方法。
ms.prod: xamarin
ms.assetid: d7a1f9bb-926d-4f93-9e8e-0fa933e330e7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: de63a0f3f6564671a50594c66b55ed095329c95c
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/21/2019
ms.locfileid: "69887635"
---
# <a name="xamarinandroid-ratingbar"></a>Xamarin Android RatingBar

RatingBar は、1 ~ 5 個の星の評価を表示する UI ウィジェットです。 ユーザーは、このセクションの星にタップで評価を選択できます。ウィジェットを作成し、ユーザーが[`RatingBar`](xref:Android.Widget.RatingBar)ウィジェットで評価を提供できるようにします。

![RatingBar の例](ratingbar-images/01-ratingbar.png)


## <a name="creating-a-ratingbar"></a>RatingBar の作成

1. **Resource/layout/Main. axml**ファイルを開き、[`RatingBar`](xref:Android.Widget.RatingBar)
   要素 (内[`LinearLayout`](xref:Android.Widget.LinearLayout)):

   ```xml
   <RatingBar android:id="@+id/ratingbar"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:numStars="5"
            android:stepSize="1.0"/>
   ```

   属性`android:numStars`は、評価バーに表示する星の数を定義します。 属性`android:stepSize`では、各星の粒度を定義します (たとえば`0.5` 、の値は半星評価を許可します)。

2. 新しい評価を設定したときに何かを行うには、の末尾に次のコードを追加します。[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
   b

    ```csharp
    RatingBar ratingbar = FindViewById<RatingBar>(Resource.Id.ratingbar);

    ratingbar.RatingBarChange += (o, e) => {
            Toast.MakeText(this, "New Rating: " + ratingbar.Rating.ToString (), ToastLength.Short).Show ();
    };
    ```

    これにより[`RatingBar`](xref:Android.Widget.RatingBar) 、を使用[`FindViewById`](xref:Android.App.Activity.FindViewById*)してレイアウトからウィジェットをキャプチャし、次にイベントメソッドを設定して、ユーザーが評価を設定したときに実行するアクションを定義します。 この場合、単純な[`Toast`](xref:Android.Widget.Toast)メッセージに新しい評価が表示されます。

3. アプリケーションを実行します。

