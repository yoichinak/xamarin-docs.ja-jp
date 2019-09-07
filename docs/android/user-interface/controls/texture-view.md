---
title: Xamarin. Android TextureView
ms.prod: xamarin
ms.assetid: DD1F3D68-5DD8-4644-8A13-08AE7719DE30
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/30/2017
ms.openlocfilehash: 2857033c5cd69e9696d2ce82feaf8212300da2c5
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70764812"
---
# <a name="xamarinandroid-textureview"></a>Xamarin. Android TextureView

`TextureView`クラスは、ビデオまたは OpenGL コンテンツストリームを表示できるようにするための、ハードウェアアクセラの2d レンダリングを使用するビューです。 たとえば、次のスクリーンショットは、 `TextureView`デバイスのカメラからライブフィードを表示する方法を示しています。

[![デバイスのカメラからのライブイメージのスクリーンショットの例](texture-view-images/22-textureviewcamera.png)](texture-view-images/22-textureviewcamera.png#lightbox)

OpenGL またはビデオコンテンツを表示するために使用することもできるクラスとは異なり、textureviewは別のウィンドウにはレンダリングされません。`SurfaceView`
そのため`TextureView` 、は他のビューと同様にビュー変換をサポートできます。 たとえば、を`TextureView`回転させるには、 `Rotation`プロパティを設定するだけで、その`Alpha`プロパティを設定して透明度を設定します。

そのため、を`TextureView`使用すると、次のコードに示すように、カメラからライブストリームを表示し、変換することができます。

```csharp
public class TextureViewActivity : Activity,
    TextureView.ISurfaceTextureListener
{
    Camera _camera;
    TextureView _textureView;
       
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        _textureView = new TextureView (this);
        _textureView.SurfaceTextureListener = this;
           
        SetContentView (_textureView);
    }
       
    public void OnSurfaceTextureAvailable (
        Android.Graphics.SurfaceTexture surface,
        int width, int height)
    {
        _camera = Camera.Open ();
        var previewSize = _camera.GetParameters ().PreviewSize;
        _textureView.LayoutParameters =
            new FrameLayout.LayoutParams (previewSize.Width,
                previewSize.Height, (int)GravityFlags.Center);

        try {
            _camera.SetPreviewTexture (surface);
            _camera.StartPreview ();
        } catch (Java.IO.IOException ex) {
            Console.WriteLine (ex.Message);
        }
           
        // this is the sort of thing TextureView enables
        _textureView.Rotation = 45.0f;
        _textureView.Alpha = 0.5f;
    }
    …
}
```

上のコードでは`TextureView` 、アクティビティの`OnCreate`メソッドにインスタンスを作成し、その`TextureView`アクティビティを`SurfaceTextureListener`のとして設定します。 にするために、アクティビティは`TextureView.ISurfaceTextureListener`インターフェイスを実装します。 `SurfaceTextureListener` `SurfaceTexture`が使用できる状態に`OnSurfaceTextAvailable`なると、システムはメソッドを呼び出します。 このメソッドでは、渡され`SurfaceTexture`たを取得し、カメラのプレビューテクスチャに設定します。 次に、上記の例のように、 `Rotation`および`Alpha`を設定するなど、通常のビューベースの操作を自由に実行できます。 デバイスで実行されているアプリケーションは、次のようになります。

[![デバイスで実行されているアプリの例、イメージの表示](texture-view-images/17-textureviewdemo.png)](texture-view-images/17-textureviewdemo.png#lightbox)

を使用`TextureView`するには、ハードウェアアクセラレーションを有効にする必要があります。これは、既定では API レベル14として設定されます。 また、この例ではカメラを使用して`android.permission.CAMERA`いるため、 `android.hardware.camera`アクセス許可と機能の両方が**androidmanifest .xml**で設定されている必要があります。

## <a name="related-links"></a>関連リンク

- [TextureViewDemo (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/textureviewdemo)
- [アイスクリームサンドイッチの導入](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 プラットフォーム](https://developer.android.com/sdk/android-4.0.html)
