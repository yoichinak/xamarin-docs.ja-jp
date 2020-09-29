---
title: Xamarin を使用した DevOps
ms.assetid: ff978cc2-5a25-46d6-921b-e51adaa65992
author: davidortinau
ms.author: daortin
manager: crdun
ms.workload:
- xamarin
ms.date: 10/23/2018
ms.openlocfilehash: 61a7017d2ba784770d1199b6332d781b36b6d0e0
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91452480"
---
# <a name="devops-with-xamarin"></a>Xamarin を使用した DevOps

Xamarin では、Android、iOS、および Windows を対象とするクロスプラットフォームのモバイル アプリを、C#、.NET、および Visual Studio を使用して作成することができます。 Xamarin を利用すると、コードの大部分をプラットフォーム間で共有し、プラットフォーム固有にする必要のあるコードをごく一部に抑えることができます。

最新のプラットフォーム向けのアプリ開発には、コードを記述するだけでなく、それ以外の多くのアクティビティが関係します。 これらのアクティビティは、アプリの完全なライフサイクルの中で DevOps (開発 + 運用) 期間になされるものあり、たとえば、作業のアジャイルな計画と追跡、コードの設計と実装、ソース コード リポジトリの管理、ビルドの実行、継続的に実行されるインテグレーションと展開の管理、テスト (単体テストと UI テストを含む)、開発環境と運用環境の両方におけるさまざまな形式の診断の実行、製品利用統計情報と分析によるアプリのパフォーマンスとユーザー動作のリアルタイムな監視などがあります。

Visual Studio、Azure DevOps Services、Team Foundation Server は、さまざまな DevOps 機能を提供しています。 これらの多くは、クロスプラットフォームのプロジェクトに完全に適用されます。 Xamarin アプリでは特にそう言えます。C# と .NET で構築し、いくつかの DevOps ツールはそれらを中心に構築されているからです。 その他のツールでは、ビルド環境およびランタイム環境との緊密な統合が必要です。 Xamarin アプリは Windows 以外のプラットフォームでも実行でき、.NET の Mono 実装を使用しているため、Xamarin では特定の必要に合わせて専門ツールが提供されます。

次の表は、Xamarin プロジェクトで動作する Visual Studio の DevOps 機能と制限がある機能を示しています。 各機能そのものの詳細については、リンク先のドキュメントを参照してください。

## <a name="agile-tools"></a>アジャイル ツール

参照リンク: **[アジャイル ツールとアジャイル プロジェクト管理の概要](/azure/devops/boards/backlogs/backlogs-overview?view=azure-devops)**

一般的なコメント: すべての計画機能と追跡機能は、プロジェクトの種類とコーディング言語には依存しません。

|機能|Xamarin でサポートされているかどうか|その他のコメント|
|-------------|----------------------------|-------------------------|
|バックログとスプリントの管理|はい||
|作業の追跡|はい||
|チーム ルーム コラボレーション|はい||
|かんばんボード|はい||
|進行状況のレポートと視覚化|はい||

## <a name="modeling"></a>モデリング

参照リンク: **[アーキテクチャの分析とモデル化](/visualstudio/modeling/analyze-and-model-your-architecture)**

デザイン機能は、コーディング言語に依存しないか、または C# のような .NET 言語と一緒に機能します。 コードに関連する側面については、「 [ソフトウェア開発におけるアーキテクチャとモデリング図の役割](/visualstudio/modeling/scenario-change-your-design-using-visualization-and-modeling#ModelingDiagramsTools) 」を参照してください。

|機能|Xamarin でサポートされているかどうか|その他のコメント|
|-------------|----------------------------|-------------------------|
|シーケンス図|はい||
|依存関係グラフ|はい||
|呼び出し階層|はい||
|クラス デザイナー|はい||
|アーキテクチャ エクスプローラー|はい||
|UML 図 (ユース ケース、アクティビティ、クラス、コンポーネント、シーケンス、および DSL)|はい||
|レイヤー図|はい||
|レイヤー検証|はい||

## <a name="code"></a>コード

|機能|Xamarin でサポートされているかどうか|その他のコメント|
|-------------|----------------------------|-------------------------|
|[Team Foundation バージョン管理 (TFVC)](/azure/devops/repos/tfvc/overview?view=vsts) または Azure Repos を使用する|はい||
|[Azure Repos で Git を使用した作業の開始](/azure/devops/repos/git/gitquickstart?view=vsts&tabs=visual-studio)|はい||
|[コード品質の向上](/visualstudio/test/improve-code-quality)|はい||
|[コード変更およびその他の履歴の検索](/visualstudio/ide/find-code-changes-and-other-history-with-codelens)|はい|ただし、実行時まで実装が解決しない、プラットフォームに固有の境界をまたぐ場合を除きます。|
|[コード マップを使用してアプリケーションをデバッグする](/visualstudio/modeling/use-code-maps-to-debug-your-applications)|はい||

