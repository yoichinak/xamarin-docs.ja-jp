---
title: トラブルシューティングのヒント
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 56137ACA-4811-B312-6860-E16D0FA123F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/15/2018
ms.openlocfilehash: 961f9f38687790343f225d95c74e00e98f594c28
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30773866"
---
# <a name="troubleshooting-tips"></a>トラブルシューティングのヒント


## <a name="getting-diagnostic-information"></a>診断情報を取得します。

Xamarin.Android には、さまざまなバグを追跡するときに検索するいくつかの場所があります。
次の設定があります。

1.  MSBuild の診断出力します。
2.  デバイスの展開を記録します。
3.  Android のデバッグ ログ出力します。


<a name="Diagnostic_MSBuild_Output" />

## <a name="diagnostic-msbuild-output"></a>MSBuild の診断の出力

診断の MSBuild では、パッケージのビルドに関連する追加情報を含めることができ、一部のパッケージ展開情報を含めることがあります。

Visual Studio 内で診断 MSBuild 出力を有効にするには:

1.  をクリックして**ツール > オプション.**
2.  左側のツリー ビューで選択**プロジェクトおよびソリューション > ビルドおよび実行します。**
3.  右側のパネルでは、診断に MSBuild のビルド出力詳細度のドロップダウン リストを設定します。
4.  **[OK]** をクリックします。
5.  パッケージから不要な要素を取り除き、再ビルドします。
6.  診断の出力は、[出力] パネルに表示されます。


/Mac OS x: を Visual Studio 内での診断の MSBuild 出力を有効にするには

1.  をクリックして**Visual Studio for Mac > 設定しています.**
2.  左側のツリー ビューで選択**プロジェクト > ビルド**
3.  右側のパネルでは、ログの詳細レベル ドロップダウンに設定の診断
4.  **[OK]** をクリックします。
5.  Visual Studio for Mac を再起動します。
6.  パッケージから不要な要素を取り除き、再ビルドします。
7.  診断出力が、エラー パッド内で表示されます (**ビュー > パッド > エラー** )、ビルド出力をクリックします。




## <a name="device-deployment-logs"></a>展開ログのデバイス

Visual Studio 内で、デバイスの展開のログ記録を有効にします。

1.  **ツール > オプション.**>
2.  左側のツリー ビューで選択**Xamarin > Android の設定**
3.  右側のパネルで [X] を有効にする **(monodroid.log をデスクトップに書き込み) 拡張機能のデバッグ ログ**チェック ボックスをオンします。
4.  ログ メッセージは、デスクトップ上の monodroid.log ファイルに書き込まれます。


Visual Studio for Mac は、常にデバイスの展開のログを書き込みます。 検索することは、多少困難です。*AndroidUtils*日 + 時間が、展開、たとえば、すべてのログ ファイルが作成された: **AndroidTools-2012-10-24_12-35-45.log**です。

-  Windows では、ログ ファイルが書き込まれます`%LOCALAPPDATA%\XamarinStudio-{VERSION}\Logs`です。
-  OS X 上にログ ファイルの書き込み`$HOME/Library/Logs/XamarinStudio-{VERSION}`です。




## <a name="android-debug-log-output"></a>Android のデバッグ ログ出力

