---
title: RatingBar
description: Android の activity を RatingBar ウィジェットを追加する方法。
ms.prod: xamarin
ms.assetid: d7a1f9bb-926d-4f93-9e8e-0fa933e330e7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: 97d2a126be70e210d2e8f4ebf4d7a25ff8777a02
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "60945454"
---
# <a name="ratingbar"></a>RatingBar

RatingBar は、1 ~ 5 つ星の評価を表示する UI ウィジェットです。 ユーザーはこのセクションでは、星をタップして、評価を選択することがあります、ユーザーが、評価に使用できるウィジェットを作成、 [ `RatingBar` ](https://developer.xamarin.com/api/type/Android.Widget.RatingBar/)ウィジェット。

![RatingBar の例](ratingbar-images/01-ratingbar.png)


## <a name="creating-a-ratingbar"></a>RatingBar を作成します。

1. 開く、 **Resource/layout/Main.axml**追加ファイルを開き、 [`RatingBar`](https://developer.xamarin.com/api/type/Android.Widget.RatingBar/)
   要素 (内部、 [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/))。

    ```xml
    <RatingBar android:id="@+id/ratingbar"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:numStars="5"
            android:stepSize="1.0"/>
    ```
   `android:numStars`数の星評価バーに表示する属性を定義します。 `android:stepSize`属性は、各星の粒度を定義します (たとえばの値`0.5`星半分の区分を許可するよう)。

2. 末尾に次のコードを追加を行うには何か新しい評価が設定されている場合、 [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle)
   方法:

    ```csharp
    RatingBar ratingbar = FindViewById<RatingBar>(Resource.Id.ratingbar);

    ratingbar.RatingBarChange += (o, e) => {
            Toast.MakeText(this, "New Rating: " + ratingbar.Rating.ToString (), ToastLength.Short).Show ();
    };
    ```

    これをキャプチャ、 [ `RatingBar` ](https://developer.xamarin.com/api/type/Android.Widget.RatingBar/)をレイアウトからウィジェット[ `FindViewById` ](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/)し、イベントのメソッドを設定し、ユーザー設定の評価時に実行するアクションを定義します。 この場合は、シンプルな[ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/)メッセージが新しい評価を表示します。

3.  アプリケーションを実行します。

