---
title: チュートリアル - Android でタッチの使用
ms.prod: xamarin
ms.assetid: E281F89B-4142-4BD8-8882-FB65508BF69E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/09/2018
ms.openlocfilehash: c4192f22ebd0ad1cde27745f5439c2d18a268ed3
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61012576"
---
# <a name="walkthrough---using-touch-in-android"></a>チュートリアル - Android でタッチの使用

お知らせする実用的なアプリケーションでは、前のセクションの概念を使用する方法を参照してください。 4 つのアクティビティで、アプリケーションを作成します。 最初のアクティビティは、さまざまな Api を示すために他のアクティビティを起動するメニューやメニューになります。 次のスクリーン ショットは、メインのアクティビティを示しています。

[![例のスクリーン ショットでタッチ Me ボタン](android-touch-walkthrough-images/image14.png)](android-touch-walkthrough-images/image14.png#lightbox)

最初のアクティビティでは、タッチのサンプルでは、イベント ハンドラーを使用して、ビューを変更する方法を示します。 ジェスチャ レコグナイザー アクティビティが紹介をサブクラス化方法`Android.View.Views`とイベントを処理するだけでなくピンチ ジェスチャを処理する方法について説明します。 3 番目と最後のアクティビティを**カスタム ジェスチャ**が表示する方法を使用して、カスタム ジェスチャ。 わかりやすくするために従い、吸収、私たちがこのチュートリアルに分割セクションでは、各セクションでは、アクティビティのいずれかに重点を置くことで。

## <a name="touch-sample-activity"></a>タッチ サンプル アクティビティ

-   プロジェクトを開く**TouchWalkthrough\_開始**します。 **MainActivity**そこには、&ndash;はユーザー アクティビティでのタッチ動作の実装次第です。 アプリケーションを実行し、をクリックした場合**タッチ サンプル**、次のアクティビティを起動する必要があります。

    [![表示の開始タッチ アクティビティのスクリーン ショット](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

-   アクティビティを開始ことを確認しましたが、ファイルを開いて**TouchActivity.cs**のハンドラーを追加し、`Touch`のイベント、 `ImageView`:

    ```csharp
    _touchMeImageView.Touch += TouchMeImageViewOnTouch;
    ```

-   次のメソッドを次に、追加**TouchActivity.cs**:

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

上記のコードで扱うことに注意してください。、`Move`と`Down`操作と同じです。 これは、場合でも、ユーザーことがありますいないリフト指であるため、`ImageView`の周囲に移動する場合、または、ユーザーが与える負荷が変わる可能性があります。 この種の変更が生成されます、`Move`アクション。

ユーザーの仕上げを毎回、 `ImageView`、`Touch`イベントが発生し、ハンドラー、メッセージが表示されます**開始タッチ**画面で、次のスクリーン ショットに示すように。

[![タッチが開始アクティビティのスクリーン ショット](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

ユーザーがタッチであれば、 `ImageView`、**タッチが開始**に表示される、`TextView`します。 ユーザーが不要になった触れること、 `ImageView`、メッセージ**終了タッチ**に表示される、`TextView`次のスクリーン ショットのように。

[![タッチの終了活動のスクリーン ショット](android-touch-walkthrough-images/image16.png)](android-touch-walkthrough-images/image16.png#lightbox)


## <a name="gesture-recognizer-activity"></a>ジェスチャ認識エンジンのアクティビティ

ジェスチャ レコグナイザー アクティビティを実装できるようになりました。 このアクティビティは、画面の周りのビューをドラッグし、ピンチ ズームを実装する方法を説明する方法を紹介します。

-   新しいアクティビティと呼ばれるアプリケーションを追加`GestureRecognizer`します。
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

-   追加、新しい Android プロジェクトに表示し、名前を`GestureRecognizerView`します。 このクラスには、次の変数を追加します。

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

-   次のコンス トラクターを追加`GestureRecognizerView`します。 このコンス トラクターは追加、`ImageView`アクティビティにします。 この時点で、コードがコンパイルされません&ndash;クラスを作成する必要があります`MyScaleListener`サイズ変更の操作に役立つ、`ImageView`ときに、ユーザーがその pinches:

    ```csharp
    public GestureRecognizerView(Context context): base(context, null, 0)
    {
        _icon = context.Resources.GetDrawable(Resource.Drawable.Icon);
        _icon.SetBounds(0, 0, _icon.IntrinsicWidth, _icon.IntrinsicHeight);
        _scaleDetector = new ScaleGestureDetector(context, new MyScaleListener(this));
    }
    ```

-   オーバーライドしなければ、アクティビティで、イメージを描画するために、`OnDraw`次のスニペットに示すようにビュー クラスのメソッド。 このコードは移動、`ImageView`で指定した位置に`_posX`と`_posY`スケール ファクターに従ってイメージのサイズとしても。

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

-   次にインスタンス変数を更新する必要があります`_scaleFactor`pinches ユーザーとして、`ImageView`します。 という名前のクラスを追加します`MyScaleListener`します。 このクラスはユーザー pinches するときに、Android によって発生するスケール イベントをリッスン、`ImageView`します。
    次の内部クラスを追加`GestureRecognizerView`します。 このクラスは、`ScaleGesture.SimpleOnScaleGestureListener`します。 このクラスは、リスナーでは、ジェスチャのサブセットに関心があるときに、サブクラスことができます、便利なクラスです。

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

-   次の方法でオーバーライドする必要があります`GestureRecognizerView`は`OnTouchEvent`します。 次のコードは、このメソッドの完全な実装を一覧表示します。 コードの多くがここでは、それでは、少し時間がかかるし、ここで何が起きてになります。 このメソッドは、まずは、必要な場合は、アイコンをスケール変更&ndash;呼び出すことによってこの処理は`_scaleDetector.OnTouchEvent`します。 次にどのようなアクションは、このメソッドを呼び出すを試してください。

    - 場合は、ユーザーは、画面をタッチ、X と Y の位置と、最初の画面をタッチ ポインターの ID を記録します。

    - 場合は、ユーザーがタッチ画面上で移動、し、見つける、ユーザーがポインターを移動する距離。

    - ユーザーが画面から外れて、指を解除しは停止ジェスチャを追跡します。

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

-   ここで、アプリケーションを実行し、ジェスチャ レコグナイザーのアクティビティを開始します。
    開始時に、画面は次のスクリーン ショットのようになります。

    [![Android のアイコンでジェスチャ レコグナイザーのスタート画面](android-touch-walkthrough-images/image17.png)](android-touch-walkthrough-images/image17.png#lightbox)

-   ここで、アイコンをタッチし、画面にドラッグします。 ピンチ ズーム ジェスチャを実行してください。 ある時点で、画面に次のスクリーン ショットのようなります可能性があります。

    [![ジェスチャが画面の周りのアイコンを移動します。](android-touch-walkthrough-images/image18.png)](android-touch-walkthrough-images/image18.png#lightbox)

この時点でする必要がありますを実現する pat 裏面: Android アプリケーションにピンチ ズームを実装しました。 休憩でき、このチュートリアルでは、3 番目と最後のアクティビティに進む&ndash;カスタム ジェスチャを使用します。

## <a name="custom-gesture-activity"></a>ジェスチャのカスタム アクティビティ

このチュートリアルの最後の画面では、カスタム ジェスチャを使用します。

このチュートリアルの目的で、ジェスチャ ライブラリが既にジェスチャのツールを使用して作成されファイル内のプロジェクトに追加**ジェスチャ/リソース/生**します。 このビットのハウスキーピングの邪魔には、チュートリアルの最後のアクティビティで取得ことができます。

-   という名前のレイアウト ファイルを追加**カスタム\_ジェスチャ\_layout.axml**をプロジェクトに、次の内容。 プロジェクトすべてのイメージは既に、**リソース**フォルダー。

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

-   次に、プロジェクトに新しいアクティビティを追加し、名前`CustomGestureRecognizerActivity.cs`します。 次の 2 行のコードに示すように、クラスに 2 つのインスタンス変数を追加します。

    ```csharp
    private GestureLibrary _gestureLibrary;
    private ImageView _imageView;
    ```

-   編集、`OnCreate`メソッドのこのアクティビティのため、it が次のコードに似ています。 このコードで何が起こってを説明する時間がかかることができます。 最初に行うことがインスタンス化、`GestureOverlayView`アクティビティのルート ビューとして設定しています。
    イベント ハンドラーも割り当てます、`GesturePerformed`のイベント`GestureOverlayView`します。 次に、作成した、レイアウト ファイルを展開しましたし、としての子ビューの追加、`GestureOverlayView`します。 最後の手順は、変数を初期化するために、`_gestureLibrary`アプリケーション リソースからのジェスチャ ファイルを読み込むとします。 何らかの理由、ジェスチャ ファイルを読み込めない場合はあまりありません、このアクティビティが実行できるので、シャット ダウン。

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

-   最後には、メソッドが実装する必要があります、`GestureOverlayViewOnGesturePerformed`次のコード スニペットに示すようにします。 ときに、`GestureOverlayView`のジェスチャを検出します。 このメソッドにコールバックします。 まず get を試みた、`IList<Prediction>`ジェスチャを呼び出すことで一致するオブジェクト`_gestureLibrary.Recognize()`します。 LINQ のビットを使用して取得する、`Prediction`ジェスチャの最高スコアを持ちます。

    一致がない場合は、ジェスチャ、十分なスコアの高い、イベント ハンドラーが何もしないで終了します。 それ以外の場合、予測の名前を確認し、ジェスチャの名前を基に表示されるイメージを変更します。

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

-   アプリケーションを実行し、カスタム ジェスチャ レコグナイザー アクティビティを開始します。 次のスクリーン ショットのようになります。

    [![スクリーン ショットを確認するイメージ](android-touch-walkthrough-images/image19.png)](android-touch-walkthrough-images/image19.png#lightbox)

    画面で、チェック マークを描画し、表示されているビットマップは次のスクリーン ショットに示すようになります。

    [![チェック マークの描画、チェック マークを認識します。](android-touch-walkthrough-images/image20.png)](android-touch-walkthrough-images/image20.png#lightbox)

    最後に、画面に、scribble を描画します。 チェック ボックスは、これらのスクリーン ショットに示すように、元のイメージに変更してください。

    [![Scribble 画面で、元のイメージが表示されます。](android-touch-walkthrough-images/image21.png)](android-touch-walkthrough-images/image21.png#lightbox)

タッチとジェスチャで Xamarin.Android を使用して Android アプリケーションを統合する方法を理解があるようになりました。


## <a name="related-links"></a>関連リンク

- [Android タッチ (サンプル) を開始](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android タッチ最後 (サンプル)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
