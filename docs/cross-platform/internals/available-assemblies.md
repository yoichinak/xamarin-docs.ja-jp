---
title: 使用できるアセンブリ
description: このドキュメントには、Xamarin.iOS、Xamarin.Android、Xamarin.Mac で使用可能なアセンブリが一覧表示されます。 .NET Standard ライブラリとポータブル クラス ライブラリに関するドキュメントにもリンクします。
ms.prod: xamarin
ms.assetid: AEF4ED0E-391F-4FA4-9F18-842BC24C272D
author: asb3993
ms.author: amburns
ms.date: 03/13/2018
ms.openlocfilehash: 8882a7cd36eab5e7905585f5de4d6585dfb53439
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57672262"
---
# <a name="available-assemblies"></a>使用できるアセンブリ

Xamarin.iOS、Xamarin.Android、および Xamarin.Mac、十数個のアセンブリがすべて付属します。 同様、デスクトップの .NET アセンブリの拡張のサブセットを Silverlight には、Xamarin プラットフォーム、いくつかの Silverlight とデスクトップの .NET アセンブリの拡張のサブセットです。

Xamarin プラットフォームは、既存のアセンブリが別のプロファイル用にコンパイルと互換性のある ABI ではできません。 (個別に、Silverlight と .NET 3.5 を対象とするソース コードを再コンパイルする必要があります) と同様に、適切なプロファイルを対象とするアセンブリを生成するソース コードを再コンパイルする必要があります。

Xamarin.Mac アプリケーションは、3 つのモードでコンパイルすることができますモバイル プロファイルを選別された Xamarin を使用する、Xamarin.Mac .NET 4.5 Framework ことができますが、既存の完全なデスクトップ アセンブリをターゲットおよび .NET API を使用するためのサポートされていない 1 つがシステムで Mono が見つかりません。インストールします。 詳細についてを参照してください、[ターゲット フレームワーク](~/mac/platform/target-framework.md)ドキュメント。

## <a name="net-standard-libraries"></a>.NET Standard ライブラリ

IOS、Android、および Mac だけでなくバインド、Xamarin プロジェクトで利用可能[.NET Standard ライブラリ](~/cross-platform/app-fundamentals/net-standard.md)します。

## <a name="portable-class-libraries"></a>ポータブル クラス ライブラリ

Xamarin プロジェクトで利用可能も[.NET ポータブル クラス ライブラリ](~/cross-platform/app-fundamentals/pcl.md)、.NET Standard 優先このテクノロジは推奨されているとします。

## <a name="supported-assemblies"></a>サポートされているアセンブリ

これらは、アセンブリで使用できる、**参照マネージャー > アセンブリ > Framework** (Visual Studio 2017) および**参照の編集 > パッケージ**(Visual Studio for Mac) との互換性Xamarin のプラットフォームです。

