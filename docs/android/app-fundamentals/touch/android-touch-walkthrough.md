---
title: チュートリアル-Android でのタッチの使用
ms.prod: xamarin
ms.assetid: E281F89B-4142-4BD8-8882-FB65508BF69E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/09/2018
ms.openlocfilehash: 484d567b5243ac282e2773d64a186e056141d8f4
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73024519"
---
# <a name="walkthrough---using-touch-in-android"></a>チュートリアル-Android でのタッチの使用

作業中のアプリケーションで、前のセクションの概念を使用する方法について説明します。 ここでは、4つのアクティビティを含むアプリケーションを作成します。 最初のアクティビティは、さまざまな Api を示すために他のアクティビティを起動するメニューまたはメニューバーです。 次のスクリーンショットは、メインアクティビティを示しています。

[![[タッチ入力] ボタンを使用したスクリーンショットの例](android-touch-walkthrough-images/image14.png)](android-touch-walkthrough-images/image14.png#lightbox)

最初のアクティビティである Touch サンプルは、ビューに接してイベントハンドラーを使用する方法を示しています。 ジェスチャレコグナイザーアクティビティは、イベントをサブ `Android.View.Views` クラス化してイベントを処理し、ピンチジェスチャの処理方法を示す方法を示します。 3番目と最後のアクティビティである**カスタムジェスチャ**は、カスタムジェスチャの使用方法を示します。 より簡単にフォローおよび吸収できるようにするために、このチュートリアルをセクションごとに分割します。各セクションでは、アクティビティの1つに焦点を合わせています。

## <a name="touch-sample-activity"></a>タッチサンプル活動

- TouchWalkthrough プロジェクトを開き **\_開始**します。 **Mainactivity**はすべて "実行" に設定されて &ndash;、アクティビティにタッチ動作を実装することになります。 アプリケーションを実行して **[タッチサンプル]** をクリックすると、次のアクティビティが開始されます。

  [タッチが開始されたアクティビティのスクリーンショット![](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

- アクティビティが開始されたことを確認したので、 **TouchActivity.cs**ファイルを開き、`ImageView`の `Touch` イベントのハンドラーを追加します。

  ```csharp
  _touchMeImageView.Touch += TouchMeImageViewOnTouch;
  ```

- 次に、次のメソッドを**TouchActivity.cs**に追加します。

  ```csharp
  private void TouchMeImageViewOnTouch(object sender, View.TouchEventArgs touchEventArgs)
  {
      string message;
      switch (touchEventArgs.Event.Action & MotionEventActions.Mask)
      {
          case MotionEventActions.Down:
          case MotionEventActions.Move:
          message = "Touch Begins";
          break;

          case MotionEventActions.Up:
          message = "Touch Ends";
          break;

          default:
          message = string.Empty;
          break;
      }

      _touchInfoTextView.Text = message;
  }
  ```

上記のコードでは、`Move` と `Down` アクションを同じものとして扱うことに注意してください。 これは、ユーザーが `ImageView`から指を離していない場合でも、ユーザーが用いるしたり、ユーザーが変更する可能性のある圧力が低下したりする可能性があるためです。 これらの種類の変更により、`Move` アクションが生成されます。

ユーザーが `ImageView`に触れるたびに、次のスクリーンショットに示すように、`Touch` イベントが発生し、ハンドラーによって画面のメッセージ**タッチが開始**されます。

[タッチが開始されたアクティビティのスクリーンショット![](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

ユーザーが `ImageView`に触れている限り、**タッチが開始**され、`TextView`に表示されます。 ユーザーが `ImageView`に触れていない場合は、次のスクリーンショットに示すように、メッセージ**タッチの終了**が `TextView`に表示されます。

[タッチエンドによるアクティビティのスクリーンショットの![](android-touch-walkthrough-images/image16.png)](android-touch-walkthrough-images/image16.png#lightbox)

## <a name="gesture-recognizer-activity"></a>ジェスチャレコグナイザーアクティビティ

次に、ジェスチャレコグナイザーアクティビティを実装します。 このアクティビティでは、画面の周りにビューをドラッグし、ピンチからズームを実装する方法の1つを示します。

- `GestureRecognizer`という名前のアプリケーションに新しいアクティビティを追加します。
  次のコードのように、このアクティビティのコードを編集します。

  ```csharp
  public class GestureRecognizerActivity : Activity
  {
      protected override void OnCreate(Bundle bundle)
      {
          base.OnCreate(bundle);
          View v = new GestureRecognizerView(this);
          SetContentView(v);
      }
  }
  ```

- 新しい Android ビューをプロジェクトに追加し、`GestureRecognizerView`という名前を指定します。 次の変数をこのクラスに追加します。

  ```csharp
  private static readonly int InvalidPointerId = -1;

  private readonly Drawable _icon;
  private readonly ScaleGestureDetector _scaleDetector;

  private int _activePointerId = InvalidPointerId;
  private float _lastTouchX;
  private float _lastTouchY;
  private float _posX;
  private float _posY;
  private float _scaleFactor = 1.0f;
  ```

- 次のコンストラクターを `GestureRecognizerView`に追加します。 このコンストラクターは、アクティビティに `ImageView` を追加します。 この時点で、コードはコンパイルされません。ユーザーが pinches を使用するときに `ImageView` のサイズを変更するのに役立つクラス `MyScaleListener` を作成する必要があり &ndash;。

  ```csharp
  public GestureRecognizerView(Context context): base(context, null, 0)
  {
      _icon = context.Resources.GetDrawable(Resource.Drawable.Icon);
      _icon.SetBounds(0, 0, _icon.IntrinsicWidth, _icon.IntrinsicHeight);
      _scaleDetector = new ScaleGestureDetector(context, new MyScaleListener(this));
  }
  ```

- アクティビティにイメージを描画するには、次のスニペットに示すように、ビュークラスの `OnDraw` メソッドをオーバーライドする必要があります。 このコードは、`ImageView` を `_posX` と `_posY` によって指定された位置に移動し、スケールファクターに従ってイメージのサイズを変更します。

  ```csharp
  protected override void OnDraw(Canvas canvas)
  {
      base.OnDraw(canvas);
      canvas.Save();
      canvas.Translate(_posX, _posY);
      canvas.Scale(_scaleFactor, _scaleFactor);
      _icon.Draw(canvas);
      canvas.Restore();
  }
  ```

- 次に、ユーザーが `ImageView`の pinches として `_scaleFactor` インスタンス変数を更新する必要があります。 `MyScaleListener`というクラスを追加します。 このクラスは、ユーザーが `ImageView`を指定したときに Android によって生成されるスケールイベントをリッスンします。
  次の内部クラスを `GestureRecognizerView`に追加します。 このクラスは `ScaleGesture.SimpleOnScaleGestureListener`です。 このクラスは、ジェスチャのサブセットに関心がある場合にリスナーがサブクラス化できる便利なクラスです。

  ```csharp
  private class MyScaleListener : ScaleGestureDetector.SimpleOnScaleGestureListener
  {
      private readonly GestureRecognizerView _view;

      public MyScaleListener(GestureRecognizerView view)
      {
          _view = view;
      }

      public override bool OnScale(ScaleGestureDetector detector)
      {
          _view._scaleFactor *= detector.ScaleFactor;

          // put a limit on how small or big the image can get.
          if (_view._scaleFactor > 5.0f)
          {
              _view._scaleFactor = 5.0f;
          }
          if (_view._scaleFactor < 0.1f)
          {
              _view._scaleFactor = 0.1f;
          }

          _view.Invalidate();
          return true;
      }
  }
  ```

- `GestureRecognizerView` でオーバーライドする必要がある次のメソッドは `OnTouchEvent`です。 次のコードは、このメソッドの完全な実装を示しています。 ここには多数のコードが含まれているので、ここで何が起こっているかを見てみましょう。 このメソッドの最初の処理として、必要に応じてアイコンをスケールします。これ &ndash;、`_scaleDetector.OnTouchEvent`を呼び出すことによって処理されます。 次に、このメソッドと呼ばれるアクションについて説明します。

  - ユーザーがで画面を操作した場合、X と Y の位置と、画面に接した最初のポインターの ID が記録されます。

  - ユーザーが画面上でタッチを移動した場合は、ユーザーがポインターを移動した距離がわかります。

  - ユーザーが画面から指を持ち上げると、ジェスチャの追跡が停止されます。

  ```csharp
  public override bool OnTouchEvent(MotionEvent ev)
  {
      _scaleDetector.OnTouchEvent(ev);

      MotionEventActions action = ev.Action & MotionEventActions.Mask;
      int pointerIndex;

      switch (action)
      {
          case MotionEventActions.Down:
          _lastTouchX = ev.GetX();
          _lastTouchY = ev.GetY();
          _activePointerId = ev.GetPointerId(0);
          break;

          case MotionEventActions.Move:
          pointerIndex = ev.FindPointerIndex(_activePointerId);
          float x = ev.GetX(pointerIndex);
          float y = ev.GetY(pointerIndex);
          if (!_scaleDetector.IsInProgress)
          {
              // Only move the ScaleGestureDetector isn't already processing a gesture.
              float deltaX = x - _lastTouchX;
              float deltaY = y - _lastTouchY;
              _posX += deltaX;
              _posY += deltaY;
              Invalidate();
          }

          _lastTouchX = x;
          _lastTouchY = y;
          break;

          case MotionEventActions.Up:
          case MotionEventActions.Cancel:
          // We no longer need to keep track of the active pointer.
          _activePointerId = InvalidPointerId;
          break;

          case MotionEventActions.PointerUp:
          // check to make sure that the pointer that went up is for the gesture we're tracking.
          pointerIndex = (int) (ev.Action & MotionEventActions.PointerIndexMask) >> (int) MotionEventActions.PointerIndexShift;
          int pointerId = ev.GetPointerId(pointerIndex);
          if (pointerId == _activePointerId)
          {
              // This was our active pointer going up. Choose a new
              // action pointer and adjust accordingly
              int newPointerIndex = pointerIndex == 0 ? 1 : 0;
              _lastTouchX = ev.GetX(newPointerIndex);
              _lastTouchY = ev.GetY(newPointerIndex);
              _activePointerId = ev.GetPointerId(newPointerIndex);
          }
          break;

      }
      return true;
  }
  ```

- 次に、アプリケーションを実行し、ジェスチャレコグナイザーアクティビティを開始します。
  起動すると、次のスクリーンショットのような画面が表示されます。

  [Android アイコンを使用した![ジェスチャ認識エンジンのスタート画面](android-touch-walkthrough-images/image17.png)](android-touch-walkthrough-images/image17.png#lightbox)

- 次に、アイコンをタッチし、画面の周りにドラッグします。 ピンチ操作でズームするジェスチャを試してください。 ある時点で、画面は次のスクリーンショットのようになります。

  [![ジェスチャ画面の周りを移動するアイコン](android-touch-walkthrough-images/image18.png)](android-touch-walkthrough-images/image18.png#lightbox)

この時点で、前に pat を提供する必要があります。これは、Android アプリケーションにピンチによるズームインを実装したばかりです。 このチュートリアルでは、カスタムジェスチャを使用して、3番目と最後のアクティビティ &ndash; に進むことができます。

## <a name="custom-gesture-activity"></a>カスタムジェスチャアクティビティ

このチュートリアルの最後の画面では、カスタムジェスチャを使用します。

このチュートリアルでは、ジェスチャライブラリはジェスチャツールを使用して既に作成されており、ファイル**リソース/生/ジェスチャ**でプロジェクトに追加されています。 このようにしてこのような作業を行うことで、チュートリアルの最後のアクティビティを使用できるようになります。

- 次の内容を使用して、**カスタム\_ジェスチャ\_layout. axml**という名前のレイアウトファイルをプロジェクトに追加します。 プロジェクトには、 **Resources**フォルダー内のすべてのイメージが既に存在します。

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:orientation="vertical"
      android:layout_width="match_parent"
      android:layout_height="match_parent">
      <LinearLayout
          android:layout_width="match_parent"
          android:layout_height="0dp"
          android:layout_weight="1" />
      <ImageView
          android:src="@drawable/check_me"
          android:layout_width="match_parent"
          android:layout_height="0dp"
          android:layout_weight="3"
          android:id="@+id/imageView1"
          android:layout_gravity="center_vertical" />
      <LinearLayout
          android:layout_width="match_parent"
          android:layout_height="0dp"
          android:layout_weight="1" />
  </LinearLayout>
  ```

- 次に、新しいアクティビティをプロジェクトに追加し、`CustomGestureRecognizerActivity.cs`という名前を指定します。 次の2行のコードに示すように、クラスに2つのインスタンス変数を追加します。

  ```csharp
  private GestureLibrary _gestureLibrary;
  private ImageView _imageView;
  ```

- このアクティビティの `OnCreate` メソッドを編集して、次のコードのようにします。 このコードで何が起こっているかについて説明します。 まず、`GestureOverlayView` をインスタンス化し、それをアクティビティのルートビューとして設定します。
  また、`GestureOverlayView`の `GesturePerformed` イベントにイベントハンドラーを割り当てます。 次に、前に作成したレイアウトファイルを拡大し、`GestureOverlayView`の子ビューとして追加します。 最後の手順では、変数 `_gestureLibrary` を初期化し、アプリケーションリソースからジェスチャファイルを読み込みます。 何らかの理由でジェスチャファイルを読み込むことができない場合、このアクティビティは実行できないため、シャットダウンされます。

  ```csharp
  protected override void OnCreate(Bundle bundle)
  {
      base.OnCreate(bundle);

      GestureOverlayView gestureOverlayView = new GestureOverlayView(this);
      SetContentView(gestureOverlayView);
      gestureOverlayView.GesturePerformed += GestureOverlayViewOnGesturePerformed;

      View view = LayoutInflater.Inflate(Resource.Layout.custom_gesture_layout, null);
      _imageView = view.FindViewById<ImageView>(Resource.Id.imageView1);
      gestureOverlayView.AddView(view);

      _gestureLibrary = GestureLibraries.FromRawResource(this, Resource.Raw.gestures);
      if (!_gestureLibrary.Load())
      {
          Log.Wtf(GetType().FullName, "There was a problem loading the gesture library.");
          Finish();
      }
  }
  ```

- 最後に、次のコードスニペットに示すように、メソッド `GestureOverlayViewOnGesturePerformed` を実装する必要があります。 `GestureOverlayView` がジェスチャを検出すると、このメソッドにコールバックします。 最初に、`_gestureLibrary.Recognize()`を呼び出してジェスチャに一致する `IList<Prediction>` オブジェクトを取得しようとします。 LINQ を使用して、ジェスチャの最高スコアを持つ `Prediction` を取得します。

  十分なスコアを持つ一致するジェスチャがない場合、イベントハンドラーは何も行わずに終了します。 それ以外の場合は、予測の名前を確認し、ジェスチャの名前に基づいて表示されるイメージを変更します。

  ```csharp
  private void GestureOverlayViewOnGesturePerformed(object sender, GestureOverlayView.GesturePerformedEventArgs gesturePerformedEventArgs)
  {
      IEnumerable<Prediction> predictions = from p in _gestureLibrary.Recognize(gesturePerformedEventArgs.Gesture)
      orderby p.Score descending
      where p.Score > 1.0
      select p;
      Prediction prediction = predictions.FirstOrDefault();

      if (prediction == null)
      {
          Log.Debug(GetType().FullName, "Nothing seemed to match the user's gesture, so don't do anything.");
          return;
      }

      Log.Debug(GetType().FullName, "Using the prediction named {0} with a score of {1}.", prediction.Name, prediction.Score);

      if (prediction.Name.StartsWith("checkmark"))
      {
          _imageView.SetImageResource(Resource.Drawable.checked_me);
      }
      else if (prediction.Name.StartsWith("erase", StringComparison.OrdinalIgnoreCase))
      {
          // Match one of our "erase" gestures
          _imageView.SetImageResource(Resource.Drawable.check_me);
      }
  }
  ```

- アプリケーションを実行し、カスタムジェスチャレコグナイザーアクティビティを開始します。 次のスクリーンショットのようになります。

  [チェックインイメージを使用したスクリーンショットの![](android-touch-walkthrough-images/image19.png)](android-touch-walkthrough-images/image19.png#lightbox)

  画面にチェックマークを描画します。表示されるビットマップは、次のスクリーンショットに示されているようになります。

  [![描画チェックマーク、チェックマークが認識されます](android-touch-walkthrough-images/image20.png)](android-touch-walkthrough-images/image20.png#lightbox)

  最後に、画面に scribble を描きます。 チェックボックスは、次のスクリーンショットに示すように、元のイメージに戻す必要があります。

  [画面上に Scribble を![、元のイメージが表示されます。](android-touch-walkthrough-images/image21.png)](android-touch-walkthrough-images/image21.png#lightbox)

Android アプリケーションで、Xamarin を使用してタッチとジェスチャを統合する方法について理解できました。

## <a name="related-links"></a>関連リンク

- [Android タッチスタート (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-touch-start)
- [Android のタッチ最終 (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-touch-final)
