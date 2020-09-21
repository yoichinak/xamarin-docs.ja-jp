---
title: ビルド プロセス
ms.prod: xamarin
ms.assetid: 3BE5EE1E-3FF6-4E95-7C9F-7B443EE3E94C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 09/11/2020
ms.openlocfilehash: d4c8e9ba717602aa30cb736957da5a61d2a91130
ms.sourcegitcommit: e4a51ca35887dd3e45016cf10111cee68d343fbe
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/11/2020
ms.locfileid: "90027608"
---
# <a name="build-process"></a>ビルド プロセス

## <a name="overview"></a>概要

Xamarin.Android ビルド プロセスは、[`Resource.designer.cs` の生成](~/android/internals/api-design.md)、`AndroidAsset`、`AndroidResource`、およびその他の[ビルド アクション](#Build_Actions)のサポート、[Android 呼び出し可能なラッパー](~/android/platform/java-integration/android-callable-wrappers.md)の生成、Android デバイスで実行するための `.apk` の生成のすべてを結び付ける役割を担っています。

## <a name="application-packages"></a>アプリケーション パッケージ

Xamarin.Android ビルド システムが生成できる Android アプリケーション パッケージ (`.apk` ファイル) には、大まかに次の 2 種類があります。

- **リリース** ビルドは、実行に追加のパッケージを必要としない、完全な自己完結型です。 App Store に提供されるのはこのパッケージです。

- **デバッグ** ビルドではありません。

偶然ではなく、これらはパッケージを生成する MSBuild `Configuration` が一致しています。

### <a name="shared-runtime"></a>共有ランタイム

*共有ランタイム*は、基本クラス ライブラリ (`mscorlib.dll` など) と Android バインド ライブラリ (`Mono.Android.dll` など) を提供する追加の Android パッケージのペアです。 デバッグ ビルドは共有ランタイムに依存して、基本クラス ライブラリとバインド アセンブリを Android アプリケーション パッケージ内に含める代わりに、デバッグ パッケージをより小さくできるようにします。

共有ランタイムは、`$(AndroidUseSharedRuntime)` プロパティを `False` に設定することで、デバッグ ビルドで無効にすることができます。

<a name="Fast_Deployment"></a>

### <a name="fast-deployment"></a>高速展開

*高速展開*は、共有ランタイムと連携して、Android アプリケーション パッケージのサイズをさらに縮小します。 これはアプリのアセンブリをパッケージ内にバンドルせずに、 `adb push` を介してターゲットにコピーすることで行います。 このプロセスにより、ビルド/展開/デバッグのサイクルが高速化されます。アセンブリ*のみ*が変更された場合、パッケージは再インストールされず、 代わりに、更新されたアセンブリだけがターゲット デバイスに再同期されるからです。

高速展開は、`adb` のディレクトリ `/data/data/@PACKAGE_NAME@/files/.__override__` への同期をブロックするデバイスでは失敗することがわかっています。

高速展開は、既定で有効になっており、`$(EmbedAssembliesIntoApk)` プロパティを `True` に設定することで、デバッグ ビルドで無効にすることができます。

## <a name="msbuild-projects"></a>MSBuild プロジェクト

Xamarin.Android ビルド プロセスは、Visual Studio for Mac と Visual Studio で使用されるプロジェクト ファイル形式でもある MSBuild に基づいています。
通常、ユーザーは、MSBuild ファイルを手動で編集する必要はありません &ndash; IDE によって完全に機能するプロジェクトが作成され、変更があれば更新され、必要に応じて自動的にビルド ターゲットが呼び出されます。

上級ユーザーが IDE の GUI でサポートされていないことを行えるように、ビルド プロセスは、プロジェクト ファイルを直接編集することでカスタマイズできるようになっています。
このページでは、Xamarin.Android 固有の機能とカスタマイズのみを文書化しています &ndash; 通常の MSBuild 項目、プロパティおよびターゲットを使用して、さらに多くのことができます。

<a name="Build_Targets"></a>

## <a name="build-targets"></a>ビルド ターゲット

Xamarin.Android プロジェクトに対して、次のビルド ターゲットが定義されています。

- **Build** &ndash; パッケージをビルドします。

- **BuildAndStartAotProfiling** &ndash; 埋め込みの AOT プロファイラーを使用してアプリをビルドし、プロファイラーの TCP ポートを `$(AndroidAotProfilerPort)`に設定して、既定のアクティビティを開始します。

  既定の TCP ポートは `9999` です。

  Xamarin.Android 10.2 で追加されました。

- **Clean** &ndash; ビルド プロセスによって生成されたすべてのファイルを削除します。

- **FinishAotProfiling** &ndash; デバイスまたはエミュレーターからの AOT プロファイラー データを TCP ポート `$(AndroidAotProfilerPort)` 経由で収集し、`$(AndroidAotCustomProfilePath)` に書き込みます。

  ポートおよびカスタム プロファイルの既定値は `9999` と `custom.aprof` です。

  追加のオプションを `aprofutil` に渡すには、`$(AProfUtilExtraOptions)` プロパティで設定します。

  このようにすると、次の記述と同じ結果が得られます。

  ```
  aprofutil $(AProfUtilExtraOptions) -s -v -f -p $(AndroidAotProfilerPort) -o "$(AndroidAotCustomProfilePath)"
  ```

  Xamarin.Android 10.2 で追加されました。

- **Install** &ndash; 既定のデバイスまたは仮想デバイスにパッケージをインストールします。

- **SignAndroidPackage** &ndash; パッケージ (`.apk`) を作成して署名します。 `/p:Configuration=Release` とともに使用して、自己完結型の "リリース" パッケージを生成します。

- **StartAndroidActivity** &ndash; デバイスまたは実行中のエミュレーターで既定のアクティビティを開始します。 別のアクティビティを開始するには、`$(AndroidLaunchActivity)` プロパティをアクティビティ名に設定します。

  これは、`adb shell am start @PACKAGE_NAME@/$(AndroidLaunchActivity)` と同じです。

  Xamarin.Android 10.2 で追加されました。

- **StopAndroidPackage** &ndash; デバイスまたは実行中のエミュレーター上のアプリケーション パッケージを完全に停止します。

  これは、`adb shell am force-stop @PACKAGE_NAME@` と同じです。

  Xamarin.Android 10.2 で追加されました。

- **Uninstall** &ndash; 既定のデバイスまたは仮想デバイスからパッケージをアンインストールします。

- **UpdateAndroidResources** &ndash; `Resource.designer.cs` ファイルを更新します。 このターゲットは通常、新しいリソースがプロジェクトに追加されたときに、IDE によって呼び出されます。

## <a name="build-extension-points"></a>拡張ポイントをビルドする

Xamarin Android のビルド システムでは、ビルド プロセスに接続するユーザー向けに、いくつかのパブリック拡張ポイントが公開されています。 これらの拡張ポイントのいずれかを使用するには、`PropertyGroup` の適切な MSBuild プロパティにカスタム ターゲットを追加する必要があります。 次に例を示します。

```xml
<PropertyGroup>
   <AfterGenerateAndroidManifest>
      $(AfterGenerateAndroidManifest);
      YourTarget;
   </AfterGenerateAndroidManifest>
</PropertyGroup>
```

ビルド プロセスの拡張に関する注意事項を次に示します。正しく記述されていないと、ビルドの拡張機能がビルドのパフォーマンスに影響を与える可能性があります (特に、拡張機能がすべてのビルドで実行される場合)。 このような拡張機能を実装する前に、MSBuild の[ドキュメント](https://docs.microsoft.com/visualstudio/msbuild/msbuild)を読むことを強くお勧めします。

- **AfterGenerateAndroidManifest** &ndash; このプロパティに示されているターゲットは、内部の `_GenerateJavaStubs` ターゲットの後に直接実行されます。 ここで、`$(IntermediateOutputPath)` の `AndroidManifest.xml` ファイルが生成されます。 したがって、生成された `AndroidManifest.xml` ファイルに変更を加える場合、この拡張ポイントを使用して行うことができます。

  Xamarin.Android 9.4 で追加されました。

- **BeforeGenerateAndroidManifest** &ndash; このプロパティに示されているターゲットは、`_GenerateJavaStubs` の前に直接実行されます。

  Xamarin.Android 9.4 で追加されました。

## <a name="build-properties"></a>ビルド プロパティ

MSBuild プロパティは、ターゲットの動作を制御します。 これらは、[MSBuild PropertyGroup 要素](https://docs.microsoft.com/visualstudio/msbuild/propertygroup-element-msbuild)内のプロジェクト ファイル (**MyApp.csproj** など) 内で指定されます。

- **Configuration** &ndash; 使用するビルド構成 ("Debug" または "Release" など) を指定します。 Configuration プロパティは、ターゲットの動作を決定するその他のプロパティの既定値を決定するために使用されます。 追加の構成は、IDE 内で作成できます。

  *既定では*、`Debug` 構成により、操作する他のファイルとパッケージの存在を必要とするより小さい Android パッケージを作成する、`Install` ターゲットと `SignAndroidPackage` ターゲットがもたらされます。

  既定の `Release` 構成により、*スタンドアロン*で、他のパッケージやファイルをインストールしなくても使用できる Android パッケージを作成する `Install` ターゲットと `SignAndroidPackage` ターゲットがもたらされます。

- **DebugSymbols** &ndash; `$(DebugType)` プロパティと組み合わせて、Android パッケージが*デバッグ可能*かどうかを決定するブール値。 デバッグ可能パッケージには、デバッグ シンボルが含まれており、`//application/@android:debuggable` 属性を `true` に設定し、`INTERNET` アクセス許可を自動的に追加して、デバッガーがプロセスにアタッチできるようにします。 `DebugSymbols` が `True` "*かつ*" `DebugType` が空の文字列または `Full` の場合、アプリケーションはデバッグ可能です。

- **DebugType** &ndash; ビルドの一部として生成するための[デバッグ シンボルの型](https://docs.microsoft.com/visualstudio/msbuild/csc-task)を指定します。これはアプリケーションがデバッグ可能かどうかにも影響します。 次のような値となる場合があります。

  - **Full**: 完全なシンボルが生成されます。 `DebugSymbols` MSBuild プロパティも `True` の場合、アプリケーション パッケージはデバッグ可能です。

  - **PdbOnly**: "PDB" シンボルが生成されます。 アプリケーション パッケージはデバッグ可能には*なりません*。

  `DebugType` が設定されていないか、空の文字列の場合、`DebugSymbols` プロパティが、アプリケーションがデバッグ可能かどうかを制御します。

  - **AndroidGenerateLayoutBindings** &ndash; `true` に設定されている場合は[レイアウトのコードビハインド](https://github.com/xamarin/xamarin-android/blob/master/Documentation/guides/LayoutCodeBehind.md)の生成を有効にし、`false` に設定されている場合は完全に無効にします。 既定値は `false` です。

### <a name="install-properties"></a>インストール プロパティ

インストール プロパティは、`Install` ターゲットと `Uninstall` ターゲットの動作を制御します。

- **AdbTarget** &ndash; Android パッケージのインストール先または削除元となる Android ターゲット デバイスを指定します。 このプロパティの値は、[`adb` ターゲット デバイス オプション](https://developer.android.com/tools/help/adb.html#issuingcommands)と同じです。

  ```bash
  # Install package onto emulator via -e
  # Use `/Library/Frameworks/Mono.framework/Commands/msbuild` on OS X
  MSBuild /t:Install ProjectName.csproj /p:AdbTarget=-e
  ```

### <a name="packaging-properties"></a>パッケージング プロパティ

パッケージング プロパティは、Android パッケージの作成を制御し、`Install` ターゲットと `SignAndroidPackage` ターゲッによって使用されます。
リリース アプリケーションをパッケージングする場合には、[署名プロパティ](#Signing_Properties)も関係します。

- **AndroidAotProfiles** &ndash; 開発者がコマンド ラインから AOT プロファイルを追加できるようにする文字列プロパティ。 セミコロンまたはコンマで区切られた絶対パスの一覧です。

  Xamarin.Android 10.1 で追加されました。

- **AndroidApkDigestAlgorithm** &ndash; `jarsigner -digestalg` で使用するダイジェスト アルゴリズムを指定する文字列値。

  既定値は `SHA-256` です。 Xamarin.Android 10.0 以前では、既定値は `SHA1` でした。

  Xamarin.Android 9.4 で追加されました。

- **AndroidApkSignerAdditionalArguments** &ndash; 開発者が `apksigner` ツールに追加の引数を指定することを許可する文字列プロパティ。

  Xamarin.Android 8.2 で追加されました。

- **AndroidApkSigningAlgorithm** &ndash; `jarsigner -sigalg` で使用する署名アルゴリズムを指定する文字列値。

  既定値は `SHA256withRSA` です。 Xamarin.Android 10.0 以前では、既定値は `md5withRSA` でした。

  Xamarin.Android 8.2 で追加されました。

- **AndroidApplication** &ndash; プロジェクトが Android アプリケーション用 (`True`) か、または Android ライブラリ プロジェクト用 (`False` または存在しない) かを示すブール値。

  `<AndroidApplication>True</AndroidApplication>` を持つプロジェクトは、Android パッケージ内に 1 つしか存在できない場合があります (残念ながら、これについてはまだ検証されていません。Android リソースに関するわかりにくいおかしなエラーになることがあります)。

- **AndroidApplicationJavaClass** &ndash; [Android.App.Application](xref:Android.App.Application) からクラスを継承するときに、`android.app.Application` の代わりに使用する完全な Java クラス名。

  このプロパティは、通常、`$(AndroidEnableMultiDex)` MSBuild プロパティなどの*他の*プロパティで設定されます。

  Xamarin.Android 6.1 で追加されました。

- **AndroidBinUtilsPath** &ndash; `ld`、ネイティブ リンカー、`as`、ネイティブ アセンブラーなどの Android [binutils][binutils] が格納されているディレクトリへのパス。 これらのツールは Android NDK に含まれており、Xamarin.Android のインストールにも含まれています。

  既定値は `$(MonoAndroidBinDirectory)\ndk\` です。

  Xamarin.Android 10.0 で追加されました。

  [binutils]: https://android.googlesource.com/toolchain/binutils/

- **AndroidBoundExceptionType** &ndash; Xamarin.Android で提供されている型で、`Android.Runtime.InputStreamInvoker`、`System.IO.Stream`、`Android.Runtime.JavaDictionary`、`System.Collections.IDictionary` など、Java 型の観点から .NET の型またはインターフェイスを実装している場合に、例外を伝播する方法を指定する文字列値。

  - `Java`:元の Java 例外の型はそのまま伝播されます。

    これは、たとえば、`InputStreamInvoker` が `System.IO.Stream` API を適切に実装していないことを意味します。これは、`Stream.Read()` から`System.IO.IOException` ではなく `Java.IO.IOException` がスローされる可能性があるためです。

    これは、10.2 より前の Xamarin.Android のすべてのリリースでの例外伝播の動作です。

    これは、Xamarin.Android 10.2 の既定値です。

  - `System`:元の Java 例外の型がキャッチされ、適切な .NET 例外の型でラップされます。

    これは、たとえば、`InputStreamInvoker` が `System.IO.Stream` を正しく実装し、`Stream.Read()` が `Java.IO.IOException` インスタンスをスロー "*しない*" ことを意味します。  (代わりに、`Exception.InnerException` の値として `Java.IO.IOException` を持つ `System.IO.IOException` をスローする場合があります)。

    これは、Xamarin.Android 11.0 で既定値になります。

  Xamarin.Android 10.2 で追加されました。

- **AndroidBuildApplicationPackage** &ndash; パッケージ (.apk) を作成して署名するかどうかを示すブール値。 この値を `True` に設定することは、[SignAndroidPackage](#Build_Targets) ビルド ターゲットを使用することと同じです。

  このプロパティのサポートは、Xamarin.Android 7.1 以降で追加されました。

  このプロパティは既定で `False` です。

- **AndroidBundleConfigurationFile** &ndash; Android アプリ バンドルをビルドするときに `bundletool` の[構成ファイル][bundle-config-format]として使用するファイル名を指定します。 このファイルは、APK を生成するためのどのディメンションでバンドルを分割するかなど、バンドルから APK を生成する方法のいくつかの側面を制御します。 Xamarin.Android では、圧縮しないままにするファイル拡張子の一覧など、これらの設定の一部が自動的に構成されることに注意してください。

  このプロパティが意味をなすのは、`$(AndroidPackageFormat)` が `aab` に設定されている場合のみです。

  Xamarin.Android 10.3 で追加されました。

  [bundle-config-format]: https://developer.android.com/studio/build/building-cmdline#bundleconfig

- **AndroidDexTool** &ndash; `dx` または `d8` の有効な値の列挙方式のプロパティ。 Xamarin.Android のビルド プロセス中に使用される Android の [dex][dex] コンパイラを示します。
  現在の既定値は `dx` です。 詳細については、[D8 と R8][d8-r8] に関するドキュメントをご覧ください。

  [dex]: https://source.android.com/devices/tech/dalvik/dalvik-bytecode
  [d8-r8]: https://github.com/xamarin/xamarin-android/blob/master/Documentation/guides/D8andR8.md

- **AndroidEnableDesugar** &ndash; `desugar` が有効かどうかを決定するブール型プロパティ。 現在、Android ではすべての Java 8 機能がサポートされておらず、`javac` コンパイラの出力に `desugar` と呼ばれるバイトコード変換を実行して、既定のツールチェーンにより新しい言語機能が実装されています。 `AndroidDexTool=dx` を使用している場合の既定値は `False`、`AndroidDexTool=d8` を使用している場合の既定値は `True` です。

- **AndroidEnableGooglePlayStoreChecks** &ndash; 開発者が次の Google Play ストア チェックを無効にできるようにするブール プロパティ:XA1004、XA1005、XA1006。 これは、Google Play ストアを対象としておらず、このチェックを実行したくない開発者にとって役立ちます。

  Xamarin.Android 9.4 で追加されました。

- **AndroidEnableMultiDex** &ndash; 最終的な `.apk` で Multi-Dex サポートを使用するかどうかを決定するブール型プロパティ。

  このプロパティのサポートは、Xamarin.Android 5.1 で追加されました。

  このプロパティは既定で `False` です。

- **AndroidEnablePreloadAssemblies** &ndash; アプリケーション パッケージ内でバンドルされているすべてのマネージド アセンブリを、プロセスの起動中に読み込むかどうかを制御するブール型プロパティ。

  `True` に設定すると、アプリケーション コードが呼び出される前に、アプリケーション パッケージ内でバンドルされているすべてのアセンブリがプロセスの起動中に読み込まれます。
  これは、Xamarin.Android 9.2 より前のリリースの Xamarin.Android の動作と一致します。

  `False` に設定すると、アセンブリは必要な場合にのみ読み込まれます。
  これにより、アプリケーションの起動がより高速になり、デスクトップの .NET セマンティクスとの整合性も高まります。  時間の短縮を確認したい場合は、`timing` を含めるよう `debug.mono.log` システム プロパティを設定して、`adb logcat` 内で `Finished loading assemblies: preloaded` メッセージを探します。

  依存関係の挿入を使うアプリケーションまたはライブラリでは、それらが同じように `AppDomain.CurrentDomain.GetAssemblies()` でアプリケーション バンドル内のすべてのアセンブリを返すことが必要な場合、それ以外ではアセンブリが必要なかった場合でも、このプロパティが `True` になることを "*必要とする*" 場合があります。

  既定では、この値は `True` に設定されます。

  Xamarin.Android 9.2 で追加されました。

- **AndroidEnableProfiledAot** &ndash; Ahead-of-Time コンパイル中に AOT プロファイルを使用するかどうかを決定するブール型プロパティ。

  プロファイルは `AndroidAotProfile` 項目グループに一覧表示されます。 この項目グループには、既定のプロファイルが含まれています。 既存のものを削除し、独自の AOT プロファイルを追加することで上書きできます。

  このプロパティのサポートは、Xamarin.Android 9.4 で追加されました。

  このプロパティは既定で `False` です。

- **AndroidEnableSGenConcurrent** &ndash; Mono の[同時 GC コレクター](https://www.mono-project.com/docs/about-mono/releases/4.8.0/#concurrent-sgen)が使用されるかどうかを決定するブール型プロパティ。

  このプロパティのサポートは、Xamarin.Android 7.2 で追加されました。

  このプロパティは既定で `False` です。

- **AndroidErrorOnCustomJavaObject** &ndash; `Java.Lang.Object` または `Java.Lang.Throwable` を継承 "*することなく*"、型が `Android.Runtime.IJavaObject`
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

- **AndroidExtraAotOptions** &ndash; `$(AndroidEnableProfiledAot)` または `$(AotAssemblies)` が `true` に設定されているプロジェクトの `Aot` タスク中に、Mono コンパイラに追加のオプションを渡すことができるようにする文字列プロパティ。 Mono クロスコンパイラを呼び出すときに、プロパティの文字列値が応答ファイルに追加されます。

  一般に、このプロパティは空白のままにしておく必要がありますが、特殊なシナリオでは、柔軟性が向上する場合があります。

  このプロパティは、関連する `$(AndroidAotAdditionalArguments)` プロパティとは異なることに注意してください。 このプロパティは、コンマ区切りの引数を Mono コンパイラの `--aot` オプションに配置します。 `$(AndroidExtraAotOptions)` では代わりに、`--verbose` や `--debug` など、完全なスタンドアロンのスペース区切りオプションがコンパイラに渡されます。

  Xamarin.Android 10.2 で追加されました。

- **AndroidFastDeploymentType** &ndash; `$(EmbedAssembliesIntoApk)` MSBuild プロパティが `False` の場合に、ターゲット デバイスの[高速展開ディレクトリ](#Fast_Deployment)に展開できる型を制御する値の `:` (コロン) 区切りのリスト。 リソースが高速展開される場合、そのリソースが生成された `.apk` に埋め込まれ*ない*ため、展開時間を短縮することができます (高速展開が増えるほど、`.apk` を再ビルドする頻度が減り、インストール プロセスを高速化できます)。有効な値を次に示します。

  - `Assemblies`:アプリケーション アセンブリを展開します。

  - `Dexes`:`.dex` ファイル、Android リソース、および Android アセットを展開します。 **この値は、Android 4.4 以降 (API-19) を実行しているデバイスで*のみ*使用できます。**

  既定値は `Assemblies` です。

  **試験的**です。 Xamarin.Android 6.1 で追加されました。

- **AndroidGenerateJniMarshalMethods** &ndash; ビルド プロセスの一環として、JNI マーシャリング メソッドの生成を有効にするブール型プロパティ。 これにより、バインディング ヘルパー コードでの System.Reflection の使用量が大幅に削減されます。

  既定では、これは False に設定されます。 開発者が新しい JNI マーシャリング メソッド機能を使用する場合、

  ```xml
  <AndroidGenerateJniMarshalMethods>True</AndroidGenerateJniMarshalMethods>
  ```

  自分の .csproj で設定できます。 または、コマンド ラインでプロパティを指定します。

  ```
  /p:AndroidGenerateJniMarshalMethods=True
  ```

  **試験的**です。 Xamarin.Android 9.2 で追加されました。
  既定値は False です。

- **AndroidGenerateJniMarshalMethodsAdditionalArguments** &ndash; `jnimarshalmethod-gen.exe` 呼び出しにさらにパラメーターを追加するために使用できる文字列プロパティ。  これはデバッグに役立つため、オプション (`-v`、`-d`、`--keeptemp` など) を使用できます。

  既定値は、空の文字列です。 これは、.csproj ファイルまたはコマンド ラインで設定できます。 次に例を示します。

  ```xml
  <AndroidGenerateJniMarshalMethodsAdditionalArguments>-v -d --keeptemp</AndroidGenerateJniMarshalMethodsAdditionalArguments>
  ```

  または

  ```
  /p:AndroidGenerateJniMarshalMethodsAdditionalArguments="-v -d --keeptemp"
  ```

  Xamarin.Android 9.2 で追加されました。

- **AndroidHttpClientHandlerType** &ndash; 既定の `System.Net.Http.HttpClient` コンストラクターによって使用される、既定の `System.Net.Http.HttpMessageHandler` の実装を制御します。 値は `HttpMessageHandler` サブクラスのアセンブリ修飾型名であり、[`System.Type.GetType(string)`](https://docs.microsoft.com/dotnet/api/system.type.gettype?view=netcore-2.0#System_Type_GetType_System_String_) での使用に適しています。
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
  > TLS 1.2 のサポートがバージョン 5.0 より前の Android で必要な場合、"*または*" TLS 1.2 のサポートが `System.Net.WebClient` および関連する API で必要な場合、`$(AndroidTlsProvider)` を使用する必要があります。

  > [!NOTE]
  > このプロパティのサポートは、[`XA_HTTP_CLIENT_HANDLER_TYPE` 環境変数](~/android/deploy-test/environment.md)を設定することにより機能します。
  > `@(AndroidEnvironment)` のビルド アクションを含むファイル内に見つかる `$XA_HTTP_CLIENT_HANDLER_TYPE` 値が優先されます。

  Xamarin.Android 6.1 で追加されました。

- **AndroidLinkMode** &ndash; Android パッケージ内に含まれるアセンブリで実行する必要がある[リンク](~/android/deploy-test/linker.md)の種類を指定します。 Android アプリケーション プロジェクトでのみ使用されます。 既定値は *SdkOnly* です。 次の値を指定できます。

  - **None**: リンクは試行されません。

  - **SdkOnly**: リンクは基本クラス ライブラリでのみ実行され、ユーザーのアセンブリでは実行されません。

  - **Full**: リンクは基本クラス ライブラリとユーザーのアセンブリで実行されます。

    > [!NOTE]
    > *Full* の `AndroidLinkMode` 値を使用すると、多くの場合、特にリフレクションを使用している場合には、アプリが破損します。 何をしているかを*十分に*理解している場合を除き、使用しないでください。

  ```xml
  <AndroidLinkMode>SdkOnly</AndroidLinkMode>
  ```

- **AndroidLinkSkip** &ndash; リンクしないアセンブリ名のセミコロン (`;`) で区切られたリストを、ファイル拡張子を使わずに指定します。 Android アプリケーション プロジェクト内でのみ使用されます。

  ```xml
  <AndroidLinkSkip>Assembly1;Assembly2</AndroidLinkSkip>
  ```

- **AndroidLinkTool** &ndash; `proguard` または `r8` の有効な値の列挙方式のプロパティ。 Java コードに使用されるコード シュリンカーを示します。 現在、既定値は空の文字列です。または、`$(AndroidEnableProguard)` が `True` の場合は `proguard` です。 詳細については、[D8 と R8][d8-r8] に関するドキュメントをご覧ください。

  [d8-r8]: https://github.com/xamarin/xamarin-android/blob/master/Documentation/guides/D8andR8.md

- **AndroidLintEnabled** &ndash; 開発者がパッケージ化プロセスの一部として、Android の `lint` ツールを実行できるようにするブール型プロパティ。

  - **AndroidLintEnabledIssues** &ndash; 有効にする lint の問題のコンマ区切りリスト。

  - **AndroidLintDisabledIssues** &ndash; 無効にする lint の問題のコンマ区切りリスト。

  - **AndroidLintCheckIssues** &ndash; チェックする lint の問題のコンマ区切りリスト。
    注: これらの問題のみがチェックされます。

  - **AndroidLintConfig** &ndash; これは Lint のスタイル構成ファイルのビルド アクションです。 有効または無効にされた問題の確認に使用できます。 複数のファイルで、コンテンツがマージされるように、このビルド アクションを使用できます。

  Android の `lint` ツールの詳細については、[Lint のヘルプ](https://developer.android.com/studio/write/lint)に関するページを参照してください。

- **AndroidManagedSymbols** &ndash; ファイル名と行番号の情報を `Release` スタック トレースから抽出できるように、シーケンス ポイントを生成するかどうかを制御するブール型プロパティ。

  Xamarin.Android 6.1 で追加されました。

- **AndroidManifest** &ndash; アプリの [`AndroidManifest.xml`](~/android/platform/android-manifest.md) のテンプレートとして使用するファイル名を指定します。
  ビルド時に、実際の `AndroidManifest.xml` を生成するためにその他の必要な値がマージされます。
  `$(AndroidManifest)` は、`/manifest/@package` 属性にパッケージ名を含める必要があります。

- **AndroidManifestMerger** &ndash; *AndroidManifest.xml* ファイルをマージするための実装を指定します。 これは、`legacy` が元の C# の実装を選択し、`manifestmerger.jar` が Google の Java 実装を選択する列挙型のプロパティです。

  現在の既定値は `legacy` です。 これは、Android Studio の動作に合わせるために、将来のリリースで `manifestmerger.jar` に変更されます。

  Google のマージャーでは、[Android ドキュメント][manifest-merger]に記載されている `xmlns:tools="http://schemas.android.com/tools"` のサポートが有効になります。

  Xamarin.Android 10.2 で導入されました。

  [manifest-merger]: https://developer.android.com/studio/build/manifest-merge

- **AndroidManifestPlaceholders** &ndash; *AndroidManifest.xml* のキーと値の置換ペアのセミコロン区切りのリストです。各ペアの形式は `key=value` です。

  たとえば、`assemblyName=$(AssemblyName)` のプロパティ値によって、*AndroidManifest.xml* に表示できる `${assemblyName}` プレースホルダーが定義されます。

  ```xml
  <application android:label="${assemblyName}"
  ```

  これにより、ビルド プロセスから *AndroidManifest.xml* ファイルに変数を挿入することができます。

- **AndroidMultiDexClassListExtraArgs** &ndash; `multidex.keep` ファイルを生成するときに、開発者が追加の引数を `com.android.multidex.MainDexListBuilder` に渡すことを許可する文字列プロパティ。

  1 つの具体的なケースは、`dx` のコンパイル中に次のエラーが発生する場合です。

  ```
  com.android.dex.DexException: Too many classes in --main-dex-list, main dex capacity exceeded
  ```

  このエラーが発生している場合は、以下を .csproj に追加できます。

  ```xml
  <DxExtraArguments>--force-jumbo </DxExtraArguments>
  <AndroidMultiDexClassListExtraArgs>--disable-annotation-resolution-workaround</AndroidMultiDexClassListExtraArgs>
  ```

  これにより、`dx` の手順を成功させることができます。

  Xamarin.Android 8.3 で追加されました。

- **AndroidPackageFormat** &ndash;`apk` または `aab` の有効な値の列挙方式のプロパティ。 これは、Android アプリケーションを [APK ファイル][apk]または [Android アプリ バンドル][bundle]としてパッケージ化するかどうかを示します。 アプリ バンドルは、Google Play での送信を目的とした `Release` ビルドの新しい形式です。 この値は現在、`apk` が既定で使用されます。

  `$(AndroidPackageFormat)` を `aab` に設定すると、Android アプリ バンドルに必要な他の MSBuild プロパティが設定されます。

  - `$(AndroidUseAapt2)` が `True`です。
  - `$(AndroidUseApkSigner)` が `False`です。
  - `$(AndroidCreatePackagePerAbi)` が `False`です。

  [apk]: https://en.wikipedia.org/wiki/Android_application_package
  [bundle]: https://developer.android.com/platform/technology/app-bundle

- **AndroidPackageNamingPolicy** &ndash; 生成された Java ソース コードの Java パッケージ名を指定するための列挙型スタイルのプロパティ。

  Xamarin.Android 10.2 以降では、サポートされている値は `LowercaseCrc64` のみです。

  Xamarin.Android 10.1 では、暫定の `LowercaseMD5` 値も使用できました。これにより、Xamarin.Android 10.0 以前で使用されてた元の Java パッケージ名スタイルに戻すことができました。 このオプションは、FIPS 準拠が適用されたビルド環境との互換性を向上させるために、Xamarin.Android 10.2 で削除されました。

  Xamarin.Android 10.1 で追加されました。

- **AndroidR8JarPath** &ndash; r8 dex コンパイラおよびシュリンカーで使用する `r8.jar` へのパス。 既定値は、Xamarin.Android のインストール パスになります。 詳細については、[D8 と R8][d8-r8] のドキュメントを参照してください。

- **AndroidSdkBuildToolsVersion** &ndash; Android SDK ビルド ツール パッケージは、特に、**aapt** ツールと **zipalign** ツールを提供します。 複数の異なるバージョンのビルド ツール パッケージを同時にインストールすることができます。 パッケージ化するビルド ツール パッケージの選択は、"優先" ビルド ツールのバージョンをチェックして、ある場合はそれを使用して行われます。"優先" バージョンが "*ない*" 場合は、インストールされている最も高いバージョンのビルド ツール パッケージが使用されます。

  `$(AndroidSdkBuildToolsVersion)` MSBuild プロパティには、優先ビルド ツールのバージョンが含まれています。 Xamarin.Android ビルド システムは `Xamarin.Android.Common.targets` に既定値を提供します。たとえば、最新の aapt がクラッシュして、前のバージョンの aapt が機能することがわかっている場合には、その既定値をプロジェクト ファイル内でオーバーライドして、別のビルド ツール バージョンを選択できます。

- **AndroidSupportedAbis** &ndash;`.apk` に含める必要がある ABI のセミコロン (`;`) で区切られたリストを含む文字列プロパティ。

  サポートされている値は次のとおりです。

  - `armeabi-v7a`
  - `x86`
  - `arm64-v8a`: Xamarin.Android 5.1 以降が必要です。
  - `x86_64`: Xamarin.Android 5.1 以降が必要です。

- **AndroidTlsProvider** &ndash; アプリケーションで使用する必要がある TLS プロバイダーを指定する文字列値。 次のいずれかの値になります。

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

- **AndroidUseApkSigner** &ndash; `jarsigner` ではなく `apksigner` ツールを使用することを開発者に許可するブール型プロパティ。

    Xamarin.Android 8.2 で追加されました。

- **AndroidUseDefaultAotProfile** &ndash; 開発者が既定の AOT プロファイルの使用を抑制できるようにするブール型プロパティです。

  既定の AOT プロファイルを抑制するには、このプロパティを `false` に設定します。

  Xamarin.Android 10.1 で追加されました。

- **AndroidUseLegacyVersionCode** &ndash; 開発者が versionCode の計算を Xamarin.Android 8.2 前の動作に戻すことを許可するブール型プロパティ。 これは、Google Play ストアに既存のアプリケーションがある開発者に向けてのみ使用する必要があります。 新しい `$(AndroidVersionCodePattern)` プロパティを使用することを強くお勧めします。

  Xamarin.Android 8.2 で追加されました。

- **AndroidUseManagedDesignTimeResourceGenerator** &ndash; デザイン時のビルドを、`aapt` ではなく管理対象リソース パーサーの使用に切り替えるブール型プロパティ。

  Xamarin.Android 8.1 で追加されました。

- **AndroidUseSharedRuntime** &ndash; ターゲット デバイスでアプリケーションを実行するために*共有ランタイム パッケージ*が必要かどうかを決定するブール型プロパティ。 共有ランタイム パッケージに依存することで、アプリケーション パッケージをより小型化し、パッケージの作成と展開プロセスを高速化できるため、ビルド/配置/デバッグのターンアラウンド サイクルの高速化が結果として得られます。

  このプロパティは、デバッグ ビルドには `True`、リリース プロジェクトには `False` にする必要があります。

- **AndroidVersionCodePattern** &ndash; 開発者がマニフェスト内の `versionCode` をカスタマイズできるようにする文字列プロパティ。
  `versionCode` の決定に関する情報は、「[Creating the Version Code for the APK](~/android/deploy-test/building-apps/abi-specific-apks.md)」 (APK のバージョン コードの作成) を参照してください。

  いくつかの例では、`abi` が `armeabi` でマニフェスト内の `versionCode` が `123` の場合、`$(AndroidCreatePackagePerAbi)` が True のときには `{abi}{versionCode}` により `1123` の versionCode が生成され、それ以外のときは 123 の値が生成されます。
  `abi` が `x86_64` でマニフェスト内の `versionCode` は `44` です。 これにより、`$(AndroidCreatePackagePerAbi)` が True の場合には `544` が生成され、それ以外の場合は `44` の値が生成されます。

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

- **AndroidVersionCodeProperties** &ndash;`AndroidVersionCodePattern` で使用するために、開発者がカスタム項目を定義できるようにする文字列プロパティ。 これらは `key=value` ペアの形式です。 `value` 内のすべての項目は整数値である必要があります。 (例: `screen=23;target=$(_AndroidApiLevel)`)。 ご覧のとおり、既存またはカスタムの MSBuild プロパティを文字列で利用することができます。

  Xamarin.Android 7.2 で追加されました。

- **AotAssemblies** &ndash; アセンブリをネイティブ コードに Ahead-of-Time コンパイルして、`.apk` に含めるかどうかを決定するブール型プロパティ。

  このプロパティのサポートは、Xamarin.Android 5.1 で追加されました。

  このプロパティは既定で `False` です。

- **EmbedAssembliesIntoApk** &ndash; アプリのアセンブリをアプリケーション パッケージに埋め込むかどうかを決定するブール型プロパティ。

  このプロパティは、リリース ビルドには `True`、デバッグ ビルドには `False` にする必要があります。 高速展開でターゲット デバイスがサポートされない場合は、デバッグ ビルドでこのプロパティを `True` にする必要がある*場合があります*。

  このプロパティが `False` の場合、`$(AndroidFastDeploymentType)` MSBuild プロパティが `.apk` に埋め込まれるものも制御するため、展開および再ビルド時間に影響を及ぼす場合があります。

- **EnableLLVM** &ndash; アセンブリをネイティブ コードに Ahead-of-Time コンパイルするときに、LLVM を使用するかどうかを決定するブール型プロパティ。

  このプロパティが有効になっているプロジェクトをビルドするには、Android NDK がインストールされている必要があります。

  このプロパティのサポートは、Xamarin.Android 5.1 で追加されました。

  このプロパティは既定で `False` です。

  `$(AotAssemblies)` MSBuild プロパティが `True` でない限り、このプロパティは無視されます。

- **EnableProguard** &ndash; [proguard](https://developer.android.com/tools/help/proguard.html) を Java コードをリンクするパッケージ化プロセスの一部として実行するかどうかを決定するブール型プロパティ。

  このプロパティのサポートは、Xamarin.Android 5.1 で追加されました。

  このプロパティは既定で `False` です。

  `True` の場合、[ProguardConfiguration](#ProguardConfiguration) ファイルは `proguard` の実行を制御するために使用されます。

- **JavaMaximumHeapSize** &ndash; `.dex` ファイルをパッケージ化プロセスの一部としてビルドする際に使用する **java**
  `-Xmx` パラメーター値を指定します。 指定されていない場合、`-Xmx` オプションでは **java** に `1G` の値が指定されます。 その他のプラットフォームに比べて、Windows では一般的にこの値が必要なことがわかりました。

  [`_CompileDex` ターゲットが `java.lang.OutOfMemoryError`](https://bugzilla.xamarin.com/show_bug.cgi?id=18327) をスローする場合には、このプロパティを指定する必要があります。

  以下を変更することで値をカスタマイズします。

  ```xml
  <JavaMaximumHeapSize>1G</JavaMaximumHeapSize>
  ```

- **JavaOptions** &ndash;`.dex` ファイルのビルド時に、**java** に渡す追加のコマンド ライン オプションを指定します。

- **LinkerDumpDependencies** &ndash; リンカーの依存関係ファイルの生成を有効にするブール型プロパティ。 このファイルは、[illinkanalyzer](https://github.com/mono/linker/blob/master/src/analyzer/README.md) ツールに対する入力として使用できます。

  既定値は False です。

- **MandroidI18n** &ndash; 照合順序や並べ替えテーブルなど、アプリケーションに含まれる国際化サポートを指定します。 値は、次の大文字と小文字を区別しない値の 1 つ以上のコンマ区切りまたはセミコロン区切りのリストです。

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

- **MonoSymbolArchive** &ndash;&ldquo;実際&rdquo;のファイル名と行番号の情報をリリース スタック トレースから抽出するため、後で `mono-symbolicate` で使用するために `.mSYM` 成果物を作成するかどうかを制御するブール型プロパティ。

  これは、デバッグ シンボルが有効 (`$(EmbedAssembliesIntoApk)` が True、`$(DebugSymbols)` が True、および `$(Optimize)` が True) になっている&ldquo;リリース&rdquo; アプリに対しては、既定で True になっています。

  Xamarin.Android 7.1 で追加されました。

### <a name="binding-project-build-properties"></a>プロジェクトのビルド プロパティをバインドする

次の MSBuild プロパティは、[バインド プロジェクト](~/android/platform/binding-java-library/index.md)で使用されます。

- **AndroidClassParser** &ndash;`.jar` ファイルの解析方法を制御する文字列プロパティ。 次の値を指定できます。

  - **class-parse**: JVM を利用せずに、`class-parse.exe` を使用して直接 Java バイトコードを解析します。 この値は試験的です。

  - **jar2xml**: Java リフレクションを使用して `.jar` ファイルから型とメンバーを抽出するには、`jar2xml.jar` を使用します。

  `jar2xml` よりも優れている `class-parse` の利点は次のとおりです。

  - `class-parse` は、*デバッグ* シンボル (`javac -g` でコンパイルされたバイトコードなど) を含む Java バイトコードからパラメーター名を抽出できます。

  - `class-parse` は、解決できない型のメンバーから継承するクラスやそのようなメンバーが含まれるクラスを "スキップ" しません。

  **試験的**です。 Xamarin.Android 6.0 で追加されました。

  既定値は `jar2xml` です。

  既定値は、将来のリリースで変更されます。

- **AndroidCodegenTarget** &ndash; コード生成ターゲット ABI を制御する文字列型プロパティ。 次の値を指定できます。

  - **XamarinAndroid**: Mono for Android 1.0 以降に付属している JNI バインド API を使用します。 Xamarin.Android 5.0 以降でビルドされたバインドのアセンブリは、Xamarin.Android 5.0 以降 (API/ABI 追加機能) でないと実行できませんが、*ソース*は前の製品バージョンと互換性があります。

  - **XAJavaInterop1**: JNI の呼び出しに Java.Interop を使用します。 `XAJavaInterop1` を使用したバインドのアセンブリは、Xamarin.Android 6.1 以降でのみビルドおよび実行できます。 Xamarin.Android 6.1 以降は、この値で `Mono.Android.dll` をバインドします。

    `XAJavaInterop1` には次の利点があります。

    - より小さなアセンブリ。

    - 継承階層の他のバインドの種類がすべて `XAJavaInterop1` 以降でビルドされる限り、`base` メソッドの呼び出しに `jmethodID` キャッシュを使用。

    - マネージド サブクラスに対して Java 呼び出し可能ラッパー コンストラクターに `jmethodID` キャッシュを使用。

    既定値は `XAJavaInterop1` です。

### <a name="resource-properties"></a>リソース プロパティ

リソースのプロパティは、Android ソースへのアクセスを提供する、`Resource.designer.cs` ファイルの生成を制御します。

- **AndroidAapt2CompileExtraArgs** &ndash; Android アセットとリソースを処理するときに、**aapt2 compile** コマンドに渡す追加のコマンド ライン オプションを指定します。

  Xamarin.Android 9.1 で追加されました。

- **AndroidAapt2LinkExtraArgs** &ndash; Android アセットとリソースを処理するときに、**aapt2 link** コマンドに渡す追加のコマンド ライン オプションを指定します。

  Xamarin.Android 9.1 で追加されました。

- **AndroidExplicitCrunch** &ndash; Xamarin.Android 11.0 ではサポートされなくなりました。

- **AndroidR8IgnoreWarnings** &ndash; `r8` に対して `-ignorewarnings` ProGuard ルールが自動的に指定されます。 これにより、特定の警告が発生した場合でも、`r8` で Dex コンパイルを使用して続けることができます。 規定値は `True` ですが、より厳密なビヘイビアーを適用するように、`False` を設定することができます。 詳細については、[ProGuard のマニュアル](https://www.guardsquare.com/products/proguard/manual/usage)を参照してください。

  Xamarin.Android 10.3 で追加されました。

- **AndroidResgenExtraArgs** &ndash; Android アセットとリソースを処理するときに、**aapt** コマンドに渡す追加のコマンド ライン オプションを指定します。

- **AndroidResgenFile** &ndash; 生成するリソース ファイルの名前を指定します。 既定のテンプレートでは、これは `Resource.designer.cs` に設定されます。

- **AndroidUseAapt2** &ndash; 開発者がパッケージ化するために `aapt2` ツールの使用を制御することを許可するブール型プロパティ。
  既定では、これは False に設定され、`aapt` が使用されます。
  開発者が新しい `aapt2` 機能を使用したい場合は、

  ```xml
  <AndroidUseAapt2>True</AndroidUseAapt2>
  ```

  自分の .csproj で設定できます。 または、コマンド ラインでプロパティを指定します。

  ```
  /p:AndroidUseAapt2=True
  ```

  Xamarin.Android 8.3 で追加されました。

- **MonoAndroidResourcePrefix** &ndash;`AndroidResource` のビルド アクションで、ファイル名の先頭から削除される*パス プレフィックス*を指定します。 これにより、リソースがある場所を変更することができます。

  既定値は `Resources` です。 Java プロジェクトの構造には、これを `res` に変更します。

<a name="Signing_Properties"></a>

### <a name="signing-properties"></a>署名プロパティ

署名プロパティは、アプリケーション パッケージを Android デバイスにインストールできるように、パッケージに署名する方法を制御します。 ビルド イテレーションを高速化するため、Xamarin.Android タスクではビルド プロセス中のパッケージへの署名は行われません。署名を行うと非常に遅くなるからです。 代わりに、インストール前またはエクスポート中に、IDE または *Install* ビルド ターゲットによって署名されます (必要な場合)。 *SignAndroidPackage* ターゲットを呼び出すと、出力ディレクトリに `-Signed.apk` のサフィックスを持つパッケージが生成されます。

既定では、必要に応じて署名ターゲットによって新しいデバッグ署名キーが生成されます。 たとえばビルド サーバーなどで特定のキーを使用する場合は、次の MSBuild プロパティが使用できます。

- **AndroidDebugKeyAlgorithm** &ndash;`debug.keystore` 用に使用する既定のアルゴリズムを指定します。 既定値は `RSA` です。

- **AndroidDebugKeyValidity** &ndash;`debug.keystore` 用に使用する既定の有効性を指定します。 既定値は `10950`、`30 * 365`、または `30 years` です。

- **AndroidDebugStoreType** &ndash; `debug.keystore` に使用するキー ストア ファイル形式を指定します。 既定値は `pkcs12` です。

  Xamarin.Android 10.2 で追加されました。

- **AndroidKeyStore** &ndash; カスタム署名情報を使用するかどうかを示すブール値。 既定値は `False` で、既定のデバッグ署名キーがパッケージの署名に使用されることを意味します。

- **AndroidSigningKeyAlias** &ndash; キーストア内のキーに別名を指定します。 これは、キーストアを作成するときに使用される **keytool -alias** 値です。

- **AndroidSigningKeyPass** &ndash;キーストア ファイル内にあるキーのパスワードを指定します。 これは、`keytool` が **Enter key password for $(AndroidSigningKeyAlias)** ($(AndroidSigningKeyAlias) のキーのパスワードを入力) を求めたときに入力される値です。

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
  > `env:` プレフィックスは、`$(AndroidPackageFormat)` が `aab` に設定されている場合はサポートされません。

- **AndroidSigningKeyStore** &ndash;`keytool` で作成されたキーストア ファイルのファイル名を指定します。 これは、**keytool -keystore** オプションで指定された値に対応します。

- **AndroidSigningStorePass** &ndash;`$(AndroidSigningKeyStore)` へのパスワードを指定します。 これは、キーストア ファイルを作成していて、**Enter keystore password:** (キーストア パスワードを入力) で求められたときに、`keytool` に指定する値です。

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
  > `env:` プレフィックスは、`$(AndroidPackageFormat)` が `aab` に設定されている場合はサポートされません。

- **JarsignerTimestampAuthorityCertificateAlias** &ndash; このプロパティを使用すると、タイムスタンプ機関のキーストアに別名を指定できます。
  詳細については、Java の[署名のタイムスタンプのサポート](https://docs.oracle.com/javase/8/docs/technotes/guides/security/time-of-signing.html)に関するドキュメントを参照してください。

  ```xml
  <PropertyGroup>
      <JarsignerTimestampAuthorityCertificateAlias>Alias</JarsignerTimestampAuthorityCertificateAlias>
  </PropertyGroup>
  ```

- **JarsignerTimestampAuthorityUrl** &ndash; このプロパティでは、タイムスタンプ機関サービスへの URL を指定できます。 これを使用して、`.apk` 署名に確実にタイムスタンプを含めるようにすることができます。
  詳細については、Java の[署名のタイムスタンプのサポート](https://docs.oracle.com/javase/8/docs/technotes/guides/security/time-of-signing.html)に関するドキュメントを参照してください。

  ```xml
  <PropertyGroup>
      <JarsignerTimestampAuthorityUrl>http://example.tsa.url</JarsignerTimestampAuthorityUrl>
  </PropertyGroup>
  ```

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

<a name="Build_Actions"></a>

## <a name="build-actions"></a>ビルド アクション

*ビルド アクション*は、プロジェクト内の[ファイルに適用](https://docs.microsoft.com/visualstudio/msbuild/common-msbuild-project-items)され、ファイルの処理方法を制御します。

### <a name="androidaarlibrary"></a>AndroidAarLibrary

.aar ファイルを直接参照するには、`AndroidAarLibrary` のビルド アクションを使用する必要があります。 このビルド アクションは、Xamarin コンポーネントで最もよく使用されます。 つまり、Google Play やその他のサービスを動作させるのに必要な .aar ファイルへの参照を含めるためです。

このビルド アクションを使用するファイルは、ライブラリ プロジェクトで見つかる埋め込みリソースと同様に扱われます。 .aar は、中間ディレクトリに抽出されます。 次に、すべてのアセット、リソース、.jar ファイルが、適切な項目グループに含まれます。

### <a name="androidboundlayout"></a>AndroidBoundLayout

`AndroidGenerateLayoutBindings` プロパティが `false` に設定された場合に、レイアウト ファイルにコードビハインドが生成されることを示します。 その他すべての面において、これは上記で説明された `AndroidResource` と同じです。 このアクションは、次のレイアウト ファイルで**のみ**使用できます。

```xml
<AndroidBoundLayout Include="Resources\layout\Main.axml" />
```

<a name="AndroidEnvironment"></a>

### <a name="androidenvironment"></a>AndroidEnvironment

`AndroidEnvironment` のビルド アクションを持つファイルは、[プロセスの起動時に環境変数とシステム プロパティを初期化する](~/android/deploy-test/environment.md)ために使用されます。
`AndroidEnvironment` ビルド アクションは、複数のファイルに適用でき、任意の順序で評価されます (そのため、複数のファイルで同じ環境変数またはシステム プロパティを指定しないでください)。

### <a name="androidfragmenttype"></a>AndroidFragmentType

レイアウト バインディング コードを生成するときに、すべての `<fragment>` レイアウト要素に使用される既定の完全修飾型を指定します。 プロパティの既定値は、標準の Android `Android.App.Fragment` 型になります。

### <a name="androidjavalibrary"></a>AndroidJavaLibrary

`AndroidJavaLibrary` のビルド アクションを持つファイルは、最終的な Android パッケージに含まれる Java アーカイブ (`.jar` ファイル) です。

### <a name="androidjavasource"></a>AndroidJavaSource

`AndroidJavaSource` のビルド アクションを持つファイルは、最終的な Android パッケージに含まれる Java ソース コードです。

### <a name="androidlintconfig"></a>AndroidLintConfig

ビルド アクション 'AndroidLintConfig' は、`AndroidLintEnabled` ビルド プロパティと組み合わせて使用する必要があります。 このビルド アクションを含むファイルはマージされ、Android の `lint` ツールに渡されます。 これは、どのテストを有効および無効にするかに関する情報を含んだ XML ファイルになります。

詳細については、[Lint に関するドキュメント](https://developer.android.com/studio/write/lint)を参照してください。

### <a name="androidnativelibrary"></a>AndroidNativeLibrary

[ネイティブ ライブラリ](~/android/platform/native-libraries.md)は、そのビルド アクションを `AndroidNativeLibrary` に設定することで、ビルドに追加されます。

Android では、複数のアプリケーション バイナリ インターフェイス (ABI) をサポートしているため、ビルド システムではネイティブ ライブラリがどの ABI に対してビルドされているかを知る必要があります。 これを行うには 2 つの方法があります。

1. パス "スニッフィング"。
2. `Abi` 項目属性の使用。

パス スニッフィングを使用すると、ネイティブ ライブラリの親ディレクトリ名が、ライブラリがターゲットとする ABI を指定するために使用されます。 したがって、ビルドに `lib/armeabi-v7a/libfoo.so` を追加すると、ABI は `armeabi-v7a` として "スニッフィング" されます。

#### <a name="item-attribute-name"></a>項目属性名

**Abi** &ndash; ネイティブ ライブラリの ABI を指定します。

```xml
<ItemGroup>
  <AndroidNativeLibrary Include="path/to/libfoo.so">
    <Abi>armeabi-v7a</Abi>
  </AndroidNativeLibrary>
</ItemGroup>
```

### <a name="androidresource"></a>AndroidResource

*AndroidResource* ビルド アクションを持つすべてのファイルは、ビルド プロセス中に Android リソースにコンパイルされ、`$(AndroidResgenFile)` を介してアクセスできます。

```xml
<ItemGroup>
  <AndroidResource Include="Resources\values\strings.xml" />
</ItemGroup>
```

上級ユーザーであれば、同じ効果的なパスを使用して、別の構成で使用されている別のリソースを使用したいと考えるかもしれません。 これは、複数のリソース ディレクトリとファイルにこれらの異なるディレクトリ内で同じ相対パスを持たせ、MSBuild 条件を使用して、異なる構成の異なるファイルを条件付きで含めることで実現できます。 次に例を示します。

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

### <a name="androidresourceanalysisconfig"></a>AndroidResourceAnalysisConfig

ビルド アクション `AndroidResourceAnalysisConfig` は、Xamarin Android Designer レイアウト診断ツールの重大度レベルの構成ファイルとしてファイルをマークします。 これは現在、レイアウト エディターでのみ使用され、ビルド メッセージでは使用されません。

詳細については、[Android リソース分析のドキュメント](https://aka.ms/androidresourceanalysis)を参照してください。

Xamarin.Android 10.2 で追加されました。

### <a name="content"></a>コンテンツ

通常の `Content` ビルド アクションはサポートされていません (コストのかかる可能性がある最初の実行手順を行わずにサポートする方法が見つかっていないからです)。

Xamarin.Android 5.1 以降では、`@(Content)` ビルド アクションを使用しようとすると、`XA0101` 警告が発生します。

### <a name="linkdescription"></a>LinkDescription

*LinkDescription* ビルド アクションを持つファイルは、[リンカーの動作を制御](~/cross-platform/deploy-test/linker.md)するために使用されます。

<a name="ProguardConfiguration"></a>

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
