---
title: チュートリアル - Android でのタッチの使用
ms.prod: xamarin
ms.assetid: E281F89B-4142-4BD8-8882-FB65508BF69E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/09/2018
ms.openlocfilehash: 625ba800ce498f80c0344c67e26bd79360de4002
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/10/2018
ms.locfileid: "34050560"
---
# <a name="walkthrough---using-touch-in-android"></a>チュートリアル - Android でのタッチの使用

お知らせ実用的なアプリケーションで、前のセクションから概念を使用する方法を参照してください。 次の 4 つの活動を使ってアプリケーションを作成します。 最初のアクティビティは、さまざまな Api を示すために他のアクティビティを起動するメニューやメニューになります。 次のスクリーン ショットは、メインのアクティビティを示しています。

[![サンプルのスクリーン ショットでタッチ Me ボタン](android-touch-walkthrough-images/image14.png)](android-touch-walkthrough-images/image14.png#lightbox)

最初のアクティビティでは、タッチのサンプルでは、ビューと接触しているにイベント ハンドラーを使用する方法を示します。 ジェスチャ レコグナイザー アクティビティが実演サブクラス化する方法`Android.View.Views`とイベントの処理だけでなくピンチ ジェスチャを処理する方法を示します。 3 番目および最後のアクティビティ**カスタム ジェスチャ**は表示する方法を使用してカスタム ジェスチャ。 わかりやすくするために従い、吸収、おをこのチュートリアルに分割のセクションでは、アクティビティのいずれかに焦点を当てたの各セクションでします。

## <a name="touch-sample-activity"></a>タッチのサンプルのアクティビティ

-   プロジェクトを開く**TouchWalkthrough\_開始**です。 **MainActivity**へのすべての設定は、&ndash;まで、アクティビティでのタッチ動作を実装することができます。 アプリケーションを実行する をクリックすると**タッチ サンプル**、次のアクティビティを起動する必要があります。

    [![表示の開始タッチ アクティビティのスクリーン ショット](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

-   これで、アクティビティの開始することを確認して、ファイルを開く**TouchActivity.cs**のハンドラーを追加し、`Touch`のイベント、 `ImageView`:

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

上記のコードで扱うことに注意してください。、`Move`と`Down`操作と同じです。 これは、いなくてもユーザー可能性がありますいないを持ち上げます指ため、`ImageView`の周囲に移動する場合、またはユーザーに作用する負荷を変更することがあります。 この種の変更が生成されます、`Move`アクション。

たびにユーザーの調整、 `ImageView`、`Touch`イベントを発生させるし、ハンドラー、メッセージが表示されます**開始タッチ**画面で、次のスクリーン ショットに示すように。

[![タッチを開始アクティビティのスクリーン ショット](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

ユーザーと接触している限り、 `ImageView`、**開始タッチ**に表示される、`TextView`です。 ときに、ユーザーが不要になったに触れること、 `ImageView`、メッセージ**終了タッチ**に表示される、`TextView`次のスクリーン ショットに示すように。

[![タッチの終了活動のスクリーン ショット](android-touch-walkthrough-images/image16.png)](android-touch-walkthrough-images/image16.png#lightbox)


## <a name="gesture-recognizer-activity"></a>ジェスチャ レコグナイザー アクティビティ

ジェスチャ レコグナイザー アクティビティを実装できるようになりました。 このアクティビティは、ピンチ操作で拡大/縮小を実装する方法の 1 つを説明し、画面の周りにビューをドラッグする方法をデモンストレーションします。

-   新しいアクティビティと呼ばれるアプリケーションを追加`GestureRecognizer`です。
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

-   新しい Android を追加、プロジェクトを表示し、名前を付けます`GestureRecognizerView`です。 このクラスに次の変数を追加します。

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

-   次のコンス トラクターを追加`GestureRecognizerView`です。 このコンス トラクターを追加、`ImageView`アクティビティにします。 この時点で、コードもはコンパイルされません&ndash;クラスを作成する必要があります`MyScaleListener`サイズを変更するのに役立つ、`ImageView`ときに、ユーザーがその pinches:

    ```csharp
    public GestureRecognizerView(Context context): base(context, null, 0)
    {
        _icon = context.Resources.GetDrawable(Resource.Drawable.Icon);
        _icon.SetBounds(0, 0, _icon.IntrinsicWidth, _icon.IntrinsicHeight);
        _scaleDetector = new ScaleGestureDetector(context, new MyScaleListener(this));
    }
    ```

-   この例のアクティビティにイメージを描画する必要がありますを上書きする、`OnDraw`次のスニペットに示すように、ビュー クラスのメソッドです。 このコードは移動、`ImageView`によって指定された位置に`_posX`と`_posY`スケール ファクターに従ってイメージのサイズとしても。

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

-   次に、インスタンス変数を更新する必要があります`_scaleFactor`ユーザーとして pinches、`ImageView`です。 呼ばれるクラスを追加します`MyScaleListener`です。 このクラスはユーザー pinches するときに、Android によって発生する規模イベントをリッスン、`ImageView`です。
    次の内部クラスを追加`GestureRecognizerView`です。 このクラスは、`ScaleGesture.SimpleOnScaleGestureListener`です。 このクラスは、リスナーできるサブクラス ジェスチャのサブセットに興味のあるときに便利なクラスを示します。

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

            _iconview.Invalidate();
            return true;
        }
    }
    ```

-   次のメソッドでオーバーライドする必要があります`GestureRecognizerView`は`OnTouchEvent`します。 次のコードには、このメソッドの完全な実装が一覧表示します。 コードの多くはここでは、それでは、時間がかかるし、何が起こっているこちらを確認します。 このメソッドは最初の手順が必要な場合は、アイコンをスケール&ndash;これは呼び出すことによって処理`_scaleDetector.OnTouchEvent`です。 次にどのようなアクションは、このメソッドを呼び出すかを実行してください。

    - 場合は、ユーザーで画面をタッチは、X と Y の位置と、最初の画面をタッチ ポインターの ID を記録します。

    - 場合は、ユーザーは、画面上のタッチを移動、しを見つけ出す、ユーザーがマウス ポインターを移動する距離。

    - ユーザーが画面をオフには、その指を解除し、おが停止するジェスチャを追跡します。

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

-   ここで、アプリケーションを実行し、ジェスチャ レコグナイザー アクティビティを開始します。
    開始時に、画面は次のスクリーン ショットのようになります。

    [![Android のアイコンがジェスチャ レコグナイザー スタート 画面](android-touch-walkthrough-images/image17.png)](android-touch-walkthrough-images/image17.png#lightbox)

-   今すぐ、アイコンをタッチし、画面にドラッグします。 ピンチ、ズーム ジェスチャを再試行してください。 ある時点で、画面が次のスクリーン ショットのようなもの表示します。

    [![画面の周りのジェスチャ移動アイコン](android-touch-walkthrough-images/image18.png)](android-touch-walkthrough-images/image18.png#lightbox)

この時点でする必要がありますを自分自身に付与、pat 裏面: Android アプリケーションで、ピンチ操作で拡大/縮小を実装しているだけです。 クイック休息を取って、このチュートリアルでは、3 番目および最後のアクティビティに進むことができます&ndash;カスタム ジェスチャを使用します。

## <a name="custom-gesture-activity"></a>カスタム ジェスチャ アクティビティ

このチュートリアルの最終画面では、カスタム ジェスチャを使用します。

このチュートリアルの目的で、ジェスチャ ライブラリが既にジェスチャ ツールを使用して作成されファイル内のプロジェクトに追加**ジェスチャ/リソース/生**です。 このビットのハウスキーピングのでは、このチュートリアルでの最後の活動取得ことができます。

-   という名前のレイアウト ファイルを追加**カスタム\_ジェスチャ\_layout.axml**次の内容を含むプロジェクトにします。 プロジェクトすべてのイメージは既に、**リソース**フォルダー。

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

-   次に、プロジェクトに新しいアクティビティを追加し、名前を付けます`CustomGestureRecognizerActivity.cs`です。 クラスに次の 2 行のコードに示すとして 2 つのインスタンス変数を追加します。

    ```csharp
    private GestureLibrary _gestureLibrary;
    private ImageView _imageView;
    ```

-   編集、`OnCreate`メソッドのこのアクティビティが次のコードのようになりますようにします。 このコードで何が起こってを説明する分ほどかかることができます。 最初に行うことがインスタンス化、`GestureOverlayView`し、アクティビティのルートのビューとして設定します。
    イベント ハンドラーも割り当てます、`GesturePerformed`のイベント`GestureOverlayView`です。 次に以前に作成されたレイアウト ファイルを展開し、子ビューとして追加する、`GestureOverlayView`です。 最後の手順は、変数を初期化する`_gestureLibrary`アプリケーション リソースからのジェスチャ ファイルを読み込むとします。 何らかの理由によりジェスチャ ファイルを読み込めない場合はほとんどありません、このアクティビティが実行できるので、シャット ダウンします。

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

-   メソッドの実装は最終的なもの`GestureOverlayViewOnGesturePerformed`次のコード スニペットで示すようにします。 ときに、`GestureOverlayView`ジェスチャを検出されたこのメソッドにコールバックします。 取得しようとして、まず、`IList<Prediction>`ジェスチャを呼び出すことによって一致するオブジェクト`_gestureLibrary.Recognize()`です。 LINQ のビットを使用して取得、`Prediction`ジェスチャのスコアが最も高いを持ちます。

    一致がない場合は、十分なスコアの高いジェスチャし、イベント ハンドラーが何もせずに終了します。 それ以外の場合、予測の名前を確認し、表示されるイメージを変更するには、ジェスチャの名前に基づきます。

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

-   アプリケーションを実行し、カスタム ジェスチャ レコグナイザー アクティビティを開始します。 次のスクリーン ショットのように表示する必要があります。

    [![スクリーン ショットを確認 Me イメージ](android-touch-walkthrough-images/image19.png)](android-touch-walkthrough-images/image19.png#lightbox)

    画面で、チェック マークを描画し、ビットマップが表示されているようになりますが、次のスクリーン ショットに示す。

    [![描画のチェック マーク、チェック マークを認識します。](android-touch-walkthrough-images/image20.png)](android-touch-walkthrough-images/image20.png#lightbox)

    最後に、画面で、scribble を描画します。 チェック ボックスは、これらのスクリーン ショットに示すように、元のイメージに変更してください。

    [![Scribble 画面で、元のイメージが表示されます。](android-touch-walkthrough-images/image21.png)](android-touch-walkthrough-images/image21.png#lightbox)

タッチとジェスチャ Xamarin.Android を使用して Android のアプリケーションに統合する方法の理解があるようになりました。


## <a name="related-links"></a>関連リンク

- [Android タッチ (サンプル) を開始](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android タッチ最終 (サンプル)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
