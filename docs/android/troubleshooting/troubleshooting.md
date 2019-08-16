---
title: トラブルシューティングのヒント
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 56137ACA-4811-B312-6860-E16D0FA123F7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/15/2018
ms.openlocfilehash: 5fccc07d35eda1ba420f48a8058d8d2a00b18fd9
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69523199"
---
# <a name="troubleshooting-tips"></a>トラブルシューティングのヒント


## <a name="getting-diagnostic-information"></a>診断情報の取得

Xamarin Android では、さまざまなバグを追跡する際にいくつかの場所を確認できます。
不足している機能には次が含まれます。

1. 診断 MSBuild の出力。
2. デバイス展開ログ。
3. Android デバッグログの出力。


<a name="Diagnostic_MSBuild_Output" />

## <a name="diagnostic-msbuild-output"></a>診断 MSBuild の出力

診断 MSBuild には、パッケージのビルドに関する追加情報を含めることができます。また、パッケージの配置情報が含まれている場合もあります。

Visual Studio 内で診断 MSBuild 出力を有効にするには:

1. [**ツール] > [オプション**] をクリックします。
2. 左側のツリービューで、[プロジェクトおよびソリューション] を選択し **> ビルドして実行**します。
3. 右側のパネルで、MSBuild ビルド出力の詳細度ドロップダウンを [診断] に設定します。
4. **[OK]** をクリックします。
5. パッケージから不要な要素を取り除き、再ビルドします。
6. 診断出力は、[出力] パネル内に表示されます。


Visual Studio for Mac/OS X で診断 MSBuild 出力を有効にするには:

1. **[Visual Studio for Mac > の設定]** をクリックします。
2. 左側のツリービューで、 **[プロジェクト > ビルド]** を選択します。
3. 右側のパネルで、[ログの詳細度] ドロップダウンを [診断] に設定します。
4. **[OK]** をクリックします。
5. Visual Studio for Mac を再起動します。
6. パッケージから不要な要素を取り除き、再ビルドします。
7. 診断出力は、[ビルド出力] ボタンをクリックすることで、エラーパッド (**表示 > パッド > エラー** ) 内に表示されます。




## <a name="device-deployment-logs"></a>デバイス展開ログ

Visual Studio 内でデバイス展開のログ記録を有効にするには:

1. **ツール > オプション...** >
2. 左側のツリービューで、 **[Xamarin > Android の設定]** を選択します。
3. 右側のパネルで、[X] 拡張機能のデバッグログを有効にします **([モノのデスクトップにログを書き込む**] チェックボックスをオンにします)。
4. ログメッセージは、デスクトップ上のログファイルに書き込まれます。


Visual Studio for Mac は、常にデバイスのデプロイログを書き込みます。 検索は少し難しくなります。次のように、デプロイが行われるたびに、 *AndroidUtils*ログファイルが作成されます。**Androidtools-2012-10-24 _12-35-45**。

- Windows では、ログファイルはに`%LOCALAPPDATA%\XamarinStudio-{VERSION}\Logs`書き込まれます。
- OS X では、ログファイルはに`$HOME/Library/Logs/XamarinStudio-{VERSION}`書き込まれます。




## <a name="android-debug-log-output"></a>Android デバッグログの出力

