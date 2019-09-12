---
title: GDB
ms.prod: xamarin
ms.assetid: CD0BE462-FA38-4881-B481-82AD05B3B8FE
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/05/2018
ms.openlocfilehash: 0599b2374addf461e59948a1926de06e6e1e746a
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70754059"
---
# <a name="gdb"></a>GDB

## <a name="overview"></a>概要

Xamarin.Android 4.10 では、`_Gdb` MSBuild ターゲットを使用することによる `gdb` の使用の部分的なサポートが導入されました。 

> [!NOTE]
> `gdb` のサポートには、Android NDK のインストールが必要になります。

`gdb` を使用する場合、次の 3 つの方法があります。

1. [高速展開を有効にしたデバッグ ビルド](#Debug_Builds_with_Fast_Deployment)。
1. [高速展開を無効にしたデバッグ ビルド](#Debug_Builds_without_Fast_Deployment)。
1. [リリース ビルド](#Release_Builds)。

問題が発生した場合は、「[トラブルシューティング](#Troubleshooting)」セクションを参照してください。

<a name="Debug_Builds_with_Fast_Deployment" />

### <a name="debug-builds-with-fast-deployment"></a>高速展開を使用するデバッグ ビルド

高速展開を有効にし、デバッグ ビルドをビルドして展開する場合、`_Gdb` MSBuild ターゲットを使用して、`gdb` をアタッチすることができます。

まず、アプリをインストールします。 これは、IDE、またはコマンド ラインを使用して行うことができます。

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:Install *.csproj
```

次に、`_Gdb` ターゲットを実行します。 実行の最後に、次のように `gdb` コマンド ラインが出力されます。

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:_Gdb *.csproj
...
    Target _Gdb:
        "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
...
```

`_Gdb` ターゲットは、`AndroidManifest.xml` ファイル内で宣言された任意の起動ツール アクティビティを起動します。 実行するアクティビティを明示的に指定するには、`RunActivity` MSBuild プロパティを使用します。 サービスとその他の Android コンストラクトの開始は、現時点ではサポートされていません。

`_Gdb` ターゲットは `gdb-symbols` ディレクトリを作成し、ターゲットの `/system/lib` および `$APPDIR/lib` ディレクトリの内容をそこにコピーします。

> [!NOTE]
> `gdb-symbols` ディレクトリの内容は、展開された Android ターゲットに関連付けられており、ターゲットを変更した場合、自動的に置き換えられません  (これをバグと見なします)。Android ターゲット デバイスを変更する場合は、このディレクトリを手動で削除する必要があります。

最後に、生成された `gdb` コマンドをコピーし、シェルで実行します。

```bash
$ "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
...
(gdb) bt
#0  0x40082e84 in nanosleep () from /Users/jon/Development/Projects/Scratch.HelloXamarin20/gdb-symbols/libc.so
#1  0x4008ffe6 in sleep () from /Users/jon/Development/Projects/Scratch.HelloXamarin20/gdb-symbols/libc.so
#2  0x74e46240 in ?? ()
#3  0x74e46240 in ?? ()
(gdb) c
```

<a name="Debug_Builds_without_Fast_Deployment" />

## <a name="debug-builds-without-fast-deployment"></a>高速展開を使用しないデバッグ ビルド

高速展開を*使用する*デバッグ ビルドは、Android NDK の `gdbserver` プログラムを高速展開の `.__override__` ディレクトリにコピーすることで機能します。 高速展開を無効にした場合、このディレクトリが存在しない可能性があります。

次の 2 つの回避策があります。

- `.__override__` ディレクトリが作成されるように、`debug.mono.log` システム プロパティを設定します。
- `.apk` 内に `gdbserver` を含めます。

### <a name="setting-the-debugmonolog-system-property"></a>`debug.mono.log` システム プロパティの設定

`debug.mono.log` システム プロパティを設定するには、`adb` コマンドを使用します。

```bash
$ adb shell setprop debug.mono.log gc
```

システム プロパティが設定されたら、高速展開を使用するデバッグ ビルド構成の場合と同じように、`_Gdb` ターゲットと出力された `gdb` コマンドを実行します。

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:_Gdb *.csproj
  ...
    Target _Gdb:
        "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
  ...
$ "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
...
(gdb) c
```

### <a name="including-gdbserver-in-your-app"></a>アプリに `gdbserver` を含める

アプリ内に `gdbserver` を含めるには、次のようにします。

1. Android NDK 内で `gdbserver` (**$ANDROID\_NDK\_PATH/prebuilt/android-arm/gdbserver/gdbserver** にある) を見つけ、それを Project ディレクトリにコピーします。

2. `gdbserver` の名前を **libs/armeabi-v7a/libgdbserver.so** に変更します。

3. `AndroidNativeLibrary` の**ビルド アクション**を使用して、Project に **libs/armeabi-v7a/libgdbserver.so** を追加します。

4. アプリケーションをリビルドし、再インストールします。

アプリが再インストールされたら、高速展開を使用するデバッグ ビルド構成の場合と同じように、`_Gdb` ターゲットと出力された `gdb` コマンドを実行します。

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:_Gdb *.csproj
  ...
    Target _Gdb:
        "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
  ...
$ "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
...
(gdb) c
```

<a name="Release_Builds" />

## <a name="release-builds"></a>リリース ビルド

`gdb` のサポートには次の 3 つが必要になります。

1. `INTERNET` アクセス許可。
2. 有効なアプリ デバッグ。
3. アクセス可能な `gdbserver`。

アプリ デバッグでは `INTERNET` アクセス許可は既定で有効になります。 アプリケーションにまだ存在しない場合は、**Properties/AndroidManifest.xml** を編集するか、[[プロジェクトのプロパティ]](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest) を編集して追加することができます。

アプリ デバッグは、[ApplicationAttribute.Debugging](xref:Android.App.ApplicationAttribute.Debuggable) カスタム属性プロパティを `true` に設定するか、次のように **Properties/AndroidManifest.xml** を編集して `//application/@android:debuggable` 属性を `true` に設定することで有効にすることできます。

```xml
<application android:label="Example.Name.Here" android:debuggable="true">
```

アクセス可能な `gdbserver` は、「[高速展開を使用しないデバッグ ビルド](#Debug_Builds_without_Fast_Deployment)」セクションに従って指定できます。

問題: `_Gdb` 既に実行されているアプリ インスタンスが MSBuild ターゲットによってすべて強制終了されます。 Android v4.0 より前のバージョンのターゲットではこのようなことはありません。

<a name="Troubleshooting" />

## <a name="troubleshooting"></a>トラブルシューティング

### <a name="mono_pmip-doesnt-work"></a>`mono_pmip` が機能しない

`mono_pmip` 関数 ([マネージド スタック フレームを取得する](https://www.mono-project.com/docs/debug+profile/debug/#debugging-with-gdb)場合に役立つ) が (現在、`_Gdb` ターゲットがプルダウンしていない) `libmonosgen-2.0.so` からエクスポートされます  (この問題は今後のリリースで修正される予定です)。

`libmonosgen-2.0.so` にある関数の呼び出しを有効にするには、次のようにターゲット デバイスから `gdb-symbols` ディレクトリにコピーします。

```bash
$ adb pull /data/data/Mono.Android.DebugRuntime/lib/libmonosgen-2.0.so Project/gdb-symbols
```

次に、デバッグ セッションを再開します。

### <a name="bus-error-10-when-running-the-gdb-command"></a>バス エラー: `gdb` コマンド実行時のバス エラー: 10

`gdb` コマンドが `"Bus error: 10"` のエラーで終了した場合は、Android デバイスを再起動します。

```bash
$ "/path/to/arm-linux-androideabi-gdb" -x "Project/gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
Copyright (C) 2011 Free Software Foundation, Inc.
...
Bus error: 10
$
```

### <a name="no-stack-trace-after-attach"></a>アタッチ後のスタック トレースがない

```bash
$ "/path/to/arm-linux-androideabi-gdb" -x "Project/gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
Copyright (C) 2011 Free Software Foundation, Inc.
...
(gdb) bt
No stack.
```

これは通常、`gdb-symbols` ディレクトリの内容が Android ターゲットと同期されていないことを示しています  (Android ターゲットを変更しましたか?)。

`gdb-symbols` ディレクトリを削除して、再試行してください。