## <a name="build"></a>ビルド

参照リンク: **[Azure Pipelines](/azure/devops/pipelines/index?view=vsts)**

|機能|Xamarin でサポートされているかどうか|その他のコメント|
|-------------|----------------------------|-------------------------|
|オンプレミス TFS サーバー|はい|ビルド コンピューターに Xamarin がインストールされている必要があります。iOS 用にビルドするには、OSX コンピューターにリンクできる必要があります。 [TFVC の使用](/azure/devops/repos/tfvc/overview?view=vsts)|
|Azure Pipelines にリンクされたオンプレミスのビルド サーバー|はい|手順については、「[Build and release agents](/azure/devops/pipelines/agents/agents?view=vsts)」 (ビルド エージェントとリリース エージェント) を参照してください。|
|Azure Pipelines のホスト コント ローラー サービス|はい|「[Build your Xamarin app](/azure/devops/pipelines/languages/xamarin?view=vsts&tabs=vsts)」 (Xamarin アプリのビルド) を参照してください。|
|事前スクリプトと事後スクリプトによるビルド定義|はい||
|継続的な統合 (ゲート チェックインを含む)|はい|Git としての TFVC へのゲート チェックインのみ、チェックイン モデルではなく、プル要求モデルで機能します。|

## <a name="test"></a>テスト

|機能|Xamarin でサポートされているかどうか|その他のコメント|
|-------------|----------------------------|-------------------------|
|テストの計画、テスト ケースの作成、およびテスト スイートの編成|はい||
|手動テスト|はい||
|テスト マネージャー (テストの記録と再生)|はい|Windows デバイスと Android エミュレーター (Visual Studio からのみ)。|
|コード カバレッジ|N/A||
|[コードの単体テスト](/visualstudio/test/unit-test-your-code/)|はい|Windows と Android を対象にする場合は、組み込みの MSTest ツールを使用できます。 Windows、Android、および iOS で単体テストを実行するには、Xamarin では NUnit が推奨されています。 「[TFVC の使用](/azure/devops/repos/tfvc/overview?view=vsts)」を参照してください。|
|[UI オートメーションを使用してコードをテストする](/visualstudio/test/use-ui-automation-to-test-your-code/)|Windows のみ|Visual Studio の UI テスト レコーダーは Windows のみです。 すべてのプラットフォームについては、[Xamarin.UITest](/appcenter/test-cloud/uitest/) を参照してください。|

## <a name="improve-code-quality"></a>コード品質の向上

参照リンク: **[コード品質の向上](/visualstudio/test/improve-code-quality)**

|機能|Xamarin でサポートされているかどうか|その他のコメント|
|-------------|----------------------------|-------------------------|
|[マネージド コードの品質の分析](/visualstudio/code-quality/analyzing-managed-code-quality-by-using-code-analysis)|はい||
|[コード クローン検出を使用した重複コードの検出](/previous-versions/hh205279(v=vs.140))|はい||
|[マネージド コードの複雑さと保守性の測定](/visualstudio/code-quality/measuring-complexity-and-maintainability-of-managed-code)|はい||
|[パフォーマンス エクスプローラー](/visualstudio/profiling/performance-explorer)|いいえ|代わりに Visual Studio for Mac [Xamarin Profiler](../profiler/index.md) を使用します。 Xamarin プロファイラーは現在プレビュー期間中であり、Windows を対象にした場合はまだ動作しないことに注意してください。|
|[.NET Framework のメモリ分析の問題](/visualstudio/misc/analyze-dotnet-framework-memory-issues)|いいえ|Visual Studio ツールには、プロファイリング用の Mono フレームワークへのフックはありません。|

## <a name="release-management"></a>リリース管理

参照リンク: ** [AZURE PIPELINES と TFS でのビルドとリリース](/azure/devops/pipelines/overview?view=vsts)**

|機能|Xamarin でサポートされているかどうか|その他のコメント|
|-------------|----------------------------|-------------------------|
|リリース プロセスの管理|はい||
|スクリプトによるサイドローディング用のサーバーへの配置|はい||
|アプリ ストアへのアップロード|Partial|一部のアプリ ストアに対して、このプロセスを自動化することができる拡張機能が使用できます。  たとえば、[Google Play の拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-vsclient.google-play)については、[Azure DevOps Services の拡張機能](https://marketplace.visualstudio.com/VSTS)を参照してください。|

## <a name="monitor-with-hockeyapp"></a>HockeyApp による監視

参照リンク: **[HockeyApp による監視](https://www.hockeyapp.net/features/)**

|機能|Xamarin でサポートされているかどうか|その他のコメント|
|-------------|----------------------------|-------------------------|
|クラッシュ分析、製品利用統計情報、およびベータ版の配布|はい||