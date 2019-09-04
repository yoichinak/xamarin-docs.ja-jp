---
title: Xamarin の手動カメラコントロール
description: このドキュメントでは、iOS で iOS AVFoundation フレームワークを使用して、手動でカメラコントロールを有効にする方法について説明します。 手動カメラコントロールを使用すると、ユーザーはフォーカス、ホワイトバランス、および露出の設定を制御できます。
ms.prod: xamarin
ms.assetid: 56340225-5F3C-4BFC-9A79-61496D7FE5B5
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 6f60b52d4fd29aacf319f9de94051e28c9876e33
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/03/2019
ms.locfileid: "70226703"
---
# <a name="manual-camera-controls-in-xamarinios"></a>Xamarin の手動カメラコントロール

Ios 8 `AVFoundation Framework`のに用意されている手動カメラコントロールは、モバイルアプリケーションが ios デバイスのカメラを完全に制御できるようにします。 この細かい制御レベルを使用して、プロフェッショナルレベルのカメラアプリケーションを作成し、静止画像やビデオを撮影しながら、カメラのパラメーターを調整することでアーティストコンポジションを提供できます。

これらのコントロールは、科学や工業アプリケーションを開発する場合にも役立ちます。結果は、イメージの正確性や美しさに対してはあまり調整されません。また、撮影されるイメージの機能や要素の強調表示についても、より多くの設定が可能です。

## <a name="avfoundation-capture-objects"></a>AVFoundation キャプチャオブジェクト

