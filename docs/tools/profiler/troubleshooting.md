---
title: Xamarin プロファイラーのトラブルシューティング
description: このドキュメントでは、Xamarin プロファイラーに関連するトラブルシューティングの情報を提供します。 これには、ログ記録と診断、IDE、およびその他のトピックに関連する問題について説明します。
ms.prod: xamarin
ms.assetid: 0060E9D1-C003-4E4C-ADE8-B406978FE891
author: topgenorth
ms.author: toopge
ms.date: 10/27/2017
ms.openlocfilehash: 71faf79ef9b783480dbb6ff4674859a9148abca3
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066911"
---
# <a name="xamarin-profiler-troubleshooting"></a>Xamarin プロファイラーのトラブルシューティング

## <a name="logging-and-diagnostics"></a>ログ記録と診断

Xamarin チームの情報を提示する場合は、問題を追跡するのに役立つなど。

- 問題、クラッシュ、または障害およびに至るまで、ワークフロー スクリーン キャスト。
- (下記参照) をログに出力します。
- **.Mlpd**プロファイリング セッション (下記参照) の生成中です。

### <a name="getting-log-outputs"></a>ログ出力を取得します。

Mac 上にログの保存`~/Library/Logs/Xamarin.Profiler/Profiler.<date>.log`です。

Windows でこれらに保存されて。`%appdata%Local//Xamarin/Log/Xamarin.Profiler/Profiler.<date>.log`懸案事項を送信するたびに、最新のログを含めてください。

詳細ログのため、この出力は発展してさらに便利な時間の経過と共に、進むとして追加しています。

<a name="gen_mlpd" />

### <a name="generating-mlpd-files"></a>.Mlpd ファイルを生成します。

**.Mlpd**ファイルは、mono ランタイム プロファイラーの圧縮された出力します。 Xamarin プロファイラー GUI がからデータを読み取る、 **.mlpd**ユーザーを表示します。 **.mlpd**ファイルは、マイクロソフトのエンジニア、プロファイラーは、データである可能性がありますの問題を診断できるようにするために、Xamarin のデバッグ ツールが役立ちます。

**.Mlpd** 、Mac ので、現在のセッションが自動的に保存用`/tmp`ディレクトリ、およびタイムスタンプで識別できます。 ログ記録を有効にする場合、最初の出力がへのパスをされます、 **.mlpd**ファイル。 **.Mlpd**ファイルは通常 ~/var/フォルダーの開始ディレクトリに保存されます。

**.Mlpd**を選択して、現在のセッションを保存することもの**ファイル > 名前を付けて保存しています.** プロファイラーのメニューには。

**Visual Studio for Mac**:

![](troubleshooting-images/image17.png "Visual studio for Mac .mlpd ファイルの保存")

**Visual Studio**:

![](troubleshooting-images/image17-vs.png "Visual Studio で .mlpd ファイルの保存")

重要な点は **.mlpd**多く情報にはが含まれており、ファイルのサイズが大きくなります。

## <a name="troubleshooting"></a>トラブルシューティング

一般的な潜在的な問題、回避策、およびに関するヒントとテクニック、プロファイラーを使用して、次の一覧が表示されます。

> [!NOTE]
> **注**: Visual Studio でなければならない**エンタープライズ**for mac Windows 上のいずれかの Visual Studio Enterprise または Visual Studio でこの機能のロックを解除するサブスクライバー

#### <a name="i-cant-see-the-ios-profiler-option-or-it-is-greyed-out-visual-studio-and-visual-studio-for-mac"></a>IOS プロファイラー オプションが表示されない、または [Visual Studio と Visual Studio for Mac] 淡色表示には

これを解決するのには、次の設定を確認します。

- デバッグ構成を使用していることを確認してください。
- SGen ガベージ コレクターを使用していることを確認します。
- プラットフォームが[サポート](~/tools/profiler/index.md#Profiler_Support)です。
- 右側のライセンスがあることを確認します。
- 内で適切に認証されたログインしていることを確認します。
- [Visual Studio]使用する必要があります[Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/enterprise/)有効なエンタープライズ ライセンスを所有します。

#### <a name="i-get-an-error-when-i-try-to-launch-the-profiler"></a>プロファイラーを起動するときにエラーが発生します。

発生した場合は、このエラー ボックス Visual Studio のプロファイラーを使用する場合。

![](troubleshooting-images/error.png "Visual Studio のプロファイラーを使用する場合、エラー ボックス")

シミュレーターを起動できなくなる原因は通常/エミュレーターです。 再試行してください、通常、アプリの実行を利用できますが、プロファイラーを再度使用する再試行の問題を修正します。

#### <a name="to-watch-a-specific-thread"></a>特定のスレッドを監視するには

具体的には監視に必要なスレッドがある場合が理想的に get を取得できるように、その作成の非常に最初のスレッドを名前`ThreadName`の代わりに`0x0`です。 Ui スレッド名を設定する例では、次のコードを使用する可能性があります。

```csharp
RunOnUiThread (() => {
  Thread.CurrentThread.Name  = "UI";
});
```

## <a name="related-links"></a>関連リンク

- [Xamarin プロファイラーを使用して、チュートリアル](~/tools/profiler/index.md)
- [メモリおよびパフォーマンスのベスト プラクティス](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [リリース ノート](https://developer.xamarin.com/releases/profiler/preview/)
