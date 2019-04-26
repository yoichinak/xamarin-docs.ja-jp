---
title: Xamarin.iOS での手動カメラ コントロール
description: このドキュメントでは、手動カメラ コントロールを有効にする iOS AVFoundation フレームワークを Xamarin.iOS で使用する方法について説明します。 手動カメラ コントロールには、コントロール フォーカス、ホワイト バランス、および公開の設定、ユーザーができるようにします。
ms.prod: xamarin
ms.assetid: 56340225-5F3C-4BFC-9A79-61496D7FE5B5
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: c61b3fee9009afb86ccd3fd0e16d7812a8e90feb
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61384307"
---
# <a name="manual-camera-controls-in-xamarinios"></a>Xamarin.iOS での手動カメラ コントロール

提供される、手動カメラ コントロール、`AVFoundation Framework`では、iOS 8、iOS デバイスのカメラを完全に制御を実行するモバイル アプリケーションを許可します。 この細かいレベルの制御は、プロフェッショナル レベルのカメラ アプリケーションを作成し、静止画像またはビデオを作成中に、カメラのパラメーターを調整することにより、アーティストのコンポジションを提供できます。

結果が正確性や、イメージの美しさに特化したことが少ないと、いくつかの機能や行われているイメージの要素を強調表示により特化したは、科学的または産業用のアプリケーションを開発するときにでもこれらのコントロールできますも役立ちます。

## <a name="avfoundation-capture-objects"></a>AVFoundation キャプチャ オブジェクト

