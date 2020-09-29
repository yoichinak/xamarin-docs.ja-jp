---
title: Xamarin. Android TextureView
ms.prod: xamarin
ms.assetid: DD1F3D68-5DD8-4644-8A13-08AE7719DE30
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/30/2017
ms.openlocfilehash: 3085131e251a787965c91216106becb6c954da85
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457706"
---
# <a name="xamarinandroid-textureview"></a>Xamarin. Android TextureView

クラスは、 `TextureView` ビデオまたは OpenGL コンテンツストリームを表示できるようにするための、ハードウェアアクセラの2d レンダリングを使用するビューです。 たとえば、次のスクリーンショットは、 `TextureView` デバイスのカメラからライブフィードを表示する方法を示しています。

[![デバイスのカメラからのライブイメージのスクリーンショットの例](texture-view-images/22-textureviewcamera.png)](texture-view-images/22-textureviewcamera.png#lightbox)

`SurfaceView`OpenGL またはビデオコンテンツを表示するために使用することもできるクラスとは異なり、TextureView は別のウィンドウにはレンダリングされません。
そのため、 `TextureView` は他のビューと同様にビュー変換をサポートできます。 たとえば、を回転させるには、 `TextureView` プロパティを設定するだけで、そのプロパティを設定し `Rotation` て透明度を設定 `Alpha` します。

そのため、を使用すると、 `TextureView` 次のコードに示すように、カメラからライブストリームを表示し、変換することができます。

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

上のコードでは、 `TextureView` アクティビティのメソッドにインスタンスを作成 `OnCreate` し、そのアクティビティをのとして設定し `TextureView` `SurfaceTextureListener` ます。 にするために、 `SurfaceTextureListener` アクティビティはインターフェイスを実装し `TextureView.ISurfaceTextureListener` ます。 `OnSurfaceTextAvailable`が使用できる状態になると、システムはメソッドを呼び出し `SurfaceTexture` ます。 このメソッドで `SurfaceTexture` は、渡されたを取得し、カメラのプレビューテクスチャに設定します。 次に、上記の例のように、およびを設定するなど、通常のビューベースの操作を自由に実行 `Rotation` `Alpha` できます。 デバイスで実行されているアプリケーションは、次のようになります。

[![デバイスで実行されているアプリの例、イメージの表示](texture-view-images/17-textureviewdemo.png)](texture-view-images/17-textureviewdemo.png#lightbox)

を使用するには、 `TextureView` ハードウェアアクセラレーションを有効にする必要があります。これは、既定では API レベル14として設定されます。 また、この例ではカメラを使用しているため、 `android.permission.CAMERA` アクセス許可と機能の両方を `android.hardware.camera` **AndroidManifest.xml**に設定する必要があります。

## <a name="related-links"></a>関連リンク

- [TextureViewDemo (サンプル)](/samples/xamarin/monodroid-samples/textureviewdemo)/)