---
title: ProGuard
description: ProGuard は、Java クラス ファイルのシュリンカー、オプティマイザー、オブファスケーター、および事前検証機能です。 これは、未使用のコードを検出して削除し、バイトコードの分析と最適化を行い、クラスとクラス メンバーを難読化します。 このガイドでは、ProGuard がどのように機能するか、プロジェクトで有効にする方法、および設定方法について説明します。 また、ProGuard の設定例もいくつか示します。
ms.prod: xamarin
ms.assetid: 29C0E850-3A49-4618-9078-D59BE0284D5A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: e65c78633ae91318bd8e9cce949bac9cc12675c0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="proguard"></a>ProGuard

_ProGuard は、Java クラス ファイルのシュリンカー、オプティマイザー、オブファスケーター、および事前検証機能です。これは、未使用のコードを検出して削除し、バイトコードの分析と最適化を行い、クラスとクラス メンバーを難読化します。このガイドでは、ProGuard がどのように機能するか、プロジェクトで有効にする方法、および設定方法について説明します。また、ProGuard の設定例もいくつか示します。_


## <a name="overview"></a>概要

ProGuard は、パッケージ化されたアプリケーションから使われていないクラス、フィールド、メソッド、属性を検出して削除します。 参照されているライブラリに対して同じ処理を行うこともできます (これは、64k 参照制限を防ぐのに役立つことがあります)。 また、Android SDK の ProGuard ツールは、バイトコードを最適化し、使われていないコード命令を削除し、残ったクラス、フィールド、メソッドを短い名前で難読化します。 ProGuard は、**入力 jar** を読み取り、圧縮、最適化、難読化、事前検証します。結果を 1 つ以上の**出力 jar** に書き込みます。 

ProGuard は、以下の手順を使って入力 APK を処理します。 

1.  **圧縮ステップ** &ndash; ProGuard は、使われているクラスとクラス メンバーを再帰的に特定します。 他のすべてのクラスおよびクラス メンバーは破棄されます。 

2.  **最適化ステップ** &ndash; ProGuard はさらにコードを最適化します。 
    たとえば、エントリ ポイントではないクラスとメソッドを private、static、または final にし、使われていないパラメーターを削除し、一部のメソッドをインライン化します。 

3.  **難読化ステップ** &ndash; ProGuard は、エントリ ポイントではないクラスおよびクラス メンバーの名前を変更します。 エントリ ポイントの名前は維持し、元の名前でアクセスできるようにします。 

4.  **事前検証ステップ** &ndash; 実行前に Java バイトコードのチェックを行い、Java VM のためにクラス ファイルに注釈を付けます。 これは、エントリ ポイントを知る必要がない唯一のステップです。 

これらの各ステップは "*省略可能*" です。 次のセクションで説明するように、Xamarin.Android の ProGuard はこれらのステップのサブセットのみを使います。 



## <a name="proguard-in-xamarinandroid"></a>Xamarin.Android での ProGuard

Xamarin.Android ProGuard の構成では、APK は難読化されません。 実際、ProGuard で難読化を有効にすることはできません (カスタム構成ファイルを使用しても)。 したがって、Xamarin.Android の ProGuard は、**圧縮**ステップと**最適化**ステップのみを実行します。 

