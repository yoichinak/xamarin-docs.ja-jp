---
title: 手動のカメラのコントロール
description: AVFoundation Framework やすくよりも優れた写真が撮影することによって手動カメラ コントロールに対するユーザーのことです。 アプリケーションは、このフレームワークを使用して、カメラのフォーカス、ホワイト バランス、および公開の設定を直接制御を実行できます。 アプリケーションでは、自動的にイメージをキャプチャする別の危険度の設定でもかっこで囲まれた露出のキャプチャを使用できます。 この記事の内容を簡単にコントロールを使用して、手動カメラ簡単な iOS 8 のモバイル アプリケーションで見てとなります。
ms.prod: xamarin
ms.assetid: 56340225-5F3C-4BFC-9A79-61496D7FE5B5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 8545dce1b9232e396c4c9e71ad5f20649eef2417
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="manual-camera-controls"></a>手動のカメラのコントロール

_AVFoundation Framework やすくよりも優れた写真が撮影することによって手動カメラ コントロールに対するユーザーのことです。アプリケーションは、このフレームワークを使用して、カメラのフォーカス、ホワイト バランス、および公開の設定を直接制御を実行できます。アプリケーションでは、自動的にイメージをキャプチャする別の危険度の設定でもかっこで囲まれた露出のキャプチャを使用できます。この記事の内容を簡単にコントロールを使用して、手動カメラ簡単な iOS 8 のモバイル アプリケーションで見てとなります。_

提供されるマニュアル カメラ コントロール、 `AVFoundation Framework` 8、iOS でモバイル アプリケーションを iOS デバイスのカメラを完全に制御を許可します。 プロフェッショナルなレベルのカメラ アプリケーションを作成し、静止画像またはビデオを作成中に、カメラのパラメーターを調整してアーティスト コンポジションを指定するのには、この制御の粒度の細かいレベルを使用できます。

結果が正しいかどうかや、画像のビューティ ・に特化小さいと、一部の機能や行われているイメージの要素を強調表示により特化、科学的または産業用のアプリケーションを開発するときにでもこれらのコントロールできますも役立ちます。

## <a name="avfoundation-capture-objects"></a>AVFoundation キャプチャ オブジェクト

