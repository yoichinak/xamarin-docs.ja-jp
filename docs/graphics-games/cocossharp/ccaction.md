---
title: CCAction によるアニメーション
description: CCAction クラスは、CocosSharp ゲームに追加するアニメーションを簡素化します。 ポーランド語を追加するまたは機能を実装するために、これらのアニメーションを使用できます。
ms.prod: xamarin
ms.assetid: 74DBD02A-6F10-4104-A61B-08CB49B733FB
author: conceptdev
ms.author: crdun
ms.date: 03/24/2017
ms.openlocfilehash: c486bb2e78579360e0f935219cd82958fedee34b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120971"
---
# <a name="animating-with-ccaction"></a>CCAction によるアニメーション

_CCAction クラスは、CocosSharp ゲームに追加するアニメーションを簡素化します。ポーランド語を追加するまたは機能を実装するために、これらのアニメーションを使用できます。_

`CCAction` CocosSharp オブジェクトをアニメーション化するために使用できる基本クラスです。 このガイドは、組み込み`CCAction`配置、スケーリング、および回転などの一般的なタスクを実装します。 継承することによってカスタム実装を作成する方法についても説明`CCAction`します。

このガイドでは、プロジェクトと呼ばれる**ActionProject**を[でダウンロードできます](https://developer.xamarin.com/samples/mobile/CCAction)します。 このガイドでは、`CCDrawNode`については、クラス、 [CCDrawNode によるジオメトリ描画 Geometry](~/graphics-games/cocossharp/ccdrawnode.md)ガイド。


## <a name="running-the-actionproject"></a>ActionProject を実行しています。

**ActionProject**は iOS と Android 向けにビルドできる CocosSharp ソリューションです。 機能を使用する方法のサンプル コードであると、`CCAction`クラスと共通のリアルタイム デモとして`CCAction`実装します。

を実行している場合は 3 つの ActionProject が表示されます`CCLabel`画面と 2 つで描画されるビジュアル オブジェクトの左上のインスタンス`CCDrawNode`さまざまなアクションを表示するためのインスタンス。

![](ccaction-images/image1.png "ActionProject 画面とさまざまなアクションを表示するための 2 つの CCDrawNode インスタンスによって描画されるビジュアル オブジェクトの左側に 3 つの表すローカライズされたインスタンスを表示します")

左のラベルの種類を示す`CCAction`画面をタップするときに作成されます。 既定で、**位置**値を選択すると、その結果、`CCMove`アクションが作成され、赤い円の部分に適用されています。

![](ccaction-images/image2.gif "位置の値を選択すると、作成し、赤い円の部分に適用される CCMove アクション中の結果")

どの種類の左側にラベルをクリックすると変更`CCAction`円で実行されます。 たとえばをクリックすると、**位置**ラベルを順を変更できる、さまざまな値。

![](ccaction-images/image3.gif "変更可能なさまざまな値を循環位置のラベルをクリックします。")

## <a name="common-variable-changing-ccaction-classes"></a>一般的な CCAction クラスの変数を変更します。

**ActionProject** 、次を使用して`CCAction`-CocosSharp の一部のクラスを継承します。

 - `CCMoveTo` – 変更を`CCNode`インスタンスの`Position`プロパティ
 - `CCScaleTo` – 変更を`CCNode`インスタンスの`Scale`プロパティ
 - `CCRotateTo` – 変更を`CCNode`インスタンスの`Rotation`プロパティ

このガイドでは、これらのアクションとして*変数を変更する*の変数に直接影響することを意味する、`CCNode`に追加されます。 その他の種類のアクションと呼びます*イージング*アクションは、このガイドで後で説明されます。

**ActionProject**これらのアクションの目的は、時間の経過と共に、変数を変更することを示します。 具体的には、これら`CCActions`コンス トラクターは、2 つの引数を受け取る: までの時間がかかると代入する値の長さ。 次のコードは、次の 3 つの種類のアクションを作成する方法を示しています。


```csharp
switch (VariableOptions [currentVariableIndex])
{
    case "Position":
        coreAction = new CCMoveTo(timeToTake, touch.Location);

        break;
    case "Scale":
        var distance = CCPoint.Distance (touch.Location, drawNodeRoot.Position);
        var desiredScale = distance / DefaultCircleRadius;
        coreAction = new CCScaleTo(timeToTake, desiredScale);

        break;
    case "Rotation":
        float differenceY = touch.Location.Y - drawNodeRoot.PositionY;
        float differenceX = touch.Location.X - drawNodeRoot.PositionX;

        float angleInDegrees = -1 * CCMathHelper.ToDegrees(
            (float)System.Math.Atan2(differenceY, differenceX));

        coreAction = new CCRotateTo (timeToTake, angleInDegrees);

        break; 
...
}
```

アクションが作成されると、次のように、CCNode に追加されます。


```csharp
nodeToAddTo.AddAction (coreAction); 
```

`AddAction` 開始、`CCAction`インスタンスの動作、およびそれが自動的に実行のロジックの後のフレームが完了するまでです。

単語で終わるを上記の各種類*に*つまり、`CCAction`が変更されます、`CCNode`アクションが完了すると、引数の値が最終的な状態を表すようにします。 などの作成、 `CCMoveTo` = 100 と Y の X 位置の = 200 の結果で、`CCNode`インスタンスの`Position`X に設定されています = 100、Y = 200 に関係なく、指定した時間の終了時刻、`CCNode`インスタンスの開始位置。

各"クラスの To"も「によって」、バージョンを持っての現在の値に、引数の値が追加されます、`CCNode`します。 などを作成する、 `CCMoveBy` = 100 と Y の X 位置の = 200 なります、`CCNode`アクションが開始されたときに位置から右側 100 単位と 200 ユニットに移行中のインスタンス。


## <a name="easing-actions"></a>操作を簡略化

変数を変更するアクションを実行する既定では、*線形補間*– アクションが一定の率で目的の値の方向に移動します。 補間する場合*位置*アクションの実行時の速度が一定に保たと線形に移動するオブジェクトすぐに開始および停止アクションの前後に移動します。 

非線形補間は、小さい少し目障りな感じ CocosSharp さまざまな変数を変更するアクションを変更するために使用できる操作を簡略化を提供するために、ポーランド語の要素を追加します。

**ActionProject**サンプルでは、このような 2 番目のラベルをクリックしてイージング アクション間で切り替えることができます (既定では**<None>**)。

![](ccaction-images/image4.gif "ユーザーがこのような 2 番目のラベルをクリックしてイージング アクション間で切り替えることができます。")

イージング アクションは、どの特定の変数設定アクションに関連付けられていないため、特に強力です。 これは、位置、回転、スケール、またはカスタム動作を割り当てる (このガイドの後半に表示されます) と同じイージング アクションを使用できることを意味します。

イージング アクション変数設定の操作をラップする (変数設定のアクションを継承する限り`CCFiniteTimeAction`)、コンス トラクターの引数として変数設定のアクションをそのまま使用しています。

たとえば、ラベルが設定されている場合**位置**、 **CCEaseElastic**タッチが検出されたときに、次のコードが実行されます (該当する行を強調表示するコードが省略されていることに注意してください)。


```csharp
CCFiniteTimeAction coreAction = null; 
...
coreAction = new CCMoveTo(timeToTake, touch.Location); 
...
CCAction easing = null; 
...
easing = new CCEaseSineOut (coreAction); 
...
nodeToAddTo.AddAction (easing); 
```

ように、アプリケーションによって、正確な同じイージングに適用できるその他の変数設定のアクションなど`CCRotateTo`:

![](ccaction-images/image5.gif "CCRotateTo などの他の変数設定操作に適用できる正確な同じイージング")


## <a name="easing-in-out-and-inout"></a>、Out および InOut を簡略化

すべてのイージング アクションが`In`、 `Out`、または`InOut`イージングの種類に追加されます。 これらの用語を参照してください、イージングを適用すると:`In`意味を先頭に適用されるイージング`Out`最後に、意味と`InOut`先頭と末尾の両方を意味します。

`In`全体 (両方の先頭と末尾に)、全体の補間で変数が適用される方法に影響が軽減のアクションがイージング アクションの最も顕著な特性は通常、最初に発生しますが、します。 同様に、`Out`イージング アクションは、補間の最後に、その動作によって特徴付けられます。 たとえば、`CCEaseBounceOut`アクションの最後にバウンス オブジェクトになります。


### <a name="out"></a>Out

`Out` 通常、イージング、補間の end で最も顕著な変更が適用されます。 たとえば、`CCEaseExponentialOut`対象の値に近づくにつれて変化する変数の変化率が低下します。

![](ccaction-images/image6.gif "対象の値に近づくにつれて、CCEaseExponentialOut の変化する変数の変化率が低下します。")


### <a name="in"></a>イン

`In` 通常、イージング、補間の先頭に最も大きな変更が適用されます。 たとえば、`CCEaseExponentialIn`より緩やかに変化、アクションの先頭に移動されます。

![](ccaction-images/image7.gif "アクションの先頭に CCEaseExponentialIn がより緩やかに変化移動します。")


### <a name="inout"></a>InOut

`InOut` 通常、先頭と末尾の両方の最も顕著な変更が適用されます。 `InOut` 簡略化は、通常は対称です。 たとえば、`CCEaseExponentialInOut`先頭とアクションの最後に、緩やかに変化が移動します。

![](ccaction-images/image8.gif "CCEaseExponentialInOut は先頭とアクションの最後にゆっくり")


## <a name="implementing-a-custom-ccaction"></a>カスタム CCAction を実装します。

CocosSharp 一般的な機能を提供するには、すべてのこれまでに説明したクラスが含まれます。 カスタム`CCAction`実装がさらに高い柔軟性を提供します。 たとえば、`CCAction`エクスペリエンス バーの塗りつぶされた比率を制御することができます、ユーザー エクスペリエンスを獲得するたびに、操作バーが滑らかに拡大されるようにします。

`CCAction` 通常、実装には、2 つのクラスが必要です。

 - `CCFiniteTimeAction` アクションを開始するため、有限の時間操作クラスは実装です。 これは、クラスがインスタンス化してに直接追加するか、`CCNode`またはイージング アクションにします。 インスタンス化し、返す必要がありますが、 `CCFiniteTimeActionState`、これには、更新を実行します。
 - `CCFiniteTimeActionState` アクションに関連する変数を更新するため、有限の時間操作状態クラスは実装です。 時間の値に基づいてターゲットの値を代入する更新関数、実装する必要がある必要があります。 このクラスは明示的に外部で参照されていません、`CCFiniteTimeAction`が作成されます。 「舞台裏」単に動作します。

**ActionProject**提供カスタム`CCFiniteTimeAction`という実装`LineWidthAction,`黄色の赤い円の上に描画される線の幅の調整に使用します。 その唯一の仕事がのインスタンスを作成して返すには、`LineWidthState`インスタンス。


```csharp
public class LineWidthAction : CCFiniteTimeAction
{
    float endWidth;

    public LineWidthAction (float duration, float width) : base(duration)
    {
        endWidth = width;
    }

    public override CCFiniteTimeAction Reverse ()
    {
        throw new NotImplementedException ();
    }

    protected override CCActionState StartAction (CCNode target)
    {
        return new LineWidthState (this, target, endWidth);
    }
}
```

、上記のとおり、`LineWidthState`の線の割り当てのしくみを示す`Width`量に従ってプロパティ`time`経過します。


```csharp
public class LineWidthState : CCFiniteTimeActionState
{
    float deltaWidth;
    float startWidth;

    LineNode castedTarget;

    public LineWidthState(LineWidthAction action, CCNode target, float endWidth) : base(action, target)
    {
        castedTarget = target as LineNode;

        if (castedTarget == null)
        {
            throw new InvalidOperationException ("The argument target must be a LineNode");
        }

        startWidth = castedTarget.Width;
        deltaWidth = endWidth - startWidth;
    }

    public override void Update (float time)
    {
        castedTarget.Width = startWidth + deltaWidth * time;
    }
} 
```

次のアニメーションで示すように、さまざまな方法で線の幅を変更するイージング操作で、LineWidthAction を結合できます。

![](ccaction-images/image9.gif "このアニメーションで示すように、イージング アクションをさまざまな方法で線の幅を変更すると、LineWidthAction を組み合わせることができます。")


### <a name="interpolation-and-the-update-method"></a>補間と、Update メソッド

別に、上記のクラスで値を格納する、唯一のロジックに在住、`LineWidthState.Update`メソッド。 `startWidth`変数、ターゲットの幅を格納する`LineNode`、アクションの開始時と`deltaWidth`変数がどの程度の値は、操作の過程で変更を格納します。

置き換えることによって、`time`変数値が 0 で、ことを確認できますターゲット`LineNode`の開始位置になります。


```csharp
castedTarget.Width = startWidth + deltaWidth * 0; 
```

同様に、ことを確認できますターゲット`LineNode`時間変数を 1 の値を代入することによって、転送先になります。


```csharp
castedTarget.Width = startWidth + deltaWidth * 1; 
```

`time`値は 0 と 1 の間に通常なりますが、常にではありません - と`Update`実装は、これらの境界を想定しないでください。 いくつかのイージング メソッド (など`CCEaseBackIn`と`CCEaseBackOut`)、0 ~ 1 の範囲外の時間値を指定します。


## <a name="conclusion"></a>まとめ

補間し、イージング、光沢のあるゲームを作成するユーザー インターフェイスを作成するときに特に重要な部分です。 このガイドは、使用する方法を説明します。`CCActions`位置や回転などの標準値およびカスタム値を補間します。 `LineWidthState`と`LineWidthAction`クラスがカスタム動作を実装する方法を示します。

## <a name="related-links"></a>関連リンク

- [CCAction](https://developer.xamarin.com/api/type/CocosSharp.CCAction)
- [CCMoveTo](https://developer.xamarin.com/api/type/CocosSharp.CCMoveTo)
- [CCScaleTo](https://developer.xamarin.com/api/type/CocosSharp.CCScaleTo)
- [CCRotateTo](https://developer.xamarin.com/api/type/CocosSharp.CCRotateTo)
- [CCDrawNode](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode)
- [完全なサンプル](https://developer.xamarin.com/samples/mobile/CCAction/)
