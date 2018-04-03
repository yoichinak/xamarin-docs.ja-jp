---
title: CocosSharp による 2D 数学的演算
description: このガイドでは、ゲームの開発のための 2D 数学について説明します。 CocosSharp 使用ゲーム開発の一般的なタスクを実行する方法について説明と、これらのタスクの背後にある数値演算について説明します。
ms.topic: article
ms.prod: xamarin
ms.assetid: 5C241AB4-F97E-4B61-B93C-F5D307BCD517
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 484bd8b19f2c51dac57a46a1ef93610ed5e13419
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/03/2018
---
# <a name="2d-math-with-cocossharp"></a>CocosSharp による 2D 数学的演算

_このガイドでは、ゲームの開発のための 2D 数学について説明します。CocosSharp 使用ゲーム開発の一般的なタスクを実行する方法について説明と、これらのタスクの背後にある数値演算について説明します。_

配置し、コードを持つオブジェクトを移動するには、あらゆる規模の開発のゲームのコア部分です。 配置および移動は、数学の使用、ゲームでは、直線または三角関数の使用の回転をに沿ってオブジェクトを移動する必要があるかどうかが必要です。 このドキュメントは、次のトピックを取り上げます。

 - ベロシティ
 - 高速化
 - CocosSharp オブジェクトの回転
 - 速度で回転の使用

開発者が、強力な数学背景は持っていないユーザーまたはユーザーが時間の長い忘れかけていた学校からこれらのトピックは、心配する必要はありません: このドキュメントは分割の概念食べるサイズ、および実際の例の理論上の説明を伴う、します。 簡単に言えば、この記事は回答を昔ながら math 学生:「は実際に必要になる場合にこの機能を使用しますか?」


## <a name="requirements"></a>要件

コード サンプルにフォームを継承するオブジェクトを使った作業前提としていますが、このドキュメントでは、主 CocosSharp の数学的な側面に焦点を当てています、`CCNode`です。 さらに、以降`CCNode`値を含まない速度とアクセラレータの場合は、コードは、VelocityX、VelocityY、AccelerationX、および AccelerationY などの値を提供するエンティティの使用を想定しています。 エンティティの詳細については、このチュートリアルを参照してください。 [CocosSharp 内のエンティティ](~/graphics-games/cocossharp/entities.md)です。


## <a name="velocity"></a>ベロシティ

ゲーム開発者という用語を使用する*ベロシティ*オブジェクトが移動 – 方法を説明する速度は何かの移動と方向が移動します。 

ベロシティは、2 種類の単位を使用して定義されます。 位置の単位と時間の単位。 たとえば、自動車の速度は、1 時間あたりマイルまたはキロメートル 1 時間として定義されます。 ゲームの開発者が多くの場合、使用する (ピクセル) 速さオブジェクトを定義する 1 秒あたりに移動します。 たとえば、行頭文字は、1 秒あたり 300 ピクセルの速度で移動可能性があります。 つまり、行頭文字が 1 秒あたり 300 ピクセルに移動の場合は、その後が移動 600 単位 2 秒、および 900 単位で、3 秒でします。 ベロシティ値では一般的には、*追加*オブジェクトの位置に (よう、私たちは、以下を参照します)。

ベロシティの単位を説明する速度を使用した用語速度通常は値の方向、独立した用語ベロシティは速度と方向の両方を参照中に。 そのため、行頭の速度 (行頭文字が必要なプロパティが含まれるクラスと仮定) の割り当ては次のようになります。


```csharp
// This bullet is not moving horizontally, so set VelocityX to 0:
bulletInstance.VelocityX = 0;
// Positive Y is "up" so move the bullet up 300 units per second:
bulletInstance.VelocityY = 300;
```


### <a name="implementing-velocity"></a>ベロシティを実装します。

移動を必要とするオブジェクトは、独自の動きのロジックを実装する必要がありますので、CocosSharp は速度を実装しません。 多くの場合、速度を実装する新しいゲーム開発者は間違い、ベロシティのフレーム レートに依存します。 次は、*を正しく実装*は正しい結果を提供するようですが、ゲームのフレーム レートに基づきます。

```csharp
// VelocityX and VelocityY will be added every time this code executes
this.PositionX += this.VelocityX;
this.PositionY += this.VelocityY;
```

(1 秒あたり 30 フレームではなく 1 秒あたり 60 フレーム) などの上位のフレーム レートでゲームを実行する場合は、オブジェクトが低速のフレーム レートで実行されている場合よりも高速移動に表示されます。 同様に、ゲームがない場合、フレーム レート (バック グラウンド プロセス、デバイスのリソースを使用する可能性があります) の高位のフレームを処理すること、ゲームは表示速度が低下します。

