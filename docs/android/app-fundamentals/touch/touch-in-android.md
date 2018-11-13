---
title: Android でのタッチ
ms.prod: xamarin
ms.assetid: 405A1FA0-4EFA-4AEB-B672-F36307B9CF16
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: a25a1c3be8c952536c0ef40b7f7c4a64f5748516
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/11/2018
ms.locfileid: "51527236"
---
# <a name="touch-in-android"></a>Android でのタッチ

はるかなど、iOS、Android 作成、ユーザーの物理的な画面操作に関するデータを保持するオブジェクト&ndash;、`Android.View.MotionEvent`オブジェクト。 このオブジェクトがどのようなアクションの実行などのデータを保持して、タッチが、配置、圧力の量が適用されたなど。A`MotionEvent`オブジェクトが次の値への移動を分割します。

-  最初のタッチ画面、または、タッチの終わりを越えて移動タッチなどの動きの型を記述するアクション コード。

-  一連の軸の値の位置を示す、`MotionEvent`およびその他の移動のプロパティなど、タッチが行われている、ときに、タッチ行われたか、および圧力の量が使用されました。
   軸の値は、上記の一覧ではすべての軸の値については説明しませんのでデバイスによっては、異なる場合があります。


`MotionEvent`オブジェクトは、アプリケーションで適切なメソッドに渡されます。 タッチ イベントに応答する Xamarin.Android アプリケーションの 3 つの方法はあります。

-  *イベント ハンドラーを割り当てる`View.Touch`*  -`Android.Views.View`クラスには、`EventHandler<View.TouchEventArgs>`アプリケーションは、ハンドラーを割り当てることができます。 これは、.NET の一般的な動作です。

-  *実装する`View.IOnTouchListener`*  -このインターフェイスのインスタンスは、ビューを使用してビュー オブジェクトに割り当てることができます。 `SetOnListener` メソッド。これは、機能的にイベント ハンドラーを割り当てるには、`View.Touch`イベント。 さまざまな見方が接している必要があります共有ロジックがある場合は、クラスを作成しに割り当てる各ビュー、独自のイベント ハンドラーよりも、このメソッドを実装する方が効率的があります。

-  *オーバーライド`View.OnTouchEvent`*  -Android サブクラスのすべてのビュー`Android.Views.View`します。 Android ビューが操作されたときに呼び出す、`OnTouchEvent`を渡して、`MotionEvent`オブジェクトをパラメーターとして。


> [!NOTE]
> すべての Android デバイスでは、タッチ スクリーンをサポートします。 

マニフェスト ファイルに次のタグを追加すると、Google Play のみを表示するタッチを有効になっているはこれらのデバイスにアプリ。

```xml
<uses-configuration android:reqTouchScreen="finger" />
```

## <a name="gestures"></a>ジェスチャ

ジェスチャとは、タッチ スクリーン上の手書きの図形です。 ジェスチャでは、画面との接続のさまざまなポイントで作成されたポイントのシーケンスから成る各ストロークを 1 つ以上のストロークを持つことができます。 Android では、マルチタッチに関連する複雑なジェスチャに画面上で単純な念からジェスチャのさまざまな種類をサポートできます。

Android に用意されて、`Android.Gestures`名前空間の管理とジェスチャに応答を具体的には。 At と呼ばれる特殊なクラスは、すべてのジェスチャの中心となる`Android.Gestures.GestureDetector`します。 このクラスがジェスチャとに基づいてイベントをリッスンする名前が示すように、`MotionEvents`オペレーティング システムによって提供されます。

ジェスチャの検出機能を実装するアクティビティをインスタンス化する必要があります、`GestureDetector`クラスのインスタンスを指定し、`IOnGestureListener`次のコード スニペットに示しますように。

```csharp
GestureOverlayView.IOnGestureListener myListener = new MyGestureListener();
_gestureDetector = new GestureDetector(this, myListener);
```

アクティビティは必要がありますも、OnTouchEvent を実装し、ジェスチャの検出機能を MotionEvent を渡します。 次のコード スニペットでは、この例を示します。

```csharp
public override bool OnTouchEvent(MotionEvent e)
{
    // This method is in an Activity
    return _gestureDetector.OnTouchEvent(e);
}
```

インスタンス`GestureDetector`ジェスチャを識別する関心のあることは、アクティビティまたはアプリケーションに通知イベントを発生させるか、によって提供されるコールバックを通じて`GestureDetector.IOnGestureListener`します。
このインターフェイスは、さまざまなジェスチャの 6 つのメソッドを提供します。

-  *OnDown* -タップが発生しますが、解放されていないときに呼び出されます。

-  *OnFling* -念を発生し、開始と終了のタッチ イベントをトリガーしたに関するデータを提供するときに呼び出されます。

-  *OnLongPress* -長押しが発生したときに呼び出されます。

-  *OnScroll* -スクロール イベントが発生したときに呼び出されます。

-  *OnShowPress* - という、OnDown が発生した後、移動、またはイベントが実行されていません。

-  *OnSingleTapUp* -1 回のタップが発生したときに呼び出されます。


多くの場合に、ジェスチャのサブセットでアプリケーションを利用することがありますのみです。 この場合、アプリケーションは GestureDetector.SimpleOnGestureListener クラスを拡張しで関心のあるイベントに対応するメソッドをオーバーライドする必要があります。

## <a name="custom-gestures"></a>カスタムのジェスチャ