Android では、多くのメッセージが[Android デバッグログ](~/android/deploy-test/debugging/android-debug-log.md)に書き込まれます。
Xamarin android のシステムプロパティを使用して、Android デバッグログへの追加メッセージの生成を制御します。 Android システムプロパティは、 [Android Debug Bridge (adb)](https://developer.android.com/guide/developing/tools/adb.html)内の*setprop は*コマンドを使用して設定できます。

```shell
adb shell setprop PROPERTY_NAME PROPERTY_VALUE
```

システムプロパティは、プロセスの起動時に読み取られるため、アプリケーションを起動する前に設定するか、システムプロパティの変更後にアプリケーションを再起動する必要があります。



### <a name="xamarinandroid-system-properties"></a>Xamarin.Android のシステム プロパティ

Xamarin Android では、次のシステムプロパティがサポートされています。

- *デバッグし*ます。デバッグ:空でない文字列の場合、これはと同じ`*mono-debug*`です。

- *mono. env*:Mono が初期化さ *|* *れる前*に、アプリケーションの起動時にエクスポートする環境変数の、パイプで区切られた (' ') リスト。 これにより、mono ログを制御する環境変数を設定できます。

    - *注*:値は *|* ' ' で区切られているため、値は引用符の追加レベルを持つ必要が\`あります。これは、 *adb シェル*\`コマンドによって一連の引用符が削除されるためです。

    - *注*:Android システムプロパティの値の長さは92文字以下でなければなりません。

    - 例:

      ```
      adb shell setprop debug.mono.env "'MONO_LOG_LEVEL=info|MONO_LOG_MASK=asm'"
      ```

- *debug.mono.log*:Android デバッグログに追加のメッセージを出力する必要があるコンポーネントのコンマ区切り (' *,* ') リスト。 既定では、何も設定されていません。 コンポーネントは次のとおりです。

    - *すべて*:すべてのメッセージを印刷する
    - *gc*:GC 関連のメッセージを出力します。
    - *グリーン*:印刷 (脆弱、グローバル) 参照の割り当てと解放のメッセージ。
    - *持つ*:ローカル参照の割り当てと解放のメッセージを出力します。

    *注*: これらは*非常*に冗長です。 本当に必要な場合を除き、有効にしないでください。

- 次のように*トレースし*ます。[Mono--trace](http://docs.go-mono.com/?link=man%3amono(1)) `=PROPERTY_VALUE`設定を設定できます。

## <a name="deleting-bin-and-obj"></a>および`bin`の削除`obj`

Xamarin. Android は、次のような状況から過去に検出されました。

- 予期しないビルドまたは実行時エラーが発生した。
- `Clean`ディレクトリ`Rebuild`と`bin`ディレクトリは、、、または手動で削除します。`obj`
- 問題は解決しません。

開発者の生産性に影響を与えるために、このような問題の解決に多大な投資を行っています。

このような問題が発生した場合は、次のようになります。

1. インクリメンタルノートを作成します。 プロジェクトをこの状態にする最後のアクションは何ですか?
1. 現在のビルドログを保存します。 もう一度ビルドし、[診断ビルドログ](#diagnostic-msbuild-output)を記録してください。
1. [バグレポート][bug]を送信します。

ディレクトリ`bin` と`obj`ディレクトリを削除する前に、これらのディレクトリを zip 形式にし、必要に応じて後で診断できるように保存します。 単`Clean`に Xamarin Android アプリケーションプロジェクトを使用すれば、作業を再び行うことができます。

[bug]: https://github.com/xamarin/xamarin-android/wiki/Submitting-Bugs,-Feature-Requests,-and-Pull-Requests

## <a name="xamarinandroid-cannot-resolve-systemvaluetuple"></a>Xamarin Android では、ValueTuple を解決できません。

このエラーは、Visual Studio との互換性がないことが原因で発生します。

- **Visual Studio 2017 更新プログラム 1**(バージョン15.1 またはそれ以前) は、 **System. ValueTuple NuGet 4.3.0** (またはそれ以前) とのみ互換性があります。

- **Visual Studio 2017 更新プログラム 2**(バージョン15.2 以降) は、4.3.1 (またはそれ以降) の**システム**とのみ互換性があります。

Visual Studio 2017 のインストールに対応する適切な system.servicemodel タプル NuGet を選択してください。


## <a name="gc-messages"></a>GC メッセージ

GC コンポーネントメッセージを表示するには、gc を含む値に "mono. log システム" プロパティを設定します。

Gc メッセージは、gc が実行されるたびに生成され、GC によって行われた作業量に関する情報を提供します。

```shell
I/monodroid-gc(12331): GC cleanup summary: 81 objects tested - resurrecting 21.
```

環境変数を次のように設定することによって`MONO_LOG_LEVEL` 、タイミング情報`debug`などの追加の GC 情報を生成できます。

```shell
adb shell setprop debug.mono.env MONO_LOG_LEVEL=debug
```

これにより、次の3つの結果を含む、(多くの) 追加の Mono メッセージが生成されます。

```shell
D/Mono (15723): GC_BRIDGE num-objects 1 num_hash_entries 81226 sccs size 81223 init 0.00ms df1 285.36ms sort 38.56ms dfs2 50.04ms setup-cb 9.95ms free-data 106.54ms user-cb 20.12ms clenanup 0.05ms links 5523436/5523436/5523096/1 dfs passes 1104 6883/11046605
D/Mono (15723): GC_MINOR: (Nursery full) pause 2.01ms, total 287.45ms, bridge 225.60 promoted 0K major 325184K los 1816K
D/Mono ( 2073): GC_MAJOR: (user request) pause 2.17ms, total 2.47ms, bridge 28.77 major 576K/576K los 0K/16K
```

メッセージでは、 `num-objects`はこの`num_hash_entries`パスが考慮するブリッジオブジェクトの数です。は、このブリッジコードの呼び出し中に処理されたオブジェクトの数です。 `GC_BRIDGE`

とのメッセージでは`total` 、は、世界が一時停止している時間 (スレッドは実行されて`bridge`いません) であり、はブリッジ処理コード (Java VM との間) でかかった時間です。 `GC_MAJOR` `GC_MINOR` ブリッジ処理が行われている間、世界は一時停止*されていません*。

 *一般に*、の`num_hash_entries`値が大きいほど、 `bridge`コレクションにかかる時間が長くなり、収集にかかる`total`時間が長くなります。



## <a name="global-reference-messages"></a>グローバル参照メッセージ

グローバル参照 loggig (グリーン) のログ記録を有効にするには、次のように、 *mono. log*システムプロパティに値が含まれている必要があります。

```shell
adb shell setprop debug.mono.log gref
```

Xamarin Android のグローバル参照を使用して Java インスタンスと関連付けられたマネージインスタンスの間のマッピングを提供します。 Java メソッドを呼び出すときは、java インスタンスを java に提供する必要があります。

残念ながら、Android エミュレーターでは、一度に2000のグローバル参照しか存在できません。 ハードウェアでは、52000のグローバル参照の制限がはるかに高くなっています。 エミュレーターでアプリケーションを実行するときには、制限が低いため、インスタンスが*どこ*から取得されたかを把握することが非常に便利な場合があります。

 *注*: グローバル参照カウントは、Xamarin. Android の内部にあり、プロセスに読み込まれた他のネイティブライブラリによって取得されたグローバル参照を含めることはできません。 グローバル参照カウントを推定値として使用します。

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

- グローバル参照の作成: *+ g +* で始まる行で、作成するコードパスのスタックトレースが提供されます。
- グローバル参照の破棄: これらは、 *-g-* で始まる行であり、グローバル参照のコードパスの破棄のスタックトレースを提供する場合があります。 GC がの場合、スタックトレースは提供されません。
- 弱いグローバル参照の作成: *+ w +* で始まる行です。
- 弱いグローバル参照破棄: これらは *、-w から*始まる行です。


すべてのメッセージでは、 *grefc*の値は、xamarin によって作成されたグローバル参照の数を示します。 *grefc*値は、xamarin によって作成された弱いグローバル参照の数です。 *Handle*または*obj-* handle の値は JNI handle の値で、' ' の後 */* の文字はハンドル値の型 (ローカル参照の場合は */l* 、グローバル参照の場合は */g* 、弱グローバル参照の場合は " */w* ") になります。

GC プロセスの一部として、グローバル参照 (+ g +) が弱いグローバル参照 (+ w + と-g-) に変換され、Java 側 GC が開始され、弱いグローバル参照が収集されたかどうかが確認されます。 まだ生きている場合は、弱い参照 (+ g +,-w) の周りに新しいグリーンが作成されます。それ以外の場合は、弱い参照が破棄されます (-w)。

## <a name="java-instance-is-created-and-wrapped-by-a-mcw"></a>Java インスタンスが作成され、MCW によってラップされます。

```shell
I/monodroid-gref(27679): +g+ grefc 2211 gwrefc 0 obj-handle 0x4066df10/L -> new-handle 0x4066df10/L from ...
I/monodroid-gref(27679): handle 0x4066df10; key_handle 0x4066df10: Java Type: `android/graphics/drawable/TransitionDrawable`; MCW type: `Android.Graphics.Drawables.TransitionDrawable`
```

## <a name="a-gc-is-being-performed"></a>GC を実行しています...

```shell
I/monodroid-gref(27679): +w+ grefc 1953 gwrefc 259 obj-handle 0x4066df10/G -> new-handle 0xde68f95f/W from take_weak_global_ref_jni
I/monodroid-gref(27679): -g- grefc 1952 gwrefc 259 handle 0x4066df10/G from take_weak_global_ref_jni
```

## <a name="object-is-still-alive-as-handle--null"></a>ハンドルが null の場合、オブジェクトはまだ生きています
## <a name="wref-turned-back-into-a-gref"></a>wref がグリーンに戻される

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x4066df10
I/monodroid-gref(27679): +g+ grefc 1930 gwrefc 39 obj-handle 0xde68f95f/W -> new-handle 0x4066df10/G from take_global_ref_jni
I/monodroid-gref(27679): -w- grefc 1930 gwrefc 38 handle 0xde68f95f/W from take_global_ref_jni
```

## <a name="object-is-dead-as-handle--null"></a>オブジェクトが停止しています。ハンドル = = null
## <a name="wref-is-freed-no-new-gref-created"></a>wref が解放されました。新しいグリーンが作成されていません

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x0
I/monodroid-gref(27679): -w- grefc 1914 gwrefc 296 handle 0xde68f95f/W from take_global_ref_jni
```

ここでは、"興味深い" 注意点が1つあります。4.0 より前の Android を実行しているターゲットでは、その値は Android ランタイムのメモリ内の Java オブジェクトのアドレスと同じになります。 (つまり、GC は移動不可で、保守的なコレクターであり、それらのオブジェクトへの直接参照を処理します)。したがって、+ g +、+ w +、-g-、+ g +、-w シーケンスの後、結果として得られる値は、元の値の値と同じになります。 これにより、ログの grepping が非常に簡単になります。

ただし、android 4.0 には移動コレクターがあり、Android ランタイム VM オブジェクトへの直接の参照はありません。 その結果、+ g +、+ w +、-g-、+ g +、-w シーケンスの後に、値が*異なること*になります。 オブジェクトが複数の Gc の後に存在する場合は、複数の値が変更されるため、インスタンスが実際に割り当てられた場所を特定するのが難しくなります。

### <a name="querying-programmatically"></a>プログラムによるクエリ

オブジェクトに`JniRuntime`対してクエリを実行することによって、すべてのカウントと wref のカウントを照会できます。

`Java.Interop.JniRuntime.CurrentRuntime.GlobalReferenceCount`-グローバル参照カウント

`Java.Interop.JniRuntime.CurrentRuntime.WeakGlobalReferenceCount`-弱い参照カウント



## <a name="android-debug-logs"></a>Android デバッグログ

[Android デバッグログ](~/android/deploy-test/debugging/android-debug-log.md)には、表示されているランタイムエラーに関する追加のコンテキストが示される場合があります。



## <a name="floating-point-performance-is-terrible"></a>浮動小数点のパフォーマンスが低下しています。

または、"アプリはリリースビルドと比べて、デバッグビルドでは10倍高速に実行されます" ということもあります。

Xamarin は複数のデバイス ABIs をサポートしています。 *armeabi*、 *armeabi-armeabi-v7a*、 *x86*。 デバイス ABIs、 **[プロジェクトのプロパティ] > [アプリケーション] タブ > サポートされているアーキテクチャ**の中で指定できます。

デバッグビルドでは、すべての ABIs を提供する Android パッケージが使用されるため、ターゲットデバイスで最も高速な ABI が使用されます。

リリースビルドでは、[プロジェクトのプロパティ] タブで ABIs のみが選択されます。複数のを選択できます。

*armeabi*は既定の abi であり、最も広範なデバイスサポートを備えています。 *ただし*、armeabi では、マルチ CPU デバイスとハードウェアの浮動小数点はサポートされていません。 その結果、armeabi リリースランタイムを使用するアプリは1つのコアに関連付けられ、ソフト浮動小数点の実装を使用します。 どちらも、アプリケーションのパフォーマンスが大幅に低下する可能性があります。

アプリで適正な浮動小数点のパフォーマンス (ゲームなど) が必要な場合は、 *armeabi-Armeabi-v7a* ABI を有効にする必要があります。 *Armeabi armeabi-v7a*ランタイムのみをサポートすることができますが、これは*armeabi*のみをサポートする古いデバイスがアプリを実行できないことを意味します。



## <a name="could-not-locate-android-sdk"></a>Android SDK が見つかりませんでした

Windows 用 Android SDK には、Google から2つのダウンロードが用意されています。
.Exe インストーラーを選択すると、インストールされている Xamarin. Android に通知するレジストリキーが書き込まれます。 .Zip ファイルを選択して自分で解凍した場合、Xamarin では、SDK を検索する場所がわかりません。 Visual Studio で SDK を使用するように Xamarin Android に指示するには、**ツール > オプション > xamarin > Android 設定**の順に移動します。

[![Xamarin Android 設定での Android SDK の場所](troubleshooting-images/01.png)](troubleshooting-images/01.png#lightbox)



## <a name="ide-does-not-display-target-device"></a>IDE にターゲットデバイスが表示されない

アプリケーションをデバイスに展開しようとしても、展開先のデバイスが [デバイスの選択] ダイアログボックスに表示されない場合があります。 これは、Android Debug Bridge が休暇時に行われると判断された場合に発生する可能性があります。

この問題を診断するには、 [adb プログラム](~/android/deploy-test/debugging/android-debug-log.md)を見つけて、次を実行します。

```shell
adb devices
```

デバイスが存在しない場合は、Android Debug Bridge サーバーを再起動して、デバイスが検出されるようにする必要があります。

```shell
adb kill-server
adb start-server
```

HTC 同期ソフトウェアによって **、adb の起動サーバー**が正常に動作しなくなる場合があります。 Adb の**開始サーバー**コマンドによって開始されているポートが印刷されない場合は、HTC 同期ソフトウェアを終了し、adb サーバーを再起動してください。


## <a name="the-specified-task-executable-keytool-could-not-be-run"></a>指定されたタスクの実行可能ファイル "keytool" を実行できませんでした

これは、Java SDK の bin ディレクトリが配置されているディレクトリがパスに含まれていないことを意味します。 [インストール](~/android/get-started/installation/index.md)ガイドの手順に従っていることを確認します。


## <a name="monodroidexe-or-aresgenexe-exited-with-code-1"></a>コード1で終了したモノ id .exe または aresgen

この問題をデバッグするには、Visual Studio に移動し、MSBuild の詳細レベルを変更します。これを行うには、次のように選択します。**ツール > オプション > プロジェクト**と**ソリューションをビルド**して **> 実行 >、MSBuild プロジェクトのビルド出力の詳細**度を設定し、この値を**Normal**に設定します。

リビルドし、Visual Studio の出力ウィンドウを確認します。このウィンドウには、完全なエラーが含まれている必要があります。

## <a name="there-is-not-enough-storage-space-on-the-device-to-deploy-the-package"></a>パッケージを展開するための十分な記憶領域がデバイスにありません

このエラーは、Visual Studio 内からエミュレーターを起動しない場合に発生します。 Visual Studio の外部でエミュレーターを起動する場合は、 `-partition-size 512`オプションを渡す必要があります。たとえば、

```shell
emulator -partition-size 512 -avd MonoDroid
```

シミュレーターを[構成するときに使用した名前](~/android/get-started/installation/windows.md#device)など、正しいシミュレーター名を使用していることを確認します。


## <a name="install_failed_invalid_apk-when-installing-a-package"></a>パッケージ\_を\_インストール\_するときに、インストールが無効な apk に失敗しました

Android パッケージ名にはピリオド (' *.* ') を含める*必要があり*ます。 パッケージ名を編集して、ピリオドが含まれるようにします。

- Visual Studio 内:
    - プロジェクトを右クリックし > プロパティ をクリックします。
    - 左側の [Android マニフェスト] タブをクリックします。
    - [パッケージ名] フィールドを更新します。
        - 「No androidmanifest」というメッセージ&ldquo;が表示された場合。 1つを追加する場合にクリックします。&rdquo;で、リンクをクリックし、[パッケージ名] フィールドを更新します。
- Visual Studio for Mac 内:
    - プロジェクト > オプションを右クリックします。
    - [ビルド/Android アプリケーション] セクションに移動します。
    - "." が含まれるように [パッケージ名] フィールドを変更します。




## <a name="install_failed_missing_shared_library-when-installing-a-package"></a>パッケージのインストール\_時\_に、\_見つからない共有ライブラリをインストールする\_

このコンテキストの "共有ライブラリ" は、ネイティブ共有ライブラリ (*libfoo.so*) ファイルでは*ありません*。代わりに、Google Maps など、ターゲットデバイスに個別にインストールする必要があるライブラリです。

Android パッケージでは、 `<uses-library/>`要素に必要な共有ライブラリを指定します。 *必要な*ライブラリがターゲットデバイスに存在しない場合 (既定で`//uses-library/@android:required`は*true*に設定されて *\_\_\_いる場合など)、パッケージのインストールは失敗します。\_ライブラリ*。

どの共有ライブラリが必要かを判断するには、*生成さ*
れた**androidmanifest .xml**ファイル (例: **obj\\Debug\\android\\androidmanifest .xml**) を表示し、 `<uses-library/>`要素。 `<uses-library/>`要素は、プロジェクトの**プロパティ\\と roidmanifest .xml**ファイルに手動で追加することも、[ユーザー属性のカスタム属性](xref:Android.App.UsesLibraryAttribute)を使用して追加することもできます。

たとえば、 *GoogleMaps*にアセンブリ参照を追加すると、Google Maps 共有ライブラリのが`<uses-library/>`暗黙的に追加されます。



## <a name="install_failed_update_incompatible-when-installing-a-package"></a>パッケージ\_を\_インストール\_するときにインストール失敗の更新プログラムに互換性がない

Android パッケージには、次の3つの要件があります。

- '. ' が含まれている必要があります。(前のエントリを参照)
- 一意の文字列パッケージ名を持つ必要があります (したがって、Chrome アプリの場合は、Android アプリ名に表示される逆 tld 規則です)。
- パッケージをアップグレードするときは、パッケージの署名キーが同じである必要があります。

そのため、次のシナリオを考えてみましょう。

1. アプリをビルド & デバッグアプリとしてデプロイする
2. 署名キーを変更した場合 (たとえば、をリリースアプリとして使用する場合) (または、既定で提供されるデバッグ署名キーが気に入らないため)
3. 最初にアプリを削除せずにインストールします。たとえば、Visual Studio 内でデバッグを行わずにデバッグ > 開始します。


この場合、署名キーの実行中にパッケージ名\_が\_変更\_されていなかったため、パッケージのインストールが失敗し、インストールエラーが発生します。 [Android デバッグログ](~/android/deploy-test/debugging/android-debug-log.md)には、次のようなメッセージも含まれます。

```shell
E/PackageManager(  146): Package [PackageName] signatures do not match the previously installed version; ignoring!
```

このエラーを解決するには、再インストールする前に、デバイスからアプリケーションを完全に削除します。


## <a name="install_failed_uid_changed-when-installing-a-package"></a>パッケージ\_の\_インストール\_時にインストールに失敗した UID が変更されました

Android パッケージをインストールすると、*ユーザー id* (UID) が割り当てられます。
現時点では、インストール済みのアプリをインストールすると、インストールが次の`INSTALL_FAILED_UID_CHANGED`ように失敗する場合があります。

```shell
ERROR [2015-03-23 11:19:01Z]: ANDROID: Deployment failed
Mono.AndroidTools.InstallFailedException: Failure [INSTALL_FAILED_UID_CHANGED]
   at Mono.AndroidTools.Internal.AdbOutputParsing.CheckInstallSuccess(String output, String packageName)
   at Mono.AndroidTools.AndroidDevice.<>c__DisplayClass2c.<InstallPackage>b__2b(Task`1 t)
   at System.Threading.Tasks.ContinuationTaskFromResultTask`1.InnerInvoke()
   at System.Threading.Tasks.Task.Execute()
```