> [!div class="mx-tdCol2BreakAll"]
> |Assembly|API の互換性|Xamarin iOS|Xamarin Android|Xamarin Mac|
> |--------|-----------------|-----------|---------------|-----------|
> |FSharp.Core.dll| |✓|✓|✓|
> |l18N.dll|Includes CJK, MidEast, Other, Rare, West|✓|✓|✓|
> |Microsoft.CSharp.dll| |✓|✓|✓|
> |Mono.CSharp.dll| |✓|✓|✓|
> |Mono.Data.Sqlite.dll|SQLite の ADO.NET プロバイダー制限事項を参照してください。|✓|✓|✓|
> |Mono.Data.Tds.dll|TDS プロトコルのサポート。使用される[System.Data.SqlClient](xref:System.Data.SqlClient)内サポート[System.Data](xref:System.Data)します。|✓|✓|✓|
> |Mono.Dynamic.&#8203;Interpreter.dll| |✓| | |
> |Mono.Security.dll|暗号化 Api です。|✓|✓|✓|
> |monotouch.dll|このアセンブリには、CocoaTouch API に c# バインディングが含まれています。 これは、クラシック iOS プロジェクト内で使用できるだけです。|✓| | |
> |MonoTouch.&#8203;Dialog-1.dll| |✓| | |
> |MonoTouch.&#8203;NUnitLite.dll| |✓| | |
> |mscorlib.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |OpenTK-1.0.dll|OpenGL/OpenAL オブジェクト指向の Api、iPhone デバイス サポートを提供するように拡張します。|✓|✓|✓|
> |System.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)、さらに次の名前空間の型。<br />System.Collections.Specialized<br />System.&#8203;ComponentModel<br />System.ComponentModel.Design<br />System.Diagnostics<br />System.IO<br />System.IO.Compression<br />System.IO.Compression.FileSystem<br />System.Net<br />System.Net.Cache<br />System.Net.Mail<br />System.Net.Mime<br />System.Net.&#8203;NetworkInformation<br />System.Net.Security<br />System.Net.Sockets<br />System.Runtime.&#8203;InteropServices<br />System.Runtime.Versioning<br />System.Security.&#8203;AccessControl<br />System.Security.Authentication<br />System.Security.&#8203;Cryptography<br />System.Security.Permissions<br />System.Threading<br />System.Timers|✓|✓|✓|
> |System.&#8203;ComponentModel.&#8203;Composition.dll| |✓|✓|✓|
> |System.&#8203;ComponentModel.&#8203;DataAnnotations.dll| |✓|✓|✓|
> |System.Core.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Data.dll|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)で[一部の機能の削除](~/ios/data-cloud/system.data.md)します。|✓|✓|✓|
> |System.Data.&#8203;Services.&#8203;Client.dll|完全な oData クライアント。|✓|✓|✓|
> |System.IO.&#8203;Compression| |✓|✓|✓|
> |System.IO.&#8203;Compression.&#8203;FileSystem| |✓|✓|✓|
> |System.Json.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Net.&#8203;Http.dll| |✓|✓|✓|
> |System.&#8203;Numerics.dll| |✓|✓|✓|
> |System.Runtime.&#8203;Serialization.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.&#8203;ServiceModel.dll|WCF スタック内に存在として[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.&#8203;ServiceModel.&#8203;Internals.dll| |✓|✓|✓|
> |System.&#8203;ServiceModel.&#8203;Web.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)、さらに次の名前空間の型。 <br />システム<br />System.ServiceModel.Channels<br />System.ServiceModel.Description<br />System.ServiceModel.Web|✓|✓|✓|
> |System.&#8203;Transactions.dll|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx); の一部[System.Data](~/ios/data-cloud/system.data.md)をサポートします。|✓|✓|✓|
> |System.Web.&#8203;Services.dll|削除されたサーバーの機能で、.NET 3.5 プロファイルからの基本的な Web サービス。|✓|✓|✓|
> |System.&#8203;Windows.dll| |✓|✓|✓|
> |System.&#8203;Xml.dll|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.&#8203;Linq.dll|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.Serialization.dll| |✓|✓|✓|
> |Xamarin.iOS.dll|このアセンブリには、CocoaTouch API に c# バインディングが含まれています。 これは、統一された iOS プロジェクトでのみ使用します。|✓| | |
> |Java.Interop.dll| | |✓| |
> |Mono.Android.dll| | |✓| |
> |Mono.Android.&#8203;Export.dll| | |✓| |
> |Mono.Posix.dll| | |✓| |
> |System.&#8203;EnterpriseServices.dll| | |✓| |
> |Xamarin.Android.&#8203;NUnitLite.dll| | |✓| |
> |Mono.CompilerServices.&#8203;SymbolWriter.dll|コンパイラ ライター。| | |✓|
> |Xamarin.Mac.dll| | | |✓|
> |System.&#8203;Drawing.dll|System.Drawing は、Xamarin.Mac、.NET 4.5、またはモバイル フレームワークの Unified API でサポートされていません。 IOS および macOS を使用してに System.Drawing サポートを追加できる、 [sysdrawing coregraphics](https://github.com/mono/sysdrawing-coregraphics)ライブラリ|✓| |✓|
