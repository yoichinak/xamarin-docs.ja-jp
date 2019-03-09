---
title: トラブルシューティングのヒント
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 56137ACA-4811-B312-6860-E16D0FA123F7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/15/2018
ms.openlocfilehash: b2f11bd09e1b1b3fd7af29a026229494a081ad11
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668557"
---
# <a name="troubleshooting-tips"></a>トラブルシューティングのヒント


## <a name="getting-diagnostic-information"></a>診断情報の取得

Xamarin.Android では、いくつかのバグを追跡するときに検索する場所がいくつかがあります。
不足している機能には次が含まれます。

1.  診断 MSBuild 出力します。
2.  デバイスのデプロイ ログ。
3.  Android のデバッグ ログ出力します。


<a name="Diagnostic_MSBuild_Output" />

## <a name="diagnostic-msbuild-output"></a>診断 MSBuild 出力

診断 MSBuild では、パッケージのビルドに関連する追加情報を含めることができ、一部のパッケージ展開情報を含めることができます。

Visual Studio 内で診断 MSBuild 出力を有効にするには:

1.  クリックして**ツール > オプション.**
2.  左側のツリー ビューで選択**プロジェクトおよびソリューション > ビルドおよび実行**
3.  右側のパネルでは、診断に MSBuild ビルド出力の詳細度ドロップダウンを設定します。
4.  **[OK]** をクリックします。
5.  パッケージから不要な要素を取り除き、再ビルドします。
6.  診断の出力は、出力パネル内で表示されます。


Mac、OS x: を Visual Studio 内で診断 MSBuild 出力を有効にするには

1.  クリックして**Visual Studio for Mac > の基本設定.**
2.  左側のツリー ビューで選択**プロジェクト > ビルド**
3.  右側のパネルでログ詳細度ドロップダウンに設定の診断
4.  **[OK]** をクリックします。
5.  Visual Studio for Mac を再起動します。
6.  パッケージから不要な要素を取り除き、再ビルドします。
7.  診断の出力は、エラー パッド内で参照できます (**ビュー > パッド > エラー** )、ビルド出力 ボタンをクリックしています。




## <a name="device-deployment-logs"></a>デバイスのデプロイ ログ

Visual Studio 内でデバイスのデプロイ ログを有効にします。

1.  **ツール > オプション.**>
2.  左側のツリー ビューで選択**Xamarin > Android の設定**
3.  右側のパネルで [X] を有効にする**拡張機能のデバッグ ログ (デスクトップに monodroid.log)** チェック ボックスをオンします。
4.  ログ メッセージは、デスクトップ上の monodroid.log ファイルに書き込まれます。


Visual Studio for Mac は、常にデバイスのデプロイ ログを書き込みます。 それらを見つけるには少し困難です。*AndroidUtils*すべて日 + など、展開が発生したときのログ ファイルが作成されます。**AndroidTools-2012-10-24_12-35-45.log**します。

-  Windows のログ ファイルが書き込まれます`%LOCALAPPDATA%\XamarinStudio-{VERSION}\Logs`します。
-  OS X 上にログ ファイルの書き込み`$HOME/Library/Logs/XamarinStudio-{VERSION}`します。




## <a name="android-debug-log-output"></a>Android のデバッグ ログ出力

