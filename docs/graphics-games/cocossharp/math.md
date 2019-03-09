---
title: CocosSharp による 2 次元数値演算
description: このガイドでは、ゲーム開発用の 2D 数学について説明します。 CocosSharp を使用して、ゲーム開発の一般的なタスクを実行する方法について説明し、これらのタスクの背後にある数学について説明します。
ms.prod: xamarin
ms.assetid: 5C241AB4-F97E-4B61-B93C-F5D307BCD517
author: conceptdev
ms.author: crdun
ms.date: 03/27/2017
ms.openlocfilehash: ac84d5b28b0f211dccb1697a4b3dbbc9cedf81e9
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670165"
---
# <a name="2d-math-with-cocossharp"></a>CocosSharp による 2 次元数値演算

_このガイドでは、ゲーム開発用の 2D 数学について説明します。CocosSharp を使用して、ゲーム開発の一般的なタスクを実行する方法について説明し、これらのタスクの背後にある数学について説明します。_

配置して、コードでオブジェクトを移動するには、あらゆる規模のゲームを開発のコア部分です。 配置と移動にゲームでは、直線、または回転角度の三角関数の使用に沿ったオブジェクトを移動する必要があるかどうか、数式の使用が必要です。 このドキュメントは、次のトピックを取り上げます。

 - ベロシティ
 - 高速化
 - CocosSharp のオブジェクトを回転させる
 - 回転を使用して、velocity の使用

開発者が厳密な数値演算バック グラウンドでは、ユーザーがないか、ユーザーが時間の長い忘れ、学校からこれらのトピックは、心配する必要はありません – このドキュメントは、概念を自作の部分に分割し、実用的な例を含む理論の説明を伴う、します。 つまり、この記事では昔からある数学学生という質問に回答は。「とは実際に必要がありますこれを使ってでしょうか。」


## <a name="requirements"></a>必要条件

コード サンプルがフォームを継承するオブジェクトの操作を想定していますが、このドキュメントでは、主 CocosSharp の数学的な側面に焦点を当てています、`CCNode`します。 さらに、以降`CCNode`値を含まないコードを velocity との高速化、VelocityX、VelocityY、AccelerationX、および AccelerationY などの値を提供するエンティティの操作を想定しています。 エンティティの詳細については、このチュートリアルを参照してください。 [CocosSharp のエンティティ](~/graphics-games/cocossharp/entities.md)します。


## <a name="velocity"></a>ベロシティ

ゲーム開発者という用語を使用して、 *velocity*オブジェクトが移動 – 方法を説明する速度が移動し、方向は移動します。 

Velocity が 2 種類の単位で定義されている: 位置の単位と時間の単位。 たとえば、自動車の速度は、1 時間あたりのマイルまたはキロメートル 1 時間あたりとして定義されます。 ゲームの開発者は多くの場合、使用してピクセルどれほど高速オブジェクトを定義する 1 秒あたりに移動します。 たとえば、行頭文字は、300 ピクセル、1 秒あたりの速度で移動可能性があります。 行頭文字が 1 秒あたり 300 ピクセルに移動しに移動 600 単位 2 秒と 900 の単位で、3 秒でします。 速度の値では一般的には、*追加*オブジェクトの位置を (後ほど以下)。

Velocity の単位を説明する速度を使いましたが用語速度通常の値を参照の方向、独立した用語の速度が速度と方向の両方には。 そのため、行頭の速度を (行頭文字が必要なプロパティが含まれるクラスを想定) の割り当てが次に示します。


```csharp
// This bullet is not moving horizontally, so set VelocityX to 0:
bulletInstance.VelocityX = 0;
// Positive Y is "up" so move the bullet up 300 units per second:
bulletInstance.VelocityY = 300;
```


### <a name="implementing-velocity"></a>Velocity の実装

CocosSharp では、移動を必要とするオブジェクトは、独自の移動ロジックを実装する必要がありますので、速度は実装しません。 多くの場合、velocity を実装する新しいのゲーム開発者では、フレーム レートに依存、velocity の間違いを行います。 次は、*を正しく実装*は正しい結果を提供するようですは、ゲームのフレーム レートに基づきます。

```csharp
// VelocityX and VelocityY will be added every time this code executes
this.PositionX += this.VelocityX;
this.PositionY += this.VelocityY;
```

