---
title: Xamarin.Mac 拡張機能のサポート
description: この記事では、Xamarin.Mac バージョン 2.10 (以降) での拡張機能のサポートについて説明します。
ms.prod: xamarin
ms.assetid: 4148F1BE-DFA0-46B6-9FCD-425A6541F510
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 03936c75d31bfd01e741ad2c09096c925dc9dbfc
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinmac-extension-support"></a>Xamarin.Mac 拡張機能のサポート

Xamarin.Mac 2.10 プレビューでは、複数の macOS 拡張ポイントのサポートが追加されました。

- Finder
- 共有
- 今日

<a name="Limitations-and-Known-Issues" />

## <a name="limitations-and-known-issues"></a>制限事項と既知の問題

次の制限し、Xamarin.Mac で拡張機能を開発するときに発生する可能性がある問題を確認します。

* 現在 Visual studio for mac のデバッグのサポートはありません。 使用して実行する必要がありますすべてデバッグ**NSLog**と**コンソール**です。 詳細については、以下のセクションのヒントを参照してください。
* 拡張機能が含まれている必要があるホスト アプリケーションでどの場合に、システムに登録に 1 回を実行します。 有効化する、**拡張子**のセクション**システム環境設定**です。 
* クラッシュやいくつかの拡張機能可能性があります不安定になるホスト アプリケーションの動作が発生します。 具体的には、 **Finder**および、**今日**のセクションで、**通知センター** 「詰まった」になる、応答しなくなる可能性があります。 これにより、拡張機能プロジェクトで Xcode で同様に、経験豊富なされており、現在 Xamarin.Mac に関連していないが表示されます。 これをシステム ログに表示される多くの場合、(を介して**コンソール**、詳細については、ヒントを参照してください) 繰り返し表示されるエラー メッセージを印刷します。 これを解決する macOS の再起動が表示されます。

<a name="Tips" />

## <a name="tips"></a>ヒント

Xamarin.Mac で拡張機能を使用する場合、次のヒントは役に立ちます。

- 現在、Xamarin.Mac では、拡張機能のデバッグはサポートされていない、デバッグの機能は、主に異なります実行と`printf`like ステートメントです。 ただし、拡張機能で実行サンド ボックス プロセスでは、したがって`Console.WriteLine`Xamarin.Mac 他のアプリケーションとは機能しません。 呼び出す[`NSLog`直接](https://gist.github.com/chamons/e2e409013a449cfbe1f2fbe5547f6554)システム ログにデバッグ メッセージが出力されます。
- キャッチされない例外、プロセスがクラッシュします拡張機能、少量の有用な情報のみを提供する、**システム ログ**です。 問題のあるコードでの折り返し、 `try/catch` (例外) をブロック`NSLog`の役立つことがあります再スローする前にします。
- **システム ログ**からアクセスできる、**コンソール**の下でアプリ**アプリケーション** > **ユーティリティ**:

    [![](extensions-images/extension02.png "システム ログ")](extensions-images/extension02.png#lightbox)
- 前述のように、拡張機能ホスト アプリケーションの実行がシステムに登録、します。 アプリケーション バンドルを削除する登録を解除します。 
- 「無効な」のバージョンのアプリの拡張機能が登録されている場合は、次のコマンドを使用して、(したがって、それらを削除することができます) それらを探します。 `plugin kit -mv`


<a name="Walkthrough-and-Sample-App" />

## <a name="walkthrough-and-sample-app"></a>チュートリアルおよびサンプル アプリ

開発者が作成され Xamarin.iOS 拡張機能と同じ方法で Xamarin.Mac 拡張子を持つ作業ためを参照してください、 [Introduction to Extensions](~/ios/platform/extensions.md)詳細についてはドキュメントです。

含む小さい例 Xamarin.Mac プロジェクト、各拡張機能の種類の実際のサンプルが存在する[ここ](https://developer.xamarin.com/samples/mac/ExtensionSamples/)です。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事の内容が、簡単に見て Xamarin.Max バージョン 2.10 (以降) アプリでの拡張機能を使用しています。

## <a name="related-links"></a>関連リンク

- [Hello Mac](~/mac/get-started/hello-mac.md)
- [ExtensionSamples](https://developer.xamarin.com/samples/mac/ExtensionSamples/)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