Android に多数のメッセージを書き込み、 [Android デバッグ ログ](~/android/deploy-test/debugging/android-debug-log.md)します。
Xamarin.Android で Android のシステム プロパティを使用して、Android のデバッグ ログに関連するメッセージの生成を制御します。 Android のシステム プロパティを使用して設定できます、 *setprop*コマンド内で、 [Android Debug Bridge (adb)](https://developer.android.com/guide/developing/tools/adb.html):

```shell
adb shell setprop PROPERTY_NAME PROPERTY_VALUE
```

システムのプロパティ、プロセスの起動時に読み取られるため必要があります設定するか前に、アプリケーションが起動するか、システムのプロパティが変更された後に、アプリケーションを再起動する必要があります。



### <a name="xamarinandroid-system-properties"></a>Xamarin.Android のシステム プロパティ

Xamarin.Android には、次のシステム プロパティがサポートされています。

-   *debug.mono.debug*:これと同じ場合、空でない文字列`*mono-debug*`します。

-   *debug.mono.env*:パイプで区切られた ('*|*')、アプリケーションの起動時にエクスポートする環境変数の一覧*する前に*mono は初期化されています。 これにより、環境変数を設定するには、そのコントロールの mono ログ記録できます。

    - *注*:値があるため '*|*' に、区切られた値は、引用符で囲むのレベルをいる必要がありますとして、 \` *adb シェル*\`コマンド引用符のセットが削除されます。

    - *注*:Android のシステム プロパティの値は 92 文字以内にすることはできます。

    - 例:

            adb shell setprop debug.mono.env "'MONO_LOG_LEVEL=info|MONO_LOG_MASK=asm'"

-   *debug.mono.log*:コンマ区切り ('*、*')、Android のデバッグ ログに関連するメッセージを印刷する必要があるコンポーネントの一覧。 既定では、何も設定されます。 コンポーネントは次のとおりです。

    -   *すべて*:すべてのメッセージを出力します。
    -   *gc*:GC に関連するメッセージを印刷します。
    -   *gref*:(脆弱なグローバル) の参照の割り当てと解放メッセージを印刷します。
    -   *lref*:ローカル参照の割り当てと解放メッセージを印刷します。

    *注*: これらは*非常に*詳細。 本当にする必要がある場合を除きを有効にしないでください。

-   *debug.mono.trace*:設定できる、 [mono--トレース](http://docs.go-mono.com/?link=man%3amono(1))`=PROPERTY_VALUE`設定します。



## <a name="xamarinandroid-cannot-resolve-systemvaluetuple"></a>Xamarin.Android は System.ValueTuple を解決することはできません。

Visual Studio と互換性がないのため、このエラーが発生します。

- **Visual Studio 2017 Update 1** (バージョン 15.1 以前) は互換性のみ、 **System.ValueTuple NuGet 4.3.0** (またはそれ以前)。

- **Visual Studio 2017 Update 2** (バージョン 15.2 以降) と互換性のあるのみ、 **System.ValueTuple NuGet 4.3.1** (またはそれ以降)。

Visual Studio 2017 のインストールに対応する正しい System.ValueTuple NuGet を選択してください。


## <a name="gc-messages"></a>GC メッセージ

Gc を格納する値を debug.mono.log システム プロパティを設定して GC コンポーネントのメッセージを表示できます。

GC を実行し、でした、GC の作業量についての情報を提供するたびに、GC のメッセージが生成されます。

```shell
I/monodroid-gc(12331): GC cleanup summary: 81 objects tested - resurrecting 21.
```

設定によって生成されるタイミング情報などの追加の GC 情報、`MONO_LOG_LEVEL`環境変数を`debug`:

```shell
adb shell setprop debug.mono.env MONO_LOG_LEVEL=debug
```

これにより、(多くの) 追加 Mono などのメッセージの結果の 3 つが発生します。

```shell
D/Mono (15723): GC_BRIDGE num-objects 1 num_hash_entries 81226 sccs size 81223 init 0.00ms df1 285.36ms sort 38.56ms dfs2 50.04ms setup-cb 9.95ms free-data 106.54ms user-cb 20.12ms clenanup 0.05ms links 5523436/5523436/5523096/1 dfs passes 1104 6883/11046605
D/Mono (15723): GC_MINOR: (Nursery full) pause 2.01ms, total 287.45ms, bridge 225.60 promoted 0K major 325184K los 1816K
D/Mono ( 2073): GC_MAJOR: (user request) pause 2.17ms, total 2.47ms, bridge 28.77 major 576K/576K los 0K/16K
```

`GC_BRIDGE`メッセージ、`num-objects`はこのパスを考慮すると、ブリッジ オブジェクトの数と`num_hash_entries`ブリッジ コードのこの呼び出し中に処理されたオブジェクトの数です。

`GC_MINOR`と`GC_MAJOR`メッセージ、`total`世界が一時停止中の時間は、(スレッドが実行されていない)、中に`bridge`のブリッジ (これは、Java VM が扱う) コードの処理にかかった時間の量です。 世界が*いない*ブリッジ処理中に一時停止しています。

 *一般に*の値が大きいほど`num_hash_entries`、以上の時間を`bridge`コレクションになります、大きい方と、`total`費やされた時間の収集になります。



## <a name="global-reference-messages"></a>グローバル参照メッセージ

グローバル参照 loggig (GREF)、ログ記録を有効にする、 *debug.mono.log*システム プロパティを含める必要があります*gref*など。

```shell
adb shell setprop debug.mono.log gref
```

Xamarin.Android では、グローバルの Android のリファレンスを使用して、Java に提供する必要がある Java インスタンスの Java のメソッドを呼び出すときに、として Java インスタンスと、関連付けられているマネージ インスタンスの間のマッピングを提供します。

残念ながら、Android エミュレーターは、時に存在する 2000 グローバル参照のみを許可します。 ハードウェアでは、52000 グローバル参照の程度の高い制限があります。 下限値はそのため、エミュレーターでアプリケーションを実行するときに、問題が発生する*場所*インスタンスのソースからを非常に便利なことができます。

 *注*: グローバルの参照カウントは、Xamarin.Android の内部としない (およびことはできません) は、プロセスに読み込まれるその他のネイティブ ライブラリによって除外グローバル参照を含めます。 推定値としてグローバル参照カウントを使用します。

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

結果の 4 つのメッセージがあります。

-  グローバル参照の作成: で始まる行は *+ g +* 、し、作成するコード パスのスタック トレースを提供します。
-  グローバル参照破棄: で始まる行は *-g-* とグローバル参照の破棄コード パスのスタック トレースを行うことがあります。 GC は、gref の破棄、スタック トレースは提供されません。
-  グローバルの弱い参照の作成: で始まる行は *+ w +* します。
-  グローバルの弱い参照破棄: で始まる行は *-w-* します。


すべてのメッセージ、 *grefc*値の Xamarin.Android が作成されると、グローバルの参照数は、中、 *grefwc*値は Xamarin.Android が作成されたグローバルの弱い参照の数。 *処理*または*obj ハンドル*値が、JNI のハンドル値と後の文字には、' */*' はハンドルの値の型: */L*ローカル参照は、 */G*のグローバルの参照と */W*グローバルの弱い参照の。

GC プロセスの一環として、グローバル参照 (+ g +) がグローバルの弱い参照に変換されます (+ w + の原因と-g-)、Java 側の GC を開始すると、およびグローバルの弱い参照をチェックして、収集されたかどうか、します。 弱い参照を新しい gref が作成されたがまだ動作している場合 (g +、-w + -)、弱い参照が破棄されるそれ以外の場合 (-w)。

## <a name="java-instance-is-created-and-wrapped-by-a-mcw"></a>Java のインスタンスが作成され、MCW によってラップされました。

```shell
I/monodroid-gref(27679): +g+ grefc 2211 gwrefc 0 obj-handle 0x4066df10/L -> new-handle 0x4066df10/L from ...
I/monodroid-gref(27679): handle 0x4066df10; key_handle 0x4066df10: Java Type: `android/graphics/drawable/TransitionDrawable`; MCW type: `Android.Graphics.Drawables.TransitionDrawable`
```

## <a name="a-gc-is-being-performed"></a>GC が実行されているとしています.

```shell
I/monodroid-gref(27679): +w+ grefc 1953 gwrefc 259 obj-handle 0x4066df10/G -> new-handle 0xde68f95f/W from take_weak_global_ref_jni
I/monodroid-gref(27679): -g- grefc 1952 gwrefc 259 handle 0x4066df10/G from take_weak_global_ref_jni
```

## <a name="object-is-still-alive-as-handle--null"></a>オブジェクトがまだ有効で、ハンドルとして! = null
## <a name="wref-turned-back-into-a-gref"></a>wref、gref に再度有効に

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x4066df10
I/monodroid-gref(27679): +g+ grefc 1930 gwrefc 39 obj-handle 0xde68f95f/W -> new-handle 0x4066df10/G from take_global_ref_jni
I/monodroid-gref(27679): -w- grefc 1930 gwrefc 38 handle 0xde68f95f/W from take_global_ref_jni
```

## <a name="object-is-dead-as-handle--null"></a>オブジェクトが配信不能、ハンドルとして null を = =
## <a name="wref-is-freed-no-new-gref-created"></a>wref が解放された、新しい gref 作成

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x0
I/monodroid-gref(27679): -w- grefc 1914 gwrefc 296 handle 0xde68f95f/W from take_global_ref_jni
```

ここで「興味深い」欠点が 1 つがある: 4.0 より前の Android を実行しているターゲット、gref 値は、Android ランタイムのメモリ内での Java オブジェクトのアドレスと一致しません。 (つまり非移動、政治的、コレクターは、GC でし、それらのオブジェクトへの直接参照を渡そうとして、)。後にそのため、g + + w +、-g-、+ g +、-w シーケンス、結果として得られる gref gref の元の値と同じ値になります。 これにより、ログを grepping は非常に簡単です。

Android 4.0 では、ただし、移動、コレクターがし、不要になった Android ランタイムへの直接参照を VM オブジェクト。 そのため、後に、g + + w +、-g-、+ g +、-w シーケンス、gref 値*異なるものになります*。 場合は、オブジェクトには、複数の Gc が生き残る、インスタンスを実際にから割り当てられたかを判断するが難しく、いくつかの gref 値によって変わります。

### <a name="querying-programmatically"></a>プログラムでクエリを実行します。

クエリを実行して GREF と WREF の両方の数を照会することができます、`JniRuntime`オブジェクト。

`Java.Interop.JniRuntime.CurrentRuntime.GlobalReferenceCount` グローバル参照カウント

`Java.Interop.JniRuntime.CurrentRuntime.WeakGlobalReferenceCount` -弱い参照カウント



## <a name="android-debug-logs"></a>Android のデバッグ ログ

[Android デバッグ ログ](~/android/deploy-test/debugging/android-debug-log.md)すれば、ランタイム エラーに関する追加のコンテキストを提供することがあります。



## <a name="floating-point-performance-is-terrible"></a>浮動小数点のパフォーマンスは、恐ろしいです。

または、「アプリ 10 倍高速で実行、リリース ビルドよりも、デバッグ ビルド!」

Xamarin.Android は、複数のデバイスの Abi をサポートしています: *armeabi*、 *armeabi v7a*、および*x86*します。 内でデバイスの Abi を指定できます**プロジェクトのプロパティ > [アプリケーション] タブ > サポートされているアーキテクチャ**します。

デバッグ ビルドでは、すべての Abi と、対象デバイスの最速の ABI のためは使用 Android パッケージを使用します。

リリース ビルドには、プロジェクトのプロパティ タブで選択した Abi にはのみが含まれます。1 つ以上選択できます。

*armeabi* ABI、既定値は、広範なデバイスをサポートしています。 *ただし*armeabi とサポートしていないデバイスのマルチ CPU ハードウェアの浮動小数点、amont など。 その結果、armeabi リリース ランタイムを使用するアプリは、1 つのコアに関連付けられるし、論理的な float 実装を使用します。 これらの両方は、アプリのパフォーマンスがはるかに低速に投稿できます。

有効にすると、アプリは、適切な浮動小数点パフォーマンス (ゲームなど) を必要とする場合、 *armeabi v7a* ABI です。 のみをサポートすることも、 *armeabi v7a*ランタイム、こうすることが古いデバイスだけをサポートする*armeabi*アプリを実行することはできません。



## <a name="could-not-locate-android-sdk"></a>Android SDK が見つかりませんでした。

2 のダウンロードは Google の Android SDK の Windows から入手できます。
.Exe インストーラーを選択した場合は、インストール先を Xamarin.Android に指示するレジストリ キーを書き込みます。 .Zip ファイルを選択して解凍して、Xamarin.Android では、SDK を検索する場所を認識しません。 移動して Visual Studio で SDK を場所、Xamarin.Android を判断できます**ツール > オプション > Xamarin > Android 設定**:

[![Xamarin Android の設定で android SDK の場所](troubleshooting-images/01.png)](troubleshooting-images/01.png#lightbox)



## <a name="ide-does-not-display-target-device"></a>IDE でターゲット デバイスが表示されません。

場合があります、デバイスがデバイスの選択 ダイアログ ボックスに表示されていない展開するデバイスにアプリケーションを配置しようとするされます。 これは、Android Debug Bridge が休暇でを決定した場合に発生します。

この問題を診断するには、検索、 [adb プログラム](~/android/deploy-test/debugging/android-debug-log.md)を実行します。

```shell
adb devices
```

デバイスが存在しない場合は、デバイスを検出できるように、Android Debug Bridge サーバーを再起動する必要があります。

```shell
adb kill-server
adb start-server
```

HTC 同期ソフトウェアができない可能性があります**adb 開始サーバー**から正常に動作します。 場合、 **adb 開始サーバー**コマンドは、どのポートで開始していますを出力しない HTC 同期ソフトウェアを終了、adb サーバーを再起動してみてください。


## <a name="the-specified-task-executable-keytool-could-not-be-run"></a>指定したタスクの実行可能ファイル"keytool"を実行できませんでした。

これは、パスに Java SDK の bin ディレクトリが配置されているディレクトリが含まれていないことを意味します。 これらの手順を実行したことを確認、[インストール](~/android/get-started/installation/index.md)ガイド。


## <a name="monodroidexe-or-aresgenexe-exited-with-code-1"></a>monodroid.exe または aresgen.exe コード 1 で終了しました

この問題をデバッグし、Visual Studio にして、これを行うに、MSBuild の詳細度レベルを変更するには、次を選択します。**ツール > オプション > プロジェクト**と**ソリューション > ビルド**と**実行 > MSBuild プロジェクト ビルド出力の詳細**にこの値を設定および**標準**します。

再構築して、完全なエラーが含まれている Visual Studio の出力ウィンドウを確認します。

## <a name="there-is-not-enough-storage-space-on-the-device-to-deploy-the-package"></a>パッケージを展開するデバイスで十分な記憶域スペースはありません。

これには、Visual Studio 内からエミュレーターを起動しないときに発生します。 Visual Studio の外部でエミュレーターを開始するときに渡す必要があります、`-partition-size 512`オプションは、例。

```shell
emulator -partition-size 512 -avd MonoDroid
```

つまり、シミュレーターの正しい名前を使用することを確認[シミュレーターを構成するときに使用した名前](~/android/get-started/installation/windows.md#device)します。


## <a name="installfailedinvalidapk-when-installing-a-package"></a>インストール\_FAILED\_無効な\_APK パッケージのインストール時

Android パッケージ名*する必要があります*にピリオド ('*.*')。 パッケージ名を編集するは、ピリオドが含まれているようにします。

-   Visual studio:
    -   プロジェクトを右クリックして > のプロパティ
    -   左側の [Android マニフェスト] タブをクリックします。
    -   パッケージ名 フィールドを更新します。
        -   メッセージが表示された場合&ldquo;いいえ AndroidManifest.xml が見つかりません。 1 つ追加する をクリックします。&rdquo;リンクをクリックし、パッケージ名 フィールドを更新します。
-   Visual Studio for Mac: 内
    -   プロジェクトを右クリックして > オプション。
    -   ビルドに移動/Android アプリケーション セクション。
    -   変更を格納するパッケージ名 フィールドを '.'。




## <a name="installfailedmissingsharedlibrary-when-installing-a-package"></a>インストール\_FAILED\_MISSING\_SHARED\_パッケージをインストールするときにライブラリ

このコンテキストで「共有ライブラリ」は*いない*ネイティブの共有ライブラリ (*libfoo.so*) ファイルです。 Google Maps など、ターゲット デバイスに個別にインストールする必要があるライブラリではなく。

Android パッケージを指定する共有ライブラリを必要と、`<uses-library/>`要素。 場合、*必要*ライブラリは、ターゲット デバイスに存在しません (例:`//uses-library/@android:required`は*true*、既定値)、パッケージのインストールが失敗*インストール\_失敗\_MISSING\_SHARED\_ライブラリ*します。

共有ライブラリが必要なことを確認するには*生成*
**AndroidManifest.xml**ファイル (例: **obj\\デバッグ\\android\\AndroidManifest.xml**) を探して、`<uses-library/>`要素。 `<uses-library/>` プロジェクトの要素を手動で追加できる**プロパティ\\AndroidManifest.xml**ファイルおよびを使用して、 [UsesLibraryAttribute カスタム属性](https://developer.xamarin.com/api/type/Android.App.UsesLibraryAttribute/)します。

アセンブリへの参照の追加など、 *Mono.Android.GoogleMaps.dll*暗黙的に追加されます、 `<uses-library/>` Google Maps 共有ライブラリ。



## <a name="installfailedupdateincompatible-when-installing-a-package"></a>インストール\_FAILED\_UPDATE\_パッケージをインストールするときに互換性がありません

Android パッケージには、次の 3 つの要件があります。

-   含める必要がありますが、'.'(前のエントリを参照してください)
-   一意の文字列のパッケージ名がある必要があります (そのため、Chrome アプリの com.android.chrome など、Android のアプリ名に表示されるリバース tld 規則)
-   パッケージのアップグレード パッケージに同じ署名キーが必要です。

したがって、このシナリオを考えてみましょう。

1.  ビルドし、デバッグ アプリとしてアプリをデプロイします。
2.  署名のキーをなどを変更する (またはに満足できないために、既定のデバッグ署名キー)、アプリをリリースとして使用
3.  削除することがなく、アプリをインストールする、デバッグなど、> デバッグなしで開始 Visual Studio 内


インストール パッケージのインストールは失敗この場合、\_FAILED\_更新\_互換性のないエラーは、パッケージ名は、署名中に変更していないため、キーがでした。 [Android デバッグ ログ](~/android/deploy-test/debugging/android-debug-log.md)のようなメッセージにも含まれます。

```shell
E/PackageManager(  146): Package [PackageName] signatures do not match the previously installed version; ignoring!
```

このエラーを解決するには、完全に削除、アプリケーション、デバイスから再インストールする前に。


## <a name="installfaileduidchanged-when-installing-a-package"></a>インストール\_FAILED\_UID\_パッケージをインストールするときに変更

Android のパッケージがインストールされているときに割り当てられます、*ユーザー id* (UID)。
*場合によって*で現在不明な理由により、既にインストールされているアプリの上にインストールするときに、インストールは失敗します`INSTALL_FAILED_UID_CHANGED`:

```shell
ERROR [2015-03-23 11:19:01Z]: ANDROID: Deployment failed
Mono.AndroidTools.InstallFailedException: Failure [INSTALL_FAILED_UID_CHANGED]
   at Mono.AndroidTools.Internal.AdbOutputParsing.CheckInstallSuccess(String output, String packageName)
   at Mono.AndroidTools.AndroidDevice.<>c__DisplayClass2c.<InstallPackage>b__2b(Task`1 t)
   at System.Threading.Tasks.ContinuationTaskFromResultTask`1.InnerInvoke()
   at System.Threading.Tasks.Task.Execute()
```

この問題を回避する*を完全にアンインストール*Android ターゲットの GUI からアプリをインストールするかを使用して、Android パッケージ`adb`:

```shell
$ adb uninstall @PACKAGE_NAME@
```

**使用しないでください**`adb uninstall -k`されますこの*保持*アプリケーションのデータをそのため、ターゲット デバイスに競合する UID を維持します。



## <a name="release-apps-fail-to-launch-on-device"></a>リリースのアプリケーションはデバイスの起動に失敗します。

Android のデバッグ ログの出力ではのようなメッセージが含まれます。

```shell
D/AndroidRuntime( 1710): Shutting down VM
W/dalvikvm( 1710): threadid=1: thread exiting with uncaught exception (group=0xb412f180)
E/AndroidRuntime( 1710): FATAL EXCEPTION: main
E/AndroidRuntime( 1710): java.lang.UnsatisfiedLinkError: Couldn't load monodroid: findLibrary returned null
E/AndroidRuntime( 1710):        at java.lang.Runtime.loadLibrary(Runtime.java:365)
```

そうである場合は、この 2 つの考えられる原因があります。

1.  .Apk では、ターゲット デバイスをサポートする ABI を提供していません。
    たとえば、.apk では、armeabi v7a のバイナリのみが含まれ、ターゲット デバイスは armeabi のみをサポートします。

2.  [Android バグ](http://code.google.com/p/android/issues/detail?id=21670)します。 大文字と小文字の場合は、アプリをアンインストール、指でのクロスおよびアプリを再インストールします。

(1) を解決するには、プロジェクトのオプション/プロパティを編集し、 [Abi のサポートの一覧に必要な ABI のサポートを追加](~/android/app-fundamentals/cpu-architectures.md)します。 を追加する必要があるどの ABI を判断するには、ターゲット デバイスに対して次の adb コマンドを実行します。

```shell
adb shell getprop ro.product.cpu.abi
adb shell getprop ro.product.cpu.abi2
```

出力には、プライマリ (および省略可能なセカンダリ) Abi です。

```shell
$ adb shell getprop | grep ro.product.cpu
[ro.product.cpu.abi2]: [armeabi]
[ro.product.cpu.abi]: [armeabi-v7a]
```

## <a name="the-outpath-property-is-not-set-for-project-ldquomyappcsprojrdquo"></a>プロジェクトの OutPath プロパティが設定されていない&ldquo;MyApp.csproj&rdquo;

通常は、HP、コンピューターと、環境変数がある&ldquo;プラットフォーム&rdquo;MCD または HPD のようなものに設定されています。 一般的に設定されているプラットフォームの MSBuild プロパティと競合&ldquo;Any CPU&rdquo;または&ldquo;x86&rdquo;します。 MSBuild が機能する前に、自分のコンピューターからこの環境変数を削除する必要があります。

-   コントロール パネル > システム > 詳細 > 環境変数

Mac の Visual Studio または Visual Studio を再起動しを再構築します。 これで、モ ノが期待どおりに機能します。

## <a name="javalangclasscastexception-monoandroidruntimejavaobject-cannot-be-cast-to"></a>中: mono.android.runtime.JavaObject にキャストできません.

Xamarin.Android 4.x は入れ子になったジェネリック型を正しくマーシャ リング適切です。 たとえば、次の C\#を使用してコード[SimpleExpandableListAdapter](https://developer.xamarin.com/api/type/Android.Widget.SimpleExpandableListAdapter/):


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


問題は、入れ子になったジェネリック型に、Xamarin.Android から正しくマーシャ リングすることです。 `List<IDictionary<string, object>>`にマーシャ リングされる、 [java.lang.ArrrayList](https://developer.xamarin.com/api/type/Java.Util.ArrayList/)が、`ArrayList`を含む`mono.android.runtime.JavaObject`インスタンス (を参照、`Dictionary<string, object>`インスタンス)を実装するものではなく[java.util.Map](https://developer.xamarin.com/api/type/Java.Util.IMap/)、次の例外の結果として得られる。

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

回避策は、指定された使用[Java コレクション型](~/android/internals/api-design.md)の代わりに、`System.Collections.Generic`型、&ldquo;内部&rdquo;型。 インスタンスをマーシャ リングする場合、適切な Java 型、これがします。 (Gref の有効期間を減らすために必要なより複雑なは、次のコードです。 使用して、元のコードの変更を簡略化できます`s/List/JavaList/g`と`s/Dictionary/JavaDictionary/g`場合 gref の有効期間が、心配はありません)。

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

[これは、将来のリリースで修正](https://bugzilla.xamarin.com/show_bug.cgi?id=5401)します。


## <a name="unexpected-nullreferenceexceptions"></a>予期しない NullReferenceExceptions

場合によっては、 [Android デバッグ ログ](~/android/deploy-test/debugging/android-debug-log.md)に比べて、NullReferenceExceptions は言うまでもを&ldquo;発生することはできません、&rdquo;またはアプリがなくなるの少し前に Android ランタイム コードの Mono に由来します。

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

これは、Android ランタイムがさまざまな理由から、ターゲットの GREF 制限に達した、何かなどに発生することが、プロセスを中止するときに発生することができます&ldquo;間違った&rdquo;JNI の使用。

このかどうかを参照するには、ようなプロセスからのメッセージを Android デバッグ ログを確認します。

```shell
E/dalvikvm(  123): VM aborting
```


## <a name="abort-due-to-global-reference-exhaustion"></a>グローバル参照の枯渇による中止します。

Android ランタイムの JNI のレイヤーには、時間の任意の時点で有効にする JNI オブジェクト参照の数に制限がのみサポートされます。 この制限を超えると、何かが中断します。

GREF (*グローバル参照*) の制限は、エミュレーターで 2000 参照とハードウェアに対する ~ 52000 を参照します。

Android のデバッグ ログにこのメッセージが表示されたらが多すぎる GREFs の作成を開始して理解しておく。

```shell
D/dalvikvm(  602): GREF has increased to 1801
```

GREF 制限に達すると、次などのメッセージが出力されます。

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


上記の例では (ちなみに、由来、 [685215 バグ](https://bugzilla.novell.com/show_bug.cgi?id=685215))、問題が多すぎる Android.Graphics.Point のインスタンスが作成されていることですを参照してください[コメント\#2](https://bugzilla.novell.com/show_bug.cgi?id=685215#c2)の修正プログラムの一覧については。この特定のバグです。

種類が多数のインスタンスを検索する便利なソリューションは、通常、割り当てられた&ndash;上記のダンプに Android.Graphics.Point&ndash;し、検索、作成された、ソース コードとその dispose で適切に (ように、Java-object 有効期間の短縮)。 これは常に適切な (\#685215 はマルチ スレッド、単純なソリューションが Dispose の呼び出しを回避できます) が、最初に検討してください。

有効にできます[GREF ログ](~/android/troubleshooting/index.md)GREFs が作成されたときと数の存在を確認します。


## <a name="abort-due-to-jni-type-mismatch"></a>JNI の型が一致しないのために中止します。

キューブを手動ロール JNI コードとして設定すると場合、型が一致しませんなど、正しくこと可能性がありますを呼び出すしようとすると`java.lang.Runnable.run`を実装していない型で`java.lang.Runnable`します。 このような場合がありますメッセージ Android デバッグ ログに次のような。

```shell
W/dalvikvm( 123): JNI WARNING: can't call Ljava/Type;;.method on instance of Lanother/java/Type;
W/dalvikvm( 123):              in Lmono/java/lang/RunnableImplementor;.n_run:()V (CallVoidMethodA)
...
E/dalvikvm( 123): VM aborting
```

## <a name="dynamic-code-support"></a>動的なコードのサポート

### <a name="dynamic-code-does-not-compile"></a>動的なコードはコンパイルされません。

C を使用する\#System.Core.dll、Microsoft.CSharp.dll および Mono.CSharp.dll をプロジェクトに追加する必要が、アプリケーションまたはライブラリに動的にします。

### <a name="in-release-build-missingmethodexception-occurs-for-dynamic-code-at-run-time"></a>リリースのビルドで MissingMethodException が動的なコードの実行時に発生します。

-   アプリケーション プロジェクトには System.Core.dll、Microsoft.CSharp.dll または Mono.CSharp.dll への参照がない可能性があります。 これらのアセンブリが参照されていることを確認します。

    -   常に注意してください動的コード コスト。 効率的なコードを必要がある場合は、動的なコードを使用していないことを検討します。

-   最初のプレビュー段階では、各アセンブリ内の型は、アプリケーション コードによって明示的に使用されていない限り、これらのアセンブリは除外されました。 回避策については、次を参照してください。 [http://lists.ximian.com/pipermail/mo...il/009798.html](http://lists.ximian.com/pipermail/monodroid/2012-April/009798.html)


## <a name="projects-built-with-aotllvm-crash-on-x86-devices"></a>AOT と LLVM クラッシュで x86 上でビルドされたプロジェクトのデバイス

ビルドされたアプリをデプロイするときに[AOT と LLVM](~/android/deploy-test/release-prep/index.md) x86 ベースのデバイスで、次のような例外エラー メッセージが表示することがあります。

```shell
Assertion: should not be reached at /Users/.../external/mono/mono/mini/tramp-x86.c:124
Fatal signal 6 (SIGABRT), code -6 in tid 4051 (amarin.bug56111)
```

これで報告される既知の問題は、 [56111](https://bugzilla.xamarin.com/show_bug.cgi?id=56111)します。 回避策では、LLVM を無効にします。
