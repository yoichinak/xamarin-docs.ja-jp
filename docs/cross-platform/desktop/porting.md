---
ms.assetid: 814857C5-D54E-469F-97ED-EE1CAA0156BB
title: デスクトップ アプリへの移植のガイド
description: UWP と Windows 10 に加え、macOS、iOS、Android、上で実行するクロスプラット フォーム アプリを作成するには、既存の Windows フォームや WPF アプリを分離する方法の簡単な説明。
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: cdf70065893a4da268f628369fa94336291ead1f
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
---
# <a name="desktop-app-porting-guidance"></a>デスクトップ アプリへの移植のガイド

ほとんどのアプリケーション コードは、次の領域のいずれかに分類できます。

* ユーザー インターフェイスのコード。 windows やボタンなど)
* サード パーティ コントロール。 グラフ)
* ビジネス ロジック (例です。 検証規則)
* ローカル データの格納とアクセス
* Web サービス、およびリモート データ アクセス

Windows フォームと WPF のビジネス ロジックの驚くほどの量に c# (または Visual Basic.NET) で記述されたアプリケーションでは、ローカル データにアクセスして、web サービスのコードは、プラットフォーム間で共有することができます。

## <a name="net-portability-analyzer"></a>.NET 移植性アナライザー

Visual Studio 2015 と 2017 サポート、 [.NET 移植性アナライザー](https://docs.microsoft.com/en-us/dotnet/articles/standard/portability-analyzer) ([for Windows をダウンロード](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer))、既存のアプリケーションを確認し、コードの量現状有姿の他のプラットフォームに移植するを指定することができます. 詳細情報を入手するにはこれから[Channel 9 ビデオ](https://channel9.msdn.com/Blogs/Seth-Juarez/A-Brief-Look-at-the-NET-Portability-Analyzer)です。

コマンド ライン ツールからダウンロードできます。 [GitHub での移植性アナライザー](https://github.com/Microsoft/dotnet-apiport)レポートと同じレポートを提供するために使用します。

## <a name="x-of-my-code-is-portable-what-next"></a>"x % のコードの移植性がします。 次のですか?"

できれば、アナライザーを示しています大規模なコードの部分は、移植性がありますが確実にしようとするすべてのアプリのいくつかの部分であるを_できません_他のプラットフォームに移動します。

コードの別の部分で詳しく説明されているこれらのバケットのいずれかに分類されます可能性があります。

* 再利用可能なコードの移植性
* 変更を必要とするコード
* コードをポータブルでないし、再書き込みが必要です。

### <a name="re-useable-portable-code"></a>再利用可能なコードの移植性

すべてのプラットフォームで利用可能な Api に対して記述された .NET コードには、プラットフォーム間の変更を実行できます。 理想的には、このコードのすべてをポータブル クラス ライブラリ、ライブラリの共有、または .NET の標準ライブラリに移動し、既存のアプリ内でテストすることができます。

その共有ライブラリは、その他のプラットフォーム (Android、iOS、macOS) などのアプリケーション プロジェクトに追加できます。

### <a name="code-that-requires-changes"></a>変更を必要とするコード

一部の .NET Api は、すべてのプラットフォームでは使用できない可能性があります。 これらの Api は、コードに存在する場合は、クロス プラットフォーム Api を使用してこれらのセクションを再書き込みする必要があります。

この以下の例は、.NET 4.6 では利用では使用できないすべてのプラットフォームでのリフレクション Api の使用します。

移植可能な Api を使用してコードを再作成した後、そのコードの共有ライブラリをパッケージ化し、既存のアプリ内でテストすることができます。

### <a name="code-that-isnt-portable-and-requires-a-re-write"></a>コードをポータブルでないし、再書き込みが必要です。

クロスプラット フォームである可能性のないコードの例については、次のとおりです。

- **ユーザー インターフェイス**: Windows フォームや WPF の画面をたとえば Android または iOS、上のプロジェクトで使用できません。 これを使用して再記述する必要があるユーザー インターフェイス[コントロール比較](~/cross-platform/desktop/controls/index.md)として参照します。

- **プラットフォーム固有の記憶域**-(ローカル SQL Server Express データベースなど)、プラットフォーム固有のテクノロジに依存するコードです。 再書き込みこれから (データベース エンジンに対して SQLite) など、クロスプラット フォームの代替を使用する必要があります。
一部のファイル システム操作しなければならない場合も調整されるため、UWP に Android および iOS を (若干異なる Api いくつかのファイル システムは、大文字といないもの)。

- **サード パーティ コンポーネント**: サード パーティのコンポーネント、アプリケーションが他のプラットフォームで利用可能かどうかを確認します。 他のユーザー (特に visual コントロール内のグラフやメディア プレーヤーのような) が非ビジュアルの NuGet パッケージなど、一部できない可能性があります。

## <a name="tips-for-making-code-portable"></a>コードの可搬に関するヒント

- **依存関係の挿入**– プラットフォームごとに異なる実装を提供し、

- **階層型アプローチ**– MVVM、MVC、MVP、またはするのに役立つその他のいくつかのパターンは、プラットフォーム固有のコードから移植可能なコードを分離するかどうか。

- **メッセージング**– アプリケーションのさまざまな部分の間のやり取りを結合解除する、コードでメッセージの受け渡しを行うこともできます。
