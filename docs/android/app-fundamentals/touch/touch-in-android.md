---
title: "Android でタッチします。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 405A1FA0-4EFA-4AEB-B672-F36307B9CF16
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 1d9cf345aa971c40f4132cc7970ed1244640da14
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="touch-in-android"></a>Android でタッチします。

IOS のように Android オブジェクトを作成、画面でのユーザーの物理的な操作に関するデータを保持する&ndash;、`Android.View.MotionEvent`オブジェクト。 このオブジェクトは、どのようなアクションが実行されるなどのデータを格納、タッチかかりました、配置、どの程度の負荷が適用されたなどです。A`MotionEvent`への移動は次の値を分割するオブジェクト。

-  初期のタッチ画面、またはタッチ終了を越えて移動タッチなどの動作の種類を表す操作コード。

-  一連の軸の値の位置を記述する、`MotionEvent`とタッチが行われて、タッチの発生したときにどの程度の負荷が使用されたなどその他の移動のプロパティです。
   上記の一覧ではすべての軸の値については説明しませんので、軸の値がデバイスによって異なる場合があります。


`MotionEvent`オブジェクトは、アプリケーションで適切なメソッドに渡されます。 タッチ イベントに応答する Xamarin.Android アプリケーション用の 3 つの方法があります。

-  *イベント ハンドラーを割り当てます`View.Touch`*  -`Android.Views.View`クラスには、`EventHandler<View.TouchEventArgs>`どのアプリケーションにハンドラーを割り当てることができます。 これは、.NET の一般的な動作です。

-  *実装する`View.IOnTouchListener`*  -このインターフェイスのインスタンスは、ビューを使用して、ビュー オブジェクトに割り当てることができます。 `SetOnListener` メソッド。これは、機能的にイベント ハンドラーを割り当てるには、`View.Touch`イベント。 接している場合に、さまざまなビューが必要ないくつかの一般的なや共有ロジックがある場合は、クラスを作成しよりに割り当てる各ビュー独自のイベント ハンドラーには、このメソッドを実装する方が効率的があります。

-  *オーバーライド`View.OnTouchEvent`*  -Android サブクラス内のすべてのビュー`Android.Views.View`です。 ビューを変更すると、Android を呼び出して、`OnTouchEvent`させ、`MotionEvent`オブジェクトをパラメーターとして。


> [!NOTE]
> すべての Android デバイスでは、タッチ スクリーンをサポートします。 

マニフェスト ファイルに次のタグを追加すると、Google Play のみ表示するタッチを有効になっているはこれらのデバイスにアプリ。

```xml
<uses-configuration android:reqTouchScreen="finger" />
```

## <a name="gestures"></a>ジェスチャ

ジェスチャは、タッチ スクリーンで手書きのシェイプです。 ジェスチャでは、画面との接続のさまざまなポイントが作成されたポイントのシーケンスで構成される各ストローク、1 つまたは複数のストロークを持つことができます。 Android では、さまざまな種類のジェスチャ、マルチタッチを含む複雑なジェスチャを画面全体での単純な fling からをサポートできます。

Android の提供、`Android.Gestures`名前空間の管理とジェスチャに応答を具体的にします。 At と呼ばれる特殊なクラスは、すべてのジェスチャの中心となる`Android.Gestures.GestureDetector`です。 名前が示すように、このクラスは、リッスン ジェスチャ、およびイベントに基づく`MotionEvents`オペレーティング システムで提供されます。

ジェスチャ探知機を実装するアクティビティをインスタンス化する必要があります、`GestureDetector`クラスし、インスタンスを提供`IOnGestureListener`次のコード スニペットで示すようにします。

```csharp
GestureOverlayView.IOnGestureListener myListener = new MyGestureListener();
_gestureDetector = new GestureDetector(this, myListener);
```

アクティビティも、OnTouchEvent を実装し、MotionEvent をジェスチャ検出します。 次のコード スニペットは、この例を示しています。

```csharp
public override bool OnTouchEvent(MotionEvent e)
{
    // This method is in an Activity
    return _gestureDetector.OnTouchEvent(e);
}
```

インスタンス`GestureDetector`ジェスチャを識別、目的には、アクティビティまたはアプリケーションに通知イベントを発生させるか、によって提供される、コールバックを通じて`GestureDetector.IOnGestureListener`です。
このインターフェイスは、さまざまなジェスチャの 6 つのメソッドを提供します。

-  *OnDown* -、タップが発生しますが、解放されていないときに呼び出されます。

-  *OnFling* -、fling が発生し、イベントを発生させた開始と終了のタッチでデータを提供するときに呼び出されます。

-  *OnLongPress* -長いキーを押してが発生したときに呼び出されます。

-  *OnScroll* -スクロール イベントが発生したときに呼び出されます。

-  *OnShowPress* -、OnDown が発生した後に、移動を呼び出されるか、イベントが実行されていません。

-  *OnSingleTapUp* -1 回のタップが発生したときに呼び出されます。


