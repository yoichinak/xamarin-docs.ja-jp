---
title: Android のデバッグ ログ
description: デバッグ ログを使用して Xamarin.Android アプリケーションをデバッグする方法。
ms.prod: xamarin
ms.assetid: 01A715FE-9E9D-9B85-8A59-6568D8A09CA5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/22/2018
ms.openlocfilehash: 8cf6c11675f0f3ddca0d5aea69e5e07160ef8454
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114783"
---
# <a name="android-debug-log"></a>Android のデバッグ ログ

開発者がアプリケーションのデバッグに使うとても一般的なトリックの 1 つは `Console.WriteLine` を呼び出すことです。 ただし、Android などのモバイル プラットフォームにコンソールはありません。 Android デバイスには、アプリの作成中に使用できるログがあります。 これは、ログの取得時に入力するコマンドから _logcat_ と呼ばれることがあります。 ログに記録されたデータを表示するには、**デバッグ ログ** ツールを使います。

## <a name="android-debug-log-overview"></a>Android のデバッグ ログの概要

**デバッグ ログ** ツールを使うと、Visual Studio でアプリをデバッグしながらログ出力を見ることができます。 デバッグ ログは次のデバイスをサポートします。

-   物理的な Android フォン、タブレット、ウェアラブル。
-   Android Emulator 上で動作する Android 仮想デバイス。 

> [!NOTE]
> **デバッグ ログ** ツールは、Xamarin Live Player では動きません。

**デバッグ ログ**では、デバイスのアプリがスタンドアロンで (つまり、Visual Studio から切断されて) 実行している間に生成されるログ メッセージは表示されません。


## <a name="accessing-the-debug-log-from-visual-studio"></a>Visual Studio からデバッグ ログにアクセスする

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

**デバイス ログ** ツールを開くには、ツール バーの **[デバイス ログ (logcat)]** アイコンをクリックします。