ために、ベロシティは多くの場合、実装時間の値を使用します。 たとえば場合、`seconds`変数を表します (秒) の数 (または分数) 最後の時間の速度が適用されてから、フレーム レートに関係なく一貫した動作を持つオブジェクトで、次のコードになります。

```csharp
// VelocityX and VelocityY will be added every time this code executes
this.PositionX += this.VelocityX * seconds;
this.PositionY += this.VelocityY * seconds;
```

下のフレーム レートでを実行するゲームが以下のオブジェクトの位置を頻繁に更新はことを検討してください。 したがって、各更新プログラムは、移動、ゲームをより頻繁に更新された場合よりもさらに、オブジェクトになります。 `seconds`前回の更新以降にどれ時間だけが経過を報告することによって、このアカウントの値します。

時間ベースの動きを追加する方法の例は、次を参照してください。[時間をカバーするこのレシピ ベース移動](https://developer.xamarin.com/recipes/cross-platform/game_development/time_based_movement/)です。


### <a name="calculating-positions-using-velocity"></a>速度を使用して位置の計算

ベロシティは、ある程度の時間の経過後にオブジェクトがされるに関する予測を行うか、ゲームを実行することがなくオブジェクトの動作の調整に役立ちますに使用できます。 たとえば、起動の箇条書きの動きを実装する開発者がインスタンス化された後に、行頭の速度を設定する必要があります。 画面のサイズは、基礎となる速度を設定するために使用できます。 開発者が知っている場合は、行頭文字は 2 秒単位で、画面の高さを移動する必要がありますし、ベロシティは、2 で割った値画面の高さを設定する必要があります。 画面が 800 ピクセルの高さの場合は、行頭の速度は 400 (ある 800/2) に設定されます。

同様に、ゲームのロジックは、オブジェクトにかかる速度を指定した宛先に到達するを計算する必要があります。 これは、トラベル用のオブジェクトの速度によって移動する距離を割ることによって計算できます。 たとえば、次のコードがどのくらいの期間を表示するラベルにテキストを割り当てる方法を示しています、ミサイルがターゲットに達するまでします。


```csharp
// We'll assume only the X axis for this example
float distanceX = target.PositionX - missile.PositionX;

float secondsToReachTarget = distanceX / missile.VelocityX;

label.Text = secondsToReachTarget + " seconds to reach target"; 
```


## <a name="acceleration"></a>高速化

*アクセラレータ*ゲームの開発における一般的な概念は、多くの類似点と同じ速度でします。 アクセラレータは、オブジェクトが高速化、または (ベロシティ値の変更時間の経過と共にどのように) のパフォーマンスの低下するかどうかを定量化します。 アクセラレータ*追加*ベロシティは、配置に追加する場合と同じように、ベロシティにします。 アクセラレータの一般的なアプリケーションには、重力、車の高速化、およびその thrusters を発生させるスペースが含まれます。 

ベロシティと同様に、アクセラレータがで定義されている位置と時間単位です。ただし、アクセラレータの時間単位を呼びます、*平方和*単位で、アクセラレータが数学的に定義されている方法が反映されます。 つまり、ゲームのアクセラレータは多くの場合で測定されます*1 秒あたりのピクセルの 2 乗*です。

オブジェクトが、X アクセラレータ乗 1 秒あたりの 10 単位の場合、速度が 10 秒ごとに増加することを意味します。 1 秒間になります秒後の 2 つの 10 単位に移動した後、陥りましたから始まる場合に、2 つ目ごとに 20 単位 (秒) します。

2 つのディメンションでアクセラレータでは、次のように割り当てることができるように、X と Y のコンポーネントが必要です。


```csharp
// No horizontal acceleration:
icicle.AccelerationX = 0;
// Simulate gravity with Y acceleration. Negative Y is down, so assign a negative value:
icicle.AccelerationY = -50; 
```


### <a name="acceleration-vs-deceleration"></a>加速減速との比較

毎日音声認識では、アクセラレータと減速は区別される場合があります、ですが、2 つの技術の違いはありません。 重力は、アクセラレータで力です。 オブジェクトが上方向へスローされた場合、重力が遅くなる原因 (減速) が、オブジェクトが上昇が停止し、重力と同じ方向に遅れが発生したら、重力は高速化、(加速する場合)。 以下に示すよう、アプリケーション、アクセラレータの同じですが、同じ方向または逆方向の移動に適用されているかどうか。 


### <a name="implementing-acceleration"></a>高速化を実装します。

実装する場合は、アクセラレータ、ベロシティに類似 – CocosSharp、によって自動的に実装されていません、時間ベースのアクセラレータ (フレーム ベースのアクセラレータ) ではなく必要な実装。 したがって (速度) と共に単純なアクセラレータの実装例を示します。

```csharp
this.VelocityX += this.AccelerationX * seconds;
this.VelocityY += this.AccelerationY * seconds;
this.PositionX += this.VelocityX * seconds;
this.PositionY += this.VelocityY * seconds;
```

上記のコードは、新機能と呼びます、*線形近似*アクセラレータの実装です。 実際には、精度、度にはかなり密接された高速化を実装するが、アクセラレータの正確なモデルではありません。 含まれている上記アクセラレータを実装する方法の概念を説明するためにします。

次の実装は、アクセラレータおよびベロシティの数学的に正確なアプリケーションです。


```csharp
float halfSecondsSquared = (seconds * seconds) / 2.0f;

this.PositionX += 
    this.Velocity.X * seconds + this.AccelerationX * halfSecondsSquared;
this.PositionY += 
    this.Velocity.Y * seconds + this.AccelerationY * halfSecondsSquared;

this.VelocityX += this.AccelerationX * seconds;
this.VelocityY += this.AccelerationY * seconds; 
```

上記のコードに最も明白な違いは、`halfSecondsSquared`変数とその使用状況を配置するには、高速化を適用します。 その理由は、数学的は扱いませんが、このチュートリアルではこの背後にある数値演算で関心のある開発者は、詳細についてを検出できる[アクセラレータの統合に関するこの説明します。](http://www.cliffsnotes.com/math/calculus/calculus/integration/distance-velocity-and-acceleration)

実際の影響`halfSecondSquare`アクセラレータが動作する数学的に正確かつ予測可能なフレーム レートに関係なく、します。 アクセラレータの線形近似はフレーム レート – される可能性がありますが低いほど、フレーム レート精度は下がります、概算値になります。 使用して`halfSecondsSquared`コードが動作するフレーム レートに関係なく同じ保証します。


## <a name="angles-and-rotation"></a>角度と回転

などの visual オブジェクト`CCSprite`経由の回転は、サポート、`Rotation`変数。 これは、(度単位) の回転を設定する値に割り当てることができます。 たとえば、次のコードが回転する方法を示しています、`CCSprite`インスタンス。


```csharp
CCSprite unrotatedSprite = new CCSprite("star.png");
unrotatedSprite.IsAntialiased = false;
unrotatedSprite.PositionX = 100;
unrotatedSprite.PositionY = 100;
this.AddChild (unrotatedSprite);

CCSprite rotatedSprite = new CCSprite("star.png");
rotatedSprite.IsAntialiased = false;
// This sprite is moved to the right so it doesn’t overlap the first
rotatedSprite.PositionX = 130;
rotatedSprite.PositionY = 100;
rotatedSprite.Rotation = 45;
this.AddChild (rotatedSprite); 
```

次のような結果が得られます。

![](math-images/image1.png "これは、結果、このスクリーン ショット")

回転角度は、45 度時計回りにする (履歴上の理由から、逆に、回転を適用する方法の数学的に) であることを確認します。

![](math-images/image2.png "回転が時計回りに 45 度回転を適用する方法の数学的に反対の歴史的な経緯によりこれは")

一般に、次のように CocosSharp および数学正規の回転を視覚化できます。

![](math-images/image3.png "一般に次のように CocosSharp および数学正規の回転で視覚化することができます。")

![](math-images/image4.png "一般に次のように CocosSharp および数学正規の回転で視覚化することができます。")

この区別は重要なので、`System.Math`ため、このクラスに慣れている開発者を使用する場合の角度を反転する必要があるクラスは反時計回りの回転を使用して`CCNode`インスタンス。

上のダイアグラムが度数; 回転を表示することに注意する必要があります。ただし、一部の数学関数 (内の関数など、`System.Math`名前空間) ことが予想され、内の値を返す*ラジアン*度ではなくです。 このガイドで少し後で 2 つの単位の種類の間で変換する方法を紹介します。


### <a name="rotating-to-face-a-direction"></a>方向を向く回転

、上記のように`CCSprite`を回転できる を使用して、`Rotation`プロパティです。 `Rotation`によって提供されるプロパティ`CCNode`(の基底クラス`CCSprite`) から継承するエンティティに回転を適用できることを意味する`CCNode`もします。 

一部のゲームでは、ターゲットに直面しているように回転するオブジェクトが必要です。 例には、プレーヤーのターゲットまたはユーザーが画面をタッチは、ポイントに向かって飛行スペースで撮影コンピューター制御敵が含まれます。 ただし、回転値必要がありますまずするおよびに基づいて計算回転されているエンティティの場所に直面するターゲットの場所。

このプロセスには、いくつかの手順が必要です。

 - 識別する、*オフセット*ターゲットのです。 オフセットは、回転を表すエンティティとターゲット エンティティの X と Y の距離を指します。
 - (以下で詳しく説明されている) のアーク タンジェント三角関数を使用して、オフセット位置からの角度を計算しています。
 - 権限、および回転を表すエンティティ参照されていない回転したときの方向を指す 0 ° の違いを調整します。
 - 数学的な回転 (反時計回り) と CocosSharp 回転角度 (時計回り) の違いを調整します。

(エンティティで記述されていると見なされます)、次の関数は、ターゲットに直面するエンティティを回転します。


```csharp
// This function assumes that it is contained in a CCNode-inheriting object
public void FacePoint(float targetX, float targetY)
{
    // Calculate the offset - the target's position relative to "this"
    float xOffset = targetX - this.PositionX;
    float yOffset = targetY - this.PositionY;

    // Make sure the target isn't the same point as "this". If so,
    // then rotation cannot be calculated.
    if (targetX != this.PositionX || targetY != this.Position.Y)
    {

        // Call Atan2 to get the radians representing the angle from 
        // "this" to the target
        float radiansToTarget = (float)System.Math.Atan2 (yOffset, xOffset);

        // Since CCNode uses degrees for its rotation, we need to convert
        // from radians
        float degreesToTarget = CCMathHelper.ToDegrees (radiansToTarget);

        // The direction that the entity faces when unrotated. In this case
        // the entity is facing "up", which is 90 degrees 
        const float forwardAngle = 90;

        // Adjust the angle we want to rotate by subtracting the
        // forward angle.
        float adjustedForDirecitonFacing = degreesToTarget - forwardAngle;

        // Invert the angle since CocosSharp uses clockwise rotation
        float cocosSharpAngle = adjustedForDirecitonFacing * -1;

        // Finally assign the rotation
        this.Rotation = rotation = cocosSharpAngle;
    }
} 
```

ここで、ユーザーが画面に触れて、次のように、ポイントを向くようにエンティティを回転させるのには、上記のコードを使用できます。


```csharp
private void HandleInput(System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
{
    if(touches.Count > 0)
    {
        CCTouch firstTouch = touches[0];
        FacePoint (firstTouch.Location.X, firstTouch.Location.Y);
    }
} 
```

このコードは、次の動作になります。

![](math-images/image5.gif "このコードは、この動作になります")

#### <a name="using-atan2-to-convert-offsets-to-angles"></a>オフセット Atan2 を使用して変換する角度を

`System.Math.Atan2` 角度にオフセットを変換するために使用します。 関数名`Atan2`三角関数の逆正接に由来します。 「2」サフィックスでは、この関数を区別標準から`Atan`アーク タンジェントの数学的な動作を厳密に一致する関数。 アーク タンジェントの値を返す-90 関数とは、+90 度 (または同等のラジアン単位)。 コンピューター ゲームなど、多くのアプリケーションでは、フルの 360 度の値の多くの場合、必要なため、`Math`クラスが含まれます`Atan2`このニーズを満たすためにします。

上記のコードが合格する Y パラメーター最初に、X パラメーターを呼び出すときに注意してください、`Atan2`メソッドです。 これは旧バージョンと通常の X、Y の位置座標の順序です。 詳細については[Atan2 ドキュメントを参照してください](https://msdn.microsoft.com/en-us/library/system.math.atan2(v=vs.110).aspx)です。

注目すべきも戻り値の値から`Atan2`ラジアンは角度を測定するために使用する別の単位であります。 このガイドはラジアン単位の詳細をカバーしましたに留意してくださいを内のすべての三角関数、`System.Math`名前空間を使用するラジアンのため CocosSharp オブジェクトで使用される前に度に任意の値を変換する必要があります。 ラジアン単位の詳細についてを参照できます[ラジアンの Wikipedia ページに](http://en.wikipedia.org/wiki/Radian)です。

#### <a name="forward-angle"></a>順方向の角度

1 回、`FacePoint`メソッド角度をラジアンに変換する、定義、`forwardAngle`値。 この値は、回転の値が 0 の場合のエンティティが直面する角度を表します。 この例では、あるエンティティを上向き、90 度 (CocosSharp 回転) ではなく数学的な回転を使用する場合に想定しています。 数学的な回転ここで使用 CocosSharp の回転とは逆おにまだしていないためです。

どのようなエンティティが、次の表示、 `forwardAngle` 90 度のようになります。

![](math-images/image6.png "これが 90 度 forwardAngle を持つエンティティの外観を示しています")


### <a name="angled-velocity"></a>山ベロシティ

これまでのオフセットを角度に変換する方法を見てみました。 このセクションでは、他のようになる – 角度を受け取りし、X に変換し、Y 値。 一般的な例には、車向きには、または、出荷が直面している方向に移動する箇条書きを撮影スペース内の移動にはが含まれます。 

概念的には、されていない回転したときに、必要なベロシティを最初に定義し、エンティティが直面している角度に速度を回転してベロシティを計算できます。 この概念を説明するには、速度 (および加速度) を 2 次元として視覚化できます*ベクター* (通常が描画される矢印として)。 ベクトルの X 印の付いたベロシティ値 = 100 と Y = 0 は、次のように視覚化できます。

![](math-images/image7.png "ベクトルの X 印の付いたベロシティ値 100 と Y を = = 0 で次のように視覚化することができます")

このベクトルは、新しい速度で回転できます。 たとえば、ベクターを (反時計回りに回転を使用)、45 度回転結果、次の。

![](math-images/image8.png "ベクトルを反時計回りに回転結果を使用してこの 45 度回転")

さいわい、速度、アクセラレータ、および偶数位置できますすべてで回転する値が適用されるしくみに関係なく、同じコード。 次の汎用的な関数は、CocosSharp 回転値によって、回転するベクトルを使用できます。


```csharp
// Rotates the argument vector by degrees specified by
// cocosSharpDegrees. In other words, the rotation
// value is expected to be clockwise.
// The vector parameter is modified, so it is both an in and out value
void RotateVector(ref CCVector2 vector, float cocosSharpDegrees)
{
    // Invert the rotation to get degrees as is normally
    // used in math (counterclockwise)
    float mathDegrees = -cocosSharpDegrees;

    // Convert the degrees to radians, as the System.Math
    // object expects arguments in radians
    float radians = CCMathHelper.ToRadians (mathDegrees);

    // Calculate the "up" and "right" vectors. This is essentially
    // a 2x2 matrix that we'll use to rotate the vector
    float xAxisXComponent = (float)System.Math.Cos (radians);
    float xAxisYComponent = (float)System.Math.Sin (radians);
    float yAxisXComponent = (float)System.Math.Cos (radians + CCMathHelper.Pi / 2.0f);
    float yAxisYComponent = (float)System.Math.Sin (radians + CCMathHelper.Pi / 2.0f);

    // Store the original vector values which will be used
    // below to perform the final operation of rotation.
    float originalX = vector.X;
    float originalY = vector.Y;

    // Use the axis values calculated above (the matrix values)
    // to rotate and assign the vector.
    vector.X = originalX * xAxisXComponent + originalY * yAxisXComponent;
    vector.Y = originalX * xAxisYComponent + originalY * yAxisYComponent;
} 
```

十分に理解、`RotateVector`メソッドは、この記事の範囲外であるいくつかの線形代数と共にコサインとサイン (正弦) の三角関数を熟知されている必要があります。 ただし、1 回実装、`RotateVector`メソッドは、回転速度ベクターを含む、任意のベクトルを使用することができます。 たとえば、次のコードは可能性がありますで指定された方向に行頭文字を起動させる、`rotation`値。


```csharp
// Create a Bullet instance
Bullet newBullet = new Bullet();

// Define the velocity of the bullet when 
// rotation is 0
CCVector2 velocity = new CCVector2 (0, 100);

// Modify the velocity according to rotation
RotateVector (ref velocity, rotation);

// Assign the newBullet's velocity using the
// rotated vector
newBullet.VelocityX = velocity.X;
newBullet.VelocityY = velocity.Y;

// Set the bullet's rotation so it faces
// the direction that it's flying
newBullet.Rotation = rotation; 
```

このコードは、ようなものを生成可能性があります。

![](math-images/image9.png "このコードは、このスクリーン ショットのようなものを生成可能性があります。")


## <a name="summary"></a>まとめ

このガイドでは、2 D ゲーム開発での一般的な数学的な概念について説明します。 割り当てるし、速度とアクセラレータ、実装する方法について説明し、オブジェクトとの任意の方向に移動ベクターを回転する方法について説明します。