ゲームは、フレーム レート (1 秒あたり 30 フレームではなく、1 秒あたり 60 フレーム) などで実行する場合、オブジェクトは、低速のフレーム レートで実行されている場合よりも高速に表示されます。 同様に、ゲームは、(これは、デバイスのリソースを使用してバック グラウンド プロセスが考えられます) フレーム レートの高位のフレームを処理できない場合は、ゲームが速度が低下する表示されます。

これに対応するには、速度は多くの場合を使用して実装時間の値。 たとえば場合、`seconds`変数を表します (秒) の数 (または端数) 最後の時間の速度が適用されるので、フレーム レートに関係なく一貫した動作を持つオブジェクトで、次のコードになり、。

```csharp
// VelocityX and VelocityY will be added every time this code executes
this.PositionX += this.VelocityX * seconds;
this.PositionY += this.VelocityY * seconds;
```

下のフレーム レートでを実行するゲームが少ないそのオブジェクトの位置を頻繁に更新ことを検討してください。 そのため、各更新プログラムは、ゲームをより頻繁に更新していた場合はさらに移動するオブジェクトになります。 `seconds`前回の更新以降の経過時間の量を報告することによって、このアカウントの値します。

時間ベースのアニメーションを追加する方法の例は、次を参照してください。[ベースの移動の時間をカバーするこのレシピ](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/game_development/time_based_movement)します。


### <a name="calculating-positions-using-velocity"></a>Velocity を使用して位置の計算

一定の時間の経過後にオブジェクトがされるについて予測を行うこと、またはゲームを実行することがなく、オブジェクトの動作を調整できる、velocity を使用できます。 たとえば、発生した項目の移動を実装する開発者がインスタンス化された後に、行頭の速度を設定する必要があります。 速度を設定するための基礎を提供する画面のサイズを使用できます。 開発者に知らせている場合は、行頭文字が 2 秒単位で、画面の高さを移動する必要がありますし、velocity は、2 で割った、画面の高さに設定する必要があります。 画面が 800 ピクセルの場合は、行頭の速度は 400 (つまり 800/2) に設定されます。

同様に、ゲーム内のロジックは、オブジェクトにかかる速度を指定した宛先に到達するを計算する必要があります。 これは、出張のオブジェクトの速度で移動する距離を割ることによって計算できます。 たとえば、次のコードはどのくらいの期間を表示するラベルにテキストを割り当てる方法を示しています。、ミサイルのターゲットに到達するまで。


```csharp
// We'll assume only the X axis for this example
float distanceX = target.PositionX - missile.PositionX;

float secondsToReachTarget = distanceX / missile.VelocityX;

label.Text = secondsToReachTarget + " seconds to reach target"; 
```


## <a name="acceleration"></a>高速化

*高速化*ゲーム開発では、一般的な概念は、多くの類似点と同じ速度でします。 高速化は、オブジェクトが高速化、または (ベロシティ値の変更時間の経過と共にどのように) パフォーマンスの低下するかどうかを定量化します。 高速化*追加*速度、velocity が配置に追加のと同じようにします。 高速化の一般的なアプリケーションには、重力、車の高速化、およびその thrusters を発生させるスペースが含まれます。 

速度と同様に、高速化で定義されている位置と時間単位です。ただし、高速化の時間の単位として参照されます、*二乗*単位で、高速化が数学的に定義されている方法が反映されます。 ゲームの高速化の多くの場合、単位の*1 秒あたりのピクセルの 2 乗*します。

オブジェクトは、2 乗 1 秒あたり 10 単位の X の高速化が、その速度が 10 秒ごとで増加するを意味します。 1 秒になります 10 秒、後に 2 単位に移動した後、かねないから始まる場合は、2 つ目のあたり 20 単位を秒します。

2 次元での高速化では次のように割り当てることができますので、X と Y コンポーネントが必要です。


```csharp
// No horizontal acceleration:
icicle.AccelerationX = 0;
// Simulate gravity with Y acceleration. Negative Y is down, so assign a negative value:
icicle.AccelerationY = -50; 
```


### <a name="acceleration-vs-deceleration"></a>加速と減速の比較

毎日音声認識で加速と減速を差別化して場合がありますが、2 つの技術の違いはありません。 重力は、高速化につながります力です。 オブジェクトが上にスローされた場合重力が低下し、(減速) が、オブジェクトが上昇が停止し、重力と同じ方向に解消が発生したら、重力は、高速化 (加速)。 下図のように、高速化のアプリケーションは、同じが同じ方向または逆方向の移動に適用されているかどうかです。 


