---
title: 'チュートリアル: Xamarin.iOS でタッチを使用します。'
description: このドキュメントでは、サンプル タッチの相互作用、ジェスチャ レコグナイザー、およびカスタム ジェスチャ レコグナイザーについて話し合い、Xamarin.iOS アプリケーションでのタッチを処理する方法について説明します。
ms.prod: xamarin
ms.assetid: 13F8289B-7A80-4959-AF3F-57874D866DCA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: fff49599d3843bb09d407316d6964ca54b6a1004
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784791"
---
# <a name="walkthrough-using-touch-in-xamarinios"></a>チュートリアル: Xamarin.iOS でタッチを使用します。

このチュートリアルでは、さまざまな種類のタッチ イベントに応答するコードを記述する方法を示します。 それぞれの例は、別の画面に含まれています。

- [サンプルをタッチ](#Touch_Samples)– タッチ イベントに応答する方法です。
- [ジェスチャ レコグナイザー サンプル](#Gesture_Recognizer_Samples)– 組み込みジェスチャ レコグナイザーを使用する方法です。
- [カスタム ジェスチャ レコグナイザー サンプル](#Custom_Gesture_Recognizer)– カスタム ジェスチャ レコグナイザーを構築する方法です。

各セクションでは、最初からコードを記述する手順を説明します。
[開始のサンプル コード](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start)完了のストーリー ボードとメニュー画面が既に含まれます。

 [![](ios-touch-walkthrough-images/image3.png "このサンプルには、メニュー画面が含まれています。")](ios-touch-walkthrough-images/image3.png#lightbox)

コードをストーリー ボードに追加し、iOS で使用できるタッチ イベントのさまざまな種類について学ぶには、以下の手順に従います。 また、開く、[完成したサンプル](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final)操作すべてを表示します。

<a name="Touch_Samples"/>

## <a name="touch-samples"></a>タッチのサンプル

このサンプルでタッチ Api の一部について説明されます。 タッチ イベントの実装に必要なコードを追加する手順に従います。


1. プロジェクトを開く**Touch_Start**です。 最初に、プロジェクトの実行をすべて、問題がないかどうかを確認し、タッチ、**タッチ サンプル**ボタンをクリックします。 (いずれのボタンは機能) には、次のような画面が表示されます。
    
    [![](ios-touch-walkthrough-images/image4.png "非稼働ボタンを使用して実行するサンプル アプリ")](ios-touch-walkthrough-images/image4.png#lightbox)


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
    
    このメソッドの動作を確認する、`UITouch`オブジェクト、および、存在する場合は、タッチが発生した場所に基づくアクションを実行します。

    * _TouchImage 内_– テキストを表示`Touches Began`ラベルおよびイメージを変更します。
    * _DoubleTouchImage 内_– ジェスチャは、ダブルタップをした場合に表示するイメージを変更します。
    * _DragImage 内_– タッチが開始されたことを示すフラグを設定します。 メソッド`TouchesMoved`かどうかをこのフラグを使用して、`DragImage`もそうでない画面を移動する必要が次の手順で説明するようです。

    上記のコードのみが扱う個々 の調整、動作はありませんが、ユーザーが画面に指を移動する場合。 操作に対する応答には実装`TouchesMoved`次のコードに示すようにします。

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

    このメソッドは、取得、`UITouch`オブジェクト、および、タッチが発生したかどうかを確認します。 タッチが発生した場合`TouchImage`画面に表示される仕上げ移動テキストします。 

    場合`touchStartedInside`が true の場合、ユーザー指を持っていることが分かって`DragImage`の周囲に移動することができます。 コードを移動`DragImage`ユーザーが画面の周りには、その指を移動するとします。

1. ユーザーが画面をオフに自分の指を離したまたは iOS タッチ イベントをキャンセルするときにケースを処理する必要があります。 実装します。 これは、`TouchesEnded`と`TouchesCancelled`次のようにします。

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
    
    これら両方のメソッドは、リセット、`touchStartedInside`フラグを false にします。 `TouchesEnded` 表示ではまた`TouchesEnded`画面にします。

1. この時点でサンプルのタッチ スクリーンが完了しました。 次のスクリーン ショットに示すように、画面が、イメージを操作するときを変更する方法に注意してください。
        
    [![](ios-touch-walkthrough-images/image4.png "アプリの開始画面")](ios-touch-walkthrough-images/image4.png#lightbox)
    
    [![](ios-touch-walkthrough-images/image5.png "ユーザーがボタンをドラッグした後、画面")](ios-touch-walkthrough-images/image5.png#lightbox)
 

<a name="Gesture_Recognizer_Samples" />

##  <a name="gesture-recognizer-samples"></a>ジェスチャ レコグナイザー サンプル

[前のセクション](#Touch_Samples)タッチ イベントを使用して、画面の周りにオブジェクトをドラッグする方法を示しました。
ここでは、タッチ イベントの削除はし、次のジェスチャ レコグナイザーを使用する方法を示します。

-  `UIPanGestureRecognizer`画面の周囲でイメージをドラッグします。
-  `UITapGestureRecognizer`二重タップ画面上に応答します。

実行する場合、[開始のサンプル コード](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start) をクリックし、**ジェスチャ レコグナイザー サンプル**ボタン、次の画面が表示されます。

 [![](ios-touch-walkthrough-images/image6.png "ジェスチャ レコグナイザー サンプル ボタン クリックすると、この画面を表示します。")](ios-touch-walkthrough-images/image6.png#lightbox)

ジェスチャ レコグナイザーを実装する次の手順に従います。


1. ファイルを編集して**GestureViewController.cs**し、次のインスタンス変数を追加します。

    ```csharp
    #region Private Variables
    private bool imageHighlighted = false;
    private RectangleF originalImageFrame = RectangleF.Empty;
    #endregion
    ```

    イメージの以前の場所を追跡するには、このインスタンス変数が必要です。
パン ジェスチャ レコグナイザーを使用して、`originalImageFrame`画面にイメージを再描画するために必要なオフセットを計算する値。

1. コント ローラーに次のメソッドを追加します。

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

    このコードをインスタンス化、`UIPanGestureRecognizer`インスタンス、およびビューに追加します。
メソッドの形式でジェスチャをターゲットを割り当てることに注意してください。 `HandleDrag` : 次の手順でこのメソッドを提供します。

1. HandleDrag を実装するのには、コント ローラーに次のコードを追加します。

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

    上記のコードは最初、ジェスチャ認識エンジンの状態を確認し、画面の周りのイメージを移動します。 コント ローラーは場所でこのコードでは、画面の周りの 1 つのイメージのドラッグをようになりましたサポートします。


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

    このコードのためのコードによく似ています、`UIPanGestureRecognizer`が使用しているターゲットのデリゲートを使用してではなく、`Action`です。 

1. 変更を行うには、最終的なものは、`ViewDidLoad`できるように、追加したばかりのメソッドを呼び出します。 次のコードに似ていますが、ViewDidLoad を変更します。

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

    値を初期化しても気付か`originalImageFrame`です。


1. アプリケーションを実行し、2 つのイメージと対話します。
次のスクリーン ショットは、これらの操作の 1 つの例を示します。
    
    [![](ios-touch-walkthrough-images/image7.png "このスクリーン ショットは、ドラッグ操作を示しています。")](ios-touch-walkthrough-images/image7.png#lightbox)



<a name="Custom_Gesture_Recognizer"/>

## <a name="custom-gesture-recognizer"></a>カスタム ジェスチャ レコグナイザー

このセクションの概念で前のセクションでは、カスタム ジェスチャ レコグナイザーをビルドするお適用されます。 カスタム ジェスチャ レコグナイザーはサブクラス`UIGestureRecognizer`は、画面上のユーザーは、"V"を描画する場合を認識し、ビットマップを切り替えます。 次のスクリーン ショットは、この画面の例を示します。

 [![](ios-touch-walkthrough-images/image8.png "ユーザーが画面に 'V' を描画するときに、アプリが認識されます。")](ios-touch-walkthrough-images/image8.png#lightbox)

カスタム ジェスチャ レコグナイザーを作成する手順に従います。


1. という名前のプロジェクトに新しいクラスを追加`CheckmarkGestureRecognizer`、され、次のコードのようになります。

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

    Reset メソッドが呼び出されます、`State`プロパティを変更するか、`Recognized`または`Ended`です。 これは、カスタム ジェスチャ認識エンジンで設定の内部状態をリセットする時刻です。
今すぐ、クラスは、次回ユーザーがアプリケーションで、操作をやり直すし、ジェスチャを認識することを再度試行する準備が完了します。



1. これで、カスタム ジェスチャ レコグナイザーを定義したので (`CheckmarkGestureRecognizer`) を編集、 **CustomGestureViewController.cs**ファイルし、次の 2 つのインスタンス変数を追加します。

    ```csharp
    #region Private Variables
    private bool isChecked = false;
    private CheckmarkGestureRecognizer checkmarkGesture;
    #endregion
    ```

1. インスタンスを作成し、当社ジェスチャ レコグナイザーを構成するには、コント ローラーに次のメソッドを追加します。

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

1. 編集`ViewDidLoad`を呼び出すように`WireUpCheckmarkGestureRecognizer`次のコード スニペットで示すように、します。

    ```csharp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();
    
        // Wire up the gesture recognizer
        WireUpCheckmarkGestureRecognizer();
    }
    ```

1. アプリケーションを実行して、画面上の"V"の描画を再試行してください。 次のスクリーン ショットに示すように、変更を表示するイメージ表示されます。
    
    [![](ios-touch-walkthrough-images/image9.png "チェック ボタン")](ios-touch-walkthrough-images/image9.png#lightbox)
    
    [![](ios-touch-walkthrough-images/image10.png "未チェックのボタン")](ios-touch-walkthrough-images/image10.png#lightbox)



上記の 3 つのセクションでは、タッチ、iOS でのイベントに応答する別の方法について説明しました: タッチ イベント、組み込みのジェスチャ レコグナイザーを使用するかカスタム ジェスチャ レコグナイザーを持つ。



## <a name="related-links"></a>関連リンク

- [iOS (サンプル) を開始するタッチ](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start)
- [iOS タッチ最終的な (サンプル)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final)
