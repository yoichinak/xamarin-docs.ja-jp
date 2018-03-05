---
title: "グラフィックスおよびアニメーション"
description: "Android では、2 次元グラフィックとアニメーションをサポートするため、非常に豊富なさまざまなフレームワークを提供します。 このトピックでは、これらのフレームワークを紹介し、Xamarin.Android アプリケーションでカスタム グラフィックと使用するためのアニメーションを作成する方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 80086318-6FE4-4711-9A71-5C8F8C28C754
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 90a2eb219ae1189e7a48e60cde9761e3e9e93e0b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="graphics-and-animation"></a>グラフィックスおよびアニメーション

_Android では、2 次元グラフィックとアニメーションをサポートするため、非常に豊富なさまざまなフレームワークを提供します。このトピックでは、これらのフレームワークを紹介し、Xamarin.Android アプリケーションでカスタム グラフィックと使用するためのアニメーションを作成する方法について説明します。_

<a name="Overview" />

## <a name="overview"></a>概要

にもかかわらずされる従来の限られた電源デバイスで実行されている、最高の評価済みのモバイル アプリケーション多くの場合がある高度なユーザー エクスペリエンス (UX)、高品質のグラフィックと、直感的な応答性の高い、動的フィールを提供するアニメーションに完了しません。 モバイル アプリケーションの取得の詳細より高度なユーザーが開始されてより多くのアプリケーションから期待できることです。

幸運にも、最新のモバイル プラットフォームでは、使いやすさを維持しながら高度なアニメーションやカスタムのグラフィックスを作成するための非常に強力なフレームワークにあります。 これにより、開発者はごくわずかな労力で充実した対話機能を追加できます。

Android API の UI フレームワークは、2 つのカテゴリにほぼ分割できます。 グラフィックスおよびアニメーション。

グラフィックスは、2D および 3D グラフィックスを行うためのさまざまなアプローチにさらに分割されます。 3D グラフィックスは、組み込み OpenGL ES (モバイル特定のバージョンの OpenGL) などのフレームワークと MonoGame (XNA ツールキットを使用した互換性のあるクロス プラットフォームのツールキット) などのサードパーティ フレームワークの数で使用できます。 3D グラフィックスが、この記事の内容のスコープ内にありませんが、組み込みの 2D の描画方法を調べます。

Android では、2 D グラフィックスの作成の 2 つの異なる API を提供します。 1 つは、高レベルの宣言型方法と、その他のプログラムによる低水準の API には。

-   **ドロウアブル リソース**&ndash;これら描画命令を XML ファイルに埋め込むことで、カスタム グラフィックスを (通常) またはプログラムで作成に使用されます。 ドロウアブル リソースは、通常の指示または 2D グラフィックを表示するために Android 用のアクションを含む XML ファイルとして定義されます。 

-   **キャンバス**&ndash;これは、基になるビットマップで直接描画を含む低レベルの API です。 表示される内容を非常に詳細に制御を提供します。 

Android には、これらの 2 次元グラフィックス手法に加えて、アニメーションを作成するいくつかの方法も提供されます。

-   **ドロウアブル アニメーション** &ndash; Android は、フレームのアニメーションと呼ばれるもサポートしています。*ドロウアブル アニメーション*です。 これは、最も単純なアニメーション API です。 Android 順番に読み込んで (と同様にマンガ) シーケンスのドロウアブル リソースを表示します。

-   **アニメーションを表示** &ndash; *ビュー アニメーション*Android では、元のアニメーションの API は、Android のすべてのバージョンで使用可能な。 この API はビュー オブジェクトでのみ機能し、それらのビューを単純な変換を実行できるのみに制限されます。
    ビューのアニメーションが通常内に XML ファイルで定義されている、`/Resources/anim`フォルダーです。

-   **プロパティのアニメーション** &ndash; Android 3.0 が導入され、新しいアニメーションの API のセットと呼ばれる*プロパティ アニメーション*です。 これらの新しい API には、任意のオブジェクトのプロパティをアニメーション化、だけでなくオブジェクトの表示に使用できる、拡張性と柔軟なシステムが導入されました。 このような柔軟性により、コードの共有を容易にすると、個別のクラスにカプセル化するアニメーションです。


ビューのアニメーションは、古い前 Android 3.0 API の (API レベル 11) をサポートする必要があるアプリケーションのより適しています。 それ以外の場合アプリケーションが上記の理由により新しいのプロパティのアニメーション API を使用する必要があります。

