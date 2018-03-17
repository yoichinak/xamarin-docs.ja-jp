---
title: "アセンブリ"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0B1ACF06-65FF-49E2-B6BC-7AEC55638ED8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
ms.openlocfilehash: 509d4f465956d56e8efa5b69153764f5ae0949a9
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/17/2018
---
# <a name="assemblies"></a>アセンブリ

## <a name="supported-assemblies"></a>サポートされているアセンブリ

これは、Xamarin で Xamarin.tvOS アプリのサポート アセンブリの一覧です。これらの詳細な一覧は、以下に記載されています。  注目すべきいくつか不作為含める`System.EnterpriseServices`、ASP.NET スタックと Windows.Forms です。

|Assembly|追加|API の互換性|
|---|---|---|
|Mono.CompilerServices.SymbolWriter.dll|1|コンパイラのライターです。|
|Mono.Data.Sqlite.dll|1.2|SQLite; 用 ADO.NET プロバイダー参照してください[制限](~/ios/data-cloud/system.data.md)です。|
|Mono.Data.Tds.dll|1.2|TDS プロトコルをサポートします。使用[System.Data.SqlClient](https://developer.xamarin.com/api/namespace/System.Data.SqlClient/)内でサポート[System.Data](~/ios/data-cloud/system.data.md)です。|
|Mono.Security.dll|1|暗号化 Api。|
|monotouch.dll|1|このアセンブリに含まれる、 [c# CocoaTouch API へのバインド](https://developer.xamarin.com/api/root/ios-unified/)です。|
|mscorlib.dll|1|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|OpenTK.dll|1|OpenGL/OpenAL オブジェクト指向の Api、 [iPhone デバイスのサポートを提供する拡張](https://developer.xamarin.com/api/namespace/OpenGLES/)です。|
|System.dll|1|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)、さらに次の名前空間の型。 <ul><li>System.Collections.Specialized</li> <li>System.ComponentModel</li> <li>System.ComponentModel.Design</li> <li>System.Diagnostics</li> <li>System.IO.Compression</li> <li>System.Net</li> <li>System.Net.Cache</li> <li>System.Net.Mail</li> <li>System.Net.Mime</li> <li>System.Net.NetworkInformation</li> <li>System.Net.Security</li> <li>System.Net.Sockets</li> <li>System.Security.Authentication</li> <li>System.Security.Cryptography</li> <li>System.Timers</li></ul>|
|System.Core.dll|1|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Data.dll|1.2|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)、[削除いくつかの機能を持つ](~/ios/data-cloud/system.data.md)します。|
|System.Data.Service.Client.dll|3.x|完全 oData クライアント。|
|System.Drawing|1|System.drawing の各 API - クラシック API のみです。<br />_Xamarin.Mac .NET 4.5 またはモバイル フレームワークの Unified API では、System.Drawing がサポートされていません。_|
|System.Json.dll|1.1|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Runtime.Serialization.dll|?|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.dll|1.1|[WCF](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services)スタック内に存在として[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.Web.dll|?|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)、さらに次の名前空間の型。 <ul><li>システム</li><li>System.ServiceModel.Channels</li><li>System.ServiceModel.Description</li><li>System.ServiceModel.Web</li></ul>|
|System.Transactions.dll|1.2|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)の一部です。 [System.Data](https://docs.microsoft.com/xamarin/ios/data-cloud/system.data)をサポートします。|
|System.Web.Services|1.1|[基本的な Web サービス](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services)削除されたサーバー機能で、.NET 3.5 プロファイルから。|
|System.Xml.dll|1|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|
|System.Xml.Linq.dll|1|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|

<a name="Summary" />

## <a name="portable-class-libraries"></a>ポータブル クラス ライブラリ

Mac のバインドだけでなく Xamarin.tvOS が利用できる[.NET ポータブル クラス ライブラリ](~/cross-platform/app-fundamentals/pcl.md)です。



## <a name="related-links"></a>関連リンク

- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリケーション プログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
