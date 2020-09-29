---
title: 使用できるアセンブリ
description: このドキュメントでは、Xamarin、Xamarin、および Xamarin. Mac で使用できるアセンブリについて説明します。 また、.NET Standard ライブラリやポータブルクラスライブラリに関するドキュメントへのリンクもあります。
ms.prod: xamarin
ms.assetid: AEF4ED0E-391F-4FA4-9F18-842BC24C272D
author: davidortinau
ms.author: daortin
ms.date: 03/13/2018
ms.openlocfilehash: cb5c4a463a8368d6a82d89baba08145f161a95d6
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457940"
---
# <a name="available-assemblies"></a>使用できるアセンブリ

Xamarin、Xamarin、および Xamarin. Mac には、多数のアセンブリが付属しています。 Silverlight がデスクトップ .NET アセンブリの拡張サブセットであるのと同様に、Xamarin プラットフォームは、いくつかの Silverlight およびデスクトップ .NET アセンブリの拡張サブセットでもあります。

Xamarin プラットフォームは、別のプロファイル用にコンパイルされた既存のアセンブリとの ABI との互換性がありません。 ソースコードを再コンパイルして、正しいプロファイルを対象とするアセンブリを生成する必要があります (Silverlight と .NET 3.5 を個別に再コンパイルする必要があるのと同じです)。

Xamarin の Mac アプリケーションは3つのモードでコンパイルできます。1つは Xamarin の curated モバイルプロファイルを使用するモードで、もう1つは、既存の完全なデスクトップアセンブリをターゲットにすることができるようにする Xamarin .NET 4.5 フレームワーク、もう1つはシステム Mono インストールで検出された .NET API を使用するサポート対象外です。 詳細については、 [ターゲットフレームワーク](~/mac/platform/target-framework.md) のドキュメントを参照してください。

## <a name="net-standard-libraries"></a>.NET Standard ライブラリ

IOS、Android、Mac の各バインドに加えて、Xamarin プロジェクトは [.NET Standard ライブラリ](~/cross-platform/app-fundamentals/net-standard.md)を使用できます。

## <a name="portable-class-libraries"></a>ポータブル クラス ライブラリ

Xamarin プロジェクトは、 [.Net ポータブルクラスライブラリ](~/cross-platform/app-fundamentals/pcl.md)を使用することもできます。ただし、このテクノロジは .NET Standard を優先するため非推奨とされます。

## <a name="supported-assemblies"></a>サポートされているアセンブリ

これらのアセンブリは、 **Reference Manager > assemblies > Framework** (Visual Studio 2017) および **編集参照 > パッケージ** (Visual Studio for Mac) で使用でき、Xamarin プラットフォームとの互換性があります。

