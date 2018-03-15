---
title: "使用可能なアセンブリ"
description: "Xamarin.iOS、Xamarin.Android、および Xamarin.Mac で使用可能なアセンブリ"
ms.topic: article
ms.prod: xamarin
ms.assetid: AEF4ED0E-391F-4FA4-9F18-842BC24C272D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/13/2018
ms.openlocfilehash: 09df3b5e203281070e21b8c18ee4ff42245c0e46
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
# <a name="available-assemblies"></a>使用可能なアセンブリ

_Xamarin.iOS、Xamarin.Android、および Xamarin.Mac で使用可能なアセンブリ_

Xamarin.iOS、Xamarin.Android、および Xamarin.Mac すべて十数か所アセンブリが付属します。 Silverlight は、デスクトップの .NET アセンブリの拡張のサブセットは、同じよう Xamarin プラットフォームもいくつかの Silverlight およびデスクトップの .NET アセンブリの拡張のサブセットです。

Xamarin プラットフォームは、既存のアセンブリが別のプロファイル用にコンパイルと互換性のある ABI ではありません。 (個別に Silverlight および .NET 3.5 を対象とするソース コードを再コンパイルする必要があります) と同様に、適切なプロファイルを対象とするアセンブリを生成するソース コードを再コンパイルする必要があります。

Xamarin.Mac アプリケーションは、3 つのモードでコンパイルすることができますモバイル プロファイルのいずれかの Xamarin を使用して curated、Xamarin.Mac .NET 4.5 Framework ことができる対象の既存の完全なデスクトップ アセンブリおよびシステム モノラルで .NET API を使用するためのサポートされていない 1 つが見つかりました。インストール。 詳細についてを参照してください、[ターゲット フレームワーク](~/mac/platform/target-framework.md)ドキュメント。


## <a name="net-standard-libraries"></a>標準の .NET ライブラリ

に加えて、iOS、Android、および Mac のバインド、Xamarin のプロジェクトが使用できる[.NET 標準ライブラリ](~/cross-platform/app-fundamentals/net-standard.md)です。

## <a name="portable-class-libraries"></a>ポータブル クラス ライブラリ
 
Xamarin プロジェクトでも使用できます[.NET ポータブル クラス ライブラリ](~/cross-platform/app-fundamentals/pcl.md).NET 標準を優先するためこのテクノロジは推奨されなくなりましたが、します。

## <a name="supported-assemblies"></a>サポートされているアセンブリ

