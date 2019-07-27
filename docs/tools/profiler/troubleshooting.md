---
title: Xamarin Profiler のトラブルシューティング
description: このドキュメントでは、Xamarin Profiler に関するトラブルシューティング情報を提供します。 ログ記録と診断、IDE、およびその他のトピックに関連する問題について説明します。
ms.prod: xamarin
ms.assetid: 0060E9D1-C003-4E4C-ADE8-B406978FE891
author: lobrien
ms.author: laobri
ms.date: 10/27/2017
ms.openlocfilehash: d6125f9d8dd2899a19181f4ccd7f9c06b06e5d5b
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68511577"
---
# <a name="xamarin-profiler-troubleshooting"></a>Xamarin Profiler のトラブルシューティング

## <a name="logging-and-diagnostics"></a>ログ記録と診断

Xamarin チームは、次のような情報を提供すると、問題を追跡するのに役立ちます。

- 問題、クラッシュ、または障害のスクリーンキャストと、ワークフローがそれにつながる。
- ログ出力 (下記参照)。
- プロファイルセッション用に生成される**mlpd** (下記参照)。

### <a name="getting-log-outputs"></a>ログ出力の取得

Mac では、ログは`~/Library/Logs/Xamarin.Profiler/Profiler.<date>.log`に保存されます。

Windows では、問題を`%appdata%Local//Xamarin/Log/Xamarin.Profiler/Profiler.<date>.log`送信するたびに最新のログが保存されます。

ここでは、ログ記録を追加しています。この出力は、時間の経過と共に増加し、より有用になります。

<a name="gen_mlpd" />

### <a name="generating-mlpd-files"></a>Mlpd ファイルを生成しています

**Mlpd**ファイルは、mono ランタイムプロファイラーの圧縮された出力です。 Xamarin Profiler GUI は、 **mlpd**からデータを読み取り、ユーザーに対して表示します。 **mlpd**ファイルは、プロファイラーがデータに関する問題を診断するのに役立つ、Xamarin 用のデバッグツールとして便利です。

現在のセッションの**mlpd**は、Mac の`/tmp`ディレクトリに自動的に保存され、タイムスタンプで識別できます。 ログ記録を有効にすると、最初の出力が**mlpd**ファイルへのパスになります。 Mlpd ファイルは通常、~/var/folders... のディレクトリに保存され**ます。**

現在のセッションの**mlpd**を保存するには、**ファイル > 名前を付けて保存...** プロファイラーのメニューから次のようにします。

**Visual Studio for Mac**:

![](troubleshooting-images/image17.png "Visual Studio for Mac に mlpd ファイルを保存しています")

**Visual Studio**:

![](troubleshooting-images/image17-vs.png "Visual Studio での mlpd ファイルの保存")

これは重要なことです **。 mlpd**には多くの情報が含まれており、ファイルサイズは大きくなることに注意してください。

## <a name="troubleshooting"></a>トラブルシューティング

次の一覧は、プロファイラーを使用する際の一般的な注意事項、回避策、ヒントとテクニックを示しています。

> [!NOTE]
> Windows または Visual Studio for Mac の Visual Studio Enterprise で、この機能のロックを解除するには、Visual Studio **Enterprise**サブスクライバーである必要があります。

#### <a name="i-cant-see-the-ios-profiler-option-or-it-is-greyed-out-visual-studio-and-visual-studio-for-mac"></a>IOS profiler オプションが表示されない、またはグレー表示になっている [Visual Studio および Visual Studio for Mac]

これを解決するには、次の設定を確認します。

- デバッグ構成を使用していることを確認する
- SGen ガベージコレクターを使用していることを確認します。
- プラットフォームが[サポートさ](~/tools/profiler/index.md#Profiler_Support)れていることを確認します。
- 適切なライセンスを持っていることを確認します。
- ログインし、適切に認証されていることを確認します。
- [Visual Studio][Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/enterprise/)を使用し、有効なエンタープライズライセンスを持っている必要があります。

#### <a name="i-get-an-error-when-i-try-to-launch-the-profiler"></a>プロファイラーを起動しようとするとエラーが発生する

Visual Studio でプロファイラーを使用するときにこのエラーボックスが表示される場合は、次のようにします。

![](troubleshooting-images/error.png "Visual Studio でプロファイラーを使用する場合のエラーボックス")

通常、シミュレーターまたはエミュレーターに起動できないことが原因です。 通常はアプリを正常に実行し、それによって得られる問題を修正してから、もう一度プロファイラーを使用してみてください。

#### <a name="to-watch-a-specific-thread"></a>特定のスレッドを監視するには

特に観察したいスレッドがある場合は、作成の先頭にあるスレッドに名前を付けることで、ではなくを取得`ThreadName`するの`0x0`が理想的です。 たとえば、スレッド名をとして`UI`設定するには、次のコードを使用します。

```csharp
RunOnUiThread (() => {
  Thread.CurrentThread.Name  = "UI";
});
```

## <a name="related-links"></a>関連リンク

- [チュートリアル-Xamarin Profiler の使用](~/tools/profiler/index.md)
- [メモリとパフォーマンスのベストプラクティス](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [リリース ノート](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/profiler/preview/index.md)