IOS デバイスでカメラを使用してビデオまたは静止画像を撮影する場合でも、これらのイメージのキャプチャに使用されるプロセスはほぼ同じです。 これは、既定の自動カメラコントロールを使用するアプリケーションや、新しい手動カメラコントロールを利用するアプリケーションに当てはまります。

 [![](intro-to-manual-camera-controls-images/image1.png "AVFoundation キャプチャオブジェクトの概要")](intro-to-manual-camera-controls-images/image1.png#lightbox)

入力は、から`AVCaptureDeviceInput`に`AVCaptureSession`よって`AVCaptureConnection`に取得されます。 結果は、静止画像またはビデオストリームとして出力されます。 プロセス全体は、によって`AVCaptureDevice`制御されます。

## <a name="manual-controls-provided"></a>手動制御の提供

IOS 8 に用意されている新しい Api を使用して、アプリケーションは次のカメラ機能を制御できます。

- **手動フォーカス**: エンドユーザーが直接フォーカスを制御できるようにすることで、アプリケーションは、撮影されたイメージをより細かく制御できます。
- **手動**による露出: 公開を手動で制御することにより、アプリケーションはユーザーに自由に制御し、ユーザーが体裁を整えられるようにすることができます。
- **手動のホワイトバランス**–画像の色を調整するために使用されます。これは、多くの場合、現実的に見えるようにします。 光源の色温度は異なるため、イメージのキャプチャに使用されるカメラの設定は、これらの違いを補うために調整されます。 この場合も、ユーザーがホワイトバランスを制御できるようにすることで、自動的には実行できない調整を行うことができます。


iOS 8 では、既存の iOS Api の拡張機能と拡張機能を使用して、イメージキャプチャプロセスをきめ細かく制御できるようになっています。

## <a name="bracketed-capture"></a>かっこで囲まれるキャプチャ

かっこで囲まれたキャプチャは、上に示した手動カメラコントロールの設定に基づいており、アプリケーションはさまざまな方法で時間をキャプチャできます。

単に説明したように、角かっこで囲まれたキャプチャとは、画像から画像へのさまざまな設定で撮影された静止画像のバーストです。

## <a name="requirements"></a>必要条件

この記事に記載されている手順を完了するには、次のものが必要です。

- **Xcode 7 以降と ios 8**以降– Apple の Xcode 7 と ios 8 以降の api は、開発者のコンピューターにインストールして構成する必要があります。
- **Visual Studio for Mac** –ユーザーデバイスで Visual Studio for Mac の最新バージョンをインストールして構成する必要があります。
- **ios 8 デバイス**-ios 8 の最新バージョンを実行している ios デバイス。 IOS シミュレーターでは、カメラの機能をテストできません。


## <a name="general-av-capture-setup"></a>一般的な AV キャプチャのセットアップ

IOS デバイスでビデオを録画する際には、一般的なセットアップコードが必要です。 このセクションでは、iOS デバイスのカメラからビデオを記録し、そのビデオをリアルタイム`UIImageView`でに表示するために必要な最小限の設定について説明します。

### <a name="output-sample-buffer-delegate"></a>出力サンプルバッファーデリゲート

最初に必要なものの1つは、サンプル出力バッファーを監視し、バッファーからアプリケーション UI `UIImageView`でに対してグラブされたイメージを表示するデリゲートです。

次のルーチンは、サンプルバッファーを監視し、UI を更新します。

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

このルーチンを配置すると、 `AppDelegate`を変更して、ライブビデオフィードを記録するために AV キャプチャセッションを開くことができます。

### <a name="creating-an-av-capture-session"></a>AV キャプチャセッションの作成

AV キャプチャセッションは、iOS デバイスのカメラからのライブビデオの記録を制御するために使用されます。 iOS アプリケーションにビデオを取り込むには、このセッションが必要です。 サンプルアプリケーションの`ManualCameraControl`例では、さまざまな場所でキャプチャセッションを使用しているため、アプリケーション`AppDelegate`全体で使用できるように、で構成されます。

アプリケーションの`AppDelegate`を変更し、必要なコードを追加するには、次の手順を実行します。


1. ソリューションエクスプローラー内の`AppDelegate.cs`ファイルをダブルクリックして、編集用に開きます。
1. 次の using ステートメントをファイルの先頭に追加します。

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
    ```

1. 次のプライベート変数と計算されるプロパティを`AppDelegate`クラスに追加します。

    ```csharp
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

1. 完成したメソッドをオーバーライドし、次のように変更します。

    ```csharp
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


このコードを配置することで、手動カメラコントロールを簡単に実験およびテスト用に実装できます。

## <a name="manual-focus"></a>手動によるフォーカス

エンドユーザーがフォーカスを直接制御できるようにすることで、アプリケーションは、撮影された画像をより視覚的に制御できます。

たとえば、専門家の写真を使用すると、画像の[フォーカスを和らげる](https://en.wikipedia.org/wiki/Bokeh)ことができます。

[![](intro-to-manual-camera-controls-images/image2.png "Bokeh 効果")](intro-to-manual-camera-controls-images/image2.png#lightbox)

または、[フォーカスのプル効果](http://www.mediacollege.com/video/camera/focus/pull.html)を作成します。次に例を示します。

[![](intro-to-manual-camera-controls-images/image3.png "フォーカスのプル効果")](intro-to-manual-camera-controls-images/image3.png#lightbox)

科学者や医療アプリケーションの作成者にとって、アプリケーションでは、実験のためにレンズをプログラムによって移動することが必要になる場合があります。 どちらの方法でも、エンドユーザーまたはアプリケーションは、イメージの取得時にフォーカスを制御できます。

### <a name="how-focus-works"></a>フォーカスのしくみ

IOS 8 アプリケーションでのフォーカスの制御に関する詳細について説明します。 IOS デバイスでフォーカスがどのように動作するかを簡単に見てみましょう。

[![](intro-to-manual-camera-controls-images/image4.png "IOS デバイスでのフォーカスのしくみ")](intro-to-manual-camera-controls-images/image4.png#lightbox)

ライトは iOS デバイスのカメラレンズに入り、イメージセンサーにフォーカスされます。 センサーとの関係で、中心点 (画像が最も鮮明なに見える領域) のセンサーコントロールからのレンズまでの距離です。 レンズの方がセンサーから遠く、距離オブジェクトは最も鮮明なであるように見えますが、近くのオブジェクトが最も鮮明なと思われます。

IOS デバイスでは、レンズは、マグネットとスプリングによってセンサーから近い場所、つまりさらに上に移動します。 その結果、レンズを正確に配置することはできません。これはデバイスによって異なるため、デバイスの向きやデバイスの時代やバネなどのパラメーターの影響を受ける可能性があるためです。

### <a name="important-focus-terms"></a>重要な用語

フォーカスを処理する際には、開発者が理解しておくべき用語がいくつかあります。

- **[フィールドの深さ]** –最も近い、最も遠い範囲内のオブジェクト間の距離です。
- **マクロ**-フォーカススペクトルの近くにあり、レンズが焦点を当てる最も近い距離です。
- **無限大**–これはフォーカスの範囲の一番上の端で、レンズがフォーカスできる最も遠い距離です。
- **ハイパーフォーカス距離**–これは、フレーム内の最も遠いオブジェクトがフォーカスの一番端に位置する、フォーカス範囲のポイントです。 言い換えると、これはフィールドの深さを最大にする焦点です。
- **レンズの位置**–上記の他のすべての用語を制御します。 これはセンサーからのレンズの距離であり、それによってフォーカスが制御されます。


これらの用語と知識を考慮して、新しい手動フォーカス制御を iOS 8 アプリケーションで正常に実装できます。

### <a name="existing-focus-controls"></a>既存のフォーカスコントロール

iOS 7 以前のバージョンでは、次のように`FocusMode`プロパティを使用して既存のフォーカス制御を提供していました。

- `AVCaptureFocusModeLocked`–フォーカスは1つのフォーカスポイントでロックされます。
- `AVCaptureFocusModeAutoFocus`–カメラは、鋭いフォーカスが検出されてそこに残るまで、すべての中心点を通じてレンズをスイープします。
- `AVCaptureFocusModeContinuousAutoFocus`: カメラは、フォーカスのない状態を検出するたびに refocuses ます。


また、既存のコントロールは、`FocusPointOfInterest`プロパティを使用して設定可能なポイントを提供し、ユーザーが特定の領域にフォーカスを設定できるようにします。 アプリケーションでは、プロパティを`IsAdjustingFocus`監視して、レンズの動きを追跡することもできます。

さらに、範囲制限は`AutoFocusRangeRestriction`プロパティによって次のように提供されています。

- `AVCaptureAutoFocusRangeRestrictionNear`–自動フォーカスを近くの深度に限定します。 QR コードやバーコードのスキャンなどの状況で役立ちます。
- `AVCaptureAutoFocusRangeRestrictionFar`–自動フォーカスを遠くの深度に限定します。 関連性のないオブジェクトがビューのフィールド (たとえば、ウィンドウフレーム) に存在する場合に便利です。


最後に、 `SmoothAutoFocus`自動フォーカスアルゴリズムの速度を低下させるプロパティがあります。また、ビデオを録画するときにアーティファクトの移動を回避するために、小さなインクリメントでステップインします。

### <a name="new-focus-controls-in-ios-8"></a>IOS 8 の新しいフォーカスコントロール

Ios 7 以降で既に提供されている機能に加えて、次の機能を使用して iOS 8 でフォーカスを制御できるようになりました。

- フォーカスをロックするときに、レンズの位置を完全に手動で制御します。
- 任意のフォーカスモードでのレンズ位置のキー値の監視。


上記の機能を実装するため`AVCaptureDevice`に、クラスは、カメラのレンズの現在`LensPosition`位置を取得するために使用される読み取り専用プロパティを含むように変更されています。

レンズの位置を手動で制御するには、キャプチャデバイスがロックされたフォーカスモードになっている必要があります。 例:

 `CaptureDevice.FocusMode = AVCaptureFocusMode.Locked;`

キャプチャデバイスのメソッドは、カメラのレンズの位置を調整するために使用されます。 `SetFocusModeLocked` オプションのコールバックルーチンを使用して、変更が有効になったときに通知を受け取ることができます。 例:

```csharp
ThisApp.CaptureDevice.LockForConfiguration(out Error);
ThisApp.CaptureDevice.SetFocusModeLocked(Position.Value,null);
ThisApp.CaptureDevice.UnlockForConfiguration();
```

上記のコードに示されているように、レンズ位置の変更を行う前に、キャプチャデバイスを構成用にロックする必要があります。 有効なレンズ位置の値は、0.0 ~ 1.0 です。

### <a name="manual-focus-example"></a>手動フォーカスの例

一般的な AV キャプチャセットアップコードを使用して、 `UIViewController`をアプリケーションのストーリーボードに追加し、次のように構成することができます。

[![](intro-to-manual-camera-controls-images/image5.png "次に示すように、UIViewController をアプリケーションのストーリーボードに追加し、構成することができます。")](intro-to-manual-camera-controls-images/image5.png#lightbox)

ビューには、次の主な要素が含まれています。

- `UIImageView`ビデオフィードを表示する。
- フォーカスモードを Automatic から Locked に変更する。`UISegmentedControl`
- `UISlider`現在のレンズの位置を表示および更新する。


次の手順を実行して、ビューコントローラーを手動フォーカスコントロールに接続します。


1. 次の using ステートメントを追加します。

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

1. 次の計算されるプロパティを追加します。

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```

1. `ViewDidLoad`メソッドをオーバーライドし、次のコードを追加します。

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

1. `ViewDidAppear`メソッドをオーバーライドし、次のを追加して、ビューが読み込まれるときの記録を開始します。

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

1. Auto モードのカメラでは、カメラがフォーカスを調整するとスライダーが自動的に移動します。

    [![](intro-to-manual-camera-controls-images/image6.png "このサンプルアプリでカメラがフォーカスを調整すると、スライダーが自動的に移動します")](intro-to-manual-camera-controls-images/image6.png#lightbox)
1. [ロック済み] セグメントをタップし、[位置] スライダーをドラッグして、レンズの位置を手動で調整します。

    [![](intro-to-manual-camera-controls-images/image7.png "レンズの位置を手動で調整する")](intro-to-manual-camera-controls-images/image7.png#lightbox)
1. アプリケーションを停止します。


上のコードでは、カメラが自動モードのときにレンズの位置を監視する方法や、ロックモードのときにスライダーを使用してレンズの位置を制御する方法を示しています。

## <a name="manual-exposure"></a>手動露出

露出とは、ソースの明るさを基準とした画像の明るさを指し、センサーにどれだけ光がかかっているか、およびセンサーのゲインレベル (ISO マッピング) によって決まります。 アプリケーションでは、公開を手動で制御することにより、エンドユーザーの自由度を高め、その外観を実現することができます。

ユーザーは、手動露出コントロールを使用して、非現実的な明るさから濃色および moody までのイメージを作成できます。

[![](intro-to-manual-camera-controls-images/image8.png "現実的でない明るいから暗いおよび moody からの露出を示すイメージのサンプル")](intro-to-manual-camera-controls-images/image8.png#lightbox)

この場合も、科学的なアプリケーションに対してプログラムによる制御を使用するか、アプリケーションのユーザーインターフェイスによって提供される手動制御を使用して、自動的にこの操作を実行できます。 どちらの方法でも、新しい iOS 8 露出 Api は、カメラの露出設定をきめ細かく制御できます。

### <a name="how-exposure-works"></a>露出のしくみ

IOS 8 アプリケーションでの公開の制御の詳細について説明する前に。 露出のしくみを簡単に見てみましょう。

[![](intro-to-manual-camera-controls-images/image9.png "露出のしくみ")](intro-to-manual-camera-controls-images/image9.png#lightbox)

公開を制御するためにまとめられる3つの基本的な要素は次のとおりです。

- **シャッター速度**–シャッターを開いてカメラセンサーに光を表示する時間の長さです。 シャッターが開いている時間が短いほど、では小さくなり、画像が鮮明になります (モーションブラーは小さくなります)。 シャッターが開いている時間が長いほど、ではより多くの光源が使用され、動きが多くなります。
- **ISO マッピング**–これはフィルム写真から借用した用語であり、フィルム内の薬品の感度を意味します。 フィルムの低い ISO 値は、粒度が低く、色がより正確に再現されています。デジタルセンサーの ISO の値が低い場合、センサーのノイズは低くなりますが、明るさは低くなります。 ISO の値が大きいほど、イメージは明るくなりますが、センサーノイズが多くなります。 デジタルセンサーの "ISO" は、物理的な機能ではなく、[電子的な利得](https://en.wikipedia.org/wiki/Gain)の尺度です。
- **レンズ絞り**–これは、レンズを開くためのサイズです。 すべての iOS デバイスでレンズの絞りが固定されているため、露出の調整に使用できる値はシャッター速度と ISO だけです。


### <a name="how-continuous-auto-exposure-works"></a>継続的自動露出のしくみ

手動露出のしくみを学習する前に、iOS デバイスでの連続自動露出のしくみを理解しておくことをお勧めします。

[![](intro-to-manual-camera-controls-images/image10.png "IOS デバイスでの連続自動露出の動作")](intro-to-manual-camera-controls-images/image10.png#lightbox)

最初は自動露出ブロックであり、理想的な露出を計算し、測定統計を継続的に開始しています。この情報を使用して、最適な ISO とシャッター速度の組み合わせを計算し、シーンを良好に明るくします。 このサイクルを AE ループと呼びます。

### <a name="how-locked-exposure-works"></a>ロックされた露出のしくみ

次に、iOS デバイスでのロックされた露出の動作について説明します。

[![](intro-to-manual-camera-controls-images/image11.png "IOS デバイスでのロックされた露出のしくみ")](intro-to-manual-camera-controls-images/image11.png#lightbox)

ここでも、最適な iOS と期間の値を計算しようとしている自動露出ブロックがあります。 ただし、このモードでは、AE ブロックが使用状況測定統計エンジンから切断されます。

### <a name="existing-exposure-controls"></a>既存の露出コントロール

iOS 7 以降では、プロパティを`ExposureMode`使用して次の既存の露出コントロールを提供します。

- `AVCaptureExposureModeLocked`–シーンを1回サンプリングし、その値をシーン全体で使用します。
- `AVCaptureExposureModeContinuousAutoExposure`–シーンを継続的にサンプリングして、適切に点灯するようにします。


を`ExposurePointOfInterest`使用すると、公開するターゲットオブジェクトを選択してシーンを公開できます。また、アプリケーションは`AdjustingExposure`プロパティを監視して、露出が調整されていることを確認できます。

### <a name="new-exposure-controls-in-ios-8"></a>IOS 8 の新しい露出コントロール

Ios 7 以降で既に提供されている機能に加えて、iOS 8 での公開を制御するために次の機能を使用できるようになりました。

- 完全に手動でカスタム公開します。
- Get、Set、およびキー値は、IOS とシャッタースピード (期間) を監視します。


上記の機能を実装するために`AVCaptureExposureModeCustom` 、新しいモードが追加されました。 のカメラがカスタムモードの場合は、次のコードを使用して露出期間と ISO を調整できます。

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.LockExposure(DurationValue,ISOValue,null);
CaptureDevice.UnlockForConfiguration();
```

自動およびロックモードでは、アプリケーションは次のコードを使用して、自動露出ルーチンのバイアスを調整できます。

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.SetExposureTargetBias(Value,null);
CaptureDevice.UnlockForConfiguration();
```

設定範囲の最小値と最大値は、アプリケーションが実行されているデバイスによって異なります。そのため、ハードコーディングしないでください。 代わりに、次のプロパティを使用して、最小値と最大値の範囲を取得します。

- `CaptureDevice.MinExposureTargetBias`
- `CaptureDevice.MaxExposureTargetBias`
- `CaptureDevice.ActiveFormat.MinISO`
- `CaptureDevice.ActiveFormat.MaxISO`
- `CaptureDevice.ActiveFormat.MinExposureDuration`
- `CaptureDevice.ActiveFormat.MaxExposureDuration`


上記のコードに示されているように、露出の変更を行う前に、キャプチャデバイスを構成用にロックする必要があります。

### <a name="manual-exposure-example"></a>手動露出の例

一般的な AV キャプチャセットアップコードを使用して、 `UIViewController`をアプリケーションのストーリーボードに追加し、次のように構成することができます。

[![](intro-to-manual-camera-controls-images/image12.png "次に示すように、UIViewController をアプリケーションのストーリーボードに追加し、構成することができます。")](intro-to-manual-camera-controls-images/image12.png#lightbox)

ビューには、次の主な要素が含まれています。

- `UIImageView`ビデオフィードを表示する。
- フォーカスモードを Automatic から Locked に変更する。`UISegmentedControl`
- オフセット`UISlider` 、期間、ISO、およびバイアスを表示および更新する4つのコントロール。


次の手順を実行して、ビューコントローラーを手動露出コントロール用に接続します。


1. 次の using ステートメントを追加します。

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
    private nfloat ExposureDurationPower = 5;
    private nfloat ExposureMinimumDuration = 1.0f/1000.0f;
    #endregion
    ```

1. 次の計算されるプロパティを追加します。

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```

1. `ViewDidLoad`メソッドをオーバーライドし、次のコードを追加します。

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

1. `ViewDidAppear`メソッドをオーバーライドし、次のを追加して、ビューが読み込まれるときの記録を開始します。

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

1. カメラを Auto モードで使用すると、カメラが露出を調整したときにスライダーが自動的に移動します。

    [![](intro-to-manual-camera-controls-images/image13.png "カメラが露出を調整するとスライダーが自動的に移動する")](intro-to-manual-camera-controls-images/image13.png#lightbox)
1. ロックされたセグメントをタップし、バイアススライダーをドラッグして、自動露出のバイアスを手動で調整します。

    [![](intro-to-manual-camera-controls-images/image14.png "自動露出のバイアスを手動で調整する")](intro-to-manual-camera-controls-images/image14.png#lightbox)
1. [カスタムセグメント] をタップし、[期間] と [ISO] スライダーをドラッグして、[露出] を手動で制御します。

    [![](intro-to-manual-camera-controls-images/image15.png "期間と ISO スライダーをドラッグして、露出を手動で制御する")](intro-to-manual-camera-controls-images/image15.png#lightbox)
1. アプリケーションを停止します。


上のコードでは、カメラが自動モードのときに露出設定を監視する方法と、スライダーを使用してロックモードまたはカスタムモードのときに露出を制御する方法について説明しました。

## <a name="manual-white-balance"></a>手動のホワイトバランス

ホワイトバランスコントロールを使用すると、ユーザーは画像の colosr のバランスを調整して、より現実的な外観にすることができます。 光源の色温度は異なるため、イメージをキャプチャするために使用するカメラの設定を調整して、これらの違いを補う必要があります。 この場合も、ユーザーがホワイトバランスを制御できるようにすることで、自動ルーチンが芸術的効果を実現することができないように、プロフェッショナルな調整を行うことができます。

[![](intro-to-manual-camera-controls-images/image16.png "ホワイトバランスの手動調整を示すサンプルイメージ")](intro-to-manual-camera-controls-images/image16.png#lightbox)

たとえば、夏時間には blueish キャストがあり、tungsten incandescent ライトの色はオレンジ色になります。 (紛らわしい、"クール" 色の色温度は、"ウォーム" 色よりも高くなっています。 色温度は、知覚ではなく物理的な測定値です)。

色温度の違いについては補正が非常に良いのですが、これはカメラではできません。 カメラは、反対のスペクトルで色を上げることで動作し、色の違いに合わせて調整します。

新しい iOS 8 公開 API を使用すると、アプリケーションはプロセスを制御し、カメラの白いバランス設定をきめ細かく制御できます。

### <a name="how-white-balance-works"></a>ホワイトバランスのしくみ

IOS 8 アプリケーションでのホワイトバランスの制御の詳細について説明する前に。 次に、ホワイトバランスのしくみについて簡単に見てみましょう。

色認識の研究では、 [cie 1931 の RGB 色空間と cie 1931 XYZ 色空間](https://en.wikipedia.org/wiki/CIE_1931_color_space)が、数学的に定義された最初の色空間になります。 これらは、1931の国際照明 (CIE) によって作成されたものです。

[![](intro-to-manual-camera-controls-images/image17.png "CIE 1931 RGB 色空間と CIE 1931 XYZ 色空間")](intro-to-manual-camera-controls-images/image17.png#lightbox)

上のグラフは、人間の目に見えるすべての色を示しています。これは、ディープブルーから明るい緑から明るい赤までです。 上のグラフに示すように、図の任意のポイントを X および Y 値でプロットできます。

グラフに表示されているように、グラフにプロットできる X 値と Y 値があります。これは、人間の構想の範囲外になる可能性があります。そのため、これらの色はカメラで再現することはできません。

上のグラフの小さい曲線は、 [Planckian Locus](https://en.wikipedia.org/wiki/Planckian_locus)と呼ばれます。これは、色温度 (ケルビン) を表し、青側 (暑すぎるか) の数値が大きくなり、赤色の側 (クール) に小さい数値が含まれます。 これらは、一般的な照明の場合に便利です。

混合照明条件では、必要な変更を行うために、ホワイトバランスの調整は Planckian Locus とは異なる必要があります。 このような状況では、調整は、CIE スケールの緑または赤/マゼンタ側にシフトする必要があります。

iOS デバイスでは、反対色のゲインを上げることで色のキャストを補正します。 たとえば、シーンの青が多すぎると、赤のゲインが上がり、補正されます。 これらの値は、デバイスに依存するように特定のデバイスに対して調整されます。

### <a name="existing-white-balance-controls"></a>既存のホワイトバランスコントロール

iOS 7 以降では、プロパティを使用`WhiteBalanceMode`して次の既存のホワイトバランスコントロールを提供していました。

- `AVCapture WhiteBalance ModeLocked`–シーンを1回サンプリングし、その値をシーン全体で使用します。
- `AVCapture WhiteBalance ModeContinuousAutoExposure`–シーンを継続的にサンプリングして、バランスが取れていることを確認します。


また、アプリケーションは、プロパティ`AdjustingWhiteBalance`を監視して、公開が調整されていることを確認できます。

### <a name="new-white-balance-controls-in-ios-8"></a>IOS 8 の新しいホワイトバランスコントロール

Ios 7 以降で既に提供されている機能に加えて、iOS 8 では、次の機能を使用して、ホワイトバランスを制御できるようになりました。

- デバイスの RGB の向上を完全に手動で制御します。
- Get、Set、およびキー値は、デバイスの RGB の向上を観察します。
- 灰色のカードを使用したホワイトバランスのサポート。
- デバイスに依存しないカラースペースとの間の変換ルーチン。


上記の機能`AVCaptureWhiteBalanceGain`を実装するために、構造体は次のメンバーと共に追加されています。

- `RedGain`
- `GreenGain`
- `BlueGain`


現在、ホワイトバランスの最大値は 4 (4) であり、 `MaxWhiteBalanceGain`プロパティから準備できます。 そのため、有効な範囲は、現在は 1 `MaxWhiteBalanceGain`から (4) までです。

プロパティ`DeviceWhiteBalanceGains`は、現在の値を確認するために使用できます。 カメラ`SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains`がロックされた白のバランスモードのときにバランスの向上を調整するには、を使用します。

#### <a name="conversion-routines"></a>変換ルーチン

変換ルーチンは、デバイスに依存しない色空間との間での変換を支援するために、iOS 8 に追加されました。 変換ルーチン`AVCaptureWhiteBalanceChromaticityValues`を実装するために、次のメンバーと共に構造体が追加されています。

- `X`-0 ~ 1 の値です。
- `Y`-0 ~ 1 の値です。


`AVCaptureWhiteBalanceTemperatureAndTintValues`構造体には、次のメンバーも追加されています。

- `Temperature`-ケルビンの浮動小数点値です。
- `Tint`-は、緑またはマゼンタから150までのオフセットであり、正の値は緑の方向に、マゼンタの場合は負の値になります。


`CaptureDevice.GetTemperatureAndTintValues`との`CaptureDevice.GetDeviceWhiteBalanceGains`メソッドを使用して、温度と濃淡、度、および RGB ゲインの色空間を変換します。

> [!NOTE]
> 変換ルーチンは、変換する値が Planckian Locus に近いほど正確です。




#### <a name="gray-card-support"></a>灰色のカードのサポート

Apple では、グレー表現という用語を使用して、iOS 8 に組み込まれているグレーのカードサポートを参照しています。 これにより、フレームの中心の 50% 以上をカバーする物理的な灰色のカードに焦点を当てることができ、それを使用してホワイトバランスが調整されます。 灰色のカードの目的は、ニュートラルに見えるホワイトを実現することです。

これは、物理灰色のカードをカメラの前に配置し、 `GrayWorldDeviceWhiteBalanceGains`プロパティを監視し、値が解決されるまで待機することで、アプリケーションに実装できます。

その後、アプリケーションは、プロパティの`SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains` `GrayWorldDeviceWhiteBalanceGains`値を使用して変更を適用し、メソッドのホワイトバランスの向上をロックします。

ホワイトバランスの変更を行う前に、キャプチャデバイスを構成用にロックする必要があります。

### <a name="manual-white-balance-example"></a>手動のホワイトバランスの例

一般的な AV キャプチャセットアップコードを使用して、 `UIViewController`をアプリケーションのストーリーボードに追加し、次のように構成することができます。

[![](intro-to-manual-camera-controls-images/image18.png "次に示すように、UIViewController をアプリケーションのストーリーボードに追加し、構成することができます。")](intro-to-manual-camera-controls-images/image18.png#lightbox)

ビューには、次の主な要素が含まれています。

- `UIImageView`ビデオフィードを表示する。
- フォーカスモードを Automatic から Locked に変更する。`UISegmentedControl`
- 気温`UISlider`と着色を表示および更新する2つのコントロール。
- 灰色のカード (灰色のワールド) 空間をサンプリングし、それらの値を使用してホワイトバランスを設定するために使用される。`UIButton`


次の手順を実行して、ホワイトバランスの手動制御のビューコントローラーを接続します。


1. 次の using ステートメントを追加します。

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

1. 次の計算されるプロパティを追加します。

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```

1. 次のプライベートメソッドを追加して、新しい白のバランス温度と濃淡を設定します。

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

1. `ViewDidLoad`メソッドをオーバーライドし、次のコードを追加します。

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

1. `ViewDidAppear`メソッドをオーバーライドし、次のを追加して、ビューが読み込まれるときの記録を開始します。

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

1. 変更内容をコードに保存し、アプリケーションを実行します。
1. カメラが Auto モードの場合、カメラが白のバランスを調整すると、スライダーが自動的に移動します。

    [![](intro-to-manual-camera-controls-images/image19.png "カメラが白のバランスを調整すると、スライダーが自動的に移動します。")](intro-to-manual-camera-controls-images/image19.png#lightbox)
1. [ロック済み] セグメントをタップし、[気温] スライダーと [色合い] スライダーをドラッグして、手動で白のバランスを調整します。

    [![](intro-to-manual-camera-controls-images/image20.png "気温と色合いのスライダーをドラッグして、手動で白いバランスを調整します。")](intro-to-manual-camera-controls-images/image20.png#lightbox)
1. ロックされたセグメントを選択した状態で、物理的な灰色のカードをカメラの前面に配置し、灰色のカードボタンをタップして白のバランスをグレーのワールドに調整します。

    [![](intro-to-manual-camera-controls-images/image21.png "灰色のカードボタンをタップして、白のバランスをグレーのワールドに調整します")](intro-to-manual-camera-controls-images/image21.png#lightbox)
1. アプリケーションを停止します。

上のコードでは、カメラが自動モードである場合、またはスライダーを使用して、ロックモードのときに白いバランスを制御する場合に、白いバランスの設定を監視する方法を示しました。

## <a name="bracketed-capture"></a>かっこで囲まれるキャプチャ

かっこで囲まれたキャプチャは、上に示した手動カメラコントロールの設定に基づいており、アプリケーションはさまざまな方法で時間をキャプチャできます。

単に説明したように、角かっこで囲まれたキャプチャとは、画像から画像へのさまざまな設定で撮影された静止画像のバーストです。

[![](intro-to-manual-camera-controls-images/image22.png "かっこで囲まれるキャプチャのしくみ")](intro-to-manual-camera-controls-images/image22.png#lightbox)

IOS 8 の角かっこで囲まれたキャプチャを使用すると、アプリケーションは一連の手動カメラコントロールを事前に設定し、1つのコマンドを発行して、現在のシーンが手動のプリセットそれぞれに対して一連のイメージを返すことができます。

### <a name="bracketed-capture-basics"></a>かっこで囲まれるキャプチャの基本

ここでも、角かっこで囲まれたキャプチャは、画像間のさまざまな設定で撮影された静止画像のバーストです。 角かっこで囲まれたキャプチャの種類は次のとおりです。

- **自動露出角かっこ**–すべてのイメージのバイアス量が変動します。
- **[手動露出角かっこ]** –すべてのイメージのシャッター速度 (期間) と ISO 金額が変化します。
- **単純なバーストブラケット**は、連続して次々に撮影された一連のイメージです。


### <a name="new-bracketed-capture-controls-in-ios-8"></a>IOS 8 の新しいかっこ付きキャプチャコントロール

かっこで囲まれたすべてのキャプチャ`AVCaptureStillImageOutput`コマンドは、クラスに実装されます。 設定の`CaptureStillImageBracket`配列を指定して一連のイメージを取得するには、メソッドを使用します。

設定を処理するために、次の2つの新しいクラスが実装されています。

- `AVCaptureAutoExposureBracketedStillImageSettings`–自動露出角かっこの`ExposureTargetBias`バイアスを設定するために使用されるプロパティが1つあります。
- `AVCaptureManual`  `ExposureBracketedStillImageSettings`–には、 `ExposureDuration`とと`ISO`いう2つのプロパティがあります。これは、手動露出角かっこのシャッター速度と ISO を設定するために使用されます。


### <a name="bracketed-capture-controls-dos-and-donts"></a>かっこで囲まれたキャプチャコントロールの実行とすべき

#### <a name="dos"></a>作業

次に示すのは、iOS 8 で、角かっこで囲まれたキャプチャコントロールを使用する場合に行う必要がある項目の一覧です。

- `PrepareToCaptureStillImageBracket`メソッドを呼び出すことによって、最悪のケースのキャプチャ状況に対してアプリを準備します。
- サンプルバッファーが同じ共有プールから取得されるものとします。
- 前の prepare 呼び出しで割り当てられたメモリを解放するに`PrepareToCaptureStillImageBracket`は、を再度呼び出して、1つのオブジェクトの配列を送信します。


#### <a name="donts"></a>すべき

次に示すのは、iOS 8 で角かっこで囲まれたキャプチャコントロールを使用している場合に実行すべきものではありません。

- 1つのキャプチャには、かっこで囲まれたキャプチャの設定の種類を混在させないでください。
- 1つのキャプチャ`MaxBracketedCaptureStillImageCount`に複数のイメージを要求しないでください。


### <a name="bracketed-capture-details"></a>かっこで囲まれるキャプチャの詳細

IOS 8 での角かっこで囲まれたキャプチャを使用する場合は、次の詳細を考慮する必要があります。

- かっこで囲まれた`AVCaptureDevice`設定は、設定を一時的にオーバーライドします。
- Flash と静止画像の安定化設定は無視されます。
- すべてのイメージで同じ出力形式 (jpeg、png など) を使用する必要があります。
- ビデオのプレビューでは、フレームを削除することができます。
- 角かっこで囲まれたキャプチャは、iOS 8 と互換性のあるすべてのデバイスでサポートされています。


この情報を考慮して、iOS 8 での角かっこで囲まれたキャプチャの使用例を見てみましょう。

### <a name="bracket-capture-example"></a>角かっこキャプチャの例

一般的な AV キャプチャセットアップコードを使用して、 `UIViewController`をアプリケーションのストーリーボードに追加し、次のように構成することができます。

[![](intro-to-manual-camera-controls-images/image23.png "次に示すように、UIViewController をアプリケーションのストーリーボードに追加し、構成することができます。")](intro-to-manual-camera-controls-images/image23.png#lightbox)

ビューには、次の主な要素が含まれています。

- `UIImageView`ビデオフィードを表示する。
- キャプチャ`UIImageViews`の結果が表示される3つの。
- ビデオフィードと結果ビューを格納する。`UIScrollView`
- いくつ`UIButton`かのプリセット設定を使用して、かっこで囲まれたキャプチャを実行するために使用される。


次の手順を実行して、角かっこで囲まれたキャプチャのビューコントローラーを接続します。


1. 次の using ステートメントを追加します。

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

1. 次の計算されるプロパティを追加します。

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    #endregion
    ```

1. 次のプライベートメソッドを追加して、必要な出力イメージビューを構築します。

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

1. `ViewDidLoad`メソッドをオーバーライドし、次のコードを追加します。

    ```csharp
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


1. `ViewDidAppear`メソッドをオーバーライドし、次のコードを追加します。

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

1. 変更内容をコードに保存し、アプリケーションを実行します。
1. シーンをフレームし、[キャプチャの角かっこ] ボタンをタップします。

    [![](intro-to-manual-camera-controls-images/image24.png "シーンをフレームし、[キャプチャの角かっこ] ボタンをタップする")](intro-to-manual-camera-controls-images/image24.png#lightbox)
1. 右から左にスワイプして、かっこで囲まれたキャプチャによって実行される3つのイメージを確認します。

    [![](intro-to-manual-camera-controls-images/image25.png "右から左にスワイプして、かっこで囲まれたキャプチャによって作成された3つのイメージを表示します。")](intro-to-manual-camera-controls-images/image25.png#lightbox)
1. アプリケーションを停止します。


上記のコードでは、iOS 8 で自動露出のかっこ付きキャプチャを構成して取得する方法を示しました。

## <a name="summary"></a>Summary

この記事では、iOS 8 によって提供される新しい手動カメラコントロールの概要について説明し、それらの機能とその動作の基本について説明しました。 手動フォーカス、手動露出、手動のホワイトバランスの例が用意されています。 最後に、前述の手動カメラコントロールを使用して、かっこで囲まれたキャプチャを取得する例を示しました。

## <a name="related-links"></a>関連リンク

- [ManualCameraControls (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/manualcameracontrols)
- [iOS 8 の概要](~/ios/platform/introduction-to-ios8.md)
