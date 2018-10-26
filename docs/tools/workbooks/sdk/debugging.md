---
title: デバッグの統合
description: このドキュメントは、Xamarin Workbooks の統合、エージェント側とクライアント側では、Windows とファルダの両方をデバッグする方法を説明します
ms.prod: xamarin
ms.assetid: 90143544-084D-49BF-B44D-7AF943668F6C
author: lobrien
ms.author: laobri
ms.date: 06/19/2018
ms.openlocfilehash: 86d9c6af93e7f59eb0e819730e46324688df7566
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106001"
---
# <a name="debugging-integrations"></a>デバッグの統合

## <a name="debugging-agent-side-integrations"></a>デバッグのエージェント側の統合

によって提供されるログ記録メソッドを使用して、最適に処理されるエージェント側の統合をデバッグ、`Log`クラス`Xamarin.Interactive.Logging`します。 参照してください、 [ `API docs` ](https://developer.xamarin.com/api/type/Xamarin.Interactive.Logging.Log/)を呼び出すメソッド。

Macos では、ログ メッセージが、ログ ビューアー メニューに表示されます (**ウィンドウ > ログ ビューアー**) と、クライアント ログに記録します。 Windows でのメッセージのみ表示されますクライアント ログに記録はログ ビューアーがあります。

クライアント ログは、macOS、Windows では、次の場所には。

- Mac の場合: `~/Library/Logs/Xamarin/Workbooks/Xamarin Workbooks {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Workbooks\logs\Xamarin Workbooks {date}.log`

統合を使用して、通常の読み込み時に注意すべき 1 つは`#r`メカニズムの開発中に、統合のアセンブリを集荷すると、_依存関係_ブックの絶対パスがある場合、それにパッケージ化使用されません。 何の役に統合を再構築する場合、伝達されません、表示する変更がある可能性があります。

## <a name="debugging-client-side-integrations"></a>デバッグのクライアント側の統合

クライアント側の統合の JavaScript で記述し、web ブラウザー画面に読み込まれると (を参照してください、[アーキテクチャ](~/tools/workbooks/sdk/architecture.md)ドキュメント)、それらをデバッグする最善の方法が mac で WebKit developer tools を使用または f12 キーの選択 ダイアログ ボックスを使用して、Windows で.

ツールの両方のセットでは、JavaScript と TypeScript ソースを表示、ブレークポイントを設定、コンソールの出力を表示し、検査および DOM を変更することができます。

### <a name="mac"></a>Mac

Mac で Xamarin Workbooks で開発者ツールを有効にするには、ターミナル、次のコマンドを実行します。

```shell
defaults write com.xamarin.Workbooks WebKitDeveloperExtras -bool true
```

Xamarin Workbooks を再起動します。 そうと表示する必要があります**要素の検査**、右クリック コンテキスト メニューのおよび新しいに表示される**開発者**ウィンドウはブックの基本設定で利用できます。 このオプションを使用して、開発者ツールの起動時に開くかを選択できます。

[![開発者のウィンドウ](debugging-images/developer-pane-small.png)](debugging-images/developer-pane.png#lightbox)

この設定は再起動専用でも、新しいブックに有効にするために、ブックは、クライアントを再起動する必要があります。 コンテキスト メニューまたは環境設定を使用して開発者ツールをアクティブ化すると、使い慣れた Safari UI が表示されます。

[![Safari の開発ツール](debugging-images/mac-dev-tools.png)](debugging-images/mac-dev-tools.png#lightbox)

Safari の開発者ツールの使用方法の詳細については、次を参照してください。、 [WebKit インスペクター ドキュメント][webkit-docs]します。

### <a name="windows"></a>Windows

IE チームが「f12 キーの選択」と呼ばれるツールを提供する、Windows 上の Internet Explorer の埋め込みインスタンスがリモート デバッガーをします。 次の場所にツールをご覧ください。

```shell
C:\Windows\System32\F12\F12Chooser.exe
```

実行の f12 キー セレクターとするには、一覧で、ブック クライアントの画面が作動する埋め込みインスタンスが表示されます。 クライアントに接続されていること、および Internet Explorer からのデバッグ ツールが表示され、使い慣れた f12 キーを選択します。

[![F12 ツール](debugging-images/windows-dev-tools.png)](debugging-images/windows-dev-tools.png#lightbox)

[webkit-docs]: https://trac.webkit.org/wiki/WebInspector
