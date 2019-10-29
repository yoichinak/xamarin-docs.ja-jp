---
title: 使用できるアセンブリ
description: このドキュメントでは、Xamarin、Xamarin、および Xamarin. Mac で使用できるアセンブリについて説明します。 また、.NET Standard ライブラリやポータブルクラスライブラリに関するドキュメントへのリンクもあります。
ms.prod: xamarin
ms.assetid: AEF4ED0E-391F-4FA4-9F18-842BC24C272D
author: davidortinau
ms.author: daortin
ms.date: 03/13/2018
ms.openlocfilehash: 238011b4762f2d394629e75fbde476e618219df2
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016362"
---
# <a name="available-assemblies"></a>使用できるアセンブリ

Xamarin、Xamarin、および Xamarin. Mac には、多数のアセンブリが付属しています。 Silverlight がデスクトップ .NET アセンブリの拡張サブセットであるのと同様に、Xamarin プラットフォームは、いくつかの Silverlight およびデスクトップ .NET アセンブリの拡張サブセットでもあります。

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
> |l18N|CJK、MidEast、Other、まれ、西を含みます|✓|✓|✓|
> |Microsoft.CSharp.dll| |✓|✓|✓|
> |Mono.CSharp.dll| |✓|✓|✓|
> |Mono.Data.Sqlite.dll|SQLite 用 ADO.NET プロバイダー「制限事項」を参照してください。|✓|✓|✓|
> |Mono.Data.Tds.dll|TDS プロトコルのサポートsystem.string 内の[system.string サポートに](xref:System.Data.SqlClient)使用[され](xref:System.Data)ます。|✓|✓|✓|
> |Mono. 動的。&#8203;インタープリター .dll| |✓| | |
> |Mono.Security.dll|暗号化 Api。|✓|✓|✓|
> |monotouch.dll|このアセンブリにはC# 、CocoaTouch API へのバインドが含まれています。 これは、クラシック iOS プロジェクト内でのみ使用できます。|✓| | |
> |Monotouch.dialog.&#8203;Dialog-1| |✓| | |
> |Monotouch.dialog.&#8203;Nunitlite .dll| |✓| | |
> |mscorlib.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |OpenTK-1.0.dll|OpenGL/OpenAL オブジェクト指向 Api。 iPhone デバイスサポートを提供するために拡張されています。|✓|✓|✓|
> |System.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)に加えて、次の名前空間の型があります。<br />System.Collections.Specialized<br />System.&#8203;System.componentmodel<br />System.componentmodel<br />System.Diagnostics<br />System.IO<br />System.IO.Compression<br />System.IO.Compression.FileSystem<br />System.Net<br />システム .Net. キャッシュ<br />System.Net.Mail<br />システム .Net. Mime<br />System.Net。&#8203;Networkinformation<br />System.Net.Security<br />System.Net.Sockets<br />System.string。&#8203;InteropServices<br />System.Runtime.Versioning<br />System. Security。&#8203;Accesscontrol-namespace<br />System.Security.Authentication<br />System. Security。&#8203;暗号化<br />System.Security.Permissions<br />System.Threading<br />システムタイマー|✓|✓|✓|
> |System.&#8203;System.componentmodel。&#8203;構成 .dll| |✓|✓|✓|
> |System.&#8203;System.componentmodel。&#8203;Dataannotations .dll| |✓|✓|✓|
> |System.Core.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Data.dll|[.Net 3.5](https://msdn.microsoft.com/library/ms229335.aspx) 。[一部の機能が削除されまし](~/ios/data-cloud/system.data.md)た。|✓|✓|✓|
> |System.string。&#8203;サービス。&#8203;クライアント .dll|完全な oData クライアント。|✓|✓|✓|
> |System.IO。&#8203;圧縮| |✓|✓|✓|
> |System.IO。&#8203;圧縮。&#8203;ファイルシステム| |✓|✓|✓|
> |System.Json.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Net。&#8203;Http.sys| |✓|✓|✓|
> |System.&#8203;数値 .dll| |✓|✓|✓|
> |System.string。&#8203;シリアル化 .dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.&#8203;ServiceModel|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)に存在する WCF スタック|✓|✓|✓|
> |System.&#8203;ServiceModel。&#8203;内部 .dll| |✓|✓|✓|
> |System.&#8203;ServiceModel。&#8203;Web .dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)に加えて、次の名前空間の型があります。 <br />システム<br />System.ServiceModel.Channels<br />System.ServiceModel.Description<br />System.ServiceModel.Web|✓|✓|✓|
> |System.&#8203;トランザクション .dll|[.Net 3.5](https://msdn.microsoft.com/library/ms229335.aspx);[システムデータ](~/ios/data-cloud/system.data.md)のサポートの一部。|✓|✓|✓|
> |System.web。&#8203;サービス .dll|.NET 3.5 プロファイルの基本的な Web サービスです。サーバー機能は削除されています。|✓|✓|✓|
> |System.&#8203;Windows .dll| |✓|✓|✓|
> |System.&#8203;Xml .dll|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.xml。&#8203;Linq .dll|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.Serialization.dll| |✓|✓|✓|
> |Xamarin.iOS.dll|このアセンブリにはC# 、CocoaTouch API へのバインドが含まれています。 これは、統合された iOS プロジェクトでのみ使用されます。|✓| | |
> |Java.Interop.dll| | |✓| |
> |Mono.Android.dll| | |✓| |
> |Mono. Android。&#8203;.Dll のエクスポート| | |✓| |
> |Mono.Posix.dll| | |✓| |
> |System.&#8203;System.enterpriseservices| | |✓| |
> |Xamarin. Android.&#8203;Nunitlite .dll| | |✓| |
> |System.runtime.compilerservices。&#8203;シンボルライター|コンパイラライターの場合。| | |✓|
> |Xamarin.Mac.dll| | | |✓|
> |System.&#8203;描画 .dll|Xamarin. Mac、.NET 4.5、またはモバイルフレームワークの Unified API では、Drawing はサポートされていません。 システムの描画サポートは、 [sysdrawing coregraphics](https://github.com/mono/sysdrawing-coregraphics) library を使用して IOS および macOS に追加できます。|✓| |✓|
