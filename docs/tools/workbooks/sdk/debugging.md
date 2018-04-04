---
title: デバッグの統合
ms.prod: xamarin
ms.assetid: 90143544-084D-49BF-B44D-7AF943668F6C
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: eaf391c399aa80de0174189ec68a2cca70125895
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="debugging-integrations"></a>デバッグの統合

## <a name="debugging-agent-side-integrations"></a>エージェント側の統合のデバッグ

によって提供されるログ メソッドを使用して、最適な方法はエージェント側の統合のデバッグ、`Log`クラス`Xamarin.Interactive.Logging`です。 参照してください、 [ `API docs` ](https://developer.xamarin.com/api/type/Xamarin.Interactive.Logging.Log/)のメソッドを呼び出します。

Macos、ログ メッセージは、両方のログ ビューアー メニューに表示されます。 (**ウィンドウ > ログ ビューアー**) およびクライアントのログです。 Windows では、メッセージのみに表示されます、クライアント ログがあるログの表示はありません。

クライアント ログは、Windows および macOS 次の場所には。

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

注意すべき 1 つの点となるは、通常を使用して統合を読み込み`#r`開発時にメカニズム integration アセンブリが取得されますとして、_依存関係_ブックの場合は、絶対パスが一緒にパッケージと使用しません。 これには、変更を何も行われませんでした、統合を再構築する場合と同様に伝達されません反映する可能性があります。

## <a name="debugging-client-side-integrations"></a>クライアント側の統合のデバッグ

クライアント側の統合が JavaScript で記述されると、web ブラウザーのサーフェイスに読み込まれます (を参照してください、[アーキテクチャ](~/tools/workbooks/sdk/architecture.md)ドキュメント)、それらをデバッグする最善の方法が mac で WebKit developer tools を使用または f12 キー セレクターを使用して、Windows 上.

両方のツールのセットを使用すると、JavaScript または TypeScript ソースを表示、ブレークポイントを設定、コンソール出力を表示し、検査および DOM の変更

### <a name="mac"></a>Mac

Mac で Xamarin ブックで、開発者ツールを有効にするのには、端末の次のコマンドを実行します。

```shell
defaults write com.xamarin.Inspector WebKitDeveloperExtras -bool true
```

Xamarin ブックを再起動します。 ようにを設定すると表示されます**検査要素**、右クリック コンテキスト メニューおよび新しいに表示される**開発者**ペインはブックの設定 で使用可能になります。 このオプションでは、開発者ツールの起動時に開かれたかどうかに選択することができます。

[![Developer ウィンドウ](debugging-images/developer-pane-small.png)](debugging-images/developer-pane.png#lightbox)

この優先順位も再起動専用に、新しいブックに有効にするためにブックは、クライアントを再起動する必要があります。 コンテキスト メニューまたは環境設定を使用して開発者ツールをアクティブ化すると、使い慣れた Safari UI が表示されます。

[![Safari の開発ツール](debugging-images/mac-dev-tools.png)](debugging-images/mac-dev-tools.png#lightbox)

Safari 開発者ツールを使用する方法の詳細については、次を参照してください。、 [WebKit インスペクター ドキュメント][webkit-docs]です。

### <a name="windows"></a>Windows

IE チームが「f12 キーの選択」と呼ばれるツールを提供する Windows では、埋め込みの Internet Explorer のインスタンスのリモート デバッガーはします。 次の場所に、ツールがあります。

```shell
C:\Windows\System32\F12\F12Chooser.exe
```

実行の F12 選択して、一覧で、ブック クライアント画面を利用する埋め込みインスタンスが表示されます。 クライアントに接続し、Internet Explorer からデバッグ ツールが表示され、使い慣れた f12 キーを選択します。

[![F12 ツール](debugging-images/windows-dev-tools.png)](debugging-images/windows-dev-tools.png#lightbox)

[webkit-docs]: https://trac.webkit.org/wiki/WebInspector