多くの場合アプリケーションがだけジェスチャのサブセットです。 ここでは、アプリケーションが GestureDetector.SimpleOnGestureListener クラスを拡張し、イベントに対応するメソッドをオーバーライドする必要がありますで関心のあること。

## <a name="custom-gestures"></a>カスタム ジェスチャ

ジェスチャは、ユーザーがアプリケーションと対話するための優れた方法です。 これまでにこれまで見てきた Api がのシンプルなジェスチャは、十分に機能より複雑なジェスチャのビットも大きな負担を証明する可能性があります。 複雑なジェスチャのヘルプ、Android には、別のカスタム ジェスチャに関連付けられている負荷の一部を容易にする Android.Gestures 名前空間内の API のセットが用意されています。

### <a name="creating-custom-gestures"></a>カスタム ジェスチャを作成します。

Android の 1.6 以降、Android SDK ジェスチャ ビルダーと呼ばれるエミュレーターに事前インストールされているアプリケーションが付属します。 このアプリケーションには、アプリケーションに埋め込むことができる定義済みのジェスチャを作成する、開発者ができるようにします。 次のスクリーン ショットは、ジェスチャ ビルダーの使用例を示しています。

[![例のジェスチャを使用してジェスチャ ビルダーのスクリーン ショット](touch-in-android-images/image11.png)](touch-in-android-images/image11.png#lightbox)

強化されたバージョンのジェスチャ ツールと呼ばれるアプリケーションは、Google Play で確認できます。 ジェスチャ ツールが、ジェスチャ ビルダーとよく似てそれが作成された後にジェスチャをテストすることができます。 この次のスクリーン ショットは、ジェスチャ ビルダーを示しています。

[![例のジェスチャを使用してジェスチャ ツールのスクリーン ショット](touch-in-android-images/image12.png)](touch-in-android-images/image12.png#lightbox)

ジェスチャ ツールは、少し方が便利ですが作成されるようにテストするジェスチャが許可されているカスタム ジェスチャを作成しは Google Play を介して簡単に使用します。

ジェスチャ ツールにより、画面の描画と名を割り当てることによって、ジェスチャを作成できます。 ジェスチャが作成された後は、デバイスの SD カードにバイナリ ファイルに保存されます。 このファイルは、デバイスから取得され、フォルダー/Resources/raw でアプリケーションをパッケージ化する必要があります。 このファイルは、Android Debug Bridge を使用してエミュレーターから取得できます。 次の例では、Galaxy Nexus からアプリケーションのリソース ディレクトリにコピーして、ファイルを示します。

```shell
$ adb pull /storage/sdcard0/gestures <projectdirectory>/Resources/raw
```

ファイルを取得した後は、ディレクトリ/Resources の内部でアプリケーションをパッケージ化/生があります。 このジェスチャ ファイルを使用する最も簡単な方法は、次のスニペットに示すように、GestureLibrary にファイルを読み込むには。

```csharp
GestureLibary myGestures = GestureLibraries.FromRawResources(this, Resource.Raw.gestures);
if (!myGestures.Load())
{
    // The library didn't load, so close the activity.
    Finish();
}
```

### <a name="using-custom-gestures"></a>カスタム ジェスチャを使用します。

アクティビティ内でカスタム ジェスチャを認識するようにそのレイアウトに追加された Android.Gesture.GestureOverlay オブジェクトが必要です。 次のコード スニペットは、プログラムによって、GestureOverlayView をアクティビティに追加する方法を示しています。

```csharp
GestureOverlayView gestureOverlayView = new GestureOverlayView(this);
gestureOverlayView.AddOnGesturePerformedListener(this);
SetContentView(gestureOverlayView);
```

次の XML スニペットは、宣言によって、GestureOverlayView を追加する方法を示しています。

```xml
<android.gesture.GestureOverlayView
    android:id="@+id/gestures"
    android:layout_width="match_parent "
    android:layout_height="match_parent" />
```

`GestureOverlayView`がいくつかのイベントは、ジェスチャを描くプロセス中に発生します。 最も注目すべきイベントが`GesturePeformed`です。 ユーザーがそのジェスチャの描画を完了したときに、このイベントが発生します。

このイベントが発生したときに、アクティビティの確認、`GestureLibrary`ジェスチャ ツールによってジェスチャのいずれかのユーザーを作成するジェスチャを照合してください。 `GestureLibrary` 予測のオブジェクトの一覧が返されます。

予測の各オブジェクトは、スコアと内のジェスチャのいずれかの名前を保持して、`GestureLibrary`です。 高いほど、スコア、確率が高く、予測では名前付きジェスチャにはユーザーによって描かジェスチャと一致します。
一般に、1.0 より小さいスコアは、不適切な一致と見なされます。

次のコードでは、一致するジェスチャの例を示します。

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

これを完了するのには、タッチとジェスチャ Xamarin.Android アプリケーションで使用する方法を理解が必要です。 お知らせ今すぐチュートリアルへお進みし、すべての作業用サンプル アプリケーションで概念を参照してください。



## <a name="related-links"></a>関連リンク

- [Android タッチ (サンプル) を開始](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android タッチ最終 (サンプル)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
