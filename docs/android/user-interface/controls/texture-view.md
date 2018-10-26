---
title: TextureView
ms.prod: xamarin
ms.assetid: DD1F3D68-5DD8-4644-8A13-08AE7719DE30
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/30/2017
ms.openlocfilehash: f43147d98599d36aa136e4ddc2975205e0e69e26
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122843"
---
# <a name="textureview"></a>TextureView

`TextureView`クラスは、ビデオや表示する OpenGL のコンテンツ ストリームを有効にするハードウェア アクセラレータによる 2D レンダリングを使用するビュー。 たとえば、次のスクリーン ショットを示しています、`TextureView`デバイスのカメラからライブ フィードを表示します。

[![デバイスのカメラからライブの画像の例のスクリーン ショット](texture-view-images/22-textureviewcamera.png)](texture-view-images/22-textureviewcamera.png#lightbox)

異なり、`SurfaceView`クラスは、OpenGL またはビデオ コンテンツを表示する使用することもできる、別のウィンドウには、TextureView はレンダリングされません。
そのため、`TextureView`は他のビューのような表示変換をサポートできます。 たとえば、回転、`TextureView`設定するだけで実現できますその`Rotation`プロパティを設定して、透過性その`Alpha`プロパティ。

そのため、使用、`TextureView`カメラからライブ ストリームの表示などを行うを変換し、次のコードに示すようにことができます。

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

上記のコードを作成、 `TextureView` 、アクティビティのインスタンス`OnCreate`メソッドとしてアクティビティを設定して、`TextureView`の`SurfaceTextureListener`します。 `SurfaceTextureListener`、アクティビティの実装、`TextureView.ISurfaceTextureListener`インターフェイス。 システムが呼び出す、`OnSurfaceTextAvailable`メソッドと、`SurfaceTexture`が使用できる状態です。 このメソッドでは、`SurfaceTexture`で通過し、カメラのプレビューのテクスチャに設定します。 自由に設定するなど、通常ビュー ベースの操作を実行し、`Rotation`と`Alpha`上記の例のようにします。 デバイスで実行されている、作成されたアプリケーションは、以下に示します。

[![イメージを表示する、デバイスで実行されているアプリの例](texture-view-images/17-textureviewdemo.png)](texture-view-images/17-textureviewdemo.png#lightbox)

使用する、`TextureView`ハードウェア高速化する必要がありますが有効にしている API レベル 14 を既定になります。 またこの例は、カメラを使用しているため、両方、`android.permission.CAMERA`アクセス許可と`android.hardware.camera`で機能を設定する必要があります、 **AndroidManifest.xml**。



## <a name="related-links"></a>関連リンク

- [TextureViewDemo (サンプル)](https://developer.xamarin.com/samples/monodroid/TextureViewDemo/)
- [Ice Cream Sandwich の概要](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 プラットフォーム](http://developer.android.com/sdk/android-4.0.html)
