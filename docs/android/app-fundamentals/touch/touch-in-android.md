---
title: Android でのタッチ
ms.prod: xamarin
ms.assetid: 405A1FA0-4EFA-4AEB-B672-F36307B9CF16
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 274c441e0507f100697fc153a9f748de1bce4cf3
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69526070"
---
# <a name="touch-in-android"></a>Android でのタッチ

IOS と同様に、Android では、ユーザーの画面&ndash; `Android.View.MotionEvent`とオブジェクトの物理的な相互作用に関するデータを保持するオブジェクトが作成されます。 このオブジェクトには、実行されたアクション、タッチが行われた場所、適用された負荷などのデータが保持されます。オブジェクト`MotionEvent`は、への移動を次の値に分割します。

- 初期タッチ、画面上でのタッチの移動、タッチの終了など、モーションの種類を記述するアクションコード。

- タッチが行われている場所、タッチが発生`MotionEvent`したとき、使用された圧力など、の移動プロパティおよびその他の移動プロパティの位置を表す軸値のセット。
   軸の値はデバイスによって異なる場合があるため、前の一覧にはすべての軸の値が記述されていません。


オブジェクト`MotionEvent`は、アプリケーション内の適切なメソッドに渡されます。 Xamarin Android アプリケーションがタッチイベントに応答するには、次の3つの方法があります。

- *イベントハンドラーをに`View.Touch`割り当てる*- `Android.Views.View`クラスには、 `EventHandler<View.TouchEventArgs>`ハンドラーを割り当てることができるアプリケーションがあります。 これは、一般的な .NET の動作です。

- このインターフェイスの*実装`View.IOnTouchListener`* インスタンスは、ビューを使用してビューオブジェクトに割り当てることができます。 `SetOnListener`b.これは、 `View.Touch`イベントハンドラーをイベントに割り当てることと機能的には同じです。 いくつかの一般的なロジックや共有ロジックがあり、それらのビューを操作するときにさまざまなビューが必要になることがある場合は、クラスを作成し、各ビューに独自のイベントハンドラーを割り当てるよりも、このメソッドを実装する方が効率的です。

- *上書き`View.OnTouchEvent`*  -Android サブクラス`Android.Views.View`のすべてのビュー。 ビューにタッチすると、Android はを呼び出し`OnTouchEvent` 、パラメーターとし`MotionEvent`てオブジェクトを渡します。


> [!NOTE]
> すべての Android デバイスがタッチスクリーンをサポートしているわけではありません。 

次のタグをマニフェストファイルに追加すると、Google Play によって、タッチが有効になっているデバイスにのみアプリが表示されるようになります。

```xml
<uses-configuration android:reqTouchScreen="finger" />
```

## <a name="gestures"></a>ジェスチャ

ジェスチャは、タッチスクリーン上のハンド描画された図形です。 ジェスチャは、1つまたは複数のストロークを持つことができます。各ストロークは、画面とは異なる接点によって作成されたポイントのシーケンスで構成されます。 Android では、画面上の単純な theory からマルチタッチを含む複雑なジェスチャまで、さまざまな種類のジェスチャをサポートできます。

Android には`Android.Gestures` 、ジェスチャの管理と応答を行うための名前空間が用意されています。 すべてのジェスチャの中核となるのは、と`Android.Gestures.GestureDetector`呼ばれる特殊なクラスです。 名前が示すように、このクラスは、オペレーティングシステムによって`MotionEvents`提供されるジェスチャとイベントをリッスンします。

ジェスチャ検出機能を実装するには、次の`GestureDetector`コードスニペットに示すように`IOnGestureListener`、アクティビティでクラスをインスタンス化し、のインスタンスを提供する必要があります。

```csharp
GestureOverlayView.IOnGestureListener myListener = new MyGestureListener();
_gestureDetector = new GestureDetector(this, myListener);
```

また、アクティビティは OnTouchEvent を実装し、MotionEvent をジェスチャ検出機能に渡す必要があります。 次のコードスニペットは、この例を示しています。

```csharp
public override bool OnTouchEvent(MotionEvent e)
{
    // This method is in an Activity
    return _gestureDetector.OnTouchEvent(e);
}
```

の`GestureDetector`インスタンスは、目的のジェスチャを識別するときに、イベントを発生させるか、によっ`GestureDetector.IOnGestureListener`て提供されるコールバックを使用して、アクティビティまたはアプリケーションに通知します。
このインターフェイスは、さまざまなジェスチャに対して6つのメソッドを提供します。

- *Ondown* -タップが発生したが、解放されていない場合に呼び出されます。

- *OnFling* -theory が発生し、イベントをトリガーした開始および終了のタッチにデータを提供するときに呼び出されます。

- *Onlongpress*は、長い押下が発生したときに呼び出されます。

- *Onscroll* -scroll イベントが発生したときに呼び出されます。

- *Onshowpress*は、ondown が発生し、move または up イベントが実行されていない状態で呼び出されました。

- *OnSingleTapUp* -シングルタップが発生したときに呼び出されます。


