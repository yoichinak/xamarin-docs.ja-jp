---
title: クロスプラットフォーム アプリでのネイティブ型の使用
description: この記事では、Android や Windows Phone Os iOS 以外のデバイスでコードが共有されているクロス プラットフォーム アプリケーションで新しい iOS Unified API をネイティブ型 (nint、nuint、nfloat) を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: B9C56C3B-E196-4ADA-A1DE-AC10D1001C2A
author: asb3993
ms.author: amburns
ms.date: 04/07/2016
ms.openlocfilehash: 489d2a76e6eff661360b24d1872ed1343c74b85e
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57667218"
---
# <a name="working-with-native-types-in-cross-platform-apps"></a>クロスプラットフォーム アプリでのネイティブ型の使用

_この記事では、Android や Windows Phone Os iOS 以外のデバイスでコードが共有されているクロス プラットフォーム アプリケーションで新しい iOS Unified API をネイティブ型 (nint、nuint、nfloat) を使用する方法について説明します。_


64 種類のネイティブ型、iOS と Mac の Api を使用します。 Android または Windows 上でも実行する共有コードを記述する場合は、共有できる通常の .NET 型への統合型の変換を管理する必要があります。

このドキュメントでは、共有/共通コードから Unified API との相互運用するさまざまな方法について説明します。

## <a name="when-to-use-the-native-types"></a>ネイティブ型を使用する場合

Xamarin.iOS および Xamarin.Mac Unified Api がまだが含まれて、 `int`、`uint`と`float`、データ型だけでなく`RectangleF`、`SizeF`と`PointF`型。 これらの既存のデータ型は、任意の共有、クロスプラット フォームのコードで使用する続行する必要があります。 新しいネイティブ データ型は、アーキテクチャに対応した型は、必要なをサポートしている Mac または iOS API への呼び出しを行う場合にのみ使用する必要があります。

によって、共有されているコードの性質がありますクロス プラットフォームのコードが処理する必要があります、 `nint`、`nuint`と`nfloat`データ型。 例: 以前を使用して四角形のデータの変換を処理するライブラリ`System.Drawing.RectangleF`Xamarin.iOS および Xamarin.Android のバージョン、アプリの間での機能を共有する、iOS のネイティブ型を処理するために更新する必要があります。

コードを共有の形式が使用されている次のセクションで紹介するようおよびサイズと複雑さ、アプリケーションのこれらの変更の処理方法によって異なります。

## <a name="code-sharing-considerations"></a>コードの共有に関する考慮事項

説明したように、[コードの共有オプション](~/cross-platform/app-fundamentals/code-sharing.md)を文書化、クロス プラットフォーム プロジェクト間でコードを共有する 2 つの主な方法があります。プロジェクトとポータブル クラス ライブラリを共有します。 2 つの種類のどれが使用されている、クロス プラットフォームのコードのネイティブ データ型を処理するときのオプションが制限されます。

### <a name="portable-class-library-projects"></a>ポータブル クラス ライブラリ プロジェクト

ポータブル クラス ライブラリ (PCL) では、サポート、およびプラットフォーム固有の機能を提供するインターフェイスを使用するプラットフォームをターゲットにできます。

PCL プロジェクトの種類が下にコンパイルされた後、`.DLL`と Unified API の意味を持たない、既存のデータ型を引き続き使用することが必要あります (`int`、 `uint`、 `float`)、PCL にソース コードと型キャスト、Pcl への呼び出しクラスとフロント エンド アプリケーションでのメソッドです。 たとえば、次のように入力します。

```csharp
using NativePCL;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

### <a name="shared-projects"></a>共有プロジェクト

共有アセット プロジェクトの種類を使用すると、個々 のプラットフォームに固有のフロント エンドのアプリにし、取得に含まれるおよびコンパイル別のプロジェクトでソース コードを整理および使用`#if`を管理するために、コンパイラ ディレクティブプラットフォームに固有の要件。

サイズと複雑さ、前面のサイズと共有されているコードの複雑さの共有コードを使用しているでのクロス プラットフォームの種類のネイティブ データのサポート方法を選択するときに考慮する必要があるモバイル アプリケーションを終了します共有アセット プロジェクトの種類。

これらの要因に基づき、次の種類のソリューションの実装を使用して、`if __UNIFIED__ ... #endif`コンパイラ ディレクティブをコードを Unified API の特定の変更を処理します。

#### <a name="using-duplicate-methods"></a>重複するメソッドを使用してください。

上で指定した四角形のデータの変換を行っているライブラリの例を実行します。 ライブラリがのみ 1 つまたは 2 つの非常に単純なメソッドが含まれる場合は、Xamarin.iOS と Xamarin.Android のこれらのメソッドの複製バージョンを作成することもできます。 例:

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

