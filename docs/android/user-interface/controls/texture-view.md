---
title: TextureView
ms.topic: article
ms.prod: xamarin
ms.assetid: DD1F3D68-5DD8-4644-8A13-08AE7719DE30
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/30/2017
ms.openlocfilehash: 7048962a93f5bd99f4a27062ecc6cc2d5b2d3398
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="textureview"></a>TextureView

`TextureView`クラスは、ビデオまたは表示する OpenGL のコンテンツ ストリームを有効にするハードウェア アクセラレータの 2D レンダリングを使用するビュー。 たとえば、次のスクリーン ショットを示しています、`TextureView`デバイスのカメラからライブ フィードを表示します。

[![デバイスのカメラからライブのイメージの例のスクリーン ショット](texture-view-images/22-textureviewcamera.png)](texture-view-images/22-textureviewcamera.png)

異なり、 `SurfaceView` OpenGL またはビデオ コンテンツを表示する使用することもできる、クラス、TextureView は別のウィンドウに表示されません。
したがって、`TextureView`を他のビューのような表示変換をサポートすることができます。 たとえば、回転、`TextureView`を設定するだけで実現できますその`Rotation`プロパティを設定して、透過性、`Alpha`プロパティ、およびなです。

そのため、`TextureView`して今すぐ行うディスプレイのように、ライブ ストリームをカメラからを変換し、次のコードに示すようにします。

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

上記のコードを作成、`TextureView`アクティビティのインスタンス`OnCreate`メソッドとしてアクティビティを設定し、`TextureView`の`SurfaceTextureListener`します。 ある、 `SurfaceTextureListener`、アクティビティの実装、`TextureView.ISurfaceTextureListener`インターフェイスです。 システムが呼び出す、`OnSurfaceTextAvailable`メソッドと、`SurfaceTexture`が使用できる状態です。 このメソッドで、`SurfaceTexture`に渡され、カメラのプレビューのテクスチャに設定するがします。 自由に設定するなど、通常のビューに基づく操作を実行し、`Rotation`と`Alpha`上記の例のように、します。 デバイスで実行されている、結果として得られるアプリケーションは、次に示します。

[![イメージを表示する、デバイスで実行されているアプリの例](texture-view-images/17-textureviewdemo.png)](texture-view-images/17-textureviewdemo.png)

使用する、 `TextureView`、ハードウェア アクセラレータ必要がある有効な既定では API レベル 14 ことができます。 この例は、カメラを使用するため、両方、`android.permission.CAMERA`権限および`android.hardware.camera`機能を設定する必要があります、 **AndroidManifest.xml**です。



## <a name="related-links"></a>関連リンク

- [TextureViewDemo (sample)](https://developer.xamarin.com/samples/monodroid/TextureViewDemo/)
- [アイスクリーム サンドイッチの概要](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 プラットフォーム](http://developer.android.com/sdk/android-4.0.html)
