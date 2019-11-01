---
title: Renderscript の概要
description: このガイドでは、Renderscript について説明し、API レベル17以上を対象とする Xamarin Android アプリケーションで組み込みの Renderscript API を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 378793C7-5E3E-40E6-ABEE-BEAEF64E6A47
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 884b69b0cdecf4f979cec314b6440974c5bac97d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73019796"
---
# <a name="an-introduction-to-renderscript"></a>Renderscript の概要

_このガイドでは、Renderscript について説明し、API レベル17以上を対象とする Xamarin Android アプリケーションで組み込みの Renderscript API を使用する方法について説明します。_

## <a name="overview"></a>概要

Renderscript は、広範なコンピューティングリソースを必要とする Android アプリケーションのパフォーマンスを向上させるために、Google によって作成されたプログラミングフレームワークです。 これは、 [C99](https://en.wikipedia.org/wiki/C99)に基づく低レベルの高パフォーマンス API です。 Cpu、Gpu、または Dsp で実行される低レベルの API であるため、Renderscript は、次のいずれかを実行する必要がある Android アプリに適しています。

- グラフィックス
- 画像処理
- 暗号化
- シグナル処理
- 数学ルーチン

Renderscript は `clang` を使用し、APK にバンドルされている LLVM バイトコードにスクリプトをコンパイルします。 アプリを初めて実行すると、LLVM バイトコードがデバイス上のプロセッサのマシンコードにコンパイルされます。 このアーキテクチャを使用すると、Android アプリケーションは、開発者自身がデバイス自体の各プロセッサ用に作成する必要がなく、マシンコードの利点を活用できます。

Renderscript ルーチンには、次の2つのコンポーネントがあります。

1. **Renderscript ランタイム**&ndash; これは、renderscript の実行を担当するネイティブ api です。 これには、アプリケーション用に記述されたすべての Renderscripts が含まれます。

2. Android **Framework のマネージラッパー** &ndash;、android アプリが renderscript ランタイムとスクリプトの制御と対話を行えるようにするマネージクラスです。 Renderscript ランタイムを制御するためのフレームワークが提供するクラスに加えて、Android ツールチェーンは、Renderscript ソースコードを調べ、Android アプリケーションで使用するためのマネージラッパークラスを生成します。

次の図は、これらのコンポーネントがどのように関連するかを示しています。

![Android Framework が Renderscript ランタイムとどのように対話するかを示す図](renderscript-images/renderscript-01.png)

Android アプリケーションで Renderscripts を使用するには、次の3つの重要な概念があります。

1. **コンテキスト**&ndash;、renderscript にリソースを割り当て、Android アプリが renderscript からデータを受け渡しできるようにする、Android SDK によって提供されるマネージ API です。

2. **_コンピューティングカーネル_** &ndash;_ルートカーネル_や_カーネル_とも呼ばれます。これは、処理を行うルーチンです。 カーネルは C 関数によく似ています。これは、割り当てられたメモリ内のすべてのデータに対して実行される並列化可能なルーチンです。

3. 割り当てられた**メモリ**&ndash; データは、 _[割り当て](xref:Android.Renderscripts.Allocation)_ によってカーネルとの間でやり取りされます。 カーネルは、1つの入力または1つの出力割り当てを持つことができます。

[Android の Renderscripts](xref:Android.Renderscripts)名前空間には、renderscripts ランタイムと対話するためのクラスが含まれています。 特に、 [`Renderscript`](xref:Android.Renderscripts.RenderScript)クラスは、renderscript エンジンのライフサイクルとリソースを管理します。 Android アプリは1つ以上の[`Android.Renderscripts.Allocation`](xref:Android.Renderscripts.Allocation)を初期化する必要があります
表す. 割り当ては、Android アプリと Renderscript ランタイム間で共有されるメモリへの割り当てとアクセスを担当するマネージ API です。 通常、入力用に1つの割り当てが作成され、必要に応じて別の割り当てを作成してカーネルの出力を保持します。 Renderscript ランタイムエンジンと関連付けられているマネージラッパークラスは、割り当てによって保持されているメモリへのアクセスを管理します。 Android アプリ開発者が余分な作業を行う必要はありません。

割り当てには、1つまたは複数の[Android. Renderscripts. 要素](xref:Android.Renderscripts.Element)が含まれます。
要素は、各割り当てのデータを記述する特殊な型です。
出力割り当ての要素型は、入力要素の型と一致している必要があります。 実行すると、Renderscript は入力割り当て内の各要素を並列に反復処理し、結果を出力割り当てに書き込みます。 要素には、次の2種類があります。

- **単純型**&ndash; 概念的には、これは C データ型、`float` または `char`と同じです。

- **複合型**&ndash;、この型は C `struct`に似ています。

Renderscript エンジンはランタイムチェックを実行し、各割り当ての要素がカーネルで要求されているものと互換性があることを確認します。 割り当て内の要素のデータ型が、カーネルが想定しているデータ型と一致しない場合、例外がスローされます。

すべての Renderscript カーネルは、の子孫である型によってラップされ[`Android.Renderscripts.Script`](xref:Android.Renderscripts.Script)
クラスの新しいインスタンスを初期化します。 `Script` クラスは、Renderscript のパラメーターを設定し、適切な `Allocations`を設定し、Renderscript を実行するために使用されます。 Android SDK には、次の2つの `Script` サブクラスがあります。

- **`Android.Renderscripts.ScriptIntrinsic`** &ndash; いくつかの一般的な renderscript タスクは Android SDK にバンドルされており、 [scriptintrinsic](xref:Android.Renderscripts.ScriptIntrinsic)クラスのサブクラスからアクセスできます。 開発者は、既に提供されているように、これらのスクリプトをアプリケーションで使用する必要はありません。

- **`ScriptC_XXXXX`** &ndash;_ユーザースクリプト_とも呼ばれ、これらは開発者が作成して apk にパッケージ化したスクリプトです。 コンパイル時に、android のツールチェーンによって、Android アプリでスクリプトを使用できるようにするマネージラッパークラスが生成されます。
  これらの生成されたクラスの名前は、`ScriptC_`というプレフィックスが付いた Renderscript ファイルの名前です。 ユーザースクリプトの作成と統合は、Xamarin Android では公式にサポートされておらず、このガイドの範囲を超えています。

この2種類のうち、`StringIntrinsic` のみが Xamarin Android でサポートされています。 このガイドでは、Xamarin Android アプリケーションで組み込みスクリプトを使用する方法について説明します。

## <a name="requirements"></a>［要件］

このガイドは、API レベル17以上を対象とする Xamarin Android アプリケーションを対象としています。 _ユーザースクリプト_の使用については、このガイドでは説明しません。

[V8 サポートライブラリ](https://www.nuget.org/packages/Xamarin.Android.Support.v8.RenderScript/)backports は、古いバージョンの Android SDK を対象とするアプリ用のているか RENDERSCRIPT API です。 このパッケージを Xamarin Android プロジェクトに追加するには、古いバージョンの Android SDK を対象とするアプリで組み込みスクリプトを利用できるようにする必要があります。

## <a name="using-intrinsic-renderscripts-in-xamarinandroid"></a>Xamarin. Android での組み込みの Renderscripts の使用

組み込みのスクリプトは、最小限のコードを追加するだけで、大量のコンピューティングタスクを実行するための優れた方法です。 デバイスの大きなクロスセクションで最適なパフォーマンスを提供するように、手動でチューニングされています。
組み込みスクリプトがマネージコードよりも10倍高速に実行されることは珍しくありません。また、カスタム C 実装より後の2倍の時間でも実行できます。 一般的な処理シナリオの多くは、組み込みスクリプトによってカバーされています。 組み込みスクリプトのこのリストには、Xamarin. Android の現在のスクリプトが記述されています。

- [ScriptIntrinsic3DLUT](xref:Android.Renderscripts.ScriptIntrinsic3DLUT) &ndash; は、3d ルックアップテーブルを使用して RGB を RGBA に変換します。 

- [ScriptIntrinsicBLAS](https://developer.android.com/reference/android/renderscript/ScriptIntrinsicBLAS.html)は、Provideshigh Performance Renderscript Api を[BLAS](http://www.netlib.org/blas/)に &ndash; します。 BLAS (基本的な線形代数サブプログラム) は、基本的なベクターおよびマトリックス操作を実行するための標準の構成要素を提供するルーチンです。 

- [ScriptIntrinsicBlend](xref:Android.Renderscripts.ScriptIntrinsicBlend) &ndash; は、2つの割り当てを結合します。

- [ScriptIntrinsicBlur](xref:Android.Renderscripts.ScriptIntrinsicBlur) &ndash; は、割り当てにガウスぼかしを適用します。

- [ScriptIntrinsicColorMatrix](xref:Android.Renderscripts.ScriptIntrinsicColorMatrix) &ndash; は、カラーマトリックスを割り当てに適用します (つまり、色を変更し、色合いを調整します)。

- [ScriptIntrinsicConvolve3x3](xref:Android.Renderscripts.ScriptIntrinsicConvolve3x3) &ndash; は、3 x 3 色行列を割り当てに適用します。

- [ScriptIntrinsicConvolve5x5](xref:Android.Renderscripts.ScriptIntrinsicConvolve5x5) &ndash; は、5 x 5 のカラーマトリックスを割り当てに適用します。

- [ScriptIntrinsicHistogram](xref:Android.Renderscripts.ScriptIntrinsicHistogram)は、組み込みのヒストグラムフィルター &ndash; ます。

- [ScriptIntrinsicLUT](xref:Android.Renderscripts.ScriptIntrinsicLUT) &ndash; は、チャネルごとのルックアップテーブルをバッファーに適用します。

- [ScriptIntrinsicResize](xref:Android.Renderscripts.ScriptIntrinsicResize) &ndash;、2d 割り当てのサイズ変更を実行するためのスクリプトです。

- [ScriptIntrinsicYuvToRGB](xref:Android.Renderscripts.ScriptIntrinsicYuvToRGB) &ndash; は、YUV バッファーを RGB に変換します。

各組み込みスクリプトの詳細については、API のドキュメントを参照してください。

Android アプリケーションで Renderscript を使用するための基本的な手順については、次に説明します。

[`Renderscript`](xref:Android.Renderscripts.RenderScript) &ndash; **renderscript コンテキストを作成**する
クラスは、Renderscript コンテキストのマネージラッパーであり、初期化、リソース管理、クリーンアップを制御します。 Renderscript オブジェクトは、`RenderScript.Create` ファクトリメソッドを使用して作成されます。このメソッドは、Android コンテキスト (アクティビティなど) をパラメーターとして受け取ります。 次のコード行は、Renderscript コンテキストを初期化する方法を示しています。

```csharp
Android.Renderscripts.RenderScript renderScript = RenderScript.Create(this);
```

**割り当てを作成**する組み込みスクリプトに応じて &ndash; 1 つまたは2つの `Allocation`を作成する必要がある場合があります。 [`Android.Renderscripts.Allocation`](xref:Android.Renderscripts.Allocation)
クラスには、組み込みのの割り当てをインスタンス化するためのファクトリメソッドがいくつか用意されています。 例として、次のコードスニペットはビットマップの割り当てを作成する方法を示しています。

```csharp
Android.Graphics.Bitmap originalBitmap;
Android.Renderscripts.Allocation inputAllocation = Allocation.CreateFromBitmap(renderScript,
                                                     originalBitmap,
                                                     Allocation.MipmapControl.MipmapFull,
                                                     AllocationUsage.Script);
```

多くの場合、スクリプトの出力データを保持する `Allocation` を作成する必要があります。 次のスニペットは、`Allocation.CreateTyped` ヘルパーを使用して、元の型と同じ型の2番目の `Allocation` をインスタンス化する方法を示しています。

```csharp
Android.Renderscripts.Allocation outputAllocation = Allocation.CreateTyped(renderScript, inputAllocation.Type);
```

スクリプト**ラッパーをインスタンス化**する組み込みの各スクリプトラッパークラス &ndash;、そのスクリプトのラッパーオブジェクトをインスタンス化するためのヘルパーメソッド (通常は `Create`) が必要です。 次のコードスニペットは、`ScriptIntrinsicBlur` ぼかすオブジェクトをインスタンス化する方法の例です。 `Element.U8_4` のヘルパーメソッドは、8ビットの符号なし整数値の4つのフィールドであるデータ型を記述する要素を作成します。これは、`Bitmap` オブジェクトのデータを保持するのに適しています。

```csharp
Android.Renderscripts.ScriptIntrinsicBlur blurScript = ScriptIntrinsicBlur.Create(renderScript, Element.U8_4(renderScript));
```

Allocation スクリプトを実際に実行するには、**割り当て、パラメーターの設定、& 実行スクリプト**&ndash; `Script` クラスによって `ForEach` メソッドが提供されます。 このメソッドは、入力データを保持している `Allocation` 内の各 `Element` を反復処理します。 場合によっては、出力を保持する `Allocation` の提供が必要になることがあります。
`ForEach` によって出力割り当ての内容が上書きされます。 前の手順のコードスニペットを使用するには、この例では、入力割り当てを割り当てる方法、パラメーターを設定する方法、最後にスクリプトを実行する方法 (結果を出力割り当てにコピーする方法) について説明します。

```csharp
blurScript.SetInput(inputAllocation);
blurScript.SetRadius(25);  // Set a pamaeter
blurScript.ForEach(outputAllocation);
```

「 [Renderscript を使用](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/drawing/blur_an_image_with_renderscript)したイメージのぼかし」レシピを調べることもできます。これは、Xamarin の組み込みスクリプトを使用する方法の完全な例です。

## <a name="summary"></a>まとめ

このガイドでは、Renderscript と、それを Xamarin Android アプリケーションで使用する方法を紹介しました。 ここでは、Renderscript の概要と、それが Android アプリケーションでどのように機能するかについて簡単に説明しました。 ここでは、Renderscript の主要なコンポーネントと、_ユーザースクリプト_と_ているかスクリプト_の違いについて説明しています。 最後に、このガイドでは、「Xamarin Android アプリケーションでの組み込みスクリプトの使用」の手順について説明しました。

## <a name="related-links"></a>関連リンク

- [Android. Renderscripts 名前空間](xref:Android.Renderscripts)
- [Renderscript を使用したイメージのぼかし](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/drawing/blur_an_image_with_renderscript)
- [Renderscript](https://developer.android.com/guide/topics/renderscript/compute.html)
- [チュートリアル: Renderscript を使用したはじめに](https://software.intel.com/en-us/articles/renderscript-basic-sample-for-android-os)
