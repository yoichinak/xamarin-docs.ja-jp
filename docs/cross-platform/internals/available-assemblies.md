---
title: "使用可能なアセンブリ"
description: "Xamarin.iOS、Xamarin.Android、および Xamarin.Mac で使用可能なアセンブリ"
ms.topic: article
ms.prod: xamarin
ms.assetid: AEF4ED0E-391F-4FA4-9F18-842BC24C272D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 3a4e39582a389774e5d17327cefecb0f676adfc6
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="available-assemblies"></a>使用可能なアセンブリ

_Xamarin.iOS、Xamarin.Android、および Xamarin.Mac で使用可能なアセンブリ_

Xamarin.iOS、Xamarin.Android、および Xamarin.Mac すべて十数か所アセンブリが付属します。 Silverlight は、デスクトップの .NET アセンブリの拡張のサブセットは、同じよう Xamarin プラットフォームもいくつかの Silverlight およびデスクトップの .NET アセンブリの拡張のサブセットです。

Xamarin プラットフォームは、既存のアセンブリが別のプロファイル用にコンパイルと互換性のある ABI ではありません。 正しいターゲットとするアセンブリを生成するソース コードを再コンパイルする必要があります。 プロファイルが (同じようにターゲット Silverlight および .NET 3.5 にソース コードを個別に再コンパイルする必要があります)。

Xamarin.Mac アプリケーションは、3 つのモードでコンパイルすることができますモバイル プロファイルのいずれかの Xamarin を使用して curated、Xamarin.Mac .NET 4.5 Framework ことができる対象の既存の完全なデスクトップ アセンブリおよびシステム モノラルで .NET API を使用するためのサポートされていない 1 つが見つかりました。インストール。 詳細についてを参照してください、[ターゲット フレームワーク](~/mac/platform/target-framework.md)ドキュメント。


## <a name="portable-class-libraries"></a>ポータブル クラス ライブラリ
 
IOS、Android、および Mac だけでなくバインド、Xamarin プラットフォームを利用できる[.NET ポータブル クラス ライブラリ](~/cross-platform/app-fundamentals/pcl.md)です。

## <a name="supported-assemblies"></a>サポートされているアセンブリ