ビデオや静止 iOS デバイスでカメラを使用してイメージを使用すると、かどうかそれらのイメージをキャプチャするために使用するプロセス、ほぼ同じです。 既定の自動カメラのコントロールまたは新規の手動カメラの制御を利用したもの使用するアプリケーションの場合は true。 次に示します。

 [![](intro-to-manual-camera-controls-images/image1.png "AVFoundation キャプチャ オブジェクトの概要")](intro-to-manual-camera-controls-images/image1.png#lightbox)

入力がから取得した、`AVCaptureDeviceInput`に、`AVCaptureSession`によって、`AVCaptureConnection`です。 静止画像またはビデオをストリームとして、出力になります。 プロセス全体がによって制御されます、`AVCaptureDevice`です。

## <a name="manual-controls-provided"></a>用意されている手動のコントロール

IOS 8 で提供される新しい Api を使用して、アプリケーションは、次のカメラ機能の制御を実行できます。

-  **手動フォーカス**– フォーカスのコントロールを直接実行するエンドユーザーを許可すると、アプリケーションが撮影されたより詳細に制御を提供できます。
-  **手動露出**– 露出を手動で制御を提供するアプリケーションはより自由をユーザーに提供し、定型の外観の実現を可能にします。
-  **手動ホワイト バランス**– ホワイト バランスが画像の色の調整に使用されます: 現実的な外観にすることには、多くの場合。 異なる光源は別の色温度を持ち、これらの相違点を補正するイメージをキャプチャするために使用するカメラの設定を調整します。 ここでも、ユーザーを制御するホワイト バランスを許可すると、ユーザー、調整できますを自動的に実行することはできません。


iOS 8 は、拡張機能を提供し、既存の iOS をこのイメージに対して、きめ細かい制御を提供する Api の機能強化がプロセスをキャプチャします。

## <a name="bracketed-capture"></a>かっこ付きのキャプチャ

角かっこのキャプチャは上に示した手動カメラのコントロールの設定に基づいており、により、アプリケーションのさまざまな方法で、特定の時点をキャプチャします。

簡単に言うと、さまざまな画像に画像の設定で撮影された画像を静止のバーストは、角かっこをキャプチャします。

## <a name="requirements"></a>要件

次がこの記事の手順を実行する必要です。

-  **Xcode 7 + および iOS 8 以降**– Apple の Xcode 7 および iOS 8、または新しい Api をインストールして、開発者のコンピューター上に構成する必要があります。
-  **Visual Studio for Mac** – Visual Studio for Mac の最新バージョンをインストールし、ユーザーのデバイスで構成します。
-  **iOS 8 デバイス**– iOS 8 の最新バージョンを実行している iOS デバイス。 IOS シミュレーターでカメラの機能をテストすることはできません。


## <a name="general-av-capture-setup"></a>[全般] AV キャプチャ セットアップ

IOS デバイスでビデオを記録するときに常に必要ないくつかの一般的なセットアップ コードがあります。 このセクションの内容は、iOS デバイスのカメラからビデオを記録し、そのビデオを表示するために必要最小限のセットアップでリアルタイム、`UIImageView`です。

### <a name="output-sample-buffer-delegate"></a>出力サンプル バッファー デリゲート

最初の 1 つ必要になりますサンプル出力バッファーを監視し、バッファーからにつかむイメージを表示するデリゲート、`UIImageView`アプリケーションの UI。

次のルーチンはサンプル バッファーを監視して、UI を更新します。

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

代わりに、このルーチンで、`AppDelegate`ライブ ビデオ フィードを記録する AV キャプチャ セッションを開くには変更できます。

### <a name="creating-an-av-capture-session"></a>AV キャプチャ セッションの作成

AV キャプチャ セッションは、iOS デバイスのカメラからライブ ビデオの記録を制御するために使用され、iOS アプリケーションにビデオを取得するために必要なされます。 例では、以降`ManualCameraControl`サンプル アプリケーションは、キャプチャ セッションを使用して複数の異なる場所で構成される、`AppDelegate`アプリケーション全体からアクセスできるとします。

アプリケーションの変更を次の操作を`AppDelegate`し、必要なコードを追加します。


1. ダブルクリックして、`AppDelegate.cs`ソリューション エクスプ ローラーでファイルを開いて編集するファイル。
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

1. 次のプライベート変数とに計算されたプロパティを追加、`AppDelegate`クラス。
    
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


配置でこのコードでは、手動カメラ コントロール簡単に実装できますの実験とのテストします。

## <a name="manual-focus"></a>手動フォーカス

フォーカスのコントロールを直接実行するエンドユーザーを許可すると、アプリケーションよりアート撮影された、制御を提供できます。

たとえば、プロ カメラマンが実現するためにイメージの焦点を和らげますことができます、 [Bokeh 効果](http://en.wikipedia.org/wiki/Bokeh):

[![](intro-to-manual-camera-controls-images/image2.png "Bokeh 効果")](intro-to-manual-camera-controls-images/image2.png#lightbox)

または、作成、[フォーカス プル効果](http://www.mediacollege.com/video/camera/focus/pull.html)など。

[![](intro-to-manual-camera-controls-images/image3.png "フォーカス プル効果")](intro-to-manual-camera-controls-images/image3.png#lightbox)

科学者や医療アプリケーションのライター、アプリケーションがプログラムによって、レンズを実験に移動しようとする可能性があります。 どちらの方法でも、新しい API により、エンドユーザーまたはイメージ時のフォーカスに制御を実行するアプリケーションのいずれかを取得できます。

### <a name="how-focus-works"></a>フォーカスの動作方法

IOS 8 アプリケーションでのフォーカスの制御の詳細を説明する前にします。 IOS デバイスでフォーカスがどのように動作するか見てをみましょう。

[![](intro-to-manual-camera-controls-images/image4.png "IOS デバイスでのフォーカスの動作方法")](intro-to-manual-camera-controls-images/image4.png#lightbox)

ライトでは、iOS デバイスでカメラのレンズを入力し、イメージ センサーにフォーカスが移動します。 センサーへのリレーションシップで、中心点 (イメージを最も鮮明な表示領域) は、ここで、レンズ センサー コントロールからの距離。 レンズは、センサーから遠いほど、距離オブジェクト見える鮮明およびオブジェクトの近くの近くに見える最も鮮明なです。

IOS デバイスで、レンズまたは移動されます近い、センサーからかけ離れて磁石および springs によってです。 その結果、レンズの正確な位置ことはできませんは異なります、デバイスとデバイスの向きや、デバイスと spring の年齢などのパラメーターの影響を受けることができます。

### <a name="important-focus-terms"></a>フォーカスの重要な用語

フォーカスを扱う場合は、開発者が精通する必要のあるいくつかの用語があります。

-  **被写し界深度**– よりもはるかにフォーカスと最も近いオブジェクト間の距離。 
-  **マクロ**-フォーカス スペクトルの終了間近、レンズを取り組める最も近い距離です。
-  **無限大**– フォーカス スペクトルの端では、レンズを取り組める端までの距離。
-  **Hyperfocal 距離**– これは、ポイント フォーカス スペクトル、フレーム内の最も遠いオブジェクトはだけ、相手側の最後にフォーカスします。 つまり、これは、被写し界深度を最大化を焦点位置です。 
-  **位置のレンズ**– 上のすべては何によって制御されるその他の用語です。 これは、センサーからなり、その結果、レンズの距離は、フォーカスのコント ローラーであります。


これらの用語と注意内のナレッジを使用して、新しい手動フォーカス コントロールが正常に実装できます iOS 8 アプリケーションにします。

### <a name="existing-focus-controls"></a>既存のフォーカス コントロール

iOS 7、および以前のバージョン指定を使用して既存のフォーカス コントロール`FocusMode`プロパティとして。

-   `AVCaptureFocusModeLocked` – フォーカスは、単一のフォーカス ポイントでロックされています。
-   `AVCaptureFocusModeAutoFocus` – カメラは、存在したままシャープ フォーカスが見つかるまで、すべての中心点を通るレンズをスイープします。
-   `AVCaptureFocusModeContinuousAutoFocus` – カメラは refocuses 焦点の条件を検出するたびにします。


既存のコントロールも経由で関心のある設定可能なポイントを提供、`FocusPointOfInterest`プロパティをできるように、ユーザーが特定の領域に焦点を絞るタップことができます。 アプリケーションを監視して、レンズ移動を追跡できますも、`IsAdjustingFocus`プロパティです。

さらに、範囲の制限は、によって提供された、`AutoFocusRangeRestriction`プロパティとして。

-   `AVCaptureAutoFocusRangeRestrictionNear` – 近くの深さを取得する、autofocus を制限します。 QR コードまたはバーコードをスキャンなどの状況で役立ちます。
-   `AVCaptureAutoFocusRangeRestrictionFar` –、Autofocus を離れている深度を制限します。 関連する既知のオブジェクトは、ビューのフィールド (たとえば、ウィンドウ フレーム) にある場合に役立ちます。


最後に、`SmoothAutoFocus`速度が低下し、自動フォーカス アルゴリズムおよびビデオを記録するときに移動するアイテムを回避するより小さな増分で手順プロパティです。

### <a name="new-focus-controls-in-ios-8"></a>IOS 8 の新しいフォーカス コントロール

機能に加えて、iOS 7 以降を既に提供されている、次の機能は iOS 8 でのフォーカスの制御に使用できるようになりました。

-  フォーカスをロックするときに、レンズ位置の完全に手動で制御します。
-  レンズ位置、フォーカス モードでのキーと値の監視。


上記の機能を実装する、`AVCaptureDevice`に含める読み取り専用のクラスが変更された`LensPosition`プロパティ カメラのレンズの現在位置を取得するために使用します。

レンズ位置の手動制御するには、キャプチャ デバイスがロックされているフォーカス モードでする必要があります。 例:

 `CaptureDevice.FocusMode = AVCaptureFocusMode.Locked;`

`SetFocusModeLocked`キャプチャ デバイスのメソッドを使用して、カメラのレンズの位置を調整します。 変更を反映させるときに通知を取得する、省略可能なコールバック ルーチンを提供できます。 例:

```csharp
ThisApp.CaptureDevice.LockForConfiguration(out Error);
ThisApp.CaptureDevice.SetFocusModeLocked(Position.Value,null);
ThisApp.CaptureDevice.UnlockForConfiguration();
```

上記のコードからわかるようには、レンズ位置の変更ができる前に、構成のキャプチャ デバイスをロックする必要があります。 有効なレンズ位置の値は、0.0 と 1.0 の間はします。

### <a name="manual-focus-example"></a>手動のフォーカスの例

代わりに、一般的な AV キャプチャ セットアップ コードで、`UIViewController`アプリケーションのストーリー ボードに追加され、次のように構成されていることができます。

[![](intro-to-manual-camera-controls-images/image5.png "UIViewController をストーリー ボードのアプリケーションに追加し、次のように構成されています。")](intro-to-manual-camera-controls-images/image5.png#lightbox)

ビューには、次の主な要素が含まれています。

-  A`UIImageView`ビデオ フィードが表示されます。
-  A`UISegmentedControl`ロックを自動からフォーカス モードを変更します。
-  A`UISlider`が表示され、レンズの現在の位置を更新します。


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
  
1. 次の計算プロパティを追加します。

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```  
  
1. 上書き、`ViewDidLoad`メソッドし、次のコードを追加します。

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
  
1. オーバーライド、`ViewDidAppear`メソッド ビューが読み込まれた場合の記録を開始するには、次を追加します。

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
  
1. Auto モードでカメラをスライダーが移動します自動的にカメラを調整します。 フォーカス。

    [![](intro-to-manual-camera-controls-images/image6.png "カメラを調整します。 このサンプル アプリでフォーカスとスライダーが自動的に移動します。")](intro-to-manual-camera-controls-images/image6.png#lightbox)
1. ロック済みセグメントをタップし、レンズの位置を手動で調整する位置のスライダーをドラッグします。

    [![](intro-to-manual-camera-controls-images/image7.png "手動でレンズ位置の調整")](intro-to-manual-camera-controls-images/image7.png#lightbox)
1. アプリケーションを停止します。


上記のコードでは、カメラが自動モードのとき、レンズの位置を監視またはスライダーを使用して、ロックのモードであるときに、レンズの位置を制御する方法を説明しました。

## <a name="manual-exposure"></a>手動露出

ソースの明るさ、基準とした画像の明るさを指す参照と光の量によってヒット数について、センサー期間、およびセンサー (ISO のマッピング) のゲイン レベルによって決定されます。 露出を手動で制御を提供すると、アプリケーションより自由にユーザーに提供終了と、定型の外観の実現を可能にします。

手動露出コントロールを使用して、ユーザーは非現実的明るい色から暗いとムーディにイメージを実行できます。

[![](intro-to-manual-camera-controls-images/image8.png "サンプルに濃い色とムーディ非現実的明るい色から露出が表示された画像")](intro-to-manual-camera-controls-images/image8.png#lightbox)

ここでも、これ行う科学アプリケーションまたはアプリケーションのユーザー インターフェイスで提供される手動のコントロールを使用して、プログラムによる制御が自動的に使用します。 どちらにしても、新しい iOS 8 の公開 Api を提供、カメラの危険度の設定をきめ細かく制御します。

### <a name="how-exposure-works"></a>危険度のしくみ

前の IOS 8 アプリケーションでリスクの度合いを制御する詳細を説明します。 危険度のしくみの概要を見てみましょう。

[![](intro-to-manual-camera-controls-images/image9.png "危険度のしくみ")](intro-to-manual-camera-controls-images/image9.png#lightbox)

露出の制御を一緒に付属している 3 つの基本的な要素は次のとおりです。

-  **シャッター スピード**– これは、シャッターはカメラ センサーに信号を開いている時間の長さ。 短いシャッターが開いて、時間、小さいライト、let と、鮮明な画像が (モーションぼかし以下)。 シャッターが開いている長く、ほど運動ぼかしに発生してに多くの光のことができますがします。
-  **ISO マッピング**– これはフィルム写真から借用した用語であり、ライトのフィルムの化学薬品の感度を指します。 フィルムの低 ISO 値より少ない粒度と色の再現をさらに細かく; があります。デジタル センサーの ISO 値が小さいが少ない明るさ、ノイズを除去するセンサーがあります。 ISO の値が高いほど、イメージが明るくなりますが、複数のセンサー ノイズを含むです。 "ISO"デジタル センサーについては、メジャーの[電子ゲイン](http://en.wikipedia.org/wiki/Gain)、物理的な機能ではありません。 
-  **レンズ絞り**– これはレンズの開口部のサイズ。 すべての iOS デバイスでのレンズの開口部は固定、露出の調整に使用できる 2 つの値は、シャッター スピードおよび ISO のためです。


### <a name="how-continuous-auto-exposure-works"></a>継続的な方法の自動公開の動作

学習する前に、手動露出の動作は適切な iOS デバイスで動作するか自動露出を継続的な方法を理解することを勧めします。

[![](intro-to-manual-camera-controls-images/image10.png "継続的な自動露出の iOS デバイスで動作")](intro-to-manual-camera-controls-images/image10.png#lightbox)

自動露出ブロックを 1 つ目は、ジョブの計算に最適な露出を持ち、統計の使用状況測定が継続的にフィードされます。この情報を使っても点灯シーンを取得するには、ISO とシャッター スピードの最適な組み合わせを計算します。 このサイクルは、"AE ループ"と呼ばれます。

### <a name="how-locked-exposure-works"></a>ロックされたどの露出 Works

次に、iOS デバイスでどのようにロックの露出 works 見ていきましょう。

[![](intro-to-manual-camera-controls-images/image11.png "露出で iOS デバイスの機能はロックされている方法")](intro-to-manual-camera-controls-images/image11.png#lightbox)

もう一度、最適な iOS および期間の値を計算しようとする自動露出ブロックがあります。 ただし、このモードでは、AE ブロックは、統計の使用状況測定のエンジンから切断されます。

### <a name="existing-exposure-controls"></a>危険度の既存のコントロール

iOS 7 以降を使用して、次の既存の公開コントロールを提供し、`ExposureMode`プロパティ。

-   `AVCaptureExposureModeLocked` – シーンを 1 回サンプリングし、シーン全体でこれらの値を使用します。
-   `AVCaptureExposureModeContinuousAutoExposure` – もが点灯していることを確認するには、継続的にシーンをサンプリングします。


`ExposurePointOfInterest`タップすると、公開するターゲット オブジェクトを選択して、シーンを公開するために使用して、アプリケーションを監視できる、`AdjustingExposure`露出が調整されているときに表示するプロパティです。

### <a name="new-exposure-controls-in-ios-8"></a>IOS 8 の新しい露出コントロール

機能に加えて、iOS 7 以降を既に提供されている、次の機能は iOS 8 の露出の制御に使用できるようになりました。

-  完全に手動のカスタム露出します。
-  Get、Set、キーと値は、IOS およびシャッター スピード (期間) を確認します。


上記の機能を実装する新しい`AVCaptureExposureModeCustom`モードが追加されました。 カメラがカスタム モードのときは、露出期間と ISO を調整する、次のコードを使用できます。

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.LockExposure(DurationValue,ISOValue,null);
CaptureDevice.UnlockForConfiguration();
```

アプリケーションは、自動およびロックのモードで、次のコードを使用して自動露出ルーチンのバイアスを調整できます。

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.SetExposureTargetBias(Value,null);
CaptureDevice.UnlockForConfiguration();
```

最小値と最大の設定の範囲は、必要がありますしないするハード コーディングされているため、アプリケーションが実行されているデバイスによって異なります。 代わりに、以下のプロパティを使用して、最小値と最大値の範囲を取得します。

-   `CaptureDevice.MinExposureTargetBias` 
-   `CaptureDevice.MaxExposureTargetBias` 
-   `CaptureDevice.ActiveFormat.MinISO` 
-   `CaptureDevice.ActiveFormat.MaxISO` 
-   `CaptureDevice.ActiveFormat.MinExposureDuration` 
-   `CaptureDevice.ActiveFormat.MaxExposureDuration` 


上記のコードからわかるようには、危険度の変更ができる前に、構成のキャプチャ デバイスをロックする必要があります。

### <a name="manual-exposure-example"></a>手動露出の例

代わりに、一般的な AV キャプチャ セットアップ コードで、`UIViewController`アプリケーションのストーリー ボードに追加され、次のように構成されていることができます。

[![](intro-to-manual-camera-controls-images/image12.png "UIViewController をストーリー ボードのアプリケーションに追加し、次のように構成されています。")](intro-to-manual-camera-controls-images/image12.png#lightbox)

ビューには、次の主な要素が含まれています。

-  A`UIImageView`ビデオ フィードが表示されます。
-  A`UISegmentedControl`ロックを自動からフォーカス モードを変更します。
-  次の 4 つ`UISlider`コントロールを表示すると、オフセット、期間、ISO、バイアスを更新します。


ワイヤ アップ ビュー コント ローラーの手動露出の調整には、次の操作を行います。


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
  
1. 次の計算プロパティを追加します。

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```  
  
1. 上書き、`ViewDidLoad`メソッドし、次のコードを追加します。

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
  
1. オーバーライド、`ViewDidAppear`メソッド ビューが読み込まれた場合の記録を開始するには、次を追加します。

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
  
1. Auto モードでカメラをスライダーが移動します自動的にカメラ露出の調整。

    [![](intro-to-manual-camera-controls-images/image13.png "カメラ露出の調整とスライダーが自動的に移動します。")](intro-to-manual-camera-controls-images/image13.png#lightbox)
1. ロックのセグメントをタップし、バイアス スライダーを自動公開のバイアスを手動で調整します。

    [![](intro-to-manual-camera-controls-images/image14.png "自動の露出のバイアスを手動で調整します。")](intro-to-manual-camera-controls-images/image14.png#lightbox)
1. カスタム セグメントをタップし、露出を手動で制御する期間と ISO のスライダーをドラッグします。

    [![](intro-to-manual-camera-controls-images/image15.png "期間と ISO スライダーをドラッグして、手動で公開を制限する")](intro-to-manual-camera-controls-images/image15.png#lightbox)
1. アプリケーションを停止します。


上記のコードが自動モードの場合に、カメラがあるときに、危険度の設定を監視する方法を示すし、スライダーを使用して、ロックまたはカスタム モードであるときに、公開を制御する方法です。

## <a name="manual-white-balance"></a>手動ホワイト バランス

ホワイト バランス コントロールをより現実的な見えるように、画像の colosr のバランスを調整できるようにします。 異なる光源は別の色温度を持ち、これらの相違点を補正するイメージをキャプチャするために使用カメラ設定を調整する必要があります。 ここでも、ユーザーを制御するホワイト バランスを許可することでようにアートの効果を実現する、自動ルーチンのサポートされていないプロフェッショナル向けの調整します。

[![](intro-to-manual-camera-controls-images/image16.png "手動ホワイト バランス調整を示すサンプル画像")](intro-to-manual-camera-controls-images/image16.png#lightbox)

たとえば、夏時間では、タングステン白熱ライト ウォーマー、黄、オレンジ色の濃淡がある一方、青のキャストがあります。 (当惑させるように、「クール」色が、「ウォーム」色より上位の色温度があります。 色温度は、物理メジャーを 1 つは知覚的されません)。

人間の点に注意が色温度、違いに補正で非常に良好には、これは、カメラを行うことはできません。 カメラは、色の違いを調整する逆のスペクトルの色をブースティングによって機能します。

新しい iOS 8 露出 API は、アプリケーションをプロセスを制御し、カメラのホワイト バランス設定の詳細に制御を提供できます。

### <a name="how-white-balance-works"></a>白色の分散の動作

IOS 8 アプリケーションのホワイト バランスの制御の詳細を説明する前にします。 白色の分散の動作を簡単に見てをみましょう。

色の認識の調査で、 [CIE 1931 RGB 色空間と CIE 1931 XYZ 色空間](http://en.wikipedia.org/wiki/CIE_1931_color_space)は最初は数学的にカラー スペースを定義します。 1931 照明 (CIE) 国際委員会によって作成されました。

[![](intro-to-manual-camera-controls-images/image17.png "領域をカラー CIE 1931 RGB 色空間と CIE 1931"xyz"")](intro-to-manual-camera-controls-images/image17.png#lightbox)

上のグラフが表示すべての色の濃い青明るい赤に明るい緑から人間の目に表示されます。 上の図に示すように、X と Y 値を持つ図上の任意の時点をプロットすることができます。

グラフ内に表示、人間のビジョンの範囲外になるグラフにプロットできる X と Y の値があるし、その結果、カメラでこれらの色を再現することはできません。

上記の表では、小さい曲線が呼び出された、 [Planckian たどった](http://en.wikipedia.org/wiki/Planckian_locus)側青 (人気) より大きい番号の度ケルビン)、「色温度を表現する赤色の側 (冷たく) の番号が低い、します。 これらは、一般的な照明場合に便利です。

混合照明条件、ホワイト バランス調整が必要な変更を加える Planckian たどったとは異なる必要があります。 調整は必要があります。 これらの状況である緑のいずれかに移動されたまたは、CIE の赤/マゼンタ側をスケールします。

iOS デバイスは、逆の色のゲインを混ぜてカラー キャストを補正します。 たとえば、シーンの青が多すぎる場合は、補正するために赤のゲインがブーストされます。 デバイスに依存しているので、特定のデバイス用の値が調整されたこれらの向上。

### <a name="existing-white-balance-controls"></a>ホワイト バランスの既存のコントロール

iOS 7 以降を使用して、次の既存ホワイト バランス コントロールを提供および`WhiteBalanceMode`プロパティ。

-   `AVCapture WhiteBalance ModeLocked` – サンプリング シーンを 1 回とシーン全体でこれらの値を使用しています。
-   `AVCapture WhiteBalance ModeContinuousAutoExposure` – 分散もあることを確認するには、継続的にシーンをサンプリングします。


アプリケーションを監視し、`AdjustingWhiteBalance`露出が調整されているときに表示するプロパティです。

### <a name="new-white-balance-controls-in-ios-8"></a>IOS 8 の新しいホワイト バランス コントロール

機能に加えて、iOS 7 以降を既に提供されている、次の機能は iOS 8 のコントロール ホワイト バランスに利用できるようになりました。

-  デバイス RGB の完全に手動で制御を取得します。
-  Get、Set、キーと値を観察 RGB デバイスを取得します。
-  灰色のカードを使用してホワイト バランスをサポートします。
-  デバイスの独立した色空間との間の変換ルーチンです。


上記の機能を実装する、`AVCaptureWhiteBalanceGain`は次のメンバーで構造体が追加されました。

-   `RedGain` 
-   `GreenGain` 
-   `BlueGain` 


最大ホワイト バランスの向上が現在は 4 つ (4) から利用できると、`MaxWhiteBalanceGain`プロパティです。 有効な範囲は 1 つ (1) から`MaxWhiteBalanceGain`(4) 現在します。

`DeviceWhiteBalanceGains`現在の値を確認するプロパティを使用できます。 使用して`SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains`バランスを調整するカメラがロックされているホワイト バランス モードを取得します。

#### <a name="conversion-routines"></a>変換ルーチン

変換ルーチンとの間への変換に役立てるために iOS 8 に追加されているデバイスの独立した色空間です。 変換ルーチンを実装する、`AVCaptureWhiteBalanceChromaticityValues`は次のメンバーで構造体が追加されました。

-   `X` は、0 ~ 1 の値。
-   `Y` は、0 ~ 1 の値。


`AVCaptureWhiteBalanceTemperatureAndTintValues`構造体は、次のメンバーでも追加されています。

-   `Temperature` -浮動ケルビンで値をポイントします。
-   `Tint` は、緑または緑の方向に向かって正の値を持つ 150 に 0 からマゼンタと負の値に向けたでマゼンタからのオフセット。


使用して、`CaptureDevice.GetTemperatureAndTintValues`と`CaptureDevice.GetDeviceWhiteBalanceGains`温度、濃淡、一番および RGB 間で変換するメソッドは、カラー スペースを取得します。

> [!NOTE]
> 変換ルーチンは、変換する値が Planckian たどったに近い正確です。




#### <a name="gray-card-support"></a>灰色のカードのサポート

Apple では、という用語は、灰色の世界を使用して、iOS 8 に組み込まれているグレーのカードのサポートを参照してください。 ユーザーは、フレームの中心となるには、少なくとも 50% をカバーおよびホワイト バランスを調整することを使用する物理灰色カードに重点を置くできます。 灰色のカードの目的は、ニュートラルに表示される空白を実現するためにです。

これをカメラの前に、物理灰色カードを配置するユーザー入力を求めるでアプリケーションに実装できます監視、`GrayWorldDeviceWhiteBalanceGains`プロパティと値を清算するまで待機しています。

アプリケーションのホワイト バランスの向上をロックは、`SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains`メソッドから値を使用して、`GrayWorldDeviceWhiteBalanceGains`変更を適用するプロパティです。

ホワイト バランスの変更を行う前に、構成のキャプチャ デバイスをロックする必要があります。

### <a name="manual-white-balance-example"></a>手動ホワイト バランスの例

代わりに、一般的な AV キャプチャ セットアップ コードで、`UIViewController`アプリケーションのストーリー ボードに追加され、次のように構成されていることができます。

[![](intro-to-manual-camera-controls-images/image18.png "UIViewController をストーリー ボードのアプリケーションに追加し、次のように構成されています。")](intro-to-manual-camera-controls-images/image18.png#lightbox)

ビューには、次の主な要素が含まれています。

-  A`UIImageView`ビデオ フィードが表示されます。
-  A`UISegmentedControl`ロックを自動からフォーカス モードを変更します。
-  2 つ`UISlider`コントロールを表示して、温度、濃淡を更新します。
-  A`UIButton`灰色カード (灰色 World) 領域をサンプリングし、それらの値を使用して、ホワイト バランスを設定するために使用します。


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
  
1. 次の計算プロパティを追加します。

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```  
  
1. 新しい空白の設定に次のプライベート メソッドを追加温度、濃淡のバランスをとる。

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
  
1. 上書き、`ViewDidLoad`メソッドし、次のコードを追加します。

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
  
1. オーバーライド、`ViewDidAppear`メソッド ビューが読み込まれた場合の記録を開始するには、次を追加します。

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
  
1. 変更を保存、コードおよびアプリケーションを実行します。
1. Auto モードでカメラをスライダーが移動します自動的にカメラ ホワイト バランスを調整します。

    [![](intro-to-manual-camera-controls-images/image19.png "カメラ ホワイト バランスを調整すると、スライダーが自動的に移動します。")](intro-to-manual-camera-controls-images/image19.png#lightbox)
1. ロック済みセグメントをタップし、ホワイト バランスを手動で調整する Temp と濃淡スライダーをドラッグします。

    [![](intro-to-manual-camera-controls-images/image20.png "ホワイト バランスを手動で調整する Temp と濃淡スライダーをドラッグします。")](intro-to-manual-camera-controls-images/image20.png#lightbox)
1. ロック セグメントは、選択したまま、カメラの前面に物理灰色カードを配置し、その灰色の世界にホワイト バランスを調整する灰色のカード ボタンをタップします。

    [![](intro-to-manual-camera-controls-images/image21.png "ボタンをタップします灰色のカードを灰色の世界ホワイト バランスを調整するには")](intro-to-manual-camera-controls-images/image21.png#lightbox)
1. アプリケーションを停止します。

上記のコードでは、カメラが自動モードの場合は、ホワイト バランス設定を監視またはスライダーを使用して、ロック モードの場合は、ホワイト バランスを制御する方法を説明しました。

## <a name="bracketed-capture"></a>かっこ付きのキャプチャ

角かっこのキャプチャは上に示した手動カメラのコントロールの設定に基づいており、により、アプリケーションのさまざまな方法で、特定の時点をキャプチャします。

簡単に言うと、さまざまな画像に画像の設定で撮影された画像を静止のバーストは、角かっこをキャプチャします。

[![](intro-to-manual-camera-controls-images/image22.png "角かっこのキャプチャのしくみ")](intro-to-manual-camera-controls-images/image22.png#lightbox)

IOS 8 の角かっこのキャプチャを使用して、アプリケーション一連の手動のカメラ コントロールを事前設定、1 つのコマンドを発行および手動のプリセットのそれぞれの一連のイメージを返す現在のシーンがあります。

### <a name="bracketed-capture-basics"></a>かっこで囲まれたキャプチャの基礎

画像に画像からさまざまな設定で撮影された画像を静止のバーストがもう一度は角かっこをキャプチャします。 使用可能なキャプチャ角かっこの種類があります。

-  **危険度の角かっこを自動**可変バイアス金額のすべてのイメージのある – です。
-  **手動の露出ブラケット**ですべてのイメージは、可変のシャッター スピード (期間) と ISO をある – 量。
-  **単純なバースト ブラケット**– 一連の連続で実行されるイメージもします。


### <a name="new-bracketed-capture-controls-in-ios-8"></a>角かっこのキャプチャの新しいコントロール iOS 8

キャプチャ角かっこのすべてのコマンドは実装されて、`AVCaptureStillImageOutput`クラスです。 使用して、`CaptureStillImageBracket`設定の指定された配列を持つ一連のイメージを取得します。

設定の処理は、2 つの新しいクラスが実装されました。

-   `AVCaptureAutoExposureBracketedStillImageSettings` – が 1 つのプロパティ、 `ExposureTargetBias`、自動露出角かっこの時差を設定するために使用します。 
-   `AVCaptureManual`  `ExposureBracketedStillImageSettings` – プロパティがある 2 つ、`ExposureDuration`と`ISO`、手動露出角かっこのシャッター スピードと ISO を設定するために使用します。 


### <a name="bracketed-capture-controls-dos-and-donts"></a>かっこで囲まれたキャプチャの制御について

#### <a name="dos"></a>アクション

角かっこのキャプチャを使用してコントロール iOS 8 の場合に行う必要がある点の一覧を次に示します。

-  最悪のキャプチャ状況では、呼び出すことによってアプリを準備する、`PrepareToCaptureStillImageBracket`メソッドです。
-  サンプルのバッファーが同じ共有プールから取得しようとしていると仮定します。
-  前の準備の呼び出しによって割り当てられたメモリを解放するには、呼び出す`PrepareToCaptureStillImageBracket`再度と 1 つのオブジェクトの配列を送信します。


#### <a name="donts"></a>注意事項

角かっこのキャプチャを使用してコントロール iOS 8 の場合に行ってはいけません項目の一覧を次に示します。

-  単一のキャプチャの設定の種類をキャプチャ角かっこを統一します。
-  要求しないで以上`MaxBracketedCaptureStillImageCount`単一のキャプチャ内のイメージです。


### <a name="bracketed-capture-details"></a>かっこで囲まれたキャプチャの詳細

次の内容は、8、iOS でキャプチャ角かっこを使用する場合に考慮される必要があります。

-  かっこで囲まれた設定を一時的に無効、`AVCaptureDevice`設定します。
-  フラッシュし、なおかつイメージ安定化設定は無視されます。
-  すべてのイメージは、同じ出力形式 (jpeg、png など) を使用する必要があります。
-  ビデオのプレビューでは、フレームを削除できます。
-  IOS 8 と互換性のあるすべてのデバイスでは、かっこで囲まれたキャプチャをサポートします。


注意この情報をキャプチャ角かっこを使用して ios 8 の例を見てをみましょう。

### <a name="bracket-capture-example"></a>角かっこのキャプチャの使用例

代わりに、一般的な AV キャプチャ セットアップ コードで、`UIViewController`アプリケーションのストーリー ボードに追加され、次のように構成されていることができます。

[![](intro-to-manual-camera-controls-images/image23.png "UIViewController をストーリー ボードのアプリケーションに追加し、次のように構成されています。")](intro-to-manual-camera-controls-images/image23.png#lightbox)

ビューには、次の主な要素が含まれています。

-  A`UIImageView`ビデオ フィードが表示されます。
-  次の 3 つ`UIImageViews`をキャプチャの結果が表示されます。
-  A`UIScrollView`ビデオ フィードと結果ビューを格納します。
-  A`UIButton`プリセット設定の一部で、角かっこのキャプチャを実行するために使用します。


ワイヤ アップ ビュー コント ローラーの角かっこをキャプチャするには、次の操作を行います。


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
  
1. 次の計算プロパティを追加します。

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    #endregion
    ```  
  
1. 必要な出力イメージ ビューを作成する次のプライベート メソッドを追加します。

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
  
1. 上書き、`ViewDidLoad`メソッドし、次のコードを追加します。
    
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
    
  
1. 上書き、`ViewDidAppear`メソッドし、次のコードを追加します。

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
    
1. 変更を保存、コードおよびアプリケーションを実行します。
1. 枠をシーンおよび角かっこのキャプチャ ボタンをタップします。

    [![](intro-to-manual-camera-controls-images/image24.png "シーンのフレームし、キャプチャの角かっこボタンをタップします。")](intro-to-manual-camera-controls-images/image24.png#lightbox)
1. 右から左角かっこキャプチャによって使用された 3 つのイメージを表示する方向にスワイプします。

    [![](intro-to-manual-camera-controls-images/image25.png "右から左角かっこキャプチャによって使用された 3 つのイメージを表示する方向にスワイプ")](intro-to-manual-camera-controls-images/image25.png#lightbox)
1. アプリケーションを停止します。


上記のコードでは、構成、および iOS 8 で、自動露出角かっこのキャプチャを実行する方法を説明しました。

## <a name="summary"></a>まとめ

この記事の内容お iOS 8 で提供される新しい手動カメラ コントロールの概要について説明し、その実行内容とその機能の基礎について説明します。 手動フォーカス、手動露出と手動ホワイト バランスの例を提供しています。 最後に、例を角かっこを使用してキャプチャ手動カメラの前に説明したコントロールを紹介しました

## <a name="related-links"></a>関連リンク

- [ManualCameraControls (サンプル)](https://developer.xamarin.com/samples/monotouch/ManualCameraControls)
- [iOS 8 の概要](~/ios/platform/introduction-to-ios8.md)
