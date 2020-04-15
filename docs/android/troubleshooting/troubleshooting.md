---
title: トラブルシューティングのヒント
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 56137ACA-4811-B312-6860-E16D0FA123F7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/15/2018
ms.openlocfilehash: 6d83afa47c459633506736b2497a82c444352c90
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "79303517"
---
# <a name="troubleshooting-tips"></a>トラブルシューティングのヒント

## <a name="getting-diagnostic-information"></a>診断情報の取得

Xamarin.Android では、さまざまなバグを調査する際にいくつかの場所を確認できます。
次の設定があります。

1. 診断MSBuildの出力。
2. デバイスのデプロイログ。
3. Androidのデバッグログの出力。

<a name="Diagnostic_MSBuild_Output" />

## <a name="diagnostic-msbuild-output"></a>診断MSBuildの出力

診断MSBuildには、パッケージのビルドに関する追加情報を含めることができます。また、パッケージのデプロイ情報が含まれている場合もあります。

Visual Studio 内で診断 MSBuild 出力を有効にするには:

1. **[ツール]、[オプション]** の順にクリックします
2. 左側のツリー ビューで、 **[プロジェクトとソリューション]、[ビルドして実行]** の順に選択します
3. 右側のパネルで、MSBuild ビルド出力の詳細ドロップダウンを[診断]に設定します
4. **[OK]** をクリックします。
5. パッケージから不要な要素を取り除き、再ビルドします。
6. 診断出力は[出力]パネルに表示されます。

Visual Studio for Mac/OS X内で診断用 MSBuild 出力を有効にするには:

1. **[Visual Studio for Mac]、[環境設定]** の順にクリックします
2. 左側のツリー ビューで、 **[プロジェクト]、[ビルド]** の順に選択します
3. 右側のパネルで、ログ詳細ドロップダウンを[診断]に設定します
4. **[OK]** をクリックします。
5. Visual Studio for Mac を再起動します。
6. パッケージから不要な要素を取り除き、再ビルドします。
7. [ビルド出力] ボタンをクリックし、エラーパッド内に診断出力を表示します ( **[表示]、[パッド]、[エラー]** )。

## <a name="device-deployment-logs"></a>デバイスのデプロイログ

Visual Studio内でデバイスのデプロイログを有効にするには:

1. **[ツール]、[オプション]** >の順にクリックします
2. 左側のツリービューで、 **[Xamarin]、[Android 設定]** を選択します。
3. 右側のパネルで、[X]**拡張デバッグログを有効にする (デスクトップのmonodroid.logに書き込まれます)** チェックボックスをオンにします。
4. ログメッセージは、デスクトップ上のファイルmonodroid.logに書き込まれます。

Visual Studio for Macは、常にデバイスのデプロイログを書き込みます。 検索は少し難しくなります。*AndroidUtils*のログファイルは、次のように毎日およびデプロイのたびに作成されます。**AndroidTools-2012-10-24_12-35-45.log**

- Windowsでは、ログファイルは`%LOCALAPPDATA%\XamarinStudio-{VERSION}\Logs`に書き込まれます。
- OS Xでは、ログファイルは`$HOME/Library/Logs/XamarinStudio-{VERSION}`に書き込まれます。

## <a name="android-debug-log-output"></a>Androidのデバッグログ出力