[![圧縮ステップと最適化ステップ](proguard-images/01-xa-chain-sml.png)](proguard-images/01-xa-chain.png#lightbox)

ProGuard を使う前に知っておく必要のある重要なことは、`Xamarin.Android` のビルド プロセスにおけるその動作方法です。 このプロセスでは次の 2 つの個別のステップが使われます。 

1.  Xamarin Android リンカー

2.  ProGuard

次に各ステップについて詳しく説明します。



### <a name="linker-step"></a>リンカー ステップ

Xamarin.Android リンカーは、アプリケーションの静的分析を使って次のことを特定します。 

-   実際に使われているアセンブリ。

-   実際に使われている型。

-   実際に使われているメンバー。 

リンカーは常に ProGuard ステップの前に実行されます。 このため、リンカーは、ProGuard で処理されると予想されるアセンブリ/型/メンバーを除去することがあります  (Xamarin.Android でのリンクの詳細については、「[Linking on Android](~/android/deploy-test/linker.md)」(Android でのリンク) をご覧ください)。



### <a name="proguard-step"></a>ProGuard ステップ

リンカー ステップが正常に完了した後は、ProGuard が実行されて未使用の Java バイトコードが削除されます。 これは、APK を最適化するステップです。 



## <a name="using-proguard"></a>ProGuard の使用

ProGuard をアプリ プロジェクトを使用するには、まず ProGuard を有効にする必要があります。 次に、Xamarin.Android のビルド プロセスに既定の ProGuard 構成ファイルを使用させるか、または ProGuard 用に独自のカスタム構成ファイルを作成することができます。 



### <a name="enabling-proguard"></a>ProGuard の有効化

ProGuard をアプリ プロジェクトで有効にするには、次の手順を使います。

1.  プロジェクトが**リリース**構成に設定されていることを確認してください (これは、ProGuard が実行するにはリンカーが実行している必要があるため重要なことです)。 

    [![リリース構成を選択する](proguard-images/02-set-release-sml.png)](proguard-images/02-set-release.png#lightbox)
   
2.  **[プロパティ] > [Android オプション]** の **[パッケージ]** タブの **[ProGuard を有効にする]** オプションをオンにして ProGuard を有効にします。 

    [![オンにした [ProGuard を有効にする] オプション](proguard-images/03-enable-proguard-sml.png)](proguard-images/03-enable-proguard.png#lightbox)

ほとんどの Xamarin.Android アプリでは、Xamarin.Android によって提供される既定の ProGuard 構成ファイルを使用すると、すべての未使用コード (のみ) を削除するのに十分です。 既定の ProGuard 構成を表示するには、**obj\\Release\\proguard\\proguard_xamarin.cfg** ファイルを開きます。 次のセクションでは、カスタマイズした ProGuard 構成ファイルを作成する方法について説明します。 



### <a name="customizing-proguard"></a>ProGuard のカスタマイズ

必要に応じて、カスタム ProGuard 構成ファイルを追加し、ProGuard ツールをより厳密に制御することができます。 たとえば、ProGuard に維持するクラスを明示的に指示できます。 そのためには、新しい **.cfg** ファイルを作成し、**[ソリューション エクスプローラー]** の **[プロパティ]** ウィンドウで `ProGuardConfiguration` ビルド アクションを適用します。 

[![ProguardConfiguration ビルド アクションの選択](proguard-images/04-build-action-sml.png)](proguard-images/04-build-action.png#lightbox)

この構成ファイルは Xamarin.Android の **proguard_xamarin.cfg** ファイルの代わりに使用するものではないことに注意してください。ProGuard は両方のファイルを使用します。 

一般的な ProGuard 構成ファイルの例を次に示します。
    

    # This is Xamarin-specific (and enhanced) configuration.

    -dontobfuscate

    -keep class mono.MonoRuntimeProvider { *; <init>(...); }
    -keep class mono.MonoPackageManager { *; <init>(...); }
    -keep class mono.MonoPackageManager_Resources { *; <init>(...); }
    -keep class mono.android.** { *; <init>(...); }
    -keep class mono.java.** { *; <init>(...); }
    -keep class mono.javax.** { *; <init>(...); }
    -keep class opentk.platform.android.AndroidGameView { *; <init>(...); }
    -keep class opentk.GameViewBase { *; <init>(...); }
    -keep class opentk_1_0.platform.android.AndroidGameView { *; <init>(...); }
    -keep class opentk_1_0.GameViewBase { *; <init>(...); }

    -keep class android.runtime.** { <init>(***); }
    -keep class assembly_mono_android.android.runtime.** { <init>(***); }
    # hash for android.runtime and assembly_mono_android.android.runtime.
    -keep class md52ce486a14f4bcd95899665e9d932190b.** { *; <init>(...); }
    -keepclassmembers class md52ce486a14f4bcd95899665e9d932190b.** { *; <init>(...); }

    # Android's template misses fluent setters...
    -keepclassmembers class * extends android.view.View {
       *** set*(***);
    }

    # also misses those inflated custom layout stuff from xml...
    -keepclassmembers class * extends android.view.View {
       <init>(android.content.Context,android.util.AttributeSet);
       <init>(android.content.Context,android.util.AttributeSet,int);
    }
    

ProGuard がアプリケーションを正しく分析できない場合があります。アプリケーションが実際に必要とするコードが削除される可能性があります。 その場合は、`-keep` 行をカスタム ProGuard 構成ファイルに追加します。 

    -keep public class MyClass

次の例では、ProGuard でスキップするクラスの実際の名前として `MyClass` を設定しています。

`[Register]` 注釈で独自の名前を登録し、その名前を使って ProGuard のルールをカスタマイズすることもできます。 Adapters、Views、BroadcastReceivers、Services、ContentProviders、Activities、Fragments の名前を登録できます。 `[Register]` カスタム属性の使用の詳細については、「[Working with JNI](~/android/platform/java-integration/working-with-jni.md)」(JNI の処理) をご覧ください。


### <a name="proguard-options"></a>ProGuard のオプション

ProGuard には、操作をきめ細かく制御するために構成できる複数のオプションが用意されています。 [ProGuard のマニュアル](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/index.html#manual/introduction.html)では、ProGuard を使うための完全なリファレンス ドキュメントが提供されています。 

Xamarin.Android では、次の ProGuard オプションがサポートされています。 


-    [入出力オプション](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#iooptions)

-    [保持オプション](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#keepoptions)

-    [圧縮オプション](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#shrinkingoptions)

-    [全般オプション](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#generaloptions)

-    [クラス パス](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#classpath)

-    [ファイル名](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#filename)

-    [ファイル フィルター](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#filefilters)

-    [フィルター](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#filters)

-    [`Keep` オプションの概要](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#keepoverview)

-    [保持オプションの修飾子](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#keepoptionmodifiers)

-    [クラスの指定](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#classspecification)

以下のオプションは、Xamarin.Android では "*無視*" されます。

-    [最適化オプション](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#optimizationoptions)

-    [難読化オプション](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#obfuscationoptions) 

-    [事前検証オプション](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#preverificationoptions)



## <a name="proguard-and-android-nougat"></a>ProGuard と Android Nougat

ProGuard を Android 7.0 以降に対して使用する場合は、Android SDK には JDK 1.8 と互換性がある新しいバージョンが含まれていないため、ProGuard の新しいバージョンをダウンロードする必要があります。

この [NuGet パッケージ](https://www.nuget.org/packages/name.atsushieno.proguard.facebook/5.3.0)を使用して、`proguard.jar` の新しいバージョンをインストールできます。 Android SDK の既定の `proguard.jar` の更新の詳細については、こちらの [Stack Overflow](http://stackoverflow.com/questions/39514518/xamarin-android-proguard-unsupported-class-version-number-52-0/39514706#39514706) をご覧ください。

[SourceForge ページ](https://sourceforge.net/projects/proguard/files/)で ProGuard のすべてのバージョンを見つけることができます。 



## <a name="example-proguard-configurations"></a>ProGuard の構成例

ProGuard 構成ファイルの 2 つの例を次に示します。 これらの例では、Xamarin.Android ビルド プロセスが**入力**、**出力**、**ライブラリ**の各 jar を提供することに注意してください。 したがって、`-keep` などの他のオプションに集中できます。 

### <a name="a-simple-android-activity"></a>簡単な Android アクティビティ

単純な Android アクティビティの構成例を次に示します。

    -injars  bin/classes
    -outjars bin/classes-processed.jar
    -libraryjars /usr/local/java/android-sdk/platforms/android-9/android.jar

    -dontpreverify
    -repackageclasses ''
    -allowaccessmodification
    -optimizations !code/simplification/arithmetic

    -keep public class mypackage.MyActivity

### <a name="a-complete-android-application"></a>完全な Android アプリケーション

機能が一式そろった Android アプリの構成例を次に示します。

    -injars  bin/classes
    -injars  libs
    -outjars bin/classes-processed.jar
    -libraryjars /usr/local/java/android-sdk/platforms/android-9/android.jar

    -dontpreverify
    -repackageclasses ''
    -allowaccessmodification
    -optimizations !code/simplification/arithmetic
    -keepattributes *Annotation*

    -keep public class * extends android.app.Activity
    -keep public class * extends android.app.Application
    -keep public class * extends android.app.Service
    -keep public class * extends android.content.BroadcastReceiver
    -keep public class * extends android.content.ContentProvider

    -keep public class * extends android.view.View {
    public <init>(android.content.Context);
    public <init>(android.content.Context, android.util.AttributeSet);
    public <init>(android.content.Context, android.util.AttributeSet, int);
    public void set*(...);
    }

    -keepclasseswithmembers class * {
    public <init>(android.content.Context, android.util.AttributeSet);
    }

    -keepclasseswithmembers class * {
    public <init>(android.content.Context, android.util.AttributeSet, int);
    }

    -keepclassmembers class * implements android.os.Parcelable {
    static android.os.Parcelable$Creator CREATOR;
    }

    -keepclassmembers class **.R$* {
    public static <fields>;
    }


## <a name="proguard-and-the-xamarinandroid-build-process"></a>ProGuard と Xamarin.Android のビルド プロセス

次のセクションでは、Xamarin.Android の**リリース** ビルドの間の ProGuard の動作について説明します。


### <a name="what-command-is-proguard-running"></a>ProGuard が実行するコマンド

ProGuard は単に、Android SDK で提供される `.jar` です。 したがって、コマンドで呼び出されます。 

```shell
java -jar proguard.jar options ...
```

### <a name="the-proguard-task"></a>ProGuard タスク

ProGuard タスクは **Xamarin.Android.Build.Tasks.dll** アセンブリ内にあります。 `_CompileToDalvikWithDx` ターゲットの一部であり、これは `_CompileDex` ターゲットの一部です。 

次のリストでは、**[ファイル] > [新しいプロジェクト]** を使って新しいプロジェクトを作成した後に生成される既定のパラメーターの例を示します。 

    ProGuardJarPath = C:\Android\android-sdk\tools\proguard\lib\proguard.jar
    AndroidSdkDirectory = C:\Android\android-sdk\
    JavaToolPath = C:\Program Files (x86)\Java\jdk1.8.0_92\\bin
    ProGuardToolPath = C:\Android\android-sdk\tools\proguard\
    JavaPlatformJarPath = C:\Android\android-sdk\platforms\android-25\android.jar
    ClassesOutputDirectory = obj\Release\android\bin\classes
    AcwMapFile = obj\Release\acw-map.txt
    ProGuardCommonXamarinConfiguration = obj\Release\proguard\proguard_xamarin.cfg
    ProGuardGeneratedReferenceConfiguration = obj\Release\proguard\proguard_project_references.cfg
    ProGuardGeneratedApplicationConfiguration = obj\Release\proguard\proguard_project_primary.cfg
    ProGuardConfigurationFiles

      {sdk.dir}tools\proguard\proguard-android.txt;
      {intermediate.common.xamarin};
      {intermediate.references};
      {intermediate.application};
      ;
     
    JavaLibrariesToEmbed = C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\MonoAndroid\v7.0\mono.android.jar
    ProGuardJarInput = obj\Release\proguard\__proguard_input__.jar
    ProGuardJarOutput = obj\Release\proguard\__proguard_output__.jar
    DumpOutput = obj\Release\proguard\dump.txt
    PrintSeedsOutput = obj\Release\proguard\seeds.txt
    PrintUsageOutput = obj\Release\proguard\usage.txt
    PrintMappingOutput = obj\Release\proguard\mapping.txt

次の例では、IDE から実行される一般的な ProGuard コマンドを示します。

```cmd
C:\Program Files (x86)\Java\jdk1.8.0_92\\bin\java.exe -jar C:\Android\android-sdk\tools\proguard\lib\proguard.jar -include obj\Release\proguard\proguard_xamarin.cfg -include obj\Release\proguard\proguard_project_references.cfg -include obj\Release\proguard\proguard_project_primary.cfg "-injars 'obj\Release\proguard\__proguard_input__.jar';'C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\MonoAndroid\v7.0\mono.android.jar'" "-libraryjars 'C:\Android\android-sdk\platforms\android-25\android.jar'" -outjars "obj\Release\proguard\__proguard_output__.jar" -optimizations !code/allocation/variable
```

## <a name="troubleshooting"></a>トラブルシューティング

### <a name="file-issues"></a>ファイルの問題

ProGuard が構成ファイルを読み取るときに、次のエラー メッセージが表示されることがあります。 

    Unknown option '-keep' in line 1 of file 'proguard.cfg'

Windows でこの問題が発生するのは、通常、`.cfg` ファイルのエンコーディングが正しくないためです。 ProGuard は、テキスト ファイルに存在する可能性がある "_バイト オーダー マーク_" (BOM) を処理できません。 BOM が存在する場合、ProGuard は上記のエラーで終了します。 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

この問題を回避するには、BOM なしでファイルを保存できるテキスト エディターでカスタム構成ファイルを編集します。 この問題を解決するには、テキスト エディターのエンコードを `UTF-8` に設定します。 たとえば、[Notepad++](https://notepad-plus-plus.org/) テキスト エディターでは、**[Encoding]\(エンコード\) &gt; [Encode in UTF-8 Without BOM]\(BOM なしの UTF-8 でエンコードする\)** を選ぶことにより、BOM を使わずにファイルを保存できます。 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

この問題を回避するには、BOM を省略することができるテキスト エディターから、カスタム構成ファイルを保存します。 

-----


### <a name="other-issues"></a>その他の問題

ProGuard を使うと発生する可能性がある一般的な問題と解決策については、ProGuard の「[Troubleshooting](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/index.html#manual/troubleshooting.html)」(トラブルシューティング) ページをご覧ください。


## <a name="summary"></a>まとめ

このガイドでは、Xamarin.Android での ProGuard の機能、アプリ プロジェクトで有効にする方法、および設定方法について説明しました。 ProGuard の構成の例を示し、一般的な問題の対処方法を説明します。 ProGuard ツールと Android の詳細については、「[Shrink Your Code and Resources](http://developer.android.com/tools/help/proguard.html)」(コードとリソースの圧縮) をご覧ください。 


## <a name="related-links"></a>関連リンク

- [リリースに向けてアプリケーションを準備する](~/android/deploy-test/release-prep/index.md)
