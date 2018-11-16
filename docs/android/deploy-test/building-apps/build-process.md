---
title: ビルド プロセス
ms.prod: xamarin
ms.assetid: 3BE5EE1E-3FF6-4E95-7C9F-7B443EE3E94C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/14/2018
ms.openlocfilehash: b63efb3f9bfa432f15415e652cd5d59f929c4488
ms.sourcegitcommit: 6be6374664cd96a7d924c2e0c37aeec4adf8be13
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/13/2018
ms.locfileid: "51617788"
---
# <a name="build-process"></a>ビルド プロセス


## <a name="overview"></a>概要

Xamarin.Android ビルド プロセスは、[`Resource.designer.cs` の生成](~/android/internals/api-design.md)、`AndroidAsset`、`AndroidResource`、およびその他の[ビルド アクション](#Build_Actions)のサポート、[Android 呼び出し可能なラッパー](~/android/platform/java-integration/android-callable-wrappers.md)の生成、Android デバイスで実行するための `.apk` の生成のすべてを結び付ける役割を担っています。


## <a name="application-packages"></a>アプリケーション パッケージ

Xamarin.Android ビルド システムが生成できる Android アプリケーション パッケージ (`.apk` ファイル) には、大まかに次の 2 種類があります。 

-   **リリース** ビルドは、実行に追加のパッケージを必要としない、完全な自己完結型です。 App Store に提供されるのはこのパッケージです。 

-   **デバッグ** ビルドではありません。 

偶然ではなく、これらはパッケージを生成する MSBuild `Configuration` が一致しています。

### <a name="shared-runtime"></a>共有ランタイム

*共有ランタイム*は、基本クラス ライブラリ (`mscorlib.dll` など) と Android バインド ライブラリ (`Mono.Android.dll` など) を提供する追加の Android パッケージのペアです。 デバッグ ビルドは共有ランタイムに依存して、基本クラス ライブラリとバインド アセンブリを Android アプリケーション パッケージ内に含める代わりに、デバッグ パッケージをより小さくできるようにします。

共有ランタイムは、`$(AndroidUseSharedRuntime)` プロパティを `False` に設定することで、デバッグ ビルドで無効にすることができます。 

<a name="Fast_Deployment" />

### <a name="fast-deployment"></a>高速展開

*高速展開*は、共有ランタイムと連携して、Android アプリケーション パッケージのサイズをさらに縮小します。 これはアプリのアセンブリをパッケージ内にバンドルせずに、 `adb push` を介してターゲットにコピーすることで行います。 このプロセスにより、ビルド/展開/デバッグのサイクルが高速化されます。アセンブリ*のみ*が変更された場合、パッケージは再インストールされず、 代わりに、更新されたアセンブリだけがターゲット デバイスに再同期されるからです。 

高速展開は、`adb` のディレクトリ `/data/data/@PACKAGE_NAME@/files/.__override__` への同期をブロックするデバイスでは失敗することがわかっています。 

高速展開は、既定で有効になっており、`$(EmbedAssembliesIntoApk)` プロパティを `True` に設定することで、デバッグ ビルドで無効にすることができます。


## <a name="msbuild-projects"></a>MSBuild プロジェクト

Xamarin.Android ビルド プロセスは、Visual Studio for Mac と Visual Studio で使用されるプロジェクト ファイル形式でもある MSBuild に基づいています。
通常、ユーザーは、MSBuild ファイルを手動で編集する必要はありません &ndash; IDE によって完全に機能するプロジェクトが作成され、変更があれば更新され、必要に応じて自動的にビルド ターゲットが呼び出されます。 

上級ユーザーが IDE の GUI でサポートされていないことを行えるように、ビルド プロセスは、プロジェクト ファイルを直接編集することでカスタマイズできるようになっています。 このページでは、Xamarin.Android 固有の機能とカスタマイズのみを文書化しています &ndash; 通常の MSBuild 項目、プロパティおよびターゲットを使用して、さらに多くのことができます。 

<a name="Build_Targets" />

## <a name="build-targets"></a>ビルド ターゲット

Xamarin.Android プロジェクトに対して、次のビルド ターゲットが定義されています。

-   **Build** &ndash; パッケージをビルドします。

-   **Clean** &ndash; ビルド プロセスによって生成されたすべてのファイルを削除します。

-   **Install** &ndash; 既定のデバイスまたは仮想デバイスにパッケージをインストールします。

-   **Uninstall** &ndash; 既定のデバイスまたは仮想デバイスからパッケージをアンインストールします。

-   **SignAndroidPackage** &ndash; パッケージ (`.apk`) を作成して署名します。 `/p:Configuration=Release` とともに使用して、自己完結型の "リリース" パッケージを生成します。

-   **UpdateAndroidResources** &ndash; `Resource.designer.cs` ファイルを更新します。 このターゲットは通常、新しいリソースがプロジェクトに追加されたときに、IDE によって呼び出されます。


## <a name="build-properties"></a>[ビルド プロパティ]

MSBuild プロパティは、ターゲットの動作を制御します。 これらは、[MSBuild PropertyGroup 要素](https://docs.microsoft.com/visualstudio/msbuild/propertygroup-element-msbuild)内のプロジェクト ファイル (**MyApp.csproj** など) 内で指定されます。

-   **Configuration** &ndash; 使用するビルド構成 ("Debug" または "Release" など) を指定します。 Configuration プロパティは、ターゲットの動作を決定するその他のプロパティの既定値を決定するために使用されます。 追加の構成は、IDE 内で作成できます。

    *既定では*、`Debug` 構成により、操作する他のファイルとパッケージの存在を必要とするより小さい Android パッケージを作成する、`Install` ターゲットと `SignAndroidPackage` ターゲットがもたらされます。

    既定の `Release` 構成により、*スタンドアロン*で、他のパッケージやファイルをインストールしなくても使用できる Android パッケージを作成する `Install` ターゲットと `SignAndroidPackage` ターゲットがもたらされます。

-   **DebugSymbols** &ndash; `$(DebugType)` プロパティと組み合わせて、Android パッケージが*デバッグ可能*かどうかを決定するブール値。 デバッグ可能パッケージには、デバッグ シンボルが含まれており、`//application/@android:debuggable` 属性を `true` に設定し、`INTERNET` アクセス許可を自動的に追加して、デバッガーがプロセスにアタッチできるようにします。 `DebugSymbols` が `True` *で* `DebugType` が空の文字列または `Full` の場合、アプリケーションはデバッグ可能です。

-   **DebugType** &ndash; ビルドの一部として生成するための[デバッグ シンボルの型](https://docs.microsoft.com/visualstudio/msbuild/csc-task)を指定します。これはアプリケーションがデバッグ可能かどうかにも影響します。 次の値を使用できます。

    - **Full**: 完全なシンボルが生成されます。 `DebugSymbols` MSBuild プロパティも `True` の場合、アプリケーション パッケージはデバッグ可能です。

    - **PdbOnly**: "PDB" シンボルが生成されます。 アプリケーション パッケージはデバッグ可能には*なりません*。

    `DebugType` が設定されていないか、空の文字列の場合、`DebugSymbols` プロパティが、アプリケーションがデバッグ可能かどうかを制御します。


### <a name="install-properties"></a>インストール プロパティ

インストール プロパティは、`Install` ターゲットと `Uninstall` ターゲットの動作を制御します。

-   **AdbTarget** &ndash; Android パッケージのインストール先または削除元となる Android ターゲット デバイスを指定します。 このプロパティの値は、[`adb` ターゲット デバイス オプション](http://developer.android.com/tools/help/adb.html#issuingcommands)と同じです。

    ```bash
    # Install package onto emulator via -e
    # Use `/Library/Frameworks/Mono.framework/Commands/msbuild` on OS X
    MSBuild /t:Install ProjectName.csproj /p:AdbTarget=-e
    ```


### <a name="packaging-properties"></a>パッケージング プロパティ

パッケージング プロパティは、Android パッケージの作成を制御し、`Install` ターゲットと `SignAndroidPackage` ターゲッによって使用されます。
リリース アプリケーションをパッケージングする場合には、[署名プロパティ](#Signing_Properties)も関係します。


-   **AndroidApkSigningAlgorithm** &ndash; `jarsigner -sigalg` で使用する署名アルゴリズムを指定する文字列値。

    既定値は `md5withRSA` です。

    Xamarin.Android 8.2 で追加されました。

-   **AndroidApplication** &ndash; プロジェクトが Android アプリケーション用 (`True`) か、または Android ライブラリ プロジェクト用 (`False` または存在しない) かを示すブール値。

    `<AndroidApplication>True</AndroidApplication>` を持つプロジェクトは、Android パッケージ内に 1 つしか存在できない場合があります (残念ながら、これについてはまだ検証されていません。Android リソースに関するわかりにくいおかしなエラーになることがあります)。

-   **AndroidBuildApplicationPackage** &ndash; パッケージ (.apk) を作成して署名するかどうかを示すブール値。 この値を `True` に設定することは、[SignAndroidPackage](#Build_Targets) ビルド ターゲットを使用することと同じです。

    このプロパティのサポートは、Xamarin.Android 7.1 以降で追加されました。

    このプロパティは既定で `False` です。

-   **AndroidEnableMultiDex** &ndash; 最終的な `.apk` で Multi-Dex サポートを使用するかどうかを決定するブール型プロパティ。

    このプロパティのサポートは、Xamarin.Android 5.1 で追加されました。

    このプロパティは既定で `False` です。

-   **AndroidEnableSGenConcurrent** &ndash; Mono の[同時 GC コレクター](http://www.mono-project.com/docs/about-mono/releases/4.8.0/#concurrent-sgen)が使用されるかどうかを決定するブール型プロパティ。

    このプロパティのサポートは、Xamarin.Android 7.2 で追加されました。

    このプロパティは既定で `False` です。

-   **AndroidErrorOnCustomJavaObject** &ndash; `Java.Lang.Object` または `Java.Lang.Throwable` を継承すること*なく*、型が `Android.Runtime.IJavaObject` を実装するかどうかを決定するブール型プロパティ。
    

    ```csharp
    class BadType : IJavaObject {
        public IntPtr Handle {
            get {return IntPtr.Zero;}
        }

        public void Dispose()
        {
        }
    }
    ```

    True の場合、このような型は XA4212 エラーを生成し、それ以外の場合は XA4212 警告を生成します。

    このプロパティのサポートは、Xamarin.Android 8.1 で追加されました。

    このプロパティは既定で `True` です。

-   **AndroidFastDeploymentType** &ndash; `$(EmbedAssembliesIntoApk)` MSBuild プロパティが `False` の場合に、ターゲット デバイスの[高速展開ディレクトリ](#Fast_Deployment)に展開できる型を制御する値の `:` (コロン) 区切りのリスト。 リソースが高速展開される場合、そのリソースが生成された `.apk` に埋め込まれ*ない*ため、展開時間を短縮することができます  (高速展開が増えるほど、`.apk` を再ビルドする頻度が減り、インストール プロセスを高速化できます)。有効な値を次に示します。

    - `Assemblies`: アプリケーション アセンブリを展開します。

    - `Dexes`: `.dex` ファイル、Android リソース、および Android アセットを展開します。 **この値は、Android 4.4 以降 (API-19) を実行しているデバイスで*のみ*使用できます。**

    既定値は `Assemblies` です。

    **試験的**です。 Xamarin.Android 6.1 で追加されました。

-   **AndroidApplicationJavaClass** &ndash; [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/) からクラスを継承するときに、`android.app.Application` の代わりに使用する完全な Java クラス名。

    このプロパティは、通常、`$(AndroidEnableMultiDex)` MSBuild プロパティなどの "*他の*" プロパティによって設定されます。

    Xamarin.Android 6.1 で追加されました。

-   **AndroidHttpClientHandlerType** &ndash; 既定の `System.Net.Http.HttpClient` コンストラクターによって使用される、既定の `System.Net.Http.HttpMessageHandler` の実装を制御します。 値は `HttpMessageHandler` サブクラスのアセンブリ修飾型名であり、[`System.Type.GetType(string)`](/dotnet/api/system.type.gettype?view=netcore-2.0#System_Type_GetType_System_String_) での使用に適しています。

    既定値は `System.Net.Http.HttpClientHandler, System.Net.Http` です。

    これは、Android Java API を使用してネットワーク要求を実行する `Xamarin.Android.Net.AndroidClientHandler` を代わりに含むようにオーバーライドできます。 これにより、基になる Android バージョンが TLS 1.2 をサポートする場合、TLS 1.2 の URL にアクセスできます。  
    TLS 1.2 のサポートが Java を通じて確実に提供されるのは、Android 5.0 以降のみです。

    *注*: TLS 1.2 のサポートがバージョン 5.0 より前の Android で必要な場合、"*または*" TLS 1.2 のサポートが `System.Net.WebClient` および関連する API で必要な場合、`$(AndroidTlsProvider)` を使用する必要があります。

    *注*: このプロパティのサポートは、[`XA_HTTP_CLIENT_HANDLER_TYPE` 環境変数](~/android/deploy-test/environment.md)を設定することにより機能します。
    `@(AndroidEnvironment)` のビルド アクションを含むファイル内に見つかる `$XA_HTTP_CLIENT_HANDLER_TYPE` 値が優先されます。

    Xamarin.Android 6.1 で追加されました。

-   **AndroidTlsProvider** &ndash; アプリケーションで使用する必要がある TLS プロバイダーを指定する文字列値。 指定できる値は次のとおりです。

    - `btls`: [HttpWebRequest](xref:System.Net.HttpWebRequest) との TLS 通信に [BoringSSL](https://boringssl.googlesource.com/boringssl) を使用します。
      これにより、Android のすべてのバージョンで TLS 1.2 を使用できます。

    - `legacy`: ネットワークの対話に過去に管理されていた SSL の実装を使用します。 これは、TLS 1.2 をサポート*していません*。

    - `default`: *Mono* で既定の TLS プロバイダーを選択することを許可します。
      Xamarin.Android 7.3 でも、これは `legacy` と同等です。  
      *注*: IDE の "既定" 値が `$(AndroidTlsProvider)` プロパティの "*削除*" をもたらすため、この値が `.csproj` の値に表示される可能性は低いです。

    - 未設定/空の文字列: Xamarin.Android 7.1 では、これは `legacy` と同等です。  
      Xamarin.Android 7.3 では、これは `btls` と同等です。

    既定値は、空の文字列です。

    Xamarin.Android 7.1 で追加されました。

-   **AndroidLinkMode** &ndash; Android パッケージ内に含まれるアセンブリで実行する必要がある[リンク](~/android/deploy-test/linker.md)の種類を指定します。 Android アプリケーション プロジェクトでのみ使用されます。 既定値は *SdkOnly* です。 次の値を指定できます。

    - **None**: リンクは試行されません。

    - **SdkOnly**: リンクは基本クラス ライブラリでのみ実行され、ユーザーのアセンブリでは実行されません。

    - **Full**: リンクは基本クラス ライブラリとユーザーのアセンブリで実行されます。 **注:** *Full* の `AndroidLinkMode` 値を使用すると、多くの場合、特にリフレクションを使用している場合には、アプリが破損します。 何をしているかを*十分に*理解している場合を除き、使用しないでください。

    ```xml
    <AndroidLinkMode>SdkOnly</AndroidLinkMode>
    ```

-   **AndroidLinkSkip** &ndash; リンクしないアセンブリ名のセミコロン (`;`) で区切られたリストを、ファイル拡張子を使わずに指定します。 Android アプリケーション プロジェクト内でのみ使用されます。

    ```xml
    <AndroidLinkSkip>Assembly1;Assembly2</AndroidLinkSkip>
    ```

-   **AndroidManagedSymbols** &ndash; ファイル名と行番号の情報を `Release` スタック トレースから抽出できるように、シーケンス ポイントを生成するかどうかを制御するブール型プロパティ。

    Xamarin.Android 6.1 で追加されました。

-   **AndroidManifest** &ndash; アプリの [`AndroidManifest.xml`](~/android/platform/android-manifest.md) のテンプレートとして使用するファイル名を指定します。
    ビルド時に、実際の `AndroidManifest.xml` を生成するためにその他の必要な値がマージされます。
    `$(AndroidManifest)` は、`/manifest/@package` 属性にパッケージ名を含める必要があります。

-   **AndroidSdkBuildToolsVersion** &ndash; Android SDK ビルド ツール パッケージは、特に、**aapt** ツールと **zipalign** ツールを提供します。 複数の異なるバージョンのビルド ツール パッケージを同時にインストールすることができます。 パッケージ化するビルド ツール パッケージの選択は、"優先" ビルド ツールのバージョンをチェックして、ある場合はそれを使用して行われます。"優先" バージョンが "*ない*" 場合は、インストールされている最も高いバージョンのビルド ツール パッケージが使用されます。

    `$(AndroidSdkBuildToolsVersion)` MSBuild プロパティには、優先ビルド ツールのバージョンが含まれています。 Xamarin.Android ビルド システムは `Xamarin.Android.Common.targets` に既定値を提供します。たとえば、最新の aapt がクラッシュして、前のバージョンの aapt が機能することがわかっている場合には、その既定値をプロジェクト ファイル内でオーバーライドして、別のビルド ツール バージョンを選択できます。

-   **AndroidSupportedAbis** &ndash; `.apk` に含める必要がある ABI のセミコロン (`;`) で区切られたリストを含む文字列プロパティ。

    サポートされている値は次のとおりです。

    -   `armeabi`
    -   `armeabi-v7a`
    -   `x86`
    -   `arm64-v8a`: Xamarin.Android 5.1 以降が必要です。
    -   `x86_64`: Xamarin.Android 5.1 以降が必要です。

-   **AndroidUseSharedRuntime** &ndash; ターゲット デバイスでアプリケーションを実行するために*共有ランタイム パッケージ*が必要かどうかを決定するブール型プロパティ。 共有ランタイム パッケージに依存することで、アプリケーション パッケージをより小型化し、パッケージの作成と展開プロセスを高速化できるため、ビルド/配置/デバッグのターンアラウンド サイクルの高速化が結果として得られます。

    このプロパティは、デバッグ ビルドには `True`、リリース プロジェクトには `False` にする必要があります。

-   **AotAssemblies** &ndash; アセンブリをネイティブ コードに Ahead-of-Time コンパイルして、`.apk` に含めるかどうかを決定するブール型プロパティ。

    このプロパティのサポートは、Xamarin.Android 5.1 で追加されました。

    このプロパティは既定で `False` です。

-   **EmbedAssembliesIntoApk** &ndash; アプリのアセンブリをアプリケーション パッケージに埋め込むかどうかを決定するブール型プロパティ。

    このプロパティは、リリース ビルドには `True`、デバッグ ビルドには `False` にする必要があります。 高速展開でターゲット デバイスがサポートされない場合は、デバッグ ビルドでこのプロパティを `True` にする必要がある*場合があります*。

    このプロパティが `False` の場合、`$(AndroidFastDeploymentType)` MSBuild プロパティが `.apk` に埋め込まれるものも制御するため、展開および再ビルド時間に影響を及ぼす場合があります。

-   **EnableLLVM** &ndash; アセンブリをネイティブ コードに Ahead-of-Time コンパイルするときに、LLVM を使用するかどうかを決定するブール型プロパティ。

    このプロパティのサポートは、Xamarin.Android 5.1 で追加されました。

    このプロパティは既定で `False` です。

    `$(AotAssemblies)` MSBuild プロパティが `True` でない限り、このプロパティは無視されます。

-   **EnableProguard** &ndash; [ProGuard](http://developer.android.com/tools/help/proguard.html) を Java コードをリンクするパッケージ化プロセスの一部として実行するかどうかを決定するブール型プロパティ。

    このプロパティのサポートは、Xamarin.Android 5.1 で追加されました。

    このプロパティは既定で `False` です。

    `True` の場合、[ProguardConfiguration](#ProguardConfiguration) ファイルは `proguard` の実行を制御するために使用されます。

-   **JavaMaximumHeapSize** &ndash; `.dex` ファイルをパッケージ化プロセスの一部としてビルドする際に使用する **java**
    `-Xmx` パラメーター値を指定します。 指定しない場合、`-Xmx` オプションが **java** に指定されません。

    [`_CompileDex` ターゲットが `java.lang.OutOfMemoryError`](https://bugzilla.xamarin.com/show_bug.cgi?id=18327) をスローする場合には、このプロパティを指定する必要があります。

    ```xml
    <JavaMaximumHeapSize>1G</JavaMaximumHeapSize>
    ```

-   **JavaOptions** &ndash; `.dex` ファイルのビルド時に、**java** に渡す追加のコマンド ライン オプションを指定します。

-   **MandroidI18n** &ndash; 照合順序や並べ替えテーブルなど、アプリケーションに含まれる国際化サポートを指定します。 値は、次の大文字と小文字を区別しない値の 1 つ以上のコンマ区切りまたはセミコロン区切りのリストです。

    -   **None**: 追加のエンコーディングは含まれません。

    -   **All**: 利用可能なすべてのエンコーディングが含まれます。

    -   **CJK**: *日本語 (EUC)* \[enc-jp, CP51932\]、*日本語 (Shift-JIS)* \[iso-2022-jp, shift\_jis, CP932\]、*日本語 (JIS)* \[CP50220\]、*簡体字中国語 (GB2312)*\[gb2312, CP936\]、*韓国語 (UHC)* \[ks\_c\_5601-1987, CP949\]、*韓国語 (EUC)* \[euc-kr, CP51949\]、*繁体字中国語 (Big5)* \[big5, CP950\]、および*簡体字中国語 (GB18030)* \[GB18030, CP54936\] などの中国語、日本語、および韓国語のエンコーディングが含まれます。

    -   **MidEast**: *トルコ語 (Windows)* \[iso-8859-9, CP1254\]、*ヘブライ語 (Windows)* \[windows-1255, CP1255\]、*アラビア語 (Windows)* \[windows-1256, CP1256\]、*アラビア語 (ISO)* \[iso-8859-6, CP28596\]、*ヘブライ語 (ISO)* \[iso-8859-8, CP28598\]、*ラテン 5 (ISO)* \[iso-8859-9, CP28599\]、および*ヘブライ語 (Iso 代替)* \[iso-8859-8, CP38598\] などの中東のエンコーディングが含まれます。

    -   **Other**: *キリル語 (Windows)* \[CP1251\]、*バルト語 (Windows)* \[iso-8859-4, CP1257\]、*ベトナム語 (Windows)* \[CP1258\]、*キリル語 (KOI8-R)* \[koi8-r, CP1251\]、*ウクライナ語 (KOI8 U)* \[koi8-u, CP1251\]、*バルト語 (ISO)* \[iso-8859-4, CP1257\]、*キリル語 (ISO)* \[iso-8859-5, CP1251\]、 *ISCII デーヴァナーガリー語* \[x-iscii-de, CP57002\]、*ISCII ベンガル語* \[x-iscii-be, CP57003\]、*ISCII タミール語* \[x-iscii-ta, CP57004\]、*ISCII テルグ語* \[x-iscii-te, CP57005\]、*ISCII アッサム語* \[x-iscii-as, CP57006\]、*ISCII オリヤー語* \[x-iscii-or, CP57007\]、*ISCII カンナダ語* \[x-iscii-ka, CP57008\]、*ISCII マラヤーラム語* \[x-iscii-ma, CP57009\]、*ISCII グジャラート語* \[x-iscii-gu, CP57010\]、*ISCII パンジャーブ語* \[x-iscii-pa, CP57011\]、および*タイ語 (Windows)* \[CP874\] などのその他のエンコーディングが含まれます。

    -   **Rare**: *IBM EBCDIC (トルコ語)*\[CP1026\]、*IBM EBCDIC (オープン システム ラテン 1)*\[CP1047\]、*IBM EBCDIC (米国-カナダとユーロ)*\[CP1140\]、*IBM EBCDIC (ドイツとユーロ)*\[CP1141\]、*IBM EBCDIC (デンマーク/ノルウェーとユーロ)*\[CP1142\]、*IBM EBCDIC (フィンランド/スウェーデンとユーロ)*\[CP1143\]、*IBMEBCDIC (イタリアとユーロ)*\[CP1144\]、*IBM EBCDIC (ラテン アメリカ/スペインとユーロ)*\[CP1145\]、*IBM EBCDIC (イギリスとユーロ)*\[CP1146\]、*IBM EBCDIC (フランスとユーロ)*\[CP1147\]、*IBM EBCDIC (インターナショナルとユーロ)*\[CP1148\]、*IBM EBCDIC (アイスランド語とユーロ)*\[CP1149\]、*IBM EBCDIC (ドイツ)*\[CP20273\]、*IBM EBCDIC (デンマーク/ノルウェー)*\[CP20277\]、*IBM EBCDIC (フィンランド/スウェーデン)*\[CP20278\]、*IBM EBCDIC (イタリア)*\[CP20280\]、*IBM EBCDIC (ラテン アメリカ/スペイン)*\[CP20284\]、*IBM EBCDIC (イギリス)*\[CP20285\]、*IBM EBCDIC (日本語カタカナ拡張)*\[CP20290\]、*IBM EBCDIC (フランス)*\[CP20297\]、*IBM EBCDIC (アラビア語)*\[CP20420\]、*IBM EBCDIC (ヘブライ語)*\[CP20424\]、*IBM EBCDIC (アイスランド語)*\[CP20871\]、*IBM EBCDIC (キリル、セルビア語、ブルガリア語)*\[CP21025\]、*IBM EBCDIC (米国-カナダ)*\[CP37\]、 *IBM EBCDIC (インターナショナル)*\[CP500\]、*アラビア語 (ASMO 708)*\[CP708\]、*中央ヨーロッパ言語 (DOS)*\[CP852\]*, キリル言語 (DOS)*\[CP855\]、*トルコ語 (DOS)*\[CP857\]*西ヨーロッパ言語 (DOS とユーロ)*\[CP858\]、*ヘブライ語 (DOS)*\[CP862\]、*アラビア語 (DOS)*\[CP864\]、*ロシア語 (DOS)*\[CP866\]、*ギリシャ語 (DOS)*\[CP869\]、*IBM EBCDIC (ラテン 2)*\[CP870\]、*IBM EBCDIC (ギリシャ語)*\[CP875\] などのまれなエンコーディングが含まれます。

    -   **West**: *西ヨーロッパ言語 (Mac)* \[macintosh, CP10000\]、*アイスランド語 (Mac)* \[x-mac-icelandic, CP10079\]、*中央ヨーロッパ言語 (Windows)* \[iso-8859-2, CP1250\]、*西ヨーロッパ言語 (Windows)* \[iso-8859-1, CP1252\]、*ギリシャ語 (Windows)* \[iso-8859-7, CP1253\]、*中央ヨーロッパ言語 (ISO)* \[iso-8859-2, CP28592\]、*ラテン 3 (ISO)* \[iso-8859-3, CP28593\]、*ギリシャ語 (ISO)* \[iso-8859-7, CP28597\]、*ラテン 9 (ISO)* \[iso-8859-15, CP28605\]、 *OEM 米国* \[CP437\]、*西ヨーロッパ言語 (DOS)* \[CP850\]、*ポルトガル語 (DOS)* \[CP860\]、*アイスランド語 (DOS)* \[CP861\]、 *フランス語 (カナダ) (DOS)* \[CP863\]、および*北欧語 (DOS)* \[CP865\] などの欧文のエンコーディングが含まれます。


    ```xml
    <MandroidI18n>West</MandroidI18n>
    ```

-   **MonoSymbolArchive** &ndash; &ldquo;実際&rdquo;のファイル名と行番号の情報をリリース スタック トレースから抽出するため、後で `mono-symbolicate` で使用するために `.mSYM` 成果物を作成するかどうかを制御するブール型プロパティ。

    これは、デバッグ シンボルが有効 (`$(EmbedAssembliesIntoApk)` が True、`$(DebugSymbols)` が True、および `$(Optimize)` が True) になっている&ldquo;リリース&rdquo; アプリに対しては、既定で True になっています。

    Xamarin.Android 7.1 で追加されました。

-   **AndroidVersionCodePattern** &ndash; 開発者がマニフェスト内の `versionCode` をカスタマイズできるようにする文字列プロパティ。
    `versionCode` の決定に関する情報は、「[Creating the Version Code for the APK](~/android/deploy-test/building-apps/abi-specific-apks.md)」 (APK のバージョン コードの作成) を参照してください。
    
    いくつかの例では、`abi` が `armeabi` でマニフェスト内の `versionCode` が `123` の場合、`$(AndroidCreatePackagePerAbi)` が True のときには `{abi}{versionCode}` により `1123` の versionCode が生成され、それ以外のときは 123 の値が生成されます。
    `abi` が `x86_64` でマニフェスト内の `versionCode` は `44` です。 これにより、`$(AndroidCreatePackagePerAbi)` が True の場合には `544` が生成され、それ以外の場合は `44` の値が生成されます。

    `versionCode` を `0` でレフト パディングしているため、レフト パディングの書式文字列 `{abi}{versionCode:0000}` を含めると、`50044` が生成されます。 または、`{abi}{versionCode:D4}` などの 10 進パディングを使用することもできます。
    これは前の例と同じ動作になります。

    値は整数である必要があるため、'0' と 'Dx' のパディングの書式文字列のみがサポートされます。
    
    事前定義済みのキー項目

    -   **abi** &ndash; アプリのターゲットとなる abi を挿入します。
        -   1 &ndash; `armeabi`
        -   2 &ndash; `armeabi-v7a`
        -   3 &ndash; `x86`
        -   4 &ndash; `arm64-v8a`
        -   5 &ndash; `x86_64`

    -   **minSDK** &ndash; `AndroidManifest.xml` または `11` (定義されていない場合) からサポートされる Sdk の最小値を挿入します。

    -   **versionCode** &ndash; `Properties\AndroidManifest.xml` から直接バージョン コードを使用します。 

    `$(AndroidVersionCodeProperties)` プロパティ (次で定義) を使用してカスタム項目を定義することができます。

    既定では、値は `{abi}{versionCode:D6}` に設定されます。 開発者が古い動作を保持する必要がある場合は、`$(AndroidUseLegacyVersionCode)` プロパティを `true` に設定することで既定値をオーバーライドできます。

    Xamarin.Android 7.2 で追加されました。

-   **AndroidVersionCodeProperties** &ndash; `AndroidVersionCodePattern` で使用するために、開発者がカスタム項目を定義できるようにする文字列プロパティ。 これらは `key=value` ペアの形式です。 `value` 内のすべての項目は整数値である必要があります。 たとえば、`screen=23;target=$(_SupportedApiLevel)` のように指定します。 ご覧のとおり、既存またはカスタムの MSBuild プロパティを文字列で利用することができます。

    Xamarin.Android 7.2 で追加されました。

-   **AndroidUseLegacyVersionCode** &ndash; 開発者が versionCode の計算を Xamarin.Android 8.2 前の動作に戻すことを許可するブール型プロパティ。 これは、Google Play ストアに既存のアプリケーションがある開発者に向けてのみ使用する必要があります。 新しい `$(AndroidVersionCodePattern)` プロパティを使用することを強くお勧めします。

    Xamarin.Android 8.2 で追加されました。

-  **AndroidUseManagedDesignTimeResourceGenerator**&ndash;デザイン時のビルドを、`aapt` ではなくマネージド リソース パーサーの使用に切り替えるブール型プロパティ。

    Xamarin.Android 8.1 で追加されました。

-  **AndroidUseApkSigner** &ndash; `jarsigner` ではなく `apksigner` ツールを使用することを開発者に許可するブール型プロパティ。

    Xamarin.Android 8.2 で追加されました。

-  **AndroidApkSignerAdditionalArguments** &ndash; 開発者が `apksigner` ツールに追加の引数を指定することを許可する文字列プロパティ。

    Xamarin.Android 8.2 で追加されました。

### <a name="binding-project-build-properties"></a>プロジェクトのビルド プロパティをバインドする

次の MSBuild プロパティは、[バインド プロジェクト](~/android/platform/binding-java-library/index.md)で使用されます。

-   **AndroidClassParser** &ndash; `.jar` ファイルの解析方法を制御する文字列プロパティ。 次の値を使用できます。

    - **class-parse**: JVM を利用せずに、`class-parse.exe` を使用して直接 Java バイトコードを解析します。 この値は試験的です。 


    - **jar2xml**: Java リフレクションを使用して `.jar` ファイルから型とメンバーを抽出するには、`jar2xml.jar` を使用します。

    `jar2xml` よりも優れている `class-parse` の利点は次のとおりです。

    - `class-parse` は、*デバッグ* シンボル (`javac -g` でコンパイルされたバイトコードなど) を含む Java バイトコードからパラメーター名を抽出できます。

    - `class-parse` は、解決できない型のメンバーから継承するクラスやそのようなメンバーが含まれるクラスを "スキップ" しません。

    **試験的**です。 Xamarin.Android 6.0 で追加されました。

    既定値は `jar2xml` です。

    既定値は、将来のリリースで変更されます。

-   **AndroidCodegenTarget** &ndash; コード生成ターゲット ABI を制御する文字列型プロパティ。 次の値を使用できます。

    - **XamarinAndroid**: Mono for Android 1.0 以降に付属している JNI バインド API を使用します。 Xamarin.Android 5.0 以降でビルドされたバインドのアセンブリは、Xamarin.Android 5.0 以降 (API/ABI 追加機能) でないと実行できませんが、*ソース*は前の製品バージョンと互換性があります。

    - **XAJavaInterop1**: JNI の呼び出しに Java.Interop を使用します。 `XAJavaInterop1` を使用したバインドのアセンブリは、Xamarin.Android 6.1 以降でのみビルドおよび実行できます。 Xamarin.Android 6.1 以降は、この値で `Mono.Android.dll` をバインドします。

      `XAJavaInterop1` には次の利点があります。

      - より小さなアセンブリ。

      - 継承階層の他のバインドの種類がすべて `XAJavaInterop1` 以降でビルドされる限り、`base` メソッドの呼び出しに `jmethodID` キャッシュを使用。

      - マネージド サブクラスに対して Java 呼び出し可能ラッパー コンストラクターに `jmethodID` キャッシュを使用。

    既定値は `XamarinAndroid` です。

    既定値は、将来のリリースで変更されます。


### <a name="resource-properties"></a>リソースのプロパティ

リソースのプロパティは、Android ソースへのアクセスを提供する、`Resource.designer.cs` ファイルの生成を制御します。 

-   **AndroidResgenExtraArgs** &ndash; Android アセットとリソースを処理するときに、**aapt** コマンドに渡す追加のコマンド ライン オプションを指定します。

-   **AndroidResgenFile** &ndash; 生成するリソース ファイルの名前を指定します。 既定のテンプレートでは、これは `Resource.designer.cs` に設定されます。

-   **MonoAndroidResourcePrefix** &ndash; `AndroidResource` のビルド アクションで、ファイル名の先頭から削除される*パス プレフィックス*を指定します。 これにより、リソースがある場所を変更することができます。

    既定値は `Resources` です。 Java プロジェクトの構造には、これを `res` に変更します。

-   **AndroidExplicitCrunch** &ndash; ローカル ドローアブルの数が非常に多いアプリをビルドする場合、初期のビルド (または再ビルド) の完了に数分かかる場合があります。 ビルド プロセスを高速化するため、このプロパティを含めて、`True` に設定してみます。 このプロパティを設定すると、ビルド プロセスで .png ファイルが事前クランチされます。

    **試験的**です。 Xamarin.Android 7.0 で追加されました。


<a name="Signing_Properties" />

### <a name="signing-properties"></a>署名プロパティ

署名プロパティは、アプリケーション パッケージを Android デバイスにインストールできるように、パッケージに署名する方法を制御します。 ビルド イテレーションを高速化するため、Xamarin.Android タスクではビルド プロセス中のパッケージへの署名は行われません。署名を行うと非常に遅くなるからです。 代わりに、インストール前またはエクスポート中に、IDE または *Install* ビルド ターゲットによって署名されます (必要な場合)。 *SignAndroidPackage* ターゲットを呼び出すと、出力ディレクトリに `-Signed.apk` のサフィックスを持つパッケージが生成されます。

既定では、必要に応じて署名ターゲットによって新しいデバッグ署名キーが生成されます。 たとえばビルド サーバーなどで特定のキーを使用する場合は、次の MSBuild プロパティが使用できます。

-   **AndroidKeyStore** &ndash; カスタム署名情報を使用するかどうかを示すブール値。 既定値は `False` で、既定のデバッグ署名キーがパッケージの署名に使用されることを意味します。

-   **AndroidSigningKeyAlias** &ndash; キーストア内のキーに別名を指定します。 これは、キーストアを作成するときに使用される **keytool -alias** 値です。 

-   **AndroidSigningKeyPass** &ndash;キーストア ファイル内にあるキーのパスワードを指定します。 これは、`keytool` が **Enter key password for $(AndroidSigningKeyAlias)** ($(AndroidSigningKeyAlias) のキーのパスワードを入力) を求めたときに入力される値です。

-   **AndroidSigningKeyStore** &ndash; `keytool` で作成されたキーストア ファイルのファイル名を指定します。 これは、**keytool -keystore** オプションで指定された値に対応します。

-   **AndroidSigningStorePass** &ndash; `$(AndroidSigningKeyStore)` へのパスワードを指定します。 これは、キーストア ファイルを作成していて、**Enter keystore password:** (キーストア パスワードを入力) で求められたときに、`keytool` に指定する値です。 

たとえば、次の `keytool` 呼び出しを考えてみます。

```shell
$ keytool -genkey -v -keystore filename.keystore -alias keystore.alias -keyalg RSA -keysize 2048 -validity 10000
Enter keystore password: keystore.filename password
Re-enter new password: keystore.filename password
...
Is CN=... correct?
  [no]:  yes

Generating 2,048 bit RSA key pair and self-signed certificate (SHA1withRSA) with a validity of 10,000 days
        for: ...
Enter key password for keystore.alias
        (RETURN if same as keystore password): keystore.alias password
[Storing filename.keystore]
```

上記で生成されたキーストアを使用するには、プロパティ グループを使用します。

```xml
<PropertyGroup>
    <AndroidKeyStore>True</AndroidKeyStore>
    <AndroidSigningKeyStore>filename.keystore</AndroidSigningKeyStore>
    <AndroidSigningStorePass>keystore.filename password</AndroidSigningStorePass>
    <AndroidSigningKeyAlias>keystore.alias</AndroidSigningKeyAlias>
    <AndroidSigningKeyPass>keystore.alias password</AndroidSigningKeyPass>
</PropertyGroup>
```

-   **AndroidDebugKeyAlgorithm** &ndash; `debug.keystore` 用に使用する既定のアルゴリズムを指定します。 既定値は `RSA` です。

-   **AndroidDebugKeyValidity** &ndash; `debug.keystore` 用に使用する既定の有効性を指定します。 既定値は `10950`、`30 * 365`、または `30 years` です。

<a name="Build_Actions" />

## <a name="build-actions"></a>ビルド アクション

*ビルド アクション*は、プロジェクト内の[ファイルに適用](https://docs.microsoft.com/visualstudio/msbuild/common-msbuild-project-items)され、ファイルの処理方法を制御します。 

<a name="AndroidEnvironment" />

### <a name="androidenvironment"></a>AndroidEnvironment

`AndroidEnvironment` のビルド アクションを持つファイルは、[プロセスの起動時に環境変数とシステム プロパティを初期化する](~/android/deploy-test/environment.md)ために使用されます。
`AndroidEnvironment` ビルド アクションは、複数のファイルに適用でき、任意の順序で評価されます (そのため、複数のファイルで同じ環境変数またはシステム プロパティを指定しないでください)。


### <a name="androidjavasource"></a>AndroidJavaSource

`AndroidJavaSource` のビルド アクションを持つファイルは、最終的な Android パッケージに含まれる Java ソース コードです。


### <a name="androidjavalibrary"></a>AndroidJavaLibrary

`AndroidJavaLibrary` のビルド アクションを持つファイルは、最終的な Android パッケージに含まれる Java アーカイブ (`.jar` ファイル) です。


### <a name="androidresource"></a>AndroidResource

*AndroidResource* ビルド アクションを持つすべてのファイルは、ビルド プロセス中に Android リソースにコンパイルされ、`$(AndroidResgenFile)` を介してアクセスできます。

```xml
<ItemGroup>
  <AndroidResource Include="Resources\values\strings.xml" />
</ItemGroup>
```

上級ユーザーであれば、同じ効果的なパスを使用して、別の構成で使用されている別のリソースを使用したいと考えるかもしれません。 これは、複数のリソース ディレクトリとファイルにこれらの異なるディレクトリ内で同じ相対パスを持たせ、MSBuild 条件を使用して、異なる構成の異なるファイルを条件付きで含めることで実現できます。 例:

```xml
<ItemGroup Condition="'$(Configuration)'!='Debug'">
  <AndroidResource Include="Resources\values\strings.xml" />
</ItemGroup>
<ItemGroup  Condition="'$(Configuration)'=='Debug'">
  <AndroidResource Include="Resources-Debug\values\strings.xml"/>
</ItemGroup>
<PropertyGroup>
  <MonoAndroidResourcePrefix>Resources;Resources-Debug<MonoAndroidResourcePrefix>
</PropertyGroup>
```

**LogicalName** &ndash; リソース パスを明示的に指定します。 ファイルの&ldquo;別名定義&rdquo;を許可して、複数の個別のリソース名として使用できるようにします。

```xml
<ItemGroup Condition="'$(Configuration)'!='Debug'">
  <AndroidResource Include="Resources/values/strings.xml"/>
</ItemGroup>
<ItemGroup Condition="'$(Configuration)'=='Debug'">
  <AndroidResource Include="Resources-Debug/values/strings.xml">
    <LogicalName>values/strings.xml</LogicalName>
  </AndroidResource>
</ItemGroup>
```


### <a name="androidnativelibrary"></a>AndroidNativeLibrary

[ネイティブ ライブラリ](~/android/platform/native-libraries.md)は、そのビルド アクションを `AndroidNativeLibrary` に設定することで、ビルドに追加されます。

Android では、複数のアプリケーション バイナリ インターフェイス (ABI) をサポートしているため、ビルド システムではネイティブ ライブラリがどの ABI に対してビルドされているかを知る必要があります。 これを行うには 2 つの方法があります。

1.  パス "スニッフィング"。
2.  `Abi` 項目属性の使用。

パス スニッフィングを使用すると、ネイティブ ライブラリの親ディレクトリ名が、ライブラリがターゲットとする ABI を指定するために使用されます。 したがって、ビルドに `lib/armeabi/libfoo.so` を追加すると、ABI は `armeabi` として "スニッフィング" されます。 


#### <a name="item-attribute-name"></a>項目属性名

**Abi** &ndash; ネイティブ ライブラリの ABI を指定します。

```xml
<ItemGroup>
  <AndroidNativeLibrary Include="path/to/libfoo.so">
    <Abi>armeabi</Abi>
  </AndroidNativeLibrary>
</ItemGroup>
```


### <a name="androidaarlibrary"></a>AndroidAarLibrary

.aar ファイルを直接参照するには、`AndroidAarLibrary` のビルド アクションを使用する必要があります。 このビルド アクションは、Xamarin コンポーネントで最もよく使用されます。 つまり、Google Play やその他のサービスを動作させるのに必要な .aar ファイルへの参照を含めるためです。

このビルド アクションを使用するファイルは、ライブラリ プロジェクトで見つかる埋め込みリソースと同様に扱われます。 .aar は、中間ディレクトリに抽出されます。 次に、すべてのアセット、リソース、.jar ファイルが、適切な項目グループに含まれます。  

### <a name="content"></a>Content

通常の `Content` ビルド アクションはサポートされていません (コストのかかる可能性がある最初の実行手順を行わずにサポートする方法が見つかっていないからです)。

Xamarin.Android 5.1 以降では、`@(Content)` ビルド アクションを使用しようとすると、`XA0101` 警告が発生します。

### <a name="linkdescription"></a>LinkDescription

*LinkDescription* ビルド アクションを持つファイルは、[リンカーの動作を制御](~/cross-platform/deploy-test/linker.md)するために使用されます。


<a name="ProguardConfiguration" />

### <a name="proguardconfiguration"></a>ProguardConfiguration

*ProguardConfiguration* ビルド アクションを持つファイルは、`proguard` 動作を制御するために使用されます。 このビルド アクションの詳細については、「[ProGuard](~/android/deploy-test/release-prep/proguard.md)」を参照してください。

`$(EnableProguard)` MSBuild プロパティが `True` でない限り、これらのファイルは無視されます。


## <a name="target-definitions"></a>ターゲットの定義

ビルド プロセスの Xamarin.Android に固有の部分は、`$(MSBuildExtensionsPath)\Xamarin\Android\Xamarin.Android.CSharp.targets` で定義されますが、アセンブリをビルドするためには、*Microsoft.CSharp.targets* などの通常の言語固有ターゲットも必要です。

次のビルド プロパティは、ターゲットの言語をインポートする前に設定する必要があります。

```xml
<PropertyGroup>
  <TargetFrameworkIdentifier>MonoDroid</TargetFrameworkIdentifier>
  <MonoDroidVersion>v1.0</MonoDroidVersion>
  <TargetFrameworkVersion>v2.2</TargetFrameworkVersion>
</PropertyGroup>
```

*Xamarin.Android.CSharp.targets* をインポートすることで、C# 用にこれらすべてのターゲットとプロパティを含めることができます。 

```xml
<Import Project="$(MSBuildExtensionsPath)\Xamarin\Android\Xamarin.Android.CSharp.targets" />
```

このファイルは、他の言語に簡単に適合させることができます。