Android に多くのメッセージの書き込みは、 [Android のデバッグ ログ](~/android/deploy-test/debugging/android-debug-log.md)です。
Xamarin.Android では、Android のシステムのプロパティを使用して、Android のデバッグ ログに関連するメッセージの生成を制御します。 Android のシステムのプロパティを設定できます、 *setprop*コマンド内で、 [Android Debug Bridge (adb)](http://developer.android.com/guide/developing/tools/adb.html):

```shell
adb shell setprop PROPERTY_NAME PROPERTY_VALUE
```

システムのプロパティは、プロセスの起動中に読み取られ、ために必要がある設定するか、アプリケーションが起動するか、システムのプロパティが変更された後に、アプリケーションを再起動する必要があります。



### <a name="xamarinandroid-system-properties"></a>Xamarin.Android のシステム プロパティ

Xamarin.Android には、次のシステム プロパティがサポートされています。

-   *debug.mono.debug*: これと同じ場合は、空でない文字列`*mono-debug*`です。

-   *debug.mono.env*: パイプで区切られた ('*|*') のアプリケーションの起動中にエクスポートする環境変数の一覧*する前に*モノラルは初期化されています。 これにより、環境変数を設定するには、そのコントロールの mono ログ記録できます。

    - *注*: 値なので '*|*'-、区切られた値が必要、引用符で囲むのレベルとして、 \` *adb シェル*\`コマンドが削除されます、引用符のセット。

    - *注*: Android のシステム プロパティの値を 92 文字の長さを超えることができます。

    - 例:

            adb shell setprop debug.mono.env "'MONO_LOG_LEVEL=info|MONO_LOG_MASK=asm'"

-   *debug.mono.log*: コンマ区切り ('*、*')、Android のデバッグ ログに関連するメッセージを出力するコンポーネントの一覧です。 既定では、何も設定します。 コンポーネントは次のとおりです。

    -   *すべて*: すべてのメッセージを印刷します。
    -   *gc*: 印刷 GC に関連するメッセージ。
    -   *gref*: (弱い、グローバル) の参照の割り当てと解放メッセージを出力します。
    -   *lref*: ローカル参照の割り当てと解放メッセージを出力します。

    *注*: これらは*非常に高く*詳細です。 必要な場合を除きを有効にしないでください。

-   *debug.mono.trace*: に設定できる、[モノラル--トレース](http://docs.go-mono.com/?link=man%3amono(1))`=PROPERTY_VALUE`設定します。



## <a name="xamarinandroid-cannot-resolve-systemvaluetuple"></a>Xamarin.Android System.ValueTuple を解決できません。

このエラーは、Visual Studio と互換性がないのために発生します。

- **Visual Studio 2017 Update 1** (15.1 またはそれ以前のバージョン) とのみ互換性が、 **System.ValueTuple NuGet 4.3.0** (またはそれ以前)。

- **Visual Studio 2017 Update 2** (15.2 またはそれ以降のバージョン) とのみ互換性が、 **System.ValueTuple NuGet 4.3.1** (またはそれ以降)。

Visual Studio 2017 のインストールに対応する正しい System.ValueTuple NuGet を選択してください。


## <a name="gc-messages"></a>GC メッセージ

GC コンポーネントのメッセージは、gc を表す値に debug.mono.log システム プロパティを設定して表示できます。

GC を実行し、でした GC の作業量についての情報を提供するたびに、GC メッセージが生成されます。

```shell
I/monodroid-gc(12331): GC cleanup summary: 81 objects tested - resurrecting 21.
```

タイミング情報などの追加の GC 情報は、設定によって生成されることができます、`MONO_LOG_LEVEL`環境変数を`debug`:

```shell
adb shell setprop debug.mono.env MONO_LOG_LEVEL=debug
```

これにより、(多くの) 追加モノラルなど、メッセージの結果としてのこれらの 3 つが発生します。

```shell
D/Mono (15723): GC_BRIDGE num-objects 1 num_hash_entries 81226 sccs size 81223 init 0.00ms df1 285.36ms sort 38.56ms dfs2 50.04ms setup-cb 9.95ms free-data 106.54ms user-cb 20.12ms clenanup 0.05ms links 5523436/5523436/5523096/1 dfs passes 1104 6883/11046605
D/Mono (15723): GC_MINOR: (Nursery full) pause 2.01ms, total 287.45ms, bridge 225.60 promoted 0K major 325184K los 1816K
D/Mono ( 2073): GC_MAJOR: (user request) pause 2.17ms, total 2.47ms, bridge 28.77 major 576K/576K los 0K/16K
```

`GC_BRIDGE`メッセージ、`num-objects`は、このパスを検討して、ブリッジのオブジェクトの数と`num_hash_entries`ブリッジ コードのこの呼び出し中に処理されたオブジェクトの数です。

`GC_MINOR`と`GC_MAJOR`メッセージ、`total`世界が一時停止中には、時間 (スレッドがない)、中に`bridge`ブリッジ コード (Java VM が扱う) の処理にかかった時間の長さです。 世界*いない*ブリッジ処理が行われるときに一時停止します。

 *一般に*の値が大きいほど`num_hash_entries`では、時間を`bridge`コレクションになりますも大きく、`total`費やされた時間の収集になります。



## <a name="global-reference-messages"></a>グローバル参照メッセージ

グローバル参照 loggig (GREF)、ログ記録を有効にする、 *debug.mono.log*システム プロパティを含める必要があります*gref*など。

```shell
adb shell setprop debug.mono.log gref
```

Xamarin.Android では、Android のグローバル参照を使用して、Java に提供する Java インスタンスが必要な Java メソッドを呼び出すときに、として Java インスタンスとの関連付けられているマネージ インスタンスの間のマッピングを提供します。

残念ながら、Android エミュレーターは、時に存在する 2000 グローバル参照のみを許可します。 ハードウェアがある程度制限値を大きく 52000 グローバル参照します。 下限は、エミュレーターを理解することでアプリケーションを実行すると、問題が発生する*場所*インスタンスの提供元が非常に役に立ちます。

 *注*: グローバルの参照カウント Xamarin.Android、内部およびしません (できません) 参照を含めるグローバル プロセスに読み込まれるその他のネイティブ ライブラリで実行します。 推定値として、グローバルの参照カウントを使用します。

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

-  グローバル参照の作成: で始まる行は、これら *+ g +* 、スタック トレースを作成するコード パスを提供するとします。
-  グローバル参照破棄: で始まる行は、これら *-g-* とグローバルの参照を廃棄コード パスのスタック トレースを行うことがあります。 GC は、gref の破棄、スタック トレースは提供されません。
-  グローバルは弱参照の作成: で始まる行は、これら *+ w +* です。
-  グローバルは弱参照破棄: で始まる行は、これら *-w-* です。


すべてのメッセージで、 *grefc*値 Xamarin.Android が作成すると、グローバルの参照の数は、中、 *grefwc*値は Xamarin.Android が作成されたグローバルの弱参照の数。 *処理*または*obj ハンドル*値が、JNI ハンドル値、および後の文字には、' */*' ハンドルの値の型です: */L*ローカル参照に、 */G*のグローバルの参照と */W*グローバルの弱い参照をします。

GC プロセスの一環としては、グローバルの弱い参照にグローバル参照 (+ g +) を変換します。 (a + w + の原因と-g-)、Java 側 GC が開始されて、およびグローバルの弱い参照をチェックして、収集された場合、します。 弱い参照を新しい gref が作成されたまだ有効である場合 (+ g +、-w-)、それ以外の場合、脆弱な ref が破棄される (-w)。

## <a name="java-instance-is-created-and-wrapped-by-a-mcw"></a>Java インスタンスが作成され、MCW によってラップされました。

```shell
I/monodroid-gref(27679): +g+ grefc 2211 gwrefc 0 obj-handle 0x4066df10/L -> new-handle 0x4066df10/L from ...
I/monodroid-gref(27679): handle 0x4066df10; key_handle 0x4066df10: Java Type: `android/graphics/drawable/TransitionDrawable`; MCW type: `Android.Graphics.Drawables.TransitionDrawable`
```

## <a name="a-gc-is-being-performed"></a>GC が実行される.

```shell
I/monodroid-gref(27679): +w+ grefc 1953 gwrefc 259 obj-handle 0x4066df10/G -> new-handle 0xde68f95f/W from take_weak_global_ref_jni
I/monodroid-gref(27679): -g- grefc 1952 gwrefc 259 handle 0x4066df10/G from take_weak_global_ref_jni
```

## <a name="object-is-still-alive-as-handle--null"></a>オブジェクトがまだ有効で、ハンドルとして! = null
## <a name="wref-turned-back-into-a-gref"></a>wref オン、gref に戻す

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x4066df10
I/monodroid-gref(27679): +g+ grefc 1930 gwrefc 39 obj-handle 0xde68f95f/W -> new-handle 0x4066df10/G from take_global_ref_jni
I/monodroid-gref(27679): -w- grefc 1930 gwrefc 38 handle 0xde68f95f/W from take_global_ref_jni
```

## <a name="object-is-dead-as-handle--null"></a>オブジェクトはハンドルとして、配信不能 = = null
## <a name="wref-is-freed-no-new-gref-created"></a>wref が作成された解放された、新しい gref です。

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x0
I/monodroid-gref(27679): -w- grefc 1914 gwrefc 296 handle 0xde68f95f/W from take_global_ref_jni
```

ここで「興味深い」欠点が 1 つがある。 4.0 より前の Android を実行しているターゲット、gref 値は、Android のランタイムのメモリ内の Java オブジェクトのアドレスと同じです。 (つまり、非移動、保守的なコレクターは、GC ではされ、それらのオブジェクトへの直接参照を渡そうとして、) されます。したがって後、g + + w +、-g-、+ g +、-w シーケンス、結果として得られる gref は元の gref 値として同じ値になります。 これにより、ログを grepping は非常に簡単です。

ただし、android 4.0 は、移動コレクターを持つし、VM オブジェクトに Android ランタイムへの直接参照を不要になった渡します。 そのため後、g + + w +、-g-、+ g +、-w シーケンス、gref 値*異なります*です。 オブジェクトには、複数の Gc が回収されなかった場合、は、いくつかの gref 値、インスタンスを実際にから割り当てられたを決定するが困難で移動します。

### <a name="querying-programmatically"></a>プログラムでクエリを実行します。

クエリを実行して GREF と WREF の両方の数を照会することができます、`JniRuntime`オブジェクト。

`Java.Interop.JniRuntime.CurrentRuntime.GlobalReferenceCount` グローバル参照カウント

`Java.Interop.JniRuntime.CurrentRuntime.WeakGlobalReferenceCount` -弱い参照カウント



## <a name="offline-activation"></a>オフラインのライセンス認証

Windows では、Xamarin.Android をアクティブにできませんまたは Mac OS X で Xamarin.Android の完全なバージョンをインストールできない場合は、参照してください、[オフラインのライセンス認証](~/android/get-started/installation/index.md)ページ。



## <a name="cant-upgrade-to-indiebusiness-from-trial-account"></a>試用版アカウントから Indie/ビジネスへのアップグレードことはできません。

最近 Xamarin.Android を購入し、既に Xamarin.Android 試用版を開始すると場合、は、Mac または Visual Studio の Visual Studio によってピックアップこのライセンスの変更を取得する次の手順を完了する必要があります。

-  Mac または Visual Studio の Visual Studio を閉じます
-  Mac 上の ~/Library/MonoAndroid または %PROGRAMDATA%\Mono から Android\License\ for Windows のすべてのファイルを削除します。
-  再 Mac または Visual Studio の Visual Studio を開き、Xamarin.Android プロジェクトのビルド


これは、はず準備して実行します。 問題が解決しない場合はしようとする可能性があります、[オフラインのライセンス認証](~/android/get-started/installation/index.md)ワークステーションのアクティブ化を完了します。



## <a name="receiving-activation-incomplete-error-message"></a>受信 ' アクティベーション不完全なエラー メッセージ

この問題は、Visual Studio を Xamarin.Android を使用する場合に発生する可能性があります。 この問題を解決するには、次の場所からしてください。 ログを送信 *contact@xamarin.com*です。

-  ログの場所: **%localappdata%\\Xamarin\\ログ**




## <a name="receiving-error-retrieving-update-information-error-message"></a>'の更新情報の取得エラー' のエラー メッセージ

時、更新プログラムは更新プログラムをチェックする場合に発生する多くの場合はこの次のエラーで失敗します。

時間の大部分は、Xamarin アカウントからログ記録するだけでこのエラーを解決することができ、ログのバックアップを作成し、します。

これを行うには、下の任意のプラットフォームを見つけるし、手順を実行してください。

**Mac の場合:**
1. Mac 用 Visual Studio を開きます
2. Mac 用 Visual Studio を選択 > アカウント.
3. [ログアウト] をクリックします。
4. [ログ] をクリックします。
5. 資格情報を入力します。
6. 更新プログラムの確認

**PC で Visual Studo の使用。**
1. Visual Studio を開く
2. ツールの選択 > Xamarin アカウント
3. [ログアウト] をクリックします。
4. [ログ] をクリックします。
5. 資格情報を入力します。
6. 更新プログラムの確認

このエラー メッセージが引き続き表示される場合は、電子メール **contact@xamarin.com**です。




## <a name="android-debug-logs"></a>Android のデバッグ ログ

[Android のデバッグ ログ](~/android/deploy-test/debugging/android-debug-log.md)が表示されるすべてのランタイム エラーに関する追加のコンテキストを提供する場合があります。



## <a name="floating-point-performance-is-terrible"></a>浮動小数点数のパフォーマンスが大きな引っかき!

また、"マイ アプリの実行 10 倍速くよりリリース ビルドでのデバッグ ビルドで!"

Xamarin.Android ABIs 複数のデバイスをサポートしています: *armeabi*、 *armeabi v7a*、および*x86*です。 内に設定可能なデバイス ABIs**プロジェクトのプロパティ > [アプリケーション] タブ > サポートされているアーキテクチャ**。

デバッグ ビルドでは、すべての ABIs を提供し、したがってを使用して、最も高速な ABI ターゲット デバイスに対してこれ Android パッケージを使用します。

リリース ビルドにはプロジェクトのプロパティ タブで選択されている ABIs にはのみが含まれます。1 つ以上を選択することができます。

*armeabi* ABI、既定値は、デバイスの広範なサポートを持ちます。 *ただし*armeabi とサポートしていないデバイスのマルチ CPU ハードウェア浮動小数点、amont の他のものです。 その結果、armeabi リリース ランタイムを使用するアプリでは、1 つのコアに関連付けし、ソフト float 実装を使用します。 これらの両方は、アプリのパフォーマンスがはるかに低速に投稿できます。

有効にして、アプリには、適切な浮動小数点パフォーマンス (ゲームなど) が必要とする場合、 *armeabi v7a* ABI。 のみをサポートすることも、 *armeabi v7a*ランタイム、つまり、古いデバイスだけをサポートする*armeabi*アプリを実行することはできません。



## <a name="could-not-locate-android-sdk"></a>Android SDK が見つかりませんでした。

2 のダウンロードは Google の Android SDK for Windows から利用できます。
.Exe インストーラーを選択した場合は、インストール先 Xamarin.Android に指示するレジストリ キーを書き込みます。 .Zip ファイルを選択して解凍して Xamarin.Android は SDK を検索する場所を特定できません。 わかります Xamarin.Android SDK が Visual Studio でに移動して**ツール > オプション > Xamarin > Android 設定**:

[![Xamarin Android の設定で android SDK の場所](troubleshooting-images/01.png)](troubleshooting-images/01.png#lightbox)



## <a name="ide-does-not-display-target-device"></a>IDE では、ターゲット デバイスは表示されません。

場合によって、デバイスがデバイスの選択 ダイアログに表示されていないに展開するデバイスにアプリケーションを配置しようとするは。 これは、Android Debug Bridge 休暇上に移動する場合に発生することができます。

この問題を診断するには、検索、 [adb プログラム](~/android/deploy-test/debugging/android-debug-log.md)、し実行します。

```shell
adb devices
```

デバイスがない場合をデバイスを検出できるように、Android Debug Bridge サーバーを再起動する必要があります。

```shell
adb kill-server
adb start-server
```

HTC の同期ソフトウェアを防ぐことがあります**adb 開始サーバー**適切に動作します。 場合、 **adb 開始サーバー**コマンドで開始されるポートを出力しません、HTC 同期ソフトウェアを終了してください、adb サーバーを再起動してみてください。


## <a name="the-specified-task-executable-keytool-could-not-be-run"></a>指定したタスクの実行可能ファイル"keytool"を実行できませんでした。

これは、ご使用のパスに、Java SDK の bin ディレクトリが配置されているディレクトリが含まれていないことを意味します。 これらの手順に従っていることを確認して、[インストール](~/android/get-started/installation/index.md)ガイドです。


## <a name="monodroidexe-or-aresgenexe-exited-with-code-1"></a>monodroid.exe または aresgen.exe コード 1 でが終了しました。

この問題をデバッグし、Visual Studio に移動して、これを行う、MSBuild 詳細レベルを変更するための選択:**ツール > オプション > プロジェクト**と**ソリューション > ビルド**と**実行 >MSBuild プロジェクト ビルド出力の詳細**にこの値を設定および**標準**です。

再構築して、エラー全文にはが含まれている Visual Studio の出力ウィンドウを確認します。

## <a name="there-is-not-enough-storage-space-on-the-device-to-deploy-the-package"></a>パッケージを展開するデバイス上に十分な記憶域がありません。

これは、Visual Studio 内からエミュレーターを起動しない場合に発生します。 Visual Studio の外部でエミュレーターを開始するときに渡す必要がある、`-partition-size 512`オプションは、例。

```shell
emulator -partition-size 512 -avd MonoDroid
```

つまり、シミュレーターの正しい名前を使用することを確認[シミュレーターを構成するときに使用した名前](~/android/get-started/installation/windows.md#device)です。


## <a name="installfailedinvalidapk-when-installing-a-package"></a>インストール\_失敗\_無効な\_APK パッケージをインストールする場合

Android パッケージ名*必要があります*ピリオドを使用する ('*.*')。 ピリオドが含まれているように、パッケージ名を編集します。

-   Visual studio は。
    -   プロジェクトを右クリックして > のプロパティ
    -   Android マニフェスト タブで、左をクリックします。
    -   パッケージ名 フィールドを更新します。
        -   メッセージを表示する場合は&ldquo;いいえ AndroidManifest.xml が見つかりました。 いずれかの追加 をクリックします。&rdquo;リンクをクリックし、パッケージ名 フィールドを更新します。
-   Visual Studio for Mac: 内
    -   プロジェクトを右クリックして > オプション。
    -   ビルドに移動/Android アプリケーション セクションです。
    -   フィールドを変更、パッケージ名を含む、'.' です。




## <a name="installfailedmissingsharedlibrary-when-installing-a-package"></a>インストール\_失敗\_MISSING\_SHARED\_パッケージをインストールするときに、ライブラリ

このコンテキストで「共有ライブラリ」は*いない*ネイティブ共有ライブラリ (*libfoo.so*) ファイルです。 ターゲット デバイスで、Google マップなどとは別にインストールする必要があるライブラリでは代わりにします。

Android パッケージを指定する共有ライブラリを必要と、`<uses-library/>`要素。 場合、*必要*ライブラリは、ターゲット デバイスに存在しません (例:`//uses-library/@android:required`は*true*、既定値) とパッケージのインストールは失敗し、*インストール\_失敗\_MISSING\_SHARED\_ライブラリ*です。

必要な共有ライブラリを指定するには、表示、*生成*
**AndroidManifest.xml**ファイル (例: **obj\\デバッグ\\android\\AndroidManifest.xml**) を探して、`<uses-library/>`要素。 `<uses-library/>` プロジェクトの要素を手動で追加できる**プロパティ\\AndroidManifest.xml**ファイルおよび via、 [UsesLibraryAttribute カスタム属性](https://developer.xamarin.com/api/type/Android.App.UsesLibraryAttribute/)です。

たとえば、アセンブリへの参照を追加する*Mono.Android.GoogleMaps.dll*は暗黙的に追加、 `<uses-library/>` Google マップ共有ライブラリ。



## <a name="installfailedupdateincompatible-when-installing-a-package"></a>インストール\_失敗\_更新\_パッケージをインストールするときに互換性がありません

Android パッケージでは、次の 3 つの要件があります。

-   含まれる必要があります、'.'(前のエントリを参照してください)
-   一意の文字列のパッケージ名がある必要があります (そのため、リバース tld 規約 Chrome アプリ com.android.chrome など、Android アプリ名に表示される)
-   パッケージをアップグレードするときに、パッケージは同じ署名キーを持つ必要があります。

したがって、このシナリオを考えてみましょう。

1.  ビルドし、デバッグ アプリとしてアプリを配置します。
2.  キーを変更する、署名などをリリース アプリとして使用 (または、既定によって提供された署名キーのデバッグに満足できないため)
3.  削除することがなく、アプリをインストールするなどのデバッグ > デバッグなしで Visual Studio 内で


パッケージのインストールは、インストール失敗このとき、\_失敗\_更新\_互換性のないエラーは、キーがでした、署名中に、パッケージ名が変更されなかったためです。 [Android のデバッグ ログ](~/android/deploy-test/debugging/android-debug-log.md)のようなメッセージも含まれます。

```shell
E/PackageManager(  146): Package [PackageName] signatures do not match the previously installed version; ignoring!
```

このエラーを解決するには、完全に削除アプリケーション、デバイスから再インストールする前にします。


## <a name="installfaileduidchanged-when-installing-a-package"></a>インストール\_失敗\_UID\_パッケージをインストールするときに変更

Android パッケージがインストールされているときに割り当てられます、*ユーザー id* (UID)。
*場合によって*で現在不明な理由により、既にインストールされているアプリをインストールするときに、インストールは失敗します`INSTALL_FAILED_UID_CHANGED`:

```shell
ERROR [2015-03-23 11:19:01Z]: ANDROID: Deployment failed
Mono.AndroidTools.InstallFailedException: Failure [INSTALL_FAILED_UID_CHANGED]
   at Mono.AndroidTools.Internal.AdbOutputParsing.CheckInstallSuccess(String output, String packageName)
   at Mono.AndroidTools.AndroidDevice.<>c__DisplayClass2c.<InstallPackage>b__2b(Task`1 t)
   at System.Threading.Tasks.ContinuationTaskFromResultTask`1.InnerInvoke()
   at System.Threading.Tasks.Task.Execute()
```

この問題を回避する*を完全にアンインストール*か、Android ターゲットの GUI からアプリをインストールするかを使用して、Android パッケージ`adb`:

```shell
$ adb uninstall @PACKAGE_NAME@
```

**使用しないでください**`adb uninstall -k`はこのように、*を保持する*アプリケーションのデータとターゲット デバイスで競合する UID を保持するためです。



## <a name="release-apps-fail-to-launch-on-device"></a>リリース アプリがデバイスの起動に失敗します。

Android のデバッグ ログの出力ではのようなメッセージが含まれます。

```shell
D/AndroidRuntime( 1710): Shutting down VM
W/dalvikvm( 1710): threadid=1: thread exiting with uncaught exception (group=0xb412f180)
E/AndroidRuntime( 1710): FATAL EXCEPTION: main
E/AndroidRuntime( 1710): java.lang.UnsatisfiedLinkError: Couldn't load monodroid: findLibrary returned null
E/AndroidRuntime( 1710):        at java.lang.Runtime.loadLibrary(Runtime.java:365)
```

場合は、この 2 つの原因があります。

1.  .Apk には、ターゲット デバイスをサポートする ABI を提供していません。
    たとえば、.apk には、armeabi v7a バイナリにはのみが含まれていて、ターゲット デバイスでは、armeabi のみがサポートします。

2.  [Android バグ](http://code.google.com/p/android/issues/detail?id=21670)です。 大文字と小文字の場合は、アプリをアンインストールするアプリを再インストールして、指をクロスします。

(1) を修正するには、プロジェクトのオプション/プロパティの編集と[ABIs のサポートの一覧に、必要な ABI のサポートを追加](~/android/app-fundamentals/cpu-architectures.md)です。 追加する必要があります。 どの ABI を特定するのには、ターゲット デバイスに対して次の adb コマンドを実行します。

```shell
adb shell getprop ro.product.cpu.abi
adb shell getprop ro.product.cpu.abi2
```

出力には、プライマリにが含まれます (と省略可能なセカンダリ) ABIs です。

```shell
$ adb shell getprop | grep ro.product.cpu
[ro.product.cpu.abi2]: [armeabi]
[ro.product.cpu.abi]: [armeabi-v7a]
```

## <a name="the-outpath-property-is-not-set-for-project-ldquomyappcsprojrdquo"></a>プロジェクトの OutPath プロパティが設定されていない&ldquo;MyApp.csproj&rdquo;

通常、つまり、HP コンピューターと、環境変数がある&ldquo;プラットフォーム&rdquo;MCD や HPD のようなものに設定されています。 設定されている一般に MSBuild Platform プロパティを使用して競合&ldquo;Any CPU&rdquo;または&ldquo;x86&rdquo;です。 MSBuild が機能するには、自分のコンピューターからこの環境変数を削除する必要があります。

-   コントロール パネル > システム > 高度な > 環境変数

Mac 用 Visual Studio または Visual Studio を再起動して、再構築する再試行してください。 期待どおりに今すぐ機能する必要があります。

## <a name="javalangclasscastexception-monoandroidruntimejavaobject-cannot-be-cast-to"></a>java.lang.ClassCastException: mono.android.runtime.JavaObject cannot be cast to...

Xamarin.Android 4.x が入れ子になったジェネリック型を正しくマーシャ リング適切です。 たとえば、次の C\#を使用したコード[SimpleExpandableListAdapter](https://developer.xamarin.com/api/type/Android.Widget.SimpleExpandableListAdapter/):


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


この問題は、ある Xamarin.Android 正しくマーシャ リングに入れ子になったジェネリック型です。 `List<IDictionary<string, object>>`にマーシャ リングされる、 [java.lang.ArrrayList](https://developer.xamarin.com/api/type/Java.Util.ArrayList/)、ですが、`ArrayList`が含まれている`mono.android.runtime.JavaObject`インスタンス (どの参照、`Dictionary<string, object>`インスタンス)を実装するものではなく[java.util.Map](https://developer.xamarin.com/api/type/Java.Util.IMap/)、その結果、次の例外に。

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

指定された使用の回避策は[Java コレクション型](~/android/internals/api-design.md)の代わりに、`System.Collections.Generic`用の型、&ldquo;内部&rdquo;型です。 これが発生適切な Java の型のインスタンスをマーシャ リングするとします。 (次のコードは gref の有効期間を短縮するために必要なよりも複雑です。 使用して元のコードを変更することを簡略化できます`s/List/JavaList/g`と`s/Dictionary/JavaDictionary/g`gref の有効期間が心配がない場合)。

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

[これは、将来のリリースで修正](https://bugzilla.xamarin.com/show_bug.cgi?id=5401)です。


## <a name="unexpected-nullreferenceexceptions"></a>予期しない NullReferenceExceptions

場合によっては、 [Android のデバッグ ログ](~/android/deploy-test/debugging/android-debug-log.md)NullReferenceExceptions を説明する&ldquo;は発生しません&rdquo;Android ランタイム コードの直前にアプリがなくなるの Mono から取得または。

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

これは、Android のランタイムをいくつかのターゲットの GREF 制限に達して、何かなどの理由が考えられますが、このプロセスを中止する場合に発生することができます&ldquo;間違った&rdquo;JNI とします。

かどうかは、大文字と小文字を参照するには、Android のようなプロセスからのメッセージをデバッグ ログを確認します。

```shell
E/dalvikvm(  123): VM aborting
```


## <a name="abort-due-to-global-reference-exhaustion"></a>グローバル参照の枯渇による中止します。

Android のランタイムの JNI レイヤーは、JNI オブジェクトへの参照時に任意の時点で有効なあります限られた数のみをサポートします。 この制限を超えたときに処理が中断されます。

GREF (*グローバル リファレンス*) の上限は、エミュレーターで 2000 参照とハードウェアに対する ~ 52000 を参照します。

このなど、Android のデバッグ ログ内のメッセージが表示される場合が多すぎます GREFs の作成を開始していることがわかってください。

```shell
D/dalvikvm(  602): GREF has increased to 1801
```

GREF 制限に到達すると、次のようメッセージが出力されます。

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


上記の例 (ちなみに、由来、 [685215 をバグ](https://bugzilla.novell.com/show_bug.cgi?id=685215)) 問題が多すぎます Android.Graphics.Point のインスタンスを作成することですを参照してください[コメント\#2](https://bugzilla.novell.com/show_bug.cgi?id=685215#c2)の修正プログラムの一覧については。この特定のバグです。

どの型が多数のインスタンスを検索する便利なソリューションが通常は割り当てられている&ndash;ダンプすると上記の Android.Graphics.Point&ndash;ここで作成される、ソース コードとそれらの dispose で適切にし、検索 (できるように、Java-object 有効期間が詰められます)。 これは常に適切な (\#685215 はマルチ スレッドで単純なソリューションが Dispose の呼び出しを回避できます)、最初に検討することができます。

有効にすることができます[GREF ログ](~/android/troubleshooting/index.md)GREFs を作成するときと、多数あるを表示します。


## <a name="abort-due-to-jni-type-mismatch"></a>JNI 型が一致したため中止されました

キューブをハンド ロール JNI コードとして設定すると場合、可能であれば、型が一致しないなど正しくを呼び出そうとする場合`java.lang.Runnable.run`実装していません。 型に`java.lang.Runnable`です。 このような場合があります、メッセージ、Android のデバッグ ログに次のような。

```shell
W/dalvikvm( 123): JNI WARNING: can't call Ljava/Type;;.method on instance of Lanother/java/Type;
W/dalvikvm( 123):              in Lmono/java/lang/RunnableImplementor;.n_run:()V (CallVoidMethodA)
...
E/dalvikvm( 123): VM aborting
```

## <a name="dynamic-code-support"></a>動的なコードのサポート

### <a name="dynamic-code-does-not-compile"></a>動的なコードはコンパイルされません。

C を使用する\#アプリケーションまたはライブラリに動的で、追加する必要が System.Core.dll、Microsoft.CSharp.dll および Mono.CSharp.dll をプロジェクトにします。

### <a name="in-release-build-missingmethodexception-occurs-for-dynamic-code-at-run-time"></a>リリース ビルドで MissingMethodException 動的コードの実行時に発生します。

-   System.Core.dll、Microsoft.CSharp.dll または Mono.CSharp.dll への参照が、アプリケーション プロジェクトにない可能性があります。 これらのアセンブリが参照されていることを確認してください。

    -   コストを抑えることに注意動的コード常にします。 効率的なコードを必要がある場合は、動的なコードを使用していないことを検討します。

-   プレビューでは、最初、各アセンブリ内の型は、アプリケーション コードによって明示的に使用されていない限り、それらのアセンブリは除外されました。 回避策については、次を参照してください。 [http://lists.ximian.com/pipermail/mo...il/009798.html](http://lists.ximian.com/pipermail/monodroid/2012-April/009798.html)


## <a name="projects-built-with-aotllvm-crash-on-x86-devices"></a>X86 で AOT + ある LLVM クラッシュでビルドされたプロジェクトのデバイス

ビルドされたアプリを配置するときに[AOT + ある LLVM](~/android/deploy-test/release-prep/index.md) x86 ベースのデバイスで、次のような例外エラー メッセージが表示することがあります。

```shell
Assertion: should not be reached at /Users/.../external/mono/mono/mini/tramp-x86.c:124
Fatal signal 6 (SIGABRT), code -6 in tid 4051 (amarin.bug56111)
```

これで報告される既知の問題は、 [56111](https://bugzilla.xamarin.com/show_bug.cgi?id=56111)です。 回避策は、ある LLVM を無効にすることです。