### <a name="implementing-acceleration"></a>高速化を実装します。

実装する場合は、高速化がベロシティに類似 – CocosSharp、によって自動的に実装されないが、時間ベースの高速化 (フレーム ベースの高速化) ではなく、必要な実装。 そのため (velocity) と共に単純な高速化の実装がようになります。

```csharp
this.VelocityX += this.AccelerationX * seconds;
this.VelocityY += this.AccelerationY * seconds;
this.PositionX += this.VelocityX * seconds;
this.PositionY += this.VelocityY * seconds;
```

上記のコードとして参照されて、*線形近似*の高速化の実装。 実際には、精度の度合いが非常に近くの高速化を実装が、高速化の正確なモデルではありません。 上の高速化を実装する方法の概念を説明するために含まれます。

次の実装では、高速化し、velocity の正確な数学的にアプリケーションを示します。


```csharp
float halfSecondsSquared = (seconds * seconds) / 2.0f;

this.PositionX += 
    this.Velocity.X * seconds + this.AccelerationX * halfSecondsSquared;
this.PositionY += 
    this.Velocity.Y * seconds + this.AccelerationY * halfSecondsSquared;

this.VelocityX += this.AccelerationX * seconds;
this.VelocityY += this.AccelerationY * seconds; 
```

上記のコードに最も明白な違いは、`halfSecondsSquared`変数とその使用法を配置するには、高速化を適用します。 その理由は、数学的が、このチュートリアルの範囲を超えていますが、この数式で関心のある開発者は詳細を確認できます[の高速化の統合については、この説明します。](http://www.cliffsnotes.com/math/calculus/calculus/integration/distance-velocity-and-acceleration)

実際の影響`halfSecondSquare`が高速化が動作する数学的に正確かつ予測可能なフレーム レートに関係なく。 高速化の線形近似はフレーム レート: 対象を低く、フレーム レート、近似に正確な少なくなります。 使用して`halfSecondsSquared`保証のフレーム レートにかかわらず同じコードで動作するようになります。


## <a name="angles-and-rotation"></a>角度と回転

などのビジュアル オブジェクト`CCSprite`経由の回転をサポートする`Rotation`変数。 これは、角度の回転を設定する値に割り当てることができます。 たとえば、次のコードが回転する方法を示しています、`CCSprite`インスタンス。


```csharp
CCSprite unrotatedSprite = new CCSprite("star.png");
unrotatedSprite.IsAntialiased = false;
unrotatedSprite.PositionX = 100;
unrotatedSprite.PositionY = 100;
this.AddChild (unrotatedSprite);

CCSprite rotatedSprite = new CCSprite("star.png");
rotatedSprite.IsAntialiased = false;
// This sprite is moved to the right so it doesn’t overlap the first
rotatedSprite.PositionX = 130;
rotatedSprite.PositionY = 100;
rotatedSprite.Rotation = 45;
this.AddChild (rotatedSprite); 
```

次のような結果が得られます。

![](math-images/image1.png "これは、結果、このスクリーン ショット")

回転角度が 45 度時計回りに (つまり歴史的な理由から、回転は数学的に適用する方法の反対) に注意してください。

![](math-images/image2.png "回転では時計回りに 45 度は歴史的な理由からは反対の回転は数学的に適用する方法に注意してください。")

一般には、次のように CocosSharp および定期的な数学の回転を視覚化できます。

![](math-images/image3.png "CocosSharp および定期的な数学の回転を視覚化して、このような一般")

![](math-images/image4.png "CocosSharp および定期的な数学の回転を視覚化して、このような一般")

この区別は重要なので、`System.Math`クラスは反時計回りの回転を使用するため、このクラスに慣れている開発者が使用する場合は、角度を反転する必要があります`CCNode`インスタンス。

上記のダイアグラムが度の回転を表示することに注意する必要があります。ただし、一部の数学関数 (関数など、`System.Math`名前空間) 期待し、内の値を返す*ラジアン*度ではなく。 このガイドで少し後で 2 つのユニットの種類間で変換する方法について説明します。


### <a name="rotating-to-face-a-direction"></a>方向に回転

上記のよう`CCSprite`を使用して回転できる、`Rotation`プロパティ。 `Rotation`によって提供されるプロパティ`CCNode`(の基本クラス`CCSprite`)、つまり、回転はから継承するエンティティに適用できる`CCNode`もします。 

いくつかのゲームでは、ターゲット直面しているため、回転するオブジェクトが必要です。 例では、プレーヤーのターゲットでは、またはユーザーが画面をタッチ、ポイントの方向に操縦スペースで撮影コンピューター制御の敵があります。 ただし、回転値必要がありますまずに基づいて計算されます直面するターゲットの場所と回転されているエンティティの場所。

このプロセスでは、いくつかの手順が必要です。

 - 識別する、*オフセット*のターゲット。 オフセットは、X と Y の距離、回転を表すエンティティと対象のエンティティ間を参照します。
 - (以下で詳しく説明します) のアーク タンジェント三角法関数を使用して、オフセット位置からの角度を計算しています。
 - 右側と回転されていないときに、回転を表すエンティティが指す方向に向かっている 0 度の違いを調整します。
 - 数学的な回転 (反時計回りに回転) と CocosSharp の回転角度 (時計回り) の違いを調整します。

(エンティティで記述すると仮定)、次の関数は、ターゲットに直面するエンティティを回転します。


```csharp
// This function assumes that it is contained in a CCNode-inheriting object
public void FacePoint(float targetX, float targetY)
{
    // Calculate the offset - the target's position relative to "this"
    float xOffset = targetX - this.PositionX;
    float yOffset = targetY - this.PositionY;

    // Make sure the target isn't the same point as "this". If so,
    // then rotation cannot be calculated.
    if (targetX != this.PositionX || targetY != this.Position.Y)
    {

        // Call Atan2 to get the radians representing the angle from 
        // "this" to the target
        float radiansToTarget = (float)System.Math.Atan2 (yOffset, xOffset);

        // Since CCNode uses degrees for its rotation, we need to convert
        // from radians
        float degreesToTarget = CCMathHelper.ToDegrees (radiansToTarget);

        // The direction that the entity faces when unrotated. In this case
        // the entity is facing "up", which is 90 degrees 
        const float forwardAngle = 90;

        // Adjust the angle we want to rotate by subtracting the
        // forward angle.
        float adjustedForDirecitonFacing = degreesToTarget - forwardAngle;

        // Invert the angle since CocosSharp uses clockwise rotation
        float cocosSharpAngle = adjustedForDirecitonFacing * -1;

        // Finally assign the rotation
        this.Rotation = rotation = cocosSharpAngle;
    }
} 
```

上記のコードは、場所、ユーザーが画面に触れて、次のように、ポイントを向くようにエンティティを回転される可能性があります。


```csharp
private void HandleInput(System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
{
    if(touches.Count > 0)
    {
        CCTouch firstTouch = touches[0];
        FacePoint (firstTouch.Location.X, firstTouch.Location.Y);
    }
} 
```

このコードは、次の動作が得られます。

![](math-images/image5.gif "このコードは、この動作を結果します。")

#### <a name="using-atan2-to-convert-offsets-to-angles"></a>角度オフセット Atan2 を使用して変換するには

`System.Math.Atan2` オフセットを角度に変換するために使用します。 関数名`Atan2`三角関数のアーク タンジェントに由来します。 「2」のサフィックスでは、この関数を区別標準から`Atan`アーク タンジェントの数学的な動作を厳密に一致する関数。 アーク タンジェントとは-90 値を返す関数と +90 度 (または同等のラジアン単位)。 コンピューター ゲームなどを含む、多くのアプリケーションが必要な多くの場合、値の完全な 360 度ため、`Math`クラスが含まれています`Atan2`このニーズを満たすためにします。

上記のコード、Y パラメーター最初に、X パラメーターを渡しますを呼び出すときに注意してください、`Atan2`メソッド。 これは、X、Y は位置座標の順序には、通常から逆方向。 詳細については[Atan2 ドキュメントを参照してください。](https://msdn.microsoft.com/library/system.math.atan2(v=vs.110).aspx)します。

注目すべきもからの戻り値`Atan2`ラジアンは角度を測定するために使用もう 1 つの単位です。 このガイドはラジアン単位の詳細は説明しませんに注意してくださいを内のすべての三角関数、`System.Math`名前空間の値は、CocosSharp オブジェクトで使用する前に度に変換する必要があります (ラジアン単位) を使用します。 詳細については、ラジアンが見つかります[ラジアンの Wikipedia ページに](https://en.wikipedia.org/wiki/Radian)。

#### <a name="forward-angle"></a>順方向の角度

1 回、`FacePoint`メソッドの角度をラジアンに変換する、定義、`forwardAngle`値。 この値は、その回転値が 0 の場合のエンティティが向いて角度を表します。 この例で、エンティティを上向き、数学の回転 (CocosSharp 回転) ではなくを使用する場合は 90 度と想定されます。 数学的な回転ここで使用 CocosSharp の回転をまだ反転していないためです。

どのようなエンティティを次に示します、`forwardAngle`が 90 度のようになります。

![](math-images/image6.png "これが 90 度の forwardAngle を持つエンティティの外観を示しています")


### <a name="angled-velocity"></a>角度の速度

これまでのオフセットを角度に変換する方法を見てみました。 ここでは、他のようになる – 角度を受け取りし、X に変換する値と Y 値。 一般的な例には、車が向いていると、方向または宇宙船が向いている方向に移動する箇条書きを撮影スペース内の移動が含まれます。 

概念的には、されていない回転したときは、目的の速度を定義するエンティティが直面している角度に速度を回転して速度を計算できます。 この概念を説明するには、速度 (および加速度) を 2 次元として視覚化できます*ベクター* (通常が描画される矢印として)。 X の速度の値のベクター = 100 と Y = 0 は、次のように視覚化できます。

![](math-images/image7.png "X の速度の値のベクター = 100 と Y = 0 は次のように視覚化することができます")

新たな速度で結果として、このベクターを回転できます。 たとえば、ベクトルを (使用反時計回りに回転) 45 度回転では、次のように結果します。

![](math-images/image8.png "ベクターをこの反時計回りに回転の結果を使って 45 度回転")

さいわい、ベロシティ、高速化、および偶数位置できますすべてで回転する値を適用する方法に関係なく、同じコード。 CocosSharp 回転値によって、ベクターを回転する次の汎用関数を使用できます。


```csharp
// Rotates the argument vector by degrees specified by
// cocosSharpDegrees. In other words, the rotation
// value is expected to be clockwise.
// The vector parameter is modified, so it is both an in and out value
void RotateVector(ref CCVector2 vector, float cocosSharpDegrees)
{
    // Invert the rotation to get degrees as is normally
    // used in math (counterclockwise)
    float mathDegrees = -cocosSharpDegrees;

    // Convert the degrees to radians, as the System.Math
    // object expects arguments in radians
    float radians = CCMathHelper.ToRadians (mathDegrees);

    // Calculate the "up" and "right" vectors. This is essentially
    // a 2x2 matrix that we'll use to rotate the vector
    float xAxisXComponent = (float)System.Math.Cos (radians);
    float xAxisYComponent = (float)System.Math.Sin (radians);
    float yAxisXComponent = (float)System.Math.Cos (radians + CCMathHelper.Pi / 2.0f);
    float yAxisYComponent = (float)System.Math.Sin (radians + CCMathHelper.Pi / 2.0f);

    // Store the original vector values which will be used
    // below to perform the final operation of rotation.
    float originalX = vector.X;
    float originalY = vector.Y;

    // Use the axis values calculated above (the matrix values)
    // to rotate and assign the vector.
    vector.X = originalX * xAxisXComponent + originalY * yAxisXComponent;
    vector.Y = originalX * xAxisYComponent + originalY * yAxisYComponent;
} 
```

完全に理解、`RotateVector`メソッドはこの記事で取り上げませんが、いくつかの線形代数、および三角関数コサインとサインについて理解することが必要です。 ただし、1 回実装、`RotateVector`速度ベクターを含む、任意のベクトルを回転するメソッドを使用できます。 次のコードがで指定された方向の行頭文字を起動するなど、`rotation`値。


```csharp
// Create a Bullet instance
Bullet newBullet = new Bullet();

// Define the velocity of the bullet when 
// rotation is 0
CCVector2 velocity = new CCVector2 (0, 100);

// Modify the velocity according to rotation
RotateVector (ref velocity, rotation);

// Assign the newBullet's velocity using the
// rotated vector
newBullet.VelocityX = velocity.X;
newBullet.VelocityY = velocity.Y;

// Set the bullet's rotation so it faces
// the direction that it's flying
newBullet.Rotation = rotation; 
```

このコードが生じるようなものです。

![](math-images/image9.png "このコードは、このスクリーン ショットのようなものを生成可能性があります。")


## <a name="summary"></a>まとめ

このガイドでは、2 D ゲーム開発の一般的な数学的概念について説明します。 割り当てるおよびベロシティの高速化を実装する方法を表示し、回転オブジェクトとベクトルを任意の方向に移動する方法について説明します。
