---
title: Xamarin for tvOS でサポートされているアセンブリ
description: TvOS アプリケーションで使用できる機能を明確にするために、このドキュメントでは、Xamarin for tvOS development でサポートされているアセンブリの一覧を示します。
ms.prod: xamarin
ms.assetid: 0B1ACF06-65FF-49E2-B6BC-7AEC55638ED8
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/07/2016
ms.openlocfilehash: 193f4a445e21416abf2fd6279cdc18228e16c985
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70283613"
---
# <a name="assemblies-supported-by-xamarin-for-tvos"></a>Xamarin for tvOS でサポートされているアセンブリ

## <a name="supported-assemblies"></a>サポートされているアセンブリ

これは、xamarin. tvOS アプリの Xamarin でサポートされているアセンブリの一覧です。これらの詳細な一覧を次に示します。  いくつかの注目`System.EnterpriseServices`すべき点として、ASP.NET スタックと Windows フォームがあります。

|Assembly|追加|API の互換性|
|---|---|---|
|Mono.CompilerServices.SymbolWriter.dll|1|コンパイラライターの場合。|
|Mono.Data.Sqlite.dll|1.2|SQLite 用 ADO.NET プロバイダー「[制限事項](~/ios/data-cloud/system.data.md)」を参照してください。|
|Mono.Data.Tds.dll|1.2|TDS プロトコルのサポートsystem.string 内の[system.string サポートに](xref:System.Data.SqlClient)使用[され](~/ios/data-cloud/system.data.md)ます。|
|Mono.Security.dll|1|暗号化 Api。|
|monotouch.dll|1|このアセンブリには、 [ C# CocoaTouch API へのバインド](https://docs.microsoft.com/dotnet/api/?view=xamarinios-10.8)が含まれています。|
|mscorlib.dll|1|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|OpenTK.dll|1|OpenGL/OpenAL オブジェクト指向 Api。 [iPhone デバイスサポートを提供するために拡張](xref:OpenGLES)されています。|
|System.dll|1|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)に加えて、次の名前空間の型があります。 <ul><li>System.Collections.Specialized</li> <li>System.ComponentModel</li> <li>System.ComponentModel.Design</li> <li>System.Diagnostics</li> <li>System.IO.Compression</li> <li>System.Net</li> <li>System.Net.Cache</li> <li>System.Net.Mail</li> <li>System.Net.Mime</li> <li>System.Net.NetworkInformation</li> <li>System.Net.Security</li> <li>System.Net.Sockets</li> <li>System.Security.Authentication</li> <li>System.Security.Cryptography</li> <li>System.Timers</li></ul>|
|System.Core.dll|1|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Data.dll|1.2|[.Net 3.5](https://msdn.microsoft.com/library/ms229335.aspx)。[一部の機能が削除されました](~/ios/data-cloud/system.data.md)。|
|System.Data.Service.Client.dll|3.x|完全な oData クライアント。|
|System.Drawing|1|Drawing API Classic API のみ。<br />_Xamarin .NET 4.5 またはモバイルフレームワークの Unified API では、Drawing はサポートされていません。_|
|System.Json.dll|1.1|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Runtime.Serialization.dll|?|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.dll|1.1|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)に存在する[WCF](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services)スタック|
|System.ServiceModel.Web.dll|?|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)に加えて、次の名前空間の型があります。 <ul><li>システム</li><li>System.ServiceModel.Channels</li><li>System.ServiceModel.Description</li><li>System.ServiceModel.Web</li></ul>|
|System.Transactions.dll|1.2|[.Net 3.5](https://msdn.microsoft.com/library/ms229335.aspx);[システムデータ](https://docs.microsoft.com/xamarin/ios/data-cloud/system.data)のサポートの一部。|
|System.Web.Services|1.1|.NET 3.5 プロファイルの[基本的な Web サービス](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services)です。サーバー機能は削除されています。|
|System.Xml.dll|1|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|
|System.Xml.Linq.dll|1|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|

<a name="Summary" />

## <a name="portable-class-libraries"></a>ポータブル クラス ライブラリ

Mac バインドに加えて、tvOS は[.Net ポータブルクラスライブラリ](~/cross-platform/app-fundamentals/pcl.md)を使用できます。

## <a name="related-links"></a>関連リンク

- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマンインターフェイスガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリプログラミングガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