ビデオや静止画像、カメラを使用して iOS デバイスで使用すると、かどうかそれらのイメージをキャプチャするために使用するプロセス、ほぼ同じです。 これは、既定の自動カメラ コントロールまたは新しい手動カメラ コントロールを活用するために使用するアプリケーションの場合は true。

 [![](intro-to-manual-camera-controls-images/image1.png "AVFoundation キャプチャ オブジェクトの概要")](intro-to-manual-camera-controls-images/image1.png#lightbox)

入力がから取得した、`AVCaptureDeviceInput`に、`AVCaptureSession`での`AVCaptureConnection`します。 結果は、静止画像またはビデオをストリームとして、いずれかの出力を示します。 プロセス全体がによって制御される、`AVCaptureDevice`します。

## <a name="manual-controls-provided"></a>提供される手動のコントロール

IOS 8 で提供される新しい Api を使用して、アプリケーションは、カメラの機能を次のコントロールにかかることができます。

-  **手動のフォーカス**– アプリケーションを直接コントロール フォーカスのエンド ユーザーを許可すると、取得したイメージより詳細に制御を提供することができます。
-  **手動露出**–、露出を手動で制御を提供し、アプリケーションはより自由をユーザーに提供し、図案化された外観の実現を可能にします。
-  **手動ホワイト バランス**– ホワイト バランスを使用して、イメージの色を調整する: 現実的な外観にするのには多くの場合。 異なる光源がある別の色温度、およびイメージをキャプチャするために使用するカメラの設定がこれらの違いを補正するために調整されます。 ここでも、ホワイト バランス経由でユーザー コントロールを許可すると、ユーザーが自動的に行うことができない調整を行うことができます。


iOS 8 の拡張機能を提供し、既存の iOS をこのイメージの詳細に制御を提供する Api の機能強化がプロセスをキャプチャします。

## <a name="bracketed-capture"></a>かっこで囲まれたキャプチャ

囲まキャプチャでは、上で示した手動カメラ コントロールからの設定に基づいており、により、アプリケーションをさまざまな方法で、特定の時点をキャプチャできます。

簡単に言えば、さまざまな設定は、画像から画像を使用して作成した静止画のバーストは、角をキャプチャします。

## <a name="requirements"></a>必要条件

次がこの記事で紹介する手順を完了する必要です。

-  **Xcode 7 以降および iOS 8 以降**– Apple の Xcode 7 と iOS 8、または新しい Api がインストールされ、開発者のコンピューターに構成する必要があります。
-  **Visual Studio for Mac** – Visual Studio for Mac の最新バージョンをインストールして、ユーザーのデバイスで構成する必要があります。
-  **iOS 8 デバイス**– iOS 8 の最新バージョンを実行している iOS デバイス。 IOS シミュレーターでカメラの機能をテストすることはできません。


## <a name="general-av-capture-setup"></a>[全般] AV キャプチャのセットアップ

IOS デバイスでビデオを記録するときに常に必要ないくつかの一般的なセットアップ コードがあります。 このセクションの内容は、iOS デバイスのカメラからビデオを記録しでそのビデオを表示するために必要最小限の設定でリアルタイム、`UIImageView`します。

### <a name="output-sample-buffer-delegate"></a>出力サンプル バッファー デリゲート

最初の点の 1 つ必要なサンプルの出力バッファーを監視し、バッファーから取得したイメージを表示するデリゲートを`UIImageView`アプリケーションの UI。

次のルーチンは、サンプル バッファーを監視され、UI が更新されます。

```csharp
using System;
using Foundation;
using UIKit;
using System.CodeDom.Compiler;
using System.Collections.Generic;
using System.Linq;
using AVFoundation;
using CoreVideo;
using CoreMedia;
using CoreGraphics;

namespace ManualCameraControls
{
    public class OutputRecorder : AVCaptureVideoDataOutputSampleBufferDelegate
    {
        #region Computed Properties
        public UIImageView DisplayView { get; set; }
        #endregion

        #region Constructors
        public OutputRecorder ()
        {

        }
        #endregion

        #region Private Methods
        private UIImage GetImageFromSampleBuffer(CMSampleBuffer sampleBuffer) {

            // Get a pixel buffer from the sample buffer
            using (var pixelBuffer = sampleBuffer.GetImageBuffer () as CVPixelBuffer) {
                // Lock the base address
                pixelBuffer.Lock (0);

                // Prepare to decode buffer
                var flags = CGBitmapFlags.PremultipliedFirst | CGBitmapFlags.ByteOrder32Little;

                // Decode buffer - Create a new colorspace
                using (var cs = CGColorSpace.CreateDeviceRGB ()) {

                    // Create new context from buffer
                    using (var context = new CGBitmapContext (pixelBuffer.BaseAddress,
                        pixelBuffer.Width,
                        pixelBuffer.Height,
                        8,
                        pixelBuffer.BytesPerRow,
                        cs,
                        (CGImageAlphaInfo)flags)) {

                        // Get the image from the context
                        using (var cgImage = context.ToImage ()) {

                            // Unlock and return image
                            pixelBuffer.Unlock (0);
                            return UIImage.FromImage (cgImage);
                        }
                    }
                }
            }
        }
        #endregion

        #region Override Methods
        public override void DidOutputSampleBuffer (AVCaptureOutput captureOutput, CMSampleBuffer sampleBuffer, AVCaptureConnection connection)
        {
            // Trap all errors
            try {
                // Grab an image from the buffer
                var image = GetImageFromSampleBuffer(sampleBuffer);

                // Display the image
                if (DisplayView !=null) {
                    DisplayView.BeginInvokeOnMainThread(() => {
                        // Set the image
                        if (DisplayView.Image != null) DisplayView.Image.Dispose();
                        DisplayView.Image = image;

                        // Rotate image to the correct display orientation
                        DisplayView.Transform = CGAffineTransform.MakeRotation((float)Math.PI/2);
                    });
                }

                // IMPORTANT: You must release the buffer because AVFoundation has a fixed number
                // of buffers and will stop delivering frames if it runs out.
                sampleBuffer.Dispose();
            }
            catch(Exception e) {
                // Report error
                Console.WriteLine ("Error sampling buffer: {0}", e.Message);
            }
        }
        #endregion
    }
}
```

インプレースでは、このルーチンで、`AppDelegate`ライブ ビデオ フィードを記録する AV キャプチャ セッションを開くには変更できます。

### <a name="creating-an-av-capture-session"></a>AV キャプチャ セッションの作成

AV キャプチャ セッションでは、iOS デバイスのカメラからライブ ビデオの記録を制御するために使用し、iOS アプリケーションにビデオを取得するために必要な。 例では、以降`ManualCameraControl`サンプル アプリケーションが複数の異なる場所で、キャプチャ セッションを使用して、構成される、`AppDelegate`されアプリケーション全体に利用できます。

アプリケーションの変更には、次の操作を行います`AppDelegate`し、必要なコードを追加します。


1. ダブルクリックして、`AppDelegate.cs`を開き、編集、ソリューション エクスプ ローラー内のファイル。
1. 次の追加、ファイルの先頭にステートメントを使用します。
    
    ```
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    ```

1. 次のプライベート変数と計算されたプロパティを追加、`AppDelegate`クラス。
    
    ```
    #region Private Variables
    private NSError Error;
    #endregion
    
    #region Computed Properties
    public override UIWindow Window {get;set;}
    public bool CameraAvailable { get; set; }
    public AVCaptureSession Session { get; set; }
    public AVCaptureDevice CaptureDevice { get; set; }
    public OutputRecorder Recorder { get; set; }
    public DispatchQueue Queue { get; set; }
    public AVCaptureDeviceInput Input { get; set; }
    #endregion
    ```
  
1. 完成したメソッドをオーバーライドしに変更します。
    
    ```
    public override void FinishedLaunching (UIApplication application)
    {
        // Create a new capture session
        Session = new AVCaptureSession ();
        Session.SessionPreset = AVCaptureSession.PresetMedium;
    
        // Create a device input
        CaptureDevice = AVCaptureDevice.DefaultDeviceWithMediaType (AVMediaType.Video);
        if (CaptureDevice == null) {
            // Video capture not supported, abort
            Console.WriteLine ("Video recording not supported on this device");
            CameraAvailable = false;
            return;
        }
    
        // Prepare device for configuration
        CaptureDevice.LockForConfiguration (out Error);
        if (Error != null) {
            // There has been an issue, abort
            Console.WriteLine ("Error: {0}", Error.LocalizedDescription);
            CaptureDevice.UnlockForConfiguration ();
            return;
        }
    
        // Configure stream for 15 frames per second (fps)
        CaptureDevice.ActiveVideoMinFrameDuration = new CMTime (1, 15);
    
        // Unlock configuration
        CaptureDevice.UnlockForConfiguration ();
    
        // Get input from capture device
        Input = AVCaptureDeviceInput.FromDevice (CaptureDevice);
        if (Input == null) {
            // Error, report and abort
            Console.WriteLine ("Unable to gain input from capture device.");
            CameraAvailable = false;
            return;
        }
    
        // Attach input to session
        Session.AddInput (Input);
    
        // Create a new output
        var output = new AVCaptureVideoDataOutput ();
        var settings = new AVVideoSettingsUncompressed ();
        settings.PixelFormatType = CVPixelFormatType.CV32BGRA;
        output.WeakVideoSettings = settings.Dictionary;
    
        // Configure and attach to the output to the session
        Queue = new DispatchQueue ("ManCamQueue");
        Recorder = new OutputRecorder ();
        output.SetSampleBufferDelegate (Recorder, Queue);
        Session.AddOutput (output);
    
        // Let tabs know that a camera is available
        CameraAvailable = true;
    }
    ```  
  
1. 変更内容をファイルに保存します。


このコードで手動カメラ コントロールを実験とテストの簡単に実装することができます。

## <a name="manual-focus"></a>手動のフォーカス

フォーカスのコントロールを直接実行するエンドユーザーを許可すると、アプリケーションは、取得したイメージに美的センスの高い制御を提供できます。

プロ カメラマンが実現するためにイメージのフォーカスを和らげますなど、[ボケ効果](https://en.wikipedia.org/wiki/Bokeh):

[![](intro-to-manual-camera-controls-images/image2.png "ボケの効果")](intro-to-manual-camera-controls-images/image2.png#lightbox)

または、作成、[フォーカス プル効果](http://www.mediacollege.com/video/camera/focus/pull.html)など。

[![](intro-to-manual-camera-controls-images/image3.png "フォーカスのプルの影響")](intro-to-manual-camera-controls-images/image3.png#lightbox)

科学や医療アプリケーションのライター、アプリケーションがプログラムによって、レンズを実験に移動しようとする可能性があります。 どちらの方法でも、新しい API により、エンドユーザーまたはイメージにフォーカスに制御を実行するアプリケーションのいずれかを取得します。

### <a name="how-focus-works"></a>フォーカスの動作方法

前に、IOS 8 アプリケーションでのフォーカスを制御するための詳細について説明します。 IOS デバイスでのフォーカスのしくみの概要を見てみましょう。

[![](intro-to-manual-camera-controls-images/image4.png "IOS デバイスでのフォーカスのしくみ")](intro-to-manual-camera-controls-images/image4.png#lightbox)

ライトは、iOS デバイスでカメラのレンズを入力し、イメージのセンサーにフォーカスがあります。 レンズ、センサーからの距離は、中心点 (イメージを最も鮮明な表示領域) の機能、センサーとの関係を制御します。 レンズは、センサーから遠いほど、距離オブジェクトが明晰と思われるおよびオブジェクトの近く、近い見える鮮明です。

IOS デバイスでレンズが近い、または移動、センサーから離れた磁石および springs で。 その結果、レンズの正確な位置ことはできません、デバイスの向きやデバイス、spring の年齢などのパラメーターの影響を受けることができ、デバイスは異なります。

### <a name="important-focus-terms"></a>フォーカスの重要な用語

フォーカスを扱うときは、開発者の知識があるいくつかの用語があります。

-  **被写し界深度**– 最も近いとフォーカスのある最も遠いオブジェクト間の距離。 
-  **マクロ**-これがフォーカス範囲のほぼ最後とレンズを集中できる最も近い距離です。
-  **無限大**– フォーカス範囲の終点で、最も遠い距離をレンズを集中できます。
-  **Hyperfocal 距離**– フォーカスの末端だけで、フレームで最も遠いオブジェクトがフォーカス範囲内のポイントです。 つまり、これは、フィールドの深さを最大化焦点の位置です。 
-  **位置のレンズ**– 上記のすべてのコントロールはその他の条項。 これは、レンズとそれによって、センサーからの距離をコント ローラーのフォーカスです。


これらの用語とを考慮してナレッジを使用して、iOS 8 アプリケーションに新しい手動フォーカス コントロールが正常に実装することができます。

### <a name="existing-focus-controls"></a>既存のフォーカス コントロール

iOS 7、および以前のバージョンの提供を使用して既存のフォーカス コントロール`FocusMode`としてのプロパティ。

-   `AVCaptureFocusModeLocked` – フォーカスがフォーカスを 1 つの時点でロックされています。
-   `AVCaptureFocusModeAutoFocus` – カメラは、ピントが見つかるまで存在したまま、焦点距離のすべてのポイントでレンズをスイープします。
-   `AVCaptureFocusModeContinuousAutoFocus` – カメラは refocuses 焦点の状態を検出したときにします。


既存のコントロールを使用して目的の設定可能なポイントも提供されている、`FocusPointOfInterest`プロパティ、タップする特定の領域に重点を置くようにします。 アプリケーション監視 レンズの移動を追跡できますも、`IsAdjustingFocus`プロパティ。

さらに、範囲の制限は、によって提供された、`AutoFocusRangeRestriction`としてのプロパティ。

-   `AVCaptureAutoFocusRangeRestrictionNear` –、Autofocus を近くにある深さに制限されます。 QR コードまたはバーコードをスキャンなどの状況で便利です。
-   `AVCaptureAutoFocusRangeRestrictionFar` –、Autofocus を離れた深さに制限されます。 関連する認識されているオブジェクトはビューのフィールド (たとえば、ウィンドウ フレーム) である場合に便利です。


最後には、`SmoothAutoFocus`自動フォーカス アルゴリズムが低下し、手順をビデオを記録するときに移動するアイテムを回避するために増分値を小さくするプロパティ。

### <a name="new-focus-controls-in-ios-8"></a>IOS 8 での新しいフォーカス コントロール

Ios 7 以降を既に提供されている機能だけでなく、次の機能の対象は iOS 8 でのフォーカスの制御に使用できるようになりました。

-  フォーカスをロックする場合は、レンズ位置の完全な手動で制御します。
-  どのモードでもフォーカス レンズ位置のキーと値の監視。


上記の機能を実装するために、`AVCaptureDevice`読み取りのみを含めるクラスが変更された`LensPosition`プロパティは、カメラのレンズの現在位置を取得するために使用します。

レンズの位置の手動制御するには、ロックのフォーカス モードでキャプチャ デバイスがあります。 例:

 `CaptureDevice.FocusMode = AVCaptureFocusMode.Locked;`

`SetFocusModeLocked`キャプチャ デバイスのメソッドを使用して、カメラのレンズの位置を調整します。 変更を反映するときに通知を取得する、省略可能なコールバック ルーチンを提供できます。 例:

```csharp
ThisApp.CaptureDevice.LockForConfiguration(out Error);
ThisApp.CaptureDevice.SetFocusModeLocked(Position.Value,null);
ThisApp.CaptureDevice.UnlockForConfiguration();
```

上記のコードで示すようは、レンズの位置の変更ができる前に、構成のキャプチャ デバイスをロックする必要があります。 レンズの位置の有効な値では、0.0 ~ 1.0 の範囲です。

### <a name="manual-focus-example"></a>手動のフォーカスの例

インプレースでの一般的な AV キャプチャ セットアップ コード、`UIViewController`アプリケーションのストーリー ボードに追加し、次のように構成されていることができます。

[![](intro-to-manual-camera-controls-images/image5.png "UIViewController をストーリー ボードのアプリケーションに追加し、次に示すように構成されています。")](intro-to-manual-camera-controls-images/image5.png#lightbox)

ビューには、次の主な要素が含まれています。

-  A`UIImageView`ビデオ フィードが表示されます。
-  A`UISegmentedControl`ロックを自動からフォーカス モードを変更します。
-  A`UISlider`する方法を紹介してレンズの現在の位置を更新します。


手動フォーカス コントロールのビュー コント ローラーをワイヤ アップするには、次の操作を行います。


1. 次の追加ステートメントを使用します。

    ```csharp
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using System.Timers;
    ```  
  
1. 次のプライベート変数を追加します。

    ```csharp
    #region Private Variables
    private NSError Error;
    private bool Automatic = true;
    #endregion
    ```  
  
1. 次の計算されたプロパティを追加します。

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```  
  
1. オーバーライド、`ViewDidLoad`メソッドを次のコードを追加します。

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
    
        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;
    
        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;
    
        // Create a timer to monitor and update the UI
        SampleTimer = new Timer (5000);
        SampleTimer.Elapsed += (sender, e) => {
            // Update position slider
            Position.BeginInvokeOnMainThread(() =>{
                Position.Value = ThisApp.Input.Device.LensPosition;
            });
        };
    
        // Watch for value changes
        Segments.ValueChanged += (object sender, EventArgs e) => {
    
            // Lock device for change
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
    
            // Take action based on the segment selected
            switch(Segments.SelectedSegment) {
            case 0:
                // Activate auto focus and start monitoring position
                Position.Enabled = false;
                ThisApp.CaptureDevice.FocusMode = AVCaptureFocusMode.ContinuousAutoFocus;
                SampleTimer.Start();
                Automatic = true;
                break;
            case 1:
                // Stop auto focus and allow the user to control the camera
                SampleTimer.Stop();
                ThisApp.CaptureDevice.FocusMode = AVCaptureFocusMode.Locked;
                Automatic = false;
                Position.Enabled = true;
                break;
            }
    
            // Unlock device
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    
        // Monitor position changes
        Position.ValueChanged += (object sender, EventArgs e) => {
    
            // If we are in the automatic mode, ignore changes
            if (Automatic) return;
    
            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.SetFocusModeLocked(Position.Value,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    }
    ```  
  
1. 上書き、`ViewDidAppear`メソッドとビューが読み込まれたらの記録を開始するには、次の追加。

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);
    
        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;
    
            ThisApp.Session.StartRunning ();
            SampleTimer.Start ();
        }
    }
    ```  
  
1. Auto モードでカメラで、スライダーが移動に自動的にように、カメラにフォーカスが調整されます。

    [![](intro-to-manual-camera-controls-images/image6.png "カメラを調整します。 このサンプル アプリでフォーカスとスライダーが自動的に移動します。")](intro-to-manual-camera-controls-images/image6.png#lightbox)
1. ロック済みのセグメントをタップし、レンズの位置を手動で調整する位置のスライダーをドラッグします。

    [![](intro-to-manual-camera-controls-images/image7.png "レンズの位置を手動で調整します。")](intro-to-manual-camera-controls-images/image7.png#lightbox)
1. アプリケーションを停止します。


上記のコードでは、カメラが自動モードの場合は、レンズの位置を監視またはスライダーを使用してロック モードである場合に、レンズの位置を制御する方法を説明しました。

## <a name="manual-exposure"></a>手動の公開

危険度を使用して、ソースの明るさを基準とした画像の明るさは、光の量によって、センサーが、方法がヒットに期間、およびセンサー (ISO のマッピング) のゲイン レベルによって決定されます。 、露出を手動で制御を提供し、アプリケーションが自由度の高いをエンドユーザーに提供し、図案化された外観の実現を可能にできます。

手動の露出コントロールを使用して、ユーザーは濃いをムーディ非現実的明るいからイメージを実行できます。

[![](intro-to-manual-camera-controls-images/image8.png "サンプルに暗色、ムーディ非現実的明るいからのリスクを示すイメージ")](intro-to-manual-camera-controls-images/image8.png#lightbox)

ここでも、これ行う科学アプリケーションまたはアプリケーションのユーザー インターフェイスで提供される手動のコントロールを使用してプログラムで制御が自動的に使用します。 どちらの方法でも、新しい iOS 8 の公開 Api を提供カメラの危険度の設定をきめ細かく制御。

### <a name="how-exposure-works"></a>危険度のしくみ

前に、IOS 8 アプリケーションの公開の管理の詳細について説明します。 危険度のしくみの概要を見てみましょう。

[![](intro-to-manual-camera-controls-images/image9.png "危険度のしくみ")](intro-to-manual-camera-controls-images/image9.png#lightbox)

危険度の制御を 3 つの基本的な要素は次のとおりです。

-  **シャッター スピード**– これは、シャッターはカメラのセンサーに光を開いている時間の長さ。 短いシャッターを開くと、時間、小さいライトが、let と、鮮明な画像が (モーション不鮮明)。 シャッターが開いている長く、ぼかしに発生するがよりモーションとで多くの光ができるようにします。
-  **ISO マッピング**– これはフィルム写真からの借用語であり、光を映画では、化学薬品の感度を参照します。 少ない粒度と色の再現をさらに細かく; フィルムの短い ISO 値があります。デジタル センサーの ISO の不足値がある明るさを下げるがセンサー ノイズが軽減します。 ISO の値が大きいほど、イメージが明るくなりますが、多くのセンサー ノイズとします。 "ISO"デジタル センサーでは、メジャーの[電子ゲイン](https://en.wikipedia.org/wiki/Gain)、物理機能ではありません。 
-  **レンズの絞り**– これはレンズの開口部のサイズです。 すべての iOS デバイスのレンズの絞りは固定され、露出の調整に使用できる 2 つの値はシャッター スピードと ISO です。


### <a name="how-continuous-auto-exposure-works"></a>継続的な自動公開動作

学習する前に、手動の危険度のしくみが適切な継続的な自動露出を理解することは、iOS デバイスで動作します。

[![](intro-to-manual-camera-controls-images/image10.png "IOS デバイスで継続的な自動露出のしくみ")](intro-to-manual-camera-controls-images/image10.png#lightbox)

まず、自動公開ブロックは、理想的なエクスポージャを計算するジョブを持ち、統計の使用状況測定が継続的に給紙されています。この情報を使っても点灯シーンを取得するには、ISO とシャッター スピードの最適な組み合わせを計算します。 このサイクルは"AE ループ"と呼ばれます。

### <a name="how-locked-exposure-works"></a>どのロックされている公開動作

次に、iOS デバイスで公開動作をロックする方法を見てみましょう。

[![](intro-to-manual-camera-controls-images/image11.png "露出が iOS デバイスではロックされている方法")](intro-to-manual-camera-controls-images/image11.png#lightbox)

ここでも、最適な iOS と期間の値を計算しようとする自動公開ブロックがあります。 ただし、このモードでは、AE ブロックは、統計の使用状況測定エンジンから切断されます。

### <a name="existing-exposure-controls"></a>既存の露出コントロール

iOS 7 以上を使用して、次の既存の公開コントロールを提供し、`ExposureMode`プロパティ。

-   `AVCaptureExposureModeLocked` – シーンを 1 回サンプリングし、シーン全体でこれらの値を使用します。
-   `AVCaptureExposureModeContinuousAutoExposure` – はも点灯していることを確認するには、継続的にシーンをサンプリングします。


`ExposurePointOfInterest`タップすると、公開先のオブジェクトを選択して、シーンを公開に使用できるし、アプリケーションを監視できる、`AdjustingExposure`プロパティを公開が調整されている場合を参照してください。

### <a name="new-exposure-controls-in-ios-8"></a>IOS 8 での新しい公開コントロール

Ios 7 以降を既に提供されている機能だけでなく、次の機能では iOS 8 での露出の制御に使用できるようになりました。

-  完全に手動のカスタム公開します。
-  Get、Set、キーと値は、IOS およびシャッター スピード (期間) を確認します。


上記の機能を実装するために、新しい`AVCaptureExposureModeCustom`モードが追加されています。 カメラにカスタムのモードがある場合、危険度の有効期間と ISO を調整する、次のコードを使用できます。

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.LockExposure(DurationValue,ISOValue,null);
CaptureDevice.UnlockForConfiguration();
```

アプリケーションは、自動およびロックのモードで、次のコードを使用して自動的に公開しルーチンのバイアスを調整できます。

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.SetExposureTargetBias(Value,null);
CaptureDevice.UnlockForConfiguration();
```

設定の最小値と最大範囲は、する必要がありますしないするハード コーディングされているため、アプリケーションが実行されているデバイスに依存します。 代わりに、次のプロパティを使用して、最小および最大値の範囲の取得します。

-   `CaptureDevice.MinExposureTargetBias` 
-   `CaptureDevice.MaxExposureTargetBias` 
-   `CaptureDevice.ActiveFormat.MinISO` 
-   `CaptureDevice.ActiveFormat.MaxISO` 
-   `CaptureDevice.ActiveFormat.MinExposureDuration` 
-   `CaptureDevice.ActiveFormat.MaxExposureDuration` 


上記のコードで示すようは、危険度の変更ができる前に、構成のキャプチャ デバイスをロックする必要があります。

### <a name="manual-exposure-example"></a>手動の危険度の例

インプレースでの一般的な AV キャプチャ セットアップ コード、`UIViewController`アプリケーションのストーリー ボードに追加し、次のように構成されていることができます。

[![](intro-to-manual-camera-controls-images/image12.png "UIViewController をストーリー ボードのアプリケーションに追加し、次に示すように構成されています。")](intro-to-manual-camera-controls-images/image12.png#lightbox)

ビューには、次の主な要素が含まれています。

-  A`UIImageView`ビデオ フィードが表示されます。
-  A`UISegmentedControl`ロックを自動からフォーカス モードを変更します。
-  次の 4 つ`UISlider`コントロールを表示すると、オフセット、期間、ISO およびバイアスを更新します。


手動の露出の調整のビュー コント ローラーをワイヤ アップするには、次の操作を行います。


1. 次の追加ステートメントを使用します。
    
    ```
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using System.Timers;
    ```  
  
1. 次のプライベート変数を追加します。

    ```csharp
    #region Private Variables
    private NSError Error; 
    private bool Automatic = true;
    private nfloat ExposureDurationPower = 5;
    private nfloat ExposureMinimumDuration = 1.0f/1000.0f;
    #endregion
    ```  
  
1. 次の計算されたプロパティを追加します。

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```  
  
1. オーバーライド、`ViewDidLoad`メソッドを次のコードを追加します。

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
    
        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;
    
        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;
    
        // Set min and max values
        Offset.MinValue = ThisApp.CaptureDevice.MinExposureTargetBias;
        Offset.MaxValue = ThisApp.CaptureDevice.MaxExposureTargetBias;
    
        Duration.MinValue = 0.0f;
        Duration.MaxValue = 1.0f;
    
        ISO.MinValue = ThisApp.CaptureDevice.ActiveFormat.MinISO;
        ISO.MaxValue = ThisApp.CaptureDevice.ActiveFormat.MaxISO;
    
        Bias.MinValue = ThisApp.CaptureDevice.MinExposureTargetBias;
        Bias.MaxValue = ThisApp.CaptureDevice.MaxExposureTargetBias;
    
        // Create a timer to monitor and update the UI
        SampleTimer = new Timer (5000);
        SampleTimer.Elapsed += (sender, e) => {
            // Update position slider
            Offset.BeginInvokeOnMainThread(() =>{
                Offset.Value = ThisApp.Input.Device.ExposureTargetOffset;
            });
    
            Duration.BeginInvokeOnMainThread(() =>{
                var newDurationSeconds = CMTimeGetSeconds(ThisApp.Input.Device.ExposureDuration);
                var minDurationSeconds = Math.Max(CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MinExposureDuration), ExposureMinimumDuration);
                var maxDurationSeconds = CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MaxExposureDuration);
                var p = (newDurationSeconds - minDurationSeconds) / (maxDurationSeconds - minDurationSeconds);
                Duration.Value = (float)Math.Pow(p, 1.0f/ExposureDurationPower);
            });
    
            ISO.BeginInvokeOnMainThread(() => {
                ISO.Value = ThisApp.Input.Device.ISO;
            });
    
            Bias.BeginInvokeOnMainThread(() => {
                Bias.Value = ThisApp.Input.Device.ExposureTargetBias;
            });
        };
    
        // Watch for value changes
        Segments.ValueChanged += (object sender, EventArgs e) => {
    
            // Lock device for change
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
    
            // Take action based on the segment selected
            switch(Segments.SelectedSegment) {
            case 0:
                // Activate auto exposure and start monitoring position
                Duration.Enabled = false;
                ISO.Enabled = false;
                ThisApp.CaptureDevice.ExposureMode = AVCaptureExposureMode.ContinuousAutoExposure;
                SampleTimer.Start();
                Automatic = true;
                break;
            case 1:
                // Lock exposure and allow the user to control the camera
                SampleTimer.Stop();
                ThisApp.CaptureDevice.ExposureMode = AVCaptureExposureMode.Locked;
                Automatic = false;
                Duration.Enabled = false;
                ISO.Enabled = false;
                break;
            case 2:
                // Custom exposure and allow the user to control the camera
                SampleTimer.Stop();
                ThisApp.CaptureDevice.ExposureMode = AVCaptureExposureMode.Custom;
                Automatic = false;
                Duration.Enabled = true;
                ISO.Enabled = true;
                break;
            }
    
            // Unlock device
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    
        // Monitor position changes
        Duration.ValueChanged += (object sender, EventArgs e) => {
    
            // If we are in the automatic mode, ignore changes
            if (Automatic) return;
    
            // Calculate value
            var p = Math.Pow(Duration.Value,ExposureDurationPower);
            var minDurationSeconds = Math.Max(CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MinExposureDuration),ExposureMinimumDuration);
            var maxDurationSeconds = CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MaxExposureDuration);
            var newDurationSeconds = p * (maxDurationSeconds - minDurationSeconds) +minDurationSeconds;
    
            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.LockExposure(CMTime.FromSeconds(p,1000*1000*1000),ThisApp.CaptureDevice.ISO,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    
        ISO.ValueChanged += (object sender, EventArgs e) => {
    
            // If we are in the automatic mode, ignore changes
            if (Automatic) return;
    
            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.LockExposure(ThisApp.CaptureDevice.ExposureDuration,ISO.Value,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    
        Bias.ValueChanged += (object sender, EventArgs e) => {
    
            // If we are in the automatic mode, ignore changes
            // if (Automatic) return;
    
            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.SetExposureTargetBias(Bias.Value,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    }
    ```  
  
1. 上書き、`ViewDidAppear`メソッドとビューが読み込まれたらの記録を開始するには、次の追加。

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);
    
        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;
    
            ThisApp.Session.StartRunning ();
            SampleTimer.Start ();
        }
    }
    ```  
  
1. Auto モードでカメラで、スライダーはカメラの露出の調整と自動的に移動されます。

    [![](intro-to-manual-camera-controls-images/image13.png "カメラの露出の調整とスライダーが自動的に移動します。")](intro-to-manual-camera-controls-images/image13.png#lightbox)
1. ロック済みのセグメントをタップし、バイアス スライダーを自動公開のバイアスを手動で調整します。

    [![](intro-to-manual-camera-controls-images/image14.png "自動の露出のバイアスを手動で調整します。")](intro-to-manual-camera-controls-images/image14.png#lightbox)
1. カスタムのセグメントをタップし、露出を手動で制御する期間と ISO のスライダーをドラッグします。

    [![](intro-to-manual-camera-controls-images/image15.png "期間と ISO スライダーをドラッグして、手動で公開を制限する")](intro-to-manual-camera-controls-images/image15.png#lightbox)
1. アプリケーションを停止します。


上記のコードが、カメラが自動のモードでは、危険度の設定を監視する方法を表示し、スライダーを使用して、ロック済みまたはカスタム モードである場合に、公開を制御する方法。

## <a name="manual-white-balance"></a>手動ホワイト バランス

ホワイト バランス コントロールより現実的な見えるように、イメージの colosr のバランスを調整できます。 異なる光源がある別の色温度、およびこれらの違いを補正するためにイメージをキャプチャするために使用するカメラの設定を調整する必要があります。 ここでも、ホワイト バランス経由でユーザー コントロールを許可することで、自動のルーチンでは、対応していないの芸術の効果を実現する professional の調整を行いますができます。

[![](intro-to-manual-camera-controls-images/image16.png "手動ホワイト バランス調整を示すサンプル画像")](intro-to-manual-camera-controls-images/image16.png#lightbox)

たとえば、夏時間が、tungsten 白熱ライトがあるホット、黄、オレンジ色の濃淡思い描いてキャスト。 (紛らわしいかもしれませんが、「クール」色が、「ウォーム」色よりも高い色温度があります。 色温度は、物理メジャーを 1 つは知覚されません)。

人間に注意してくださいは色温度、違いに補正で非常に良好なが、これは、カメラができないもの。 色の違いを調整する、反対側の領域の色を混ぜて、カメラが動作します。

新しい iOS 8 の公開 API は、プロセスの制御を受け取って、カメラのホワイト バランス設定をきめ細かく制御を提供するアプリケーションを使用できます。

### <a name="how-white-balance-works"></a>白色の分散のしくみ

前に、IOS 8 アプリケーションのホワイト バランスを制御するための詳細について説明します。 白色の分散の動作を簡単に見てをみましょう。

色の知覚の調査、 [CIE 1931 RGB 色空間と CIE 1931 XYZ 色空間](https://en.wikipedia.org/wiki/CIE_1931_color_space)は最初は数学的に色空間を定義します。 1931 で照明 (CIE) 国際委員会によって作成されました。

[![](intro-to-manual-camera-controls-images/image17.png "CIE 1931 RGB 色空間と CIE 1931 XYZ 色空間")](intro-to-manual-camera-controls-images/image17.png#lightbox)

上記のグラフが表示されますすべて色の濃い青を明るい赤に明るい緑から人間の目に表示されます。 上のグラフで示されている、X と Y の値を図上の任意のポイントをプロットすることができます。

としてグラフに表示される、人間のビジョンの範囲外になるグラフにプロットできる X と Y の値があるし、その結果、これらの色は、カメラで再現できません。

上記の表では、小規模な曲線と呼ばれる、 [Planckian たどった](https://en.wikipedia.org/wiki/Planckian_locus)、(暑)、青の側で大きな数値の度ケルビンの場合) の「色温度を表現する赤色の側 (冷却器) 番号が低い。 これらは、一般的なライティングの場合に便利です。

混合照明条件では、ホワイト バランス調整が必要な変更を行う Planckian たどったから逸脱する必要があります。 調整は必要があります。 このような場合に、緑のいずれかに移動または、CIE の赤/赤紫側スケールできます。

iOS デバイスでは、反対側の色のゲインを混ぜてカラー キャストを補正します。 たとえば、シーンの青が多すぎる場合は、補正するために赤のゲインがブーストされます。 デバイスに依存しているので、値が特定のデバイスに合わせて調整されたこれらの向上。

### <a name="existing-white-balance-controls"></a>ホワイト バランスの既存のコントロール

iOS 7 以降を使用して次のような既存ホワイト バランス コントロールを提供および`WhiteBalanceMode`プロパティ。

-   `AVCapture WhiteBalance ModeLocked` – 1 回のシーンとシーン全体でこれらの値を使用してサンプリングします。
-   `AVCapture WhiteBalance ModeContinuousAutoExposure` – 分散もあることを確認するには、継続的にシーンをサンプリングします。


アプリケーションを監視し、`AdjustingWhiteBalance`プロパティを公開が調整されている場合を参照してください。

### <a name="new-white-balance-controls-in-ios-8"></a>IOS 8 での新しいホワイト バランス コントロール

に加えて、ios 7 以降を既に提供されている機能の次の機能は iOS 8 では、コントロール ホワイト バランスで使用できるようになりました。

-  デバイスの RGB の完全に手動で制御を取得します。
-  Get、Set、キーと値を観察 RGB のデバイスを取得します。
-  灰色のカードを使用してホワイト バランスをサポートします。
-  デバイスの独立した色空間との間の変換ルーチン。


上記の機能を実装するために、`AVCaptureWhiteBalanceGain`構造体は、次のメンバーが追加されています。

-   `RedGain` 
-   `GreenGain` 
-   `BlueGain` 


最大ホワイト バランス ゲインは現在、4 (4) から利用できると、`MaxWhiteBalanceGain`プロパティ。 有効な範囲は 1 つ (1) からように`MaxWhiteBalanceGain`(4) 現在します。

`DeviceWhiteBalanceGains`現在の値を確認するプロパティを使用できます。 使用`SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains`バランスを調整する、カメラがロックされているホワイト バランス モードを取得します。

#### <a name="conversion-routines"></a>変換ルーチン

IOS 8、およびからの変換を支援する変換ルーチンが追加されているデバイスの独立した色空間。 変換ルーチンを実装するために、`AVCaptureWhiteBalanceChromaticityValues`構造体は、次のメンバーが追加されています。

-   `X` 0 から 1 の値です。
-   `Y` 0 から 1 の値です。


`AVCaptureWhiteBalanceTemperatureAndTintValues`構造体は、次のメンバーでも追加されました。

-   `Temperature` -は、浮動小数点ケルビンで値。
-   `Tint` -緑または緑色の方向に向けての正の値を持つ 150 に 0 からマゼンタおよびマゼンタの負方向からのオフセットです。


使用して、`CaptureDevice.GetTemperatureAndTintValues`と`CaptureDevice.GetDeviceWhiteBalanceGains`温度、濃淡、一番および RGB の間で変換するメソッドは、色空間を取得します。

> [!NOTE]
> 変換ルーチンより正確な Planckian たどったに変換する値が近づきます。




#### <a name="gray-card-support"></a>灰色のカードのサポート

Apple では、灰色の世界の用語を使用して、iOS 8 に組み込まれているグレーのカードのサポートを参照してください。 ユーザーが、フレームの中心の少なくとも 50% をカバーしを使用してホワイト バランスの調整を物理灰色カードに集中できます。 灰色のカードの目的は、ニュートラル表示される空白を実現するためにです。

これにより、カメラの前にグレー物理カードを配置するように求めるアプリケーションで実装できます監視、`GrayWorldDeviceWhiteBalanceGains`プロパティと値に落ち着くまで待機しています。

アプリケーションは、ホワイト バランスの向上をロックし、`SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains`メソッドから値を使用して、`GrayWorldDeviceWhiteBalanceGains`変更を適用するプロパティ。

ホワイト バランスの変更を行う前に、構成のキャプチャ デバイスをロックする必要があります。

### <a name="manual-white-balance-example"></a>手動ホワイト バランスの例

インプレースでの一般的な AV キャプチャ セットアップ コード、`UIViewController`アプリケーションのストーリー ボードに追加し、次のように構成されていることができます。

[![](intro-to-manual-camera-controls-images/image18.png "UIViewController をストーリー ボードのアプリケーションに追加し、次に示すように構成されています。")](intro-to-manual-camera-controls-images/image18.png#lightbox)

ビューには、次の主な要素が含まれています。

-  A`UIImageView`ビデオ フィードが表示されます。
-  A`UISegmentedControl`ロックを自動からフォーカス モードを変更します。
-  2 つ`UISlider`コントロールが表示され、温度、濃淡が更新されます。
-  A`UIButton`灰色カード (灰色 World) 領域をサンプリングし、それらの値を使用してホワイト バランスを設定するために使用します。


手動ホワイト バランス コントロールのビュー コント ローラーをワイヤ アップするには、次の操作を行います。


1. 次の追加ステートメントを使用します。

    ```csharp
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using System.Timers;
    ```  
  
1. 次のプライベート変数を追加します。

    ```csharp
    #region Private Variables
    private NSError Error; 
    private bool Automatic = true;
    #endregion
    ```
  
1. 次の計算されたプロパティを追加します。

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```  
  
1. 新しいホワイトを設定する次のプライベート メソッドを追加する温度、濃淡のバランスを取る。

    ```csharp
    #region Private Methods
    void SetTemperatureAndTint() {
        // Grab current temp and tint
        var TempAndTint = new AVCaptureWhiteBalanceTemperatureAndTintValues (Temperature.Value, Tint.Value);

        // Convert Color space
        var gains = ThisApp.CaptureDevice.GetDeviceWhiteBalanceGains (TempAndTint);

        // Set the new values
        if (ThisApp.CaptureDevice.LockForConfiguration (out Error)) {
            gains = NomralizeGains (gains);
            ThisApp.CaptureDevice.SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains (gains, null);
            ThisApp.CaptureDevice.UnlockForConfiguration ();
        }
    }
    
    AVCaptureWhiteBalanceGains NomralizeGains (AVCaptureWhiteBalanceGains gains)
    {
        gains.RedGain = Math.Max (1, gains.RedGain);
        gains.BlueGain = Math.Max (1, gains.BlueGain);
        gains.GreenGain = Math.Max (1, gains.GreenGain);

        float maxGain = ThisApp.CaptureDevice.MaxWhiteBalanceGain;
        gains.RedGain = Math.Min (maxGain, gains.RedGain);
        gains.BlueGain = Math.Min (maxGain, gains.BlueGain);
        gains.GreenGain = Math.Min (maxGain, gains.GreenGain);

        return gains;
    }
    #endregion
    ```   
  
1. オーバーライド、`ViewDidLoad`メソッドを次のコードを追加します。

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;

        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;

        // Set min and max values
        Temperature.MinValue = 1000f;
        Temperature.MaxValue = 10000f;

        Tint.MinValue = -150f;
        Tint.MaxValue = 150f;

        // Create a timer to monitor and update the UI
        SampleTimer = new Timer (5000);
        SampleTimer.Elapsed += (sender, e) => {
            // Convert color space
            var TempAndTint = ThisApp.CaptureDevice.GetTemperatureAndTintValues (ThisApp.CaptureDevice.DeviceWhiteBalanceGains);

            // Update slider positions
            Temperature.BeginInvokeOnMainThread (() => {
                Temperature.Value = TempAndTint.Temperature;
            });

            Tint.BeginInvokeOnMainThread (() => {
                Tint.Value = TempAndTint.Tint;
            });
        };

        // Watch for value changes
        Segments.ValueChanged += (sender, e) => {
            // Lock device for change
            if (ThisApp.CaptureDevice.LockForConfiguration (out Error)) {

                // Take action based on the segment selected
                switch (Segments.SelectedSegment) {
                case 0:
                // Activate auto focus and start monitoring position
                    Temperature.Enabled = false;
                    Tint.Enabled = false;
                    ThisApp.CaptureDevice.WhiteBalanceMode = AVCaptureWhiteBalanceMode.ContinuousAutoWhiteBalance;
                    SampleTimer.Start ();
                    Automatic = true;
                    break;
                case 1:
                // Stop auto focus and allow the user to control the camera
                    SampleTimer.Stop ();
                    ThisApp.CaptureDevice.WhiteBalanceMode = AVCaptureWhiteBalanceMode.Locked;
                    Automatic = false;
                    Temperature.Enabled = true;
                    Tint.Enabled = true;
                    break;
                }

                // Unlock device
                ThisApp.CaptureDevice.UnlockForConfiguration ();
            }
        };

        // Monitor position changes
        Temperature.TouchUpInside += (sender, e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic)
                return;

            // Update white balance
            SetTemperatureAndTint ();
        };

        Tint.TouchUpInside += (sender, e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic)
                return;

            // Update white balance
            SetTemperatureAndTint ();
        };

        GrayCardButton.TouchUpInside += (sender, e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic)
                return;

            // Get gray card values
            var gains = ThisApp.CaptureDevice.GrayWorldDeviceWhiteBalanceGains;

            // Set the new values
            if (ThisApp.CaptureDevice.LockForConfiguration (out Error)) {
                ThisApp.CaptureDevice.SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains (gains, null);
                ThisApp.CaptureDevice.UnlockForConfiguration ();
            }
        };
    }
    ``` 
  
1. 上書き、`ViewDidAppear`メソッドとビューが読み込まれたらの記録を開始するには、次の追加。

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);
    
        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;
    
            ThisApp.Session.StartRunning ();
            SampleTimer.Start ();
        }
    }
    ```  
  
1. 変更を保存、コードをアプリケーションを実行します。
1. Auto モードでカメラで、スライダーはカメラ ホワイト バランスを調整すると自動的に移動されます。

    [![](intro-to-manual-camera-controls-images/image19.png "カメラ ホワイト バランスを調整すると、スライダーが自動的に移動します。")](intro-to-manual-camera-controls-images/image19.png#lightbox)
1. ロック済みのセグメントをタップし、ホワイト バランスを手動で調整する Temp および濃淡スライダーをドラッグします。

    [![](intro-to-manual-camera-controls-images/image20.png "ホワイト バランスを手動で調整する Temp および濃淡スライダーをドラッグします")](intro-to-manual-camera-controls-images/image20.png#lightbox)
1. ロック済みセグメントは、選択したまま、カメラの前面にグレー物理カードを配置し、その灰色の世界にホワイト バランスを調整する灰色のカード ボタンをタップします。

    [![](intro-to-manual-camera-controls-images/image21.png "灰色の世界にホワイト バランスを調整する灰色のカード ボタンをタップします。")](intro-to-manual-camera-controls-images/image21.png#lightbox)
1. アプリケーションを停止します。

上記のコードでは、カメラが自動モードの場合は、ホワイト バランス設定を監視またはロック モードである場合に、ホワイト バランスを制御するスライダーを使用する方法を説明しました。

## <a name="bracketed-capture"></a>かっこで囲まれたキャプチャ

囲まキャプチャでは、上で示した手動カメラ コントロールからの設定に基づいており、により、アプリケーションをさまざまな方法で、特定の時点をキャプチャできます。

簡単に言えば、さまざまな設定は、画像から画像を使用して作成した静止画のバーストは、角をキャプチャします。

[![](intro-to-manual-camera-controls-images/image22.png "角のキャプチャのしくみ")](intro-to-manual-camera-controls-images/image22.png#lightbox)

IOS 8 で囲まキャプチャを使用して、アプリケーションを一連の手動カメラ コントロールを事前設定、1 つのコマンドを発行して、手動のプリセットのそれぞれについて一連のイメージを返す現在のシーンがあります。

### <a name="bracketed-capture-basics"></a>かっこで囲まれたキャプチャの基礎

画像を画像からさまざまな設定で撮影された静止画像のバーストが、もう一度キャプチャ角です。 使用可能なキャプチャ角の種類があります。

-  **危険度の角かっこの自動**すべてのイメージがさまざまなバイアス量が。
-  **手動の露出ブラケット**– すべてのイメージが可変のシャッター スピード (期間) と ISO がある量。
-  **単純なバースト ブラケット**– 一連の連続で実行されるイメージもします。


### <a name="new-bracketed-capture-controls-in-ios-8"></a>新しい囲まキャプチャ内のコントロール iOS 8

角をキャプチャするすべてのコマンドが実装されている、`AVCaptureStillImageOutput`クラス。 使用して、`CaptureStillImageBracket`設定の指定した配列に一連のイメージを取得します。

設定を処理するために、2 つの新しいクラスが実装されました。

-   `AVCaptureAutoExposureBracketedStillImageSettings` – 1 つのプロパティを持つその`ExposureTargetBias`自動公開の角かっこのバイアスの設定に使用されます。 
-   `AVCaptureManual`  `ExposureBracketedStillImageSettings` – 2 つのプロパティを持つその`ExposureDuration`と`ISO`手動露出の角かっこのシャッター スピードと ISO 設定に使用されます。 


### <a name="bracketed-capture-controls-dos-and-donts"></a>かっこで囲まれたキャプチャすべきこととすべきでないことを制御します。

#### <a name="dos"></a>Do

IOS 8 でコントロールを囲むキャプチャを使用して際に行う必要がある事項の一覧を次には。

-  キャプチャの最悪の状況に呼び出すことによってアプリを準備する、`PrepareToCaptureStillImageBracket`メソッド。
-  サンプルのバッファーが同じ共有プールから取得しようとしていると仮定します。
-  準備の前の呼び出しで割り当てられたメモリを解放するには、呼び出す`PrepareToCaptureStillImageBracket`もう一度し、1 つのオブジェクトの配列を送信します。


#### <a name="donts"></a>すべきでないこと

IOS 8 でコントロールを囲むキャプチャを使用している場合の処理しない点の一覧を次には。

-  設定の種類、単一のキャプチャでキャプチャ角を混在させないでください。
-  要求しないで以上`MaxBracketedCaptureStillImageCount`単一のキャプチャ内のイメージ。


### <a name="bracketed-capture-details"></a>かっこで囲まれたキャプチャの詳細

IOS 8 で囲まキャプチャを使用する場合、考慮事項に、次の詳細を考慮する必要があります。

-  かっこで囲まれた設定を一時的に上書き、`AVCaptureDevice`設定します。
-  Flash とまだイメージ安定化の設定は無視されます。
-  すべてのイメージは、同じ出力形式 (jpeg、png など) を使用する必要があります。
-  ビデオのプレビューでは、フレームを削除できます。
-  かっこで囲まれたキャプチャは、iOS 8 と互換性のあるすべてのデバイスでサポートされます。


この情報に注意してくださいでは、iOS 8 で囲まキャプチャを使用する例を見てをみましょう。

### <a name="bracket-capture-example"></a>角かっこのキャプチャの例

インプレースでの一般的な AV キャプチャ セットアップ コード、`UIViewController`アプリケーションのストーリー ボードに追加し、次のように構成されていることができます。

[![](intro-to-manual-camera-controls-images/image23.png "UIViewController をストーリー ボードのアプリケーションに追加し、次に示すように構成されています。")](intro-to-manual-camera-controls-images/image23.png#lightbox)

ビューには、次の主な要素が含まれています。

-  A`UIImageView`ビデオ フィードが表示されます。
-  次の 3 つ`UIImageViews`をキャプチャの結果が表示されます。
-  A`UIScrollView`ビデオ フィードと結果ビューを格納するためです。
-  A`UIButton`いくつかの既定の設定で囲まキャプチャを実行するために使用します。


ワイヤ アップ ビュー コント ローラーの角をキャプチャするには、次の操作を行います。


1. 次の追加ステートメントを使用します。

    ```csharp
    using System;
    using System.Drawing;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using CoreImage;
    ```  
  
1. 次のプライベート変数を追加します。

    ```csharp
    #region Private Variables
    private NSError Error;
    private List<UIImageView> Output = new List<UIImageView>();
    private nint OutputIndex = 0;
    #endregion
    ```    
  
1. 次の計算されたプロパティを追加します。

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    #endregion
    ```  
  
1. 必要な出力のイメージ ビューを作成する次のプライベート メソッドを追加します。

    ```csharp
    #region Private Methods
    private UIImageView BuildOutputView(nint n) {
    
        // Create a new image view controller
        var imageView = new UIImageView (new CGRect (CameraView.Frame.Width * n, 0, CameraView.Frame.Width, CameraView.Frame.Height));
    
        // Load a temp image
        imageView.Image = UIImage.FromFile ("Default-568h@2x.png");
    
        // Add a label
        UILabel label = new UILabel (new CGRect (0, 20, CameraView.Frame.Width, 24));
        label.TextColor = UIColor.White;
        label.Text = string.Format ("Bracketed Image {0}", n);
        imageView.AddSubview (label);
    
        // Add to scrolling view
        ScrollView.AddSubview (imageView);
    
        // Return new image view
        return imageView;
    }
    #endregion
    ```  
  
1. オーバーライド、`ViewDidLoad`メソッドを次のコードを追加します。
    
    ```
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
    
        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;
    
        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;
    
        // Setup scrolling area
        ScrollView.ContentSize = new SizeF (CameraView.Frame.Width * 4, CameraView.Frame.Height);
    
        // Add output views
        Output.Add (BuildOutputView (1));
        Output.Add (BuildOutputView (2));
        Output.Add (BuildOutputView (3));
    
        // Create preset settings
        var Settings = new AVCaptureBracketedStillImageSettings[] {
            AVCaptureAutoExposureBracketedStillImageSettings.Create(-2.0f),
            AVCaptureAutoExposureBracketedStillImageSettings.Create(0.0f),
            AVCaptureAutoExposureBracketedStillImageSettings.Create(2.0f)
        };
    
        // Wireup capture button
        CaptureButton.TouchUpInside += (sender, e) => {
            // Reset output index
            OutputIndex = 0;
    
            // Tell the camera that we are getting ready to do a bracketed capture
            ThisApp.StillImageOutput.PrepareToCaptureStillImageBracket(ThisApp.StillImageOutput.Connections[0],Settings,async (bool ready, NSError err) => {
                // Was there an error, if so report it
                if (err!=null) {
                    Console.WriteLine("Error: {0}",err.LocalizedDescription);
                }
            });
    
            // Ask the camera to snap a bracketed capture
            ThisApp.StillImageOutput.CaptureStillImageBracket(ThisApp.StillImageOutput.Connections[0],Settings, (sampleBuffer, settings, err) =>{
                // Convert raw image stream into a Core Image Image
                var imageData = AVCaptureStillImageOutput.JpegStillToNSData(sampleBuffer);
                var image = CIImage.FromData(imageData);
    
                // Display the resulting image
                Output[OutputIndex++].Image = UIImage.FromImage(image);
    
                // IMPORTANT: You must release the buffer because AVFoundation has a fixed number
                // of buffers and will stop delivering frames if it runs out.
                sampleBuffer.Dispose();
            });
        };
    }
    ```
    
  
1. オーバーライド、`ViewDidAppear`メソッドを次のコードを追加します。

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);
    
        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;
    
            ThisApp.Session.StartRunning ();
        }
    }
    
    ```  
    
1. 変更を保存、コードをアプリケーションを実行します。
1. フレーム シーンをし、キャプチャの角かっこボタンをタップします。

    [![](intro-to-manual-camera-controls-images/image24.png "シーンのフレームし、キャプチャの角かっこボタンをタップします。")](intro-to-manual-camera-controls-images/image24.png#lightbox)
1. 右から左角のキャプチャされた 3 つのイメージを表示するにスワイプします。

    [![](intro-to-manual-camera-controls-images/image25.png "スワイプ右から左に囲まキャプチャされた 3 つのイメージを参照してください。")](intro-to-manual-camera-controls-images/image25.png#lightbox)
1. アプリケーションを停止します。


上記のコードでは、構成、および iOS 8 で、自動公開囲まキャプチャを実行する方法を説明しました。

## <a name="summary"></a>まとめ

この記事では iOS 8 で提供される新しい手動カメラ コントロールの概要を説明し、その実行内容とそのしくみの基礎を説明しました。 手動のフォーカス、手動露出と手動ホワイト バランスの例を指定します。 最後に、例、角を使用してキャプチャ、既に説明した手動カメラ コントロールを紹介しました

## <a name="related-links"></a>関連リンク

- [ManualCameraControls (サンプル)](https://developer.xamarin.com/samples/monotouch/ManualCameraControls)
- [iOS 8 の概要](~/ios/platform/introduction-to-ios8.md)
