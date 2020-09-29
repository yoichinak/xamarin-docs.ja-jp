---
title: 'チュートリアル: Xamarin でのタッチの使用'
description: このドキュメントでは、Xamarin iOS アプリケーションでタッチを処理する方法について説明します。タッチ操作のサンプル、ジェスチャレコグナイザー、およびカスタムジェスチャレコグナイザーについて説明します。
ms.prod: xamarin
ms.assetid: 13F8289B-7A80-4959-AF3F-57874D866DCA
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 3d296c36febfb5671c816372aa97661494179b83
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91433908"
---
# <a name="walkthrough-using-touch-in-xamarinios"></a>チュートリアル: Xamarin でのタッチの使用

このチュートリアルでは、さまざまな種類のタッチイベントに応答するコードを記述する方法について説明します。 それぞれの例は、別の画面に含まれています。

- [タッチサンプル](#Touch_Samples) –タッチイベントに応答する方法。
- [ジェスチャレコグナイザーのサンプル](#Gesture_Recognizer_Samples) –組み込みのジェスチャレコグナイザーの使用方法。
- [カスタムジェスチャレコグナイザーのサンプル](#Custom_Gesture_Recognizer) –カスタムジェスチャ認識エンジンを構築する方法。

各セクションには、最初からコードを記述する手順が含まれています。

次の手順に従ってコードをストーリーボードに追加し、iOS で使用できるさまざまな種類のタッチイベントについて説明します。 または、完成した [サンプル](/samples/xamarin/ios-samples/applicationfundamentals-touch-final) を開いてすべての機能を確認します。

<a name="Touch_Samples"></a>

## <a name="touch-samples"></a>タッチサンプル

このサンプルでは、いくつかのタッチ Api について説明します。 タッチイベントを実装するために必要なコードを追加するには、次の手順に従います。

1. プロジェクト **Touch_Start**を開きます。 まず、プロジェクトを実行してすべて問題ないことを確認し、[ **タッチサンプル** ] ボタンにタッチします。 次のような画面が表示されます (ただし、どのボタンも動作しません)。

    [![動作しないボタンを使用したサンプルアプリの実行](ios-touch-walkthrough-images/image4.png)](ios-touch-walkthrough-images/image4.png#lightbox)

1. **TouchViewController.cs**ファイルを編集し、次の2つのインスタンス変数をクラスに追加し `TouchViewController` ます。

    ```csharp
    #region Private Variables
    private bool imageHighlighted = false;
    private bool touchStartedInside;
    #endregion
    ```

1. 次の `TouchesBegan` コードに示すように、メソッドを実装します。

    ```csharp
    public override void TouchesBegan(NSSet touches, UIEvent evt)
    {
        base.TouchesBegan(touches, evt);

        // If Multitouch is enabled, report the number of fingers down
        TouchStatus.Text = string.Format ("Number of fingers {0}", touches.Count);

        // Get the current touch
        UITouch touch = touches.AnyObject as UITouch;
        if (touch != null)
        {
            // Check to see if any of the images have been touched
            if (TouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                // Fist image touched
                TouchImage.Image = UIImage.FromBundle("TouchMe_Touched.png");
                TouchStatus.Text = "Touches Began";
            } else if (touch.TapCount == 2 && DoubleTouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                // Second image double-tapped, toggle bitmap
                if (imageHighlighted)
                {
                    DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe.png");
                    TouchStatus.Text = "Double-Tapped Off";
                }
                else
                {
                    DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe_Highlighted.png");
                    TouchStatus.Text = "Double-Tapped On";
                }
                imageHighlighted = !imageHighlighted;
            } else if (DragImage.Frame.Contains(touch.LocationInView(View)))
            {
                // Third image touched, prepare to drag
                touchStartedInside = true;
            }
        }
    }
    ```

    このメソッドは、オブジェクトをチェックすることによって機能 `UITouch` します。また、存在する場合は、タッチの発生箇所に基づいて何らかのアクションを実行します。

    - [_内部 TouchImage_ ] –ラベルのテキストを表示 `Touches Began` し、画像を変更します。
    - _DoubleTouchImage 内_ -ジェスチャがダブルタップの場合に表示されるイメージを変更します。
    - [ _DragImage 内_] –タッチが開始されたことを示すフラグを設定します。 メソッドは、このフラグを使用して、 `TouchesMoved` `DragImage` 次の手順で説明するように、画面の周りを移動するかどうかを判断します。

    上記のコードは個々の操作のみを扱っていますが、ユーザーが画面上で指を動かしても動作はありません。 移動に対応するには、 `TouchesMoved` 次のコードに示すようにを実装します。

    ```csharp
    public override void TouchesMoved(NSSet touches, UIEvent evt)
    {
        base.TouchesMoved(touches, evt);
        // get the touch
        UITouch touch = touches.AnyObject as UITouch;
        if (touch != null)
        {
            //==== IMAGE TOUCH
            if (TouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                TouchStatus.Text = "Touches Moved";
            }

            //==== IMAGE DRAG
            // check to see if the touch started in the drag me image
            if (touchStartedInside)
            {
                // move the shape
                float offsetX = touch.PreviousLocationInView(View).X - touch.LocationInView(View).X;
                float offsetY = touch.PreviousLocationInView(View).Y - touch.LocationInView(View).Y;
                DragImage.Frame = new RectangleF(new PointF(DragImage.Frame.X - offsetX, DragImage.Frame.Y - offsetY), DragImage.Frame.Size);
            }
        }
    }
    ```

    このメソッドは、オブジェクトを取得し、 `UITouch` タッチがどこで発生したかを確認します。 でタッチが発生した場合は、移動した `TouchImage` テキストが画面に表示されます。

    が true の場合は、ユーザーが指を持っていて、 `touchStartedInside` 移動していること `DragImage` がわかります。 ユーザーが指を画面の上に移動すると、コードが移動し `DragImage` ます。

1. ユーザーが画面から指を離したとき、または iOS がタッチイベントをキャンセルしたときに、そのケースを処理する必要があります。 そのために、次のように `TouchesEnded` とを実装し `TouchesCancelled` ます。

    ```csharp
    public override void TouchesCancelled(NSSet touches, UIEvent evt)
    {
        base.TouchesCancelled(touches, evt);

        // reset our tracking flags
        touchStartedInside = false;
        TouchImage.Image = UIImage.FromBundle("TouchMe.png");
        TouchStatus.Text = "";
    }

    public override void TouchesEnded(NSSet touches, UIEvent evt)
    {
        base.TouchesEnded(touches, evt);
        // get the touch
        UITouch touch = touches.AnyObject as UITouch;
        if (touch != null)
        {
            //==== IMAGE TOUCH
            if (TouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                TouchImage.Image = UIImage.FromBundle("TouchMe.png");
                TouchStatus.Text = "Touches Ended";
            }
        }
        // reset our tracking flags
        touchStartedInside = false;
    }
    ```

    どちらの方法でもフラグが `touchStartedInside` false にリセットされます。 `TouchesEnded` は画面にも表示され `TouchesEnded` ます。

1. この時点で、[タッチサンプル] 画面が完成しました。 次のスクリーンショットに示すように、各イメージを操作すると画面がどのように変化するかに注目してください。

    [![アプリの開始画面](ios-touch-walkthrough-images/image4.png)](ios-touch-walkthrough-images/image4.png#lightbox)

    [![ユーザーがボタンをドラッグした後の画面](ios-touch-walkthrough-images/image5.png)](ios-touch-walkthrough-images/image5.png#lightbox)

<a name="Gesture_Recognizer_Samples"></a>

## <a name="gesture-recognizer-samples"></a>ジェスチャレコグナイザーのサンプル

[前のセクション](#Touch_Samples)では、タッチイベントを使用して、オブジェクトを画面の周りにドラッグする方法を示していました。
このセクションでは、タッチイベントを除去し、次のジェスチャレコグナイザーを使用する方法を示します。

- `UIPanGestureRecognizer`画面の周りのイメージをドラッグするための。
- `UITapGestureRecognizer`画面上のダブルタップに応答する。

ジェスチャレコグナイザーを実装するには、次の手順に従います。

1. **GestureViewController.cs**ファイルを編集し、次のインスタンス変数を追加します。

    ```csharp
    #region Private Variables
    private bool imageHighlighted = false;
    private RectangleF originalImageFrame = RectangleF.Empty;
    #endregion
    ```

    イメージの前の場所を追跡するには、このインスタンス変数が必要です。
パンジェスチャ認識エンジンは、値を使用して、 `originalImageFrame` 画面上のイメージを再描画するために必要なオフセットを計算します。

1. コントローラーに次のメソッドを追加します。

    ```csharp
    private void WireUpDragGestureRecognizer()
    {
        // Create a new tap gesture
        UIPanGestureRecognizer gesture = new UIPanGestureRecognizer();

        // Wire up the event handler (have to use a selector)
        gesture.AddTarget(() => HandleDrag(gesture));  // to be defined

        // Add the gesture recognizer to the view
        DragImage.AddGestureRecognizer(gesture);
    }
    ```

    このコードは、 `UIPanGestureRecognizer` インスタンスをインスタンス化し、ビューに追加します。
メソッドの形式でジェスチャにターゲットを割り当てることに注意して `HandleDrag` ください。このメソッドは、次の手順で提供されます。

1. HandleDrag を実装するには、コントローラーに次のコードを追加します。

    ```csharp
    private void HandleDrag(UIPanGestureRecognizer recognizer)
    {
        // If it's just began, cache the location of the image
        if (recognizer.State == UIGestureRecognizerState.Began)
        {
            originalImageFrame = DragImage.Frame;
        }

        // Move the image if the gesture is valid
        if (recognizer.State != (UIGestureRecognizerState.Cancelled | UIGestureRecognizerState.Failed
            | UIGestureRecognizerState.Possible))
        {
            // Move the image by adding the offset to the object's frame
            PointF offset = recognizer.TranslationInView(DragImage);
            RectangleF newFrame = originalImageFrame;
            newFrame.Offset(offset.X, offset.Y);
            DragImage.Frame = newFrame;
        }
    }
    ```

    上記のコードでは、まずジェスチャ認識エンジンの状態を確認し、画面の周りにイメージを移動します。 このコードを使用すると、コントローラーは画面上の1つのイメージのドラッグをサポートできるようになります。

1. `UITapGestureRecognizer`DoubleTouchImage に表示されるイメージを変更するを追加します。 コントローラーに次のメソッドを追加し `GestureViewController` ます。

    ```csharp
    private void WireUpTapGestureRecognizer()
    {
        // Create a new tap gesture
        UITapGestureRecognizer tapGesture = null;

        // Report touch
        Action action = () => {
            TouchStatus.Text = string.Format("Image touched at: {0}",tapGesture.LocationOfTouch(0, DoubleTouchImage));

            // Toggle the image
            if (imageHighlighted)
            {
                DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe.png");
            }
            else
            {
                DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe_Highlighted.png");
            }
            imageHighlighted = !imageHighlighted;
        };

        tapGesture = new UITapGestureRecognizer(action);

        // Configure it
        tapGesture.NumberOfTapsRequired = 2;

        // Add the gesture recognizer to the view
        DoubleTouchImage.AddGestureRecognizer(tapGesture);
    }
    ```

    このコードは、のコードによく似ていますが、を `UIPanGestureRecognizer` 使用しているターゲットに対してデリゲートを使用する代わりに、を使用し `Action` ます。

1. 最後に、追加したメソッドを呼び出すように変更する必要があり `ViewDidLoad` ます。 次のコードのように ViewDidLoad を変更します。

    ```csharp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();

        Title = "Gesture Recognizers";

        // Save initial state
        originalImageFrame = DragImage.Frame;

        WireUpTapGestureRecognizer();
        WireUpDragGestureRecognizer();
    }
    ```

    の値を初期化することにも注意して `originalImageFrame` ください。

1. アプリケーションを実行し、2つのイメージを操作します。
次のスクリーンショットは、これらの相互作用の一例です。

    [![このスクリーンショットは、ドラッグ操作を示しています。](ios-touch-walkthrough-images/image7.png)](ios-touch-walkthrough-images/image7.png#lightbox)

<a name="Custom_Gesture_Recognizer"></a>

## <a name="custom-gesture-recognizer"></a>カスタムジェスチャレコグナイザー

このセクションでは、カスタムジェスチャ認識エンジンを構築するために、前のセクションの概念を適用します。 カスタムジェスチャ認識エンジンはサブクラス `UIGestureRecognizer` であり、ユーザーが画面に "V" を描画したときに認識され、ビットマップを切り替えます。 次のスクリーンショットは、この画面の例を示しています。

 [![アプリは、ユーザーが画面に ' V ' を描画したときに認識します](ios-touch-walkthrough-images/image8.png)](ios-touch-walkthrough-images/image8.png#lightbox)

カスタムジェスチャ認識エンジンを作成するには、次の手順に従います。

1. という名前のプロジェクトに新しいクラスを追加 `CheckmarkGestureRecognizer` し、次のコードのようにします。

    ```csharp
    using System;
    using CoreGraphics;
    using Foundation;
    using UIKit;

    namespace Touch
    {
        public class CheckmarkGestureRecognizer : UIGestureRecognizer
        {
            #region Private Variables
            private CGPoint midpoint = CGPoint.Empty;
            private bool strokeUp = false;
            #endregion

            #region Override Methods
            /// <summary>
            ///   Called when the touches end or the recognizer state fails
            /// </summary>
            public override void Reset()
            {
                base.Reset();

                strokeUp = false;
                midpoint = CGPoint.Empty;
            }

            /// <summary>
            ///   Is called when the fingers touch the screen.
            /// </summary>
            public override void TouchesBegan(NSSet touches, UIEvent evt)
            {
                base.TouchesBegan(touches, evt);

                // we want one and only one finger
                if (touches.Count != 1)
                {
                    base.State = UIGestureRecognizerState.Failed;
                }

                Console.WriteLine(base.State.ToString());
            }

            /// <summary>
            ///   Called when the touches are cancelled due to a phone call, etc.
            /// </summary>
            public override void TouchesCancelled(NSSet touches, UIEvent evt)
            {
                base.TouchesCancelled(touches, evt);
                // we fail the recognizer so that there isn't unexpected behavior
                // if the application comes back into view
                base.State = UIGestureRecognizerState.Failed;
            }

            /// <summary>
            ///   Called when the fingers lift off the screen
            /// </summary>
            public override void TouchesEnded(NSSet touches, UIEvent evt)
            {
                base.TouchesEnded(touches, evt);
                //
                if (base.State == UIGestureRecognizerState.Possible && strokeUp)
                {
                    base.State = UIGestureRecognizerState.Recognized;
                }

                Console.WriteLine(base.State.ToString());
            }

            /// <summary>
            ///   Called when the fingers move
            /// </summary>
            public override void TouchesMoved(NSSet touches, UIEvent evt)
            {
                base.TouchesMoved(touches, evt);

                // if we haven't already failed
                if (base.State != UIGestureRecognizerState.Failed)
                {
                    // get the current and previous touch point
                    CGPoint newPoint = (touches.AnyObject as UITouch).LocationInView(View);
                    CGPoint previousPoint = (touches.AnyObject as UITouch).PreviousLocationInView(View);

                    // if we're not already on the upstroke
                    if (!strokeUp)
                    {
                        // if we're moving down, just continue to set the midpoint at
                        // whatever point we're at. when we start to stroke up, it'll stick
                        // as the last point before we upticked
                        if (newPoint.X >= previousPoint.X && newPoint.Y >= previousPoint.Y)
                        {
                            midpoint = newPoint;
                        }
                        // if we're stroking up (moving right x and up y [y axis is flipped])
                        else if (newPoint.X >= previousPoint.X && newPoint.Y <= previousPoint.Y)
                        {
                            strokeUp = true;
                        }
                        // otherwise, we fail the recognizer
                        else
                        {
                            base.State = UIGestureRecognizerState.Failed;
                        }
                    }
                }

                Console.WriteLine(base.State.ToString());
            }
            #endregion
        }
    }
    ```

    Reset メソッドは、 `State` プロパティがまたはのいずれかに変更されたときに呼び出され `Recognized` `Ended` ます。 これは、カスタムジェスチャ認識エンジンで内部状態セットをリセットするための時間です。
次に、ユーザーがアプリケーションと対話するときに、クラスを最新の状態にして、ジェスチャを再度認識する準備ができました。

1. カスタムジェスチャ認識エンジン () を定義したので `CheckmarkGestureRecognizer` 、 **CustomGestureViewController.cs** ファイルを編集し、次の2つのインスタンス変数を追加します。

    ```csharp
    #region Private Variables
    private bool isChecked = false;
    private CheckmarkGestureRecognizer checkmarkGesture;
    #endregion
    ```

1. ジェスチャ認識エンジンをインスタンス化して構成するには、次のメソッドをコントローラーに追加します。

    ```csharp
    private void WireUpCheckmarkGestureRecognizer()
    {
        // Create the recognizer
        checkmarkGesture = new CheckmarkGestureRecognizer();

        // Wire up the event handler
        checkmarkGesture.AddTarget(() => {
            if (checkmarkGesture.State == (UIGestureRecognizerState.Recognized | UIGestureRecognizerState.Ended))
            {
                if (isChecked)
                {
                    CheckboxImage.Image = UIImage.FromBundle("CheckBox_Unchecked.png");
                }
                else
                {
                    CheckboxImage.Image = UIImage.FromBundle("CheckBox_Checked.png");
                }
                isChecked = !isChecked;
            }
        });

        // Add the gesture recognizer to the view
        View.AddGestureRecognizer(checkmarkGesture);
    }
    ```

1. `ViewDidLoad` `WireUpCheckmarkGestureRecognizer` 次のコードスニペットに示すように、を編集してを呼び出します。

    ```csharp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();

        // Wire up the gesture recognizer
        WireUpCheckmarkGestureRecognizer();
    }
    ```

1. アプリケーションを実行し、画面に "V" を描画します。 次のスクリーンショットに示すように、表示されているイメージが変化していることがわかります。

    [![ボタンがオンにされました](ios-touch-walkthrough-images/image9.png)](ios-touch-walkthrough-images/image9.png#lightbox)

    [![オフになっているボタン](ios-touch-walkthrough-images/image10.png)](ios-touch-walkthrough-images/image10.png#lightbox)

上記の3つのセクションでは、iOS のタッチイベントに応答するさまざまな方法を示しています。タッチイベント、組み込みのジェスチャレコグナイザー、またはカスタムジェスチャレコグナイザーを使用します。

## <a name="related-links"></a>関連リンク

- [iOS タッチの最終版 (サンプル)](/samples/xamarin/ios-samples/applicationfundamentals-touch-final)