これらのフレームワークのすべてが実行可能なオプションは、ただし可能であれば、基本設定する必要がありますを指定するプロパティのアニメーションにはより柔軟な API を使用するとします。 プロパティのアニメーションは、コードを簡単に共有を行い、コードの保守の簡略化する個別のクラスにカプセル化のアニメーション ロジックに対して許可します。


## <a name="accessibility"></a>ユーザー補助

グラフィックスおよびアニメーション魅力的な Android アプリ、楽しく; を使用するにはただし、いくつかの相互作用が screenreaders、代替入力デバイス、または支援ズームを実行することに注意してくださいです。
また、オーディオ機能を使用しないいくつかの相互作用が発生する可能性があります。

注意のアクセシビリティを持つが設計されている場合は、アプリはこのような状況でより使いやすく: ヒントと、ユーザー インターフェイスでナビゲーション アシスタンスを提供し、テキスト コンテンツまたは UI の視覚要素の説明があることを確認します。

参照してください[Google のユーザー補助ガイド](http://developer.android.com/guide/topics/ui/accessibility/)Android のユーザー補助機能の Api を利用する方法についての詳細。


<a name="2D_Graphics" />

## <a name="2d-graphics"></a>2D グラフィック

ドロウアブル リソースは、Android のアプリケーションでよく使用される手法です。 同様に、その他のリソース ドロウアブル リソース宣言&ndash;XML ファイルで定義されている場合、します。 この方法は、リソースからコードを明確に分離できます。 これによって簡単に開発と保守を更新または Android アプリケーションでグラフィックスを変更するためのコードを変更する必要はないためです。 ただし、ドロウアブル リソースは多数の単純で一般的なグラフィック要件に役立ちますが、それらが不足している電源とキャンバス API のコントロールです。

その他の手法を使用して、[キャンバス](https://developer.xamarin.com/api/type/Android.Graphics.Canvas/)オブジェクト、System.Drawing または iOS の中核となる図形など他の従来の API フレームワークによく似ています。 キャンバスのオブジェクトを使用してどのように 2 D グラフィックスのほとんどのコントロールが作成されたを提供します。 ドロウアブル リソースが動作しないかを使用するが難しくなりますの状況に適しています。 たとえば、カスタムのスライダー コントロールの外観が変更されますスライダーの値に関連する計算に基づいてを描画するために必要な場合があります。

最初にドロウアブル リソースを調べてみましょう。 単純なされ、最も一般的なカスタム描画ケースに対応します。

<a name="Drawable Resources" />

### <a name="drawable-resources"></a>ドロウアブル リソース

ドロウアブル リソースがディレクトリ内の XML ファイルで定義されている`/Resources/drawable`です。 PNG、または JPEG の埋め込みとは異なり、ドロウアブル リソースの密度固有のバージョンを提供する必要はありません。
実行時に、Android アプリケーションはこれらのリソースを読み込むし、2 次元グラフィックスを作成するような XML ファイルに含まれている手順を使用します。
Android では、いくつかの異なる種類のドロウアブル リソースを定義します。

-   [ShapeDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Shape) &ndash;プリミティブ幾何学図形を描画し、その図形上のグラフィック効果の制限されたセットを適用するドロウアブル オブジェクトです。 ボタンをカスタマイズまたはするテキスト ビューの背景を設定するなどの非常に便利です。 使用する方法の例が表示されます、`ShapeDrawable`この記事で後述します。

-   [StateListDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#StateList) &ndash;ウィジェット/コントロールの状態に基づいて外観を変更するドロウアブル リソースです。 たとえば、ボタンはかどうかが押されたかに応じて、外観を変更できます。

-   [LayerDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList) &ndash;このドロウアブル リソースに別の上に 1 つの他のいくつかのドロウアブルをスタックします。 例、 *LayerDrawable*次のスクリーン ショットに表示されます。

    ![LayerDrawable 例](graphics-and-animation-images/image1.png)

-   [TransitionDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Transition) &ndash;これは、 *LayerDrawable*が 1 つの違い。 A *TransitionDrawable*上示す別の 1 つのレイヤーをアニメーション化することができます。

-   [LevelListDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#LevelList) &ndash;に非常に似ています、 *StateListDrawable*点で、特定の条件に基づくイメージが表示されます。 ただしとは異なり、 *StateListDrawable*、 *LevelListDrawable*整数値に基づくイメージが表示されます。 例、 *LevelListDrawable* WiFi 信号の強度を表示することです。 WiFi 信号変更の強度、として表示されるドロウアブルを変更します。

-   [ScaleDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Scale)/[ClipDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Clip) &ndash;これらドロウアブル提供スケーリングと機能の領域の両方でその名前からわかるように、します。 *ScaleDrawable*ドロウアブルいるときは拡大縮小、 *ClipDrawable*別のディスプレイをクリップします。

-   [InsetDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Inset) &ndash;このドロウアブル適用くぼみドロウアブル リソースを別の側面にします。 これは、ような状況は、ビューには、ビューの実際の範囲よりも小さい、背景が必要がある場合に使用されます。

-   XML [BitmapDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Bitmap) &ndash;このファイルは、一連の手順については、XML では、実際のビットマップを実行することです。 Android を実行できる一部の操作とは、タイル、ディザリング、およびアンチ エイリアスです。 レイアウトの背景にビットマップを並べて表示すると非常に一般的な用途の 1 つです。


#### <a name="drawable-example"></a>ドロウアブル例

2D グラフィックを使用して、作成する方法の簡単な例を見てみましょう、`ShapeDrawable`です。 A `ShapeDrawable` 4 つの基本的な図形のいずれかを定義できます。 四角形、楕円、線、およびリングします。 グラデーション、色、サイズなどの基本的な効果を適用することもできます。 次の XML は、`ShapeDrawable`を参照してください、 *AnimationsDemo*コンパニオン プロジェクト (ファイルで`Resources/drawable/shape_rounded_blue_rect.xml`)。
紫のグラデーションの背景を含む四角形を定義し、角を丸きます。

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

レイアウトやその他のディスプレイの次の XML に示すように宣言によってドロウアブルこのリソースは参照できます。

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

ドロウアブル リソースは、プログラムでも適用できます。 次のコード スニペットは、プログラムによって、TextView の背景を設定する方法を示しています。

```csharp
TextView tv = FindViewById<TextView>(Resource.Id.shapeDrawableTextView);
tv.SetBackgroundResource(Resource.Drawable.shape_rounded_blue_rect);
```

表示するようになりますこの新機能を実行、 *AnimationsDemo*プロジェクトし、メイン メニューから図形ドロウアブル項目を選択します。 次のスクリーン ショットに類似したお表示されます。

![カスタム背景では、角が丸い、グラデーション ドロウアブル Textview](graphics-and-animation-images/image1.png)

XML 要素とドロウアブル リソースの構文の詳細についてを参照してください[Google のドキュメント](http://developer.android.com/guide/topics/resources/drawable-resource.html#Shape)です。

<a name="Using the Canvas Drawing API" />

### <a name="using-the-canvas-drawing-api"></a>キャンバスの描画 API を使用します。

ドロウアブルは強力な制限があります。 一定の処理は実行不可能または非常に複雑な (例: デバイスのカメラで撮影する画像にフィルターを適用)。 ドロウアブル リソースを使用して、赤目減少を適用する非常に困難になります。
代わりに、キャンバス API は、アプリケーションを選択的に、画像の特定の部分の色を変更する非常に細かい制御をできます。

キャンバスで一般的に使用される 1 つのクラスは、[ペイント](https://developer.xamarin.com/api/type/Android.Graphics.Paint/)クラスです。 このクラスは、描画する方法の色とスタイル情報を保持します。 このような色と透明度の処理を提供する使用されます。

キャンバスの API を使用して、*画家のモデル*2D グラフィックスを描画します。
操作は、互いの上に連続する層に適用されます。 各操作では、基になるビットマップのいくつかの領域を説明します。 領域には、以前に塗りつぶされる領域が重なっていると、新しいペイントは部分的にまたは、完全に古いが見えにくきます。 これは、System.Drawing、および iOS のコア グラフィックなどの他の描画 Api の多くの作業のと同じ方法です。

取得する 2 つの方法、`Canvas`オブジェクト。 最初の方法では、定義、[ビットマップ](https://developer.xamarin.com/api/type/Android.Graphics.Bitmap/)オブジェクト、およびインスタンス化し、`Canvas`それを持つオブジェクト。 たとえば、次のコード スニペットとビットマップを基になる新しいキャンバスを作成します。

```csharp
Bitmap bitmap = Bitmap.CreateBitmap(100, 100, Bitmap.Config.Argb8888);
Canvas canvas = new Canvas(b);
```

取得するその他の方法、`Canvas`し、オブジェクトが、 [OnDraw](https://developer.xamarin.com/api/member/Android.Views.View.OnDraw/)から提供されるコールバック メソッド、[ビュー](https://developer.xamarin.com/api/type/Android.Views.View/)基本クラスです。 Android は、ビューを選択し、自身で描画する必要がありますで渡しますを決めるときにこのメソッドを呼び出して、`Canvas`を使用するビューのオブジェクト。

キャンバス クラスでは、プログラムで描画の指示を提供するメソッドを公開します。 例:

-   [Canvas.DrawPaint](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawPaint/p/Android.Graphics.Paint/) &ndash;キャンバス全体のビットマップを指定したペイントで塗りつぶします。

-   [Canvas.DrawPath](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawPath/p/Android.Graphics.Path/Android.Graphics.Paint/) &ndash;指定のペイントを使用して指定の幾何学図形を描画します。

-   [Canvas.DrawText](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawText/p/System.String/System.Single/System.Single/Android.Graphics.Paint/) &ndash;指定された色と、キャンバス上のテキストを描画します。 位置にテキストが描画された`x,y`です。


<a name="Drawing with the Canvas API" />

#### <a name="drawing-with-the-canvas-api"></a>キャンバスの API を使用した描画

アクションでキャンバス API の使用例を見てみましょう。 次のコード スニペットは、ビューを描画する方法を示しています。

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

上記のコードではこのは、まず、赤色の塗料と緑色の塗料オブジェクトを作成します。 赤、キャンバスのコンテンツで塗りつぶしたし、キャンバスの幅の 25% は、緑色の四角形を描画するキャンバスを指示します。 この例でわかるようにして`AnimationsDemo`このアーティクルのソース コードに含まれているプロジェクト。 アプリケーションを開始して、メイン メニューから描画項目を選択すると、次のような画面が行ってください。

![画面では、赤色の塗料と緑色の塗料オブジェクト](graphics-and-animation-images/image3.png)

<a name="Animation" />

## <a name="animation"></a>アニメーション

ユーザーなど、アプリケーション内で移動することができます。 アニメーションは、アプリケーションのユーザー エクスペリエンスを向上させるし、ヘルプを目立たせるする優れた方法です。最適なアニメーション自然と感じているために、ユーザーしないに注意してくださいれるものであります。 Android では、アニメーションの次の 3 つ API を提供します。

-   **アニメーション表示**&ndash;これは、元の API です。 これらのアニメーションでは、特定のビューに関連付けられてされ、ビューの内容に単純な変換を実行できます。 簡潔さのため、この API を目的とする場合にも役立ちますはアルファ アニメーション、回転、およびなどと同様にします。

-   **プロパティのアニメーション**&ndash;プロパティのアニメーションは Android 3.0 で導入されました。 これらには、ほぼすべての内容をアニメーション化するアプリケーションが有効にします。 そのオブジェクトは、画面に表示されていない場合でも、任意のオブジェクトのプロパティを変更するプロパティのアニメーションを使用できます。

-   **ドロウアブル アニメーション**&ndash;これを非常に単純なアニメーションを適用するために使用される特殊なドロウアブル リソースはレイアウトに影響します。

一般に、アニメーションのプロパティは、推奨されるシステムの方が柔軟性とより多くの機能を提供しています。 使用します。

<a name="View Animations" />

### <a name="view-animations"></a>ビューのアニメーション

ビューのアニメーションはビューに限定されており、開始と終了ポイント、サイズ、回転、および透明度などの値をアニメーションを実行できるのみ。
アニメーションのこれらの型と呼ばれる通常*トゥイーン アニメーション*です。 ビューのアニメーションを定義できます 2 つの方法&ndash;コードでプログラムによって、または XML ファイルを使用しています。 XML ファイルは、読みやすく理解し管理が容易とビューのアニメーションを宣言することをお勧めです。

アニメーションの XML ファイルに保存する、 `/Resources/anim` Xamarin.Android プロジェクトのディレクトリ。 このファイルは、ルート要素として、次の要素のいずれかが必要です。

-   `alpha` &ndash; フェードインまたはフェードアウト アニメーション。

-   `rotate` &ndash; 回転アニメーション。

-   `scale` &ndash; サイズ変更アニメーション。

-   `translate` &ndash; 水平/垂直方向の動きです。

-   `set` &ndash; アニメーションの他の要素の 1 つ以上を保持するコンテナー。

既定では、XML ファイル内のすべてのアニメーションを同時に適用されます。 アニメーションが順番に実行するために、設定、`android:startOffset`上記で定義された要素の 1 つの属性です。

使用してアニメーションの変化率に影響することは、*インターポレータ*です。 Interpolator により高速化、繰り返し、または decelerated アニメーション効果。 Android のフレームワークでは、(ただしこれらに限定されません) など、既定でいくつかの補間を提供します。

-   `AccelerateInterpolator` / `DecelerateInterpolator` &ndash; これらの補間を増減させて、アニメーションの変化率。

-   `BounceInterpolator` &ndash; 変更は、最後に bounces です。

-   `LinearInterpolator` &ndash; 変更の速度は定数です。


次の XML は、これらの要素を結合するアニメーション ファイルの例を示しています。

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

このアニメーションをアニメーションのすべてを同時に実行します。 最初の小数点以下桁数アニメーションは、イメージを水平方向に拡大され、垂直方向に縮小され、イメージは同時に 45 度反時計回りに回転させるようおよび縮小、画面から消失しています。

アニメーションは、ビューにでプログラムでアニメーションを膨張させると、ビューに適用することによって適用できます。 Android ヘルパー クラスを提供する`Android.Views.Animations.AnimationUtils`はアニメーション リソースを展開しのインスタンスを返すを`Android.Views.Animations.Animation`です。 このオブジェクトは呼び出すことによって、ビューに適用`StartAnimation`を渡すと、`Animation`オブジェクト。 次のコード スニペットは、この例を示しています。

```csharp
Animation myAnimation = AnimationUtils.LoadAnimation(Resource.Animation.MyAnimation);
ImageView myImage = FindViewById<ImageView>(Resource.Id.imageView1);
myImage.StartAnimation(myAnimation);
```

ビューのアニメーションのしくみの基本的な理解がある、プロパティのアニメーションを移動することができます。

<a name="Property Animations" />

### <a name="property-animations"></a>プロパティのアニメーション

プロパティ アニメーターは、Android 3.0 で導入された新しい API です。
任意のオブジェクトの任意のプロパティをアニメーション化するために使用する拡張可能な API を提供します。

すべてのプロパティのアニメーションがのインスタンスによって作成された、[アニメーター](https://developer.xamarin.com/api/type/Android.Animation.Animator/)サブクラスです。 アプリケーションがこのクラスを直接使用しないでください、使用する代わりにそのサブクラスのいずれか。

-   [ValueAnimator](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/) &ndash;このクラスは、全体のプロパティのアニメーション API で最も重要なクラスです。 変更する必要があるプロパティの値を計算します。 `ViewAnimator`それらの値を直接更新できません代わりに、アニメーション化されたオブジェクトを更新するために使用できるイベントを発生させます。

-   [ObjectAnimator](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/) &ndash;このクラスのサブクラスは、`ValueAnimator`です。 ターゲット オブジェクトとプロパティの更新をそのまま使用してアニメーション オブジェクトのプロセスを簡略化するものです。

-   [AnimationSet](https://developer.xamarin.com/api/type/Android.Animation.AnimatorSet/) &ndash;関係で相互にアニメーションの実行を調整するため、このクラスはします。 アニメーションでは、それらの間に同時に、順番に、または、指定した時間がかかるを実行可能性があります。


*評価者*アニメーターでアニメーションの中に、新しい値を計算するために使用する特殊なクラスです。 既定では、Android には、次の評価者が用意されています。

-   [IntEvaluator](https://developer.xamarin.com/api/type/Android.Animation.IntEvaluator/) &ndash;整数プロパティの値を計算します。

-   [FloatEvaluator](https://developer.xamarin.com/api/type/Android.Animation.FloatEvaluator/) &ndash; float のプロパティの値を計算します。

-   [ArgbEvaluator](https://developer.xamarin.com/api/type/Android.Animation.ArgbEvaluator/) &ndash;カラー プロパティの値を計算します。

アニメーション化されるプロパティがない場合、 `float`、`int`カラー、アプリケーション作成することも、独自のエバリュエーター実装することによって、`ITypeEvaluator`インターフェイスです。 (カスタム エバリュエーターを実装する、このトピックの範囲を超えては) です。

#### <a name="using-the-valueanimator"></a>使用して、ValueAnimator

すべてのアニメーションに 2 つの部分があります: アニメーション化された値を計算して、一部のオブジェクトのプロパティでそれらの値を設定します。 
[ValueAnimator](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/)のみ、値が計算されますが、直接オブジェクトに対して機能するがないことです。 代わりに、アニメーションの実行中に呼び出されるイベント ハンドラー内でオブジェクトが更新されます。 この設計では、1 つのアニメーション化された値から更新するいくつかのプロパティを使用します。

インスタンスを取得する`ValueAnimator`を呼び出して、次のファクトリ メソッドのいずれか。

-  `ValueAnimator.OfInt`
-  `ValueAnimator.OfFloat`
-  `ValueAnimator.OfObject`

元に戻す、`ValueAnimator`インスタンスが必要その継続時間は、次の設定し開始することができます。 次の例では、1000 ミリ秒の期間を 0 ~ 1 の値をアニメーション化する方法を示します。

```csharp
ValueAnimator animator = ValueAnimator.OfInt(0, 100);
animator.SetDuration(1000);
animator.Start();
```

自体は、上記のコード スニペットは非常に便利ですが、&ndash;アニメーターが実行されますが、更新された値のターゲットがありません。 `Animator`クラスの新しい値がリスナーに通知する必要があることに決めた場合は、更新イベントが発生します。 アプリケーションでは、次のコード スニペットで示すように、このイベントに応答するイベント ハンドラーを提供できます。

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

理解することができました`ValueAnimator`、により、詳細については、`ObjectAnimator`です。

#### <a name="using-the-objectanimator"></a>使用して、ObjectAnimator

[ObjectAnimator](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/)のサブクラスは、`ViewAnimator`のタイミングのエンジンと値の計算を結合する、`ValueAnimator`イベント ハンドラーを接続するために必要なロジックを使用します。 `ValueAnimator`イベント ハンドラーを明示的に接続するためのアプリケーションが必要です&ndash;`ObjectAnimator`うえでこの手順の注意されます。

用の API`ObjectAnimator`用の API によく似ています`ViewAnimator`、オブジェクトおよび更新するプロパティの名前を指定することが必要です。 次の例を使用する例を示しています`ObjectAnimator`:。

```csharp
MyCustomObject myObj = new MyCustomObject();
myObj.SomeIntegerValue = -1;

ObjectAnimator animator = ObjectAnimator.OfFloat(myObj, "SomeIntegerValue", 0, 100);
animator.SetDuration(1000);
animator.Start();
```

前のコード スニペットからわかるように`ObjectAnimator`を削減し、オブジェクトをアニメーション化するために必要なコードを簡略化できます。

<a name="Drawable Animations" />

### <a name="drawable-animations"></a>ドロウアブル アニメーション

最後のアニメーション API とは、ドロウアブル Animation API です。 ドロウアブル アニメーション ドロウアブル リソースの 1 つの系列、他の後に読み込んで表示して、順番にフリップ it マンガに似ています。

ドロウアブル リソースを持つ XML ファイルで定義されます、`<animation-list>`ルート要素と、一連の要素`<item>`アニメーションで各フレームを定義する要素。 この XML ファイルに保存、`/Resource/drawable`アプリケーションのフォルダーです。 次の XML では、ドロウアブル アニメーションの例を示します。

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

このアニメーションは、6 つのフレームで実行されます。 `android:duration`属性は、各フレームの表示期間を宣言します。 次のコード スニペットは、ドロウアブル アニメーションを作成して、ユーザーが画面上のボタンをクリックしたときに開始することの例を示します。

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

この時点で Api の Android アプリケーションで使用できるアニメーションの基礎を説明します。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事には多数の新しい概念が導入されましたし、API は、Android アプリケーションを一部のグラフィックスを追加するのです。 まず、さまざまな 2 D グラフィックスの API について説明し、Android により、アプリケーションは、キャンバスのオブジェクトを使用して、画面に直接描画する方法を説明します。 XML ファイルを使用して宣言して作成するグラフィックスを許可するいくつかの代替方法を説明しました。 もうしばらくお API について説明新旧の Android でのアニメーションを作成するためにします。



## <a name="related-links"></a>関連リンク

- [アニメーションのデモ (サンプル)](https://developer.xamarin.com/samples/monodroid/AnimationDemo)
- [アニメーションやグラフィック](http://developer.android.com/guide/topics/graphics/index.html)
- [モバイル アプリを現実に近づけるにアニメーションを使用します。](http://youtu.be/ikSk_ILg3d0)
- [AnimationDrawable](https://developer.xamarin.comhttps://developer.xamarin.com/api/type/Android.Graphics.Drawables.AnimationDrawable/)
- [Canvas](https://developer.xamarin.comhttps://developer.xamarin.com/api/type/Android.Graphics.Canvas/)
- [オブジェクトのアニメーター](https://developer.xamarin.comhttps://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/)
- [値アニメーター](https://developer.xamarin.comhttps://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/)
