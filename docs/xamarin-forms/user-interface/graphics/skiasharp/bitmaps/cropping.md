---
title: トリミング (SkiaSharp ビットマップを)
description: SkiaSharp を使用して、トリミング四角形を対話形式で表示するためのユーザーインターフェイスをデザインする方法について説明します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 0A79AB27-C69F-4376-8FFE-FF46E4783F30
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 639b9db51d4a9f0bb0ddd55a3d35bcbda7e31962
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93366465"
---
# <a name="cropping-skiasharp-bitmaps"></a>トリミング (SkiaSharp ビットマップを)

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

オブジェクトをコンストラクターに渡す方法については、「 [**SkiaSharp ビットマップの作成と描画**](drawing.md) 」を参照して `SKBitmap` `SKCanvas` ください。 そのキャンバスで呼び出された描画メソッドを使用すると、グラフィックスがビットマップにレンダリングされます。 これらの描画メソッドにはが含ま `DrawBitmap` れています。つまり、この手法では、あるビットマップの一部またはすべてを別のビットマップに転送できます (変換が適用されている場合など)。

この手法は、 [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKRect,SkiaSharp.SKPaint)) 変換元と変換先の四角形を使用してメソッドを呼び出すことにより、ビットマップをトリミングするために使用できます。

```csharp
canvas.DrawBitmap(bitmap, sourceRect, destRect);
```

ただし、トリミングを実装するアプリケーションは、ユーザーがトリミング四角形を対話的に選択するためのインターフェイスを備えていることがよくあります。

![トリミングのサンプル](cropping-images/CroppingSample.png "トリミングのサンプル")

この記事では、そのインターフェイスに焦点を当てています。

## <a name="encapsulating-the-cropping-rectangle"></a>トリミング四角形をカプセル化する

トリミングロジックの一部をという名前のクラスで分離すると便利です `CroppingRectangle` 。 コンストラクターのパラメーターには、最大の四角形が含まれます。これは、通常、トリミングするビットマップのサイズと、オプションの縦横比です。 コンストラクターは、最初に最初のトリミング四角形を定義します。これは、型のプロパティで公開され `Rect` `SKRect` ます。 この最初のトリミング四角形は、ビットマップ四角形の幅と高さの80% ですが、縦横比が指定されている場合は調整されます。

```csharp
class CroppingRectangle
{
    ···
    SKRect maxRect;             // generally the size of the bitmap
    float? aspectRatio;

    public CroppingRectangle(SKRect maxRect, float? aspectRatio = null)
    {
        this.maxRect = maxRect;
        this.aspectRatio = aspectRatio;

        // Set initial cropping rectangle
        Rect = new SKRect(0.9f * maxRect.Left + 0.1f * maxRect.Right,
                          0.9f * maxRect.Top + 0.1f * maxRect.Bottom,
                          0.1f * maxRect.Left + 0.9f * maxRect.Right,
                          0.1f * maxRect.Top + 0.9f * maxRect.Bottom);

        // Adjust for aspect ratio
        if (aspectRatio.HasValue)
        {
            SKRect rect = Rect;
            float aspect = aspectRatio.Value;

            if (rect.Width > aspect * rect.Height)
            {
                float width = aspect * rect.Height;
                rect.Left = (maxRect.Width - width) / 2;
                rect.Right = rect.Left + width;
            }
            else
            {
                float height = rect.Width / aspect;
                rect.Top = (maxRect.Height - height) / 2;
                rect.Bottom = rect.Top + height;
            }

            Rect = rect;
        }
    }

    public SKRect Rect { set; get; }
    ···
}
```

使用可能な情報の1つは、右 `CroppingRectangle` `SKPoint` 左、右上、下から左、および左下にあるトリミング四角形の4つの角に対応する値の配列です。

```csharp
class CroppingRectangle
{
    ···
    public SKPoint[] Corners
    {
        get
        {
            return new SKPoint[]
            {
                new SKPoint(Rect.Left, Rect.Top),
                new SKPoint(Rect.Right, Rect.Top),
                new SKPoint(Rect.Right, Rect.Bottom),
                new SKPoint(Rect.Left, Rect.Bottom)
            };
        }
    }
    ···
}
```

この配列は、と呼ばれる次のメソッドで使用され `HitTest` ます。 パラメーターは、 `SKPoint` 指タッチまたはマウスクリックに対応するポイントです。 メソッドは、パラメーターによって指定された距離内で、指またはマウスポインターがタッチしたコーナーに対応するインデックス (0、1、2、または 3) を返し `radius` ます。

