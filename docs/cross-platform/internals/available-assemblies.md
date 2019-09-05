---
title: 使用できるアセンブリ
description: このドキュメントでは、Xamarin、Xamarin、および Xamarin. Mac で使用できるアセンブリについて説明します。 また、.NET Standard ライブラリやポータブルクラスライブラリに関するドキュメントへのリンクもあります。
ms.prod: xamarin
ms.assetid: AEF4ED0E-391F-4FA4-9F18-842BC24C272D
author: conceptdev
ms.author: crdun
ms.date: 03/13/2018
ms.openlocfilehash: 3d3524a0f99d78fc356dc7c4bb3e3aca1940a718
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70278446"
---
# <a name="available-assemblies"></a>使用できるアセンブリ

Xamarin.iOS、Xamarin.Android、および Xamarin.Mac、には十数個のアセンブリがすべて付属します。 Silverlight が、デスクトップの .NET アセンブリの拡張サブセットであるのと同様に、 Xamarin プラットフォームも、いくつかの Silverlight とデスクトップの .NET アセンブリの拡張のサブセットです。

Xamarin プラットフォームは、別のプロファイル用にコンパイルされた既存のアセンブリとの ABI との互換性がありません。 ソースコードを再コンパイルして、正しいプロファイルを対象とするアセンブリを生成する必要があります (Silverlight と .NET 3.5 を個別に再コンパイルする必要があるのと同じです)。

Xamarin アプリケーションは3つのモードでコンパイルできます。1つは Xamarin の curated Mobile プロファイルを使用するモードで、もう1つは、既存の完全なデスクトップアセンブリをターゲットにすることを可能にする Xamarin .NET 4.5 フレームワーク、もう1つはシステム Mono の .NET API を使用する、サポートされていないモードです。スト. 詳細については、[ターゲットフレームワーク](~/mac/platform/target-framework.md)のドキュメントを参照してください。

## <a name="net-standard-libraries"></a>.NET Standard ライブラリ

IOS、Android、Mac の各バインドに加えて、Xamarin プロジェクトは[.NET Standard ライブラリ](~/cross-platform/app-fundamentals/net-standard.md)を使用できます。

## <a name="portable-class-libraries"></a>ポータブル クラス ライブラリ

Xamarin プロジェクトは、 [.Net ポータブルクラスライブラリ](~/cross-platform/app-fundamentals/pcl.md)を使用することもできます。ただし、このテクノロジは .NET Standard を優先するため非推奨とされます。

## <a name="supported-assemblies"></a>サポートされているアセンブリ

これらのアセンブリは、 **Reference Manager > assemblies > Framework** (Visual Studio 2017) および**編集参照 > パッケージ**(Visual Studio for Mac) で使用でき、Xamarin プラットフォームとの互換性があります。

