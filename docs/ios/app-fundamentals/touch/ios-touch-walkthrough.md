---
title: 'チュートリアル: Xamarin.iOS でのタッチの使用'
description: このドキュメントでは、サンプルのタッチ操作、ジェスチャ レコグナイザー、およびカスタム ジェスチャ レコグナイザーを説明する Xamarin.iOS アプリケーションでタッチを処理する方法について説明します。
ms.prod: xamarin
ms.assetid: 13F8289B-7A80-4959-AF3F-57874D866DCA
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: bff4d46ac9d5fe893cbb0a2dfa032e1b9f6daa0e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61407815"
---
# <a name="walkthrough-using-touch-in-xamarinios"></a>チュートリアル: Xamarin.iOS でのタッチの使用

このチュートリアルでは、さまざまな種類のタッチ イベントに応答するコードを記述する方法を示します。 それぞれの例は、別々 の画面に含まれています。

- [サンプルのタッチ](#Touch_Samples)– タッチ イベントに応答する方法。
- [ジェスチャ認識エンジン サンプル](#Gesture_Recognizer_Samples)– 組み込みジェスチャ レコグナイザーを使用する方法。
- [カスタムのジェスチャ レコグナイザー サンプル](#Custom_Gesture_Recognizer)– カスタム ジェスチャ認識エンジンを構築する方法。

各セクションでは、ゼロからコードを記述する手順を説明します。
[サンプル コードを開始](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start)既に完全なストーリー ボードとメニュー画面が含まれます。

 [![](ios-touch-walkthrough-images/image3.png "サンプルには、メニュー画面が含まれます。")](ios-touch-walkthrough-images/image3.png#lightbox)

コード、ストーリー ボードを追加し、iOS で使用可能なタッチ イベントのさまざまな種類の詳細には、次の手順に従います。 また、開く、[完成したサンプル](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final)作業すべてを表示します。

<a name="Touch_Samples"/>

## <a name="touch-samples"></a>タッチ サンプル

このサンプルでは、タッチ Api の一部が紹介されます。 タッチ イベントを実装するために必要なコードを追加するこれらの手順に従います。


1. プロジェクトを開く**Touch_Start**します。 最初にプロジェクトを実行するすべてが、正常かどうかを確認し、タッチ、**タッチ サンプル**ボタンをクリックします。 (ただし、いずれのボタンは機能)、次のような画面が表示されます。
    
    [![](ios-touch-walkthrough-images/image4.png "サンプル アプリの非稼働ボタンと実行")](ios-touch-walkthrough-images/image4.png#lightbox)


1. ファイルを編集して**TouchViewController.cs**クラスに次の 2 つのインスタンス変数を追加および`TouchViewController`:

    ```csharp 
    #region Private Variables
    private bool imageHighlighted = false;
    private bool touchStartedInside;
    #endregion
    ```


1. 実装、`TouchesBegan`メソッドを次のコードに示すようにします。

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
    
    このメソッドの動作をチェックするか、`UITouch`オブジェクト、および、存在する場合は、タッチが発生した場所に基づくアクションを実行します。

    * _TouchImage 内_– テキストを表示します`Touches Began`ラベルとイメージを変更します。
    * _DoubleTouchImage 内_– ダブルタップ ジェスチャ場合に表示されるイメージを変更します。
    * _DragImage 内_–、タッチが開始されたことを示すフラグを設定します。 メソッド`TouchesMoved`かを判断するこのフラグを使用して、`DragImage`か、画面を移動する必要が次の手順で説明するようです。

    個々 の仕上げにのみ、上記のコードを処理、動作がないまま、ユーザーが画面に指を移動する場合。 移動するに対応する実装`TouchesMoved`以下のコードに示すようにします。

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

    このメソッドは、取得、`UITouch`オブジェクト、し、タッチが発生した場所を確認します。 タッチが発生した場合`TouchImage`、画面に表示されるタッチが移動テキスト。 

    場合`touchStartedInside`が true の場合、ユーザーがに指を持っていることがわかって`DragImage`の周囲に移動することができます。 コードが移動`DragImage`ユーザーが画面に指を移動するとします。

1. ユーザーが画面をオフに自分の指を離したまたは iOS タッチ イベントをキャンセルするときにケースを処理する必要があります。 実装します。 これは、`TouchesEnded`と`TouchesCancelled`次に示すよう。

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
    
    これらのメソッドの両方がリセットされます、`touchStartedInside`フラグを false にします。 `TouchesEnded` 表示ではまた`TouchesEnded`画面。

1. この時点でサンプルのタッチ スクリーンが完了しました。 次のスクリーン ショットで示すように、画面がのイメージのそれぞれの操作を変更する方法に注意してください。
        
    [![](ios-touch-walkthrough-images/image4.png "アプリの開始画面")](ios-touch-walkthrough-images/image4.png#lightbox)
    
    [![](ios-touch-walkthrough-images/image5.png "ユーザーがボタンをドラッグした後、画面")](ios-touch-walkthrough-images/image5.png#lightbox)
 

<a name="Gesture_Recognizer_Samples" />

##  <a name="gesture-recognizer-samples"></a>ジェスチャ認識エンジンのサンプル

[前のセクション](#Touch_Samples)タッチ イベントを使用して、画面を中心にオブジェクトをドラッグする方法を示しました。
ここでは、タッチ イベントの削除はし、次のジェスチャ レコグナイザーを使用する方法について説明します。

-  `UIPanGestureRecognizer`画面の周囲でイメージをドラッグします。
-  `UITapGestureRecognizer`画面上にダブル タップに応答します。

実行する場合、[サンプル コードを開始](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start) をクリックし、**ジェスチャ認識エンジン サンプル**ボタン、次の画面が表示されます。

 [![](ios-touch-walkthrough-images/image6.png "ジェスチャ認識エンジンのサンプル ボタンをクリックすると、この画面を示しています")](ios-touch-walkthrough-images/image6.png#lightbox)

ジェスチャ レコグナイザーを実装するためにこれらの手順に従います。


1. ファイルを編集して**GestureViewController.cs**し、次のインスタンス変数を追加します。

    ```csharp
    #region Private Variables
    private bool imageHighlighted = false;
    private RectangleF originalImageFrame = RectangleF.Empty;
    #endregion
    ```

    このインスタンスの変数の前のイメージの場所を追跡する必要があります。
パン ジェスチャ レコグナイザーを使用して、`originalImageFrame`画面にイメージを再描画するために必要なオフセットを計算する値。

1. コント ローラーには、次のメソッドを追加します。

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

    このコードをインスタンス化、`UIPanGestureRecognizer`インスタンス、およびそれをビューに追加します。
メソッドの形式でジェスチャをターゲットを割り当てることに注意してください。 `HandleDrag` – このメソッドは、次の手順で提供されます。

1. HandleDrag を実装するには、コント ローラーに次のコードを追加します。

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

    上記のコードはまずジェスチャ認識エンジンの状態を確認し、イメージを画面に移動し、します。 コント ローラーはこのコードで、画面イメージを 1 つをドラッグすることをようになりましたサポートします。


1. 追加、`UITapGestureRecognizer`その DoubleTouchImage に表示されるイメージが変更されます。 次のメソッドを追加、`GestureViewController`コント ローラー。

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

    このコードは、コードによく似ています、`UIPanGestureRecognizer`が使用しているターゲットのデリゲートを使用する代わりに、`Action`します。 

1. 最後に実行する必要がありますが、変更`ViewDidLoad`追加したメソッドを呼び出すようにします。 次のコードのように、ViewDidLoad に変更します。

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

    値を初期化しているにも気付か`originalImageFrame`します。


1. アプリケーションを実行し、2 つのイメージを操作します。
次のスクリーン ショットでは、これらのインタラクションの 1 つの例を示します。
    
    [![](ios-touch-walkthrough-images/image7.png "このスクリーン ショットは、ドラッグ操作を示しています。")](ios-touch-walkthrough-images/image7.png#lightbox)



<a name="Custom_Gesture_Recognizer"/>

## <a name="custom-gesture-recognizer"></a>カスタム ジェスチャ レコグナイザー

このセクションでは、カスタム ジェスチャ レコグナイザーを構築する前のセクションの概念が適用されます。 カスタム ジェスチャ レコグナイザーはサブクラス`UIGestureRecognizer`とは、画面で、ユーザーは、"V"を描画する場合を認識し、ビットマップを切り替えます。 次のスクリーン ショットでは、この画面の例を示します。

 [![](ios-touch-walkthrough-images/image8.png "ユーザーが画面に 'V' を描画するときに、アプリが認識されます。")](ios-touch-walkthrough-images/image8.png#lightbox)

カスタムのジェスチャ認識エンジンを作成する次の手順に従います。


1. という名前のプロジェクトに新しいクラスを追加`CheckmarkGestureRecognizer`、し、次のコードのようになります。

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

    Reset メソッドが呼び出されます、`State`プロパティを変更するか、`Recognized`または`Ended`します。 これは、カスタム ジェスチャ レコグナイザーの設定の内部状態をリセットする時刻です。
今すぐクラスは、次回ユーザーが、アプリケーションと対話を新たに開始し、ジェスチャの認識を再試行する準備が完了します。



1. カスタム ジェスチャ レコグナイザーを定義したところ (`CheckmarkGestureRecognizer`) を編集、 **CustomGestureViewController.cs**ファイルを開き、次の 2 つのインスタンス変数を追加します。

    ```csharp
    #region Private Variables
    private bool isChecked = false;
    private CheckmarkGestureRecognizer checkmarkGesture;
    #endregion
    ```

1. インスタンス化し、ジェスチャ レコグナイザーを構成するには、コント ローラーに次のメソッドを追加します。

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

1. 編集`ViewDidLoad`を呼び出すよう`WireUpCheckmarkGestureRecognizer`の次のコード スニペットに示すようにします。

    ```csharp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();
    
        // Wire up the gesture recognizer
        WireUpCheckmarkGestureRecognizer();
    }
    ```

1. アプリケーションを実行し、画面に"V"を描いてください。 次のスクリーン ショットに示すように、変更を表示、イメージが表示されます。
    
    [![](ios-touch-walkthrough-images/image9.png "チェック ボタン")](ios-touch-walkthrough-images/image9.png#lightbox)
    
    [![](ios-touch-walkthrough-images/image10.png "オフ ボタン")](ios-touch-walkthrough-images/image10.png#lightbox)



3 つのセクション タッチ iOS でのイベントに応答するさまざまな方法が示されている手順を上記: タッチ イベントでは、組み込みのジェスチャ認識機能を使用して、またはカスタムのジェスチャ認識エンジン。



## <a name="related-links"></a>関連リンク

- [iOS (サンプル) を開始するタッチ](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start)
- [iOS の最終タッチ (サンプル)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final)
