---
title: TvOS 用の Xamarin でのアセンブリのサポートされています。
description: TvOS アプリケーションで使用できる機能を明確には、するためには、このドキュメントは、Xamarin で tvOS 開発のサポートされるアセンブリの一覧を提供します。
ms.prod: xamarin
ms.assetid: 0B1ACF06-65FF-49E2-B6BC-7AEC55638ED8
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/07/2016
ms.openlocfilehash: df50b4280335001f2d27ff23a91e4098eed3ba99
ms.sourcegitcommit: 495680e74c72e7c570e68cde95d3d3643b1fcc8a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/02/2019
ms.locfileid: "58870210"
---
# <a name="assemblies-supported-by-xamarin-for-tvos"></a>TvOS 用の Xamarin でのアセンブリのサポートされています。

## <a name="supported-assemblies"></a>サポートされているアセンブリ

Xamarin.tvOS アプリを Xamarin でサポートされるアセンブリの一覧です。これらの詳細な一覧は、以下に記載されています。  いくつか注目すべき漏れを含める`System.EnterpriseServices`、ASP.NET スタックと Windows.Forms します。

|Assembly|追加|API の互換性|
|---|---|---|
|Mono.CompilerServices.SymbolWriter.dll|1|コンパイラ ライター。|
|Mono.Data.Sqlite.dll|1.2|SQLite の ADO.NET プロバイダー参照してください[制限](~/ios/data-cloud/system.data.md)します。|
|Mono.Data.Tds.dll|1.2|TDS プロトコルのサポート。使用される[System.Data.SqlClient](xref:System.Data.SqlClient)内サポート[System.Data](~/ios/data-cloud/system.data.md)します。|
|Mono.Security.dll|1|暗号化 Api です。|
|monotouch.dll|1|このアセンブリに含まれる、 [C# CocoaTouch API へのバインド](https://docs.microsoft.com/dotnet/api/?view=xamarinios-10.8)します。|
|mscorlib.dll|1|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|OpenTK.dll|1|OpenGL/OpenAL オブジェクト指向 Api、 [iPhone デバイス サポートを提供する拡張](xref:OpenGLES)します。|
|System.dll|1|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)、さらに次の名前空間の型。 <ul><li>System.Collections.Specialized</li> <li>System.ComponentModel</li> <li>System.ComponentModel.Design</li> <li>System.Diagnostics</li> <li>System.IO.Compression</li> <li>System.Net</li> <li>System.Net.Cache</li> <li>System.Net.Mail</li> <li>System.Net.Mime</li> <li>System.Net.NetworkInformation</li> <li>System.Net.Security</li> <li>System.Net.Sockets</li> <li>System.Security.Authentication</li> <li>System.Security.Cryptography</li> <li>System.Timers</li></ul>|
|System.Core.dll|1|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Data.dll|1.2|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)、[削除いくつかの機能を備えた](~/ios/data-cloud/system.data.md)します。|
|System.Data.Service.Client.dll|3.x|完全な oData クライアント。|
|System.Drawing|1|System.Drawing API - クラシック API のみです。<br />_Xamarin.Mac .NET 4.5 またはモバイル フレームワークの Unified API では、System.Drawing がサポートされていません。_|
|System.Json.dll|1.1|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Runtime.Serialization.dll|?|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.dll|1.1|[WCF](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services)スタック内に存在として[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.Web.dll|?|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)、さらに次の名前空間の型。 <ul><li>システム</li><li>System.ServiceModel.Channels</li><li>System.ServiceModel.Description</li><li>System.ServiceModel.Web</li></ul>|
|System.Transactions.dll|1.2|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx); の一部[System.Data](https://docs.microsoft.com/xamarin/ios/data-cloud/system.data)をサポートします。|
|System.Web.Services|1.1|[基本的な Web サービス](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services)削除されたサーバーの機能で、.NET 3.5 プロファイルから。|
|System.Xml.dll|1|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|
|System.Xml.Linq.dll|1|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|

<a name="Summary" />

## <a name="portable-class-libraries"></a>ポータブル クラス ライブラリ

Xamarin.tvOS が利用できるだけでなく、Mac バインド[.NET ポータブル クラス ライブラリ](~/cross-platform/app-fundamentals/pcl.md)します。

## <a name="related-links"></a>関連リンク

- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 用のアプリのプログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
