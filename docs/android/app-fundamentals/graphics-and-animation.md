---
title: グラフィックスとアニメーション
description: Android では、2 D グラフィックスとアニメーションをサポートするため、非常に豊富で多様なフレームワークを提供します。 このトピックでは、これらのフレームワークを紹介し、Xamarin.Android アプリケーションでカスタムのグラフィックスとアニメーションの使用を作成する方法について説明します。
ms.prod: xamarin
ms.assetid: 80086318-6FE4-4711-9A71-5C8F8C28C754
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: c49b8855bccaf2eca825096746769d7f201736c5
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116889"
---
# <a name="graphics-and-animation"></a>グラフィックスとアニメーション

_Android では、2 D グラフィックスとアニメーションをサポートするため、非常に豊富で多様なフレームワークを提供します。このトピックでは、これらのフレームワークを紹介し、Xamarin.Android アプリケーションでカスタムのグラフィックスとアニメーションの使用を作成する方法について説明します。_


## <a name="overview"></a>概要

にもかかわらず、制限付きの電源のこれまではデバイスで実行されている、最高の評価済みのモバイル アプリケーション多くの場合がある高度なユーザー エクスペリエンス (UX)、高品質なグラフィックとアニメーションを直感的で応答性の高い、動的フィールを提供します。 モバイル アプリケーションしより高度なユーザーが開始するアプリケーションからより多くの注意点です。

さいわい、最新のモバイル プラットフォームには使いやすさを維持しながら高度なアニメーションとカスタム グラフィックスを作成するための非常に強力なフレームワークがあります。 これにより、開発者はごくわずかな労力で高度な対話機能を追加できます。

Android の UI API フレームワークは、2 つのカテゴリに分割する約できます: グラフィックスとアニメーション。

グラフィックスは、2 D および 3D グラフィックスを行うためのさまざまな方法をさらに分割されます。 3D グラフィックは、組み込み OpenGL ES (モバイル特定のバージョンの OpenGL) などのフレームワークと MonoGame (XNA toolkit を使用した互換性のあるクロスプラット フォームのツールキット) などのサードパーティ製フレームワークの数を利用します。 3D グラフィックスが、この記事の範囲内にありませんが、組み込みの 2D 描画技法を究明します。

Android は、2 D グラフィックスを作成するための 2 つの異なる API を提供します。 1 つは、高レベルの宣言型の方法と他のプログラムによる低レベルの API です。

-   **ドローアブル リソース**&ndash;カスタム グラフィックスを描画命令を XML ファイルに埋め込むことでは、プログラム、または (通常はより) 作成に使用されます。 描画可能なリソースは通常の手順または 2D グラフィックを表示するために Android 用のアクションを含む XML ファイルとして定義されます。 

-   **キャンバス**&ndash;これは、基になるビットマップ上で直接描画を含む低レベルの API です。 表示される内容を非常にきめ細かく制御を提供します。 

これらの 2D グラフィックス手法だけでなく Android には、アニメーションを作成するいくつかの方法も用意されています。

-   **Drawable アニメーション** &ndash; Android と呼ばれるフレーム単位アニメーションもサポートしています。 *Drawable アニメーション*します。 これは、最も単純なアニメーション API です。 Android は、ロード、順番に (マンガ) 同様のシーケンスで描画可能なリソースが表示されます。

-   **アニメーション表示** &ndash; *ビュー アニメーション*元のアニメーション API の Android では、Android のすべてのバージョンで利用します。 この API はビュー オブジェクトでのみ機能し、それらのビューを単純な変換を実行できるのみに制限されます。
    通常、ビューのアニメーションは内の XML ファイルで定義、`/Resources/anim`フォルダー。

-   **プロパティのアニメーション** &ndash; Android 3.0 導入のアニメーション API の新しいセットと呼ばれる*プロパティのアニメーション*します。 これらの新しい API には、任意のオブジェクトのプロパティをアニメーション化するだけでなくオブジェクトを表示するために使用できる、拡張性と柔軟なシステムが導入されました。 この柔軟性により、アニメーション コード共有を容易にすると、個別のクラスにカプセル化します。


ビューのアニメーションは、古い 3.0 より前の Android API の (API レベル 11) をサポートする必要があるアプリケーションに適してします。 それ以外の場合アプリケーションは、上の理由から上記に説明した新しいのプロパティのアニメーション API を使用してください。

