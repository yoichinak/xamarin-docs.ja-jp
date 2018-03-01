---
title: "アセンブリ"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0B1ACF06-65FF-49E2-B6BC-7AEC55638ED8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
ms.openlocfilehash: 0a435e80173ca3ccb76dba0719ec2eea6eb72611
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="assemblies"></a>アセンブリ

## <a name="supported-assemblies"></a>サポートされているアセンブリ

これは、Xamarin で Xamarin.tvOS アプリのサポート アセンブリの一覧です。これらの詳細な一覧は、以下に記載されています。  注目すべきいくつか不作為含める`System.EnterpriseServices`、ASP.NET スタックと Windows.Forms です。

<table align="center" border="1" cellpadding="1" cellspacing="1" width="90%">
      <tbody>
        <tr>
          <td>
            <strong>Assembly</strong>
          </td>
          <td>
            <strong>追加</strong>
          </td>
          <td>
            <strong>API の互換性</strong>
          </td>
        </tr>
        <tr>
          <td valign="top">
Mono.CompilerServices.SymbolWriter.dll </td>
          <td align="center" valign="top">
1 </td>
          <td valign="top">
コンパイラのライターです。
          </td>
        </tr>
        <tr>
          <td valign="top">
Mono.Data.Sqlite.dll </td>
          <td align="center" valign="top">
1.2 </td>
          <td>
SQLite; 用 ADO.NET プロバイダー参照してください&nbsp;<a href="~/ios/data-cloud/system.data.md">制限</a>です。
          </td>
        </tr>
        <tr>
          <td valign="top">
Mono.Data.Tds.dll </td>
          <td align="center" valign="top">
1.2 </td>
          <td>
TDS プロトコルをサポートします。使用&nbsp;<a href="https://developer.xamarin.com/api/namespace/System.Data.SqlClient/" target="_blank">System.Data.SqlClient</a>&nbsp;内でサポート&nbsp;<a href="~/ios/data-cloud/system.data.md">System.Data</a>です。
          </td>
        </tr>
        <tr>
          <td>
Mono.Security.dll </td>
          <td align="center" valign="top">
1 </td>
          <td>
暗号化 Api。
          </td>
        </tr>
        <tr>
          <td valign="top">
monotouch.dll </td>
          <td align="center" valign="top">
1 </td>
          <td>
このアセンブリに含まれる、&nbsp;<a href="https://developer.xamarin.com/api/root/ios-unified/" target="_blank">c# CocoaTouch API へのバインド</a>です。
          </td>
        </tr>
        <tr>
          <td valign="top">
mscorlib.dll </td>
          <td align="center" valign="top">
1 </td>
          <td>
            <a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx" target="_blank">Silverlight</a>
          </td>
        </tr>
        <tr>
          <td valign="top">
OpenTK.dll </td>
          <td align="center" valign="top">
1 </td>
          <td>
OpenGL/OpenAL オブジェクト指向の Api、&nbsp;<a href="https://developer.xamarin.com/api/namespace/OpenGLES/" target="_blank">iPhone デバイスのサポートを提供する拡張</a>です。
          </td>
        </tr>
        <tr>
          <td align="left" valign="top">
System.dll </td>
          <td align="center" valign="top">
1 </td>
          <td>
            <p><a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx" target="_blank">Silverlight</a>、さらに次の名前空間の型。</p>
    
            <ul>
              <li>System.Collections.Specialized</li>
              <li>System.ComponentModel</li>
              <li>System.ComponentModel.Design</li>
              <li>System.Diagnostics</li>
              <li>System.IO.Compression</li>
              <li>System.Net</li>
              <li>System.Net.Cache</li>
              <li>System.Net.Mail</li>
              <li>System.Net.Mime</li>
              <li>System.Net.NetworkInformation</li>
              <li>System.Net.Security</li>
              <li>System.Net.Sockets</li>
              <li>System.Security.Authentication</li>
              <li>System.Security.Cryptography</li>
              <li>System.Timers</li>
            </ul>
    
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Core.dll </td>
          <td align="center" valign="top">
1 </td>
          <td>
            <a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx" target="_blank">Silverlight</a>
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Data.dll </td>
          <td align="center" valign="top">
1.2 </td>
          <td>
            <a href="http://msdn.microsoft.com/en-us/library/ms229335.aspx" target="_blank">.NET 3.5</a>&nbsp;、&nbsp;<a href="~/ios/data-cloud/system.data.md">削除いくつかの機能を持つ</a>します。
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Data.Service.Client.dll </td>
          <td align="center" valign="top">
3.x </td>
          <td>
完全 oData クライアント。
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Drawing </td>
          <td align="center" valign="top">
1 </td>
          <td>
            <p>System.drawing の各 API - クラシック API のみです。</p>
            <p><i>Xamarin.Mac .NET 4.5 またはモバイル フレームワークの Unified API では、System.Drawing がサポートされていません。</i></p>
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Json.dll </td>
          <td align="center" valign="top">
1.1 </td>
          <td>
            <a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx" target="_blank">Silverlight</a>
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Runtime.Serialization.dll </td>
          <td align="center" valign="top">
?
          </td>
          <td>
            <a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx" target="_blank">Silverlight</a>
          </td>
        </tr>
        <tr>
          <td valign="top">
System.ServiceModel.dll </td>
          <td align="center" valign="top">
1.1 </td>
          <td>
            <a href="http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services" target="_blank">WCF</a>&nbsp;スタック内に存在として&nbsp;<a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx" target="_blank">Silverlight</a>
          </td>
        </tr>
        <tr>
          <td valign="top">
System.ServiceModel.Web.dll </td>
          <td align="center" valign="top">
?
          </td>
          <td>
            <a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx" target="_blank">Silverlight</a>、さらに次の名前空間の型。 <p>&nbsp;</p>
    
            <ul>
              <li>システム</li>
              <li>System.ServiceModel.Channels</li>
              <li>System.ServiceModel.Description</li>
              <li>System.ServiceModel.Web</li>
            </ul>
    
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Transactions.dll </td>
          <td align="center" valign="top">
1.2 </td>
          <td>
            <a href="http://msdn.microsoft.com/en-us/library/ms229335.aspx" target="_blank">.NET 3.5</a>の一部です。&nbsp;<a href="~/ios/data-cloud/system.data.md">System.Data</a>&nbsp;をサポートします。
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Web.Services </td>
          <td align="center" valign="top">
1.1 </td>
          <td>
            <a href="http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services" target="_blank">基本的な Web サービス</a>&nbsp;削除されたサーバー機能で、.NET 3.5 プロファイルから。
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Xml.dll </td>
          <td align="center" valign="top">
1 </td>
          <td>
            <a href="http://msdn.microsoft.com/en-us/library/ms229335.aspx" target="_blank">.NET 3.5</a>
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Xml.Linq.dll </td>
          <td align="center" valign="top">
1 </td>
          <td>
            <a href="http://msdn.microsoft.com/en-us/library/ms229335.aspx" target="_blank">.NET 3.5</a><br>
            <br>
&nbsp; </td>
        </tr>
      </tbody>
    </table>

<a name="Summary" />

## <a name="portable-class-libraries"></a>ポータブル クラス ライブラリ

Mac のバインドだけでなく Xamarin.tvOS が利用できる[.NET ポータブル クラス ライブラリ](~/cross-platform/app-fundamentals/pcl.md)です。



## <a name="related-links"></a>関連リンク

- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリケーション プログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
