---
title: Xamarin Android RatingBar
description: Android アクティビティに RatingBar ウィジェットを追加する方法。
ms.prod: xamarin
ms.assetid: d7a1f9bb-926d-4f93-9e8e-0fa933e330e7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/29/2018
ms.openlocfilehash: 529fecb4e24e83ef7b783815843e132347d99262
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029153"
---
# <a name="xamarinandroid-ratingbar"></a>Xamarin Android RatingBar

RatingBar は、1 ~ 5 個の星の評価を表示する UI ウィジェットです。 ユーザーは、このセクションの星でタップによって評価を選択できます。ユーザーが[`RatingBar`](xref:Android.Widget.RatingBar)ウィジェットで評価を提供できるようにするウィジェットを作成します。

![RatingBar の例](ratingbar-images/01-ratingbar.png)

## <a name="creating-a-ratingbar"></a>RatingBar の作成

1. **リソース/レイアウト/メインの axml**ファイルを開き、 [`RatingBar`](xref:Android.Widget.RatingBar)を追加します。
   要素 ( [`LinearLayout`](xref:Android.Widget.LinearLayout)内):

   ```xml
   <RatingBar android:id="@+id/ratingbar"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:numStars="5"
            android:stepSize="1.0"/>
   ```

   `android:numStars` 属性は、評価バーに表示する星の数を定義します。 `android:stepSize` 属性は、各星の粒度を定義します (たとえば、`0.5` の値によって星の評価が許可されます)。

2. 新しい評価を設定したときに何かを行うには、の末尾に次のコードを追加し[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
   b

    ```csharp
    RatingBar ratingbar = FindViewById<RatingBar>(Resource.Id.ratingbar);

    ratingbar.RatingBarChange += (o, e) => {
            Toast.MakeText(this, "New Rating: " + ratingbar.Rating.ToString (), ToastLength.Short).Show ();
    };
    ```

    これにより、 [`FindViewById`](xref:Android.App.Activity.FindViewById*)を使用してレイアウトから[`RatingBar`](xref:Android.Widget.RatingBar)ウィジェットがキャプチャされ、次にイベントメソッドを設定して、ユーザーが評価を設定したときに実行するアクションを定義します。 この場合、単純な[`Toast`](xref:Android.Widget.Toast)メッセージに新しい評価が表示されます。

3. アプリケーションを実行します。
