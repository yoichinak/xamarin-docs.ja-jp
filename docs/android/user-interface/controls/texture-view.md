---
title: Xamarin. Android TextureView
ms.prod: xamarin
ms.assetid: DD1F3D68-5DD8-4644-8A13-08AE7719DE30
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/30/2017
ms.openlocfilehash: 5d6b1b01cf9597a1d7ae9de762eff1514b494663
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029120"
---
# <a name="xamarinandroid-textureview"></a>Xamarin. Android TextureView

`TextureView` クラスは、ビデオまたは OpenGL コンテンツストリームを表示できるようにするための、ハードウェアアクセラの2D レンダリングを使用するビューです。 たとえば、次のスクリーンショットは、デバイスのカメラからライブフィードを表示する `TextureView` を示しています。

[![デバイスのカメラからのライブイメージのスクリーンショットの例](texture-view-images/22-textureviewcamera.png)](texture-view-images/22-textureviewcamera.png#lightbox)

OpenGL またはビデオコンテンツの表示にも使用できる `SurfaceView` クラスとは異なり、TextureView は別のウィンドウにはレンダリングされません。
そのため、`TextureView` は他のビューと同様にビュー変換をサポートできます。 たとえば、`TextureView` を回転させるには、単にその `Rotation` プロパティを設定し、その `Alpha` プロパティを設定してその透明度を設定します。

そのため、`TextureView` を使用すると、次のコードに示すように、カメラからライブストリームを表示して変換することができます。

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

上記のコードでは、アクティビティの `OnCreate` メソッドに `TextureView` インスタンスを作成し、アクティビティを `TextureView`の `SurfaceTextureListener`として設定します。 `SurfaceTextureListener`にするために、アクティビティは `TextureView.ISurfaceTextureListener` インターフェイスを実装します。 `SurfaceTexture` が使用できる状態になると、システムは `OnSurfaceTextAvailable` メソッドを呼び出します。 このメソッドでは、渡された `SurfaceTexture` を取得し、カメラのプレビューテクスチャに設定します。 次に、上記の例のように、`Rotation` や `Alpha`の設定など、通常のビューベースの操作を自由に実行できます。 デバイスで実行されているアプリケーションは、次のようになります。

[デバイスで実行されているアプリの例を![し、イメージを表示する](texture-view-images/17-textureviewdemo.png)](texture-view-images/17-textureviewdemo.png#lightbox)

`TextureView`を使用するには、ハードウェアアクセラレータを有効にする必要があります。これは、既定では API レベル14のようになります。 また、この例ではカメラを使用しているため、`android.permission.CAMERA` のアクセス許可と `android.hardware.camera` 機能の両方が**Androidmanifest .xml**で設定されている必要があります。

## <a name="related-links"></a>関連リンク

- [TextureViewDemo (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/textureviewdemo)
- [アイスクリームサンドイッチの導入](https://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 プラットフォーム](https://developer.android.com/sdk/android-4.0.html)