多くの場合、アプリケーションはジェスチャのサブセットのみを対象にすることができます。 この場合、アプリケーションは GestureDetector クラスを拡張し、関心のあるイベントに対応するメソッドをオーバーライドする必要があります。

## <a name="custom-gestures"></a>カスタムジェスチャ

ジェスチャは、ユーザーがアプリケーションと対話するための優れた方法です。 これまでに見てきた Api は、単純なジェスチャには十分ですが、より複雑なジェスチャに対しては、少し煩雑を証明することがあります。 Android では、より複雑なジェスチャに対応するために、カスタムジェスチャに関連する負荷の一部を簡単にするために、別の API セットが Android のジェスチャ名前空間に用意されています。

### <a name="creating-custom-gestures"></a>カスタムジェスチャの作成

Android 1.6 以降、Android SDK には、ジェスチャビルダーと呼ばれるエミュレーターにプレインストールされたアプリケーションが付属しています。 このアプリケーションを使用すると、開発者は、アプリケーションに埋め込むことができる定義済みのジェスチャを作成できます。 次のスクリーンショットは、ジェスチャビルダーの例を示しています。

[![ジェスチャの例を含むジェスチャビルダーのスクリーンショット](touch-in-android-images/image11.png)](touch-in-android-images/image11.png#lightbox)

ジェスチャツールと呼ばれるこのアプリケーションの改良されたバージョンは Google Play 見つかります。 ジェスチャツールは、作成後にジェスチャをテストできる点を除いて、ジェスチャビルダーとよく似ています。 次のスクリーンショットは、ジェスチャビルダーを示しています。

[![ジェスチャの例を含むジェスチャツールのスクリーンショット](touch-in-android-images/image12.png)](touch-in-android-images/image12.png#lightbox)

ジェスチャツールは、ジェスチャを作成時にテストし、Google Play で簡単に使用できるようにすることで、カスタムジェスチャを作成する場合に少し便利です。

ジェスチャツールを使用すると、画面上に描画し、名前を割り当てることによってジェスチャを作成できます。 ジェスチャが作成されると、デバイスの SD カードのバイナリファイルに保存されます。 このファイルは、デバイスから取得し、アプリケーションと共に、次のフォルダーにパッケージ化してパッケージ化する必要があります。 このファイルは、Android Debug Bridge を使用してエミュレーターから取得できます。 次の例では、ファイルを Galaxy の場合はアプリケーションのリソースディレクトリにコピーします。

```shell
$ adb pull /storage/sdcard0/gestures <projectdirectory>/Resources/raw
```

ファイルを取得した後は、アプリケーションと共にパッケージ化する必要があります。 このジェスチャファイルを使用する最も簡単な方法は、次のスニペットに示すように、ファイルを GestureLibrary に読み込むことです。

```csharp
GestureLibrary myGestures = GestureLibraries.FromRawResources(this, Resource.Raw.gestures);
if (!myGestures.Load())
{
    // The library didn't load, so close the activity.
    Finish();
}
```

### <a name="using-custom-gestures"></a>カスタムジェスチャの使用

アクティビティ内のカスタムジェスチャを認識するには、そのレイアウトに GestureOverlay オブジェクトが追加されている必要があります。 次のコードスニペットは、プログラムを利用して GestureOverlayView をアクティビティに追加する方法を示しています。

```csharp
GestureOverlayView gestureOverlayView = new GestureOverlayView(this);
gestureOverlayView.AddOnGesturePerformedListener(this);
SetContentView(gestureOverlayView);
```

次の XML スニペットは、GestureOverlayView を宣言によって追加する方法を示しています。

```xml
<android.gesture.GestureOverlayView
    android:id="@+id/gestures"
    android:layout_width="match_parent "
    android:layout_height="match_parent" />
```

に`GestureOverlayView`は、ジェスチャの描画プロセス中に発生するいくつかのイベントがあります。 最も興味深いイベントは`GesturePerformed`です。 このイベントは、ユーザーがジェスチャの描画を完了したときに発生します。

このイベントが発生すると、アクティビティは、 `GestureLibrary`ユーザーがジェスチャツールによって作成されたジェスチャの1つを使用して、ユーザーとのジェスチャを試行するように要求します。 `GestureLibrary`は、予測オブジェクトの一覧を返します。

各予測オブジェクトは、 `GestureLibrary`内のいずれかのジェスチャのスコアと名前を保持します。 スコアが高いほど、予測に示されているジェスチャが、ユーザーによって描画されたジェスチャと一致する可能性が高くなります。
一般に、1.0 より低いスコアは、一致していないと見なされます。

次のコードは、ジェスチャを照合する例を示しています。

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

これにより、Xamarin Android アプリケーションでタッチとジェスチャを使用する方法を理解できるようになります。 チュートリアルに進んで、実用的なサンプルアプリケーションのすべての概念を確認しましょう。



## <a name="related-links"></a>関連リンク

- [Android タッチスタート (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-touch-start)
- [Android のタッチ最終 (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-touch-final)
