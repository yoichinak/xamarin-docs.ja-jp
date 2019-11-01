---
title: Live アプリケーションの検査
description: このドキュメントでは、Xamarin Inspector を使用してアプリケーションを検査する方法について説明します。 また、Xamarin Inspector ツールの制限事項についても説明します。
ms.prod: xamarin
ms.assetid: 91B3206E-B2A5-4660-A6E5-B924B8FE69A7
author: davidortinau
ms.author: daortin
ms.date: 06/19/2018
ms.openlocfilehash: bbb1e21139b5f073e2cc7e3d4781e8bc38334449
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73006304"
---
# <a name="inspecting-live-applications"></a>Live アプリケーションの検査

企業のお客様は、ライブアプリの検査を利用できます。

1. Visual Studio for Mac または Visual Studio で[サポートされているアプリプロジェクト](~/tools/inspector/install.md#supported-platforms)を開きます。
1. デバッグ モードでアプリケーションを実行します。
1. IDE ツールバーの **[検査]** ボタンをクリックします (Visual Studio では、 **[ツール]** メニューまたは **[デバッグ]** メニューから 現在の **[アプリの検査...]** を選択することもできます)。

[![](inspect-images/mac-heres-the-button.png "Click the Inspect button in the IDE toolbar")](inspect-images/mac-heres-the-button.png#lightbox)

新しい REPL プロンプトが表示され、新しい Xamarin Inspector クライアントウィンドウが開きます。

[![](inspect-images/inspector-0.7.0-map-inspect-small.png "A new Xamarin Inspector client window will open, with a fresh REPL prompt")](inspect-images/inspector-0.7.0-map-inspect.png#lightbox)

このウィンドウが表示されたら、対話C#形式のプロンプトを使用して、ステートメントC#や式の実行と評価を行うことができます。 これは、ターゲットプロセスのコンテキストでコードが評価されることを意味します。 この例では、表示されている iOS アプリケーションに対して実行されるコードを示しています。

アプリケーションの状態に対して行われた変更は、実際にはターゲットプロセスで発生するため、をC#使用してアプリケーションをライブで変更することも、アプリケーションの状態をライブで検査することもできます。

たとえば、iOS では、主なドライバーである UIApplication delegate クラスを見つけることができます。このクラスは、アプリケーションの状態を多数格納します。

```csharp
var del = (MyApp.AppDelegate) UIApplication.SharedApplication.Delegate
del.Database.GetAllCustomers ()
...
del.Database.AddCustomer (...)
```

(各送信は複数行エディターで行われることに注意してください。 `Shift + Enter` によって新しい行が作成され、`Cmd + Enter` (Windows では`Ctrl + Enter`) によって評価用のコードが送信されます。 `Enter` は、安全なときに自動的に送信されます)。

[検査] ボタンを使用すると、アプリケーションのビジュアル要素に簡単にアクセスできます。 このボタンをクリックすると、アプリケーションをクリックして UI 要素を選択できます。 変数 `selectedView` は、画面上の実際の要素を指すように割り当てられます。 上のスクリーンショットでは、選択した `UISearchBar` の `selectedView.BarTintColor` にアクセスして編集する方法を確認できます。

ライブビジュアルツリーも非常に便利です。 これは、ビュー階層の現在のスナップショットを表します。 REPL で `selectedView` を設定する行を選択して、ビューのプロパティ値を表示できます。 Mac では、階層ビューの3D 分解視覚化を操作できます。 Windows では、ビューのプロパティ値を視覚的に編集できます。

## <a name="known-limitations"></a>既知の制限事項

- ビューの選択は、メインディスプレイでのみサポートされています。
- プロパティグリッドの編集は Mac では使用できません。 Windows では、いくつかのデータ型に制限されています。 REPL を使用して、より強力な編集を行います。
- IDE でインスペクターのアドイン/拡張機能がインストールされ、有効になっている限り、デバッグモードで起動するたびに、アプリにコードが挿入されます。 アプリで奇妙な動作が発生した場合は、インスペクターのアドイン/拡張機能の無効化またはアンインストール、IDE の再起動、およびノートを試してください。 ご意見[をお聞かせください。](~/tools/inspector/install.md#reporting-bugs)
- UI 要素を検査しても変更されない場合は、[お知らせください。](~/tools/inspector/install.md#reporting-bugs)これはバグを示している可能性があるためです。
