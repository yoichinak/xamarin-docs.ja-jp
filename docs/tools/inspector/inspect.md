---
title: Live アプリケーションの検査
description: このドキュメントでは、Xamarin Inspector を使用してアプリケーションを検査する方法について説明します。 Xamarin Inspector ツールの制限事項についても説明します。
ms.prod: xamarin
ms.assetid: 91B3206E-B2A5-4660-A6E5-B924B8FE69A7
author: lobrien
ms.author: laobri
ms.date: 06/19/2018
ms.openlocfilehash: 2def0a01bdd28af5eefb76afc19a0e49fd1df355
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67831549"
---
# <a name="inspecting-live-applications"></a>Live アプリケーションの検査

ライブ アプリの検査は企業のお客様に使用できます。

1. いずれかを開く[アプリ プロジェクトがサポートされている](~/tools/inspector/install.md#supported-platforms)Visual Studio for Mac または Visual Studio でします。
1. デバッグ モードでアプリケーションを実行します。
1. をクリックして、**検査**IDE ツールバーのボタン (Visual Studio で、**検査現在アプリ...** から使用可能なメニュー項目も、**ツール**または**デバッグ**メニュー)。

[![](inspect-images/mac-heres-the-button.png "IDE ツールバーで [検査] ボタンをクリックします。")](inspect-images/mac-heres-the-button.png#lightbox)

新しい Xamarin Inspector クライアント ウィンドウが開き、新しい REPL プロンプトされます。

[![](inspect-images/inspector-0.7.0-map-inspect-small.png "新しい REPL プロンプトで、新しい Xamarin Inspector クライアント ウィンドウが開きます")](inspect-images/inspector-0.7.0-map-inspect.png#lightbox)

対話型があるこのウィンドウが表示されたら、C#プロンプトを実行し、評価に使用できるC#ステートメントと式。 この一意では、コードがターゲット プロセスのコンテキストで評価されることです。 この場合、コードに対して表示される iOS アプリケーションを実行して表示されます。

アプリケーションの状態に対して行った変更が実際に発生しているターゲット プロセスで使用できるようにC#変更するのには、アプリケーションが live、またはライブ アプリケーションの状態を検査することができます。

たとえば、ios では、(多くのアプリケーションの状態を保存しました、)、メイン ドライバーは、UIApplication デリゲート クラスを見つけたい可能性があります。

    var del = (MyApp.AppDelegate) UIApplication.SharedApplication.Delegate
    del.Database.GetAllCustomers ()
    ...
    del.Database.AddCustomer (...)

(複数行のエディターで、各送信が発生することに注意してください。 `Shift + Enter` 新しい行を作成し、 `Cmd + Enter` (`Ctrl + Enter` Windows 上) 評価のためのコードが送信されます。 `Enter` 自動的に送信する安全なタイミングです。)

アプリケーションのビジュアル要素を取得するより便利な方法は、「検査」ボタンを使用してです。 これをクリックすると、アプリケーションをクリックして、UI 要素を選択できます。 変数`selectedView`が画面上の実際の要素をポイントに割り当てられます。 上記のスクリーン ショットでは、アクセスおよび編集する方法を確認できます`selectedView.BarTintColor`上、`UISearchBar`を選択しました。

ライブ ビジュアル ツリーも非常に役立ちます。 ビュー階層の現在のスナップショットを表します。 設定する行を選択できる`selectedView`REPL では、ビューのプロパティの値を確認します。 Mac では、階層型ビューの 3D 分解された視覚エフェクトを操作できます。 Windows では、ビューのプロパティの値を視覚的に編集できます。

## <a name="known-limitations"></a>既知の制限事項

- ビューの選択は、メイン ディスプレイにのみサポートされます。
- プロパティ グリッドの編集 for Mac ではないし、Windows では、いくつかのデータ型に制限されます。 強力な編集のためには、REPL を使用します。
- Inspector addin/拡張機能がインストールされ、お使いの IDE で有効になって、限りをアプリにコードを挿入して、デバッグ モードで起動するたびには。 アプリで動作する場合は、お試しに無効にすると、インスペクター addin/拡張機能をアンインストールまたは IDE を再起動、および再確認してください。 してください[バグ報告](~/tools/inspector/install.md#reporting-bugs)知らせです。
- UI 要素を検査すると、そのままで変更、ください[お知らせ](~/tools/inspector/install.md#reporting-bugs)バグを示す可能性があります。

