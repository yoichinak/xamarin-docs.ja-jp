---
title: "クロスプラット フォーム アプリでネイティブ型の使用"
description: "この記事では、コードが Android や Windows Phone Os などの非 iOS デバイスと共有されているクロス プラットフォーム アプリケーションで新しい iOS Unified API をネイティブ型 (nint、nuint、nfloat) の使用について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: B9C56C3B-E196-4ADA-A1DE-AC10D1001C2A
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/07/2016
ms.openlocfilehash: 2e177afa9124095f00edacbeb71512d5cd9bb219
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-native-types-in-cross-platform-apps"></a>クロスプラット フォーム アプリでネイティブ型の使用

_この記事では、コードが Android や Windows Phone Os などの非 iOS デバイスと共有されているクロス プラットフォーム アプリケーションで新しい iOS Unified API をネイティブ型 (nint、nuint、nfloat) の使用について説明します。_


64 型のネイティブ型は、iOS および Mac の Api を使用します。 同様に、Android や Windows で実行されている共有コードを記述する場合は、共有できる通常の .NET 型に統合型の変換を管理する必要があります。

このドキュメントでは、広く共有コードから Unified API との相互運用するさまざまな方法について説明します。

## <a name="when-to-use-the-native-types"></a>ネイティブ型を使用します。

Xamarin.iOS および Xamarin.Mac Unified Api がまだが含まれて、 `int`、`uint`と`float`データ型だけでなく`RectangleF`、`SizeF`と`PointF`型です。 これらの既存のデータ型は、任意の共有、クロスプラット フォームのコードで使用する続行する必要があります。 新しいネイティブ データ型は、アーキテクチャに対応する型は、必要なをサポートしている Mac または iOS API への呼び出しを行う場合にのみ使用する必要があります。

共有されているコードの性質、によってありますクロスプラット フォームのコードに対処する必要があります、 `nint`、`nuint`と`nfloat`データ型。 例: が以前使っていた四角形のデータの変換を処理するライブラリ`System.Drawing.RectangleF`Xamarin.iOS および Xamarin.Android バージョン、アプリの間で機能を共有すると iOS のネイティブ型を処理するように更新する必要があります。

サイズと、アプリケーションの複雑さによって異なりますこれらの変更の処理方法と、次のセクションで紹介するようコードを共有の形式が使用されてです。

## <a name="code-sharing-considerations"></a>コード共有の考慮事項

説明したように、[コードの共有オプション](~/cross-platform/app-fundamentals/code-sharing.md)ドキュメントで、クロス プラットフォーム プロジェクト間でコードを共有する 2 つの主な方法: 共有プロジェクト、ポータブル クラス ライブラリです。 次の 2 種類が使用されて、クロスプラット フォーム コードのネイティブ データ型を処理するときのオプションが制限されます。

### <a name="portable-class-library-projects"></a>ポータブル クラス ライブラリ プロジェクト

ポータブル クラス ライブラリ (PCL) は、サポート、およびプラットフォーム固有の機能を提供するインターフェイスを使用するプラットフォームをターゲットにできます。

PCL プロジェクトの種類が下にコンパイルされた後、 `.DLL` Unified API の意味がないと、既存のデータ型の使用を継続することが必要あります (`int`、 `uint`、 `float`)、PCL にソース コードと型キャスト、Pcl への呼び出しクラスとフロント エンド アプリケーションでのメソッドです。 たとえば、次のように入力します。

```csharp
using NativePCL;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

### <a name="shared-projects"></a>共有プロジェクト

共有アセット プロジェクトの種類を使用すると、個々 のプラットフォーム固有のフロント エンドのアプリにし、取得含まれているおよびコンパイルされる別のプロジェクトで、ソース コードを整理および使用`#if`を管理するために、コンパイラ ディレクティブプラットフォームに固有の要件です。

サイズと前の複雑さは、モバイル アプリケーションをサイズ、および共有されているコードの複雑さとの共有コードを消費している、プラットフォーム間での種類のネイティブ データのサポート方法の選択時に考慮する必要がありますを終了します共有アセット プロジェクトの種類。

これらの要因に基づき、次の種類のソリューションが実装されているを使用して、`if __UNIFIED__ ... #endif`コンパイラ ディレクティブをコードに Unified API の特定の変更を処理します。

#### <a name="using-duplicate-methods"></a>重複したメソッドを使用します。

上記四角形のデータの変換を行っているライブラリの例を実行します。 ライブラリがのみ 1 つまたは 2 つの非常に単純なメソッドが含まれる場合は、Xamarin.iOS および Xamarin.Android をこれらのメソッドの複製バージョンを作成することもできます。 例:

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

