---
title: "Android のデバッグ ログ"
ms.topic: article
ms.prod: xamarin
ms.assetid: 01A715FE-9E9D-9B85-8A59-6568D8A09CA5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 99b9ed9e3c71766f483f7b00996137aae7a247d1
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="android-debug-log"></a>Android のデバッグ ログ

## <a name="android-debug-log-overview"></a>Android のデバッグ ログの概要

開発者がアプリケーションのデバッグに使うとても一般的なトリックの 1 つは `Console.WriteLine` を呼び出すことです。 ただし、Android などのモバイル プラットフォームにコンソールはありません。 Android デバイスには、アプリの作成中に使用できるログがあります。 これは、ログの取得時に入力するコマンドから "logcat" と呼ばれることがあります。

## <a name="accessing-from-visual-studio"></a>Visual Studio からのアクセス

Visual Studio 2015 以降では、Android および iOS のログ パッドが統合されています。

### <a name="visual-studio-2015--2017"></a>Visual Studio 2015 および 2017

Visual Studio 用の新しい [デバイス ログ] ツール ウィンドウには、Android および iOS デバイスのログが表示されます。 次のコマンドのいずれかを実行して表示できます。 

-   **[ビュー]、[その他のウィンドウ]、[デバイス ログ]**
-   **[ツール] -> [Android] -> [デバイス ログ]**
-   **[Android ツール バー] -> [デバイス ログ]**

ツール ウィンドウが表示されたら、デバイス コンボ ボックスから物理デバイスを選択できます。 デバイスを選択すると、テーブル内の実行アプリからログ エントリの追加が自動的に開始されます。 デバイス間の切り替えによって、デバイスのログ記録の停止と開始が切り替わります。デバイスをコンボ ボックスに表示するには、Android プロジェクトを読み込む必要があります。 デバイスがコンボ ボックスに表示されない場合は、まず [デバッグの開始] ドロップ ダウンに表示できるかどうかを確認します。 

このツール ウィンドウでは、ログ エントリのテーブル、デバイス選択用のコンボ ボックス、ログ エントリをクリアする方法、検索ボックス、および再生/停止/一時停止ボタンが提供されます。 


<a name="Accessing_from_the_Command_Line" />

## <a name="accessing-from-the-command-line"></a>コマンド ラインからのアクセス

デバッグ ログを表示するには、コマンド ラインを使用する方法もあります。 コンソール ウィンドウを開き、Android SDK プラットフォームツール フォルダー (**C:\android-sdk-windows\platform-tools** など) に移動します。 

1 つのデバイスのみを接続している場合、次のコマンドでログを表示できます。

```shell
$ adb logcat
```

複数のデバイスを接続している場合、デバイスを識別する必要があります。 たとえば、`adb -d logcat` を実行すると、接続されている物理デバイスのログのみが表示されます。また、`adb -e logcat` の場合は、実行されているエミュレーターのログのみが表示されます。 

その他のコマンドは、**adb** を実行すると表示されます。

<a name="Writing_to_the_Debug_Log" />


## <a name="writing-to-the-debug-log"></a>デバッグ ログへの書き込み

[Android.Util.Log](https://developer.xamarin.com/api/type/Android.Util.Log/) クラスに対してメソッドを使用して、デバッグ ログにメッセージを書き込むことができます。 

```csharp
string tag = "myapp";

Log.Info (tag, "this is an info message");
Log.Warn (tag, "this is a warning message");
Log.Error (tag, "this is an error message");
```

次のような内容が生成されます。

```shell
I/myapp   (11103): this is an info message
W/myapp   (11103): this is a warning message
E/myapp   (11103): this is an error message
```

<a name="Interesting_Messages" />

## <a name="interesting-messages"></a>興味深いメッセージ

ログを読み取るとき、特にログ スニペットを他のユーザーに提供するとき (たとえば、完全なログ ファイルでは (1) 多すぎて、(2) 読みにくい場合)、起点となる*最も重要な*行は次のような行です。

```shell
I/ActivityManager(12944): Starting: Intent { act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] flg=0x10200000 cmp=GcTest.GcTest/gctest.Activity1 } from pid 24175
```

具体的には、次の正規表現と一致する行です。

```shell
^I.*ActivityManager.*Starting: Intent
```

また、アプリケーション パッケージ名を含みます。 これは、アクティビティの開始に対応する行であり、次のメッセージの*ほとんど* (ただしすべてではありません) がアプリケーションと関連しています。 

特に、各メッセージには、メッセージを生成するプロセスのプロセス識別子 (pid) が含まれています。 上記の `ActivityManager` メッセージでは、プロセス `12944` からメッセージを生成しました。 デバッグ対象のアプリケーションのプロセスを判断するには、mono.MonoRuntimeProvider メッセージを探します。 

```shell
I/ActivityThread(  602): Pub TouchTest.TouchTest.__mono_init__: mono.MonoRuntimeProvider
```

このメッセージは、開始されたプロセスから生成されます。同じ pid を含む以降のメッセージは、すべて同じプロセスから生成されます。 
