---
title: クロスプラットフォーム アプリでのネイティブ型の使用
description: この記事では、Android や Windows Phone Os などの iOS 以外のデバイスでコードを共有するクロスプラットフォームアプリケーションで、新しい iOS Unified API ネイティブ型 (nint、nuint、nint) を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: B9C56C3B-E196-4ADA-A1DE-AC10D1001C2A
author: davidortinau
ms.author: daortin
ms.date: 04/07/2016
ms.openlocfilehash: 4819f8fc88a8a2c730fc25215e862d68cc5bd643
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457290"
---
# <a name="working-with-native-types-in-cross-platform-apps"></a>クロスプラットフォーム アプリでのネイティブ型の使用

_この記事では、Android や Windows Phone Os などの iOS 以外のデバイスでコードを共有するクロスプラットフォームアプリケーションで、新しい iOS Unified API ネイティブ型 (nint、nuint、nint) を使用する方法について説明します。_

64型のネイティブ型は、iOS および Mac Api と連携して動作します。 Android または Windows で実行される共有コードを記述する場合は、統合型を共有できる通常の .NET 型に変換することを管理する必要があります。

このドキュメントでは、共有/共通コードから Unified API と相互運用するさまざまな方法について説明します。

## <a name="when-to-use-the-native-types"></a>ネイティブ型を使用する場合

Xamarin と Xamarin の統合 Api には、、、およびの各 `int` `uint` `float` データ型に加えて `RectangleF` `SizeF` 、、 `PointF` 、およびの各型も含まれます。 これらの既存のデータ型は、すべての共有のクロスプラットフォームコードで引き続き使用できます。 新しいネイティブデータ型は、アーキテクチャ対応型のサポートが必要な場合に、Mac または iOS API の呼び出しを行うときにのみ使用してください。

共有されているコードの性質によっては、クロスプラットフォームコードが、、およびの各データ型を処理する必要がある場合があり `nint` `nuint` `nfloat` ます。 たとえば、Xamarin と Xamarin の間で機能を共有するために以前に使用していた四角形のデータの変換を処理するライブラリは `System.Drawing.RectangleF` 、ios でネイティブ型を処理するように更新する必要があります。

これらの変更がどのように処理されるかは、アプリケーションのサイズと複雑さ、および使用されているコード共有の形式によって異なります。次のセクションで説明します。

## <a name="code-sharing-considerations"></a>コード共有に関する考慮事項

コード共有の [オプションに関する](~/cross-platform/app-fundamentals/code-sharing.md) ドキュメントに記載されているように、クロスプラットフォームプロジェクト間でコードを共有するには、共有プロジェクトとポータブルクラスライブラリの2つの主な方法があります。 2種類のうちどちらが使用されているかは、クロスプラットフォームコードでネイティブデータ型を処理するときのオプションを制限します。

### <a name="portable-class-library-projects"></a>ポータブルクラスライブラリプロジェクト

ポータブルクラスライブラリ (PCL) を使用すると、サポートするプラットフォームをターゲットにすることができ、インターフェイスを使用してプラットフォーム固有の機能を提供することができます。

Pcl プロジェクトの種類はにコンパイルされ、 `.DLL` Unified API は意味がないため、既存のデータ型 (、、) を pcl ソースコードで使用し続けることが強制され `int` `uint` `float` ます。また、「」と入力すると、pcl のクラスおよびメソッドへの呼び出しがフロントエンドアプリケーションにキャストされます。 次に例を示します。

```csharp
using NativePCL;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

### <a name="shared-projects"></a>共有プロジェクト

Shared Asset プロジェクトの種類を使用すると、ソースコードを別のプロジェクトで整理して、個々のプラットフォーム固有のフロントエンドアプリに追加してコンパイルし、 `#if` プラットフォーム固有の要件を管理するために必要なコンパイラディレクティブを使用することができます。

共有コードを使用しているフロントエンドモバイルアプリケーションのサイズと複雑さは、共有されているコードのサイズと複雑さに応じて、クロスプラットフォームの共有アセットプロジェクトでネイティブデータ型をサポートする方法を選択する際に考慮する必要があります。

これらの要因に基づいて、次の種類のソリューションをコンパイラディレクティブを使用して実装し、 `if __UNIFIED__ ... #endif` コードに対する Unified API 固有の変更を処理することができます。

#### <a name="using-duplicate-methods"></a>重複するメソッドの使用

上で指定した四角形のデータに対して変換を実行しているライブラリの例を見てください。 ライブラリに非常に単純なメソッドが1つまたは2つしか含まれていない場合は、Xamarin および Xamarin Android 用に重複したバージョンのメソッドを作成することもできます。 次に例を示します。

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
using CoreGraphics;
#endif

namespace NativeShared
{
    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        #if __UNIFIED__
            public static nfloat CalculateArea(CGRect rect) {

                // Calculate area...
                return (rect.Width * rect.Height);

            }
        #else
            public static float CalculateArea(RectangleF rect) {

                // Calculate area...
                return (rect.Width * rect.Height);

            }
        #endif
        #endregion
    }
}
```

上記のコードで `CalculateArea` は、ルーチンが非常に単純であるため、条件付きコンパイルを使用して、メソッドの別の Unified API バージョンを作成しました。 一方、ライブラリに多数のルーチンまたは複数の複雑なルーチンが含まれている場合、このソリューションは実現できません。これは、すべてのメソッドが変更またはバグ修正のために同期されるという問題が生じるためです。

#### <a name="using-method-overloads"></a>メソッドオーバーロードの使用

このような場合、ソリューションでは、32ビットのデータ型を使用してメソッドのオーバーロードバージョンを作成して、 `CGRect` パラメーターや戻り値として取得し、その値をに変換し `RectangleF` ます (からへの変換 `nfloat` が可逆変換であることを認識 `float` します)。また、実際の作業を行うには、ルーチンの元のバージョンを呼び出します 次に例を示します。

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
using CoreGraphics;
#endif

namespace NativeShared
{
    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        #if __UNIFIED__
            public static nfloat CalculateArea(CGRect rect) {

                // Call original routine to calculate area
                return (nfloat)CalculateArea((RectangleF)rect);

            }
        #endif

        public static float CalculateArea(RectangleF rect) {

            // Calculate area...
            return (rect.Width * rect.Height);

        }

        #endregion
    }
}

```

