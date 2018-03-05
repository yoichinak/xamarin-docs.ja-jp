---
title: "CCAction をアニメーション化します。"
description: "CCAction クラスでは、追加のアニメーション CocosSharp ゲームを簡略化します。 機能を実装したり、ポーランド語を追加する、これらのアニメーションを使用できます。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 74DBD02A-6F10-4104-A61B-08CB49B733FB
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: 2852cf0e141e8239cee8dbe580576f4571c919a3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="animating-with-ccaction"></a>CCAction をアニメーション化します。

_CCAction クラスでは、追加のアニメーション CocosSharp ゲームを簡略化します。機能を実装したり、ポーランド語を追加する、これらのアニメーションを使用できます。_

`CCAction` CocosSharp オブジェクトをアニメーション化するために使用する基本クラスです。 このガイドには、組み込みがについて説明します`CCAction`配置、スケーリング、および回転などの一般的なタスクの実装です。 継承することによってカスタム実装を作成する方法についても説明`CCAction`です。

このガイドでは、プロジェクトと呼ばれる**ActionProject**を[でダウンロードできます](https://developer.xamarin.com/samples/mobile/CCAction)です。 このガイドでは、`CCDrawNode`については、クラス、 [CCDrawNode で描画 Geometry](~/graphics-games/cocossharp/ccdrawnode.md)ガイドです。


# <a name="running-the-actionproject"></a>ActionProject を実行しています。

**ActionProject** CocosSharp ソリューションが iOS および Android 用構築されたことができます。 両方の使用方法のコード サンプルとして機能し、`CCAction`クラスと共通のリアルタイムのデモとして`CCAction`実装します。

を実行する場合は 3 つの ActionProject が表示されます`CCLabel`画面および 2 で描画されたビジュアル オブジェクトの左側にインスタンス`CCDrawNode`さまざまなアクションを表示するためのインスタンス。

![](ccaction-images/image1.png "ActionProject 画面と 2 つの CCDrawNode インスタンスによって作成されます。 さまざまなアクションを表示するためのビジュアル オブジェクトの左側に 3 つの表すローカライズされたインスタンスが表示されます。")

左のラベルの種類を示す`CCAction`画面上をタップすると作成されます。 既定では、**位置**値を選択すると、その結果、`CCMove`アクションが作成され、赤い円に適用されています。

![](ccaction-images/image2.gif "位置の値を選択すると、その結果、CCMove アクションの作成され、赤い円に適用")

左のラベルをクリックすると変更の種類の`CCAction`円でを実行します。 たとえばをクリックすると、**位置**ラベルが順番に異なる値を変更できます。

![](ccaction-images/image3.gif "位置のラベルをクリックするとが順番に異なる値を変更できます。")


# <a name="common-variable-changing-ccactions"></a>共通の変数を変更する CCActions 

**ActionProject** 、次を使用して`CCAction`-CocosSharp の一部となっているクラスを継承します。

 - `CCMoveTo` – 変更、`CCNode`インスタンスの`Position`プロパティ
 - `CCScaleTo` – 変更、`CCNode`インスタンスの`Scale`プロパティ
 - `CCRotateTo` – 変更、`CCNode`インスタンスの`Rotation`プロパティ

このガイドでは、これらのアクションとして*変数を変更する*の変数に直接影響することを意味する、`CCNode`に追加されます。 その他の種類のアクションと呼びます*イージング*アクションは、このガイドの後半で説明します。

**ActionProject**これらのアクションの目的は、時間の経過と共に、変数を変更することを示します。 具体的には、これら`CCActions`コンス トラクターは、2 つの引数を受け取る: 有効にして代入する値の長さ。 次のコードは、次の 3 つの種類のアクションを作成する方法を示しています。


```csharp
switch (VariableOptions [currentVariableIndex])
{
    case "Position":
        coreAction = new CCMoveTo(timeToTake, touch.Location);

        break;
    case "Scale":
        var distance = CCPoint.Distance (touch.Location, drawNodeRoot.Position);
        var desiredScale = distance / DefaultCircleRadius;
        coreAction = new CCScaleTo(timeToTake, desiredScale);

        break;
    case "Rotation":
        float differenceY = touch.Location.Y - drawNodeRoot.PositionY;
        float differenceX = touch.Location.X - drawNodeRoot.PositionX;

        float angleInDegrees = -1 * CCMathHelper.ToDegrees(
            (float)System.Math.Atan2(differenceY, differenceX));

        coreAction = new CCRotateTo (timeToTake, angleInDegrees);

        break; 
...
}
```

アクションが作成されると、次のように、CCNode に追加されます。


```csharp
nodeToAddTo.AddAction (coreAction); 
```

`AddAction` 開始、`CCAction`インスタンスの動作、およびそれが自動的に実行、ロジックの後に、フレームが完了するまでです。

という単語で終わる上の各種類の一覧に*に*を意味する、`CCAction`は、変更、`CCNode`引数の値が、アクションが完了すると、最終的な状態を表すようにします。 などを作成する、 `CCMoveTo` X の位置 = 100 と Y = 200 の結果で、`CCNode`インスタンスの`Position`X に設定されている = 100、Y = 200 に関係なく、指定した時間の終了時刻、`CCNode`インスタンスの場所を開始します。

各"To"クラスも「によって」、バージョンを持っての現在の値には引数の値が追加されます、`CCNode`です。 などの作成、 `CCMoveBy` X の位置 = 100 と Y = 200 になります、`CCNode`アクションが開始されたときでは位置から右側 100 単位に、200 の単位を移動するインスタンス。


# <a name="easing-actions"></a>アクションの簡略化

変数を変更するアクションを実行する既定では、*線形補間*– アクションが一定の割合で目的の値の方向に移動します。 補間する場合*位置*の速度はそのまま、アクションを実行し、直線的に移動するオブジェクトがすぐに開始され先頭と、操作の最後に移動は停止します。 

非線形補間では、小さい少し目障りな感じがし、CocosSharp さまざまな変数を変更するアクションを変更するために使用するアクションを簡略化を提供するために、ポーランド語の要素を追加します。

**ActionProject**サンプルについては、この種類の 2 番目のラベルをクリックしてイージング操作の間で切り替えるおできます (既定値 **<None>** )。

![](ccaction-images/image4.gif "ユーザーは、この種類の 2 番目のラベルをクリックしてイージング操作を切り替えることができます。")

イージング アクションは、どの特定の変数設定アクションに関連付けられていないために、特に強力です。 これは、位置、回転、小数点以下桁数、またはカスタム動作を割り当てる (このガイドの後半で表示されます) と同じイージング アクションを使用できることを意味します。

変数設定の操作をラップするイージング アクション (変数設定アクションを継承から限り`CCFiniteTimeAction`)、コンス トラクターの引数として変数設定アクションを受け入れることによりします。

たとえば、ラベルが設定されている場合**位置**、 **CCEaseElastic**タッチが検出されたときに、次のコードが実行されます (該当する行を強調表示するコードが省略されているに注意してください)。


```csharp
CCFiniteTimeAction coreAction = null; 
...
coreAction = new CCMoveTo(timeToTake, touch.Location); 
...
CCAction easing = null; 
...
easing = new CCEaseSineOut (coreAction); 
...
nodeToAddTo.AddAction (easing); 
```

アプリケーションによってように、正確な同じイージングに適用できるその他の変数設定アクションなど`CCRotateTo`:

![](ccaction-images/image5.gif "CCRotateTo などの他の変数設定アクションに正確な同じイージングを適用できます。")


# <a name="easing-in-out-and-inout"></a>イージング In、Out、および InOut

すべてのアクションをイージングが`In`、 `Out`、または`InOut`イージング型に追加されます。 イージングが適用されるときは、これらの用語を参照:`In`イージングは開始時に、適用することを意味`Out`終了時に、ことを意味し、`InOut`先頭と末尾の両方を意味します。

`In`全体の補間 (両方の先頭と末尾に)、全体で変数が適用される方法に影響がアクションを簡略化いますが、通常最もわかりやすい特性イージング アクションの先頭に配置します。 同様に、`Out`イージング アクションは、補間の終了時の動作によって特徴付けられます。 たとえば、`CCEaseBounceOut`アクションの終了時バウンス オブジェクトになります。


## <a name="out"></a>Out

`Out` 一般に容易には、補間の最後に最も大きな変更が適用されます。 たとえば、`CCEaseExponentialOut`対象の値に近づくにつれて、変化する変数の変化率が低下します。

![](ccaction-images/image6.gif "対象の値に近づくにつれて、CCEaseExponentialOut の変化する変数の変化率が低下します。")


## <a name="in"></a>イン

`In` 通常の簡略化すると、補間の先頭に最も大きな変更が適用されます。 たとえば、`CCEaseExponentialIn`より遅くなりアクションの先頭に移動されます。

![](ccaction-images/image7.gif "アクションの先頭に CCEaseExponentialIn はより遅くなり移動します。")


## <a name="inout"></a>InOut

`InOut` 通常の先頭と末尾の両方で最も顕著な変更を適用します。 `InOut` イージングは通常対称です。 たとえば、`CCEaseExponentialInOut`先頭と、操作の最後に、緩やかに変化が移動します。

![](ccaction-images/image8.gif "先頭と末尾のアクションで CCEaseExponentialInOut が緩やかに変化移動します。")


# <a name="implementing-a-custom-ccaction"></a>カスタム CCAction を実装します。

CocosSharp 一般的な機能を提供するには、すべてのこれまでに説明したクラスが含まれます。 カスタム`CCAction`の実装がさらに高い柔軟性を提供します。 たとえば、`CCAction`エクスペリエンス バーの塗りつぶしの比率を制御することできますエクスペリエンス バーは、ユーザー エクスペリエンスが加算されるたびに滑らかに拡大できるようにします。

`CCAction` 実装には、通常、2 つのクラスが必要です。

 - `CCFiniteTimeAction` 実装: 有限時間アクション クラスは、操作を開始するためです。 インスタンス化するクラスに直接追加するか、`CCNode`またはイージング アクションにします。 インスタンスを作成およびを返す必要があります、 `CCFiniteTimeActionState`、更新プログラムが実行されます。
 - `CCFiniteTimeActionState` 実装 – 有限時間アクション状態クラスをアクションに関連する変数を更新します。 時刻の値に基づいてターゲットの値を割り当てます更新関数を実装してする必要があります。 このクラスは明示的に外部で参照されていません、`CCFiniteTimeAction`作成します。 「舞台裏」単に動作します。

**ActionProject** 、カスタムの提供`CCFiniteTimeAction`と呼ばれる実装`LineWidthAction,`黄色の赤い円の上に描画される線の幅の調整に使用されます。 その唯一の役割のインスタンスを作成して返す、`LineWidthState`インスタンス。


```csharp
public class LineWidthAction : CCFiniteTimeAction
{
    float endWidth;

    public LineWidthAction (float duration, float width) : base(duration)
    {
        endWidth = width;
    }

    public override CCFiniteTimeAction Reverse ()
    {
        throw new NotImplementedException ();
    }

    protected override CCActionState StartAction (CCNode target)
    {
        return new LineWidthState (this, target, endWidth);
    }
}
```

、上記のように、`LineWidthState`線の割り当ての作業を行う`Width`量に基づきプロパティ`time`経過します。


```csharp
public class LineWidthState : CCFiniteTimeActionState
{
    float deltaWidth;
    float startWidth;

    LineNode castedTarget;

    public LineWidthState(LineWidthAction action, CCNode target, float endWidth) : base(action, target)
    {
        castedTarget = target as LineNode;

        if (castedTarget == null)
        {
            throw new InvalidOperationException ("The argument target must be a LineNode");
        }

        startWidth = castedTarget.Width;
        deltaWidth = endWidth - startWidth;
    }

    public override void Update (float time)
    {
        castedTarget.Width = startWidth + deltaWidth * time;
    }
} 
```

次のアニメーションに示すように、さまざまな方法で線幅を変更するイージング操作で、LineWidthAction を結合できます。

![](ccaction-images/image9.gif "このアニメーションで示すようには、さまざまな方法で線幅を変更するイージングのアクション、LineWidthAction を結合できます。")


## <a name="interpolation-and-the-update-method"></a>補間と、Update メソッド

上記のクラス内の値を格納する場合を除いて、唯一のロジックが住んでいる、`LineWidthState.Update`メソッドです。 `startWidth`変数がターゲットの幅を格納`LineNode`、アクションの開始時と`deltaWidth`変数がどの程度の値は、操作の過程で変更を保存します。

置き換えることによって、 `time` 0 の値を持つ変数、ことが分かりますターゲット`LineNode`の開始位置になります。


```csharp
castedTarget.Width = startWidth + deltaWidth * 0; 
```

同様に、ことが分かりますターゲット`LineNode`時間変数を 1 の値に置き換えることによってその送信先になります。


```csharp
castedTarget.Width = startWidth + deltaWidth * 1; 
```

`time`値は 0 ~ 1 - で通常は常にではありません - と`Update`実装は、これらの境界を想定しないでください。 一部のイージング メソッド (など`CCEaseBackIn`と`CCEaseBackOut`) 0 ~ 1 の範囲外での時刻値を指定します。


# <a name="conclusion"></a>まとめ

補間とイージング、光沢のあるゲームを作成するユーザー インターフェイスを作成するときに特に重要な部分です。 このガイドを使用する方法を説明する`CCActions`位置と回転などの標準の値だけでなくカスタム値を補間します。 `LineWidthState`と`LineWidthAction`クラスは、カスタム アクションを実装する方法を示します。

## <a name="related-links"></a>関連リンク

- [CCAction](https://developer.xamarin.com/api/type/CocosSharp.CCAction)
- [CCMoveTo](https://developer.xamarin.com/api/type/CocosSharp.CCMoveTo)
- [CCScaleTo](https://developer.xamarin.com/api/type/CocosSharp.CCScaleTo)
- [CCRotateTo](https://developer.xamarin.com/api/type/CocosSharp.CCRotateTo)
- [CCDrawNode](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode)
- [完全なサンプル](https://developer.xamarin.com/samples/mobile/CCAction/)