これらのフレームワークのすべてが実行可能なオプション、ただし可能であれば、設定する必要がありますを指定するプロパティのアニメーションより柔軟な API を使用します。 コード共有を容易になり、コードのメンテナンスを簡素化する個別のクラスにカプセル化のロジックをアニメーションのプロパティのアニメーションを許可します。


## <a name="accessibility"></a>ユーザー補助

グラフィックスとアニメーションが魅力的な Android アプリに役立つ楽しく; を使用するにはただしは、支援のズームまたはなれません、代替の入力デバイスを使用して、いくつかのやり取りが行われることに注意してください。
また、オーディオ機能を使用しない一部のやり取りが行われる可能性があります。

アクセシビリティを念頭に設計されている場合は、アプリはこのような状況でより使いやすい: ヒントと、ユーザー インターフェイスでのナビゲーション支援を提供し、テキスト コンテンツまたは UI の視覚要素の説明があることを確認します。

参照してください[Google のアクセシビリティ ガイド](http://developer.android.com/guide/topics/ui/accessibility/)Android のアクセシビリティの Api を利用する方法の詳細について。



## <a name="2d-graphics"></a>2D グラフィックス

描画可能なリソースは、Android アプリケーションで一般的な手法です。 他のリソースと描画可能なリソースは宣言型&ndash;XML ファイルで定義します。 このアプローチは、リソースからのコードを明確に分離できます。 更新または Android のアプリケーションのグラフィックを変更するコードを変更する必要はありませんので、開発と保守を簡素化このことができます。 ただし、描画可能なリソースは多数の単純で一般的なグラフィック要件場合に便利ですがない機能と、キャンバスの API のコントロール。

その他の手法を使用して、[キャンバス](https://developer.xamarin.com/api/type/Android.Graphics.Canvas/)オブジェクト、System.Drawing や iOS のコアの描画など他の従来の API フレームワークによく似ています。 キャンバス オブジェクトを使用してどのように 2D グラフィックスのほとんどのコントロールが作成を提供します。 または描画可能なリソースが、正しく機能しませんを使用するが難しくなりますような状況に適しています。 たとえば、スライダーの値に関連する計算に基づいて外観が変わりますカスタム スライダー コントロールを描画するために必要な場合があります。

最初に描画可能なリソースを調べてみましょう。 単純な最も一般的なカスタム描画のケースについて説明します。


### <a name="drawable-resources"></a>描画可能なリソース

描画可能なリソースがディレクトリ内の XML ファイルで定義されている`/Resources/drawable`します。 PNG、または JPEG の埋め込みとは異なり、密度に固有のバージョンの描画可能なリソースを提供する必要はありません。
時に、Android アプリケーションはこれらのリソースを読み込むし、これらの XML ファイルに含まれる指示を使用して 2D グラフィックスを作成します。
Android では、いくつかの異なる種類の描画可能なリソースを定義します。

-   [ShapeDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Shape) &ndash;プリミティブの幾何学図形を描画し、限られたその図形にグラフィック効果を適用する描画可能なオブジェクトになります。 ボタンをカスタマイズまたはするテキスト ビューの背景を設定するなどの非常に便利です。 使用する方法の例が表示されます、`ShapeDrawable`この記事で後述します。

-   [StateListDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#StateList) &ndash;ウィジェットとコントロールの状態に基づいて外観を変更する描画可能なリソースであります。 たとえば、ボタンは、かどうかが押されたかどうかに応じてには、その外観を変更する可能性があります。

-   [LayerDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList) &ndash;この描画可能なリソースに別の上に 1 つの他のいくつかのドローアブルをスタックします。 例を*LayerDrawable*次のスクリーン ショットに示します。

    ![LayerDrawable 例](graphics-and-animation-images/image1.png)

-   [TransitionDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Transition) &ndash;これは、 *LayerDrawable*が 1 つの違い。 A *TransitionDrawable*上示す別の 1 つの層をアニメーション化することができます。

-   [LevelListDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#LevelList) &ndash;に非常に似ています、 *StateListDrawable*ことで、特定の条件に基づくイメージが表示されます。 ただしとは異なり、 *StateListDrawable*、 *LevelListDrawable*整数値に基づくイメージが表示されます。 例を*LevelListDrawable*は WiFi 信号の強度を表示することです。 WiFi のシグナルの変更の強度、として表示される drawable を変更します。

-   [ScaleDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Scale)/[ClipDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Clip) &ndash;スケーリングおよび機能をクリッピングこれらドローアブルの提示をその名のとおりです。 *ScaleDrawable*は別の描画可能な while、スケール、 *ClipDrawable*別のディスプレイをクリップします。

-   [InsetDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Inset) &ndash;この Drawable 適用インセット描画可能なリソースを別の側面にします。 ビューが背景の色をビューの実際の範囲より小さい必要がある場合に使用されます。

-   XML [BitmapDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Bitmap) &ndash;このファイルは実際のビットマップに対して実行するには XML では、命令のセット。 Android を実行できる一部の操作とは、タイル、ディザリング、およびアンチ エイリアスです。 レイアウトの背景にビットマップを並べて表示する非常に一般的な用途の 1 つです。


#### <a name="drawable-example"></a>描画可能な例

2D グラフィックを使用して、作成する方法の簡単な例を見て、`ShapeDrawable`します。 A `ShapeDrawable` 4 つの基本的な図形のいずれかを定義できます。 四角形、楕円、線、およびリングします。 グラデーション、色、サイズなどの基本的な効果を適用することもできます。 次の XML に、`ShapeDrawable`にありますが、 *AnimationsDemo*コンパニオン プロジェクト (ファイルの`Resources/drawable/shape_rounded_blue_rect.xml`)。
紫の背景のグラデーションの四角形を定義し、角が丸きます。

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android" android:shape="rectangle">
<!-- Specify a gradient for the background -->
<gradient android:angle="45"
          android:startColor="#55000066"
          android:centerColor="#00000000"
          android:endColor="#00000000"
          android:centerX="0.75" />

<padding android:left="5dp"
          android:right="5dp"
          android:top="5dp"
          android:bottom="5dp" />

<corners android:topLeftRadius="10dp"
          android:topRightRadius="10dp"
          android:bottomLeftRadius="10dp"
          android:bottomRightRadius="10dp" />
</shape>
```

この描画可能なリソースでは、レイアウトやその他のディスプレイのように、次の XML に示すように宣言を参照できます。

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:background="#33000000">
    <TextView android:layout_width="wrap_content"
              android:layout_height="wrap_content"
              android:layout_centerInParent="true"
              android:background="@drawable/shape_rounded_blue_rect"
              android:text="@string/message_shapedrawable" />
</RelativeLayout>
```

描画可能なリソースは、プログラムでも適用できます。 次のコード スニペットでは、プログラムで、TextView の背景を設定する方法を示します。

```csharp
TextView tv = FindViewById<TextView>(Resource.Id.shapeDrawableTextView);
tv.SetBackgroundResource(Resource.Drawable.shape_rounded_blue_rect);
```

これがどのような確認、実行、 *AnimationsDemo*プロジェクトし、メイン メニューから、図形の描画可能な項目を選択します。 次のスクリーン ショットのような画面が表示されます。

![角が丸い、グラデーションの描画可能なカスタム背景を持つ Textview](graphics-and-animation-images/image1.png)

XML 要素と描画可能なリソースの構文の詳細についてを参照してください。 [Google のドキュメント](http://developer.android.com/guide/topics/resources/drawable-resource.html#Shape)します。


### <a name="using-the-canvas-drawing-api"></a>キャンバス描画 API を使用します。

ドローアブルは強力ですが、制限があります。 特定の情報は不可能または非常に複雑な (例: デバイス上のカメラで撮影する画像にフィルターを適用)。 描画可能なリソースを使用して、赤目減少を適用する非常に困難になります。
代わりに、Canvas API では、画像の特定の部分の色を選択的に変更するを非常に細かく制御するアプリケーション。

キャンバスでよく使用される 1 つのクラスは、[ペイント](https://developer.xamarin.com/api/type/Android.Graphics.Paint/)クラス。 このクラスは、描画する方法についての色とスタイルの情報を保持します。 このような色と透明度モ ノの提供に使用されます。

キャンバスの API を使用して、*ペインタ モデル*2D グラフィックスを描画するためにします。
操作は、相互に連続したレイヤーに適用されます。 各操作では、基になるビットマップのいくつかの領域を説明します。 領域には、以前に塗りつぶされる領域が重なっていると、新しい描画は部分的にまたは、古いが完全に不明確です。 これは、System.Drawing、および iOS のコア グラフィックスなどその他の多くの描画 Api の動作と同じ方法です。

取得する 2 つの方法がある、`Canvas`オブジェクト。 最初の方法では、定義、[ビットマップ](https://developer.xamarin.com/api/type/Android.Graphics.Bitmap/)オブジェクト、およびインスタンス化し、`Canvas`られてオブジェクト。 たとえば、次のコード スニペットでは、基になるビットマップで新しいキャンバスを作成します。

```csharp
Bitmap bitmap = Bitmap.CreateBitmap(100, 100, Bitmap.Config.Argb8888);
Canvas canvas = new Canvas(b);
```

取得するその他の方法、`Canvas`し、オブジェクトが、 [OnDraw](https://developer.xamarin.com/api/member/Android.Views.View.OnDraw/)から提供されるコールバック メソッド、[ビュー](https://developer.xamarin.com/api/type/Android.Views.View/)基本クラス。 Android は、ビュー自体を描画する必要がありますを渡しますを決めるときにこのメソッドを呼び出して、`Canvas`を使用するビューのオブジェクト。

キャンバスのクラスは、プログラムで描画の指示を提供するメソッドを公開します。 例えば:

-   [Canvas.DrawPaint](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawPaint/p/Android.Graphics.Paint/) &ndash;キャンバス全体のビットマップを指定の描画で塗りつぶします。

-   [Canvas.DrawPath](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawPath/p/Android.Graphics.Path/Android.Graphics.Paint/) &ndash;指定のペイントを使用して、指定の幾何学図形を描画します。

-   [Canvas.DrawText](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawText/p/System.String/System.Single/System.Single/Android.Graphics.Paint/) &ndash;で、指定したキャンバス上のテキストを描画します。 位置にテキストが描画された`x,y`します。



#### <a name="drawing-with-the-canvas-api"></a>Canvas API による描画

キャンバスの API の動作の例を見てみましょう。 次のコード スニペットでは、ビューを描画する方法を示します。

```csharp
public class MyView : View
{
    protected override void OnDraw(Canvas canvas)
    {
        base.OnDraw(canvas);
        Paint green = new Paint {
            AntiAlias = true,
            Color = Color.Rgb(0x99, 0xcc, 0),
        };
        green.SetStyle(Paint.Style.FillAndStroke);

        Paint red = new Paint {
            AntiAlias = true,
            Color = Color.Rgb(0xff, 0x44, 0x44)
        };
        red.SetStyle(Paint.Style.FillAndStroke);

        float middle = canvas.Width * 0.25f;
        canvas.DrawPaint(red);
        canvas.DrawRect(0, 0, middle, canvas.Height, green);
    }
}
```

このコードでは、まず、赤いペイントと緑の描画オブジェクトを作成します。 赤、キャンバスのコンテンツを設定し、キャンバスの幅の 25% である緑の四角形を描画するために、キャンバスに指示します。 この例で確認できます`AnimationsDemo`この記事のソース コードに含まれているプロジェクト。 アプリケーションの起動、メイン メニューから描画項目を選択するは、次のような画面が必要です。

![ペイントの赤と緑の描画オブジェクトを画面します。](graphics-and-animation-images/image3.png)


## <a name="animation"></a>アニメーション

ユーザーはなど、アプリケーションに移動することです。 アニメーションは、アプリケーションのユーザー エクスペリエンスを改善し、目立たせてする優れた方法です。最適なアニメーションとは、自然に感じているので、ユーザーを確認しないものです。 Android では、アニメーションの次の 3 つ API を提供します。

-   **アニメーション表示**&ndash;これは元の API です。 これらのアニメーションは、特定のビューに関連付けられているし、ビューの内容に単純な変換を実行することができます。 単純さのためこの API の場合にも役立ちますなどのアルファ アニメーション、回転、およびなど。

-   **プロパティ アニメーション** &ndash; Android 3.0 で導入されたプロパティのアニメーション。 これらは、ほとんどすべてのものをアニメーション化するアプリケーションを有効にします。 そのオブジェクトは、画面に表示されていない場合でも、任意のオブジェクトのプロパティを変更するプロパティのアニメーションを使用できます。

-   **Drawable アニメーション**&ndash;このレイアウトに非常に単純なアニメーションを適用するために使用される特殊な描画可能なリソースに影響します。

一般に、プロパティのアニメーションは、柔軟性が高くより多くの機能を提供し、使用する推奨されるシステムです。


### <a name="view-animations"></a>ビューのアニメーション

ビューのアニメーションをビューに限定されており、開始と終了ポイント、サイズ、回転、および透明度などの値でのみアニメーションを実行できます。
これらの種類のアニメーションがすると通常呼ばれる*トゥイーン アニメーション*します。 ビューのアニメーションを定義できます 2 つの方法&ndash;コードまたは XML ファイルを使用してプログラムを使用します。 XML ファイルは、読みやすく、保守が簡単とビューのアニメーションを宣言することをお勧めです。

アニメーションの XML ファイルを保管、 `/Resources/anim` Xamarin.Android プロジェクトのディレクトリ。 このファイルは、ルート要素として、次の要素のいずれかで必要です。

-   `alpha` &ndash; フェードインまたはフェードアウト アニメーション。

-   `rotate` &ndash; 回転アニメーション。

-   `scale` &ndash; サイズ変更のアニメーション。

-   `translate` &ndash; 水平方向と垂直方向の動きです。

-   `set` &ndash; その他のアニメーション要素の 1 つ以上保持するコンテナー。

既定では、XML ファイル内のすべてのアニメーションを同時に適用されます。 アニメーションを順番に実行するために、設定、`android:startOffset`上記で定義された要素の 1 つの属性。

使用してアニメーション内の変化率に影響することができます、*インターポレーター*します。 Interpolator を使うと、アニメーション効果を高速、繰り返し、または decelerated します。 Android のフレームワークは、(ただしこれらに限りません) などの特別ないくつか用意されているインターポレーターを提供します。

-   `AccelerateInterpolator` / `DecelerateInterpolator` &ndash; これらのインターポレーターを増やしたり減らしたりのアニメーションで変化率。

-   `BounceInterpolator` &ndash; 最後に変更が bounces します。

-   `LinearInterpolator` &ndash; 変化の速度は定数です。


次の XML では、これらの要素を結合するアニメーション ファイルの例を示します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android=http://schemas.android.com/apk/res/android
     android:shareInterpolator="false">

    <scale android:interpolator="@android:anim/accelerate_decelerate_interpolator"
           android:fromXScale="1.0"
           android:toXScale="1.4"
           android:fromYScale="1.0"
           android:toYScale="0.6"
           android:pivotX="50%"
           android:pivotY="50%"
           android:fillEnabled="true"
           android:fillAfter="false"
           android:duration="700" />

    <set android:interpolator="@android:anim/accelerate_interpolator">
        <scale android:fromXScale="1.4"
               android:toXScale="0.0"
               android:fromYScale="0.6"
               android:toYScale="0.0"
               android:pivotX="50%"
               android:pivotY="50%"
               android:fillEnabled="true"
               android:fillBefore="false"
               android:fillAfter="true"
               android:startOffset="700"
               android:duration="400" />

        <rotate android:fromDegrees="0"
                android:toDegrees="-45"
                android:toYScale="0.0"
                android:pivotX="50%"
                android:pivotY="50%"
                android:fillEnabled="true"
                android:fillBefore="false"
                android:fillAfter="true"
                android:startOffset="700"
                android:duration="400" />
    </set>
</set>
```

このアニメーションはすべてのアニメーションを同時に実行します。 最初のスケールのアニメーションは、イメージを水平方向に拡張され、垂直方向に縮小し、イメージは同時に 45 度反時計回りに回転させるよう縮小、画面から消えます。

アニメーションは、ビューでプログラムでアニメーションを膨張させることと、そのビューに適用することによって適用できます。 Android は、ヘルパー クラスを提供します。`Android.Views.Animations.AnimationUtils`をは、アニメーションのリソースを展開し、のインスタンスを返す`Android.Views.Animations.Animation`します。 このオブジェクトが呼び出すことによって、ビューに適用されます`StartAnimation`を渡して、`Animation`オブジェクト。 次のコード スニペットでは、この例を示します。

```csharp
Animation myAnimation = AnimationUtils.LoadAnimation(Resource.Animation.MyAnimation);
ImageView myImage = FindViewById<ImageView>(Resource.Id.imageView1);
myImage.StartAnimation(myAnimation);
```

ビューのアニメーションの動作の基本について理解したら、プロパティのアニメーションを移動することができます。


### <a name="property-animations"></a>プロパティのアニメーション

アニメーターのプロパティは、Android 3.0 で導入された新しい API です。
任意のオブジェクトの任意のプロパティをアニメーション化するために使用する拡張可能な API を提供します。

すべてのプロパティのアニメーションがのインスタンスによって作成された、 [Animator](https://developer.xamarin.com/api/type/Android.Animation.Animator/)サブクラスです。 サブクラスのいずれかを使用する代わりには、アプリケーションはこのクラスを直接使用されません。

-   [ValueAnimator](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/) &ndash;このクラスは、全体のプロパティのアニメーション API で最も重要なクラスです。 変更する必要があるプロパティの値を計算します。 `ViewAnimator`それらの値を直接更新できません代わりに、アニメーション化されたオブジェクトを更新するために使用できるイベントが発生します。

-   [ObjectAnimator](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/) &ndash;このクラスのサブクラスは、`ValueAnimator`します。 対象オブジェクトおよび更新するプロパティをそのまま使用してオブジェクトをアニメーション処理のプロセスを簡略化するものです。

-   [AnimationSet](https://developer.xamarin.com/api/type/Android.Animation.AnimatorSet/) &ndash;このクラスは、リレーションシップで相互にアニメーションの実行を調整することを担当します。 アニメーションは、同時に、順番に、または指定された時間とそれらの間を実行できます。


*評価者*アニメーターでアニメーションの中に新しい値の計算に使用される特殊なクラスです。 既定では、Android は、次のエバリュエータを提供します。

-   [IntEvaluator](https://developer.xamarin.com/api/type/Android.Animation.IntEvaluator/) &ndash;整数プロパティの値を計算します。

-   [FloatEvaluator](https://developer.xamarin.com/api/type/Android.Animation.FloatEvaluator/) &ndash; float プロパティの値を計算します。

-   [ArgbEvaluator](https://developer.xamarin.com/api/type/Android.Animation.ArgbEvaluator/) &ndash;カラー プロパティの値を計算します。

アニメーション化されているプロパティがない場合、 `float`、`int`カラー、アプリケーション作成することも、独自のエバリュエーターの実装によって、`ITypeEvaluator`インターフェイス。 (カスタム エバリュエーターを実装する、このトピックの範囲を超えては) です。

#### <a name="using-the-valueanimator"></a>ValueAnimator を使用します。

2 つの部分をすべてのアニメーション: アニメーション化された値を計算し、いくつかのオブジェクトのプロパティでこれらの値を設定します。 
[ValueAnimator](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/)は、値の計算のみが直接オブジェクトで動作するがないこと。 代わりに、アニメーションの有効期間中に呼び出されるイベント ハンドラー内でオブジェクトが更新されます。 この設計では、1 つのアニメーション化された値から更新するいくつかのプロパティを使用します。

インスタンスを取得する`ValueAnimator`ファクトリ メソッドは、次のいずれかを呼び出します。

-  `ValueAnimator.OfInt`
-  `ValueAnimator.OfFloat`
-  `ValueAnimator.OfObject`

1 回は元に戻す、`ValueAnimator`インスタンスが必要その継続時間は、次の設定、および開始します。 次の例では、1000 ミリ秒にわたって 0 ~ 1 の値をアニメーション化する方法を示します。

```csharp
ValueAnimator animator = ValueAnimator.OfInt(0, 100);
animator.SetDuration(1000);
animator.Start();
```

自体は、上記のコード スニペットは非常に役に立ちませんが、&ndash;アニメーターが実行されますが、ターゲットの更新後の値はありません。 `Animator`クラスの新しい値のリスナーに通知する必要があることに決めた場合は、更新イベントが発生します。 アプリケーションでは、次のコード スニペットに示すように、このイベントに応答するイベント ハンドラーを提供できます。

```csharp
MyCustomObject myObj = new MyCustomObject();
myObj.SomeIntegerValue = -1;

animator.Update += (object sender, ValueAnimator.AnimatorUpdateEventArgs e) =>
{
    int newValue = (int) e.Animation.AnimatedValue;
    // Apply this new value to the object being animated.
    myObj.SomeIntegerValue = newValue;
};
```

理解できた`ValueAnimator`、により、詳細については、`ObjectAnimator`します。

#### <a name="using-the-objectanimator"></a>ObjectAnimator を使用します。

[ObjectAnimator](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/)のサブクラスは、`ViewAnimator`のタイミング エンジンと値の計算を結合する、`ValueAnimator`イベント ハンドラーを接続するために必要なロジックを使用します。 `ValueAnimator`アプリケーション イベント ハンドラーを明示的に接続する必要があります&ndash;`ObjectAnimator`がのこの手順の処理します。

用の API`ObjectAnimator`用 API によく似ています`ViewAnimator`オブジェクトと、更新するプロパティの名前を指定することが必要です。 次の例を使用する例を示しています`ObjectAnimator`:。

```csharp
MyCustomObject myObj = new MyCustomObject();
myObj.SomeIntegerValue = -1;

ObjectAnimator animator = ObjectAnimator.OfFloat(myObj, "SomeIntegerValue", 0, 100);
animator.SetDuration(1000);
animator.Start();
```

上記のコード スニペットからわかるように`ObjectAnimator`を削減し、オブジェクトをアニメーション化するために必要なコードを簡略化できます。


### <a name="drawable-animations"></a>描画可能なアニメーション

最終的なアニメーション API は、Drawable アニメーション API です。 描画可能なアニメーションが後に、その他の一連の描画可能なリソースの 1 つを読み込むし、それらを順番に表示、フリップ it マンガに似ています。

描画可能なリソースを含む XML ファイルで定義されます、`<animation-list>`ルート要素と、一連の要素`<item>`アニメーション内の各フレームを定義する要素。 この XML ファイルに格納されます、`/Resource/drawable`アプリケーションのフォルダー。 次の XML では、drawable アニメーションの例を示します。

```xml
<animation-list xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:drawable="@drawable/asteroid01" android:duration="100" />
  <item android:drawable="@drawable/asteroid02" android:duration="100" />
  <item android:drawable="@drawable/asteroid03" android:duration="100" />
  <item android:drawable="@drawable/asteroid04" android:duration="100" />
  <item android:drawable="@drawable/asteroid05" android:duration="100" />
  <item android:drawable="@drawable/asteroid06" android:duration="100" />
</animation-list>
```

このアニメーションは、6 つのフレームで実行されます。 `android:duration`属性は各フレームの表示時間を宣言します。 次のコード スニペットでは、描画可能なアニメーションを作成して、ユーザーが画面上のボタンをクリックしたときに開始することの例を示します。

```csharp
AnimationDrawable _asteroidDrawable;

protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    _asteroidDrawable = (Android.Graphics.Drawables.AnimationDrawable)
    Resources.GetDrawable(Resource.Drawable.spinning_asteroid);

    ImageView asteroidImage = FindViewById<ImageView>(Resource.Id.imageView2);
    asteroidImage.SetImageDrawable((Android.Graphics.Drawables.Drawable) _asteroidDrawable);

    Button asteroidButton = FindViewById<Button>(Resource.Id.spinAsteroid);
    asteroidButton.Click += (sender, e) =>
    {
        _asteroidDrawable.Start();
    };
}
```

この時点で、Android アプリケーションで使用可能な Api のアニメーションの基礎について説明しました。


## <a name="summary"></a>まとめ

この記事では、多くの新しい概念を導入して、一部のグラフィックスを Android アプリケーションに追加するための API。 まず、さまざまな 2D グラフィックス API の説明し、Android がキャンバス オブジェクトを使用して画面に直接描画するためにアプリケーションを使用する方法について説明します。 XML ファイルを使用して宣言によって作成するグラフィックスを許可するいくつかの代替方法を説明しました。 に移動、新旧の API の Android でのアニメーションの作成について説明します。



## <a name="related-links"></a>関連リンク

- [アニメーションのデモ (サンプル)](https://developer.xamarin.com/samples/monodroid/AnimationDemo)
- [アニメーションとグラフィックス](http://developer.android.com/guide/topics/graphics/index.html)
- [モバイル アプリに命をアニメーションを使用してください。](http://youtu.be/ikSk_ILg3d0)
- [AnimationDrawable](https://developer.xamarin.com/api/type/Android.Graphics.Drawables.AnimationDrawable/)
- [Canvas](https://developer.xamarin.com/api/type/Android.Graphics.Canvas/)
- [オブジェクトの Animator](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/)
- [値 Animator](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/)
