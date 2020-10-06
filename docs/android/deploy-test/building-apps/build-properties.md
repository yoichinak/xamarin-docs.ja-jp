---
title: ビルド プロパティ
description: このドキュメントでは、Xamarin.Android ビルド プロセスでサポートされているすべてのプロパティを一覧します。
ms.prod: xamarin
ms.assetid: FC0DBC08-EBCB-4D2D-AB3F-76B54E635C22
ms.technology: xamarin-android
author: jonpryor
ms.author: jopryo
ms.date: 09/21/2020
ms.openlocfilehash: aeb0cca9ead1a0f0a3f5b1dec88b2470289cd589
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91454937"
---
# <a name="build-properties"></a>ビルド プロパティ

MSBuild プロパティは、[ターゲット](~/android/deploy-test/building-apps/build-targets.md)の動作を制御します。
これらは、[MSBuild PropertyGroup](/visualstudio/msbuild/propertygroup-element-msbuild) 内のプロジェクト ファイル (**MyApp.csproj** など) 内で指定されます。

## <a name="adbtarget"></a>AdbTarget

`$(AdbTarget)` プロパティは、Android パッケージのインストール先または削除元となる Android ターゲット デバイスを指定します。
このプロパティの値は、[`adb` ターゲット デバイス オプション](https://developer.android.com/tools/help/adb.html#issuingcommands)と同じです。

## <a name="aftergenerateandroidmanifest"></a>AfterGenerateAndroidManifest

このプロパティに一覧表示された MSBuild ターゲットは、`$(IntermediateOutputPath)` で `AndroidManifest.xml` ファイルが生成される場所である内部の `_GenerateJavaStubs` ターゲットの直後に実行されます。 生成された `AndroidManifest.xml` ファイルに変更を加える場合、この拡張ポイントを使用して行うことができます。

Xamarin.Android 9.4 で追加されました。

## <a name="androidaapt2compileextraargs"></a>AndroidAapt2CompileExtraArgs

Android アセットとリソースを処理するときに、**aapt2 compile** コマンドに渡す追加のコマンド ライン オプションを指定します。

Xamarin.Android 9.1 で追加されました。

## <a name="androidaapt2linkextraargs"></a>AndroidAapt2LinkExtraArgs

Android アセットとリソースを処理するときに、**aapt2 link** コマンドに渡す追加のコマンド ライン オプションを指定します。

Xamarin.Android 9.1 で追加されました。

## <a name="androidaotcustomprofilepath"></a>AndroidAotCustomProfilePath

プロファイラー データを保持するために `aprofutil` が作成する必要があるファイル。

## <a name="androidaotprofiles"></a>AndroidAotProfiles

開発者がコマンド ラインから AOT プロファイルを追加できるようにする文字列プロパティ。 セミコロンまたはコンマで区切られた絶対パスの一覧です。
Xamarin.Android 10.1 で追加されました。

## <a name="androidaotprofilerport"></a>AndroidAotProfilerPort

プロファイル データを取得するときに `aprofutil` が接続する必要があるポート。

## <a name="androidapkdigestalgorithm"></a>AndroidApkDigestAlgorithm

`jarsigner -digestalg` で使用するダイジェスト アルゴリズムを指定する文字列値。

既定値は `SHA-256` です。 Xamarin.Android 10.0 以前では、既定値は `SHA1` でした。

Xamarin.Android 9.4 で追加されました。

## <a name="androidapksigneradditionalarguments"></a>AndroidApkSignerAdditionalArguments

開発者が `apksigner` ツールに追加の引数を指定することを許可する文字列プロパティ。

Xamarin.Android 8.2 で追加されました。

## <a name="androidapksigningalgorithm"></a>AndroidApkSigningAlgorithm

`jarsigner -sigalg` で使用する署名アルゴリズムを指定する文字列値。

既定値は `SHA256withRSA` です。 Xamarin.Android 10.0 以前では、既定値は `md5withRSA` でした。

Xamarin.Android 8.2 で追加されました。

## <a name="androidapplication"></a>AndroidApplication

プロジェクトが Android アプリケーション用 (`True`) か、または Android ライブラリ プロジェクト用 (`False` または存在しない) かを示すブール値。

`<AndroidApplication>True</AndroidApplication>` を持つプロジェクトは、Android パッケージ内に 1 つしか存在できない場合があります (残念ながら、これについてはまだ検証されていません。Android リソースに関するわかりにくいおかしなエラーになることがあります)。

## <a name="androidapplicationjavaclass"></a>AndroidApplicationJavaClass

[Android.App.Application](xref:Android.App.Application) からクラスを継承するときに、`android.app.Application` の代わりに使用する完全な Java クラス名。

このプロパティは、通常、`$(AndroidEnableMultiDex)` MSBuild プロパティなどの*他の*プロパティで設定されます。

Xamarin.Android 6.1 で追加されました。

## <a name="androidbinutilspath"></a>AndroidBinUtilsPath

`ld`、ネイティブ リンカー、`as`、ネイティブ アセンブラーなどの Android [binutils][binutils] が格納されているディレクトリへのパス。 これらのツールは Android NDK に含まれており、Xamarin.Android のインストールにも含まれています。

既定値は `$(MonoAndroidBinDirectory)\ndk\` です。

Xamarin.Android 10.0 で追加されました。

[binutils]: https://android.googlesource.com/toolchain/binutils/

## <a name="androidboundexceptiontype"></a>AndroidBoundExceptionType

Xamarin.Android が提供する型によって Java 型の観点から .NET の型またはインターフェイスが実装される場合に (たとえば、`Android.Runtime.InputStreamInvoker` と `System.IO.Stream` または `Android.Runtime.JavaDictionary` と `System.Collections.IDictionary`)、例外を伝播する方法を指定する文字列値。

- `Java`:元の Java 例外の型はそのまま伝播されます。

  これは、たとえば、`InputStreamInvoker` が `System.IO.Stream` API を適切に実装していないことを意味します。これは、`Stream.Read()` から`System.IO.IOException` ではなく `Java.IO.IOException` がスローされる可能性があるためです。

  これは、11.1 より前の Xamarin.Android のすべてのリリースでの例外伝播の動作です。

  これは、Xamarin.Android 11.1 の既定値です。

- `System`:元の Java 例外の型がキャッチされ、適切な .NET 例外の型でラップされます。

  これは、たとえば、`InputStreamInvoker` が `System.IO.Stream` を正しく実装し、`Stream.Read()` が `Java.IO.IOException` インスタンスをスロー "*しない*" ことを意味します。  (代わりに、`Exception.InnerException` の値として `Java.IO.IOException` を持つ `System.IO.IOException` をスローする場合があります)。

  これは、.NET 6.0 で既定値になります。

Xamarin.Android 10.2 で追加されました。

## <a name="androidbuildapplicationpackage"></a>AndroidBuildApplicationPackage

パッケージ (.apk) を作成して署名するかどうかを示すブール値。 この値を `True` に設定することは、[`SignAndroidPackage`](~/android/deploy-test/building-apps/build-targets.md#install)
ビルド ターゲットを使用することと同じです。

このプロパティのサポートは、Xamarin.Android 7.1 以降で追加されました。

このプロパティは既定で `False` です。

## <a name="androidbundleconfigurationfile"></a>AndroidBundleConfigurationFile

Android アプリ バンドルをビルドするときに `bundletool` の[構成ファイル][bundle-config-format]として使用するファイル名を指定します。 このファイルは、APK を生成するためのどのディメンションでバンドルを分割するかなど、バンドルから APK を生成する方法のいくつかの側面を制御します。 Xamarin.Android では、圧縮しないままにするファイル拡張子の一覧など、これらの設定の一部が自動的に構成されることに注意してください。

このプロパティが意味をなすのは、[`$(AndroidPackageFormat)`](#androidpackageformat) が `aab` に設定されている場合のみです。

Xamarin.Android 10.3 で追加されました。

[bundle-config-format]: https://developer.android.com/studio/build/building-cmdline#bundleconfig

## <a name="androidclassparser"></a>AndroidClassParser

`.jar` ファイルの解析方法を制御する文字列プロパティ。 次の値を指定できます。

- **class-parse**: JVM を利用せずに、`class-parse.exe` を使用して直接 Java バイトコードを解析します。 この値は試験的です。

- **jar2xml**: Java リフレクションを使用して `.jar` ファイルから型とメンバーを抽出するには、`jar2xml.jar` を使用します。

`jar2xml` よりも優れている `class-parse` の利点は次のとおりです。

- `class-parse` は、"*デバッグ*" シンボル (`javac -g` でコンパイルされたバイトコード) を含む Java バイトコードからパラメーター名を抽出できます。

- `class-parse` は、解決できない型のメンバーから継承するクラスやそのようなメンバーが含まれるクラスを "スキップ" しません。

**試験的**です。 Xamarin.Android 6.0 で追加されました。

既定値は `jar2xml` です。

`jar2xml` のサポートは廃止されており、.NET 6 の一環として `jar2xml` のサポートは削除されます。

## <a name="androidcodegentarget"></a>AndroidCodegenTarget

コード生成ターゲット ABI を制御する文字列プロパティ。
次の値を指定できます。

- **XamarinAndroid**: Mono for Android 1.0 以降に付属している JNI バインド API を使用します。 Xamarin.Android 5.0 以降でビルドされたバインドのアセンブリは、Xamarin.Android 5.0 以降 (API/ABI 追加機能) でないと実行できませんが、*ソース*は前の製品バージョンと互換性があります。

- **XAJavaInterop1**: JNI の呼び出しに Java.Interop を使用します。 `XAJavaInterop1` を使用したバインドのアセンブリは、Xamarin.Android 6.1 以降でのみビルドおよび実行できます。 Xamarin.Android 6.1 以降は、この値で `Mono.Android.dll` をバインドします。

`XAJavaInterop1` には次の利点があります。

- より小さなアセンブリ。

- 継承階層の他のバインドの種類がすべて `XAJavaInterop1` 以降でビルドされる限り、`base` メソッドの呼び出しに `jmethodID` キャッシュを使用。

- マネージド サブクラスに対して Java 呼び出し可能ラッパー コンストラクターに `jmethodID` キャッシュを使用。

既定値は `XAJavaInterop1` です。

## <a name="androidcreatepackageperabi"></a>AndroidCreatePackagePerAbi

1 つの `.apk` ですべての ABI をサポートするのでなく、([`$(AndroidSupportedAbis)`](#androidsupportedabis) で指定された ABI ごとに) ファイルの "*セット*" を作成する必要があるかどうかを決定するブール プロパティ。

「[ABI 固有の APK のビルド](~/android/deploy-test/building-apps/abi-specific-apks.md)」ガイドも参照してください。

## <a name="androiddebugkeyalgorithm"></a>AndroidDebugKeyAlgorithm

`debug.keystore` 用に使用する既定のアルゴリズムを指定します。 既定値は `RSA` です。

## <a name="androiddebugkeyvalidity"></a>AndroidDebugKeyValidity

`debug.keystore` 用に使用する既定の有効性を指定します。 既定値は `10950`、`30 * 365`、または `30 years` です。

## <a name="androiddebugstoretype"></a>AndroidDebugStoreType

`debug.keystore` に使用するキー ストア ファイル形式を指定します。 既定値は `pkcs12` です。

Xamarin.Android 10.2 で追加されました。

## <a name="androiddextool"></a>AndroidDexTool

`dx` または `d8` の有効な値の列挙方式のプロパティ。 Xamarin.Android のビルド プロセス中に使用される Android の [dex][dex] コンパイラを示します。
現在の既定値は `dx` です。 詳細については、[D8 と R8][d8-r8] に関するドキュメントをご覧ください。

[dex]: https://source.android.com/devices/tech/dalvik/dalvik-bytecode
[d8-r8]: https://github.com/xamarin/xamarin-android/blob/master/Documentation/guides/D8andR8.md

## <a name="androidenabledesugar"></a>AndroidEnableDesugar

`desugar` が有効かどうかを決定するブール型プロパティ。 現在、Android ではすべての Java 8 機能がサポートされておらず、`javac` コンパイラの出力に `desugar` と呼ばれるバイトコード変換を実行して、既定のツールチェーンにより新しい言語機能が実装されています。 `AndroidDexTool=dx` を使用している場合の既定値は `False`、[`$(AndroidDexTool)`](#androiddextool)=`d8` を使用している場合の既定値は `True` です。

## <a name="androidenablegoogleplaystorechecks"></a>AndroidEnableGooglePlayStoreChecks

開発者が次の Google Play ストア チェックを無効にできるようにするブール プロパティ: XA1004、XA1005、XA1006。 これは、Google Play ストアを対象としておらず、このチェックを実行したくない開発者にとって役立ちます。

Xamarin.Android 9.4 で追加されました。

## <a name="androidenablemultidex"></a>AndroidEnableMultiDex

最終的な `.apk` で Multi-Dex サポートを使用するかどうかを決定するブール型プロパティ。

このプロパティのサポートは、Xamarin.Android 5.1 で追加されました。

このプロパティは既定で `False` です。

## <a name="androidenablepreloadassemblies"></a>AndroidEnablePreloadAssemblies

アプリケーション パッケージ内でバンドルされているすべてのマネージド アセンブリを、プロセスの起動中に読み込むかどうかを制御するブール型プロパティ。

`True` に設定すると、アプリケーション コードが呼び出される前に、アプリケーション パッケージ内でバンドルされているすべてのアセンブリがプロセスの起動中に読み込まれます。
これは、Xamarin.Android 9.2 より前のリリースの Xamarin.Android の動作と一致します。

`False` に設定すると、アセンブリは必要な場合にのみ読み込まれます。
これにより、アプリケーションの起動がより高速になり、デスクトップの .NET セマンティクスとの整合性も高まります。  時間の短縮を確認したい場合は、`timing` を含めるよう `debug.mono.log` システム プロパティを設定して、`adb logcat` 内で `Finished loading assemblies: preloaded` メッセージを探します。

依存関係の挿入を使うアプリケーションまたはライブラリでは、それらが同じように `AppDomain.CurrentDomain.GetAssemblies()` でアプリケーション バンドル内のすべてのアセンブリを返すことが必要な場合、それ以外ではアセンブリが必要なかった場合でも、このプロパティが `True` になることを "*必要とする*" 場合があります。

既定では、この値は `True` に設定されます。

Xamarin.Android 9.2 で追加されました。

## <a name="androidenableprofiledaot"></a>AndroidEnableProfiledAot

Ahead-of-Time コンパイル中に AOT プロファイルを使用するかどうかを決定するブール型プロパティ。

該当するプロファイルは、[`@(AndroidAotProfile)`](~/android/deploy-test/building-apps/build-items.md#androidaotprofile)
項目グループに一覧表示されます。 この項目グループには、既定のプロファイルが含まれています。 既存のものを削除し、独自の AOT プロファイルを追加することで上書きできます。

このプロパティのサポートは、Xamarin.Android 9.4 で追加されました。

このプロパティは既定で `False` です。

## <a name="androidenablesgenconcurrent"></a>AndroidEnableSGenConcurrent

Mono の[同時 GC コレクター](https://www.mono-project.com/docs/about-mono/releases/4.8.0/#concurrent-sgen)を使用するかどうかを決定するブール型プロパティ。

このプロパティのサポートは、Xamarin.Android 7.2 で追加されました。

このプロパティは既定で `False` です。

## <a name="androiderroroncustomjavaobject"></a>AndroidErrorOnCustomJavaObject

`Java.Lang.Object` または `Java.Lang.Throwable` を継承 "*することなく*"、型が `Android.Runtime.IJavaObject`
 を実装するかどうかを決定するブール型プロパティ。

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

## <a name="androidexplicitcrunch"></a>AndroidExplicitCrunch

Xamarin.Android 11.0 ではサポートされなくなりました。

## <a name="androidextraaotoptions"></a>AndroidExtraAotOptions

[`$(AndroidEnableProfiledAot)`](#androidenableprofiledaot) または [`$(AotAssemblies)`](#aotassemblies) が `true` に設定されているプロジェクトの `Aot` タスク中に、Mono コンパイラに追加のオプションを渡すことができるようにする文字列プロパティ。
Mono クロスコンパイラを呼び出すときに、プロパティの文字列値が応答ファイルに追加されます。

一般に、このプロパティは空白のままにしておく必要がありますが、特殊なシナリオでは、柔軟性が向上する場合があります。

このプロパティは、関連する `$(AndroidAotAdditionalArguments)` プロパティとは異なることに注意してください。 このプロパティは、コンマ区切りの引数を Mono コンパイラの `--aot` オプションに配置します。 `$(AndroidExtraAotOptions)` では代わりに、`--verbose` や `--debug` など、完全なスタンドアロンのスペース区切りオプションがコンパイラに渡されます。

Xamarin.Android 10.2 で追加されました。

## <a name="androidfastdeploymenttype"></a>AndroidFastDeploymentType

[`$(EmbedAssembliesIntoApk)`](#embedassembliesintoapk) MSBuild プロパティが `False` の場合に、ターゲット デバイスの[高速展開ディレクトリ](~/android/deploy-test/building-apps/build-process.md#Fast_Deployment)に展開できる型を制御する値の `:` (コロン) 区切りのリスト。 リソースが高速展開される場合、そのリソースが生成された `.apk` に埋め込まれ*ない*ため、展開時間を短縮することができます (高速展開が増えるほど、`.apk` を再ビルドする頻度が減り、インストール プロセスを高速化できます)。有効な値を次に示します。

- `Assemblies`:アプリケーション アセンブリを展開します。

- `Dexes`:`.dex` ファイル、Android リソース、および Android アセットを展開します。 **この値は、Android 4.4 以降 (API-19) を実行しているデバイスで*のみ*使用できます。**

既定値は `Assemblies` です。

**試験的**です。 Xamarin.Android 6.1 で追加されました。

## <a name="androidgeneratejnimarshalmethods"></a>AndroidGenerateJniMarshalMethods

ビルド プロセスの一環として、JNI マーシャリング メソッドの生成を有効にするブール型プロパティ。 これにより、バインディング ヘルパー コードでの `System.Reflection` の使用量が大幅に削減されます。

既定では、これは False に設定されます。 開発者が新しい JNI マーシャリング メソッド機能を使用する場合、

```xml
<AndroidGenerateJniMarshalMethods>True</AndroidGenerateJniMarshalMethods>
```

該当する `.csproj` 内に上の内容を設定できます。 または、コマンド ラインでプロパティを指定します。

```shell
/p:AndroidGenerateJniMarshalMethods=True
```

**試験的**です。 Xamarin.Android 9.2 で追加されました。
既定値は False です。

## <a name="androidgeneratejnimarshalmethodsadditionalarguments"></a>AndroidGenerateJniMarshalMethodsAdditionalArguments

`jnimarshalmethod-gen.exe` の呼び出しにさらにパラメーターを追加するために使用できる文字列プロパティ。  これはデバッグに役立つため、オプション (`-v`、`-d`、`--keeptemp` など) を使用できます。

既定値は、空の文字列です。 これは、`.csproj` ファイルまたはコマンド ラインで設定できます。 次に例を示します。

```xml
<AndroidGenerateJniMarshalMethodsAdditionalArguments>-v -d --keeptemp</AndroidGenerateJniMarshalMethodsAdditionalArguments>
```

または

```shell
/p:AndroidGenerateJniMarshalMethodsAdditionalArguments="-v -d --keeptemp"
```

Xamarin.Android 9.2 で追加されました。

## <a name="androidgeneratelayoutbindings"></a>AndroidGenerateLayoutBindings

`true` に設定されている場合は[レイアウトのコードビハインド](https://github.com/xamarin/xamarin-android/blob/master/Documentation/guides/LayoutCodeBehind.md)の生成を有効にし、`false` に設定されている場合は完全に無効にします。 既定値は `false` です。

## <a name="androidhttpclienthandlertype"></a>AndroidHttpClientHandlerType

既定の `System.Net.Http.HttpClient` コンストラクターによって使用される、既定の `System.Net.Http.HttpMessageHandler` の実装を制御します。 値は `HttpMessageHandler` サブクラスのアセンブリ修飾型名であり、[`System.Type.GetType(string)`](/dotnet/api/system.type.gettype#System_Type_GetType_System_String_) での使用に適しています。
このプロパティでは次の値が最も一般的です。

- `Xamarin.Android.Net.AndroidClientHandler`:Android Java API を使用して、ネットワーク要求を実行します。 これにより、基になる Android バージョンが TLS 1.2 をサポートする場合、TLS 1.2 の URL にアクセスできます。 TLS 1.2 のサポートが Java を通じて確実に提供されるのは、Android 5.0 以降のみです。

  これは、Visual Studio のプロパティページの **Android** オプションと、Visual Studio for Mac プロパティ ページの **AndroidClientHandler** オプションに対応しています。

  Visual Studio で **[最低限の Android バージョン]** が **[Android 5.0 (Lollipop)]** 以上に構成されている、または Visual Studio for Mac で **[ターゲット プラットフォーム]** が **[最新および最高]** に設定されている場合、新しいプロジェクトのウィザードで、新しいプロジェクトに対してこのオプションが選択されます。

- 空の文字列を設定解除します。これは、`System.Net.Http.HttpClientHandler, System.Net.Http` と同じです

  これは、Visual Studio のプロパティ ページの**既定**オプションに対応しています。

  Visual Studio で **[最低限の Android バージョン]** が **[Android 4.4.87]** 以下に構成されている、または Visual Studio for Mac で **[ターゲット プラットフォーム]** が **[最新の開発]** または **[最大の互換性]** に設定されている場合、新しいプロジェクトのウィザードで、新しいプロジェクトに対してこのオプションが選択されます。

- `System.Net.Http.HttpClientHandler, System.Net.Http`:マネージド `HttpMessageHandler` を使用します。

  これは、Visual Studio のプロパティ ページの**マネージド**オプションに対応しています。

> [!NOTE]
> TLS 1.2 のサポートがバージョン 5.0 より前の Android で必要な場合、"*または*" TLS 1.2 のサポートが `System.Net.WebClient` および関連する API で必要な場合、[`$(AndroidTlsProvider)`](#androidtlsprovider) を使用する必要があります。

> [!NOTE]
> このプロパティのサポートは、[`XA_HTTP_CLIENT_HANDLER_TYPE` 環境変数](~/android/deploy-test/environment.md)を設定することにより機能します。
> [`@(AndroidEnvironment)`](~/android/deploy-test/building-apps/build-items.md#androidenvironment) のビルド アクションを含むファイル内で見つかる `$XA_HTTP_CLIENT_HANDLER_TYPE` 値
> が優先されます。

Xamarin.Android 6.1 で追加されました。

## <a name="androidkeystore"></a>AndroidKeyStore

カスタム署名情報を使用するかどうかを示すブール値。 既定値は `False` で、既定のデバッグ署名キーがパッケージの署名に使用されることを意味します。

## <a name="androidlaunchactivity"></a>AndroidLaunchActivity

起動する Android アクティビティ。

## <a name="androidlinkmode"></a>AndroidLinkMode

Android パッケージ内に含まれるアセンブリで実行する必要がある[リンク](~/android/deploy-test/linker.md)の種類を指定します。 Android アプリケーション プロジェクトでのみ使用されます。 既定値は *SdkOnly* です。 次の値を指定できます。

- **None**: リンクは試行されません。

- **SdkOnly**: リンクは基本クラス ライブラリでのみ実行され、ユーザーのアセンブリでは実行されません。

- **Full**: リンクは基本クラス ライブラリとユーザーのアセンブリで実行されます。

  > [!NOTE]
  > *Full* の `AndroidLinkMode` 値を使用すると、多くの場合、特にリフレクションを使用している場合には、アプリが破損します。 何をしているかを*十分に*理解している場合を除き、使用しないでください。

```xml
<AndroidLinkMode>SdkOnly</AndroidLinkMode>
```

## <a name="androidlinkskip"></a>AndroidLinkSkip

リンクしないアセンブリ名のセミコロン (`;`) で区切られたリストを、ファイル拡張子を使わずに指定します。 Android アプリケーション プロジェクト内でのみ使用されます。

```xml
<AndroidLinkSkip>Assembly1;Assembly2</AndroidLinkSkip>
```

## <a name="androidlinktool"></a>AndroidLinkTool

`proguard` または `r8` の有効な値の列挙方式のプロパティ。 Java コードに使用されるコード シュリンカーを示します。 現在、既定値は空の文字列です。または、`$(AndroidEnableProguard)` が `True` の場合は `proguard` です。 詳細については、[D8 と R8][d8-r8] に関するドキュメントをご覧ください。

[d8-r8]: https://github.com/xamarin/xamarin-android/blob/master/Documentation/guides/D8andR8.md

## <a name="androidlintenabled"></a>AndroidLintEnabled

開発者がパッケージ化プロセスの一部として、Android の `lint` ツールを実行できるようにするブール型プロパティ。

`$(AndroidLintEnabled)`=True の場合、次のプロパティが使用されます。

- [`$(AndroidLintEnabledIssues)`](#androidlintenabledissues):
- [`$(AndroidLintDisabledIssues)`](#androidlintdisabledissues):
- [`$(AndroidLintCheckIssues)`](#androidlintcheckissues):

次のビルド アクションを使用することもできます。

- [`@(AndroidLintConfig)`](~/android/deploy-test/building-apps/build-items.md#androidlintconfig):

Android の `lint` ツールの詳細については、[Lint のヘルプ](https://developer.android.com/studio/write/lint)に関するページを参照してください。

## <a name="androidlintenabledissues"></a>AndroidLintEnabledIssues

このプロパティは、[`$(AndroidLintEnabled)`](#androidlintenabled)=True 時にのみ使用されます。

有効にする lint の問題のコンマ区切りのリスト。

## <a name="androidlintdisabledissues"></a>AndroidLintDisabledIssues

このプロパティは、[`$(AndroidLintEnabled)`](#androidlintenabled)=True 時にのみ使用されます。

無効にする lint の問題のコンマ区切りのリスト。

## <a name="androidlintcheckissues"></a>AndroidLintCheckIssues

このプロパティは、[`$(AndroidLintEnabled)`](#androidlintenabled)=True 時にのみ使用されます。

チェックする lint の問題のコンマ区切りのリスト。

注: これらの問題のみがチェックされます。

## <a name="androidmanagedsymbols"></a>AndroidManagedSymbols

ファイル名と行番号の情報を `Release` スタック トレースから抽出できるように、シーケンス ポイントを生成するかどうかを制御するブール型プロパティ。

Xamarin.Android 6.1 で追加されました。

## <a name="androidmanifest"></a>AndroidManifest

アプリの [`AndroidManifest.xml`](~/android/platform/android-manifest.md) のテンプレートとして使用するファイル名を指定します。
ビルド時に、実際の `AndroidManifest.xml` を生成するためにその他の必要な値がマージされます。
`$(AndroidManifest)` は、`/manifest/@package` 属性にパッケージ名を含める必要があります。

## <a name="androidmanifestmerger"></a>AndroidManifestMerger

*AndroidManifest.xml* ファイルをマージするための実装を指定します。 これは、`legacy` が元の C# の実装を選択し、`manifestmerger.jar` が Google の Java 実装を選択する列挙型のプロパティです。

現在の既定値は `legacy` です。 これは、Android Studio の動作に合わせるために、将来のリリースで `manifestmerger.jar` に変更されます。

Google のマージャーでは、[Android ドキュメント][manifest-merger]に記載されている `xmlns:tools="http://schemas.android.com/tools"` のサポートが有効になります。

Xamarin.Android 10.2 で導入されました。

[manifest-merger]: https://developer.android.com/studio/build/manifest-merge

## <a name="androidmanifestplaceholders"></a>AndroidManifestPlaceholders

*AndroidManifest.xml* のキーと値の置換ペアのセミコロン区切りのリストです。各ペアの形式は `key=value` です。

たとえば、`assemblyName=$(AssemblyName)` のプロパティ値によって、*AndroidManifest.xml* に表示できる `${assemblyName}` プレースホルダーが定義されます。

```xml
<application android:label="${assemblyName}"
```

これにより、ビルド プロセスから *AndroidManifest.xml* ファイルに変数を挿入することができます。

## <a name="androidmultidexclasslistextraargs"></a>AndroidMultiDexClassListExtraArgs

`multidex.keep` ファイルを生成するときに、開発者が追加の引数を `com.android.multidex.MainDexListBuilder` に渡すことを許可する文字列プロパティ。

1 つの具体的なケースは、`dx` のコンパイル中に次のエラーが発生する場合です。

```text
com.android.dex.DexException: Too many classes in --main-dex-list, main dex capacity exceeded
```

このエラーが発生している場合は、以下を `.csproj` に追加できます。

```xml
<DxExtraArguments>--force-jumbo </DxExtraArguments>
<AndroidMultiDexClassListExtraArgs>--disable-annotation-resolution-workaround</AndroidMultiDexClassListExtraArgs>
```

これにより、`dx` の手順を成功させることができます。

Xamarin.Android 8.3 で追加されました。

## <a name="androidpackageformat"></a>AndroidPackageFormat

`apk` または `aab` の有効な値の列挙方式のプロパティ。 これは、Android アプリケーションを [APK ファイル][apk]または [Android アプリ バンドル][bundle]としてパッケージ化するかどうかを示します。 アプリ バンドルは、Google Play での送信を目的とした `Release` ビルドの新しい形式です。 この値は現在、`apk` が既定で使用されます。

`$(AndroidPackageFormat)` を `aab` に設定すると、Android アプリ バンドルに必要な他の MSBuild プロパティが設定されます。

- [`$(AndroidUseAapt2)`](~/android/deploy-test/building-apps/build-properties.md#androiduseaapt2) が `True` です。
- [`$(AndroidUseApkSigner)`](#androiduseapksigner) が `False` です。
- [`$(AndroidCreatePackagePerAbi)`](#androidcreatepackageperabi) が `False` です。

[apk]: https://en.wikipedia.org/wiki/Android_application_package
[bundle]: https://developer.android.com/platform/technology/app-bundle

## <a name="androidpackagenamingpolicy"></a>AndroidPackageNamingPolicy

生成された Java ソース コードの Java パッケージ名を指定するための列挙型スタイルのプロパティ。

Xamarin.Android 10.2 以降では、サポートされている値は `LowercaseCrc64` のみです。

Xamarin.Android 10.1 では、暫定の `LowercaseMD5` 値も使用できました。これにより、Xamarin.Android 10.0 以前で使用されてた元の Java パッケージ名スタイルに戻すことができました。 このオプションは、FIPS 準拠が適用されたビルド環境との互換性を向上させるために、Xamarin.Android 10.2 で削除されました。

Xamarin.Android 10.1 で追加されました。

## <a name="androidr8ignorewarnings"></a>AndroidR8IgnoreWarnings

`r8` の `-ignorewarnings` proguard ルールを指定します。 これにより、特定の警告が発生した場合でも、`r8` で Dex コンパイルを使用して続けることができます。 規定値は `True` ですが、より厳密なビヘイビアーを適用するように、`False` を設定することができます。 詳細については、[ProGuard のマニュアル](https://www.guardsquare.com/products/proguard/manual/usage)を参照してください。

Xamarin.Android 10.3 で追加されました。

## <a name="androidr8jarpath"></a>AndroidR8JarPath

r8 dex コンパイラおよびシュリンカーで使用する `r8.jar` へのパス。 既定値は、Xamarin.Android のインストール パスになります。 詳細については、[D8 と R8][d8-r8] のドキュメントを参照してください。

## <a name="androidresgenextraargs"></a>AndroidResgenExtraArgs

Android アセットとリソースを処理するときに、**aapt** コマンドに渡す追加のコマンド ライン オプションを指定します。

## <a name="androidresgenfile"></a>AndroidResgenFile

生成するリソース ファイルの名前を指定します。 既定のテンプレートでは、これは `Resource.designer.cs` に設定されます。

## <a name="androidsdkbuildtoolsversion"></a>AndroidSdkBuildToolsVersion

Android SDK ビルド ツール パッケージは、特に、**aapt** および **zipalign** ツールを提供します。 複数の異なるバージョンのビルド ツール パッケージを同時にインストールすることができます。 パッケージ化するビルド ツール パッケージの選択は、"優先" ビルド ツールのバージョンをチェックして、ある場合はそれを使用して行われます。"優先" バージョンが "*ない*" 場合は、インストールされている最も高いバージョンのビルド ツール パッケージが使用されます。

`$(AndroidSdkBuildToolsVersion)` MSBuild プロパティには、優先ビルド ツールのバージョンが含まれています。 Xamarin.Android ビルド システムは `Xamarin.Android.Common.targets` に既定値を提供します。たとえば、最新の aapt がクラッシュして、前のバージョンの aapt が機能することがわかっている場合には、その既定値をプロジェクト ファイル内でオーバーライドして、別のビルド ツール バージョンを選択できます。

## <a name="androidsigningkeyalias"></a>AndroidSigningKeyAlias

キーストア内のキーに別名を指定します。 これは、キーストアを作成するときに使用される **keytool -alias** 値です。

## <a name="androidsigningkeypass"></a>AndroidSigningKeyPass

キーストア ファイル内にあるキーのパスワードを指定します。 これは、`keytool` が **Enter key password for $(AndroidSigningKeyAlias)** ($(AndroidSigningKeyAlias) のキーのパスワードを入力) を求めたときに入力される値です。

Xamarin.Android 10.0 以前では、このプロパティはプレーンテキストのパスワードのみをサポートしています。

Xamarin.Android 10.1 以降では、このプロパティは、パスワードを格納する環境変数またはファイルを指定するために使用できる `env:` および `file:` プレフィックスもサポートします。 これらのオプションを使用すると、ビルド ログにパスワードが表示されないようにすることができます。

たとえば、*AndroidSigningPassword* という名前の環境変数を使用するには、次のようにします。

```xml
<PropertyGroup>
    <AndroidSigningKeyPass>env:AndroidSigningPassword</AndroidSigningKeyPass>
</PropertyGroup>
```

`C:\Users\user1\AndroidSigningPassword.txt` にあるファイルを使用するには、次のようにします。

```xml
<PropertyGroup>
    <AndroidSigningKeyPass>file:C:\Users\user1\AndroidSigningPassword.txt</AndroidSigningKeyPass>
</PropertyGroup>
```

> [!NOTE]
> `env:` プレフィックスは、[`$(AndroidPackageFormat)`](#androidpackageformat) が `aab` に設定されている場合はサポートされません。

## <a name="androidsigningkeystore"></a>AndroidSigningKeyStore

`keytool` で作成されたキーストア ファイルのファイル名を指定します。 これは、**keytool -keystore** オプションで指定された値に対応します。

## <a name="androidsigningstorepass"></a>AndroidSigningStorePass

[`$(AndroidSigningKeyStore)`](#androidsigningkeystore) へのパスワードを指定します。
これは、キーストア ファイルを作成していて、**Enter keystore password:** (キーストア パスワードを入力) で求められたときに、`keytool` に指定する値です。

Xamarin.Android 10.0 以前では、このプロパティはプレーンテキストのパスワードのみをサポートしています。

Xamarin.Android 10.1 以降では、このプロパティは、パスワードを格納する環境変数またはファイルを指定するために使用できる `env:` および `file:` プレフィックスもサポートします。 これらのオプションを使用すると、ビルド ログにパスワードが表示されないようにすることができます。

たとえば、*AndroidSigningPassword* という名前の環境変数を使用するには、次のようにします。

```xml
<PropertyGroup>
    <AndroidSigningStorePass>env:AndroidSigningPassword</AndroidSigningStorePass>
</PropertyGroup>
```

`C:\Users\user1\AndroidSigningPassword.txt` にあるファイルを使用するには、次のようにします。

```xml
<PropertyGroup>
    <AndroidSigningStorePass>file:C:\Users\user1\AndroidSigningPassword.txt</AndroidSigningStorePass>
</PropertyGroup>
```

> [!NOTE]
> `env:` プレフィックスは、[`$(AndroidPackageFormat)`](#androidpackageformat) が `aab` に設定されている場合はサポートされません。

## <a name="androidsupportedabis"></a>AndroidSupportedAbis

`.apk` に含める必要がある ABI のセミコロン (`;`) で区切られたリストを含む文字列プロパティ。

サポートされている値は次のとおりです。

- `armeabi-v7a`
- `x86`
- `arm64-v8a`: Xamarin.Android 5.1 以降が必要です。
- `x86_64`: Xamarin.Android 5.1 以降が必要です。

## <a name="androidtlsprovider"></a>AndroidTlsProvider

アプリケーションで使用する必要がある TLS プロバイダーを指定する文字列値。 次のいずれかの値になります。

- 空の文字列を設定解除します。Xamarin.Android 7.3 以上では、これは `btls` と同等です。

  Xamarin.Android 7.1 では、これは `legacy` と同等です。

  これは、Visual Studio のプロパティ ページの**既定**の設定に対応しています。

- `btls`: [HttpWebRequest](xref:System.Net.HttpWebRequest) との TLS 通信に [BoringSSL](https://boringssl.googlesource.com/boringssl) を使用します。

  これにより、Android のすべてのバージョンで TLS 1.2 を使用できます。

  これは、Visual Studio のプロパティ ページの**ネイティブ TLS 1.2+** の設定に対応しています。

- `legacy`:Xamarin.Android 10.1 以前では、ネットワークの対話に過去に管理されていた SSL の実装を使用します。 これは、TLS 1.2 をサポート*していません*。

  これは、Visual Studio のプロパティ ページの**マネージド TLS 1.0** の設定に対応しています。

  Xamarin.Android 10.2 以降では、この値は無視され、`btls` 設定が使用されます。

- `default`:この値は、Xamarin.Android プロジェクトで使用される可能性はほとんどありません。 代わりに空の文字列を使用することをお勧めします。これは、Visual Studio のプロパティ ページの**既定**の設定に対応しています。

  `default` 値は、Visual Studio のプロパティ ページでは提供されません。

  これは現在、`legacy` と同じです。

Xamarin.Android 7.1 で追加されました。

## <a name="androiduseaapt2"></a>AndroidUseAapt2

開発者がパッケージ化するために `aapt2` ツールの使用を制御することを許可するブール型プロパティ。
既定では、これは False になり、Xamarin.Android では `aapt` が使用されます。
開発者が新しい `aapt2` 機能を使用することを望んでいる場合は、

```xml
<AndroidUseAapt2>True</AndroidUseAapt2>
```

該当する `.csproj` 内に上の内容を追加します。 または、コマンド ラインでプロパティを指定します。

```shell
/p:AndroidUseAapt2=True
```

Xamarin.Android 8.3 で追加されました。

## <a name="androiduseapksigner"></a>AndroidUseApkSigner

`jarsigner` ではなく `apksigner` ツールを使用することを開発者に許可するブール型プロパティ。

Xamarin.Android 8.2 で追加されました。

## <a name="androidusedefaultaotprofile"></a>AndroidUseDefaultAotProfile

開発者が既定の AOT プロファイルの使用を抑制できるようにするブール型プロパティ。

既定の AOT プロファイルを抑制するには、このプロパティを `false` に設定します。

Xamarin.Android 10.1 で追加されました。

## <a name="androiduselegacyversioncode"></a>AndroidUseLegacyVersionCode

開発者が versionCode の計算を Xamarin.Android 8.2 前の動作に戻すことを許可するブール型プロパティ。 これは、Google Play ストアに既存のアプリケーションがある開発者に向けてのみ使用する必要があります。 新しい [`$(AndroidVersionCodePattern)`](#androidversioncodepattern) プロパティを使用することを強くお勧めします。

Xamarin.Android 8.2 で追加されました。

## <a name="androidusemanageddesigntimeresourcegenerator"></a>AndroidUseManagedDesignTimeResourceGenerator

デザイン時のビルドを、`aapt` ではなく管理対象リソース パーサーの使用に切り替えるブール型プロパティ。

Xamarin.Android 8.1 で追加されました。

## <a name="androidusesharedruntime"></a>AndroidUseSharedRuntime

ターゲット デバイスでアプリケーションを実行するために "*共有ランタイム パッケージ*" が必要かどうかを決定するブール型プロパティ。 共有ランタイム パッケージに依存することで、アプリケーション パッケージをより小型化し、パッケージの作成と展開プロセスを高速化できるため、ビルド/配置/デバッグのターンアラウンド サイクルの高速化が結果として得られます。

このプロパティは、デバッグ ビルドには `True`、リリース プロジェクトには `False` にする必要があります。

## <a name="androidversioncodepattern"></a>AndroidVersionCodePattern

開発者がマニフェスト内の `versionCode` をカスタマイズできるようにする文字列プロパティ。
`versionCode` の決定に関する情報は、「[Creating the Version Code for the APK](~/android/deploy-test/building-apps/abi-specific-apks.md)」 (APK のバージョン コードの作成) を参照してください。

いくつかの例では、`abi` が `armeabi` であり、マニフェスト内の `versionCode` が `123` である場合、`$(AndroidCreatePackagePerAbi)` が True のときには `{abi}{versionCode}` によって `1123` の versionCode が生成され、それ以外のときは 123 の値が生成されます。
`abi` が `x86_64` であり、マニフェスト内の `versionCode` が `44` である場合は次のようになります。 `$(AndroidCreatePackagePerAbi)` が True のときには `544` が生成され、それ以外のときには `44` の値が生成されます。

`versionCode` を `0` でレフト パディングしているため、レフト パディングの書式文字列 `{abi}{versionCode:0000}` を含めると、`50044` が生成されます。 または、`{abi}{versionCode:D4}` などの 10 進パディングを使用することもできます。
これは前の例と同じ動作になります。

値は整数である必要があるため、'0' と 'Dx' のパディングの書式文字列のみがサポートされます。

事前定義済みのキー項目

- **abi**  &ndash; アプリのターゲットとなる abi を挿入します。
  - 2 &ndash; `armeabi-v7a`
  - 3 &ndash; `x86`
  - 4 &ndash; `arm64-v8a`
  - 5 &ndash; `x86_64`

- **minSDK**  &ndash;`AndroidManifest.xml` または `11` (定義されていない場合) からサポートされる Sdk の最小値を挿入します。

- **versionCode** &ndash;`Properties\AndroidManifest.xml` から直接バージョン コードを使用します。

`$(AndroidVersionCodeProperties)` プロパティ (次で定義) を使用してカスタム項目を定義することができます。

既定では、値は `{abi}{versionCode:D6}` に設定されます。 開発者が古い動作を保持する必要がある場合は、`$(AndroidUseLegacyVersionCode)` プロパティを `true` に設定することで既定値をオーバーライドできます。

Xamarin.Android 7.2 で追加されました。

## <a name="androidversioncodeproperties"></a>AndroidVersionCodeProperties

[`$(AndroidVersionCodePattern)`](#androidversioncodepattern) で使用するために、開発者がカスタム項目を定義できるようにする文字列プロパティ。
これらは `key=value` ペアの形式です。 `value` 内のすべての項目は整数値である必要があります。 (例: `screen=23;target=$(_AndroidApiLevel)`)。 ご覧のとおり、既存またはカスタムの MSBuild プロパティを文字列で利用することができます。

Xamarin.Android 7.2 で追加されました。

## <a name="aotassemblies"></a>AotAssemblies

アセンブリをネイティブ コードに Ahead-of-Time コンパイルして、`.apk` に含めるかどうかを決定するブール型プロパティ。

このプロパティのサポートは、Xamarin.Android 5.1 で追加されました。

このプロパティは既定で `False` です。

## <a name="aprofutilextraoptions"></a>AProfUtilExtraOptions

`aprofutil` に渡す追加のオプション。

## <a name="beforegenerateandroidmanifest"></a>BeforeGenerateAndroidManifest

このプロパティに一覧表示されている MSBuild ターゲットは、`_GenerateJavaStubs` の前に直接実行されます。

Xamarin.Android 9.4 で追加されました。

## <a name="configuration"></a>構成

使用するビルド構成 ("Debug" や "Release" など) を指定します。 Configuration プロパティは、ターゲットの動作を決定するその他のプロパティの既定値を決定するために使用されます。 追加の構成は、IDE 内で作成できます。

"*既定*" では、`Debug` 構成によって、[`Install`](~/android/deploy-test/building-apps/build-targets.md#install)
および [`SignAndroidPackage`](~/android/deploy-test/building-apps/build-targets.md#signandroidpackage)
ターゲットが生成され、動作する上で他のファイルやパッケージの存在が必要となる、より小さい Android パッケージが作成されます。

既定の `Release` 構成によって [`Install`](~/android/deploy-test/building-apps/build-targets.md#install)
および [`SignAndroidPackage`](~/android/deploy-test/building-apps/build-targets.md#signandroidpackage)
ターゲットが生成されます。これらが作成する Android パッケージは、"*スタンドアロン*" であり、他のパッケージやファイルをインストールしなくても使用できます。

## <a name="debugsymbols"></a>DebugSymbols

[`$(DebugType)`](#debugtype) プロパティと組み合わせて、Android パッケージが "*デバッグ可能*" かどうかを決定するブール値。
デバッグ可能なパッケージは、デバッグ シンボルを含んでいて、[`//application/@android:debuggable` 属性](https://developer.android.com/guide/topics/manifest/application-element#debug)を `true` に設定し、[`INTERNET`](https://developer.android.com/reference/android/Manifest.permission#INTERNET) アクセス許可を自動的に追加して、
デバッガーがプロセスにアタッチできるようにします。 `DebugSymbols` が `True` "*かつ*" `DebugType` が空の文字列または `Full` の場合、アプリケーションはデバッグ可能です。

## <a name="debugtype"></a>DebugType

ビルドの一部として生成するための[デバッグ シンボルの型](/visualstudio/msbuild/csc-task)を指定します。これはアプリケーションがデバッグ可能かどうかにも影響します。 次のような値となる場合があります。

- **Full**: 完全なシンボルが生成されます。 [`DebugSymbols`](#debugsymbols)
  MSBuild プロパティも `True` の場合、アプリケーション パッケージはデバッグ可能です。

- **PdbOnly**: "PDB" シンボルが生成されます。 アプリケーション パッケージはデバッグ可能ではありません。

`DebugType` が設定されていないか、空の文字列の場合、`DebugSymbols` プロパティが、アプリケーションがデバッグ可能かどうかを制御します。

## <a name="embedassembliesintoapk"></a>EmbedAssembliesIntoApk

アプリのアセンブリをアプリケーション パッケージに埋め込む必要があるかどうかを決定するブール型プロパティ。

このプロパティは、リリース ビルドには `True`、デバッグ ビルドには `False` にする必要があります。 高速展開でターゲット デバイスがサポートされない場合は、デバッグ ビルドでこのプロパティを `True` にする必要がある*場合があります*。

このプロパティが `False` の場合、[`$(AndroidFastDeploymentType)`](#androidfastdeploymenttype)
MSBuild プロパティは `.apk` に埋め込まれるものも制御するため、デプロイおよびリビルド時間が影響を受ける場合があります。

## <a name="enablellvm"></a>EnableLLVM

アセンブリをネイティブ コードに Ahead-of-Time コンパイルするときに、LLVM を使用するかどうかを決定するブール型プロパティ。

このプロパティが有効になっているプロジェクトをビルドするには、Android NDK がインストールされている必要があります。

このプロパティのサポートは、Xamarin.Android 5.1 で追加されました。

このプロパティは既定で `False` です。

[`$(AotAssemblies)`](#aotassemblies) MSBuild プロパティが `True` でない限り、このプロパティは無視されます。

## <a name="enableproguard"></a>EnableProguard

[proguard](https://developer.android.com/tools/help/proguard.html) を Java コードをリンクするパッケージ化プロセスの一部として実行するかどうかを決定するブール型プロパティ。

このプロパティのサポートは、Xamarin.Android 5.1 で追加されました。

このプロパティは既定で `False` です。

`True` の場合、[@(ProguardConfiguration)](~/android/deploy-test/building-apps/build-items.md#proguardconfiguration) ファイルは `proguard` の実行を制御するために使用されます。

## <a name="javamaximumheapsize"></a>JavaMaximumHeapSize

`.dex` ファイルをパッケージ化プロセスの一部としてビルドする際に使用する **java**
`-Xmx` パラメーター値を指定します。 指定されていない場合、`-Xmx` オプションでは **java** に `1G` の値が指定されます。 その他のプラットフォームに比べて、Windows では一般的にこの値が必要なことがわかりました。

[`_CompileDex` ターゲットが `java.lang.OutOfMemoryError`](https://bugzilla.xamarin.com/show_bug.cgi?id=18327) をスローする場合には、このプロパティを指定する必要があります。

以下を変更することで値をカスタマイズします。

```xml
<JavaMaximumHeapSize>1G</JavaMaximumHeapSize>
```

## <a name="javaoptions"></a>JavaOptions

`.dex` ファイルのビルド時に、**java** に渡す追加のコマンド ライン オプションを指定します。

## <a name="jarsignertimestampauthoritycertificatealias"></a>JarsignerTimestampAuthorityCertificateAlias

このプロパティを使用すると、タイムスタンプ機関のキーストアに別名を指定できます。
詳細については、Java の[署名のタイムスタンプのサポート](https://docs.oracle.com/javase/8/docs/technotes/guides/security/time-of-signing.html)に関するドキュメントを参照してください。

```xml
<PropertyGroup>
    <JarsignerTimestampAuthorityCertificateAlias>Alias</JarsignerTimestampAuthorityCertificateAlias>
</PropertyGroup>
```

## <a name="jarsignertimestampauthorityurl"></a>JarsignerTimestampAuthorityUrl

このプロパティでは、タイムスタンプ機関サービスへの URL を指定できます。 これを使用して、`.apk` 署名に確実にタイムスタンプを含めるようにすることができます。
詳細については、Java の[署名のタイムスタンプのサポート](https://docs.oracle.com/javase/8/docs/technotes/guides/security/time-of-signing.html)に関するドキュメントを参照してください。

```xml
<PropertyGroup>
    <JarsignerTimestampAuthorityUrl>http://example.tsa.url</JarsignerTimestampAuthorityUrl>
</PropertyGroup>
```

## <a name="linkerdumpdependencies"></a>LinkerDumpDependencies

リンカーの依存関係ファイルの生成を有効にするブール型プロパティ。 このファイルは、[illinkanalyzer](https://github.com/mono/linker/blob/master/src/analyzer/README.md) ツールに対する入力として使用できます。

既定値は False です。

## <a name="mandroidi18n"></a>MandroidI18n

照合順序や並べ替えテーブルなど、アプリケーションに含まれる国際化サポートを指定します。 値は、次の大文字と小文字を区別しない値の 1 つ以上のコンマ区切りまたはセミコロン区切りのリストです。

- **None**: 追加のエンコーディングは含まれません。

- **All**: 利用可能なすべてのエンコーディングが含まれます。

- **CJK**: *日本語 (EUC)* \[enc-jp, CP51932\]、*日本語 (Shift-JIS)* \[iso-2022-jp, shift\_jis, CP932\]、*日本語 (JIS)* \[CP50220\]、*簡体中国語 (GB2312)* \[gb2312, CP936\]、*韓国語 (UHC)* \[ks\_c\_5601-1987, CP949\]、*韓国語 (EUC)* \[euc-kr, CP51949\]、*繁体中国語 (Big5)* \[big5, CP950\]、および*簡体中国語 (GB18030)* \[GB18030, CP54936\] などの中国語、日本語、および韓国語のエンコーディングが含まれます。

- **MidEast**: *トルコ語 (Windows)* \[iso-8859-9, CP1254\]、*ヘブライ語 (Windows)* \[windows-1255, CP1255\]、*アラビア語 (Windows)* \[windows-1256, CP1256\]、*アラビア語 (ISO)* \[iso-8859-6, CP28596\]、*ヘブライ語 (ISO)* \[iso-8859-8, CP28598\]、*ラテン 5 (ISO)* \[iso-8859-9, CP28599\]、および*ヘブライ語 (Iso 代替)* \[iso-8859-8, CP38598\] などの中東のエンコーディングが含まれます。

- **Other**: *キリル語 (Windows)* \[CP1251\]、*バルト語 (Windows)* \[iso-8859-4, CP1257\]、*ベトナム語 (Windows)* \[CP1258\]、*キリル語 (KOI8-R)* \[koi8-r, CP1251\]、*ウクライナ語 (KOI8 U)* \[koi8-u, CP1251\]、*バルト語 (ISO)* \[iso-8859-4, CP1257\]、*キリル語 (ISO)* \[iso-8859-5, CP1251\]、 *ISCII デーヴァナーガリー語* \[x-iscii-de, CP57002\]、*ISCII ベンガル語* \[x-iscii-be, CP57003\]、*ISCII タミール語* \[x-iscii-ta, CP57004\]、*ISCII テルグ語* \[x-iscii-te, CP57005\]、*ISCII アッサム語* \[x-iscii-as, CP57006\]、*ISCII オリヤー語* \[x-iscii-or, CP57007\]、*ISCII カンナダ語* \[x-iscii-ka, CP57008\]、*ISCII マラヤーラム語* \[x-iscii-ma, CP57009\]、*ISCII グジャラート語* \[x-iscii-gu, CP57010\]、*ISCII パンジャーブ語* \[x-iscii-pa, CP57011\]、および*タイ語 (Windows)* \[CP874\] などのその他のエンコーディングが含まれます。

- **Rare**: *IBM EBCDIC (トルコ語)* \[CP1026\]、*IBM EBCDIC (オープン システム ラテン 1)* \[CP1047\]、*IBM EBCDIC (米国-カナダとユーロ)* \[CP1140\]、*IBM EBCDIC (ドイツとユーロ)* \[CP1141\]、*IBM EBCDIC (デンマーク/ノルウェーとユーロ)* \[CP1142\]、*IBM EBCDIC (フィンランド/スウェーデンとユーロ)* \[CP1143\]、*IBMEBCDIC (イタリアとユーロ)* \[CP1144\]、*IBM EBCDIC (ラテン アメリカ/スペインとユーロ)* \[CP1145\]、*IBM EBCDIC (イギリスとユーロ)* \[CP1146\]、*IBM EBCDIC (フランスとユーロ)* \[CP1147\]、*IBM EBCDIC (インターナショナルとユーロ)* \[CP1148\]、*IBM EBCDIC (アイスランド語とユーロ)* \[CP1149\]、*IBM EBCDIC (ドイツ)* \[CP20273\]、*IBM EBCDIC (デンマーク/ノルウェー)* \[CP20277\]、*IBM EBCDIC (フィンランド/スウェーデン)* \[CP20278\]、*IBM EBCDIC (イタリア)* \[CP20280\]、*IBM EBCDIC (ラテン アメリカ/スペイン)* \[CP20284\]、*IBM EBCDIC (イギリス)* \[CP20285\]、*IBM EBCDIC (日本語カタカナ拡張)* \[CP20290\]、*IBM EBCDIC (フランス)* \[CP20297\]、*IBM EBCDIC (アラビア語)* \[CP20420\]、*IBM EBCDIC (ヘブライ語)* \[CP20424\]、*IBM EBCDIC (アイスランド語)* \[CP20871\]、*IBM EBCDIC (キリル、セルビア語、ブルガリア語)* \[CP21025\]、*IBM EBCDIC (米国-カナダ)* \[CP37\]、 *IBM EBCDIC (インターナショナル)* \[CP500\]、*アラビア語 (ASMO 708)* \[CP708\]、*中央ヨーロッパ言語 (DOS)* \[CP852\]*, キリル言語 (DOS)* \[CP855\]、*トルコ語 (DOS)* \[CP857\]*西ヨーロッパ言語 (DOS とユーロ)* \[CP858\]、*ヘブライ語 (DOS)* \[CP862\]、*アラビア語 (DOS)* \[CP864\]、*ロシア語 (DOS)* \[CP866\]、*ギリシャ語 (DOS)* \[CP869\]、*IBM EBCDIC (ラテン 2)* \[CP870\]、*IBM EBCDIC (ギリシャ語)* \[CP875\] などのまれなエンコーディングが含まれます。

- **West**: *西ヨーロッパ言語 (Mac)* \[macintosh, CP10000\]、*アイスランド語 (Mac)* \[x-mac-icelandic, CP10079\]、*中央ヨーロッパ言語 (Windows)* \[iso-8859-2, CP1250\]、*西ヨーロッパ言語 (Windows)* \[iso-8859-1, CP1252\]、*ギリシャ語 (Windows)* \[iso-8859-7, CP1253\]、*中央ヨーロッパ言語 (ISO)* \[iso-8859-2, CP28592\]、*ラテン 3 (ISO)* \[iso-8859-3, CP28593\]、*ギリシャ語 (ISO)* \[iso-8859-7, CP28597\]、*ラテン 9 (ISO)* \[iso-8859-15, CP28605\]、 *OEM 米国* \[CP437\]、*西ヨーロッパ言語 (DOS)* \[CP850\]、*ポルトガル語 (DOS)* \[CP860\]、*アイスランド語 (DOS)* \[CP861\]、 *フランス語 (カナダ) (DOS)* \[CP863\]、および*北欧語 (DOS)* \[CP865\] などの欧文のエンコーディングが含まれます。

```xml
<MandroidI18n>West</MandroidI18n>
```

## <a name="monoandroidresourceprefix"></a>MonoAndroidResourcePrefix

`AndroidResource` のビルド アクションで、ファイル名の先頭から削除される "*パス プレフィックス*" を指定します。 これにより、リソースがある場所を変更することができます。

既定値は `Resources` です。 Java プロジェクトの構造には、これを `res` に変更します。

## <a name="monosymbolarchive"></a>MonoSymbolArchive

&ldquo;実際の&rdquo; ファイル名と行番号の情報をリリース スタック トレースから抽出するために、後で `mono-symbolicate` で使用するための `.mSYM` 成果物を作成するかどうかを制御するブール型プロパティ。

これは、デバッグ シンボルが有効になっている &ldquo;リリース アプリ&rdquo; に対しては、既定で True になっています: [`$(EmbedAssembliesIntoApk)`](#embedassembliesintoapk) が True、[`$(DebugSymbols)`](~/android/deploy-test/building-apps/build-properties.md#debugsymbols)
が True、および [`$(Optimize)`](/visualstudio/msbuild/common-msbuild-project-properties)
が True。

Xamarin.Android 7.1 で追加されました。