```csharp
class CroppingRectangle
{
    ···
    public int HitTest(SKPoint point, float radius)
    {
        SKPoint[] corners = Corners;

        for (int index = 0; index < corners.Length; index++)
        {
            SKPoint diff = point - corners[index];

            if ((float)Math.Sqrt(diff.X * diff.X + diff.Y * diff.Y) < radius)
            {
                return index;
            }
        }

        return -1;
    }
    ···
}
```

タッチまたはマウスポイントがコーナーの単位内にない場合 `radius` 、メソッドは1を返し &ndash; ます。

の最後のメソッド `CroppingRectangle` が呼び出され `MoveCorner` ます。これは、タッチまたはマウスの動きに応じて呼び出されます。 2つのパラメーターは、移動されるコーナーのインデックスと、そのコーナーの新しい場所を示します。 メソッドの前半では、角の新しい位置に基づいてトリミング四角形を調整しますが、常にビットマップのサイズであるの境界内に配置し `maxRect` ます。 また、このロジックでは、 `MINIMUM` トリミング四角形が何も折りたたまれないように、フィールドを考慮します。

```csharp
class CroppingRectangle
{
    const float MINIMUM = 10;   // pixels width or height
    ···
    public void MoveCorner(int index, SKPoint point)
    {
        SKRect rect = Rect;

        switch (index)
        {
            case 0: // upper-left
                rect.Left = Math.Min(Math.Max(point.X, maxRect.Left), rect.Right - MINIMUM);
                rect.Top = Math.Min(Math.Max(point.Y, maxRect.Top), rect.Bottom - MINIMUM);
                break;

            case 1: // upper-right
                rect.Right = Math.Max(Math.Min(point.X, maxRect.Right), rect.Left + MINIMUM);
                rect.Top = Math.Min(Math.Max(point.Y, maxRect.Top), rect.Bottom - MINIMUM);
                break;

            case 2: // lower-right
                rect.Right = Math.Max(Math.Min(point.X, maxRect.Right), rect.Left + MINIMUM);
                rect.Bottom = Math.Max(Math.Min(point.Y, maxRect.Bottom), rect.Top + MINIMUM);
                break;

            case 3: // lower-left
                rect.Left = Math.Min(Math.Max(point.X, maxRect.Left), rect.Right - MINIMUM);
                rect.Bottom = Math.Max(Math.Min(point.Y, maxRect.Bottom), rect.Top + MINIMUM);
                break;
        }

        // Adjust for aspect ratio
        if (aspectRatio.HasValue)
        {
            float aspect = aspectRatio.Value;

            if (rect.Width > aspect * rect.Height)
            {
                float width = aspect * rect.Height;

                switch (index)
                {
                    case 0:
                    case 3: rect.Left = rect.Right - width; break;
                    case 1:
                    case 2: rect.Right = rect.Left + width; break;
                }
            }
            else
            {
                float height = rect.Width / aspect;

                switch (index)
                {
                    case 0:
                    case 1: rect.Top = rect.Bottom - height; break;
                    case 2:
                    case 3: rect.Bottom = rect.Top + height; break;
                }
            }
        }

        Rect = rect;
    }
}
```

メソッドの後半では、オプションの縦横比を調整します。

このクラスのすべての要素はピクセル単位であることに注意してください。

## <a name="a-canvas-view-just-for-cropping"></a>トリミングのためだけのキャンバスビュー

`CroppingRectangle`先ほど見たクラスは、から派生したクラスによって使用され `PhotoCropperCanvasView` `SKCanvasView` ます。 このクラスは、ビットマップとトリミング四角形を表示するだけでなく、トリミング四角形を変更するためのタッチイベントまたはマウスイベントを処理します。

コンストラクターには `PhotoCropperCanvasView` ビットマップが必要です。 縦横比は省略可能です。 コンストラクターは、 `CroppingRectangle` このビットマップと縦横比に基づいて型のオブジェクトをインスタンス化し、フィールドとして保存します。

```csharp
class PhotoCropperCanvasView : SKCanvasView
{
    ···
    SKBitmap bitmap;
    CroppingRectangle croppingRect;
    ···
    public PhotoCropperCanvasView(SKBitmap bitmap, float? aspectRatio = null)
    {
        this.bitmap = bitmap;

        SKRect bitmapRect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
        croppingRect = new CroppingRectangle(bitmapRect, aspectRatio);
        ···
    }
    ···
}
```

