---
ms.assetid: 814857C5-D54E-469F-97ED-EE1CAA0156BB
title: デスクトップ アプリの移植のガイダンス
description: UWP と Windows 10 だけなく、macOS、iOS、Android、上で実行するクロスプラット フォーム アプリを作成するには、既存の Windows フォームまたは WPF アプリを分離する方法の簡単な説明です。
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 3d3af9c78b7486e7ebfb063a3cb00fabdbd0f5b7
ms.sourcegitcommit: 6be6374664cd96a7d924c2e0c37aeec4adf8be13
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/13/2018
ms.locfileid: "51617541"
---
# <a name="desktop-app-porting-guidance"></a>デスクトップ アプリの移植のガイダンス

ほとんどのアプリケーション コードは、次の領域のいずれかに分類できます。

* ユーザー インターフェイスのコード (例: windows やボタンなど)
* サードパーティ製のコントロール (例: グラフ)
* ビジネス ロジック (例: 検証規則)
* ローカル データの格納とアクセス
* Web サービスとリモート データ アクセス

記述された Windows フォームや WPF アプリケーションのC#(または Visual Basic.NET) 驚くほどのビジネス ロジック、ローカル データ アクセス、および web サービスのコードをプラットフォーム間で共有できます。

## <a name="net-portability-analyzer"></a>.NET portability Analyzer

Visual Studio 2015 と 2017 のサポート、 [.NET Portability Analyzer](https://docs.microsoft.com/dotnet/articles/standard/portability-analyzer) ([Windows 用のダウンロード](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer))、既存のアプリケーションを確認し、コードの量現状有姿を他のプラットフォームに移植することがわかるができます. これから詳細情報を入手できる[Channel 9 のビデオ](https://channel9.msdn.com/Blogs/Seth-Juarez/A-Brief-Look-at-the-NET-Portability-Analyzer)します。

コマンド ライン ツールをからダウンロードできます[GitHub での移植性アナライザー](https://github.com/Microsoft/dotnet-apiport)と同じレポートを提供するために使用します。

## <a name="x-of-my-code-is-portable-what-next"></a>"x % のコードは移植可能です。 次のステップ"

うまくいけば、アナライザーを示しています、大規模なコードの一部は移植可能があるすべてのアプリの一部になる確実にする_できません_他のプラットフォームに移動します。

コードの別の部分は、以下で詳しく説明したこれらのバケットのいずれかに分類されます可能性があります。

* 再利用可能な移植可能なコード
* 変更を必要とするコード
* コードを移植できないし、再書き込みが必要です。

### <a name="re-useable-portable-code"></a>再利用可能な移植可能なコード

すべてのプラットフォームで利用可能な Api に対して記述された .NET コードには、クロス プラットフォームの変更を取得できます。 理想的には、このすべてのコードをポータブル クラス ライブラリ、ライブラリの共有、または .NET Standard ライブラリに移動し、既存のアプリ内でテストしてになります。

共有ライブラリは、その他のプラットフォーム (Android、iOS、macOS) などのアプリケーション プロジェクトに追加できます。

### <a name="code-that-requires-changes"></a>変更を必要とするコード

一部の .NET Api は、すべてのプラットフォームでできない場合があります。 これらの Api は、コードに存在する場合は、クロス プラットフォーム Api を使用してこれらのセクションを再作成する必要があります。

この追加の例は、リフレクション Api は .NET 4.6 で使用できますが、すべてのプラットフォームで使用されていないを使用します。

移植可能な Api を使用してコードを再作成した後、そのコードの共有ライブラリをパッケージ化し、既存のアプリ内でテストできる必要があります。

### <a name="code-that-isnt-portable-and-requires-a-re-write"></a>コードを移植できないし、再書き込みが必要です。

クロス プラットフォームである可能性があるコードの例を次のとおりです。

- **ユーザー インターフェイス**– Windows Forms または WPF の画面をたとえば Android や iOS では、上のプロジェクトで使用できません。 ユーザー インターフェイスを再記述する必要はこれを使用して[コントロールの比較](~/cross-platform/desktop/controls/index.md)として参照します。

- **プラットフォーム固有の記憶域**-(ローカルの SQL Server Express データベース) などのプラットフォームに固有のテクノロジに依存するコードです。 再書き込みこれから (データベース エンジンに SQLite) など、クロスプラット フォーム対応の代替を使用する必要があります。
一部のファイル システム操作は、UWP に Android および iOS (例: に若干異なる Api があるため、調整する必要もあります。 いくつかのファイル システムは、大文字と他のユーザーはありません)。

- **サード パーティ製コンポーネント**– アプリケーションでサード パーティ製のコンポーネントが他のプラットフォームで使用できるかどうかを確認します。 他のユーザー (特に visual コントロール グラフやメディア プレーヤーなど) が使用可能ないくつか非ビジュアルの NuGet パッケージなどがあります。

## <a name="tips-for-making-code-portable"></a>コードの可搬するためのヒント

- **依存関係の注入**– 各プラットフォームでは、さまざまな実装を提供し、

- **アプローチを階層化**– MVVM、MVC、MVP、またはするのに役立つその他のいくつかのパターンは、プラットフォーム固有のコードから移植可能なコードを区切るかどうか。

- **メッセージング**– コードでメッセージ パッシングを使用して、アプリケーションの異なる部分間の相互作用を結合解除できます。
