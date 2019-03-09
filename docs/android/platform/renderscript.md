---
title: Renderscript の概要
description: このガイドでは、Renderscript を紹介し、組み込み Renderscript API の Xamarin.Android アプリケーションでそのターゲット API レベル 17 以上を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 378793C7-5E3E-40E6-ABEE-BEAEF64E6A47
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 8364310d23739c05ff97ea8aa8fa4c56f89ea40c
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670728"
---
# <a name="an-introduction-to-renderscript"></a>Renderscript の概要

_このガイドでは、Renderscript を紹介し、組み込み Renderscript API の Xamarin.Android アプリケーションでそのターゲット API レベル 17 以上を使用する方法について説明します。_

## <a name="overview"></a>概要

Renderscript は、広範なコンピューティング リソースを必要とする Android アプリケーションのパフォーマンスを向上させるためには、Google によって作成されたプログラミング フレームワークです。 低レベルの高パフォーマンスに基づく API は[C99](https://en.wikipedia.org/wiki/C99)します。 低レベルの Cpu、Gpu、あるいは Dsp で実行される API であるため Renderscript は、次のいずれかを実行する必要がある Android アプリに適しています。

* グラフィックス
* 画像処理
* 暗号化
* 信号処理
* 数学的なルーチン

Renderscript を使用して`clang`APK にバンドルされている LLVM バイトのコードにスクリプトをコンパイルします。 最初に、アプリの実行時に、LLVM バイト コードは、デバイスのプロセッサをマシン コードにコンパイルされます。 このアーキテクチャでは、Android アプリケーションを記述すること、プロセッサごとに、デバイス自体自体せず、開発者のマシン語コードの利点を利用します。

Renderscript ルーチンへの 2 つのコンポーネントです。

1. **Renderscript ランタイム** &ndash; Renderscript の実行に責任を持っているネイティブ Api になります。 これには、アプリケーション用に記述された任意の Renderscripts が含まれます。

2. **Android のフレームワークからのマネージ ラッパー** &ndash;マネージ クラスを制御し、Renderscript ランタイム スクリプトと対話する Android アプリを許可します。 Renderscript ランタイムを制御するためのフレームワークが提供するクラス、だけでなく Android ツール チェーンは Renderscript のソース コードを調べるし、Android アプリケーションで使用するためのマネージ ラッパー クラスを生成します。

次の図は、これらのコンポーネントを関連付ける方法を示しています。

![Android のフレームワークが Renderscript ランタイムと対話する方法を示す図](renderscript-images/renderscript-01.png)

Renderscripts を Android アプリケーションで使用するための 3 つの重要な概念があります。

1. **コンテキスト**&ndash;マネージ Renderscript にリソースを割り当てるし、Android アプリを渡すし、Renderscript からデータを受信することを許可する Android SDK で提供される API。

2. **A_計算カーネル_** &ndash;とも呼ばれる、_ルートのカーネル_または_カーネル_、この作業を行うルーチン。 カーネルが C の関数とよく似ています。割り当てられたメモリ内のすべてのデータに対する実行される並列化可能なルーチンになります。

3. **割り当てられたメモリ**&ndash;経由のカーネルとの間でデータが渡される、 _[割り当て](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/)_ します。 カーネルは、1 つの入力を必要がありますや、1 つの出力の割り当て。

[Android.Renderscripts](https://developer.xamarin.com/api/namespace/Android.Renderscripts/) Renderscript ランタイムとやり取りするためのクラスが名前空間に含まれています。 具体的には、 [ `Renderscript` ](https://developer.xamarin.com/api/type/Android.Renderscripts.RenderScript/)クラスは、ライフ サイクルと Renderscript エンジンのリソースを管理します。 Android アプリは、1 つまたは複数を初期化する必要があります。 [`Android.Renderscripts.Allocation`](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/)
オブジェクト。 割り当ては、割り当てと Android アプリと Renderscript ランタイム間で共有されるメモリへのアクセスを担当するマネージ API です。 通常、入力、1 つの割り当てが作成され、カーネルの出力を保持するために必要に応じて別の割り当てが作成されます。 Renderscript ランタイム エンジンと関連付けられたマネージ ラッパー クラスは、割り当てによって保持されているメモリへのアクセスを管理する、余分な作業を行うため、Android アプリ開発者の必要はありません。

割り当てには 1 つまたは複数が含まれます[Android.Renderscripts.Elements](https://developer.xamarin.com/api/type/Android.Renderscripts.Element/)します。
要素は、各割り当てのデータを記述する特殊な型です。
要素の型の割り当て、出力の入力要素の型に一致する必要があります。 実行する場合、Renderscript は入力の割り当てを並列に内の各要素を反復処理し、結果を出力に書き込む割り当て。 要素の 2 種類あります。

- **単純型**&ndash;概念的にはこれは、C データ型と同じ`float`または`char`します。

- **複合型**&ndash;この型は、C のような`struct`します。

Renderscript エンジンでは、各割り当ての要素が、カーネルで必要なものと互換性があることを確認するランタイム チェックを実行します。 割り当て内の要素のデータ型では、カーネルが予期しているデータ型は一致しない場合、例外がスローされます。

子孫である型ですべての Renderscript カーネルが折り返される、 [`Android.Renderscripts.Script`](https://developer.xamarin.com/api/type/Android.Renderscripts.Script/)
クラスの新しいインスタンスを初期化します。 `Script`クラスを使用する Renderscript のパラメーターを設定、適切な設定を`Allocations`、および、Renderscript を実行します。 2 つ`Script`Android SDK のサブクラス。


- **`Android.Renderscripts.ScriptIntrinsic`** &ndash; Android SDK にバンドルされておよびのサブクラスによってアクセス可能ないくつかの一般的な Renderscript タスク、 [ScriptIntrinsic](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsic/)クラス。 開発者は既に提供されているように、そのアプリケーション内でこれらのスクリプトを使用する追加の手順は必要はありません。

- **`ScriptC_XXXXX`** &ndash; 呼ばれます_ユーザー スクリプト_、これらは開発者が作成され、APK にパッケージ化されるスクリプト。 Android ツール チェーンでは、コンパイル時にスクリプトを Android アプリで使用できるようにするマネージ ラッパー クラスが生成されます。
  生成されたクラスの名前が付いて、Renderscript ファイルの名前`ScriptC_`します。 記述して、ユーザーのスクリプトを組み込むことは正式にサポートされていません Xamarin.Android でこのガイドの対象外です。

これら 2 つの型ののみ、 `StringIntrinsic` Xamarin.Android ではサポートされています。 このガイドは、Xamarin.Android アプリケーションで組み込みのスクリプトを使用する方法について説明します。

## <a name="requirements"></a>必要条件

このガイドでは、そのターゲット API レベル 17 以上の Xamarin.Android アプリケーションは。 使用_ユーザー スクリプト_このガイドでは説明しません。

[Xamarin.Android V8 サポート ライブラリ](https://www.nuget.org/packages/Xamarin.Android.Support.v8.RenderScript/)backports Android SDK の以前のバージョンを対象とするアプリのいる Renderscript API の。 このパッケージを Xamarin.Android プロジェクトに追加する必要がありますを許可するアプリ組み込みのスクリプトを活用する Android SDK の旧バージョンを対象。

## <a name="using-intrinsic-renderscripts-in-xamarinandroid"></a>Xamarin.Android での組み込み Renderscripts の使用

組み込みのスクリプトは、追加のコード量が最小限でコンピューティング集中型のタスクを実行する優れた方法です。 デバイスの大規模なクロス セクションに最適なパフォーマンスを提供するために調整を手にしました。
組み込みのスクリプトを 10 倍のマネージ コードよりも高速およびカスタムの C# 実装よりも後に 2 ~ 3 x 回を実行することは珍しくはありません。 組み込みのスクリプトでは、通常の処理のシナリオの多くがについて説明します。 この一連の組み込みのスクリプトでは、Xamarin.Android では、現在のスクリプトについて説明します。

- [ScriptIntrinsic3DLUT](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsic3DLUT//) &ndash; RGBA 3D ルックアップ テーブルを使用する RGB に変換します。 

- [ScriptIntrinsicBLAS](https://developer.android.com/reference/android/renderscript/ScriptIntrinsicBLAS.html) &ndash; Provideshigh パフォーマンス Renderscript Api に[BLAS](http://www.netlib.org/blas/)します。 BLAS (基本線形代数サブプログラム) は、基本的なベクトルおよびマトリックスの操作を実行するための標準の構成ブロックを提供するルーチンです。 

- [ScriptIntrinsicBlend](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicBlend) &ndash; 2 つの割り当てをブレンドします。

- [ScriptIntrinsicBlur](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicBlur) &ndash;ガウスのぼかしを割り当てに適用されます。

- [ScriptIntrinsicColorMatrix](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicColorMatrix/) &ndash; (つまり、色の変更、調整 hue) 割り当てにカラー行列を適用します。

- [ScriptIntrinsicConvolve3x3](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicConvolve3x3/) &ndash; 3 x 3 カラー行列を割り当てに適用されます。

- [ScriptIntrinsicConvolve5x5](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicConvolve5x5/) &ndash; 5 x 5 カラー行列を割り当てに適用されます。

- [ScriptIntrinsicHistogram](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicHistogram/) &ndash;ヒストグラムの組み込みフィルター。

- [ScriptIntrinsicLUT](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicLUT/) &ndash;チャネルごとのルックアップ テーブルをバッファーに適用されます。

- [ScriptIntrinsicResize](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicResize/) &ndash; 2D の割り当てのサイズ変更を実行するためのスクリプト。

- [ScriptIntrinsicYuvToRGB](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicYuvToRGB/) &ndash; RGB YUV バッファーに変換します。

詳細については、組み込みのスクリプトの各 API のドキュメントを参照してください。

Android アプリケーションで Renderscript を使用するための基本的な手順について説明します。

**Renderscript コンテキスト作成** &ndash; [`Renderscript`](https://developer.xamarin.com/api/type/Android.Renderscripts.RenderScript/)
クラスは、マネージ ラッパー Renderscript コンテキストとは、リソース管理の初期化を制御し、クリーンアップします。 使用して、Renderscript オブジェクトを作成、`RenderScript.Create`ファクトリ メソッドをパラメーターとして (アクティビティ) など、Android のコンテキストを取得します。 次のコード行では、Renderscript コンテキストを初期化する方法を示しています。

```csharp
Android.Renderscripts.RenderScript renderScript = RenderScript.Create(this);
```

**割り当てを作成**&ndash;組み込みのスクリプトによって 1 つまたは 2 つの作成に必要な場合があります`Allocation`秒。 、 [`Android.Renderscripts.Allocation`](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/)
クラスには、組み込みの割り当てをインスタンス化を支援するいくつかのファクトリ メソッドがあります。 例としては、次のコード スニペットは、ビットマップの割り当てを作成する方法を示します。

```csharp
Android.Graphics.Bitmap originalBitmap;
Android.Renderscripts.Allocation inputAllocation = Allocation.CreateFromBitmap(renderScript,
                                                     originalBitmap,
                                                     Allocation.MipmapControl.MipmapFull,
                                                     AllocationUsage.Script);
```

作成する必要する多くの場合、`Allocation`スクリプトの出力データを保持します。 この次のスニペットを使用する方法を示しています、 `Allocation.CreateTyped` 2 つ目のインスタンスを作成するためのヘルパー`Allocation`元と同じ型こと。

```csharp
Android.Renderscripts.Allocation outputAllocation = Allocation.CreateTyped(renderScript, inputAllocation.Type);
```

**スクリプト ラッパーをインスタンス化**&ndash;の組み込みのスクリプトのラッパー クラスのヘルパー メソッドがあります (と通常呼ばれる`Create`) スクリプトのラッパー オブジェクトをインスタンス化するためです。 次のコード スニペットは、インスタンス化する方法の例を`ScriptIntrinsicBlur`ぼかしオブジェクト。 `Element.U8_4`ヘルパー メソッドは 4 つのフィールドのデータを保持するのに適したの 8 ビットの符号なし整数値であるデータ型を記述する要素を作成、`Bitmap`オブジェクト。

```csharp
Android.Renderscripts.ScriptIntrinsicBlur blurScript = ScriptIntrinsicBlur.Create(renderScript, Element.U8_4(renderScript));
```

**Allocation(s)、パラメーターの設定、およびスクリプトの実行を割り当てる** &ndash; 、`Script`クラスには、 `ForEach` Renderscript を実際に実行するメソッド。 このメソッドは、それぞれを反復する`Element`で、`Allocation`入力データを保持します。 場合によってで提供するために必要な場合があります、`Allocation`出力を保持しています。
`ForEach` 出力の内容が上書きされる割り当て。 この例を前の手順のコード スニペットを実行するには、入力の割り当てを割り当て、パラメーターを設定、および、最後にスクリプトを実行する方法を示しています (結果を出力にコピー割り当て)。

```csharp
blurScript.SetInput(inputAllocation);
blurScript.SetRadius(25);  // Set a pamaeter
blurScript.ForEach(outputAllocation);
```

チェック アウトすることができます、 [Renderscript でイメージをぼかす](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/drawing/blur_an_image_with_renderscript)レシピ、Xamarin.Android で組み込みのスクリプトを使用する方法の完全な例が。

## <a name="summary"></a>まとめ

このガイドには、Xamarin.Android アプリケーションで使用する方法と Renderscript が導入されました。 また、Renderscript と Android のアプリケーションでそのしくみについて簡単に説明します。 Renderscript との違いの重要なコンポーネントのいくつか説明されている_ユーザー スクリプト_と_いるスクリプト_します。 最後に、このガイドでは、Xamarin.Android アプリケーションで組み込みのスクリプトを使用する手順について説明します。



## <a name="related-links"></a>関連リンク

- [Android.Renderscripts 名前空間](https://developer.xamarin.com/api/namespace/Android.Renderscripts/)
- [ぼかし Renderscript イメージ](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/drawing/blur_an_image_with_renderscript)
- [Renderscript](https://developer.android.com/guide/topics/renderscript/compute.html)
- [チュートリアル: Renderscript の概要](https://software.intel.com/en-us/articles/renderscript-basic-sample-for-android-os)