このクラスはから派生しているため、 `SKCanvasView` イベントのハンドラーをインストールする必要はありません `PaintSurface` 。 代わりに、メソッドをオーバーライドでき `OnPaintSurface` ます。 メソッドはビットマップを表示し、 `SKPaint` 現在のトリミング四角形を描画するためにフィールドとして保存されたいくつかのオブジェクトを使用します。

```csharp
class PhotoCropperCanvasView : SKCanvasView
{
    const int CORNER = 50;      // pixel length of cropper corner
    ···
    SKBitmap bitmap;
    CroppingRectangle croppingRect;
    SKMatrix inverseBitmapMatrix;
    ···
    // Drawing objects
    SKPaint cornerStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.White,
        StrokeWidth = 10
    };

    SKPaint edgeStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.White,
        StrokeWidth = 2
    };
    ···
    protected override void OnPaintSurface(SKPaintSurfaceEventArgs args)
    {
        base.OnPaintSurface(args);

        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Gray);

        // Calculate rectangle for displaying bitmap
        float scale = Math.Min((float)info.Width / bitmap.Width, (float)info.Height / bitmap.Height);
        float x = (info.Width - scale * bitmap.Width) / 2;
        float y = (info.Height - scale * bitmap.Height) / 2;
        SKRect bitmapRect = new SKRect(x, y, x + scale * bitmap.Width, y + scale * bitmap.Height);
        canvas.DrawBitmap(bitmap, bitmapRect);

        // Calculate a matrix transform for displaying the cropping rectangle
        SKMatrix bitmapScaleMatrix = SKMatrix.MakeIdentity();
        bitmapScaleMatrix.SetScaleTranslate(scale, scale, x, y);

        // Display rectangle
        SKRect scaledCropRect = bitmapScaleMatrix.MapRect(croppingRect.Rect);
        canvas.DrawRect(scaledCropRect, edgeStroke);

        // Display heavier corners
        using (SKPath path = new SKPath())
        {
            path.MoveTo(scaledCropRect.Left, scaledCropRect.Top + CORNER);
            path.LineTo(scaledCropRect.Left, scaledCropRect.Top);
            path.LineTo(scaledCropRect.Left + CORNER, scaledCropRect.Top);

            path.MoveTo(scaledCropRect.Right - CORNER, scaledCropRect.Top);
            path.LineTo(scaledCropRect.Right, scaledCropRect.Top);
            path.LineTo(scaledCropRect.Right, scaledCropRect.Top + CORNER);

            path.MoveTo(scaledCropRect.Right, scaledCropRect.Bottom - CORNER);
            path.LineTo(scaledCropRect.Right, scaledCropRect.Bottom);
            path.LineTo(scaledCropRect.Right - CORNER, scaledCropRect.Bottom);

            path.MoveTo(scaledCropRect.Left + CORNER, scaledCropRect.Bottom);
            path.LineTo(scaledCropRect.Left, scaledCropRect.Bottom);
            path.LineTo(scaledCropRect.Left, scaledCropRect.Bottom - CORNER);

            canvas.DrawPath(path, cornerStroke);
        }

        // Invert the transform for touch tracking
        bitmapScaleMatrix.TryInvert(out inverseBitmapMatrix);
    }
    ···
}
```

クラスのコードは、 `CroppingRectangle` ビットマップのピクセルサイズのトリミング四角形を基にしています。 ただし、クラスによるビットマップの表示 `PhotoCropperCanvasView` は、表示領域のサイズに基づいて拡大縮小されます。 `bitmapScaleMatrix`オーバーライドで計算されたは、 `OnPaintSurface` ビットマップピクセルからビットマップのサイズと位置にマップされます。 この行列は、ビットマップに対して相対的に表示できるようにトリミング四角形を変換するために使用されます。

オーバーライドの最後の行は `OnPaintSurface` 、の逆を受け取り、 `bitmapScaleMatrix` フィールドとして保存し `inverseBitmapMatrix` ます。 これはタッチ処理に使用されます。

