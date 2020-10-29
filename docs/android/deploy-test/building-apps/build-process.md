---
title: ビルド プロセス
description: このドキュメントでは、Xamarin.Android ビルド プロセスの概要について説明します。
ms.prod: xamarin
ms.assetid: 3BE5EE1E-3FF6-4E95-7C9F-7B443EE3E94C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 09/11/2020
ms.openlocfilehash: 4a89cfbb2406b6a5cda125044d43736dfa02d791
ms.sourcegitcommit: 01ccefd54c0ced724784dbe1aec9ecfc9b00e633
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92630232"
---
# <a name="build-process"></a>ビルド プロセス

Xamarin.Android ビルド プロセスは、[`Resource.designer.cs` の生成](~/android/internals/api-design.md)、[`@(AndroidAsset)`](~/android/deploy-test/building-apps/build-items.md#androidasset)、[`@(AndroidResource)`](~/android/deploy-test/building-apps/build-items.md#androidresource)、およびその他の[ビルド アクション](~/android/deploy-test/building-apps/build-items.md)のサポート、[Android 呼び出し可能なラッパー](~/android/platform/java-integration/android-callable-wrappers.md)の生成、Android デバイスでの実行に向けた `.apk` の生成の、すべてを結び付ける役割を担っています。

## <a name="application-packages"></a>アプリケーション パッケージ

Xamarin.Android ビルド システムが生成できる Android アプリケーション パッケージ (`.apk` ファイル) には、大まかに次の 2 種類があります。

- **リリース** ビルドは、実行に追加のパッケージを必要としない、完全な自己完結型です。 App Store に提供されるのはこのパッケージです。

- **デバッグ** ビルドではありません。

偶然ではなく、これらはパッケージを生成する MSBuild `Configuration` が一致しています。

## <a name="shared-runtime"></a>共有ランタイム

*共有ランタイム* は、基本クラス ライブラリ (`mscorlib.dll` など) と Android バインド ライブラリ (`Mono.Android.dll` など) を提供する追加の Android パッケージのペアです。 デバッグ ビルドは共有ランタイムに依存して、基本クラス ライブラリとバインド アセンブリを Android アプリケーション パッケージ内に含める代わりに、デバッグ パッケージをより小さくできるようにします。

共有ランタイムは、[`$(AndroidUseSharedRuntime)`](~/android/deploy-test/building-apps/build-properties.md#androidusesharedruntime)
プロパティを `False` に設定することで、デバッグ ビルドで無効にすることができます。

<a name="Fast_Deployment"></a>

## <a name="fast-deployment"></a>高速展開

*高速展開* は、共有ランタイムと連携して、Android アプリケーション パッケージのサイズをさらに縮小します。 これはアプリのアセンブリをパッケージ内にバンドルせずに、 `adb push` を介してターゲットにコピーすることで行います。 このプロセスにより、ビルド/展開/デバッグのサイクルが高速化されます。アセンブリ *のみ* が変更された場合、パッケージは再インストールされず、 代わりに、更新されたアセンブリだけがターゲット デバイスに再同期されるからです。

高速展開は、`adb` のディレクトリ `/data/data/@PACKAGE_NAME@/files/.__override__` への同期をブロックするデバイスでは失敗することがわかっています。

高速展開は、既定で有効になっており、`$(EmbedAssembliesIntoApk)` プロパティを `True` に設定することで、デバッグ ビルドで無効にすることができます。

## <a name="msbuild-projects"></a>MSBuild プロジェクト

Xamarin.Android ビルド プロセスは、Visual Studio for Mac と Visual Studio で使用されるプロジェクト ファイル形式でもある MSBuild に基づいています。
通常、ユーザーは、MSBuild ファイルを手動で編集する必要はありません &ndash; IDE によって完全に機能するプロジェクトが作成され、変更があれば更新され、必要に応じて自動的にビルド ターゲットが呼び出されます。

上級ユーザーが IDE の GUI でサポートされていないことを行えるように、ビルド プロセスは、プロジェクト ファイルを直接編集することでカスタマイズできるようになっています。
このページでは、Xamarin.Android 固有の機能とカスタマイズのみを文書化しています &ndash; 通常の MSBuild 項目、プロパティおよびターゲットを使用して、さらに多くのことができます。

<a name="Build_Targets"></a>

## <a name="binding-projects"></a>プロジェクトのバインド

次の MSBuild プロパティは、[バインド プロジェクト](~/android/platform/binding-java-library/index.md)で使用されます。

- [`$(AndroidClassParser)`](~/android/deploy-test/building-apps/build-properties.md#androidclassparser)
- [`$(AndroidCodegenTarget)`](~/android/deploy-test/building-apps/build-properties.md#androidcodegentarget)

## <a name="resourcedesignercs-generation"></a>`Resource.designer.cs` の生成

次の MSBuild プロパティは、`Resource.designer.cs` ファイルの生成を制御するために使用されます。

- [`$(AndroidAapt2CompileExtraArgs)`](~/android/deploy-test/building-apps/build-properties.md#androidaapt2compileextraargs)
- [`$(AndroidAapt2LinkExtraArgs)`](~/android/deploy-test/building-apps/build-properties.md#androidaapt2linkextraargs)
- [`$(AndroidExplicitCrunch)`](~/android/deploy-test/building-apps/build-properties.md#androidexplicitcrunch)
- [`$(AndroidR8IgnoreWarnings)`](~/android/deploy-test/building-apps/build-properties.md#androidr8ignorewarnings)
- [`$(AndroidResgenExtraArgs)`](~/android/deploy-test/building-apps/build-properties.md#androidresgenextraargs)
- [`$(AndroidResgenFile)`](~/android/deploy-test/building-apps/build-properties.md#androidresgenfile)
- [`$(AndroidUseAapt2)`](~/android/deploy-test/building-apps/build-properties.md#androiduseaapt2)
- [`$(MonoAndroidResourcePrefix)`](~/android/deploy-test/building-apps/build-properties.md#monoandroidresourceprefix)

## <a name="signing-properties"></a>署名プロパティ

署名プロパティは、アプリケーション パッケージを Android デバイスにインストールできるように、パッケージに署名する方法を制御します。 ビルド イテレーションを高速化するため、Xamarin.Android タスクではビルド プロセス中のパッケージへの署名は行われません。署名を行うと非常に遅くなるからです。 代わりに、インストール前またはエクスポート中に、IDE または *Install* ビルド ターゲットによって署名されます (必要な場合)。 *SignAndroidPackage* ターゲットを呼び出すと、出力ディレクトリに `-Signed.apk` のサフィックスを持つパッケージが生成されます。

既定では、必要に応じて署名ターゲットによって新しいデバッグ署名キーが生成されます。 たとえば、ビルド サーバーなどで特定のキーを使用する場合は、次の MSBuild プロパティが使用されます。

- [`$(AndroidDebugKeyAlgorithm)`](~/android/deploy-test/building-apps/build-properties.md#androiddebugkeyalgorithm)
- [`$(AndroidDebugKeyValidity)`](~/android/deploy-test/building-apps/build-properties.md#androiddebugkeyvalidity)
- [`$(AndroidDebugStoreType)`](~/android/deploy-test/building-apps/build-properties.md#androiddebugstoretype)
- [`$(AndroidKeyStore)`](~/android/deploy-test/building-apps/build-properties.md#androidkeystore)
- [`$(AndroidSigningKeyAlias)`](~/android/deploy-test/building-apps/build-properties.md#androidsigningkeyalias)
- [`$(AndroidSigningKeyPass)`](~/android/deploy-test/building-apps/build-properties.md#androidsigningkeypass)
- [`$(AndroidSigningKeyStore)`](~/android/deploy-test/building-apps/build-properties.md#androidsigningkeystore)
- [`$(AndroidSigningStorePass)`](~/android/deploy-test/building-apps/build-properties.md#androidsigningstorepass)
- [`$(JarsignerTimestampAuthorityCertificateAlias)`](~/android/deploy-test/building-apps/build-properties.md#jarsignertimestampauthoritycertificatealias)
- [`$(JarsignerTimestampAuthorityUrl)`](~/android/deploy-test/building-apps/build-properties.md#jarsignertimestampauthorityurl)

### <a name="keytool-option-mapping"></a>`keytool` オプションのマッピング

次の `keytool` の呼び出しについて考えてみましょう。

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

拡張ポイントには、次のものが含まれます。

- [`$(AfterGenerateAndroidManifest)](~/android/deploy-test/building-apps/build-properties.md#aftergenerateandroidmanifest)
- [`$(BeforeGenerateAndroidManifest)](~/android/deploy-test/building-apps/build-properties.md#beforegenerateandroidmanifest)

ビルド プロセスの拡張に関する注意事項を次に示します。正しく記述されていないと、ビルドの拡張機能がビルドのパフォーマンスに影響を与える可能性があります (特に、拡張機能がすべてのビルドで実行される場合)。 このような拡張機能を実装する前に、MSBuild の[ドキュメント](/visualstudio/msbuild/msbuild)を読むことを強くお勧めします。

## <a name="target-definitions"></a>ターゲットの定義

ビルド プロセスの Xamarin.Android に固有の部分は、`$(MSBuildExtensionsPath)\Xamarin\Android\Xamarin.Android.CSharp.targets` で定義されますが、アセンブリをビルドするためには、 *Microsoft.CSharp.targets* などの通常の言語固有ターゲットも必要です。

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