> [!div class="mx-tdCol2BreakAll"]
> |Assembly|API の互換性|Xamarin iOS|Xamarin Android|Xamarin Mac|
> |--------|-----------------|-----------|---------------|-----------|
> |FSharp.Core.dll| |✓|✓|✓|
> |l18N.dll|CJK、MidEast、Other、まれ、西を含みます|✓|✓|✓|
> |Microsoft.CSharp.dll| |✓|✓|✓|
> |Mono.CSharp.dll| |✓|✓|✓|
> |Mono.Data.Sqlite.dll|SQLite 用 ADO.NET プロバイダー「制限事項」を参照してください。|✓|✓|✓|
> |Mono.Data.Tds.dll|TDS プロトコルのサポートsystem.string 内の[system.string サポートに](xref:System.Data.SqlClient)使用[され](xref:System.Data)ます。|✓|✓|✓|
> |Mono.Dynamic.&#8203;Interpreter.dll| |✓| | |
> |Mono.Security.dll|暗号化 Api。|✓|✓|✓|
> |monotouch.dll|このアセンブリにはC# 、CocoaTouch API へのバインドが含まれています。 これは、クラシック iOS プロジェクト内でのみ使用できます。|✓| | |
> |MonoTouch.&#8203;Dialog-1.dll| |✓| | |
> |MonoTouch.&#8203;NUnitLite.dll| |✓| | |
> |mscorlib.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |OpenTK-1.0.dll|OpenGL/OpenAL オブジェクト指向 Api。 iPhone デバイスサポートを提供するために拡張されています。|✓|✓|✓|
> |System.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)に加えて、次の名前空間の型があります。<br />System.Collections.Specialized<br />System.&#8203;ComponentModel<br />System.ComponentModel.Design<br />System.Diagnostics<br />System.IO<br />System.IO.Compression<br />System.IO.Compression.FileSystem<br />System.Net<br />System.Net.Cache<br />System.Net.Mail<br />System.Net.Mime<br />System.Net.&#8203;NetworkInformation<br />System.Net.Security<br />System.Net.Sockets<br />System.Runtime.&#8203;InteropServices<br />System.Runtime.Versioning<br />System.Security.&#8203;AccessControl<br />System.Security.Authentication<br />System.Security.&#8203;Cryptography<br />System.Security.Permissions<br />System.Threading<br />System.Timers|✓|✓|✓|
> |System.&#8203;ComponentModel.&#8203;Composition.dll| |✓|✓|✓|
> |System.&#8203;ComponentModel.&#8203;DataAnnotations.dll| |✓|✓|✓|
> |System.Core.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Data.dll|[.Net 3.5](https://msdn.microsoft.com/library/ms229335.aspx) 。[一部の機能が削除されまし](~/ios/data-cloud/system.data.md)た。|✓|✓|✓|
> |System.Data.&#8203;Services.&#8203;Client.dll|完全な oData クライアント。|✓|✓|✓|
> |System.IO.&#8203;Compression| |✓|✓|✓|
> |System.IO.&#8203;Compression.&#8203;FileSystem| |✓|✓|✓|
> |System.Json.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Net.&#8203;Http.dll| |✓|✓|✓|
> |System.&#8203;Numerics.dll| |✓|✓|✓|
> |System.Runtime.&#8203;Serialization.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.&#8203;ServiceModel.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)に存在する WCF スタック|✓|✓|✓|
> |System.&#8203;ServiceModel.&#8203;Internals.dll| |✓|✓|✓|
> |System.&#8203;ServiceModel.&#8203;Web.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)に加えて、次の名前空間の型があります。 <br />システム<br />System.ServiceModel.Channels<br />System.ServiceModel.Description<br />System.ServiceModel.Web|✓|✓|✓|
> |System.&#8203;Transactions.dll|[.Net 3.5](https://msdn.microsoft.com/library/ms229335.aspx);[システムデータ](~/ios/data-cloud/system.data.md)のサポートの一部。|✓|✓|✓|
> |System.Web.&#8203;Services.dll|.NET 3.5 プロファイルの基本的な Web サービスです。サーバー機能は削除されています。|✓|✓|✓|
> |System.&#8203;Windows.dll| |✓|✓|✓|
> |System.&#8203;Xml.dll|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.&#8203;Linq.dll|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.Serialization.dll| |✓|✓|✓|
> |Xamarin.iOS.dll|このアセンブリにはC# 、CocoaTouch API へのバインドが含まれています。 これは、統合された iOS プロジェクトでのみ使用されます。|✓| | |
> |Java.Interop.dll| | |✓| |
> |Mono.Android.dll| | |✓| |
> |Mono.Android.&#8203;Export.dll| | |✓| |
> |Mono.Posix.dll| | |✓| |
> |System.&#8203;EnterpriseServices.dll| | |✓| |
> |Xamarin.Android.&#8203;NUnitLite.dll| | |✓| |
> |Mono.CompilerServices.&#8203;SymbolWriter.dll|コンパイラライターの場合。| | |✓|
> |Xamarin.Mac.dll| | | |✓|
> |System.&#8203;Drawing.dll|Xamarin. Mac、.NET 4.5、またはモバイルフレームワークの Unified API では、Drawing はサポートされていません。 システムの描画サポートは、 [sysdrawing coregraphics](https://github.com/mono/sysdrawing-coregraphics) library を使用して IOS および macOS に追加できます。|✓| |✓|