`TouchEffect`オブジェクトはフィールドとしてインスタンス化され、コンストラクターはイベントにハンドラーをアタッチし `TouchAction` ますが、を `TouchEffect` 派生の親のコレクションに追加する必要があり `Effects` _parent_ `SKCanvasView` ます。これにより、オーバーライドでが行われ `OnParentSet` ます。

```csharp
class PhotoCropperCanvasView : SKCanvasView
{
    ···
    const int RADIUS = 100;     // pixel radius of touch hit-test
    ···
    CroppingRectangle croppingRect;
    SKMatrix inverseBitmapMatrix;

    // Touch tracking
    TouchEffect touchEffect = new TouchEffect();
    struct TouchPoint
    {
        public int CornerIndex { set; get; }
        public SKPoint Offset { set; get; }
    }

    Dictionary<long, TouchPoint> touchPoints = new Dictionary<long, TouchPoint>();
    ···
    public PhotoCropperCanvasView(SKBitmap bitmap, float? aspectRatio = null)
    {
        ···
        touchEffect.TouchAction += OnTouchEffectTouchAction;
    }
    ···
    protected override void OnParentSet()
    {
        base.OnParentSet();

        // Attach TouchEffect to parent view
        Parent.Effects.Add(touchEffect);
    }
    ···
    void OnTouchEffectTouchAction(object sender, TouchActionEventArgs args)
    {
        SKPoint pixelLocation = ConvertToPixel(args.Location);
        SKPoint bitmapLocation = inverseBitmapMatrix.MapPoint(pixelLocation);

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                // Convert radius to bitmap/cropping scale
                float radius = inverseBitmapMatrix.ScaleX * RADIUS;

                // Find corner that the finger is touching
                int cornerIndex = croppingRect.HitTest(bitmapLocation, radius);

                if (cornerIndex != -1 && !touchPoints.ContainsKey(args.Id))
                {
                    TouchPoint touchPoint = new TouchPoint
                    {
                        CornerIndex = cornerIndex,
                        Offset = bitmapLocation - croppingRect.Corners[cornerIndex]
                    };

                    touchPoints.Add(args.Id, touchPoint);
                }
                break;

            case TouchActionType.Moved:
                if (touchPoints.ContainsKey(args.Id))
                {
                    TouchPoint touchPoint = touchPoints[args.Id];
                    croppingRect.MoveCorner(touchPoint.CornerIndex,
                                            bitmapLocation - touchPoint.Offset);
                    InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (touchPoints.ContainsKey(args.Id))
                {
                    touchPoints.Remove(args.Id);
                }
                break;
        }
    }

    SKPoint ConvertToPixel(Xamarin.Forms.Point pt)
    {
        return new SKPoint((float)(CanvasSize.Width * pt.X / Width),
                           (float)(CanvasSize.Height * pt.Y / Height));
    }
}
```

ハンドラーによって処理されるタッチイベント `TouchAction` は、デバイスに依存しない単位になります。 これらの最初の部分は、クラスの下部にあるメソッドを使用してピクセルに変換し、を `ConvertToPixel` 使用して単位に変換する必要があり `CroppingRectangle` `inverseBitmapMatrix` ます。

イベントの場合 `Pressed` 、 `TouchAction` ハンドラーは `HitTest` のメソッドを呼び出し `CroppingRectangle` ます。 これによって1以外のインデックスが返された場合、 &ndash; トリミング四角形の角の1つが操作されています。 そのインデックスとコーナーからの実際のタッチポイントのオフセットは、オブジェクトに格納され、 `TouchPoint` ディクショナリに追加され `touchPoints` ます。

イベントについて `Moved` `MoveCorner` は、のメソッドを呼び出して、 `CroppingRectangle` 縦横比を調整できるようにコーナーを移動します。

を使用するプログラムは、いつでも `PhotoCropperCanvasView` プロパティにアクセスでき `CroppedBitmap` ます。 このプロパティは、のプロパティを使用して、トリミングされ `Rect` `CroppingRectangle` たサイズの新しいビットマップを作成します。 `DrawBitmap`コピー先とコピー元の四角形を含むのバージョンは、元のビットマップのサブセットを抽出します。

