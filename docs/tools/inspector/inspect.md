---
title: "ライブ アプリケーションの検査"
description: "視覚化し、ライブ アプリのデバッグ"
ms.topic: article
ms.prod: xamarin
ms.assetid: 91B3206E-B2A5-4660-A6E5-B924B8FE69A7
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/29/2017
ms.openlocfilehash: bd6d47f98435cc68ecf4156423526c31dbac09da
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="inspecting-live-applications"></a>ライブ アプリケーションの検査

ライブ アプリ検査は、企業ユーザーが利用できます。


1. [インスペクター (&)、Xamarin のブックをインストールします。](~/tools/inspector/install.md)

1. いずれかを開いて[アプリ プロジェクトはサポートされて](~/tools/inspector/install.md#supported-platforms)Mac または Visual Studio の Visual Studio でします。
1. デバッグ モードでアプリケーションを実行します。
1. をクリックして、**検査**IDE ツールバーのボタン (Visual Studio で、**検査現在アプリ..。**から使用可能なメニュー項目も、**ツール**または**デバッグ**メニュー)。



[ ![](inspect-images/mac-heres-the-button.png "IDE、ツールバーの [検査] ボタンをクリックします。")](inspect-images/mac-heres-the-button.png)

新しい Xamarin インスペクター クライアント ウィンドウが開き、新しい REPL プロンプトされます。

[ ![](inspect-images/inspector-0.7.0-map-inspect-small.png "新しい Xamarin インスペクター クライアント ウィンドウが開き、新しい REPL プロンプト")](inspect-images/inspector-0.7.0-map-inspect.png)

このウィンドウが表示されたら、対話的な c# プロンプトを実行し、C# の場合は、ステートメントよぶ式の評価に使用できる必要があります。 どうしてこんなことが一意では、コードがターゲット プロセスのコンテキストで評価されることです。 この場合、表示される iOS アプリケーションに対して実行されているコードを示します。

アプリケーションの状態に対して行った変更がターゲット プロセスで実際に起こっているか、使用できるように C# の場合、アプリケーションを変更する live、またはライブ アプリケーションの状態を検査することができます。

たとえば、iOS で (多数のアプリケーションの状態を格納お場所) で主要なドライバーは、UIApplication デリゲート クラスを検索するします可能性があります。

    var del = (MyApp.AppDelegate) UIApplication.SharedApplication.Delegate
    del.Database.GetAllCustomers ()
    ...
    del.Database.AddCustomer (...)

(複数行のエディターで、各送信が発生することに注意してください。 `Shift + Enter` 新しい行を作成および`Cmd + Enter`(`Ctrl + Enter` on Windows) 評価のためのコードが送信されます。 `Enter` 自動的に送信セーフであるとします。)

アプリケーションのビジュアル要素を取得する方が便利な方法は、「検査」ボタンを使用してです。 これをクリックすると、アプリケーションをクリックすると、UI 要素を選択できます。 変数`selectedView`が画面上の実際の要素をポイントに割り当てられます。 上記のスクリーン ショットでは、アクセス方法と、編集を確認できます`selectedView.BarTintColor`上、`UISearchBar`選択しました。

ライブ ビジュアル ツリーも非常に役立ちます。 ビュー階層の現在のスナップショットを表します。 設定する行を選択できます`selectedView`REPL では、ビューのプロパティの値が表示されます。 Mac では、複数層のビューの 3D 分割視覚エフェクトと対話できます。 Windows では、視覚的にビューのプロパティの値を編集することができます。

## <a name="known-limitations"></a>既知の制限事項

 - ビューの選択は、メイン ディスプレイにのみサポートされます。
 - プロパティ グリッドの編集を使用できない Mac での Windows では、いくつかのデータ型に制限されます。 強力な編集のために REPL を使用します。
 - インスペクター アドインと拡張機能がインストールされ、IDE で有効になっている、限りは、デバッグ モードで起動するたびに、アプリにコードを挿入おはします。 アプリでの すべての奇妙な動作に注意してください場合はお試しに無効にすると、インスペクター アドインと拡張機能をアンインストールまたは、IDE の再起動、および再確認にしてください。 してください[バグ報告](~/tools/inspector/install.md#reporting-bugs)知らせ!。
 - UI 要素を調べることでを変更するか、発生すると、次のようにしてください。[知らせ](~/tools/inspector/install.md#reporting-bugs)、これは、バグを示している可能性があります。