上記のコードであるため、`CalculateArea`ルーチンは非常に単純で、条件付きコンパイルを使用し、個別に Unified API バージョンのメソッドを作成しました。 その一方で場合は、ライブラリには、多くのルーチンまたはいくつかの複雑なルーチンが含まれている、このソリューションことはできません、すべてのメソッドの変更やバグ修正の同期を保つ問題があるとします。

#### <a name="using-method-overloads"></a>オーバー ロード メソッドを使用して

その場合は、ソリューションが、32 ビットのデータ型を使用して、今すぐ実行されているようにメソッドのオーバー ロード バージョンを作成するあります`CGRect`パラメーターまたは戻り値を変換するには、その値を`RectangleF`(から変換することを知ること`nfloat`に。`float`損失を伴う変換)、元のバージョンの実際の作業を行うルーチンを呼び出します。 例:

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

ここでも、有効桁数の損失がアプリケーションの特定のニーズに応じて結果に影響しない限り、最適なソリューションになります。

#### <a name="using-alias-directives"></a>Using エイリアス ディレクティブ

別の解決策は、精度の低下の問題がある領域を使用する`using`共有ソース コード ファイルの先頭に次のコードをなどによって、いずれかに変換するネイティブおよび CoreGraphics データ型のエイリアスを作成するディレクティブ必要な`int`、`uint`または`float`値`nint`、`nuint`または`nfloat`:

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

コードの例になります。

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

注意が変更されましたここで、`CalculateArea`メソッドの戻り値、`nfloat`標準ではなく`float`します。 しようとして、コンパイル エラーが発生しましたが受けようにこれは_暗黙的に_変換、`nfloat`の計算結果 (乗算される両方の値があるため`nfloat`) に、`float`値を返します。

コードがコンパイルされ、非 Unified API デバイスで実行する場合、`using nfloat = global::System.Single;`マップ、`nfloat`を`Single`に暗黙的に変換を`float`、コンシューマー側のフロント エンド アプリケーションを呼び出すことができます、`CalculateArea`メソッドなし変更します。


#### <a name="using-type-conversions-in-the-front-end-app"></a>フロント エンド アプリでの型変換の使用

のみ、フロント エンド アプリケーションするいくつかの共有コード ライブラリへの呼び出しには、ある別のソリューションは変更なし、ライブラリのままにすることし、既存のルーチンを呼び出すときに、Xamarin.iOS または Xamarin.Mac アプリケーションでのキャストを入力しないでください。 たとえば、次のように入力します。

```csharp
using NativeShared;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

コンシューマー側アプリケーションが数百の共有コード ライブラリへの呼び出しを行った場合これをもう一度できない可能性があります、最適なソリューション。

アプリケーションのアーキテクチャに基づく、可能性がありますなります上記ソリューションの 1 つ以上を使用してネイティブ データをサポートするクロス プラットフォーム コードで (必要な場合) を入力します。


## <a name="xamarinforms-applications"></a>Xamarin.Forms アプリケーション

次が必要ですが、Unified API アプリケーションとも共有されるクロス プラットフォームの Ui に Xamarin.Forms を使用します。

- バージョン 1.3.1 ソリューション全体を使用する必要があります (またはそれ以上) の Xamarin.Forms NuGet パッケージ。
- Xamarin.iOS カスタム レンダリング共有 (共有プロジェクトまたは PCL) UI コードを使用された方法に基づいてソリューションが前述の同じ型を使用します。

標準的なクロス プラットフォーム アプリケーションでは、ように既存の 32 ビットのデータ型使用してください、クロス プラットフォームの共有コードではほとんどすべての状況に。 新しいネイティブ データ型は、アーキテクチャに対応した型は、必要なをサポートしている Mac または iOS API への呼び出しを行う場合にのみ使用する必要があります。

詳細については、次を参照してください。 この[既存の Xamarin.Forms アプリの更新](https://developer.xamarin.com/guides/cross-platform/macios/updating-xamarin-forms-apps/)ドキュメント。

## <a name="summary"></a>まとめ

この記事では、Unified API アプリケーションとその影響クロスプラット内のネイティブ データ型に使用するときに「」を参照があります。 クロス プラットフォーム ライブラリで、新しいネイティブ データ型を使用する必要がありますの状況で使用できるいくつかのソリューションを紹介しました。 Unified Api をサポートしている Xamarin.Forms のクロスプラット フォーム対応のアプリケーションでのクイック ガイドを説明しました。



## <a name="related-links"></a>関連リンク

- [Unified API](~/cross-platform/macios/unified/index.md)
- [ネイティブ型](~/cross-platform/macios/nativetypes.md)
- [コード共有のオプション](~/cross-platform/app-fundamentals/code-sharing.md)
- [コード共有のサンプル](https://developer.xamarin.com/samples/mobile/SharingCode/)