ここでも、精度の低下がアプリケーション固有のニーズの結果に影響を与えない限り、これは優れたソリューションです。

#### <a name="using-alias-directives"></a>Using エイリアスディレクティブ

精度が低下している領域の場合、別の解決策として、ディレクティブを使用し `using` て、共有ソースコードファイルの先頭に次のコードを追加し、必要な `int` 、 `uint` または `float` 値をに変換し `nint` `nuint` `nfloat` ます。

```csharp
#if __UNIFIED__
    // Mappings Unified CoreGraphic classes to MonoTouch classes
    using RectangleF = global::CoreGraphics.CGRect;
    using SizeF = global::CoreGraphics.CGSize;
    using PointF = global::CoreGraphics.CGPoint;
#else
    // Mappings Unified types to MonoTouch types
    using nfloat = global::System.Single;
    using nint = global::System.Int32;
    using nuint = global::System.UInt32;
#endif
```

コード例は次のようになります。

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
    // Map Unified CoreGraphic classes to MonoTouch classes
    using RectangleF = global::CoreGraphics.CGRect;
    using SizeF = global::CoreGraphics.CGSize;
    using PointF = global::CoreGraphics.CGPoint;
#else
    // Map Unified types to MonoTouch types
    using nfloat = global::System.Single;
    using nint = global::System.Int32;
    using nuint = global::System.UInt32;
#endif

namespace NativeShared
{

    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        public static nfloat CalculateArea(RectangleF rect) {

            // Calculate area...
            return (rect.Width * rect.Height);

        }
        #endregion
    }
}
```

ここでは、標準では `CalculateArea` なくを返すようにメソッドを変更したことに注意して `nfloat` `float` ください。 これは、 _implicitly_ `nfloat` (両方の値が乗算されるため) の計算結果を `nfloat` 戻り値に暗黙的に変換しようとしたときに、コンパイルエラーが発生しないようにするためです `float` 。

コードをコンパイルして非 Unified API デバイスで実行すると、はをにマップします `using nfloat = global::System.Single;` `nfloat` 。これにより、を使用している `Single` `float` フロントエンドアプリケーションが `CalculateArea` 変更なしでメソッドを呼び出すことができるようにに暗黙的に変換されます。

#### <a name="using-type-conversions-in-the-front-end-app"></a>フロントエンドアプリでの型変換の使用

フロントエンドアプリケーションが共有コードライブラリへの呼び出しをごく一部しか行わない場合、別の解決策として、ライブラリを変更せずに、既存のルーチンを呼び出すときに Xamarin または Xamarin. Mac アプリケーションで型キャストを行うことができます。 次に例を示します。

```csharp
using NativeShared;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

使用しているアプリケーションが共有コードライブラリに対して何百もの呼び出しを行う場合は、この方法が適切でない可能性があります。

アプリケーションのアーキテクチャに基づいて、これらのソリューションの1つ以上を使用して、クロスプラットフォームコードでネイティブデータ型 (必要な場合) をサポートする場合があります。

## <a name="xamarinforms-applications"></a>Xamarin. フォームアプリケーション

次に示すのは、Unified API アプリケーションと共有されるクロスプラットフォームの Ui に対して Xamarin. Forms を使用する場合です。

- ソリューション全体で、1.3.1 NuGet パッケージのバージョン (またはそれ以降) を使用している必要があります。
- すべての Xamarin のカスタムレンダリングでは、UI コードの共有 (共有プロジェクトまたは PCL) に基づいて、上記と同じ種類のソリューションを使用します。

標準的なクロスプラットフォームアプリケーションと同様に、既存の32ビットデータ型は、ほとんどの状況において、すべての共有のクロスプラットフォームコードで使用する必要があります。 新しいネイティブデータ型は、アーキテクチャ対応型のサポートが必要な場合に、Mac または iOS API の呼び出しを行うときにのみ使用してください。

詳細については、 [既存の Xamarin. Forms アプリの更新](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md) に関するドキュメントを参照してください。

## <a name="summary"></a>まとめ

この記事では、Unified API アプリケーションでネイティブデータ型を使用する場合と、それによるクロスプラットフォームの影響について説明しました。 クロスプラットフォームライブラリで新しいネイティブデータ型を使用する必要がある場合に使用できるいくつかのソリューションを提供しました。 また、Xamarin. Forms クロスプラットフォームアプリケーションで統合された Api をサポートするための簡単なガイドを見てきました。

## <a name="related-links"></a>関連リンク

- [Unified API](~/cross-platform/macios/unified/index.md)
- [ネイティブ型](~/cross-platform/macios/nativetypes.md)
- [コード共有のオプション](~/cross-platform/app-fundamentals/code-sharing.md)
- [コード共有のサンプル](/samples/xamarin/mobile-samples/sharingcode/)