---
title: SkiaSharp のビットマップをトリミングします。
description: SkiaSharp を使用して対話的に desribing をトリミングする四角形のユーザー インターフェイスを設計する方法について説明します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 0A79AB27-C69F-4376-8FFE-FF46E4783F30
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
ms.openlocfilehash: 653904da37354db52ef6bbd303355e98ddc1582f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122635"
---
# <a name="cropping-skiasharp-bitmaps"></a>SkiaSharp のビットマップをトリミングします。

[**の作成と描画 SkiaSharp ビットマップ**](drawing.md)方法を説明する記事、`SKBitmap`にオブジェクトを渡すことができます、`SKCanvas`コンス トラクター。 ビットマップにレンダリングされるそのキャンバス原因グラフィックスで呼び出される任意の描画メソッド。 描画メソッドが含まれます`DrawBitmap`、つまり、この手法でを転送する 1 つのビットマップの一部またはすべて別のビットマップにおそらく適用された変換を許可します。

その方法を使用するには呼び出すことで、ビットマップをトリミングするため、 [ `DrawBitmap` ](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKRect,SkiaSharp.SKPaint))元とコピー先の四角形を持つメソッド。

```csharp
canvas.DrawBitmap(bitmap, sourceRect, destRect);
```

ただし、多くの場合、トリミングを実装するアプリケーションは、対話形式で、トリミングする四角形を選択するユーザー インターフェイスを提供します。

![サンプルのトリミング](cropping-images/CroppingSample.png "サンプルのトリミング")

この記事では、そのインターフェイスについて説明します。

## <a name="encapsulating-the-cropping-rectangle"></a>四角形をトリミングをカプセル化します。

という名前のクラスでトリミング ロジックの一部を分離することをお勧め`CroppingRectangle`します。 これは一般にトリミングされるビットマップのサイズ、最大の四角形と縦横比は省略可能なコンス トラクターのパラメーターが含まれます。 コンス トラクターが最初に初期トリミング四角形をパブリックで、これを定義、`Rect`型のプロパティ`SKRect`します。 この初期トリミングする四角形は、幅の 80% とビットマップの四角形の高さが、縦横比が指定されている場合、調整。

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

1 つの有用な情報を`CroppingRectangle`もの配列は、使用できるように`SKPoint`左、右、右、下および左の順序でトリミングする四角形の 4 つの角に対応する値。

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

この配列が呼び出される次のメソッドで使用される`HitTest`します。 `SKPoint`パラメーターは指タッチに対応するポイント、またはマウスをクリックします。 メソッドのインデックス (0、1、2、または 3) を返しますで指定された範囲内で指やマウスのポインターの操作された、コーナーに対応する、`radius`パラメーター。 

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

場合は、タッチまたはマウス ポイント内でした`radius`上隅にあるすべてのユニットは、メソッドを返します&ndash;1。

最後のメソッドで`CroppingRectangle`と呼びます`MoveCorner`、タッチまたはマウスの移動に応答と呼ばれます。 2 つのパラメーターは、移動される上隅およびその隅の新しい場所のインデックスを示します。 メソッドの最初の半分を調整します。 四角形をトリミング、隅にあるのが常にの境界内に新しい場所に基づく`maxRect`、これは、ビットマップのサイズです。 このロジックも考慮に入れた、 `MINIMUM` nothing に四角形をトリミングを折りたたみを回避するためにフィールド。

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

メソッドの 2 つ目の半分は、省略可能な縦横比を調整します。

ピクセル単位でこのクラスのすべてのものであることに留意してください。

## <a name="a-canvas-view-just-for-cropping"></a>だけをトリミングするためキャンバスの表示

`CroppingRectangle`説明したクラスによって使用されます、`PhotoCropperCanvasView`から派生したクラス`SKCanvasView`します。 このクラスは、トリミングする四角形を変更するため、タッチまたはマウス イベントを処理するほか、ビットマップとトリミングの四角形を表示する責任を負います。

`PhotoCropperCanvasView`コンス トラクターには、ビットマップが必要です。 縦横比は省略可能です。 コンス トラクターが型のオブジェクトをインスタンス化`CroppingRectangle`このビットマップの縦横比に基づくし、フィールドとして保存します。

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

このクラスはから派生するので`SKCanvasView`のハンドラーをインストールする必要はありません、`PaintSurface`イベント。 代わりにオーバーライドできますその`OnPaintSurface`メソッド。 メソッドは、ビットマップを表示し、いくつかを使用して`SKPaint`オブジェクトの現在のトリミングの四角形を描画するためにフィールドとして保存します。

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