[![ツール バーでのデバイス ログ ツールの場所](android-debug-log-images/vswin-01-logcat-sml.png)](android-debug-log-images/vswin-01-logcat.png#lightbox)

または、次のいずれかのメニューを選んで**デバイス ログ** ツールを起動します。

-   **[ビュー]、[その他のウィンドウ]、[デバイス ログ]**
-   **[ツール] -> [Android] -> [デバイス ログ]**

次のスクリーンショットは、**デバッグ ツール** ウィンドウのさまざまな部分を示したものです。

[![デバッグ ツール ウィンドウの部分](android-debug-log-images/vswin-03-features-sml.png)](android-debug-log-images/vswin-03-features.png#lightbox)

-   **デバイス セレクター** &ndash; 監視対象の物理デバイスまたは実行中のエミュレーターを選びます。

-   **ログ エントリ** &ndash; logcat からのログ メッセージのテーブルです。

-   **ログ エントリの消去** &ndash; テーブルから現在のログ エントリをすべて消去します。

-   **再生/一時停止** &ndash; 新しいログ エントリの表示の更新と一時停止を切り替えます。

-   **停止** &ndash; 新しいログ エントリの表示を停止します。

-   **検索ボックス** &ndash; ログ エントリのサブセットをフィルター処理するには、このボックスに検索文字列を入力します。


**デバッグ ログ** ツール ウィンドウが表示されているときは、デバイス プルダウン メニューを使って監視対象の Android デバイスを選びます。

[![デバイス セレクターの場所](android-debug-log-images/vswin-02-devices-combo-sml.png)](android-debug-log-images/vswin-02-devices-combo.png#lightbox)

デバイスを選択すると、**デバイス ログ** ツールは実行中のアプリからのログ エントリを自動的に追加します。これらのログ エントリは、ログ エントリのテーブルに表示されます。 デバイスを切り替えると、デバイスのログはいったん停止してから開始します。 デバイス セレクターにデバイスが表示されるためには、先に Android プロジェクトを読み込む必要があることに注意してください。 デバイスがデバイス セレクターに表示されない場合は、Visual Studio の **[開始]** ボタンの横にあるデバイス ドロップダウン メニューでデバイスが使用できることを確認します。


# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

**デバイス ログ**を開くには、**[表示] > [パッド] > [デバイス ログ]** の順にクリックします。

[![[デバイス ログ] メニュー項目の場所](android-debug-log-images/vsmac-01-logcat-sml.png)](android-debug-log-images/vsmac-01-logcat.png#lightbox)

次のスクリーンショットは、**デバッグ ツール** ウィンドウのさまざまな部分を示したものです。

[![デバッグ ツール ウィンドウの機能](android-debug-log-images/vsmac-03-features-sml.png)](android-debug-log-images/vsmac-03-features.png#lightbox)

-   **デバイス セレクター** &ndash; 監視対象の物理デバイスまたは実行中のエミュレーターを選びます。

-   **ログ エントリ** &ndash; logcat からのログ メッセージのテーブルです。

-   **ログ エントリの消去** &ndash; テーブルから現在のログ エントリをすべて消去します。

-   **検索ボックス** &ndash; ログ エントリのサブセットをフィルター処理するには、このボックスに検索文字列を入力します。

-   **メッセージの表示** &ndash; 情報メッセージの表示を切り替えます。

-   **警告の表示** &ndash; 警告メッセージの表示を切り替えます (警告メッセージは黄色で表示されます)。

-   **エラーの表示** &ndash; エラー メッセージの表示を切り替えます (警告メッセージは赤色で表示されます)。

-   **再接続** &ndash; デバイスに再接続して、ログ エントリの表示を更新します。

-   **マーカーの追加** &ndash; 最新のログ エントリの後にマーカー メッセージ (`--- Marker N ---` など) を挿入します。_N_ は、1 から始まって新しいマーカーが追加されると 1 ずつインクリメントされるカウンターです。

デバッグ ログ ツール ウィンドウが表示されているときは、デバイス プルダウン メニューを使って監視対象の Android デバイスを選びます。

[![デバイス セレクターの場所](android-debug-log-images/vsmac-02-devices-combo-sml.png)](android-debug-log-images/vsmac-02-devices-combo.png#lightbox)

デバイスを選択すると、**デバイス ログ** ツールは実行中のアプリからのログ エントリを自動的に追加します。これらのログ エントリは、ログ エントリのテーブルに表示されます。 デバイスを切り替えると、デバイスのログはいったん停止してから開始します。 デバイス セレクターにデバイスが表示されるためには、先に Android プロジェクトを読み込む必要があることに注意してください。 デバイスがデバイス セレクターに表示されない場合は、Visual Studio の **[開始]** ボタンの横にあるデバイス ドロップダウン メニューでデバイスが使用できることを確認します。

-----


## <a name="accessing-from-the-command-line"></a>コマンド ラインからのアクセス

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

デバッグ ログを表示するには、コマンド ラインを使う方法もあります。 コマンド プロンプト ウィンドウを開き、Android SDK の platform-tools フォルダーに移動します (SDK platform-tools フォルダーの通常の位置: **C:\\Program Files (x86)\\Android\\android-sdk\\platform-tools**)。

接続されているデバイス (物理デバイスまたはエミュレーター) が 1 つだけの場合は、次のコマンドを入力してログを表示できます。

```shell
$ adb logcat
```

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

デバッグ ログを表示するには、コマンド ラインを使う方法もあります。 ターミナル ウィンドウを開き、Android SDK の platform-tools フォルダーに移動します (SDK platform-tools フォルダーの通常の位置: **/Users/username/Library/Developer/Xamarin/android-sdk-macosx/platform-tools**)。

接続されているデバイス (物理デバイスまたはエミュレーター) が 1 つだけの場合は、次のコマンドを入力してログを表示できます。

```shell
$ ./adb logcat
```

-----


複数のデバイスが接続されている場合は、デバイスを明示的に指定する必要があります。 たとえば、**adb -d logcat** では、接続されている物理デバイスのログのみが表示されます。また、**adb -e logcat** では、実行されているエミュレーターのログのみが表示されます。

その他のコマンドについては、「**adb**」と入力してヘルプ メッセージをご覧ください。


## <a name="writing-to-the-debug-log"></a>デバッグ ログへの書き込み

[Android.Util.Log](https://developer.xamarin.com/api/type/Android.Util.Log/) クラスのメソッドを使って、**デバッグ ログ**にメッセージを書き込むことができます。
例: 

```csharp
string tag = "myapp";

Log.Info (tag, "this is an info message");
Log.Warn (tag, "this is a warning message");
Log.Error (tag, "this is an error message");
```

これにより、次のような出力が生成されます。

```shell
I/myapp   (11103): this is an info message
W/myapp   (11103): this is a warning message
E/myapp   (11103): this is an error message
```

`Console.WriteLine` を使用して**デバッグ ログ**に書き込むこともできます &ndash; このメッセージは logcat に表示されますが、出力形式が若干異なります (この手法は Android で Xamarin.Forms アプリをデバッグする場合に特に便利です)。

```csharp
System.Console.WriteLine ("DEBUG - Button Clicked!");
```

これにより、logcat に次のような出力が生成されます。

```
Info (19543) / mono-stdout: DEBUG - Button Clicked!
```

## <a name="interesting-messages"></a>興味深いメッセージ

ログを読むとき (特に、ログ スニペットを他のユーザーに提供するとき)、ログ ファイル全体を熟読するのは面倒なことががよくあります。
ログ メッセージに目を通しやすくするには、最初に次のようなログ エントリを探します。

```shell
I/ActivityManager(12944): Starting: Intent { act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] flg=0x10200000 cmp=GcTest.GcTest/gctest.Activity1 } from pid 24175
```

具体的には、アプリケーション パッケージの名前も含まれている正規表現に一致する行を探します。

```shell
^I.*ActivityManager.*Starting: Intent
```

これは、アクティビティの開始に対応する行であり、次のメッセージの*ほとんど* (ただしすべてではありません) がアプリケーションと関連しています。

すべてのメッセージには、メッセージを生成するプロセスのプロセス識別子 (pid) が含まれていることに注意してください。 上記の `ActivityManager` メッセージでは、プロセス `12944` からメッセージを生成しました。 デバッグ対象のアプリケーションのプロセスを判断するには、**mono.MonoRuntimeProvider** メッセージを探します。 

```shell
I/ActivityThread(  602): Pub TouchTest.TouchTest.__mono_init__: mono.MonoRuntimeProvider
```

このメッセージは、開始されたプロセスからのものです。 この PID を含む後続のメッセージはすべて、同じプロセスから生成されたものです。
