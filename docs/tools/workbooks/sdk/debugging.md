---
title: 統合のデバッグ
description: このドキュメントでは、Windows と Mac でエージェント側とクライアント側の両方の Xamarin Workbooks 統合をデバッグする方法について説明します。
ms.prod: xamarin
ms.assetid: 90143544-084D-49BF-B44D-7AF943668F6C
author: lobrien
ms.author: laobri
ms.date: 06/19/2018
ms.openlocfilehash: 6b89c0855b35a10a2afcbb69c4a011079c2aaf1d
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68511862"
---
# <a name="debugging-integrations"></a>統合のデバッグ

## <a name="debugging-agent-side-integrations"></a>エージェント側の統合のデバッグ

エージェント側の統合のデバッグは、の`Log` `Xamarin.Interactive.Logging`クラスによって提供されるログ記録メソッドを使用して行うことをお勧めします。

MacOS では、ログメッセージは、[ログビューアー] メニュー (**ウィンドウ > ログビューアー**) とクライアントログの両方に表示されます。 Windows では、ログビューアーが存在しないため、メッセージはクライアントログにのみ表示されます。

クライアントログは、macOS および Windows 上の次の場所にあります。

- Mac`~/Library/Logs/Xamarin/Workbooks/Xamarin Workbooks {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Workbooks\logs\Xamarin Workbooks {date}.log`

注意すべき1つの点は、開発時に通常`#r`のメカニズムを使用して統合を読み込む場合、統合アセンブリがブックの_依存関係_として取得され、絶対パスが使用されていない場合にパッケージによってパッケージ化されることです。 これにより、統合を再構築しても何も行わなかったように、変更が反映されないように見える場合があります。

## <a name="debugging-client-side-integrations"></a>クライアント側の統合のデバッグ

クライアント側の統合は JavaScript で記述され、web ブラウザーサーフェイスに読み込まれます ([アーキテクチャ](~/tools/workbooks/sdk/architecture.md)のドキュメントを参照してください)。デバッグする最良の方法は、Mac 上の WebKit 開発者ツールを使用するか、Windows で F12 選択を使用することです。

どちらのツールセットでも、JavaScript/TypeScript ソースの表示、ブレークポイントの設定、コンソール出力の表示、および DOM の検査と変更を行うことができます。

### <a name="mac"></a>Mac

Mac で Xamarin Workbooks 用の開発者ツールを有効にするには、ターミナルで次のコマンドを実行します。

```shell
defaults write com.xamarin.Workbooks WebKitDeveloperExtras -bool true
```

次に、Xamarin Workbooks を再起動します。 この操作を行うと、右クリックのコンテキストメニューに **[要素の検査]** が表示され、ブックの設定 で新しい **[開発者]** ペインが使用できるようになります。 このオプションを使用すると、スタートアップ時に開発者ツールを開くかどうかを選択できます。

[![開発者ウィンドウ](debugging-images/developer-pane-small.png)](debugging-images/developer-pane.png#lightbox)

この設定は再起動のみを目的としているため、新しいブックで有効にするには、ブッククライアントを再起動する必要があります。 コンテキストメニューまたは基本設定を使用して開発者ツールをアクティブ化すると、使い慣れた Safari UI が表示されます。

[![Safari 開発ツール](debugging-images/mac-dev-tools.png)](debugging-images/mac-dev-tools.png#lightbox)

Safari 開発者ツールの使用方法の詳細については、 [WebKit inspector のドキュメント][webkit-docs]を参照してください。

### <a name="windows"></a>Windows

Windows では、IE チームは "F12 Chooser" と呼ばれるツールを提供します。このツールは、Internet Explorer の組み込みインスタンスのリモートデバッガーです。 ツールは次の場所で見つけることができます。

```shell
C:\Windows\System32\F12\F12Chooser.exe
```

F12 の選択を実行すると、リスト内のブックのクライアント画面を支える埋め込みインスタンスが表示されます。 これを選択すると、Internet Explorer の使い慣れた F12 デバッグツールが表示され、クライアントに接続されます。

[![F12 ツール](debugging-images/windows-dev-tools.png)](debugging-images/windows-dev-tools.png#lightbox)

[webkit-docs]: https://trac.webkit.org/wiki/WebInspector
