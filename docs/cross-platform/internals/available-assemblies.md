---
title: 使用できるアセンブリ
description: このドキュメントでは、Xamarin、Xamarin、および Xamarin. Mac で使用できるアセンブリについて説明します。 また、.NET Standard ライブラリやポータブルクラスライブラリに関するドキュメントへのリンクもあります。
ms.prod: xamarin
ms.assetid: AEF4ED0E-391F-4FA4-9F18-842BC24C272D
author: davidortinau
ms.author: daortin
ms.date: 03/13/2018
ms.openlocfilehash: 31066d09b1e753dd054a6a908b626ca3edee008e
ms.sourcegitcommit: 04929b5ff4384ca807727bec7c0467111a7eb283
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/10/2020
ms.locfileid: "75867618"
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
> |FSharp.Core.dll| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |l18N.dll|CJK、MidEast、Other、まれ、西を含みます|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |Microsoft.CSharp.dll| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |Mono.CSharp.dll| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |Mono.Data.Sqlite.dll|SQLite 用 ADO.NET プロバイダー「制限事項」を参照してください。|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |Mono.Data.Tds.dll|TDS プロトコルのサポートsystem.string 内の[system.string サポートに](xref:System.Data.SqlClient)使用[され](xref:System.Data)ます。|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |Mono.Dynamic.&#8203;Interpreter.dll| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")| | |
> |Mono.Security.dll|暗号化 Api。|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |monotouch.dll|このアセンブリにはC# 、CocoaTouch API へのバインドが含まれています。 これは、クラシック iOS プロジェクト内でのみ使用できます。|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")| | |
> |MonoTouch.&#8203;Dialog-1.dll| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")| | |
> |MonoTouch.&#8203;NUnitLite.dll| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")| | |
> |mscorlib.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |OpenTK-1.0.dll|OpenGL/OpenAL オブジェクト指向 Api。 iPhone デバイスサポートを提供するために拡張されています。|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)に加えて、次の名前空間の型があります。<br />System.Collections.Specialized<br />System.&#8203;ComponentModel<br />System.ComponentModel.Design<br />System.Diagnostics<br />System.IO<br />System.IO.Compression<br />System.IO.Compression.FileSystem<br />System.Net<br />System.Net.Cache<br />System.Net.Mail<br />System.Net.Mime<br />System.Net.&#8203;NetworkInformation<br />System.Net.Security<br />System.Net.Sockets<br />System.Runtime.&#8203;InteropServices<br />System.Runtime.Versioning<br />System.Security.&#8203;AccessControl<br />System.Security.Authentication<br />System.Security.&#8203;Cryptography<br />System.Security.Permissions<br />System.Threading<br />System.Timers|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.&#8203;ComponentModel.&#8203;Composition.dll| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.&#8203;ComponentModel.&#8203;DataAnnotations.dll| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.Core.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.Data.dll|[.Net 3.5](https://msdn.microsoft.com/library/ms229335.aspx) 。[一部の機能が削除されまし](~/ios/data-cloud/system.data.md)た。|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.Data.&#8203;Services.&#8203;Client.dll|完全な oData クライアント。|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.IO.&#8203;Compression| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.IO.&#8203;Compression.&#8203;FileSystem| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.Json.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.Net.&#8203;Http.dll| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.&#8203;Numerics.dll| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.Runtime.&#8203;Serialization.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.&#8203;ServiceModel.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)に存在する WCF スタック|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.&#8203;ServiceModel.&#8203;Internals.dll| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.&#8203;ServiceModel.&#8203;Web.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)に加えて、次の名前空間の型があります。 <br />System<br />System.ServiceModel.Channels<br />System.ServiceModel.Description<br />System.ServiceModel.Web|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.&#8203;Transactions.dll|[.Net 3.5](https://msdn.microsoft.com/library/ms229335.aspx);[システムデータ](~/ios/data-cloud/system.data.md)のサポートの一部。|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.Web.&#8203;Services.dll|.NET 3.5 プロファイルの基本的な Web サービスです。サーバー機能は削除されています。|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.&#8203;Windows.dll| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.&#8203;Xml.dll|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.Xml.&#8203;Linq.dll|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.Xml.Serialization.dll| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |Xamarin.iOS.dll|このアセンブリにはC# 、CocoaTouch API へのバインドが含まれています。 これは、統合された iOS プロジェクトでのみ使用されます。|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")| | |
> |Java.Interop.dll| | |![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")| |
> |Mono.Android.dll| | |![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")| |
> |Mono.Android.&#8203;Export.dll| | |![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")| |
> |Mono.Posix.dll| | |![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")| |
> |System.&#8203;EnterpriseServices.dll| | |![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")| |
> |Xamarin.Android.&#8203;NUnitLite.dll| | |![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")| |
> |Mono.CompilerServices.&#8203;SymbolWriter.dll|コンパイラライターの場合。| | |![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |Xamarin.Mac.dll| | | |![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.&#8203;Drawing.dll|Xamarin. Mac、.NET 4.5、またはモバイルフレームワークの Unified API では、Drawing はサポートされていません。 システムの描画サポートは、 [sysdrawing coregraphics](https://github.com/mono/sysdrawing-coregraphics) library を使用して IOS および macOS に追加できます。|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")| |![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|