Android では、[Androidデバッグログ](~/android/deploy-test/debugging/android-debug-log.md)に多くのメッセージが書き込まれます。
Xamarin.Androidのシステムプロパティを使用して、Androidデバッグログへの追加メッセージの生成を制御します。 Androidのシステムプロパティは、[Android Debug Bridge (adb)](https://developer.android.com/guide/developing/tools/adb.html)内の *setprop*コマンドを使用して設定できます。

```shell
adb shell setprop PROPERTY_NAME PROPERTY_VALUE
```

システムプロパティはプロセスの起動時に読み取られるため、アプリケーションを起動する前に設定するか、システムプロパティの変更後にアプリケーションを再起動する必要があります。

### <a name="xamarinandroid-system-properties"></a>Xamarin.Android のシステム プロパティ

Xamarin.Android では、次のシステムプロパティがサポートされています。

- *debug.mono.debug*:空でない文字列の場合、これは`*mono-debug*`に相当します。

- *debug.mono.env*:アプリケーションの起動時、monoが初期化される*前に*エクスポートする環境変数のパイプ区切り(' *|* ')の一覧。 これにより、mono ログを制御する環境変数を設定できます。

  > [!NOTE]
  > 値は' *|* 'で区切られているため、値には上位の引用符を追加する必要があります。これは \`*adb シェル*\` コマンドによって引用符のセットが削除されるためです。

  > [!NOTE]
  > Android システムプロパティの値の長さは92文字以下でなければなりません。

  例:

  ```
  adb shell setprop debug.mono.env "'MONO_LOG_LEVEL=info|MONO_LOG_MASK=asm'"
  ```

- *debug.mono.log*:Android デバッグログに追加のメッセージを出力する必要があるコンポーネントのコンマ区切り (' *,* ') リスト。 既定では、何も設定されていません。 次のものがコンポーネントに含まれます。

  - *all*:すべてのメッセージを出力する
  - *gc*:GC関連のメッセージを出力します。
  - *gref*:弱グローバル、グローバル参照の割り当てと解放のメッセージを出力します。
  - *lref*:local参照の割り当てと解放のメッセージを出力します。

  > [!NOTE]
  > これらは、非常に*詳細*です。 本当に必要な場合を除き、有効にしないでください。

- *debug.mono.trace*:[mono--trace](http://docs.go-mono.com/?link=man%3amono(1))`=PROPERTY_VALUE` 設定を設定できます。

## <a name="deleting-bin-and-obj"></a>`bin`と`obj`を削除しています

Xamarin.Android は、過去に次のような状況から検出されました。

- 予期しないビルドまたはランタイムエラーが発生した。
- `Clean`、`Rebuild`、または手動で`bin`と`obj`のディレクトリを削除します。
- 問題は解決します。

開発者の生産性に影響を与えるため、このような問題の解決に多大な投資を行っています。

このような問題が発生した場合は、次のようになります。

1. 注意深く記憶します。 プロジェクトをこの状態にする最後のアクションは何ですか?
1. 現在のビルドログを保存します。 もう一度ビルドし、[診断ビルドログ](#diagnostic-msbuild-output)を記録します。
1. [バグレポート][bug]を送信します。

`bin`と`obj`のディレクトリを削除する前に、必要に応じて後で診断できるようzip形式で保存します。 多くの場合、Xamarin.Android アプリケーションプロジェクトを`Clean`するだけで、作業内容を再度取得できます。

[bug]: https://github.com/xamarin/xamarin-android/wiki/Submitting-Bugs,-Feature-Requests,-and-Pull-Requests

## <a name="xamarinandroid-cannot-resolve-systemvaluetuple"></a>Xamarin.Android では、System.ValueTupleは解決できません

このエラーは、Visual Studioとの互換性がないことが原因で発生します。

- **Visual Studio 2017 Update 1** (バージョン15.1以前) は、**System.ValueTuple NuGet 4.3.0**(またはそれ以前)とのみ互換性があります。

- **Visual Studio 2017 Update 2** (バージョン15.2以降) は、**System.ValueTuple NuGet 4.3.1**(またはそれ以降)とのみ互換性があります。

インストールしたVisual Studio 2017に対応する適切なSystem.ValueTuple NuGetを選択してください。

## <a name="gc-messages"></a>GCメッセージ

GCコンポーネントメッセージを表示するには、debug.mono.logシステムプロパティをgc を含む値に設定します。

GCメッセージは、GCが実行されるたびに生成され、GCによって行われた作業量に関する情報を提供します。

```shell
I/monodroid-gc(12331): GC cleanup summary: 81 objects tested - resurrecting 21.
```

タイミング情報などの追加のGC 情報は、`MONO_LOG_LEVEL`環境変数を`debug`に設定することにより生成されます。

```shell
adb shell setprop debug.mono.env MONO_LOG_LEVEL=debug
```

これにより、次の3つの結果を含む(多くの)追加のMono メッセージが生成されます。

```shell
D/Mono (15723): GC_BRIDGE num-objects 1 num_hash_entries 81226 sccs size 81223 init 0.00ms df1 285.36ms sort 38.56ms dfs2 50.04ms setup-cb 9.95ms free-data 106.54ms user-cb 20.12ms clenanup 0.05ms links 5523436/5523436/5523096/1 dfs passes 1104 6883/11046605
D/Mono (15723): GC_MINOR: (Nursery full) pause 2.01ms, total 287.45ms, bridge 225.60 promoted 0K major 325184K los 1816K
D/Mono ( 2073): GC_MAJOR: (user request) pause 2.17ms, total 2.47ms, bridge 28.77 major 576K/576K los 0K/16K
```

`GC_BRIDGE`メッセージで、`num-objects`はこのパスが考慮しているブリッジオブジェクトの数を示し、`num_hash_entries`は、このブリッジコードの呼び出し中に処理されたオブジェクトの数を示します。

`GC_MINOR`や`GC_MAJOR`のメッセージでは、`total`はワールドが中断されている(スレッドが実行されていない)時間 を示し、`bridge`はブリッジ処理コード(Java VMとの間の処理) でかかった時間の長さを示します。 ブリッジ処理が行われている間には、ワールドは中断*しません*。

 *一般的に*、`num_hash_entries`の値が大きいほど、`bridge`コレクションにかかる時間が長くなり、収集にかかる`total` 時間が長くなります。

## <a name="global-reference-messages"></a>グローバル参照メッセージを出力します。

グローバル参照(GREF)のログ記録を有効にするには、*debug.mono.log*システムプロパティには、次のように*gref*が含まれている必要があります。

```shell
adb shell setprop debug.mono.log gref
```

Xamarin.AndroidはAndroidのグローバル参照を使用してJavaインスタンスと関連付けられたマネージインスタンスの間のマッピングを提供します。Java メソッドを呼び出すときは、Javaインスタンスを Javaに提供する必要があります。

残念ながら、Android エミュレーターでは一度に2000のグローバル参照しか存在できません。 ハードウェアでは、遥かに多い52000までグローバル参照の制限が緩和されます。 エミュレーターでアプリケーションを実行すると、この低い制限が問題になる場合があります。そのため、インスタンスの*場所*を把握することは非常に便利です。

> [!NOTE]
> グローバル参照のカウントはXamarin.Android の内部にあり、プロセスに読み込まれた他のネイティブライブラリによって取得されたグローバル参照を含めることはありません(できません)。 グローバル参照カウントを推定値として使用します。

```shell
I/monodroid-gref(12405): +g+ grefc 108 gwrefc 0 obj-handle 0x40517468/L -> new-handle 0x40517468/L from    at Java.Lang.Object.RegisterInstance(IJavaObject instance, IntPtr value, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Object.SetHandle(IntPtr value, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Object..ctor(IntPtr handle, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor..ctor(System.Action handler, Boolean removable)
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor..ctor(System.Action handler)
I/monodroid-gref(12405):    at Android.App.Activity.RunOnUiThread(System.Action action)
I/monodroid-gref(12405):    at Mono.Samples.Hello.HelloActivity.UseLotsOfMemory(Android.Widget.TextView textview)
I/monodroid-gref(12405):    at Mono.Samples.Hello.HelloActivity.<OnCreate>m__3(System.Object o)
I/monodroid-gref(12405): handle 0x40517468; key_handle 0x40517468: Java Type: `mono/java/lang/RunnableImplementor`; MCW type: `Java.Lang.Thread+RunnableImplementor`
I/monodroid-gref(12405): Disposing handle 0x40517468
I/monodroid-gref(12405): -g- grefc 107 gwrefc 0 handle 0x40517468/L from    at Java.Lang.Object.Dispose(System.Object instance, IntPtr handle, IntPtr key_handle, JObjectRefType handle_type)
I/monodroid-gref(12405):    at Java.Lang.Object.Dispose()
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor.Run()
I/monodroid-gref(12405):    at Java.Lang.IRunnableInvoker.n_Run(IntPtr jnienv, IntPtr native__this)
I/monodroid-gref(12405):    at System.Object.c200fe6f-ac33-441b-a3a0-47659e3f6750(IntPtr , IntPtr )
I/monodroid-gref(27679): +w+ grefc 1916 gwrefc 296 obj-handle 0x406b2b98/G -> new-handle 0xde68f4bf/W from take_weak_global_ref_jni
I/monodroid-gref(27679): -w- grefc 1915 gwrefc 294 handle 0xde691aaf/W from take_global_ref_jni
```

結果として、次の4つのメッセージがあります。

- グローバル参照の作成: *+g+* で始まる行で、作成するコードパスのスタックトレースが提供されます。
- グローバル参照の破棄: これらは *-g-* で始まる行であり、グローバル参照のコードパスの破棄のスタックトレースを提供する場合があります。 GCがgrefを処分する場合、スタックトレースは提供されません。
- 弱グローバル参照の作成: *+w+* で始まる行です。
- 弱グローバル参照の作成: *-w-* で始まる行です。

すべてのメッセージにおいて、*grefc*値は、Xamarin.Androidによって作成されたグローバル参照の数になります。一方、*grefwc*値は、Xamarin.Androidで作成された弱グローバル参照の数です。 *ハンドル*または*obj ハンドル*値はJNI handleの値であり、' */* 'の後の文字はハンドル値の型です。ローカル参照の場合は */L*、グローバル参照の場合は */G*、弱グローバル参照の場合は */W*になります。

GCプロセスの一部として、グローバル参照(+g+) が弱グローバル参照 (+w+と-g-を引き起こします)に変換され、Java側のGC が開始され、弱グローバル参照が収集されたかどうかが確認されます。 まだそれが生きている場合には、弱参照 (+g+,-w-) の周りに新しいgrefが作成されます。それ以外の場合は、弱参照が破棄されます(-w)。

## <a name="java-instance-is-created-and-wrapped-by-a-mcw"></a>Javaインスタンスが作成され、MCWによってラップされます。

```shell
I/monodroid-gref(27679): +g+ grefc 2211 gwrefc 0 obj-handle 0x4066df10/L -> new-handle 0x4066df10/L from ...
I/monodroid-gref(27679): handle 0x4066df10; key_handle 0x4066df10: Java Type: `android/graphics/drawable/TransitionDrawable`; MCW type: `Android.Graphics.Drawables.TransitionDrawable`
```

## <a name="a-gc-is-being-performed"></a>GCを実行しています...

```shell
I/monodroid-gref(27679): +w+ grefc 1953 gwrefc 259 obj-handle 0x4066df10/G -> new-handle 0xde68f95f/W from take_weak_global_ref_jni
I/monodroid-gref(27679): -g- grefc 1952 gwrefc 259 handle 0x4066df10/G from take_weak_global_ref_jni
```

## <a name="object-is-still-alive-as-handle--null"></a>ハンドルがnullでないならば、オブジェクトはまだ生きています
## <a name="wref-turned-back-into-a-gref"></a>wrefはgrefに戻されます

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x4066df10
I/monodroid-gref(27679): +g+ grefc 1930 gwrefc 39 obj-handle 0xde68f95f/W -> new-handle 0x4066df10/G from take_global_ref_jni
I/monodroid-gref(27679): -w- grefc 1930 gwrefc 38 handle 0xde68f95f/W from take_global_ref_jni
```

## <a name="object-is-dead-as-handle--null"></a>ハンドルがnullなら、オブジェクトは死んでいます
## <a name="wref-is-freed-no-new-gref-created"></a>wref が解放され、新しいgrefは作成されません

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x0
I/monodroid-gref(27679): -w- grefc 1914 gwrefc 296 handle 0xde68f95f/W from take_global_ref_jni
```

ここで「興味深い」注意点が1つあります。4.0以前のAndroid を実行しているターゲットでは、その値はAndroidランタイムのメモリ内のJava オブジェクトのアドレスと同じになります。 (つまり、GCは移動不可で保守的なコレクターであり、それらのオブジェクトへの直接参照を処理します。)したがって、+g+、+w+、-g-、+g+、-w-シーケンスの後に結果として得られる値は、元のgrefの値と同じになります。 これにより、ログのgrepが非常に簡単になります。

ただし、Android 4.0には移動コレクターがあり、AndroidランタイムVMオブジェクトへの直接の参照が与えられることはありません。 その結果、+g+、+w+、-g-、+g+、-w-シーケンスの後には、*のgrefの値は*と異なるでしょう。 オブジェクトが複数のGCの後に存在する場合には複数の値が変更されるため、インスタンスが実際に割り当てられた場所を特定するのが難しくなります。

### <a name="querying-programmatically"></a>プログラムによるクエリ

`JniRuntime`オブジェクトに対してクエリを実行することによって、GREFとWREFの数を計算することができます。

`Java.Interop.JniRuntime.CurrentRuntime.GlobalReferenceCount`-グローバル参照の数

`Java.Interop.JniRuntime.CurrentRuntime.WeakGlobalReferenceCount`-弱参照の数

## <a name="android-debug-logs"></a>Androidのデバッグログ

[Androidのデバッグログ](~/android/deploy-test/debugging/android-debug-log.md)では、表示されているランタイムエラーに関する追加のコンテキストが提供される場合があります。

## <a name="floating-point-performance-is-terrible"></a>浮動小数点のパフォーマンスが低下しています!

または、「デバッグビルドではリリースビルドと比べてアプリが10倍高速に実行されます!」

Xamarin.Android は、複数のデバイスABIをサポートしています。 *armeabi*、*armeabi-v7a*、そして*x86*です。 デバイスABIは **[プロジェクトのプロパティ]、[アプリケーション]、[サポートされているアーキテクチャ]** で指定できます。

デバッグビルドではすべてのABIを提供するAndroidパッケージが使用されるため、ターゲットデバイスで最も高速なABIが使用されます。

リリースビルドでは、[プロジェクトのプロパティ]タブで選択したABIのみが使用されます。2つ以上を選択することも可能です。

*armeabi*は既定のABI であり、最も広範なデバイスサポートを備えています。 *ただし*、armeabi ではマルチCPU デバイスとハードウェアの浮動小数点はサポートされていません。 したがって、armeabiリリースランタイムを使用するアプリは1つのコアに関連付けられ、ソフトウェアの浮動小数点を実装します。 このどちらも、アプリケーションのパフォーマンスを大幅に低下させる可能性があります。

アプリで適切な浮動小数点のパフォーマンスを必要とする場合(ゲームなど)では、*armeabi-v7a*ABI を有効にする必要があります。 *armeabi-v7a*ランタイムだけをサポートすることもできますが、これは*armeabi*のみをサポートする古いデバイスがアプリを実行できないことを意味します。

## <a name="could-not-locate-android-sdk"></a>Android SDKが見つかりませんでした

Windows 用Android SDK には、Googleから2つのダウンロードが用意されています。
.exeインストーラーを選択すると、XamarinAndroidにインストール場所を通知するレジストリキーが書き込まれます。 .zipファイルを選択して自分で解凍した場合、Xamarin.AndroidにはSDKを検索する場所がわかりません。 Visual Studio上でXamarin AndroidにSDKの場所を通知するには、 **[ツール]、[オプション]、[Xamarin]、[Androidの設定]** の順に移動します。

[![Xamarin.Android 設定でのAndroid SDKの場所](troubleshooting-images/01.png)](troubleshooting-images/01.png#lightbox)

## <a name="ide-does-not-display-target-device"></a>IDEにターゲットデバイスが表示されない

アプリケーションをデバイスにデプロイしようとしたときに、[デバイスの選択]ダイアログにデプロイする先のデバイスが表示されないことがあります。 これは、Android Debug Bridgeがお休みしてしまったときに起きる場合があります。

この問題を診断するには、[adb program](~/android/deploy-test/debugging/android-debug-log.md)を見つけて、次のとおり実行します。

```shell
adb devices
```

デバイスが存在しない場合には、Android Debug Bridgeサーバーを再起動して、デバイスが検出されるようにする必要があります。

```shell
adb kill-server
adb start-server
```

HTC Syncソフトウェアは**adb start-server** の正常な動作を妨げることがあります。 **adb start-server**コマンドで開始されているポートが出力されない場合にはHTC Syncソフトウェアを終了し、adbサーバーを再起動してください。

## <a name="the-specified-task-executable-keytool-could-not-be-run"></a>指定された実行可能タスク「Keytool」を実行できませんでした。

これは、Java SDKのbinディレクトリが配置されているディレクトリが、PATHに含まれていないことを意味します。 [インストール](~/android/get-started/installation/index.md)ガイドの手順に従っていたことを確認します。

## <a name="monodroidexe-or-aresgenexe-exited-with-code-1"></a>monodroid.exeかaresgen.exeがコード1で終了します

この問題をデバッグするには、Visual Studioに移動し、MSBuildの詳細レベルを変更します。これを行うには、次のように選択します。 **[ツール]、[オプション]、[プロジェクト**と**ソリューション]、[ビルド**と**実行]、[MSBuildプロジェクトビルドの出力の詳細度]** の順に選択 し、この値を**通常**に設定します。

リビルドし、Visual Studioの出力ウィンドウを確認します。そこにエラーの完全な詳細が含まれているでしょう。

## <a name="there-is-not-enough-storage-space-on-the-device-to-deploy-the-package"></a>パッケージを展開するための十分な記憶領域がデバイスにありません

このエラーは、Visual Studio内からエミュレーターを起動していない場合に発生します。 Visual Studioの外部でエミュレーターを起動する場合には、`-partition-size 512`オプションを渡す必要があります。たとえば、

```shell
emulator -partition-size 512 -avd MonoDroid
```

シミュレーター[を構成するときに使用した名前](~/android/get-started/installation/windows.md#device)など、正しいシミュレーター名を使用していることを確認します。

## <a name="install_failed_invalid_apk-when-installing-a-package"></a>パッケージのインストール時にINSTALL\_FAILED\_INVALID\_APKとなりました

Androidのパッケージ名にはピリオド (' *.* ') を含む*必要が*あります。 パッケージ名を編集して、ピリオドが含まれるようにします。

- Visual Studioでは:
  - プロジェクトを右クリックし、[プロパティ]を選択します。
  - 左側の[Androidマニフェスト]タブをクリックします。
  - パッケージ名の欄を更新します。
    - &ldquo;AndroidManifest.xmlというメッセージが表示された場合は 追加するをクリックします。&rdquo;、リンクをクリックし、パッケージ名の欄を更新します。
- Visual Studio for Macでは:
  - プロジェクトを右クリックし、[オプション]を選択します。
  - [ビルド/Android アプリケーション]セクションに移動します。
  - 「.」が含まれるようにパッケージ名の欄を変更します。

## <a name="install_failed_missing_shared_library-when-installing-a-package"></a>パッケージのインストール時にINSTALL\_FAILED\_MISSING\_SHARED\_LIBRARY となりました

ここで言う「共有ライブラリ」 は、ネイティブの共有ライブラリ (*libfoo.so*) ファイルでは*ありません*。Google Mapsなど、ターゲットデバイスに個別にインストールする必要があるライブラリを指します。

Android パッケージでは、`<uses-library/>`要素に必要な共有ライブラリを指定します。 *必要な*ライブラリがターゲットデバイスに存在しない場合 (`//uses-library/@android:required`が既定で*true*である場合など)、パッケージのインストールは*INSTALL\_FAILED\_MISSING\_SHARED\_LIBRARY*で失敗します。

どの共有ライブラリが必要かを判断するには、*生成された*
**AndroidManifest.xml** ファイル (例: **obj\\Debug\\android\\AndroidManifest.xml**) を開き、`<uses-library/>`要素を探します。 `<uses-library/>`要素は、プロジェクトの**プロパティ\\AndroidManifest.xml**ファイルに手動で追加することも、[UsesLibraryAttributeのカスタム属性](xref:Android.App.UsesLibraryAttribute)を使用して追加することもできます。

たとえば、アセンブリ参照を*Mono.GoogleMap.dll* に追加すると、Google Maps共有ライブラリの`<uses-library/>`が暗黙的に追加されます。

## <a name="install_failed_update_incompatible-when-installing-a-package"></a>パッケージをインストールするときに、INSTALL\_FAILED\_UPDATE\_INCOMPATIBLEとなりました

Androidパッケージには、次の3つの要件があります。

- 「.」が含まれている必要があります。(前のエントリを参照)
- 一意の文字列パッケージ名を持つ必要があります(したがってAndroid app名には逆tld則が適用され、例えばChrome appの場合にはcom.android.chromeとなります)。
- パッケージをアップグレードするときは、パッケージの署名キーが同じである必要があります。

ここで次のシナリオを考えてみましょう。

1. アプリをデバッグアプリとしてビルド&デプロイします。
2. そして、たとえばリリースアプリとして使用する場合に(または、既定で提供されるデバッグ署名キーが気に入らない場合)、署名キーを変更します。
3. そして、Visual Studio内で[デバッグ]、[開始]からデバッグするなどしてそれを削除しないまま、アプリをインストールします。

この場合、署名キーが変更されているにも関わらずパッケージ名が変更されていなかったため、INSTALL\_FAILED\_UPDATE\_INCOMPATIBLEによりインストールが失敗します。 [Android のデバッグログ](~/android/deploy-test/debugging/android-debug-log.md)には、次のようなメッセージも含まれます。

```shell
E/PackageManager(  146): Package [PackageName] signatures do not match the previously installed version; ignoring!
```

このエラーを解決するには、再インストールする前にデバイスからアプリケーションを完全に削除します。

## <a name="install_failed_uid_changed-when-installing-a-package"></a>パッケージのインストール時にINSTALL\_FAILED\_UID\_CHANGEDとなりました

Androidパッケージをインストールすると、*ユーザーID*(UID)が割り当てられます。
*場合によっては*、現時点では不明な理由により、既にインストールされているアプリをインストールすると`INSTALL_FAILED_UID_CHANGED`で失敗します。

```shell
ERROR [2015-03-23 11:19:01Z]: ANDROID: Deployment failed
Mono.AndroidTools.InstallFailedException: Failure [INSTALL_FAILED_UID_CHANGED]
   at Mono.AndroidTools.Internal.AdbOutputParsing.CheckInstallSuccess(String output, String packageName)
   at Mono.AndroidTools.AndroidDevice.<>c__DisplayClass2c.<InstallPackage>b__2b(Task`1 t)
   at System.Threading.Tasks.ContinuationTaskFromResultTask`1.InnerInvoke()
   at System.Threading.Tasks.Task.Execute()
```

この問題を回避するには、Androidパッケージを *完全にアンインストール*するか、してAndroidターゲットのGUI からアプリをインストールします`adb`。

```shell
$ adb uninstall @PACKAGE_NAME@
```

`adb uninstall -k`を**使用しないでください**。アプリケーションデータが*保存*され、ターゲットデバイスで競合するUID が保持されてしまいます。

## <a name="release-apps-fail-to-launch-on-device"></a>デバイスでリリースアプリを起動できない

Android のデバッグログには、次のようなメッセージも含まれます。

```shell
D/AndroidRuntime( 1710): Shutting down VM
W/dalvikvm( 1710): threadid=1: thread exiting with uncaught exception (group=0xb412f180)
E/AndroidRuntime( 1710): FATAL EXCEPTION: main
E/AndroidRuntime( 1710): java.lang.UnsatisfiedLinkError: Couldn't load monodroid: findLibrary returned null
E/AndroidRuntime( 1710):        at java.lang.Runtime.loadLibrary(Runtime.java:365)
```

この問題の原因は2つ考えられます。

1. .apkに、ターゲットデバイスでサポートされているABI が用意されていません。
    たとえば、.apkにarmeabi-v7aバイナリのみが含まれ、ターゲットデバイスはarmeabiのみをサポートしている場合です。

2. [Androidのバグ](https://code.google.com/p/android/issues/detail?id=21670)。 このような場合には、アプリをアンインストールし、祈り、そしてアプリを再インストールしましょう。

(1)を修正するには、プロジェクトのオプションとプロパティを編集し [必要なABI のサポートをサポートされている ABI](~/android/app-fundamentals/cpu-architectures.md)の一覧に追加します。 どのABI を追加する必要があるかを判断するには、ターゲットデバイスに対して次のadb コマンドを実行します。

```shell
adb shell getprop ro.product.cpu.abi
adb shell getprop ro.product.cpu.abi2
```

出力には、プライマリ(セカンダリを持つ場合もあります)のABIが含まれます。

```shell
$ adb shell getprop | grep ro.product.cpu
[ro.product.cpu.abi2]: [armeabi]
[ro.product.cpu.abi]: [armeabi-v7a]
```

## <a name="the-outpath-property-is-not-set-for-project-ldquomyappcsprojrdquo"></a>プロジェクト&ldquo;MyApp.csproj&rdquo;のOutPath プロパティが設定されていません

一般に、これはHP コンピューター上で環境変数&ldquo;Platform&rdquo;がMCDやHPDなどに設定されていることを意味します。 これは、通常&ldquo;Any CPU&rdquo;または&ldquo;x86&rdquo;に設定されるMSBuildプラットフォームプロパティと競合します。 MSBuildを機能させるには、この環境変数をコンピューターから削除する必要があります。

- [コントロールパネル]、[システム]、[高度な設定]、[環境変数]

Visual StudioまたはVisual Studio for Macを再起動し、リビルドを試行します。 正常に機能するようになったはずです。

## <a name="javalangclasscastexception-monoandroidruntimejavaobject-cannot-be-cast-to"></a>java.lang.ClassCastException: mono.android.runtime.JavaObjectにキャストできません

Xamarin.Android 4.xでは、ジェネリック型の入れ子は適切にマーシャリングされていません。 例えば、[SimpleExpandableListAdapter](xref:Android.Widget.SimpleExpandableListAdapter)を使用する次のC\#コードについて考えてみます。

```csharp
// BAD CODE; DO NOT USE
var groupData = new List<IDictionary<string, object>> () {
        new Dictionary<string, object> {
                { "NAME", "Group 1" },
                { "IS_EVEN", "This group is odd" },
        },
};
var childData = new List<IList<IDictionary<string, object>>> () {
        new List<IDictionary<string, object>> {
                new Dictionary<string, object> {
                        { "NAME", "Child 1" },
                        { "IS_EVEN", "This group is odd" },
                },
        },
};
mAdapter = new SimpleExpandableListAdapter (
        this,
        groupData,
        Android.Resource.Layout.SimpleExpandableListItem1,
        new string[] { "NAME", "IS_EVEN" },
        new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 },
        childData,
        Android.Resource.Layout.SimpleExpandableListItem2,
        new string[] { "NAME", "IS_EVEN" },
        new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 }
);
```

問題は、Xamarin.Android ではジェネリック型の入れ子を正しくマーシャリングできないことにあります。 `List<IDictionary<string, object>>`は[java.lang.ArrrayList](xref:Java.Util.ArrayList)にマーシャリングされていますが、`ArrayList`は[java.util.Map](xref:Java.Util.IMap)を実装するものではなく(`Dictionary<string, object>`インスタンスを参照する)`mono.android.runtime.JavaObject`インスタンスであるため、次の例外が発生します。

```shell
E/AndroidRuntime( 2991): FATAL EXCEPTION: main
E/AndroidRuntime( 2991): java.lang.ClassCastException: mono.android.runtime.JavaObject cannot be cast to java.util.Map
E/AndroidRuntime( 2991):        at android.widget.SimpleExpandableListAdapter.getGroupView(SimpleExpandableListAdapter.java:278)
E/AndroidRuntime( 2991):        at android.widget.ExpandableListConnector.getView(ExpandableListConnector.java:446)
E/AndroidRuntime( 2991):        at android.widget.AbsListView.obtainView(AbsListView.java:2271)
E/AndroidRuntime( 2991):        at android.widget.ListView.makeAndAddView(ListView.java:1769)
E/AndroidRuntime( 2991):        at android.widget.ListView.fillDown(ListView.java:672)
E/AndroidRuntime( 2991):        at android.widget.ListView.fillFromTop(ListView.java:733)
E/AndroidRuntime( 2991):        at android.widget.ListView.layoutChildren(ListView.java:1622)
```

回避策としては、指定された&ldquo;内部&rdquo; 型のための`System.Collections.Generic`型の代わりに[Java コレクション型](~/android/internals/api-design.md)を使用します。 これにより、インスタンスをマーシャリングするときに、適切なJavaの型が得られます。 (次のコードは、grefの有効期限を短縮するために必要以上に複雑です。 grefの有効期限が気にならない場合には、`s/List/JavaList/g`と`s/Dictionary/JavaDictionary/g`を使用して元のコードをより単純化することができます)。

```csharp
// insert good code here
using (var groupData = new JavaList<IDictionary<string, object>> ()) {
    using (var groupEntry = new JavaDictionary<string, object> ()) {
        groupEntry.Add ("NAME", "Group 1");
        groupEntry.Add ("IS_EVEN", "This group is odd");
        groupData.Add (groupEntry);
    }
    using (var childData = new JavaList<IList<IDictionary<string, object>>> ()) {
        using (var childEntry = new JavaList<IDictionary<string, object>> ())
        using (var childEntryDict = new JavaDictionary<string, object> ()) {
            childEntryDict.Add ("NAME", "Child 1");
            childEntryDict.Add ("IS_EVEN", "This child is odd.");
            childEntry.Add (childEntryDict);
            childData.Add (childEntry);
        }
        mAdapter = new SimpleExpandableListAdapter (
            this,
            groupData,
            Android.Resource.Layout.SimpleExpandableListItem1,
            new string[] { "NAME", "IS_EVEN" },
            new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 },
            childData,
            Android.Resource.Layout.SimpleExpandableListItem2,
            new string[] { "NAME", "IS_EVEN" },
            new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 }
        );
    }
}
```

[これは今後のリリースで修正される予定です](https://bugzilla.xamarin.com/show_bug.cgi?id=5401)。

## <a name="unexpected-nullreferenceexceptions"></a>想定外のNullReferenceExceptions

&ldquo;動作の失敗&rdquo;あるいはMono for Android ランタイムコードに由来するアプリが停止する直前に、[Androidのデバッグログ](~/android/deploy-test/debugging/android-debug-log.md)がNullReferenceExceptions を示すことがあります。

```shell
E/mono(15202): Unhandled Exception: System.NullReferenceException: Object reference not set to an instance of an object
E/mono(15202):   at Java.Lang.Object.GetObject (IntPtr handle, System.Type type, Boolean owned)
E/mono(15202):   at Java.Lang.Object._GetObject[IOnTouchListener] (IntPtr handle, Boolean owned)
E/mono(15202):   at Java.Lang.Object.GetObject[IOnTouchListener] (IntPtr handle, Boolean owned)
E/mono(15202):   at Android.Views.View+IOnTouchListenerAdapter.n_OnTouch_Landroid_view_View_Landroid_view_MotionEvent_(IntPtr jnienv, IntPtr native__this, IntPtr native_v, IntPtr native_e)
E/mono(15202):   at (wrapper dynamic-method) object:b039cbb0-15e9-4f47-87ce-442060701362 (intptr,intptr,intptr,intptr)
```

or

```shell
E/mono    ( 4176): Unhandled Exception:
E/mono    ( 4176): System.NullReferenceException: Object reference not set to an instance of an object
E/mono    ( 4176): at Android.Runtime.JNIEnv.NewString (string)
E/mono    ( 4176): at Android.Util.Log.Info (string,string)
```

これは、Androidランタイムがプロセスを中止することを決定した場合に発生する可能性があります。これは、ターゲットのGREFの制限に達したり、JNIに何かしらの&rdquo;異常&ldquo;が発生したりしたなど、さまざまな理由で発生する可能性があります。

この問題が発生しているかどうかを確認するには、Androidデバッグログで、次のようなプロセスからのメッセージを確認します。

```shell
E/dalvikvm(  123): VM aborting
```

## <a name="abort-due-to-global-reference-exhaustion"></a>グローバル参照の枯渇による終了

AndroidランタイムのJNI レイヤーでサポートされるのは、特定の時点で有効なJNI オブジェクト参照の数が限られている場合のみです。 この制限を超えると、状況は破綻します。

GREF(*グローバル参照*) の制限は、エミュレーターでは2000参照、ハードウェアでは最大52000参照です。

Androidデバッグログに次のようなメッセージが表示されたときには、作成されたGREFが多すぎることがわかっています。

```shell
D/dalvikvm(  602): GREF has increased to 1801
```

GREFの上限に達した場合は、次のようなメッセージが出力されます。

```shell
D/dalvikvm(  602): GREF has increased to 2001
W/dalvikvm(  602): Last 10 entries in JNI global reference table:
W/dalvikvm(  602):  1991: 0x4057eff8 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1992: 0x4057f010 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1993: 0x40698e70 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1994: 0x40698e88 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1995: 0x40698ea0 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1996: 0x406981f0 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1997: 0x40698208 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1998: 0x40698220 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1999: 0x406956a8 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  2000: 0x406956c0 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602): JNI global reference table summary (2001 entries):
W/dalvikvm(  602):    51 of Ljava/lang/Class; 164B (41 unique)
W/dalvikvm(  602):    46 of Ljava/lang/Class; 188B (17 unique)
W/dalvikvm(  602):     6 of Ljava/lang/Class; 212B (6 unique)
W/dalvikvm(  602):    11 of Ljava/lang/Class; 236B (7 unique)
W/dalvikvm(  602):     3 of Ljava/lang/Class; 260B (3 unique)
W/dalvikvm(  602):     4 of Ljava/lang/Class; 284B (2 unique)
W/dalvikvm(  602):     8 of Ljava/lang/Class; 308B (6 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 316B
W/dalvikvm(  602):     4 of Ljava/lang/Class; 332B (3 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 356B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 380B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 428B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 452B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 476B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 500B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 548B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 572B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 596B (2 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 692B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 956B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1004B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1148B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 1172B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1316B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 3428B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 3452B
W/dalvikvm(  602):     1 of Ljava/lang/String; 28B
W/dalvikvm(  602):     2 of Ldalvik/system/VMRuntime; 12B (1 unique)
W/dalvikvm(  602):    10 of Ljava/lang/ref/WeakReference; 28B (10 unique)
W/dalvikvm(  602):     1 of Ldalvik/system/PathClassLoader; 44B
W/dalvikvm(  602):  1553 of Landroid/graphics/Point; 20B (1553 unique)
W/dalvikvm(  602):   261 of Landroid/graphics/Point; 28B (261 unique)
W/dalvikvm(  602):     1 of Landroid/view/MotionEvent; 100B
W/dalvikvm(  602):     1 of Landroid/app/ActivityThread$ApplicationThread; 28B
W/dalvikvm(  602):     1 of Landroid/content/ContentProvider$Transport; 28B
W/dalvikvm(  602):     1 of Landroid/view/Surface$CompatibleCanvas; 44B
W/dalvikvm(  602):     1 of Landroid/view/inputmethod/InputMethodManager$ControlledInputConnectionWrapper; 36B
W/dalvikvm(  602):     1 of Landroid/view/ViewRoot$1; 12B
W/dalvikvm(  602):     1 of Landroid/view/ViewRoot$W; 28B
W/dalvikvm(  602):     1 of Landroid/view/inputmethod/InputMethodManager$1; 28B
W/dalvikvm(  602):     1 of Landroid/view/accessibility/AccessibilityManager$1; 28B
W/dalvikvm(  602):     1 of Landroid/widget/LinearLayout$LayoutParams; 44B
W/dalvikvm(  602):     1 of Landroid/widget/LinearLayout; 332B
W/dalvikvm(  602):     2 of Lorg/apache/harmony/xnet/provider/jsse/TrustManagerImpl; 28B (1 unique)
W/dalvikvm(  602):     1 of Landroid/view/SurfaceView$MyWindow; 36B
W/dalvikvm(  602):     1 of Ltouchtest/RenderThread; 92B
W/dalvikvm(  602):     1 of Landroid/view/SurfaceView$3; 12B
W/dalvikvm(  602):     1 of Ltouchtest/DrawingView; 412B
W/dalvikvm(  602):     1 of Ltouchtest/Activity1; 180B
W/dalvikvm(  602): Memory held directly by tracked refs is 75624 bytes
E/dalvikvm(  602): Excessive JNI global references (2001)
E/dalvikvm(  602): VM aborting
```

上記の例 (ちなみに、これは[バグ685215](https://bugzilla.novell.com/show_bug.cgi?id=685215)によるものです)では、問題は大量のAndroid. Graphics.Pointインスタンスが生成されていることにあります。この特定のバグの修正の一覧については、[コメント\#2](https://bugzilla.novell.com/show_bug.cgi?id=685215#c2)を参照してください。

通常、便利な解決策としては、上のダンプ&ndash;上から&ndash;Android.Graphics.Pointインスタンスが過剰に生成されている型を見つけて、そのソースコード上の位置を特定し、適切に破棄します(Javaオブジェクトの有効期間が短縮されるようにするためです)。 これは常に適切であるとは限りません(\#685215 はマルチスレッドであるため、単純なソリューションとしては破棄の呼び出しを回避することです) が、最初に考慮する必要があります。

[GREFのログ記録](~/android/troubleshooting/index.md)を有効にすると、GREFが作成された時点と存在する数を確認できます。

## <a name="abort-due-to-jni-type-mismatch"></a>JNIの型不一致による終了

JNIコードをハンドロールする場合、型が正しく一致しない可能性があります。たとえば、`java.lang.Runnable`を実装していない型で`java.lang.Runnable.run`を呼び出そうとした場合です。 これが発生すると、Androidデバッグログに次のようなメッセージが表示されます。

```shell
W/dalvikvm( 123): JNI WARNING: can't call Ljava/Type;;.method on instance of Lanother/java/Type;
W/dalvikvm( 123):              in Lmono/java/lang/RunnableImplementor;.n_run:()V (CallVoidMethodA)
...
E/dalvikvm( 123): VM aborting
```

## <a name="dynamic-code-support"></a>動的コードのサポート

### <a name="dynamic-code-does-not-compile"></a>動的コードはコンパイルされません。

動的なC\#をアプリやライブラリで使用するには、プロジェクトにSystem.Core.dll、Microsoft.CSharp.dll、そしてMono.CSharp.dllを追加する必要があります。

### <a name="in-release-build-missingmethodexception-occurs-for-dynamic-code-at-run-time"></a>リリースビルドで、実行時に動的コードのMissingMethodException が発生します。

- おそらくアプリのプロジェクトに、System.Core.dll、Microsoft.CSharp.dll、またはMono.CSharp.dllへの参照が含まれていません。 これらのアセンブリが参照されていることを確認します。

  - 動的コードは常にコストがかかることに注意してください。 効率的なコードが必要な場合は、動的コードを使用しないことを検討してください。

- 最初のプレビューでは、各アセンブリの型がアプリケーションコードによって明示的に使用されていない限り、これらのアセンブリは除外されていました。 回避策については、次を参照してください。 [http://lists.ximian.com/pipermail/mo...il/009798.html](http://lists.ximian.com/pipermail/monodroid/2012-April/009798.html)

## <a name="projects-built-with-aotllvm-crash-on-x86-devices"></a>AOT+LLVMでビルドしたプロジェクトがx86デバイス上でクラッシュする

[AOT+LLVM](~/android/deploy-test/release-prep/index.md)でビルドしたアプリをx86ベースのデバイスにデプロイすると、次のような例外が表示されることがあります。

```shell
Assertion: should not be reached at /Users/.../external/mono/mono/mini/tramp-x86.c:124
Fatal signal 6 (SIGABRT), code -6 in tid 4051 (Xamarin.bug56111)
```

これは既知の問題であり、回避策はLLVMを無効にすることです。