> [!div class="mx-tdCol2BreakAll"]
> |アセンブリ|API の互換性|Xamarin iOS|Xamarin Android|Xamarin Mac|
> |--------|-----------------|-----------|---------------|-----------|
> |FSharp.Core.dll| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |l18N.dll|CJK、MidEast、Other、まれ、西を含みます|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |Microsoft.CSharp.dll| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |Mono.CSharp.dll| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |Mono.Data.Sqlite.dll|SQLite 用 ADO.NET プロバイダー「制限事項」を参照してください。|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |Mono.Data.Tds.dll|TDS プロトコルのサポートsystem.string 内の [system.string サポートに](xref:System.Data.SqlClient) 使用 [され](xref:System.Data)ます。|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |Mono. Dynamic &#8203;Interpreter.dll| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")| | |
> |Mono.Security.dll|暗号化 Api。|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |monotouch.dll|このアセンブリには、CocoaTouch API への C# バインドが含まれています。 これは、クラシック iOS プロジェクト内でのみ使用できます。|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")| | |
> |&#8203;Monotouch.dialog Dialog-1.dll| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")| | |
> |&#8203;Monotouch.dialog NUnitLite.dll| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")| | |
> |mscorlib.dll|[Silverlight](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838194(v=vs.95))|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |OpenTK-1.0.dll|OpenGL/OpenAL オブジェクト指向 Api。 iPhone デバイスサポートを提供するために拡張されています。|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.dll|[Silverlight](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838194(v=vs.95))に加えて、次の名前空間の型があります。<br />System.Collections.Specialized<br />&#8203;System.componentmodel<br />System.componentmodel<br />System.Diagnostics<br />System.IO<br />System.IO.Compression<br />System.IO.Compression.FileSystem<br />System.Net<br />システム .Net. キャッシュ<br />System.Net.Mail<br />システム .Net. Mime<br />System.Net NetworkInformation の &#8203;<br />System.Net.Security<br />System.Net.Sockets<br />InteropServices &#8203;<br />System.Runtime.Versioning<br />Accesscontrol-namespace &#8203;<br />System.Security.Authentication<br />&#8203;暗号化<br />System.Security.Permissions<br />System.Threading<br />システムタイマー|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |&#8203;System.componentmodel. &#8203;Composition.dll| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |&#8203;System.componentmodel. &#8203;DataAnnotations.dll| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.Core.dll|[Silverlight](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838194(v=vs.95))|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.Data.dll|[.Net 3.5](/previous-versions/ms229335(v=vs.100)) 。 [一部の機能が削除されまし](~/ios/data-cloud/system.data.md)た。|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |&#8203;Services. &#8203;Client.dll|完全な oData クライアント。|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |&#8203;System.IO 圧縮| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.IO Compression. &#8203;ファイルシステム &#8203;| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.Json.dll|[Silverlight](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838194(v=vs.95))|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |&#8203;System.Net Http.dll| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |&#8203;Numerics.dll| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |&#8203;Serialization.dll|[Silverlight](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838194(v=vs.95))|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |&#8203;ServiceModel.dll|[Silverlight](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838194(v=vs.95))に存在する WCF スタック|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System. &#8203;ServiceModel. &#8203;Internals.dll| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System. &#8203;ServiceModel. &#8203;Web.dll|[Silverlight](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838194(v=vs.95))に加えて、次の名前空間の型があります。 <br />システム<br />System.ServiceModel.Channels<br />System.ServiceModel.Description<br />System.ServiceModel.Web|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |&#8203;Transactions.dll|[.Net 3.5](/previous-versions/ms229335(v=vs.100)); [システムデータ](~/ios/data-cloud/system.data.md) のサポートの一部。|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.web. &#8203;Services.dll|.NET 3.5 プロファイルの基本的な Web サービスです。サーバー機能は削除されています。|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |&#8203;Windows.dll| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |&#8203;Xml.dll|[.NET 3.5](/previous-versions/ms229335(v=vs.100))|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.Xml。 &#8203;Linq.dll|[.NET 3.5](/previous-versions/ms229335(v=vs.100))|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |System.Xml.Serialization.dll| |![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")|![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")|![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |Xamarin.iOS.dll|このアセンブリには、CocoaTouch API への C# バインドが含まれています。 これは、統合された iOS プロジェクトでのみ使用されます。|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")| | |
> |Java.Interop.dll| | |![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")| |
> |Mono.Android.dll| | |![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")| |
> |Mono. Android. &#8203;Export.dll| | |![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")| |
> |Mono.Posix.dll| | |![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")| |
> |&#8203;EnterpriseServices.dll| | |![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")| |
> |&#8203;NUnitLite.dll| | |![Xamarin. Android がサポートされる](~/media/shared/yes.png "Xamarin. Android がサポートされる")| |
> |System.runtime.compilerservices &#8203;SymbolWriter.dll|コンパイラライターの場合。| | |![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |Xamarin.Mac.dll| | | |![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|
> |&#8203;Drawing.dll|Xamarin. Mac、.NET 4.5、またはモバイルフレームワークの Unified API では、Drawing はサポートされていません。 システムの描画サポートは、 [sysdrawing coregraphics](https://github.com/mono/sysdrawing-coregraphics) library を使用して IOS および macOS に追加できます。|![Xamarin. iOS がサポートされています](~/media/shared/yes.png "Xamarin. iOS がサポートされています")| |![Xamarin. Mac がサポートされています](~/media/shared/yes.png "Xamarin. Mac がサポートされています")|