> [!div class="mx-tdCol2BreakAll"]
> |Assembly|API の互換性|Xamarin iOS|Xamarin Android|Xamarin Mac|
> |--------|-----------------|-----------|---------------|-----------|
> |FSharp.Core.dll| |✓|✓|✓|
> |l18N.dll|西 CJK、MidEast、まれで、他が含まれています|✓|✓|✓|
> |Microsoft.CSharp.dll| |✓|✓|✓|
> |Mono.CSharp.dll| |✓|✓|✓|
> |Mono.Data.Sqlite.dll|SQLite; 用 ADO.NET プロバイダー制限事項を参照してください。|✓|✓|✓|
> |Mono.Data.Tds.dll|TDS プロトコルをサポートします。使用[System.Data.SqlClient](https://developer.xamarin.com/api/namespace/System.Data.SqlClient/)内でサポート[System.Data](https://developer.xamarin.com/api/namespace/System.Data/)です。|✓|✓|✓|
> |Mono.Dynamic.&#8203;Interpreter.dll| |✓| | |
> |Mono.Security.dll|暗号化 Api。|✓|✓|✓|
> |monotouch.dll|このアセンブリには、CocoaTouch API に c# バインドが含まれています。 これはクラシック iOS プロジェクト内で使用できるのみです。|✓| | |
> |MonoTouch.&#8203;Dialog-1.dll| |✓| | |
> |MonoTouch.&#8203;NUnitLite.dll| |✓| | |
> |mscorlib.dll|[Silverlight](https://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |OpenTK-1.0.dll|OpenGL/OpenAL オブジェクト指向の Api では、拡張が iPhone デバイスのサポートを提供します。|✓|✓|✓|
> |System.dll|[Silverlight](https://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx)、さらに次の名前空間の型。<br />System.Collections.Specialized<br />System.&#8203;ComponentModel<br />System.ComponentModel.Design<br />System.Diagnostics<br />System.IO<br />System.IO.Compression<br />System.IO.Compression.FileSystem<br />System.Net<br />System.Net.Cache<br />System.Net.Mail<br />System.Net.Mime<br />System.Net.&#8203;NetworkInformation<br />System.Net.Security<br />System.Net.Sockets<br />System.Runtime.&#8203;InteropServices<br />System.Runtime.Versioning<br />System.Security.&#8203;AccessControl<br />System.Security.Authentication<br />System.Security.&#8203;Cryptography<br />System.Security.Permissions<br />System.Threading<br />System.Timers|✓|✓|✓|
> |System.&#8203;ComponentModel.&#8203;Composition.dll| |✓|✓|✓|
> |System.&#8203;ComponentModel.&#8203;DataAnnotations.dll| |✓|✓|✓|
> |System.Core.dll|[Silverlight](https://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Data.dll|[.NET 3.5](http://msdn.microsoft.com/en-us/library/ms229335.aspx)で[一部の機能の削除](~/ios/data-cloud/system.data.md)です。|✓|✓|✓|
> |System.Data.&#8203;Services.&#8203;Client.dll|完全 oData クライアント。|✓|✓|✓|
> |System.IO.&#8203;Compression| |✓|✓|✓|
> |System.IO.&#8203;Compression.&#8203;FileSystem| |✓|✓|✓|
> |System.Json.dll|[Silverlight](http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Net.&#8203;Http.dll| |✓|✓|✓|
> |System.&#8203;Numerics.dll| |✓|✓|✓|
> |System.Runtime.&#8203;Serialization.dll|[Silverlight](http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.&#8203;ServiceModel.dll|WCF のスタック内に存在と[Silverlight](http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.&#8203;ServiceModel.&#8203;Internals.dll| |✓|✓|✓|
> |System.&#8203;ServiceModel.&#8203;Web.dll|[Silverlight](http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx)、さらに次の名前空間の型。 <br />システム<br />System.ServiceModel.Channels<br />System.ServiceModel.Description<br />System.ServiceModel.Web|✓|✓|✓|
> |System.&#8203;Transactions.dll|[.NET 3.5](http://msdn.microsoft.com/en-us/library/ms229335.aspx)の一部です。 [System.Data](~/ios/data-cloud/system.data.md)をサポートします。|✓|✓|✓|
> |System.Web.&#8203;Services.dll|削除されたサーバー機能で、.NET 3.5 プロファイルから基本的な Web サービスです。|✓|✓|✓|
> |System.&#8203;Windows.dll| |✓|✓|✓|
> |System.&#8203;Xml.dll|[.NET 3.5](http://msdn.microsoft.com/en-us/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.&#8203;Linq.dll|[.NET 3.5](http://msdn.microsoft.com/en-us/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.Serialization.dll| |✓|✓|✓|
> |Xamarin.iOS.dll|このアセンブリには、CocoaTouch API に c# バインドが含まれています。 これは、統合 iOS プロジェクトでのみ使用します。|✓| | |
> |Java.Interop.dll| | |✓| |
> |Mono.Android.dll| | |✓| |
> |Mono.Android.&#8203;Export.dll| | |✓| |
> |Mono.Posix.dll| | |✓| |
> |System.&#8203;EnterpriseServices.dll| | |✓| |
> |Xamarin.Android.&#8203;NUnitLite.dll| | |✓| |
> |Mono.CompilerServices.&#8203;SymbolWriter.dll|コンパイラのライターです。| | |✓|
> |Xamarin.Mac.dll| | | |✓|
> |System.&#8203;Drawing.dll|System.drawing の各 API - クラシック API のみです。Xamarin.Mac .NET 4.5 またはモバイル フレームワークの Unified API では、System.Drawing がサポートされていません。IOS および OS X を使用してに System.Drawing サポートを追加することができます、 [sysdrawing coregraphics](https://github.com/mono/sysdrawing-coregraphics)ライブラリ|✓| |✓|