上記のコードでので、`CalculateArea`ルーチンは非常に単純なが条件付きコンパイルを使用して別の統合 API のバージョンのメソッドを作成します。 その一方で場合は、ライブラリには、多くのルーチンまたはいくつかの複雑なルーチンが含まれている、このソリューションはありません実行不可能なメソッドのすべての変更またはバグの修正の同期を保つの問題があるとします。

#### <a name="using-method-overloads"></a>オーバー ロード メソッドを使用して

その場合は、ソリューションがありますをできるようにここでは、32 ビット データ型を使用してメソッドのオーバー ロード バージョンを作成`CGRect`パラメーターまたは戻り値、として変換するには、その値、 `RectangleF` (から変換することを知ること`nfloat`に`float`損失を伴う変換が)、実際の作業を実行するルーチンの元のバージョンを呼び出します。 例:

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

もう一度、良いソリューションは、有効桁数の損失が、アプリケーションの特定のニーズに応じて結果に影響しない限り、これです。

#### <a name="using-alias-directives"></a>Using エイリアス ディレクティブ

別の解決策は、ここで、精度の低下は問題領域を使用する`using`を共有ソース コード ファイルの先頭に次のコードを含めて、いずれかに変換するネイティブおよび CoreGraphics データ型のエイリアスを作成するディレクティブ必要な`int`、`uint`または`float`値`nint`、`nuint`または`nfloat`:

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

できるように、サンプル コードになります。

```csharp
using System;
using System.Drawing;

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

ここに変更しておきます、`CalculateArea`メソッドの戻り値、`nfloat`標準ではなく`float`です。 これは、操作が実行しようとして、コンパイル エラーになりましていないように_暗黙的に_変換、 `nfloat` 、計算の結果 (両方の値が乗算されるので`nfloat`) に、`float`値を返します。

コードがコンパイルされ、非統合 API のデバイスで実行する場合、`using nfloat = global::System.Single;`マップ、`nfloat`を`Single`これは暗黙的に変換し、`float`呼び出しに利用するフロント エンド アプリケーションを許可する、`CalculateArea`せずメソッド変更します。


#### <a name="using-type-conversions-in-the-front-end-app"></a>フロント エンド アプリケーションでの型変換の使用

イベントで、フロント エンド アプリケーションでは、共有コード ライブラリへの呼び出しのほんの一部しか利用、別のソリューションは変更されていないライブラリのままにすることができ、既存のルーチンを呼び出すときに、Xamarin.iOS または Xamarin.Mac アプリケーションでのキャストを入力しないでください。 たとえば、次のように入力します。

```csharp
using NativeShared;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

処理を行うアプリケーション数百台、共有コード ライブラリへの呼び出しの場合、このもう一度、できない可能性があります良いソリューションです。

アプリケーションのアーキテクチャに基づく、可能性があります結局はネイティブ データをサポートするために、上記の解決策の 1 つ以上を使用してクロスプラット フォーム コードの種類と (必須) where です。


## <a name="xamarinforms-applications"></a>Xamarin.Forms アプリケーション

次は Unified API アプリケーションとも共有されるクロスプラット フォームの Ui に Xamarin.Forms を使用する必要です。

- ソリューション全体を使用するバージョン 1.3.1 に指定する必要があります (またはそれ以上) の Xamarin.Forms NuGet パッケージです。
- Xamarin.iOS カスタム レンダリングの共有 (共有プロジェクトまたは PCL) UI コードを使用した方法に基づいて、同じ種類の上に示したソリューションを使用します。

クロスプラット フォームの標準のアプリケーションと同様に、既存の 32 ビット データ型を付ける、プラットフォーム間で共有コードではほとんどすべての状況にします。 新しいネイティブ データ型は、アーキテクチャに対応する型は、必要なをサポートしている Mac または iOS API への呼び出しを行う場合にのみ使用する必要があります。

詳細については、次を参照してください。 この[既存 Xamarin.Forms アプリの更新](http://developer.xamarin.com/guides/cross-platform/macios/updating-xamarin-forms-apps/)ドキュメント。

## <a name="summary"></a>まとめ

この記事で Unified API アプリケーションとその影響クロス プラットフォームのネイティブ データ型を使用するときに「」を参照があります。 クロスプラット フォーム ライブラリで、新しいネイティブ データ型を使用する必要がありますの状況で使用できるいくつかのソリューションを提供しました。 クイック ガイド Xamarin.Forms クロスプラット フォームのアプリケーションで Api の統合をサポートする説明しました。



## <a name="related-links"></a>関連リンク

- [Unified API](~/cross-platform/macios/unified/index.md)
- [ネイティブ型](~/cross-platform/macios/nativetypes.md)
- [コード共有のオプション](~/cross-platform/app-fundamentals/code-sharing.md)
- [コード共有のサンプル](https://developer.xamarin.com/samples/mobile/SharingCode/)