この問題を回避するには、android ターゲットの GUI からアプリをインストールするか、を使用し`adb`て、android パッケージを完全にアンインストールします。

```shell
$ adb uninstall @PACKAGE_NAME@
```

**使用**しない。これにより、アプリケーションデータが保持されるため、競合する UID がターゲットデバイスに保持されます。 `adb uninstall -k`



## <a name="release-apps-fail-to-launch-on-device"></a>デバイスでリリースアプリを起動できない

Android デバッグログの出力には、次のようなメッセージが含まれます。

```shell
D/AndroidRuntime( 1710): Shutting down VM
W/dalvikvm( 1710): threadid=1: thread exiting with uncaught exception (group=0xb412f180)
E/AndroidRuntime( 1710): FATAL EXCEPTION: main
E/AndroidRuntime( 1710): java.lang.UnsatisfiedLinkError: Couldn't load monodroid: findLibrary returned null
E/AndroidRuntime( 1710):        at java.lang.Runtime.loadLibrary(Runtime.java:365)
```

その場合は、次の2つの原因が考えられます。

1. Apk には、ターゲットデバイスでサポートされている ABI が用意されていません。
    たとえば、apk には armeabi-armeabi-v7a バイナリのみが含まれ、ターゲットデバイスは armeabi のみをサポートします。