ジェスチャとは、ユーザーがアプリケーションと対話するための優れた方法です。 これまで行ってきた Api は、単純なジェスチャで十分ですより複雑なジェスチャを少し面倒であるかもしれません。 複雑なジェスチャのヘルプ、Android には、カスタム ジェスチャに関連付けられている負荷の一部を容易にする Android.Gestures 名前空間内の別の API のセットが用意されています。

### <a name="creating-custom-gestures"></a>カスタム ジェスチャを作成します。

Android の 1.6 以降、Android SDK エミュレーターのジェスチャ Builder というに事前インストールされているアプリケーションが付属します。 このアプリケーションは、開発者はアプリケーションに埋め込むことができる定義済みのジェスチャを作成できます。 次のスクリーン ショットでは、ジェスチャのビルダーの例を示します。

[![例のジェスチャでジェスチャ ビルダーのスクリーン ショット](touch-in-android-images/image11.png)](touch-in-android-images/image11.png#lightbox)

ジェスチャのツールと呼ばれるこのアプリケーションの改良版には、Google Play を確認できます。 ジェスチャ ツールには、作成した後に、ジェスチャをテストできますが、ジェスチャ ビルダーとよく似て。 この次のスクリーン ショットは、ジェスチャのビルダーを示しています。

[![例のジェスチャでジェスチャ ツールのスクリーン ショット](touch-in-android-images/image12.png)](touch-in-android-images/image12.png#lightbox)

ジェスチャのツールは少しを作成すると、テストするジェスチャが許可されているカスタム ジェスチャを作成するために便利で、Google Play で簡単に使用します。

ジェスチャのツールは、ジェスチャに作成するには、画面上に描画し、名前の割り当てを使用できます。 ジェスチャが作成された後、デバイスの SD カードにバイナリ ファイルに保存されます。 このファイルは、デバイスから取得され、フォルダー/Resources/raw でアプリケーションとパッケージ化する必要があります。 このファイルは、Android Debug Bridge を使用してエミュレーターから取得できます。 次の例では、Galaxy Nexus からファイルをアプリケーションのリソース ディレクトリにコピーを示します。

```shell
$ adb pull /storage/sdcard0/gestures <projectdirectory>/Resources/raw
```

ファイルを取得すると、ディレクトリ/Resources 内でアプリケーションをパッケージ化/生がなければなりません。 このジェスチャ ファイルを使用する最も簡単な方法は、次のスニペットに示すように、GestureLibrary、ファイルを読み込むには。

```csharp
GestureLibary myGestures = GestureLibraries.FromRawResources(this, Resource.Raw.gestures);
if (!myGestures.Load())
{
    // The library didn't load, so close the activity.
    Finish();
}
```

### <a name="using-custom-gestures"></a>カスタム ジェスチャを使用します。

アクティビティ内でカスタム ジェスチャを認識するようにそのレイアウトに追加された Android.Gesture.GestureOverlay オブジェクトが必要です。 次のコード スニペットでは、プログラムによって、GestureOverlayView をアクティビティに追加する方法を示します。

```csharp
GestureOverlayView gestureOverlayView = new GestureOverlayView(this);
gestureOverlayView.AddOnGesturePerformedListener(this);
SetContentView(gestureOverlayView);
```

次の XML スニペットは、宣言によって、GestureOverlayView を追加する方法を示します。

```xml
<android.gesture.GestureOverlayView
    android:id="@+id/gestures"
    android:layout_width="match_parent "
    android:layout_height="match_parent" />
```

`GestureOverlayView`は、ジェスチャの描画の処理中に発生するいくつかのイベントがあります。 最も注目すべきイベントが`GesturePerformed`します。 ユーザーに、ジェスチャの描画が完了したときに、このイベントが発生します。

このイベントが発生したときに、アクティビティの確認、`GestureLibrary`を試して、ジェスチャ ツールによって、ジェスチャ、ジェスチャのいずれかのユーザーの作成と一致します。 `GestureLibrary` 予測のオブジェクトの一覧が返されます。

予測の各オブジェクトは、スコアと内のジェスチャのいずれかの名前を保持、`GestureLibrary`します。 高いほど、スコアが高く予測にという名前のジェスチャはユーザーによって描かジェスチャと一致します。
一般的に言えば、1.0 より低いスコアは、不適切な一致と見なされます。

次のコードでは、ジェスチャの一致の例を示します。

```csharp
private void GestureOverlayViewOnGesturePerformed(object sender, GestureOverlayView.GesturePerformedEventArgs gesturePerformedEventArgs)
{
    // In this example _gestureLibrary was instantiated in OnCreate
    IEnumerable<Prediction> predictions = from p in _gestureLibrary.Recognize(gesturePerformedEventArgs.Gesture)
    orderby p.Score descending
    where p.Score > 1.0
    select p;
    Prediction prediction = predictions.FirstOrDefault();

    if (prediction == null)
    {
        Log.Debug(GetType().FullName, "Nothing matched the user's gesture.");
        return;
    }

    Toast.MakeText(this, prediction.Name, ToastLength.Short).Show();
}
```

これを行うには、Xamarin.Android アプリケーションでタッチとジェスチャを使用する方法を理解が必要です。 お知らせようになりましたチュートリアルに進むし、動作するサンプル アプリケーションの概念のすべてを参照してください。



## <a name="related-links"></a>関連リンク

- [Android タッチ (サンプル) を開始](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android タッチ最後 (サンプル)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
