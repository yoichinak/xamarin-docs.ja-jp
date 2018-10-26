---
title: Xamarin.Mac 拡張機能のサポート
description: このドキュメントでは、Xamarin.Mac の Finder や共有など、今日の拡張機能のサポートについて説明します。 制限事項と既知の問題、チュートリアルおよびサンプル アプリへのリンクを検査し、拡張機能を操作するためのヒントを提供します。
ms.prod: xamarin
ms.assetid: 4148F1BE-DFA0-46B6-9FCD-425A6541F510
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 60b981a764a2656210730ae0602ff32dc580cd0a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117565"
---
# <a name="xamarinmac-extension-support"></a>Xamarin.Mac 拡張機能のサポート

Xamarin.Mac 2.10 プレビューでは、複数の macOS の拡張ポイントのサポートが追加されました。

- ファインダー
- 共有
- 今日

<a name="Limitations-and-Known-Issues" />

## <a name="limitations-and-known-issues"></a>制限事項と既知の問題

次の制限し、Xamarin.Mac で拡張機能を開発するときに発生する可能性がある問題を把握します。

* 現在 Visual studio for mac のデバッグのサポートはありません。 使用して実行する必要がありますすべてデバッグ**NSLog**と**コンソール**します。 詳細については以下のヒント」セクションを参照してください。
* 拡張機能が含まれている必要がありますが、ホスト アプリケーションで時は、システムでのレジスタに 1 回を実行します。 有効にする必要がありますし、**拡張子**のセクション**システム環境設定**します。 
* いくつかの拡張機能のクラッシュは、ホスト アプリケーションが不安定になるし、予想外の動作が発生する可能性があります。 具体的には、 **Finder**と**今日**のセクション、**通知センター** 「詰まった」になるし、応答しなくなる可能性があります。 これにより、拡張機能プロジェクトで Xcode でも、経験豊富なされており、現在 Xamarin.Mac に関連付けられていないが表示されます。 これを確認して、システム ログに多くの場合、(を使用して**コンソール**、詳細については、ヒントを参照してください) に繰り返されるエラー メッセージを出力します。 これを解決する macOS の再起動が表示されます。

<a name="Tips" />

## <a name="tips"></a>ヒント

Xamarin.Mac で拡張機能を使用する場合、次のヒントが役に立ちます。

- 現在、Xamarin.Mac では、デバッグ拡張機能はサポートされていない、デバッグ エクスペリエンスは主に異なります実行と`printf`などのステートメント。 ただし、拡張機能で実行、サンド ボックス プロセスしたがって`Console.WriteLine`他の Xamarin.Mac アプリケーションは機能しません。 呼び出す[`NSLog`直接](https://gist.github.com/chamons/e2e409013a449cfbe1f2fbe5547f6554)システム ログにデバッグ メッセージを出力します。
- キャッチされない例外が少量で有用な情報のみを提供する、拡張機能プロセスをクラッシュ、**システム ログ**します。 面倒なコードをラップ、 `try/catch` (例外) ブロックを`NSLog`の前に、再スローは役立つ可能性があります。
- **システム ログ**からアクセスできる、**コンソール**の下でアプリ**アプリケーション** > **ユーティリティ**:

    [![](extensions-images/extension02.png "システム ログ")](extensions-images/extension02.png#lightbox)
- 前述のように、拡張機能のホスト アプリケーションを実行しているをシステムに登録します。 アプリケーション バンドルを削除する登録を解除します。 
- 「無効な」のバージョンのアプリの拡張機能が登録されている場合を検索します (そのため、削除することができます)、次のコマンドを使用します。 `plugin kit -mv`


<a name="Walkthrough-and-Sample-App" />

## <a name="walkthrough-and-sample-app"></a>チュートリアルとサンプル アプリ

開発者は作成し、Xamarin.iOS の拡張機能と同じ方法で Xamarin.Mac 拡張機能の使用ためを参照してください、[拡張機能の概要](~/ios/platform/extensions.md)詳細についてはドキュメントです。

小さなを格納している例 Xamarin.Mac プロジェクト、各拡張機能の種類の実際のサンプルが見つかります[ここ](https://developer.xamarin.com/samples/mac/ExtensionSamples/)します。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、簡単に見て Xamarin.Max バージョン 2.10 (以降) のアプリでの拡張機能を使用しました。

## <a name="related-links"></a>関連リンク

- [Hello Mac](~/mac/get-started/hello-mac.md)
- [ExtensionSamples](https://developer.xamarin.com/samples/mac/ExtensionSamples/)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
