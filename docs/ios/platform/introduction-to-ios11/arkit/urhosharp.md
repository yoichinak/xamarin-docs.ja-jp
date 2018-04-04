---
title: UrhoSharp で ARKit の使用
ms.prod: xamarin
ms.assetid: 877AF974-CC2E-48A2-8E1A-0EF9ABF2C92D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/01/2016
ms.openlocfilehash: 95c9c602d0bfe1b77fda453a137dfdfc12a975c9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="using-arkit-with-urhosharp"></a>UrhoSharp で ARKit の使用

導入に伴い[ARKit](https://developer.apple.com/arkit/)Apple が簡単になった、augmented reality アプリケーションを作成する開発者向けです。 ARKit がデバイスの正確な位置を追跡し、世界のさまざまな画面を検出し、blend のコードに ARKit から到着したデータを開発者がします。

[UrhoSharp](~/graphics-games/urhosharp/index.md) 3D アプリケーションを作成に使用できる包括的で使いやすい 3D API を提供します。   これらのどちらも値が、実世界に関する物理情報を提供する ARKit と結果を表示する Urho 混ざりです。

このページでは、優れた augmented reality アプリケーションを作成するには、一緒にこれら 2 つの環境に接続する方法について説明します。


## <a name="the-basics"></a>基本事項

実行する、世界の上に存在 3D コンテンツ表示される iPhone で。   Blend のコンテンツを含む、3 D、携帯電話のカメラからの内容と 3D オブジェクトは、そのルームの一部であるどおりに動作 - これは、この環境にオブジェクトのアンカーで携帯電話のユーザーがいることを確認する室内を移動するとします。

![ARKit でアニメーションの図](urhosharp-images/image1.gif)


取り上げる Urho ライブラリを 3D のアセットを読み込んで、世界中に配置して、ARKit を使用では、世界の電話の場所と同様に、カメラからビデオ ストリームを取得します。   ユーザーは、自分の電話に移動すると、Urho エンジンが表示されている座標システムを更新するのに場所に変更を使用します。

これにより、3 D 空間にオブジェクトを配置して、ユーザーが移動、3 D のオブジェクトの場所が反映されますが配置される場所と場所。

## <a name="setting-up-your-application"></a>アプリケーションの設定

### <a name="ios-application-launch"></a>iOS アプリケーションの起動

IOS アプリケーションを作成し、3 D コンテンツを起動する必要があります、実装するのサブクラスを作成することで、これを行う、 [ `Urho.Application` ](https://developer.xamarin.com/api/type/Urho.Application/)オーバーライドすることで、セットアップ コードを提供し、`Start`メソッドです。  これは、ここで、シーンのデータ、イベント ハンドラーに設定の設定を取得します。

導入されています、`Urho.ArkitApp`クラスをサブクラスとして持つ`Urho.Application`およびその`Start`メソッドでは、面倒な作業です。   型の基本クラスを変更するアプリケーションは、既存の Urho を行うには必要な`Urho.ArkitApp`世界で、urho シーンを実行するアプリケーションがあるとします。

### <a name="the-arkitapp-class"></a>ArkitApp クラス

このクラスでは、オペレーティング システムによって配信される便利な既定値、キー オブジェクトの一部に、両方のシーンのセットだけでなく ARKit イベントの処理が提供します。

セットアップ実行、`Start`仮想メソッドです。   サブクラスでこのメソッドをオーバーライドするときにする必要があることを確認して、親にチェーンするを使用して`base.Start()`独自の実装にします。

`Start`メソッドは、シーン、ビューポート、カメラおよびディレクショナル ライトを設定し、サーフェスのパブリック プロパティとして。

- [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Scene/)オブジェクトを保持するには
- 方向[ `Light` ](https://developer.xamarin.com/api/type/Urho.Light/)シャドウ、および場所を利用し、`LightNode`プロパティ
- [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera/) ARKit アプリケーションに更新プログラムを配信する際にコンポーネントが含まれるが更新されると
- [ `ViewPort` ](https://developer.xamarin.com/api/type/Urho.Viewport/)結果を表示します。


### <a name="your-code"></a>コード

サブクラス化する必要があります、`ArkitApp`クラスし、オーバーライド、`Start`メソッドです。   最初に行う必要があります、メソッドは、チェーンまで、`ArkitApp.Start`を呼び出して`base.Start()`です。  その後をシーンにオブジェクトを追加、ライト、shadows または処理するイベントのカスタマイズを ArkitApp のプロパティ設定のいずれかを使用できます。

ARKit/UrhoSharp の例では、テクスチャのアニメーションの文字を読み込みし、次の実装と、アニメーションの再生します。

    ```csharp
    public class MutantDemo : ArkitApp
    {
        [Preserve]
        public MutantDemo(ApplicationOptions opts) : base(opts) { }

        Node mutantNode;

        protected override void Start()
        {
            base.Start ();

            // Mutant
            mutantNode = Scene.CreateChild();
            mutantNode.Rotation = new Quaternion(x: 0, y:15, z:0);
            mutantNode.Position = new Vector3(0, -1f, 2f); /*two meters away*/
            mutantNode.SetScale(0.5f);

            var mutant = mutantNode.CreateComponent<AnimatedModel>();
            mutant.Model = ResourceCache.GetModel("Models/Mutant.mdl");
            mutant.Material = ResourceCache.GetMaterial("Materials/mutant_M.xml");

            var animation = mutantNode.CreateComponent<AnimationController>();
            animation.Play("Animations/Mutant_HipHop1.ani", 0, true, 0.2f);
        }
    }
    ```

3D コンテンツの augmented reality で表示されるこの時点で行う必要があることを実際にします。

あなたの資産をこの形式にエクスポートする必要があります、Urho は 3D モデルや、アニメーション、カスタム書式を使用します。   などのツールを使用することができます、 [Urho3D Blender アドイン](https://github.com/reattiva/Urho3D-Blender)と[UrhoAssetImporter](https://github.com/EgorBo/UrhoAssetImporter) Urho で必要な形式に 3D の最大これら、DAE、OBJ、Blend、人気のある形式からこれらのアセットを変換することができます。

3D を Urho を使用してアプリケーションの作成の詳細については、次を参照してください。、 [UrhoSharp 概要](~/graphics-games/urhosharp/introduction.md)ガイドです。

## <a name="arkitapp-in-depth"></a>徹底的に ArkitApp

> [!NOTE]
> このセクションでは UrhoSharp と ARKit の既定のエクスペリエンスをカスタマイズする、または統合のしくみに関するより深い洞察を取得する必要がある開発者を対象としています。   このセクションを読む必要はありません。

ARKit API が非常に単純ですが、作成し、構成、 [ARSession](https://developer.apple.com/documentation/arkit/arsession)オブジェクトを配信を開始し、 [ARFrame](https://developer.apple.com/documentation/arkit/arframe)オブジェクト。   デバイスの推定の実際の位置と同様に、カメラでキャプチャされたイメージ両方にはが含まれます。

私たちは、コンテンツを含む、3 D、お待ちしておりますカメラによって配信される、イメージを作成するはし、UrhoSharp デバイスの場所や位置の可能性を一致するようにカメラを調整します。

次の図は、何が起きて、`ArkitApp`クラス。

[![クラスと、ArkitApp 画面のダイアグラム](urhosharp-images/image2.png)](urhosharp-images/image2.png#lightbox)

### <a name="rendering-the-frames"></a>フレームのレンダリング

考え方は単純、イメージの合計を生成するために、3 D グラフィックスにカメラからビデオを結合します。     キャプチャしたイメージの一連のシーケンスで取得おと Urho シーンでこの入力を混在おされます。

挿入がそれを実行する最も簡単な方法、 [ `RenderPathCommand` ](https://developer.xamarin.com/api/type/Urho.RenderPathCommand/) 、メイン[ `RenderPath`](https://developer.xamarin.com/api/type/Urho.RenderPath/)です。  これは、一連の 1 つのフレームを描画する実行されるコマンドです。  このコマンドに渡して、テクスチャで、ビューポートが挿入されます。    これを設定、プロセスは、最初のフレームにし、番目で実際の定義を行う**ARRenderPath.xml**この時点で読み込まれるファイル。

ただし、はこれら 2 つの環境を一緒に合成する 2 つの問題に直面します。


1. Ios の場合、GPU のテクスチャが 2 の累乗である解像度をいる必要がありますが、おをカメラから取得するフレームには、例 2 の累乗である解決策はありません: 1280 x 720 です。
2. フレームがでエンコードされた[YUV](https://en.wikipedia.org/wiki/YUV)形式、2 つのイメージの輝度およびおよび彩度によって表されます。

YUV フレームは、次の 2 つの異なる解像度で提供されます。  輝度 (基本的に、グレースケール イメージ) と容量より小さい 640 x 360 色光度コンポーネントを表す 1280 x 720 の画像:

![結合 Y UV コンポーネントを示す画像](urhosharp-images/image3.png)


OpenGL ES を使用して、完全な色分けされたイメージを描画するには、テクスチャのスロットから輝度 (Y 成分) と色の光度 (UV 面) を取得する小さなシェーダーを作成する必要があります。  UrhoSharp で、名前に"sDiffMap"と"sNormalMap"とある RGB 形式に変換します。

```csharp
mat4 ycbcrToRGBTransform = mat4(
    vec4(+1.0000, +1.0000, +1.0000, +0.0000),
    vec4(+0.0000, -0.3441, +1.7720, +0.0000),
    vec4(+1.4020, -0.7141, +0.0000, +0.0000),
    vec4(-0.7010, +0.5291, -0.8860, +1.0000));

vec4 ycbcr = vec4(texture2D(sDiffMap, vTexCoord).r,
                    texture2D(sNormalMap, vTexCoord).ra, 1.0);
gl_FragColor = ycbcrToRGBTransform * ycbcr;
```

2 つの解決の電源が入っていないテクスチャを表示するためには、次のパラメーターを持つテクスチャ 2d を定義する必要があります。

```chsarp
// texture for UV-plane;
cameraUVtexture = new Texture2D();
cameraUVtexture.SetNumLevels(1);
cameraUVtexture.SetSize(640, 360, Graphics.LuminanceAlphaFormat, TextureUsage.Dynamic);
cameraUVtexture.FilterMode = TextureFilterMode.Bilinear;
cameraUVtexture.SetAddressMode(TextureCoordinate.U, TextureAddressMode.Clamp);
cameraUVtexture.SetAddressMode(TextureCoordinate.V, TextureAddressMode.Clamp);
```

したがっては背景としてキャプチャしたイメージを表示し、そのような恐ろしい変異形上の任意のシーンをレンダリングできません。

### <a name="adjusting-the-camera"></a>カメラを調整します。

`ARFrame`オブジェクトでは、推定デバイスの位置も含まれます。  これで、ゲーム カメラ ARFrame を移動する必要がありますが、ARKit する前に、でしたトラック デバイスの方向 (ロール、ピッチ、およびヨー) とレンダリング ビデオ上にピン留めされたホログラム - だけもう少しデバイスを移動するかどうかの大きな問題ホログラムおが変動します。

ジャイロスコープなどの組み込みのセンサーが動きを追跡することはできないために発生する、アクセラレータのみできます。  各フレームと抽出機能ポイントを追跡、入力できるように正確な ARKit 解析では、移動、回転のデータを含むマトリックスを変換します。

たとえば、現在の位置の取得に示します。

```csharp
var row = arCamera.Transform.Row3;
CameraNode.Position = new Vector3(row.X, row.Y, -row.Z);
```

使用して`-row.Z`ARKit した右辺座標系を使用しているためです。


### <a name="plane-detection"></a>平面の検出

ARKit が水平方向の平面を検出できるとこの機能では、現実の世界と対話することができます、実際のテーブルまたはフロアに、変異形を配置したなどです。 行うには最も簡単な方法では、HitTest メソッド (raycasting) を使用します。 画面座標に変換 (0.5; 0.5 中心) 現実世界の座標に (0 以外の 0 になります。 0 は最初のフレームの場所)。

```chsarp
protected Vector3? HitTest(float screenX = 0.5f, float screenY = 0.5f)
{
    var result = ARSession.CurrentFrame.HitTest(new CGPoint(screenX, screenY),
        ARHitTestResultType.ExistingPlaneUsingExtent)?.FirstOrDefault();
    if (result != null)
    {
        var row = result.WorldTransform.Row3;
        return new Vector3(row.X, row.Y, -row.Z);
    }
    return null;
}
```

今すぐ をタップして、デバイス画面上の場所に応じて水平画面で、変異形を配置おできます。

```chsarp
void OnTouchEnd(TouchEndEventArgs e)
{
    float x = e.X / (float)Graphics.Width;
    float y = e.Y / (float)Graphics.Height;
    var pos = HitTest(x, y);
    if (pos != null)
    mutantNode.Position = pos.Value;
}
```

![ビューが進め平面をアニメーション化された図を変更します。](urhosharp-images/image4.gif)

### <a name="realistic-lighting"></a>現実的な光源

現実の世界照明条件に応じて、仮想のシーンは明るくしたり暗く周囲に合わせてをする必要があります。 ARFrame には、Urho アンビエント光の調整に使用できる LightEstimate プロパティが含まれています。 これは、次のように。


    var ambientIntensity = (float) frame.LightEstimate.AmbientIntensity / 1000f;
    var zone = Scene.GetComponent<Zone>();
    zone.AmbientColor = Color.White * ambientIntensity;


### <a name="beyond-ios---hololens"></a>IOS - HoloLens を超える

UrhoSharp[すべての主要なオペレーティング システムで実行](~/graphics-games/urhosharp/platform/index.md)ので、他の場所で、既存のコードを再利用することができます。

HoloLens は、実行する最も魅力的なプラットフォームのいずれかです。   つまり、iOS および HoloLens UrhoSharp を使用して優れた実際の補完されたアプリケーションを構築する間で簡単に切り替えることができます。

MutantDemo ソースを検索できる[github.com/EgorBo/ARKitXamarinDemo](https://github.com/EgorBo/ARKitXamarinDemo)です。


## <a name="related-links"></a>関連リンク

- [UrhoSharp](~/graphics-games/urhosharp/index.md)
- [(UrhoSharp) を持つ ARKitXamarinDemo (サンプル)](https://github.com/EgorBo/ARKitXamarinDemo)
