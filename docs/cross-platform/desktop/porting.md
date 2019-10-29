---
ms.assetid: 814857C5-D54E-469F-97ED-EE1CAA0156BB
title: デスクトップアプリの移植に関するガイダンス
description: 既存の Windows フォームまたは WPF アプリを分離して、macOS、iOS、Android、UWP/Windows 10 で実行するクロスプラットフォームアプリを作成する方法について簡単に説明します。
author: davidortinau
ms.author: daortin
ms.date: 04/26/2017
ms.openlocfilehash: 181034a4936b2da010a2fcd280ded1a3419d43ae
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016449"
---
# <a name="desktop-app-porting-guidance"></a>デスクトップアプリの移植に関するガイダンス

ほとんどのアプリケーションコードは、次のいずれかの領域に分類できます。

- ユーザーインターフェイスコード (例 ウィンドウとボタン)
- サードパーティ製のコントロール (例 棒
- ビジネスロジック (例 検証規則)
- ローカルデータの格納とアクセス
- Web サービスとリモートデータアクセス

(または Visual Basic.NET) でC#記述された WINDOWS フォームおよび WPF アプリケーションの場合、ビジネスロジック、ローカルデータアクセス、および web サービスのコードは、プラットフォーム間で共有できます。

## <a name="net-portability-analyzer"></a>.NET 移植性アナライザー

Visual Studio 2017 以降では、 [.Net 移植性アナライザー](https://docs.microsoft.com/dotnet/articles/standard/portability-analyzer) ([Windows 用ダウンロード](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer)) がサポートされています。これにより、既存のアプリケーションを調査し、他のプラットフォームに対して "その他" のコードを移植することができます。 詳細については、 [Channel 9 のビデオ](https://channel9.msdn.com/Blogs/Seth-Juarez/A-Brief-Look-at-the-NET-Portability-Analyzer)を参照してください。

また、 [GitHub の移植性アナライザー](https://github.com/Microsoft/dotnet-apiport)からコマンドラインツールをダウンロードし、同じレポートを提供するために使用することもできます。

## <a name="x-of-my-code-is-portable-what-next"></a>"x% のコードは移植可能です。 次は何ですか? "

Analyzer は、コードの大部分が移植可能であることを示していますが、他のプラットフォームに移動_できない_すべてのアプリの一部であることは確かです。

コードのチャンクは、次のいずれかのバケットに分類されることがあります。詳細については、以下を参照してください。

- 再有効な移植可能なコード
- 変更が必要なコード
- 移植できず、再書き込みが必要なコード

### <a name="re-useable-portable-code"></a>再有効な移植可能なコード

すべてのプラットフォームで使用可能な Api に対して記述された .NET コードは、変更されていないクロスプラットフォームにすることができます。 理想的には、すべてのコードをポータブルクラスライブラリ、共有ライブラリ、または .NET Standard ライブラリに移動し、既存のアプリ内でテストすることができます。

その共有ライブラリは、他のプラットフォーム (Android、iOS、macOS など) のアプリケーションプロジェクトに追加できます。

### <a name="code-that-requires-changes"></a>変更が必要なコード

一部の .NET Api は、すべてのプラットフォームで使用できるとは限りません。 これらの Api がコードに存在する場合は、クロスプラットフォーム Api を使用するようにこれらのセクションを再記述する必要があります。

この例には、.NET 4.6 で利用できるリフレクション Api の使用が含まれていますが、すべてのプラットフォームで使用できるわけではありません。

移植可能な Api を使用してコードを書き直すと、そのコードを共有ライブラリにパッケージ化して、既存のアプリ内でテストできるようになります。

### <a name="code-that-isnt-portable-and-requires-a-re-write"></a>移植できず、再書き込みが必要なコード

クロスプラットフォームになる可能性のないコードの例を次に示します。

- **ユーザーインターフェイス**– Windows フォームまたは WPF 画面は、Android または iOS 上のプロジェクトでは使用できません。たとえば、のようになります。 この[コントロールの比較](~/cross-platform/desktop/controls/index.md)を参照として使用して、ユーザーインターフェイスを再記述する必要があります。

- プラットフォーム固有の**ストレージ**-プラットフォーム固有のテクノロジ (ローカル SQL Server Express データベースなど) に依存するコード。 クロスプラットフォームの代替手段 (データベースエンジン用 SQLite など) を使用して、これを再記述する必要があります。
UWP は Android と iOS に若干異なる Api を使用しているため、一部のファイルシステム操作も調整する必要があります (例として、 一部のファイルシステムでは大文字と小文字が区別され、他のファイルは区別されません

- **サードパーティのコンポーネント**–アプリケーションのサードパーティコンポーネントが他のプラットフォームで利用可能かどうかを確認します。 Visual NuGet 以外のパッケージなど、一部は使用できる場合があります (特にグラフやメディアプレーヤーのようなビジュアルコントロール)。

## <a name="tips-for-making-code-portable"></a>コードの移植性を高めるためのヒント

- **依存関係の注入**–プラットフォームごとに異なる実装を提供します。

- **レイヤー**化されたアプローチ– MVVM、MVC、MVP など、プラットフォーム固有のコードからポータブルコードを分離するのに役立つその他のパターン。

- **メッセージング**–コードに渡すメッセージを使用して、アプリケーションの異なる部分間の相互作用を逆にすることができます。
