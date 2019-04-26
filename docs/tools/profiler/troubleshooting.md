---
title: Xamarin Profiler のトラブルシューティング
description: このドキュメントでは、Xamarin Profiler に関連するトラブルシューティングの情報を提供します。 これには、ログ記録と診断、IDE、およびその他のトピックに関連する問題について説明します。
ms.prod: xamarin
ms.assetid: 0060E9D1-C003-4E4C-ADE8-B406978FE891
author: lobrien
ms.author: laobri
ms.date: 10/27/2017
ms.openlocfilehash: f9b4da5b6dfe3f0254340d9175b08198bd52a45a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61160564"
---
# <a name="xamarin-profiler-troubleshooting"></a>Xamarin Profiler のトラブルシューティング

## <a name="logging-and-diagnostics"></a>ログ記録と診断

Xamarin チームの情報を提供する場合は、問題を追跡するのに役立つなど。

- 問題、クラッシュ、または失敗、およびそれに至るまで、ワークフローのスクリーン キャスト。
- (下記参照) をログに出力されます。
- **.Mlpd**プロファイリング セッション (下記参照) 用に生成されます。

### <a name="getting-log-outputs"></a>ログ出力を取得します。

Mac 上にログの保存`~/Library/Logs/Xamarin.Profiler/Profiler.<date>.log`します。

Windows 上に保存されます`%appdata%Local//Xamarin/Log/Xamarin.Profiler/Profiler.<date>.log`問題を送信するたびに、最新のログを含めてください。

この出力は、増減して時間の経過と共により有用になるため、進むと、詳細ログ記録を追加いたします。

<a name="gen_mlpd" />

### <a name="generating-mlpd-files"></a>.Mlpd ファイルを生成します。

**.Mlpd**ファイルが、mono ランタイム プロファイラーの圧縮された出力されます。 Xamarin Profiler の GUI はからデータを読み取り、 **.mlpd**し、ユーザーに表示します。 **.mlpd**ファイルは、マイクロソフトのエンジニアが、Profiler は、データである可能性がありますの問題を診断できるようにするために、Xamarin 用デバッグ ツールが役立ちます。

**.Mlpd**で Mac の現在のセッションが自動的に保存の`/tmp`ディレクトリ、タイムスタンプで識別できます。 最初の出力がへのパスをするログ記録を有効にした場合、 **.mlpd**ファイル。 **.Mlpd** ~/var/フォルダーの開始ディレクトリにファイルを保存通常されます.

**.Mlpd**を選択して現在のセッションを保存することもの**ファイル > 名前を付けて保存.** Profiler のメニューには。

**Visual Studio for Mac**:

![](troubleshooting-images/image17.png "Visual studio for Mac .mlpd ファイルを保存しています")

**Visual Studio**:

![](troubleshooting-images/image17-vs.png "Visual Studio で .mlpd ファイルの保存")

注意することが重要 **.mlpd**多くの情報を含み、ファイル サイズが大きくなります。

## <a name="troubleshooting"></a>トラブルシューティング

共通の潜在的な問題、回避策、およびヒントとテクニックについて、Profiler を使用して、次の一覧が表示されます。

> [!NOTE]
> **注**: Visual Studio を使用する必要があります**Enterprise** for mac で Windows のいずれかの Visual Studio Enterprise または Visual Studio でこの機能のロックを解除するサブスクライバー

#### <a name="i-cant-see-the-ios-profiler-option-or-it-is-greyed-out-visual-studio-and-visual-studio-for-mac"></a>IOS プロファイラー オプションを表示できないまたはグレー [Visual Studio と Visual Studio for Mac]

これを解決するのには、次の設定を確認します。

- デバッグ構成を使用していることを確認します。
- SGen ガベージ コレクターを使用していることを確認します。
- プラットフォームが[サポート](~/tools/profiler/index.md#Profiler_Support)します。
- 適切なライセンスがあることを確認します。
- 適切に認証されたログインしていることを確認します。
- [Visual Studio]必要がありますを使用して[Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/enterprise/)有効なエンタープライズ ライセンスを所有します。

#### <a name="i-get-an-error-when-i-try-to-launch-the-profiler"></a>プロファイラーを起動しようとするとエラーが発生しました

Visual Studio で、プロファイラーの使用時にこのエラーのボックスに実行すると: 場合

![](troubleshooting-images/error.png "Visual Studio で、プロファイラーを使用する場合、エラー ボックス")

シミュレーターを起動することであるため、通常は、/エミュレーター。 お試しくださいと通常、アプリの実行、利用できますが、し、もう一度、Profiler を使用しようが問題を解決します。

#### <a name="to-watch-a-specific-thread"></a>特定のスレッドを監視するには

具体的には監視に必要なスレッドがある場合に名前を取得するには、その作成の最初に、スレッドの最適ななります`ThreadName`の代わりに`0x0`します。 たとえば、スレッド名を設定する`UI`、次のコードを使用できます。

```csharp
RunOnUiThread (() => {
  Thread.CurrentThread.Name  = "UI";
});
```

## <a name="related-links"></a>関連リンク

- [チュートリアル - Xamarin Profiler の使用](~/tools/profiler/index.md)
- [メモリとパフォーマンスのベスト プラクティス](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [リリース ノート](https://developer.xamarin.com/releases/profiler/preview/)