```csharp
class PhotoCropperCanvasView : SKCanvasView
{
    ···
    SKBitmap bitmap;
    CroppingRectangle croppingRect;
    ···
    public SKBitmap CroppedBitmap
    {
        get
        {
            SKRect cropRect = croppingRect.Rect;
            SKBitmap croppedBitmap = new SKBitmap((int)cropRect.Width,
                                                  (int)cropRect.Height);
            SKRect dest = new SKRect(0, 0, cropRect.Width, cropRect.Height);
            SKRect source = new SKRect(cropRect.Left, cropRect.Top,
                                       cropRect.Right, cropRect.Bottom);

            using (SKCanvas canvas = new SKCanvas(croppedBitmap))
            {
                canvas.DrawBitmap(bitmap, source, dest);
            }

            return croppedBitmap;
        }
    }
    ···
}
```

## <a name="hosting-the-photo-cropper-canvas-view"></a>Photo cropper canvas ビューのホスト

トリミングロジックを処理する2つのクラスでは、 **[SkiaSharpFormsDemos](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** アプリケーションの **写真トリミング** ページの処理がほとんどありません。 XAML ファイルは、 `Grid` `PhotoCropperCanvasView` と [ **完了** ] ボタンをホストするために、をインスタンス化します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SkiaSharpFormsDemos.Bitmaps.PhotoCroppingPage"
             Title="Photo Cropping">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid x:Name="canvasViewHost"
              Grid.Row="0"
              BackgroundColor="Gray"
              Padding="5" />

        <Button Text="Done"
                Grid.Row="1"
                HorizontalOptions="Center"
                Margin="5"
                Clicked="OnDoneButtonClicked" />
    </Grid>
</ContentPage>
```

は、 `PhotoCropperCanvasView` 型のパラメーターを必要とするため、XAML ファイルでインスタンス化できません `SKBitmap` 。

代わりに、 `PhotoCropperCanvasView` リソースビットマップの1つを使用して、分離コードファイルのコンストラクターでがインスタンス化されます。

```csharp
public partial class PhotoCroppingPage : ContentPage
{
    PhotoCropperCanvasView photoCropper;
    SKBitmap croppedBitmap;

    public PhotoCroppingPage ()
    {
        InitializeComponent ();

        SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(GetType(),
            "SkiaSharpFormsDemos.Media.MountainClimbers.jpg");

        photoCropper = new PhotoCropperCanvasView(bitmap);
        canvasViewHost.Children.Add(photoCropper);
    }

    void OnDoneButtonClicked(object sender, EventArgs args)
    {
        croppedBitmap = photoCropper.CroppedBitmap;

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(croppedBitmap, info.Rect, BitmapStretch.Uniform);
    }
}
```

ユーザーは、トリミング四角形を操作できます。

[![Photo Cropper 1](cropping-images/PhotoCropping1.png "Photo Cropper 1")](cropping-images/PhotoCropping1-Large.png#lightbox)

適切なトリミング四角形が定義されている場合は、[ **完了** ] ボタンをクリックします。 ハンドラーは、 `Clicked` のプロパティからトリミングされたビットマップを取得 `CroppedBitmap` し、 `PhotoCropperCanvasView` `SKCanvasView` このトリミングされたビットマップを表示する新しいオブジェクトでページのすべての内容を置き換えます。

[![Photo Cropper 2](cropping-images/PhotoCropping2.png "Photo Cropper 2")](cropping-images/PhotoCropping2-Large.png#lightbox)

の2番目の引数 `PhotoCropperCanvasView` を 1.78 f (例:) に設定します。

```csharp
photoCropper = new PhotoCropperCanvasView(bitmap, 1.78f);
```

トリミング四角形が、高解像度テレビの 16 ~ 9 の縦横比の特性に制限されていることがわかります。

## <a name="dividing-a-bitmap-into-tiles"></a>ビットマップをタイルに分割する

Xamarin.Forms有名な14-15 パズルのバージョンは、 [_Mobile Apps Xamarin.Forms を作成する_](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)本の22章にあり、 [**XamagonXuzzle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle)としてダウンロードできます。 ただし、自分の写真ライブラリのイメージに基づいてパズルを追加すると、より楽しいものになります (多くの場合、難しくなります)。

このバージョンの14-15 パズルは **[SkiaSharpFormsDemos](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** アプリケーションに含まれており、 **写真パズル** というタイトルの一連のページで構成されています。

**PhotoPuzzlePage1** ファイルは、次の要素で構成されます `Button` 。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SkiaSharpFormsDemos.Bitmaps.PhotoPuzzlePage1"
             Title="Photo Puzzle">

    <Button Text="Pick a photo from your library"
            VerticalOptions="CenterAndExpand"
            HorizontalOptions="CenterAndExpand"
            Clicked="OnPickButtonClicked"/>

</ContentPage>
```

分離コードファイルは、 `Clicked` `IPhotoLibrary` ユーザーがフォトライブラリから写真を選択できるようにするために、依存サービスを使用するハンドラーを実装します。

```csharp
public partial class PhotoPuzzlePage1 : ContentPage
{
    public PhotoPuzzlePage1 ()
    {
        InitializeComponent ();
    }

    async void OnPickButtonClicked(object sender, EventArgs args)
    {
        IPhotoLibrary photoLibrary = DependencyService.Get<IPhotoLibrary>();
        using (Stream stream = await photoLibrary.PickPhotoAsync())
        {
            if (stream != null)
            {
                SKBitmap bitmap = SKBitmap.Decode(stream);

                await Navigation.PushAsync(new PhotoPuzzlePage2(bitmap));
            }
        }
    }
}
```

次に、メソッドはに移動し `PhotoPuzzlePage2` 、選択されたビットマップをコンストラクターでに渡します。

ライブラリから選択した写真は、写真ライブラリに表示されているようには向きませんが、回転または反転される可能性があります。 (これは、特に iOS デバイスの問題です)。そのため、では、 `PhotoPuzzlePage2` イメージを目的の向きに回転させることができます。 XAML ファイルには、 **90&#x00B0; Right** (時計回り)、 **90&#x00B0; Left** (反時計回り)、および **Done** というラベルが付いた3つのボタンが含まれています。

分離コードファイルは、 **[SkiaSharp ビットマップの作成と描画に関する](drawing.md#rotating-bitmaps)** 記事に示されているビットマップ回転ロジックを実装します。 ユーザーは、90画像を時計回りまたは反時計回りに90度回転できます。

```csharp
public partial class PhotoPuzzlePage2 : ContentPage
{
    SKBitmap bitmap;

    public PhotoPuzzlePage2 (SKBitmap bitmap)
    {
        this.bitmap = bitmap;

        InitializeComponent ();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform);
    }

    void OnRotateRightButtonClicked(object sender, EventArgs args)
    {
        SKBitmap rotatedBitmap = new SKBitmap(bitmap.Height, bitmap.Width);

        using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
        {
            canvas.Clear();
            canvas.Translate(bitmap.Height, 0);
            canvas.RotateDegrees(90);
            canvas.DrawBitmap(bitmap, new SKPoint());
        }

        bitmap = rotatedBitmap;
        canvasView.InvalidateSurface();
    }

    void OnRotateLeftButtonClicked(object sender, EventArgs args)
    {
        SKBitmap rotatedBitmap = new SKBitmap(bitmap.Height, bitmap.Width);

        using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
        {
            canvas.Clear();
            canvas.Translate(0, bitmap.Width);
            canvas.RotateDegrees(-90);
            canvas.DrawBitmap(bitmap, new SKPoint());
        }

        bitmap = rotatedBitmap;
        canvasView.InvalidateSurface();
    }

    async void OnDoneButtonClicked(object sender, EventArgs args)
    {
        await Navigation.PushAsync(new PhotoPuzzlePage3(bitmap));
    }
}
```

ユーザーが [ **完了** ] ボタンをクリックすると、 `Clicked` ハンドラーはに移動し `PhotoPuzzlePage3` 、ページのコンストラクターで最後に回転したビットマップを渡します。

`PhotoPuzzlePage3` 写真のトリミングを許可します。 このプログラムのタイルの4×4のグリッドに分割するには、正方形のビットマップが必要です。

**PhotoPuzzlePage3** ファイルには、、を `Label` `Grid` ホストする、 `PhotoCropperCanvasView` およびもう1つの **Done** ボタンが含まれています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SkiaSharpFormsDemos.Bitmaps.PhotoPuzzlePage3"
             Title="Photo Puzzle">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Label Text="Crop the photo to a square"
               Grid.Row="0"
               FontSize="Large"
               HorizontalTextAlignment="Center"
               Margin="5" />

        <Grid x:Name="canvasViewHost"
              Grid.Row="1"
              BackgroundColor="Gray"
              Padding="5" />

        <Button Text="Done"
                Grid.Row="2"
                HorizontalOptions="Center"
                Margin="5"
                Clicked="OnDoneButtonClicked" />
    </Grid>
</ContentPage>
```

分離コードファイルは、コンストラクターに渡されたビットマップを使用してをインスタンス化し `PhotoCropperCanvasView` ます。 2番目の引数としてに1が渡されることに注意して `PhotoCropperCanvasView` ください。 この縦横比1は、トリミング四角形を強制的に正方形にするためのものです。

```csharp
public partial class PhotoPuzzlePage3 : ContentPage
{
    PhotoCropperCanvasView photoCropper;

    public PhotoPuzzlePage3(SKBitmap bitmap)
    {
        InitializeComponent ();

        photoCropper = new PhotoCropperCanvasView(bitmap, 1f);
        canvasViewHost.Children.Add(photoCropper);
    }

    async void OnDoneButtonClicked(object sender, EventArgs args)
    {
        SKBitmap croppedBitmap = photoCropper.CroppedBitmap;
        int width = croppedBitmap.Width / 4;
        int height = croppedBitmap.Height / 4;

        ImageSource[] imgSources = new ImageSource[15];

        for (int row = 0; row < 4; row++)
        {
            for (int col = 0; col < 4; col++)
            {
                // Skip the last one!
                if (row == 3 && col == 3)
                    break;

                // Create a bitmap 1/4 the width and height of the original
                SKBitmap bitmap = new SKBitmap(width, height);
                SKRect dest = new SKRect(0, 0, width, height);
                SKRect source = new SKRect(col * width, row * height, (col + 1) * width, (row + 1) * height);

                // Copy 1/16 of the original into that bitmap
                using (SKCanvas canvas = new SKCanvas(bitmap))
                {
                    canvas.DrawBitmap(croppedBitmap, source, dest);
                }

                imgSources[4 * row + col] = (SKBitmapImageSource)bitmap;
            }
        }

        await Navigation.PushAsync(new PhotoPuzzlePage4(imgSources));
    }
}
```

**Done** ボタンハンドラーは、トリミングされたビットマップの幅と高さ (これらの2つの値は同じである必要があります) を取得し、それを15個の個別のビットマップに分割します。それぞれの幅と高さは1/4 です。 (16 ビットマップの最後のビットマップは作成されません)。 `DrawBitmap` コピー元とコピー先の四角形を持つメソッドを使用すると、大きなビットマップのサブセットに基づいてビットマップを作成できます。

## <a name="converting-to-no-locxamarinforms-bitmaps"></a>変換 ( Xamarin.Forms ビットマップに)

メソッドで `OnDoneButtonClicked` は、15ビットマップ用に作成された配列の型は次のようになり [`ImageSource`](xref:Xamarin.Forms.ImageSource) ます。

```csharp
ImageSource[] imgSources = new ImageSource[15];
```

`ImageSource`Xamarin.Formsビットマップをカプセル化する基本型です。 幸い、SkiaSharp では SkiaSharp ビットマップからビットマップに変換でき Xamarin.Forms ます。 **SkiaSharp** アセンブリは、 [`SKBitmapImageSource`](xref:SkiaSharp.Views.Forms.SKBitmapImageSource) から派生するクラスを定義しますが、 `ImageSource` SkiaSharp オブジェクトに基づいて作成することもでき `SKBitmap` ます。 `SKBitmapImageSource` また、との間の変換も定義 `SKBitmapImageSource` `SKBitmap` し `SKBitmap` ます。これは、オブジェクトがビットマップとして配列に格納される方法です Xamarin.Forms 。

```csharp
imgSources[4 * row + col] = (SKBitmapImageSource)bitmap;
```

このビットマップの配列は、コンストラクターとしてに渡され `PhotoPuzzlePage4` ます。 このページは完全であり Xamarin.Forms 、SkiaSharp は使用しません。 これは [**XamagonXuzzle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle)とよく似ているため、ここでは説明しませんが、選択した写真は15マスのタイルに分割されています。

[![写真パズル1](cropping-images/PhotoPuzzle1.png "写真パズル1")](cropping-images/PhotoPuzzle1-Large.png#lightbox)

[ **ランダム** 化] ボタンを押すと、すべてのタイルが合成されます。

[![写真パズル2](cropping-images/PhotoPuzzle2.png "写真パズル2")](cropping-images/PhotoPuzzle2-Large.png#lightbox)

これで、正しい順序に戻すことができます。 空白の四角形と同じ行または列にあるタイルをタップすると、それらを空白の四角形に移動できます。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)