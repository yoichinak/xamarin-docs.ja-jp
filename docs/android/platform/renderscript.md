---
title: Renderscript の概要
description: このガイドでは、Renderscript を紹介し、組み込み Renderscript API の Xamarin.Android アプリケーションでそのターゲット API レベル 17 以上の使用方法について説明します。
ms.prod: xamarin
ms.assetid: 378793C7-5E3E-40E6-ABEE-BEAEF64E6A47
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: f9e21a51c409c5444f137a63eb2c6fadfef03cbe
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30772017"
---
# <a name="an-introduction-to-renderscript"></a>Renderscript の概要

_このガイドでは、Renderscript を紹介し、組み込み Renderscript API の Xamarin.Android アプリケーションでそのターゲット API レベル 17 以上の使用方法について説明します。_

## <a name="overview"></a>概要

Renderscript は、広範なコンピューティング リソースを必要とする Android アプリケーションのパフォーマンスを向上させるためには Google によって作成されたプログラミング フレームワークです。 低レベルの高パフォーマンスに基づいて API が[C99](http://en.wikipedia.org/wiki/C99)です。 低レベルの Cpu、Gpu または Dsp で実行される API であるため Renderscript は Android アプリを次のいずれかを実行する必要がある場合に適してします。

* グラフィックス
* 画像処理
* 暗号化
* 信号処理
* 数値演算ルーチン

Renderscript が使用されます`clang`APK にバンドルされているある LLVM バイトのコードにスクリプトをコンパイルします。 実行すると、アプリは最初に、ある LLVM バイトのコードは、デバイス上のプロセッサのマシン語コードにコンパイルされます。 このアーキテクチャでは、プロセッサごとに、デバイスを自身で作成しなければならなくなってせず、開発者のマシン語コードの利点を利用する Android アプリケーションです。

これには、Renderscript ルーチンを 2 つのコンポーネントがあります。

1. **Renderscript ランタイム** &ndash; Renderscript を実行する責任は、ネイティブの Api です。 これには、アプリケーション用に記述された任意の Renderscripts が含まれます。

2. **Android のフレームワークからのマネージ ラッパー** &ndash;マネージ クラス、Android アプリを制御し、Renderscript ランタイムとスクリプトと対話できるようにします。 Renderscript ランタイムを制御するためのフレームワークが提供するクラス、に加えて Android ツール チェーンは Renderscript ソース コードを調べるし、Android アプリケーションで使用するためのマネージ ラッパー クラスを生成します。

次の図は、これらのコンポーネントとの関係を示しています。

![Android のフレームワークが Renderscript ランタイムとやり取りする方法を示すダイアグラム](renderscript-images/renderscript-01.png)

Android アプリケーションで Renderscripts を使用するための 3 つの重要な概念があります。

1. **コンテキスト**&ndash;マネージ Renderscript にリソースを割り当てるし、Android アプリを渡すし、Renderscript からデータを受信を許可する Android SDK が提供する API。

2. **A_カーネルをコンピューティング_** &ndash;とも呼ばれる、_ルート カーネル_または_カーネル_、この処理を行うルーチンです。 カーネルが C の関数とよく似ています。これは、割り当てられたメモリ内のすべてのデータ上で実行される並列ルーチンです。

3. **割り当てられたメモリ**&ndash;と経由のカーネル データが渡される、 _[割り当て](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/)_ です。 カーネルは、1 つの入力を必要がありますか、1 つの出力の割り当て。

[Android.Renderscripts](https://developer.xamarin.com/api/namespace/Android.Renderscripts/)名前空間には、Renderscript ランタイムとやり取りするためのクラスが含まれています。 具体的には、 [ `Renderscript` ](https://developer.xamarin.com/api/type/Android.Renderscripts.RenderScript/)ライフ サイクルと Renderscript エンジンのリソースを管理するクラス。 Android アプリは、1 つまたは複数を初期化する必要があります[ `Android.Renderscripts.Allocation` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/)オブジェクト。 割り当ては、マネージ API の割り当てと Android アプリと Renderscript ランタイムの間で共有されるメモリへのアクセスを担当します。 通常、入力には、1 つの割り当てを作成し、カーネルの出力を保持するために必要に応じて別の割り当てを作成します。 Renderscript ランタイム エンジンと関連付けられているマネージ ラッパー クラスは、割り当てによって保持されているメモリへのアクセスを管理するを追加の作業を行うには、Android アプリの開発者の必要はありません。

割り当てには 1 つまたは複数が含まれます[Android.Renderscripts.Elements](https://developer.xamarin.com/api/type/Android.Renderscripts.Element/)です。
要素は、1 回の割り当てデータを記述する特殊な型です。
割り当てに一致する必要があります、出力の要素の型、入力要素の型。 実行する場合、Renderscript は並列で入力割り当て内の各要素を反復処理して、出力に結果を書き込む割り当てします。 要素の 2 つの種類があります。

- **単純型**&ndash;概念的には、C データ型と同じではこの`float`または`char`です。

- **複合型**&ndash;この型は、C のような`struct`します。

Renderscript エンジンでは、各割り当て内の要素が、カーネルで必要なものと互換性があることを確認する実行時チェックを実行します。 割り当ての要素のデータ型が、カーネルが予測しているデータ型が一致しない場合、例外がスローされます。

すべての Renderscript カーネルの子孫である型でラップされます、 [ `Android.Renderscripts.Script` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Script/)クラスです。 `Script`クラスを使用する Renderscript のパラメーターの設定、適切な設定を`Allocations`、および、Renderscript を実行します。 2 つ`Script`Android SDK 内のサブクラス。


- **`Android.Renderscripts.ScriptIntrinsic`** &ndash; Android SDK にバンドルは、いくつかの一般的な Renderscript タスクおよびのサブクラス アクセス、 [ScriptIntrinsic](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsic/)クラスです。 開発者は、追加の手順は既に提供された、アプリケーションでこれらのスクリプトを使用する必要はありません。

- **`ScriptC_XXXXX`** &ndash; 呼ばれる_ユーザー スクリプト_は開発者が作成され、APK 内にパッケージ化されているスクリプトです。 Android ツール チェーンでは、コンパイル時に Android アプリで使用するスクリプトを許可するマネージ ラッパー クラスが生成されます。
  これらの生成されたクラスの名前が付いて、Renderscript ファイルの名前`ScriptC_`です。 記述およびユーザーのスクリプトを組み込むことは公式にサポートされていません Xamarin.Android、および、このガイドの対象外です。

これら 2 つの型ののみ、 `StringIntrinsic` Xamarin.Android でサポートされています。 このガイドでは、Xamarin.Android アプリケーションで組み込みのスクリプトを使用する方法を説明します。

## <a name="requirements"></a>要件

このガイドでは、そのターゲット API レベル 17 以上 Xamarin.Android アプリケーションには。 使用_ユーザー スクリプト_このガイドでは説明しません。

[Xamarin.Android V8 サポート ライブラリ](https://www.nuget.org/packages/Xamarin.Android.Support.v8.RenderScript/)backports Android SDK の古いバージョンを対象とするアプリのいる Renderscript API です。 Xamarin.Android プロジェクトにこのパッケージを追加することができるようにアプリ固有のスクリプトを利用する Android SDK の旧バージョンを対象。

## <a name="using-intrinsic-renderscripts-in-xamarinandroid"></a>Xamarin.Android で組み込み Renderscripts の使用

組み込みのスクリプトは、最小限の追加のコードの量での負荷の高いコンピューティング タスクを実行する優れた方法です。 デバイスの大規模なクロス セクションに最適なパフォーマンスを提供するチューニング手の形になった。
マネージ コードよりも高速 x 10 および 2 ~ 3 倍の後に、カスタムの C 実装よりも実行する組み込みのスクリプトは珍しいはありません。 一般的な処理シナリオの多くは、組み込みのスクリプトで説明します。 この組み込みのスクリプトの一覧には、Xamarin.Android 内の現在のスクリプトがについて説明します。

- [ScriptIntrinsic3DLUT](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsic3DLUT//) &ndash; RGBA 3D ルックアップ テーブルを使用する RGB に変換します。 

- [ScriptIntrinsicBLAS](https://developer.android.com/reference/android/renderscript/ScriptIntrinsicBLAS.html) &ndash; Provideshigh パフォーマンス Renderscript Api に[BLAS](http://www.netlib.org/blas/)です。 BLAS (基本的な線形代数サブプログラム) は、基本的なベクターとマトリックスの操作を実行するための標準の構成ブロックを提供するルーチンです。 

- [ScriptIntrinsicBlend](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicBlend) &ndash;ブレンドに 2 つの割り当て。

- [ScriptIntrinsicBlur](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicBlur) &ndash;ガウスのぼかしを割り当てに適用されます。

- [ScriptIntrinsicColorMatrix](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicColorMatrix/) &ndash; (つまり変更色を調整して色合い) 割り当てにカラー行列を適用します。

- [ScriptIntrinsicConvolve3x3](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicConvolve3x3/) &ndash;割り当てに 3 x 3 カラー行列を適用します。

- [ScriptIntrinsicConvolve5x5](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicConvolve5x5/) &ndash;割り当てに 5 列 5 行のカラー行列を適用します。

- [ScriptIntrinsicHistogram](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicHistogram/) &ndash;組み込みヒストグラム フィルター。

- [ScriptIntrinsicLUT](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicLUT/) &ndash;バッファーをチャネルごとのルックアップ テーブルを適用します。

- [ScriptIntrinsicResize](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicResize/) &ndash; 2D の割り当てのサイズ変更を実行するためのスクリプト。

- [ScriptIntrinsicYuvToRGB](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicYuvToRGB/) &ndash; RGB YUV バッファーに変換します。

詳細については、組み込みのスクリプトの各 API のドキュメントを参照してください。

Android アプリケーションで Renderscript を使用するための基本的な手順について説明します。

**Renderscript コンテキストを作成** &ndash; 、 [ `Renderscript` ](https://developer.xamarin.com/api/type/Android.Renderscripts.RenderScript/)クラスはマネージ ラッパー Renderscript コンテキストとは、リソース管理の初期化を制御し、クリーンアップします。 使用して、Renderscript オブジェクトを作成、`RenderScript.Create`をパラメーターとして (アクティビティ) など、Android のコンテキストを受け取るファクトリ メソッド。 次のコード行では、Renderscript コンテキストを初期化する方法を示します。

```csharp
Android.Renderscripts.RenderScript renderScript = RenderScript.Create(this);
```

**割り当てを作成する**&ndash;組み込みのスクリプトによって 1 つまたは 2 つ作成するために必要な場合があります`Allocation`s。 [ `Android.Renderscripts.Allocation` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/)クラスには、組み込みの割り当てをインスタンス化に役立てるためにいくつかのファクトリ メソッド。 例としては、次のコード スニペットは、ビットマップの割り当てを作成する方法を示します。

```csharp
Android.Graphics.Bitmap originalBitmap;
Android.Renderscripts.Allocation inputAllocation = Allocation.CreateFromBitmap(renderScript,
                                                     originalBitmap,
                                                     Allocation.MipmapControl.MipmapFull,
                                                     AllocationUsage.Script);
```

多くの場合、必要があります。 作成する、`Allocation`スクリプトの出力データを保持するためにします。 この次のスニペットを使用する方法を示しています、 `Allocation.CreateTyped` 2 台目のインスタンスを作成するためのヘルパー`Allocation`元と同じデータ型を。

```csharp
Android.Renderscripts.Allocation outputAllocation = Allocation.CreateTyped(renderScript, inputAllocation.Type);
```

**スクリプトのラッパーをインスタンス化**&ndash;組み込みスクリプト ラッパー クラスのそれぞれがヘルパー メソッドを持ちます (と通常呼ばれる`Create`) スクリプトのラッパー オブジェクトをインスタンス化するためです。 次のコード スニペットは、インスタンス化する方法の例、`ScriptIntrinsicBlur`ぼかしオブジェクト。 `Element.U8_4`ヘルパー メソッドが 4 つのフィールドのデータを保持するのに適したの 8 ビットの符号なし整数値のあるデータ型を説明する要素を作成、`Bitmap`オブジェクト。

```csharp
Android.Renderscripts.ScriptIntrinsicBlur blurScript = ScriptIntrinsicBlur.Create(renderScript, Element.U8_4(renderScript));
```

**Allocation(s)、パラメーターの設定、およびスクリプトの実行を割り当てる** &ndash; 、`Script`クラスを提供、`ForEach`メソッドを実際には、Renderscript を実行します。 このメソッドは、それぞれを反復する`Element`で、`Allocation`入力データを保持します。 場合によっては、提供するために必要な場合があります、`Allocation`出力を保持します。
`ForEach` 出力の内容が上書きされます割り当てします。 コード スニペットは、前の手順を実行にこの例では入力の割り当てを割り当てると、パラメーターを設定して、スクリプトを最後に実行 (出力に結果のコピー割り当て)。

```csharp
blurScript.SetInput(inputAllocation);
blurScript.SetRadius(25);  // Set a pamaeter
blurScript.ForEach(outputAllocation);
```

チェック アウトすることができます、 [Renderscript 使用してイメージをぼかしたり](https://developer.xamarin.com/recipes/android/other_ux/drawing/blur_an_image_with_renderscript/)Xamarin.Android で組み込みのスクリプトを使用する方法の完全な例は、レシピします。

## <a name="summary"></a>まとめ

このガイドには、Renderscript と Xamarin.Android アプリケーションで使用する方法が導入されました。 また、Renderscript とは何と Android アプリケーションでそのしくみについて簡単に説明します。 Renderscript との違いの重要なコンポーネントのいくつかが説明されている_ユーザー スクリプト_と_いるスクリプト_です。 最後に、このガイドでは、Xamarin.Android アプリケーションで組み込みのスクリプトを使用する手順について説明します。



## <a name="related-links"></a>関連リンク

- [Android.Renderscripts 名前空間](https://developer.xamarin.com/api/namespace/Android.Renderscripts/)
- [Renderscript 使用してイメージをぼかしたり](https://developer.xamarin.com/recipes/android/other_ux/drawing/blur_an_image_with_renderscript/)
- [Renderscript](https://developer.android.com/guide/topics/renderscript/compute.html)
- [チュートリアル: Renderscript の概要](https://software.intel.com/en-us/articles/renderscript-basic-sample-for-android-os)