2. [Android のバグ](http://code.google.com/p/android/issues/detail?id=21670)。 このような場合は、アプリをアンインストールし、指を使用してアプリを再インストールします。

(1) を修正するには、プロジェクトのオプションとプロパティを編集し、[必要な ABI のサポートをサポートされている ABIs の一覧に追加](~/android/app-fundamentals/cpu-architectures.md)します。 どの ABI を追加する必要があるかを判断するには、ターゲットデバイスに対して次の adb コマンドを実行します。

```shell
adb shell getprop ro.product.cpu.abi
adb shell getprop ro.product.cpu.abi2
```

出力には、プライマリ (およびオプションのセカンダリ) ABIs が含まれます。

```shell
$ adb shell getprop | grep ro.product.cpu
[ro.product.cpu.abi2]: [armeabi]
[ro.product.cpu.abi]: [armeabi-v7a]
```

## <a name="the-outpath-property-is-not-set-for-project-ldquomyappcsprojrdquo"></a>プロジェクト&ldquo;MyApp の outpath プロパティが設定されていません&rdquo;

通常、これは HP コンピューターがあり、環境変数&ldquo;プラットフォーム&rdquo;が MCD や hpd などに設定されていることを意味します。 これは、 &ldquo;通常は Any CPU&rdquo;または&ldquo;x86&rdquo;に設定されている MSBuild プラットフォームプロパティと競合します。 MSBuild を機能させるには、この環境変数をコンピューターから削除する必要があります。

- コントロールパネル > システム > 高度な > 環境変数

Visual Studio または Visual Studio for Mac を再起動し、リビルドを試行します。 正常に機能するようになりました。

## <a name="javalangclasscastexception-monoandroidruntimejavaobject-cannot-be-cast-to"></a>ClassCastException: JavaObject をにキャストすることはできません...

Xamarin 2.x では、入れ子になったジェネリック型が適切にマーシャリングされていません。 たとえば、\# [simpleexpandablelistadapter](xref:Android.Widget.SimpleExpandableListAdapter)を使用する次の C コードについて考えてみます。


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


問題は、Xamarin. Android で入れ子になったジェネリック型が正しくマーシャリングされないことです。 `List<IDictionary<string, object>>` は [java.lang.ArrrayList](xref:Java.Util.ArrayList) にマーシャリングされていますが、`ArrayList` には、(`Dictionary<string, object>` インスタンスを参照する) `mono.android.runtime.JavaObject` インスタンスが含まれています。これは、[java.util.Map](xref:Java.Util.IMap) を実装するものではありません。その結果、次の例外が発生します。

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

この回避策は、 &ldquo;内部&rdquo;型の`System.Collections.Generic`型ではなく、提供された[Java コレクション型](~/android/internals/api-design.md)を使用することです。 これにより、インスタンスをマーシャリングするときに、適切な Java の種類が得られます。 (次のコードは、期間を短縮するために必要以上に複雑です。 `s/List/JavaList/g` また`s/Dictionary/JavaDictionary/g` 、を使用して元のコードを変更することもできます。

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

[これは、将来のリリースで修正される予定](https://bugzilla.xamarin.com/show_bug.cgi?id=5401)です。


## <a name="unexpected-nullreferenceexceptions"></a>予期しない NullReferenceExceptions

場合によっては、[Android Debug Log](~/android/deploy-test/debugging/android-debug-log.md) に、&ldquo;発生できない&rdquo; nullreferenceexceptionsや、アプリが停止する直前にMonoforAndroidのランタイムコードが表示されることがあります。

```shell
E/mono(15202): Unhandled Exception: System.NullReferenceException: Object reference not set to an instance of an object
E/mono(15202):   at Java.Lang.Object.GetObject (IntPtr handle, System.Type type, Boolean owned)
E/mono(15202):   at Java.Lang.Object._GetObject[IOnTouchListener] (IntPtr handle, Boolean owned)
E/mono(15202):   at Java.Lang.Object.GetObject[IOnTouchListener] (IntPtr handle, Boolean owned)
E/mono(15202):   at Android.Views.View+IOnTouchListenerAdapter.n_OnTouch_Landroid_view_View_Landroid_view_MotionEvent_(IntPtr jnienv, IntPtr native__this, IntPtr native_v, IntPtr native_e)
E/mono(15202):   at (wrapper dynamic-method) object:b039cbb0-15e9-4f47-87ce-442060701362 (intptr,intptr,intptr,intptr)
```

または

```shell
E/mono    ( 4176): Unhandled Exception:
E/mono    ( 4176): System.NullReferenceException: Object reference not set to an instance of an object
E/mono    ( 4176): at Android.Runtime.JNIEnv.NewString (string)
E/mono    ( 4176): at Android.Util.Log.Info (string,string)
```

これは、Android ランタイムがプロセスを中止することを決定した場合に発生する可能性があります。これは、ターゲットの JNI の制限に&ldquo;達し&rdquo;たり、で何らかの問題が発生したりするなど、さまざまな理由で発生する可能性があります。

この問題が発生しているかどうかを確認するには、Android デバッグログで、次のようなプロセスからのメッセージを確認します。

```shell
E/dalvikvm(  123): VM aborting
```


## <a name="abort-due-to-global-reference-exhaustion"></a>グローバル参照の枯渇による中止

Android ランタイムの JNI レイヤーでサポートされるのは、特定の時点で有効な JNI オブジェクト参照の数が限られている場合のみです。 この制限を超えると、何かが中断します。

グリーン (*global reference*) の制限は、エミュレーターでは2000の参照、ハードウェアでは ~ 52000 の参照です。

Android デバッグログに次のようなメッセージが表示されたときに、作成される GREFs が多すぎることがわかっています。

```shell
D/dalvikvm(  602): GREF has increased to 1801
```

上限に達した場合は、次のようなメッセージが出力されます。

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


上の例 (ちなみに、[バグ 685215](https://bugzilla.novell.com/show_bug.cgi?id=685215)から発生します) で問題は、作成されている Android... のインスタンスが多すぎることです。この特定のバグの修正の一覧については、[コメント\#2](https://bugzilla.novell.com/show_bug.cgi?id=685215#c2)を参照してください。

通常、便利な解決策は、割り当てら&ndash;れて&ndash;いるインスタンスの数が多すぎている型を検索し、それをソースコード内で作成し、それらを適切に破棄することです (Java オブジェクトの有効期間が短縮されます)。 これは常に適切で\#あるとは限りません (685215 はマルチスレッドであるため、単純なソリューションは Dispose 呼び出しを回避します) が、最初に考慮する必要があります。

作成した GREFs がどの程度存在するかを確認するには、その[ログ記録](~/android/troubleshooting/index.md)を有効にすることができます。


## <a name="abort-due-to-jni-type-mismatch"></a>JNI の型が一致しないため、中止します

JNI コードを手動でロールアウトする場合、型が正しく一致しない可能性があります。たとえば、を実装`java.lang.Runnable.run` `java.lang.Runnable`していない型でを呼び出そうとした場合などです。 これが発生すると、Android デバッグログに次のようなメッセージが表示されます。

```shell
W/dalvikvm( 123): JNI WARNING: can't call Ljava/Type;;.method on instance of Lanother/java/Type;
W/dalvikvm( 123):              in Lmono/java/lang/RunnableImplementor;.n_run:()V (CallVoidMethodA)
...
E/dalvikvm( 123): VM aborting
```

## <a name="dynamic-code-support"></a>動的コードのサポート

### <a name="dynamic-code-does-not-compile"></a>動的コードはコンパイルされません。

アプリケーションまたは\#ライブラリで C 動的を使用するには、プロジェクトに system.servicemodel、Microsoft の .dll、および Mono を追加する必要があります。

### <a name="in-release-build-missingmethodexception-occurs-for-dynamic-code-at-run-time"></a>リリースビルドでは、実行時に動的コードの MissingMethodException が発生します。

- おそらく、アプリケーションプロジェクトには、System. .dll、Microsoft. CSharp. .dll への参照が含まれていません。 これらのアセンブリが参照されていることを確認します。

    - 動的コードは常にコストがかかることに注意してください。 効率的なコードが必要な場合は、動的コードを使用しないことを検討してください。

- 最初のプレビューでは、各アセンブリの型がアプリケーションコードによって明示的に使用されていない限り、これらのアセンブリは除外されていました。 回避策については、次を参照してください。[http://lists.ximian.com/pipermail/mo...il/009798.html](http://lists.ximian.com/pipermail/monodroid/2012-April/009798.html)


## <a name="projects-built-with-aotllvm-crash-on-x86-devices"></a>、X86 デバイスで AOT + LLVM クラッシュでビルドされたプロジェクト

X86 ベースのデバイスで[AOT + LLVM](~/android/deploy-test/release-prep/index.md)でビルドされたアプリをデプロイすると、次のような例外エラーメッセージが表示される場合があります。

```shell
Assertion: should not be reached at /Users/.../external/mono/mono/mini/tramp-x86.c:124
Fatal signal 6 (SIGABRT), code -6 in tid 4051 (amarin.bug56111)
```

これは、 [56111](https://bugzilla.xamarin.com/show_bug.cgi?id=56111)で報告されている既知の問題です。 この回避策は、LLVM を無効にすることです。
