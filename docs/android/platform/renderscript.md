---
title: Renderscript の概要
description: このガイドでは、Renderscript について紹介し、API レベル 17 以降をターゲットとする Xamarin.Android アプリケーションで組み込みの Renderscript API を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 378793C7-5E3E-40E6-ABEE-BEAEF64E6A47
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 884b69b0cdecf4f979cec314b6440974c5bac97d
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "73019796"
---
# <a name="an-introduction-to-renderscript"></a>Renderscript の概要

_このガイドでは、Renderscript について紹介し、API レベル 17 以降をターゲットとする Xamarin.Android アプリケーションで組み込みの Renderscript API を使用する方法について説明します。_

## <a name="overview"></a>概要

Renderscript は、膨大なコンピューティング リソースを必要とする Android アプリケーションのパフォーマンスを向上させる目的で Google が作成したプログラミング フレームワークです。 これは、[C99](https://en.wikipedia.org/wiki/C99) に基づいた低レベルでハイ パフォーマンスな API です。 CPU、GPU、または DSP 上で実行される低レベル API であるため、Renderscript は次のいずれかを実行する必要がある Android アプリに適しています。

- グラフィックス
- 画像処理
- 暗号化
- シグナル処理
- 数学ルーチン

Renderscript では、`clang` を使用して、APK にバンドルされている LLVM バイト コードにスクリプトをコンパイルします。 アプリを初めて実行すると、LLVM バイト コードがデバイス上のプロセッサに合わせたマシン コードにコンパイルされます。 このアーキテクチャにより、Android アプリケーションではマシン コードの利点を活用できます。開発者自身がデバイス上のプロセッサごとに作成する必要はありません。

Renderscript ルーチンには、次の 2 つのコンポーネントがあります。

1. **Renderscript ランタイム** &ndash; Renderscript の実行を担当するネイティブ API です。 これには、アプリケーション用に記述されたすべての Renderscript が含まれます。

2. **Android Framework のマネージド ラッパー** &ndash; Android アプリで Renderscript ランタイムとスクリプトを制御および操作できるようになるマネージド クラス。 フレームワークに用意されている Renderscript ランタイムを制御するためのクラスに加え、Android ツールチェーンでは、Renderscript ソース コードが確認され、Android アプリケーションに使用するマネージド ラッパー クラスが生成されます。

次の図は、これらのコンポーネントがどのように関連するかを示しています。

![Android Framework が Renderscript ランタイムとどのように対話するかを示す図](renderscript-images/renderscript-01.png)

Android アプリケーションで Renderscript を使用するには、次の 3 つの重要な概念があります。

1. **コンテキスト** &ndash; Android SDK が提供するマネージド API。リソースが Renderscript に割り当てられ、Android アプリと Renderscript がデータをやり取りできるようになります。

2. **"_コンピューティング カーネル_"** &ndash; "_ルート カーネル_" または "_カーネル_" とも呼ばれます。これは作業を実行するルーチンです。 カーネルは C 関数とよく似ています。割り当てられたメモリ内のすべてのデータに対して実行される並列化可能なルーチンです。

3. **割り当てられたメモリ** &ndash; データは、" _[Allocation](xref:Android.Renderscripts.Allocation)_ " を介してカーネルとの間でやり取りされます。 カーネルは、1 つの入力と 1 つの出力の Allocation を持つことができます。

[Android.Renderscripts](xref:Android.Renderscripts) 名前空間には、Renderscript ランタイムと対話するためのクラスが含まれています。 特に、[`Renderscript`](xref:Android.Renderscripts.RenderScript) クラスでは、Renderscript エンジンのライフサイクルとリソースが管理されます。 Android アプリでは、1 つ以上の [`Android.Renderscripts.Allocation`](xref:Android.Renderscripts.Allocation)
オブジェクトを初期化する必要があります Allocation は、Android アプリと Renderscript ランタイム間で共有されるメモリの割り当てとアクセスを担当するマネージド API です。 通常、入力用に 1 つの Allocation が作成され、必要に応じて、カーネルの出力を保持するためにもう 1 つの Allocation が作成されます。 Renderscript ランタイム エンジンと関連するマネージド ラッパー クラスで、Allocation によって保持されているメモリへのアクセスが管理されます。Android アプリ開発者が追加の作業を行う必要はありません。

Allocation には、1 つまたは複数の [Android.Renderscripts.Element](xref:Android.Renderscripts.Element) が含まれます。
Element は、各 Allocation のデータを記述する特殊な型です。
出力 Allocation の Element の型は、入力 Element の型と一致する必要があります。 実行すると、Renderscript によって入力 Allocation の各 Element が並列で反復処理され、結果が出力 Allocation に書き込まれます。 Element には、次の 2 種類があります。

- **単純型** &ndash; 概念的には、C のデータ型の `float` または `char` と同じです。

- **複合型** &ndash; この型は、C の `struct` と似ています。

各 Allocation の Element がカーネルに必要なものと互換性があることを確認するために、Renderscript エンジンによってランタイム チェックが実行されます。 Allocation 内の Element のデータ型が、カーネルで想定されているデータ型と一致しない場合は、例外がスローされます。

すべての Renderscript カーネルは、[`Android.Renderscripts.Script`](xref:Android.Renderscripts.Script) の子孫である型によってラップされます。
クラスの新しいインスタンスを初期化します。 `Script` クラスは、Renderscript のパラメーターの設定、適切な `Allocations` の設定、および Renderscript の実行に使用されます。 Android SDK には、次の 2 つの `Script` サブクラスがあります。

- **`Android.Renderscripts.ScriptIntrinsic`** &ndash; 一般的な Renderscript タスクの一部は Android SDK にバンドルされており、[ScriptIntrinsic](xref:Android.Renderscripts.ScriptIntrinsic) クラスのサブクラスからアクセスできます。 これらのスクリプトは既に用意されているため、開発者がアプリケーションで使用するために追加の手順を実行する必要はありません。

- **`ScriptC_XXXXX`** &ndash; "_ユーザー スクリプト_" とも呼ばれます。開発者によって記述され、APK にパッケージ化されたスクリプトです。 コンパイル時に、Android ツールチェーンによって、Android アプリでスクリプトを使用できるようになるマネージド ラッパー クラスが生成されます。
  これらの生成されるクラスの名前は、Renderscript ファイルの名前であり、先頭に `ScriptC_` が付きます。 ユーザー スクリプトの記述と組み込みは、Xamarin.Android では公式にサポートされておらず、このガイドでは扱いません。

この 2 種類のうち、`StringIntrinsic` のみが Xamarin.Android でサポートされています。 このガイドでは、Xamarin.Android アプリケーションで組み込みスクリプトを使用する方法について説明します。

## <a name="requirements"></a>必要条件

このガイドは、API レベル 17 以降をターゲットとする Xamarin.Android アプリケーション向けです。 "_ユーザー スクリプト_" の使用については、このガイドでは扱っていません。

[Xamarin.Android V8 サポート ライブラリ](https://www.nuget.org/packages/Xamarin.Android.Support.v8.RenderScript/)は、以前のバージョンの Android SDK をターゲットとするアプリ向けの組み込みの Renderscript API をバックポートします。 このパッケージを Xamarin.Android プロジェクトに追加すると、以前のバージョンの Android SDK をターゲットとするアプリが組み込みのスクリプトを利用できるようになります。

## <a name="using-intrinsic-renderscripts-in-xamarinandroid"></a>Xamarin.Android での組み込みの Renderscript の使用

組み込みのスクリプトは、最小限の追加コードで大量のコンピューティング タスクを実行するために適した方法です。 これらは、多彩なデバイスで最適なパフォーマンスを実現できるように手動で調整されています。
組み込みのスクリプトがマネージド コードよりも 10 倍速くなり、カスタムの C 実装後に 2 から 3 倍の時間がかかることは珍しくありません。 一般的な処理シナリオの多くは、組み込みのスクリプトによってカバーされています。 組み込みのスクリプトのこの一覧では、Xamarin.Android の現在のスクリプトについて説明します。

- [ScriptIntrinsic3DLUT](xref:Android.Renderscripts.ScriptIntrinsic3DLUT) &ndash; 3D ルックアップ テーブルを使用して RGB を RGBA に変換します。 

- [ScriptIntrinsicBLAS](https://developer.android.com/reference/android/renderscript/ScriptIntrinsicBLAS.html) &ndash; 高パフォーマンスの Renderscript API を [BLAS](http://www.netlib.org/blas/) に提供します。 BLAS (Basic Linear Algebra Subprograms) は、基本的なベクターおよびマトリックス演算を実行するための標準の構成要素が用意されているルーチンです。 

- [ScriptIntrinsicBlend](xref:Android.Renderscripts.ScriptIntrinsicBlend) &ndash; 2 つの Allocation を結合します。

- [ScriptIntrinsicBlur](xref:Android.Renderscripts.ScriptIntrinsicBlur) &ndash; Allocation にガウスぼかしを適用します。

- [ScriptIntrinsicColorMatrix](xref:Android.Renderscripts.ScriptIntrinsicColorMatrix) &ndash; カラー マトリックスを Allocation に適用します (つまり、色の変更、色相の調整)。

- [ScriptIntrinsicConvolve3x3](xref:Android.Renderscripts.ScriptIntrinsicConvolve3x3) &ndash; 3 x 3 のカラー マトリックスを Allocation に適用します。

- [ScriptIntrinsicConvolve5x5](xref:Android.Renderscripts.ScriptIntrinsicConvolve5x5) &ndash; 5 x 5 のカラー マトリックスを Allocation に適用します。

- [ScriptIntrinsicHistogram](xref:Android.Renderscripts.ScriptIntrinsicHistogram) &ndash; 組み込みのヒストグラム フィルターです。

- [ScriptIntrinsicLUT](xref:Android.Renderscripts.ScriptIntrinsicLUT) &ndash; チャネルごとのルックアップ テーブルをバッファーに適用します。

- [ScriptIntrinsicResize](xref:Android.Renderscripts.ScriptIntrinsicResize) &ndash; 2D 割り当てのサイズ変更を実行するためのスクリプトです。

- [ScriptIntrinsicYuvToRGB](xref:Android.Renderscripts.ScriptIntrinsicYuvToRGB) &ndash; YUV バッファーを RGB に変換します。

各組み込みのスクリプトの詳細については、API のドキュメントを参照してください。

次に Android アプリケーションで Renderscript を使用するための基本的な手順について説明します。

**Renderscript コンテキストを作成する** &ndash; [`Renderscript`](xref:Android.Renderscripts.RenderScript)
クラスは、Renderscript コンテキストのマネージド ラッパーです。初期化、リソース管理、クリーンアップの制御に使用されます。 Renderscript オブジェクトは、`RenderScript.Create` ファクトリ メソッドを使用して作成されます。これはパラメーターとして Android コンテキスト (Activity など) を受け取ります。 次のコード行は、Renderscript コンテキストを初期化する方法を示しています。

```csharp
Android.Renderscripts.RenderScript renderScript = RenderScript.Create(this);
```

**Allocation を作成する** &ndash; 組み込みのスクリプトによっては、必要に応じて 1 つまたは 2 つの `Allocation` を作成します。 [`Android.Renderscripts.Allocation`](xref:Android.Renderscripts.Allocation)
クラスには、組み込みの割り当てをインスタンス化するために役立ついくつかのファクトリ メソッドがあります。 たとえば、次のコード スニペットは、Bitmap の Allocation を作成する方法を示しています。

```csharp
Android.Graphics.Bitmap originalBitmap;
Android.Renderscripts.Allocation inputAllocation = Allocation.CreateFromBitmap(renderScript,
                                                     originalBitmap,
                                                     Allocation.MipmapControl.MipmapFull,
                                                     AllocationUsage.Script);
```

多くの場合、スクリプトの出力データを保持するために `Allocation` を作成する必要があります。 次のスニペットは、`Allocation.CreateTyped` ヘルパーを使用して、元と同じ型の 2 番目の `Allocation` をインスタンス化する方法を示しています。

```csharp
Android.Renderscripts.Allocation outputAllocation = Allocation.CreateTyped(renderScript, inputAllocation.Type);
```

**スクリプト ラッパーをインスタンス化する** &ndash; 各組み込みのスクリプト ラッパー クラスには、そのスクリプトのラッパー オブジェクトをインスタンス化するためのヘルパー メソッド (通常は `Create` と呼ばれます) が必要です。 次のコード スニペットは、`ScriptIntrinsicBlur` ぼかしオブジェクトをインスタンス化する方法の例です。 `Element.U8_4` ヘルパー メソッドを使うと、`Bitmap` オブジェクトのデータを保持するために適した、8 ビットの符号なし整数値の 4 つのフィールドであるデータ型を記述する Element が作成されます。

```csharp
Android.Renderscripts.ScriptIntrinsicBlur blurScript = ScriptIntrinsicBlur.Create(renderScript, Element.U8_4(renderScript));
```

**Allocation の割り当て、パラメーターの設定、スクリプトの実行** &ndash; `Script` クラスには、Renderscript を実際に実行する `ForEach` メソッドが用意されています。 このメソッドを使うと、入力データを保持している `Allocation` 内の個々の `Element` を反復処理できます。 場合によっては、出力を保持する `Allocation` を用意する必要があります。
`ForEach` によって、出力 Allocation の内容が上書きされます。 前の手順からコード スニペットを続行するために、この例では、入力 Allocation を割り当て、パラメーターを設定し、最後にスクリプトを実行する (そして結果を出力 Allocation にコピーする) 方法を示します。

```csharp
blurScript.SetInput(inputAllocation);
blurScript.SetRadius(25);  // Set a pamaeter
blurScript.ForEach(outputAllocation);
```

[Renderscript を使用して画像をぼかす](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/drawing/blur_an_image_with_renderscript)方法のページを参照してください。Xamarin.Android で組み込みのスクリプトを使用する方法の例として最適です。

## <a name="summary"></a>まとめ

このガイドでは、Renderscript と、Xamarin.Android アプリケーションでの使用方法について紹介しました。 Renderscript の概要と、Android アプリケーションでどのように機能するかについて簡単に説明しました。 Renderscript の主要なコンポーネントの一部と、"_ユーザー スクリプト_" と "_組み込みのスクリプト_" の違いについて説明しました。 最後に、このガイドでは、Xamarin.Android アプリケーションで組み込みのスクリプトを使用する手順について説明しました。

## <a name="related-links"></a>関連リンク

- [Android.Renderscripts 名前空間](xref:Android.Renderscripts)
- [Renderscript を使用して画像をぼかす](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/drawing/blur_an_image_with_renderscript)
- [Renderscript](https://developer.android.com/guide/topics/renderscript/compute.html)
- [チュートリアル: Renderscript の概要](https://software.intel.com/en-us/articles/renderscript-basic-sample-for-android-os)