<style type="text/css"> .tg {罫線-折りたたみ: 折りたたみ; 境界線-間隔: 0;} .tg td {パディング: 10 px 5 px; 境界線-幅: 1px; オーバーフロー: 非表示になります word-break: normal;} .tg th {フォント-重み: 標準; padding: 10 px 5 px; 境界線-幅: 1px; オーバーフロー: 非表示になります。 word-break: normal;} .tg .tg zx05。{フォントの太さ: 太字; 背景色: #d1d9dd; 垂直位置揃え: 上部} .tg .tg w6qd {背景色: #91e2f4; テキスト配置: 中央以外の垂直位置揃え: 上部} .tg .tg yw4l {垂直位置揃え: 上部} .tg .tg q3ci {背景色: #cfefa7; テキスト配置: 中央揃えです。垂直位置揃え: top} .tg .tg p7et {背景色: #cec0ec; テキスト配置: 中央以外の垂直位置揃え: top} </style>
<table class="tg">
  <tr>
    <th class="tg-zx05">Assembly</th>
    <th class="tg-zx05">API の互換性</th>
    <th class="tg-zx05">Xamarin.iOS</th>
    <th class="tg-zx05">Xamarin.Android</th>
    <th class="tg-zx05">Xamarin.Mac</th>
  </tr>
  <tr>
    <td class="tg-yw4l">FSharp.Core.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">l18N.dll</td>
    <td class="tg-yw4l">西 CJK、MidEast、まれで、他が含まれています</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Microsoft.CSharp.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Mono.CSharp.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Mono.Data.Sqlite.dll</td>
    <td class="tg-yw4l">SQLite; 用 ADO.NET プロバイダー参照してください<a href="~/ios/data-cloud/system.data.md">制限</a>です。</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Mono.Data.Tds.dll</td>
    <td class="tg-yw4l">TDS プロトコルをサポートします。使用<a href="https://developer.xamarin.com/api/namespace/System.Data.SqlClient/">System.Data.SqlClient</a>内でサポート<a href="https://developer.xamarin.com/api/namespace/System.Data/">System.Data</a>です。</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Mono.Dynamic.Interpreter.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">Mono.Security.dll</td>
    <td class="tg-yw4l">暗号化 Api。</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">monotouch.dll</td>
    <td class="tg-yw4l">このアセンブリには、CocoaTouch API に c# バインドが含まれています。 これはクラシック iOS プロジェクト内で使用できるのみです。</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">MonoTouch.Dialog-1.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">MonoTouch.NUnitLite.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">mscorlib.dll</td>
    <td class="tg-yw4l"><a ref="https://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx">Silverlight</a></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">OpenTK-1.0.dll</td>
    <td class="tg-yw4l">OpenGL/OpenAL オブジェクト指向の Api では、拡張が iPhone デバイスのサポートを提供します。</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.dll</td>
    <td class="tg-yw4l"><a href="https://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx">Silverlight</a>、さらに次の名前空間の型。<br>System.Collections.Specialized<br>System.ComponentModel<br>System.ComponentModel.Design<br>System.Diagnostics<br>System.IO<br>System.IO.Compression<br>System.IO.Compression.FileSystem<br>System.Net<br>System.Net.Cache<br>System.Net.Mail<br>System.Net.Mime<br>System.Net.NetworkInformation<br>System.Net.Security<br>System.Net.Sockets<br>System.Runtime.InteropServices<br>System.Runtime.Versioning<br>System.Security.AccessControl<br>System.Security.Authentication<br>System.Security.Cryptography<br>System.Security.Permissions<br>System.Threading<br>System.Timers</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.ComponentModel.Composition.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.ComponentModel.DataAnnotations.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Core.dll</td>
    <td class="tg-yw4l"><a ref="https://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx">Silverlight</a></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Data.dll</td>
    <td class="tg-yw4l"><a href="http://msdn.microsoft.com/en-us/library/ms229335.aspx">.NET 3.5</a> 、<a href="~/ios/data-cloud/system.data.md">削除いくつかの機能を持つ</a>します。</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Data.Services.Client.dll</td>
    <td class="tg-yw4l">完全 oData クライアント。</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.IO.Compression</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.IO.Compression.FileSystem</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Json.dll</td>
    <td class="tg-yw4l"><a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx">Silverlight</a></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Net.Http.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Numerics.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Runtime.Serialization.dll</td>
    <td class="tg-yw4l"><a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx">Silverlight</a></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.ServiceModel.dll</td>
    <td class="tg-yw4l">WCF のスタック内に存在と<a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx">Silverlight</a></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.ServiceModel.Internals.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.ServiceModel.Web.dll</td>
    <td class="tg-yw4l"><a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx">Silverlight</a>、さらに次の名前空間の型。 <br>システム<br>System.ServiceModel.Channels<br>System.ServiceModel.Description<br>System.ServiceModel.Web</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Transactions.dll</td>
    <td class="tg-yw4l"><a href="http://msdn.microsoft.com/en-us/library/ms229335.aspx">.NET 3.5</a>の一部です。 <a href="~/ios/data-cloud/system.data.md">System.Data</a>をサポートします。</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Web.Services.dll</td>
    <td class="tg-yw4l">削除されたサーバー機能で、.NET 3.5 プロファイルから基本的な Web サービスです。</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Windows.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Xml.dll</td>
    <td class="tg-yw4l"><a href="http://msdn.microsoft.com/en-us/library/ms229335.aspx">.NET 3.5</a></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Xml.Linq.dll</td>
    <td class="tg-yw4l"><a href="http://msdn.microsoft.com/en-us/library/ms229335.aspx">.NET 3.5</a></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Xml.Serialization.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Xamarin.iOS.dll</td>
    <td class="tg-yw4l">このアセンブリには、CocoaTouch API に c# バインドが含まれています。 これは、統合 iOS プロジェクトでのみ使用します。</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">Java.Interop.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">Mono.Android.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">Mono.Android.Export.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">Mono.Posix.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.EnterpriseServices.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">Xamarin.Android.NUnitLite.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">Mono.CompilerServices.SymbolWriter.dll</td>
    <td class="tg-yw4l">コンパイラのライターです。</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Xamarin.Mac.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Drawing.dll</td>
    <td class="tg-yw4l">System.drawing の各 API - クラシック API のみです。<br>Xamarin.Mac .NET 4.5 またはモバイル フレームワークの Unified API では、System.Drawing がサポートされていません。<br>IOS および OS X を使用してに System.Drawing サポートを追加することができます、 <a href="https://github.com/mono/sysdrawing-coregraphics">sysdrawing coregraphics</a>ライブラリ</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-yw4l"></td>
    <td class="tg-p7et">✓</td>
  </tr>
</table>