内のコード、`CroppingRectangle`クラスがに基づいて、ビットマップのピクセル サイズの四角形をトリミングします。 ただしでのビットマップの表示、`PhotoCropperCanvasView`クラスが表示領域のサイズに基づいて調整します。 `bitmapScaleMatrix`で計算される、`OnPaintSurface`が表示されているサイズとビットマップの位置にビットマップのピクセルからマップをオーバーライドします。 このマトリックスは、ビットマップを基準に表示できるように、トリミングする四角形を変換には使用されます。

最後の行、`OnPaintSurface`上書きの逆関数を受け取り、`bitmapScaleMatrix`として保存し、`inverseBitmapMatrix`フィールド。 これは、タッチ処理のために使用されます。

A`TouchEffect`フィールドとしてオブジェクトがインスタンス化され、コンス トラクターにハンドラーをアタッチする、`TouchAction`イベントが、`TouchEffect`に追加する必要があります、`Effects`のコレクション、_親_の`SKCanvasView`派生の行われるように、`OnParentSet`をオーバーライドします。

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

タッチ イベントで処理、`TouchAction`ハンドラーがデバイスに依存しない単位。 これらを使用してピクセルに変換する必要が最初、 `ConvertToPixel` 、クラスの下部にあるメソッドに変換し、`CroppingRectangle`ユニットを使用して`inverseBitmapMatrix`します。

`Pressed` 、イベント、`TouchAction`ハンドラーの呼び出し、`HitTest`メソッドの`CroppingRectangle`します。 インデックスを返しますこれ以外の場合&ndash;1、トリミングする四角形の四隅のいずれかの操作されます。 インデックスとの隅にある実際のタッチ点のオフセットが格納されていること、`TouchPoint`オブジェクトし、を追加、`touchPoints`ディクショナリ。

`Moved` 、イベント、`MoveCorner`メソッドの`CroppingRectangle`縦横比を調整可能で、下に移動すると呼びます。

いつでもでもプログラムを使用して、`PhotoCropperCanvasView`にアクセスできる、`CroppedBitmap`プロパティ。 このプロパティを使用して、`Rect`のプロパティ、`CroppingRectangle`トリミング、サイズの新しいビットマップを作成します。 バージョンの`DrawBitmap`送信先と送信元の四角形し抽出元のビットマップのサブセット。

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

## <a name="hosting-the-photo-cropper-canvas-view"></a>写真の cropper キャンバスのビューをホストしています。

トリミングのロジックを処理するこれら 2 つのクラスで、**写真のトリミング**ページで、 **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** アプリケーションがごくわずかな作業を行います。 XAML ファイルのインスタンスを作成、`Grid`ホストに、`PhotoCropperCanvasView`と**完了**ボタン。

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

`PhotoCropperCanvasView`型のパラメーターを必要とするために、XAML ファイルでインスタンス化できない`SKBitmap`します。

代わりに、`PhotoCropperCanvasView`リソース ビットマップのいずれかを使用して、分離コード ファイルのコンス トラクターでインスタンス化されます。

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

ユーザーは、四角形をトリミングを操作できます。

