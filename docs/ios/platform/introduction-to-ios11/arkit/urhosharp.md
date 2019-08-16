---
title: Xamarin の UrhoSharp で ARKit を使用する
description: このドキュメントでは、Xamarin で ARKit アプリを設定する方法について説明します。次に、フレームがどのようにレンダリングされるか、カメラを調整する方法、プレーンを検出する方法、照明を操作する方法などについて説明します。 また、UrhoSharp と HoloLens のコードの記述についても説明します。
ms.prod: xamarin
ms.assetid: 877AF974-CC2E-48A2-8E1A-0EF9ABF2C92D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/01/2017
ms.openlocfilehash: 106d6100d373c8d14a35aaee59035cf4a98083a5
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69528254"
---
# <a name="using-arkit-with-urhosharp-in-xamarinios"></a>Xamarin の UrhoSharp で ARKit を使用する

[Arkit](https://developer.apple.com/arkit/)の導入により、Apple は、開発者が拡張された現実アプリケーションを簡単に作成できるようになりました。 ARKit は、デバイスの正確な位置を追跡し、世界中のさまざまな画面を検出し、開発者が ARKit から送信されるデータをコードに合成します。

[Urhosharp](~/graphics-games/urhosharp/index.md)は、3d アプリケーションを作成するために使用できる包括的で使いやすい 3d API を提供します。   これらの両方を組み合わせて、ARKit を使用して世界の物理的な情報を提供し、Urho を使用して結果を表示することができます。

このページでは、これら2つの世界を接続して、優れた拡張現実アプリケーションを作成する方法について説明します。

## <a name="the-basics"></a>基本事項

IPhone/iPad で見られるように、世界の上に3D コンテンツを表示したいと思います。   これは、デバイスのカメラのコンテンツと3D コンテンツをブレンドすることです。また、デバイスのユーザーが部屋の周りを移動し、3D オブジェクトがその部屋の一部として動作するようにします。これは、オブジェクトをこの世界に固定することによって行われます。

![ARKit のアニメーション化される図](urhosharp-images/image1.gif)

ここでは、Urho ライブラリを使用して3D アセットを読み込み、世界に配置します。また、ARKit を使用して、カメラからのビデオストリームや世界中の電話の位置を取得します。   ユーザーが電話で移動するときに、場所の変更を使用して、Urho エンジンが表示している座標系を更新します。

このようにして、3D 空間にオブジェクトを配置し、ユーザーを移動すると、3D オブジェクトの位置と位置が反映されます。

## <a name="setting-up-your-application"></a>アプリケーションの設定

### <a name="ios-application-launch"></a>iOS アプリケーションの起動

IOS アプリケーションでは、3d コンテンツを作成して起動する必要があります。これを行うには`Urho.Application` 、のサブクラスを実装するを`Start`作成し、メソッドをオーバーライドしてセットアップコードを指定します。  ここでは、シーンにデータを入力し、イベントハンドラーを設定します。

`Urho.ArkitApp`クラスを作成`Urho.Application`し、その`Start`メソッドで大量の処理を行うクラスを導入しました。   既存の Urho アプリケーションに対して行う必要があるのは、基底クラスを型`Urho.ArkitApp`に変更し、世界中の Urho シーンを実行するアプリケーションがある場合です。

### <a name="the-arkitapp-class"></a>ArkitApp クラス

このクラスは、便利な既定のセットを提供します。これには、いくつかの主要なオブジェクトを含むシーンと、オペレーティングシステムによって配信される ARKit イベントの処理が含まれます。

セットアップは`Start`仮想メソッドで実行されます。   サブクラスでこのメソッドをオーバーライドする場合は、独自の実装でを使用`base.Start()`して、親にチェーンしていることを確認する必要があります。

メソッド`Start`は、シーン、ビューポート、カメラ、および指向性ライトを設定し、それらをパブリックプロパティとして設定します。

- オブジェクトを保持する。 `Scene`
- 影付き`Light`の方向で、プロパティを`LightNode`使用してその場所を使用できます。
- `Camera` arkit によってアプリケーションに更新プログラムが配信されるときにコンポーネントが更新される。
- 結果`ViewPort`を表示する。

### <a name="your-code"></a>コード

次に、 `ArkitApp`クラスをサブクラス化し、 `Start`メソッドをオーバーライドする必要があります。   メソッドが最初に行う必要があるのは、 `ArkitApp.Start`を呼び出す`base.Start()`ことによってにチェーンすることです。  その後、ArkitApp によって設定されたプロパティのいずれかを使用して、オブジェクトをシーンに追加したり、処理するライト、シャドウ、またはイベントをカスタマイズしたりすることができます。

ARKit/UrhoSharp サンプルは、テクスチャを使用してアニメーション化された文字を読み込み、次の実装を使用してアニメーションを再生します。

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

これで、3D コンテンツを強化された現実に表示するために、この時点で実行する必要があります。

Urho では、3D モデルとアニメーションのカスタム形式を使用するため、資産をこの形式にエクスポートする必要があります。   [Urho3D Blender アドイン](https://github.com/reattiva/Urho3D-Blender)や[UrhoAssetImporter](https://github.com/EgorBo/UrhoAssetImporter)などのツールを使用すると、DBX、DAE、OBJ、Blend、3d-Max などの一般的な形式から、Urho が必要とする形式にこれらの資産を変換できます。

Urho を使用して3D アプリケーションを作成する方法の詳細については、「 [UrhoSharp](~/graphics-games/urhosharp/introduction.md)ガイドの概要」を参照してください。

## <a name="arkitapp-in-depth"></a>ArkitApp の詳細

> [!NOTE]
> このセクションは、UrhoSharp と ARKit の既定のエクスペリエンスをカスタマイズする開発者を対象としています。また、統合のしくみについて深い洞察を得ることを目的としています。   このセクションを読む必要はありません。

ARKit API は非常に単純です。[ARFrame](https://developer.apple.com/documentation/arkit/arframe) オブジェクトの配信を開始する[ARSession](https://developer.apple.com/documentation/arkit/arsession) を作成および構成します。   これらには、カメラによってキャプチャされたイメージと、デバイスの実際の実際の位置の両方が含まれます。

カメラによって配信される画像を3D コンテンツと共に作成し、デバイスの位置と位置の可能性に合わせて UrhoSharp のカメラを調整します。

次の図は、 `ArkitApp`クラスで行われる処理を示しています。

[![ArkitApp のクラスと画面の図](urhosharp-images/image2.png)](urhosharp-images/image2.png#lightbox)

### <a name="rendering-the-frames"></a>フレームのレンダリング

この考え方は単純で、カメラからのビデオと3D グラフィックスを組み合わせて、結合された画像を生成します。     これらのキャプチャされたイメージを順番に取得し、この入力を Urho シーンと組み合わせます。

これを行う最も簡単な方法は、を`RenderPathCommand`メイン`RenderPath`に挿入することです。  これは、1つのフレームを描画するために実行される一連のコマンドです。  このコマンドを実行すると、ビューポートに渡されるすべてのテクスチャが表示されます。    プロセスの最初のフレームにこれを設定し、実際の定義はこの時点で読み込まれる次の**Arrenderpath .xml**ファイルで行います。

ただし、この2つのワールドをブレンドするための2つの問題に直面しています。


1. IOS では、GPU テクスチャの解像度は2である必要がありますが、カメラから取得するフレームには2の累乗の解像度がありません。次に例を示します。1280 x 720.
2. フレームは、3つのイメージ (ルミナンスとクロマ) で表される、 [YUV](https://en.wikipedia.org/wiki/YUV)形式でエンコードされます。

YUV フレームには、2つの異なる解像度があります。  輝度 (基本的にはグレースケールイメージ) と、クロミナンスコンポーネントの 640 x 360 を表す、1280 x 720 のイメージ。

![Y と UV コンポーネントの組み合わせを示す画像](urhosharp-images/image3.png)


OpenGL を使用して全色の画像を描画するには、テクスチャスロットから輝度 (Y 成分) とクロミナンス (UV 平面) を取得する小さなシェーダーを作成する必要があります。  UrhoSharp には、"SsNormalMap Map" と "" という名前が付いており、それらを RGB 形式に変換します。

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

2つの解像度の力を持たないテクスチャをレンダリングするには、次のパラメーターを使用して Texture2D を定義する必要があります。

```chsarp
// texture for UV-plane;
cameraUVtexture = new Texture2D();
cameraUVtexture.SetNumLevels(1);
cameraUVtexture.SetSize(640, 360, Graphics.LuminanceAlphaFormat, TextureUsage.Dynamic);
cameraUVtexture.FilterMode = TextureFilterMode.Bilinear;
cameraUVtexture.SetAddressMode(TextureCoordinate.U, TextureAddressMode.Clamp);
cameraUVtexture.SetAddressMode(TextureCoordinate.V, TextureAddressMode.Clamp);
```

そのため、キャプチャしたイメージを背景としてレンダリングし、その上にあるシーンをレンダリングして、そのようにすることができます。

### <a name="adjusting-the-camera"></a>カメラの調整

オブジェクト`ARFrame`には、推定されるデバイスの位置も含まれます。  ここでは、ゲームカメラ ARFrame を移動する必要がありました。 Arframe は、デバイスの向き (ロール、ピッチ、ヨー) を追跡し、ピン留めされたホログラムをビデオの上に表示することはあまり重要ではありませんでした。ただし、デバイスを移動する場合は、ビットホログラムがずれます。

これは、ジャイロスコープなどの組み込みのセンサーが移動を追跡できず、アクセラレーションのみであることが原因で発生します。  ARKit は各フレームを分析し、特徴ポイントを抽出して追跡します。これにより、移動データと回転データを含む正確な変換行列が得られます。

たとえば、現在の位置を取得するには、次のようにします。

```csharp
var row = arCamera.Transform.Row3;
CameraNode.Position = new Vector3(row.X, row.Y, -row.Z);
```

Arkit `-row.Z`は右手座標系を使用するため、を使用します。


### <a name="plane-detection"></a>平面の検出

ARKit は、水平方向のプレーンを検出できます。この機能により、たとえば、実際のテーブルまたはフロアに変異を配置できます。 これを行う最も簡単な方法は、System.windows.media.visualtreehelper.hittest メソッド (raycasting) を使用することです。 画面座標 (0.5 は中央) を実際の座標に変換します (0、0、0は、最初のフレームの位置) です。

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

ここで、デバイス画面上のどこにタップするかに応じて、ミュータントを水平方向の表面に配置できます。

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

![アニメーション図ビューの移動として平面を変更する](urhosharp-images/image4.gif)

### <a name="realistic-lighting"></a>リアルな照明

実際の照明条件によっては、その環境に合わせて仮想シーンを明るくしたり暗くしたりする必要があります。 ARFrame には、Urho アンビエントライトを調整するために使用できる LightEstimate プロパティが含まれています。これは次のように行われます。

```csharp
var ambientIntensity = (float) frame.LightEstimate.AmbientIntensity / 1000f;
var zone = Scene.GetComponent<Zone>();
zone.AmbientColor = Color.White * ambientIntensity;
```

### <a name="beyond-ios---hololens"></a>IOS-HoloLens 以外

UrhoSharp は、[すべての主要なオペレーティングシステムで実行](~/graphics-games/urhosharp/platform/index.md)されるので、既存のコードを他の場所で再利用できます。

HoloLens は、それが実行されている最も魅力的なプラットフォームの1つです。   これは、iOS と HoloLens を簡単に切り替えて、UrhoSharp を使用してすばらしい拡張現実アプリケーションを構築できることを意味します。

[Github.com/EgorBo/ARKitXamarinDemo](https://github.com/EgorBo/ARKitXamarinDemo)で、ミューテーターのデモソースを見つけることができます。


## <a name="related-links"></a>関連リンク

- [UrhoSharp](~/graphics-games/urhosharp/index.md)
- [ARKitXamarinDemo (UrhoSharp) (サンプル)](https://github.com/EgorBo/ARKitXamarinDemo)
