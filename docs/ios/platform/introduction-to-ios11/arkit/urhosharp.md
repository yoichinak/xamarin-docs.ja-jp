---
title: Xamarin.iOS で UrhoSharp を ARKit を使用します。
description: 検出方法平面のこのドキュメントは、Xamarin.iOS で ARKit のアプリを設定する方法について説明し、フレームのレンダリング方法をカメラを調整する方法を見て、照明などを操作する方法 UrhoSharp、HoloLens のコードの作成についても説明します。
ms.prod: xamarin
ms.assetid: 877AF974-CC2E-48A2-8E1A-0EF9ABF2C92D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/01/2017
ms.openlocfilehash: b02ecc8a40f6ff8a1862d50202439d369003a53d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61262440"
---
# <a name="using-arkit-with-urhosharp-in-xamarinios"></a>Xamarin.iOS で UrhoSharp を ARKit を使用します。

導入に伴い[ARKit](https://developer.apple.com/arkit/)Apple が簡単に拡張現実のアプリケーションを作成する開発者向け。 ARKit がデバイスの正確な位置を追跡し、世界のさまざまな画面を検出し、コードに ARKit から返されたデータのブレンドする開発者の役目です。

[UrhoSharp](~/graphics-games/urhosharp/index.md) 3D アプリケーションを作成に使用できる包括的で使いやすい 3D API を提供します。   これらのどちらも ARKit 物理世界では、情報を提供して、結果を表示するために Urho ブレンドします。

このページには、優れた拡張現実のアプリケーションを作成するためにこれら 2 つの世界に接続する方法について説明します。


## <a name="the-basics"></a>基本事項

IPhone または iPad に表示されるよう、世界の上に存在する 3D コンテンツが実行します。   考え方としては、3 D コンテンツに、デバイスのカメラからコンテンツを調和させるのにはおよびがその部屋の一部では、3 D オブジェクトの動作 - これは、この世界にオブジェクトをアンカーでのデバイスのユーザーがいることを確認する部屋が移動します。

![ARKit でアニメーション化された図](urhosharp-images/image1.gif)


取り上げる Urho ライブラリを当社の 3D アセットを読み込み、世界では、上に配置し、ARKit 使用して、電話、世界中の場所と同様に、カメラからビデオ ストリームを取得します。   ユーザーが自分の電話で移動すると、Urho エンジンが表示されている座標システムを更新するのに場所に変更を使用します。

これにより、3 次元空間でオブジェクトを配置して、ユーザーの移動時に、3 D オブジェクトの場所が反映されますが置かれた場所と場所。

## <a name="setting-up-your-application"></a>アプリケーションの設定

### <a name="ios-application-launch"></a>iOS アプリケーションの起動

作成し、3 D コンテンツを起動する必要がある iOS アプリケーションを実装するのサブクラスを作成してこれを行う、 [ `Urho.Application` ](https://developer.xamarin.com/api/type/Urho.Application/)オーバーライドすることで、セットアップ コードを提供し、`Start`メソッド。  これは、シーンを取得、イベント ハンドラーのセットアップはこれにデータを含むです。

導入されています、`Urho.ArkitApp`クラスをサブクラスとして持つ`Urho.Application`しその`Start`メソッドが面倒な作業を行います。   型の基本クラスを変更するアプリケーションは、既存の Urho を行う必要があるすべて`Urho.ArkitApp`urho シーン、世界中で実行されるアプリケーションがあるとします。

### <a name="the-arkitapp-class"></a>ArkitApp クラス

このクラスは、オペレーティング システムが提供されるため、便利な既定値、いくつか主要なオブジェクトを使用したシーン両方のセットと ARKit イベントの処理を提供します。

セットアップが、`Start`仮想メソッド。   サブクラスにこのメソッドをオーバーライドする場合は、ことを確認して、親にチェーンを使用して行う必要があります。 `base.Start()` 、独自の実装にします。

`Start`メソッドは、シーン、ビューポート、カメラ、指向性光の場合を設定し、それらのパブリック プロパティとして。

- [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Scene/)のオブジェクトを保持するには
- 方向[ `Light` ](https://developer.xamarin.com/api/type/Urho.Light/)シャドウ、および場所を利用し、`LightNode`プロパティ
- [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera/) ARKit アプリケーションに更新プログラムを配信する際にコンポーネントが含まれるが更新されると
- [ `ViewPort` ](https://developer.xamarin.com/api/type/Urho.Viewport/)結果を表示します。


### <a name="your-code"></a>コード

サブクラスを`ArkitApp`クラスし、オーバーライド、`Start`メソッド。   メソッドが行う必要があります最初に行うことが最大チェーンは、`ArkitApp.Start`呼び出して`base.Start()`します。  その後はオブジェクトをシーンに追加、ライト、shadows または処理するイベントをカスタマイズする ArkitApp プロパティの設定のいずれかを使用できます。

ARKit/UrhoSharp の例では、テクスチャのアニメーションの文字をロードし、次の実装、アニメーションを再生します。

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

および 3D コンテンツの augmented reality で表示されます。 この時点で行う必要があることですが本当にします。

Urho は、資産をこの形式にエクスポートする必要があるために、3 D モデルやアニメーションのカスタム形式を使用します。   などのツールを使用することができます、 [Urho3D Blender アドイン](https://github.com/reattiva/Urho3D-Blender)と[UrhoAssetImporter](https://github.com/EgorBo/UrhoAssetImporter) Urho によって必要な形式には、3 D の最大、DBX、DAE、OBJ、Blend などの人気のある形式からこれらのアセットを変換することができます。

Urho を使用して 3D のアプリケーションの作成の詳細については、次を参照してください。、 [UrhoSharp の概要](~/graphics-games/urhosharp/introduction.md)ガイド。

## <a name="arkitapp-in-depth"></a>深さで ArkitApp

> [!NOTE]
> このセクションでは ARKit UrhoSharp の既定のエクスペリエンスをカスタマイズする、または統合のしくみに関するより深い洞察を取得する必要がある開発者を対象としています。   このセクションを読む必要はありません。

ARKit API は非常に単純で、作成および構成する、 [ARSession](https://developer.apple.com/documentation/arkit/arsession)オブジェクトを配信を開始し、 [ARFrame](https://developer.apple.com/documentation/arkit/arframe)オブジェクト。   これらには、両方のカメラとデバイスの推定の実際の位置によってキャプチャされたイメージが含まれます。

私たちは、3D コンテンツを使用することで、カメラで配信されるイメージの作成し、デバイスの場所や位置の可能性を一致するように UrhoSharp のカメラを調整します。

次の図は、何が起きて、`ArkitApp`クラス。

[![クラスと、ArkitApp で画面のダイアグラム](urhosharp-images/image2.png)](urhosharp-images/image2.png#lightbox)

### <a name="rendering-the-frames"></a>フレームのレンダリング

考え方は単純、結合されたイメージを生成するために、3 D グラフィックスでカメラからビデオを結合します。     一連のこれらのキャプチャされたイメージのシーケンスで取得しましたと Urho シーンをこの入力を混在させることができます。

これを行う最も簡単な方法は、挿入する、 [ `RenderPathCommand` ](https://developer.xamarin.com/api/type/Urho.RenderPathCommand/)をメイン[ `RenderPath`](https://developer.xamarin.com/api/type/Urho.RenderPath/)します。  これは、一連の 1 つのフレームを描画するために実行されるコマンドです。  このコマンドはすべてのテクスチャを渡すことで、ビューポートを入力します。    これを設定、プロセスは、最初のフレームでし番目で実際の定義を行う**ARRenderPath.xml**この時点で読み込まれるファイル。

ただし、これら 2 つの世界をブレンドする 2 つの問題に直面しました。


1. Ios では、GPU のテクスチャの解像度 2 の累乗である必要がありますが、カメラから取得するフレームには、例 2 の累乗である解像度はありません。1280 x 720 します。
2. フレームがでエンコードされた[YUV](https://en.wikipedia.org/wiki/YUV)形式、2 つのイメージの輝度およびおよび彩度によって表されます。

YUV フレームは、2 つのさまざまな解像度で提供されます。  輝度 (基本的に、グレースケール イメージ) とくらい小さい 640 x 360 クロミナンス コンポーネントを表す 1280 x 720 イメージ:

![結合 Y と UV コンポーネントを示すイメージ](urhosharp-images/image3.png)


OpenGL ES を使用して、完全な色分けされたイメージを描画するには、テクスチャのスロットから輝度 (Y コンポーネント) とクロミナンス (UV 面) を取得する小さなシェーダーを記述する必要あります。  UrhoSharp は、"sDiffMap"と"sNormalMap"の名前を持つおよび RGB 形式に変換します。

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

2 つの解決の電源が入っていないテクスチャを表示するために、次のパラメーターで Texture2D を定義しなければ。

```chsarp
// texture for UV-plane;
cameraUVtexture = new Texture2D();
cameraUVtexture.SetNumLevels(1);
cameraUVtexture.SetSize(640, 360, Graphics.LuminanceAlphaFormat, TextureUsage.Dynamic);
cameraUVtexture.FilterMode = TextureFilterMode.Bilinear;
cameraUVtexture.SetAddressMode(TextureCoordinate.U, TextureAddressMode.Clamp);
cameraUVtexture.SetAddressMode(TextureCoordinate.V, TextureAddressMode.Clamp);
```

そのため、背景としてキャプチャしたイメージをレンダリングし、その上の任意のシーンをレンダリングするには、そのような恐ろしい変異形できなくなっています。

### <a name="adjusting-the-camera"></a>カメラを調整します。

`ARFrame`オブジェクトでは、推定のデバイスの位置も含まれます。  ユーザーのずれ ARFrame ゲームのカメラを移動する必要がありますが、ARKit する前に、でしたをトラック デバイスの向き (ロール、ピッチ、ヨー) とレンダリング、ビデオの上にピン留めされたホログラム - デバイスを少し移動するかどうか、たいしたホログラムされます。

ジャイロスコープなどの組み込みセンサーの動きを追跡することはできないために発生する、のみの高速化できます。  ARKit 分析の各フレームと抽出機能ポイントを追跡し、そのため、正確性を提供することがでは、データの移動と回転を含むマトリックスを変換します。

たとえば、これは、現在の位置の取得です。

```csharp
var row = arCamera.Transform.Row3;
CameraNode.Position = new Vector3(row.X, row.Y, -row.Z);
```

使用して`-row.Z`ARKit は右手座標系を使用するためです。


### <a name="plane-detection"></a>平面の検出

ARKit は水平面を検出することが、この機能により、現実の世界との対話など、実際のテーブルまたはフロアに、変異形を行って ことができます。 HitTest メソッド (レイキャスト) を使用することを行う最も簡単な方法です。 画面座標に変換 (0.5; 0.5 中心) 実際の座標に (0; 0; 0 は最初のフレームの位置)。

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

ここをタップして、デバイス画面上の位置によって水平画面で、変異形を配置できます。

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

![ビューの移動として平面をアニメーション化された図を変更します。](urhosharp-images/image4.gif)

### <a name="realistic-lighting"></a>リアルなライティング

実際の照明条件に応じて仮想のシーンは明るくしたり暗く周囲のにより適したものにする必要があります。 ARFrame には、Urho のアンビエント光を調整するために使用できる LightEstimate プロパティが含まれています。 これは、次のように。

```csharp
var ambientIntensity = (float) frame.LightEstimate.AmbientIntensity / 1000f;
var zone = Scene.GetComponent<Zone>();
zone.AmbientColor = Color.White * ambientIntensity;
```

### <a name="beyond-ios---hololens"></a>IOS - HoloLens を超える

UrhoSharp[すべての主要なオペレーティング システム上で実行](~/graphics-games/urhosharp/platform/index.md)ので、他の場所で、既存のコードを再利用することができます。

HoloLens で実行される最も魅力的なプラットフォームの 1 つです。   つまり、iOS および HoloLens UrhoSharp を使用する最高の Augmented Reality アプリケーションを構築する間で簡単に切り替えることができます。

MutantDemo ソースを検索できる[github.com/EgorBo/ARKitXamarinDemo](https://github.com/EgorBo/ARKitXamarinDemo)します。


## <a name="related-links"></a>関連リンク

- [UrhoSharp](~/graphics-games/urhosharp/index.md)
- [ARKitXamarinDemo (with UrhoSharp) (sample)](https://github.com/EgorBo/ARKitXamarinDemo)