[![写真の Cropper 1](cropping-images/PhotoCropping1.png "Cropper 1 の写真")](cropping-images/PhotoCropping1-Large.png#lightbox)

優れたトリミング四角形が定義されている場合にクリックして、**完了**ボタンをクリックします。 `Clicked`ハンドラーからトリミング後のビットマップを取得します、`CroppedBitmap`プロパティの`PhotoCropperCanvasView`、しを新しいページのすべてのコンテンツを置き換えます`SKCanvasView`このトリミング後のビットマップを表示するオブジェクト。

[![写真の Cropper 2](cropping-images/PhotoCropping2.png "Cropper 2 の写真")](cropping-images/PhotoCropping2-Large.png#lightbox)

2 番目の引数を設定してみてください`PhotoCropperCanvasView`1.78f (たとえば) にします。

```csharp
photoCropper = new PhotoCropperCanvasView(bitmap, 1.78f);
```

高精細テレビの特性 16 ~ 9 の縦横比に制限されている、トリミングの四角形が表示されます。

<a name="tile-division" />

## <a name="dividing-a-bitmap-into-tiles"></a>ビットマップをタイルに分割します。

著名人の Xamarin.Forms バージョン 14 ~ 15 のパズルは、書籍の第 22 章で登場した[_を Xamarin.Forms での Mobile Apps の作成_](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)としてダウンロードできます[ **XamagonXuzzle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle)します。 ただし、パズルになりより楽しく (多くの場合、さらに難しく) フォト ライブラリからイメージに基づいて場合。

14 ~ 15 のパズルのこのバージョンの一部は、 **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** アプリケーションで構成される、一連のページ「**パズルの写真**。

**PhotoPuzzlePage1.xaml**ファイルから成る、 `Button`:

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

実装する分離コード ファイル、`Clicked`ハンドラーを使用する、`IPhotoLibrary`ユーザーがフォト ライブラリから写真を選択できるようにする依存関係サービス。

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

メソッドに移動し、 `PhotoPuzzlePage2`、選択したビットマップ コンス トラクターに渡します。

フォト ライブラリで登場した回転または上下をライブラリから選択した写真は指向しないことができます。 (これは、iOS デバイスに特に問題です)。そのため、`PhotoPuzzlePage2`必要な向きにイメージを回転することができます。 XAML ファイルには、ラベルの付いた 3 つのボタンが含まれています。 **90&#x00B0;右**(つまりまで、時計回り) **90&#x00B0;左**(反時計回りに回転)、および**完了**します。

分離コード ファイル、情報の記事に示すようにビットマップ回転ロジックを実装する **[SkiaSharp ビットマップの描画の作成と](drawing.md#rotating-bitmaps)** します。 ユーザーは、何回でも時計回りまたは反時計回りに 90 度のイメージを回転できます。 

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

ユーザーがクリックすると、**完了** ボタン、`Clicked`ハンドラーに移動する`PhotoPuzzlePage3`、最終的な回転したビットマップをページのコンス トラクターに渡します。

`PhotoPuzzlePage3` 写真のトリミングを使用できます。 プログラムでは、正方形のビットマップを 4 個のグリッドのタイルに分割する必要があります。

**PhotoPuzzlePage3.xaml**ファイルが含まれています、 `Label`、`Grid`ホストに、 `PhotoCropperCanvasView`、もう 1 つと**完了**ボタン。

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

分離コード ファイルをインスタンス化、`PhotoCropperCanvasView`ビットマップをそのコンス トラクターに渡されます。 1 は、2 番目の引数として渡される通知`PhotoCropperCanvasView`します。 1 の場合は、この縦横比は、正方形にする四角形をトリミングを強制します。

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

**完了** ボタンのハンドラーが (これら 2 つの値は同じはず) トリミングされたビットマップの高さと幅を取得し、し、それぞれが 1/4 15 の独立したビットマップに分割し、幅と、元の高さ。 (可能な 16 のビットマップの最後のタスクは作成されません)。`DrawBitmap`元とコピー先の四角形を持つメソッドより大きなビットマップのサブセットに基づいて作成されるビットマップを使用します。

## <a name="converting-to-xamarinforms-bitmaps"></a>Xamarin.Forms のビットマップに変換します。

`OnDoneButtonClicked`型のメソッド、15 のビットマップを作成した配列が[ `ImageSource` ](xref:Xamarin.Forms.ImageSource):

```csharp
ImageSource[] imgSources = new ImageSource[15];
```

`ImageSource` ビットマップをカプセル化する Xamarin.Forms の基本型です。 さいわい、SkiaSharp SkiaSharp ビットマップから Xamarin.Forms ビットマップへの変換をできます。 **SkiaSharp.Views.Forms**アセンブリを定義、 [ `SKBitmapImageSource` ](xref:SkiaSharp.Views.Forms.SKBitmapImageSource)クラスから派生した`ImageSource`、SkiaSharp に基づいて作成できますが、`SKBitmap`オブジェクト。 `SKBitmapImageSource` 間の変換を定義`SKBitmapImageSource`と`SKBitmap`、それで方法`SKBitmap`オブジェクトは、Xamarin.Forms のビットマップとして配列に格納されます。

```csharp
imgSources[4 * row + col] = (SKBitmapImageSource)bitmap;
```

このビットマップの配列が、コンス トラクターとして渡される`PhotoPuzzlePage4`します。 そのページは、Xamarin.Forms では完全され、すべて SkiaSharp を使用しません。 非常に似ています[ **XamagonXuzzle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle)、ので、ここで説明しませんが、15 の正方形のタイルに分割された、選択した写真が表示されます。

[![写真のパズル 1](cropping-images/PhotoPuzzle1.png "パズル 1 の写真")](cropping-images/PhotoPuzzle1-Large.png#lightbox)

キーを押して、 **Randomize**  ボタンをすべてのタイルをどのように。

[![写真のパズル 2](cropping-images/PhotoPuzzle2.png "パズル 2 の写真")](cropping-images/PhotoPuzzle2-Large.png#lightbox)

ここで正しい順序で再度に配置できます。 空の四角形に移動することで同じ行または列を空の四角形として、タイルをタップすることができます。 

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
