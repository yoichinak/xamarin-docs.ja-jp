---
title: ビルド項目
description: このドキュメントでは、Xamarin.Android ビルド プロセスでサポートされているすべての項目グループを一覧表示します。
ms.prod: xamarin
ms.assetid: 5EBEE1A5-3879-45DD-B1DE-5CD4327C2656
ms.technology: xamarin-android
author: jonpryor
ms.author: jopryo
ms.date: 09/23/2020
ms.openlocfilehash: 90efe2533f971180124d044ec39ddcf1591b9d36
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91455041"
---
# <a name="build-items"></a>ビルド項目

ビルド項目によって、Xamarin Android アプリケーションまたはライブラリ プロジェクトのビルド方法が制御されます。

## <a name="androidasset"></a>AndroidAsset

Java Android プロジェクトの `assets` フォルダーに含まれるファイルである [Android アセット](https://developer.android.com/guide/topics/resources/providing-resources#OriginalFiles)をサポートしています。

## <a name="androidaarlibrary"></a>AndroidAarLibrary

`.aar` ファイルを直接参照するには、`AndroidAarLibrary` のビルド アクションを使用する必要があります。 このビルド アクションは、Xamarin コンポーネントで最もよく使用されます。 つまり、Google Play やその他のサービスを動作させるために必要な `.aar` ファイルへの参照を含めるためです。

このビルド アクションを使用するファイルは、ライブラリ プロジェクトで見つかる埋め込みリソースと同様に扱われます。 `.aar` は、中間ディレクトリに抽出されます。 次に、すべてのアセット、リソース、`.jar` ファイルが、適切な項目グループに含まれます。

## <a name="androidaotprofile"></a>AndroidAotProfile

ガイド付き AOT のプロファイルでの使用を目的に、AOT プロファイルを提供するために使用されます。

## <a name="androidboundlayout"></a>AndroidBoundLayout

`AndroidGenerateLayoutBindings` プロパティが `false` に設定された場合に、レイアウト ファイルにコードビハインドが生成されることを示します。 その他すべての面において、これは上記で説明された `AndroidResource` と同じです。 このアクションは、次のレイアウト ファイルで**のみ**使用できます。

```xml
<AndroidBoundLayout Include="Resources\layout\Main.axml" />
```

## <a name="androidenvironment"></a>AndroidEnvironment

`AndroidEnvironment` のビルド アクションを持つファイルは、[プロセスの起動時に環境変数とシステム プロパティを初期化する](~/android/deploy-test/environment.md)ために使用されます。
`AndroidEnvironment` ビルド アクションは、複数のファイルに適用でき、任意の順序で評価されます (そのため、複数のファイルで同じ環境変数またはシステム プロパティを指定しないでください)。

## <a name="androidfragmenttype"></a>AndroidFragmentType

レイアウト バインディング コードを生成するときに、すべての `<fragment>` レイアウト要素に使用される既定の完全修飾型を指定します。 プロパティの既定値は、標準の Android `Android.App.Fragment` 型になります。

## <a name="androidjavalibrary"></a>AndroidJavaLibrary

`AndroidJavaLibrary` のビルド アクションを持つファイルは、最終的な Android パッケージに含まれる Java アーカイブ (`.jar` ファイル) です。

## <a name="androidjavasource"></a>AndroidJavaSource

`AndroidJavaSource` のビルド アクションを持つファイルは、最終的な Android パッケージに含まれる Java ソース コードです。

## <a name="androidlintconfig"></a>AndroidLintConfig

ビルド アクション 'AndroidLintConfig' は、[`$(AndroidLintEnabled)`](~/android/deploy-test/building-apps/build-properties.md#androidlintenabled) プロパティと組み合わせて使用する必要があります。
プロパティに基づきます)。 このビルド アクションを含むファイルはマージされ、Android の `lint` ツールに渡されます。 これは、どのテストを有効および無効にするかに関する情報を含んだ XML ファイルになります。

詳細については、[Lint に関するドキュメント](https://developer.android.com/studio/write/lint)を参照してください。

## <a name="androidnativelibrary"></a>AndroidNativeLibrary

[ネイティブ ライブラリ](~/android/platform/native-libraries.md)は、そのビルド アクションを `AndroidNativeLibrary` に設定することで、ビルドに追加されます。

Android では、複数のアプリケーション バイナリ インターフェイス (ABI) をサポートしているため、ビルド システムではネイティブ ライブラリがどの ABI に対してビルドされているかを知る必要があります。 これを行うには 2 つの方法があります。

1. パス "スニッフィング"。
2. `Abi` 項目属性の使用。

パス スニッフィングを使用すると、ネイティブ ライブラリの親ディレクトリ名が、ライブラリがターゲットとする ABI を指定するために使用されます。 したがって、ビルドに `lib/armeabi-v7a/libfoo.so` を追加すると、ABI は `armeabi-v7a` として "スニッフィング" されます。

### <a name="item-attribute-name"></a>項目属性名

**Abi** &ndash; ネイティブ ライブラリの ABI を指定します。

```xml
<ItemGroup>
  <AndroidNativeLibrary Include="path/to/libfoo.so">
    <Abi>armeabi-v7a</Abi>
  </AndroidNativeLibrary>
</ItemGroup>
```

## <a name="androidresource"></a>AndroidResource

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

## <a name="androidresourceanalysisconfig"></a>AndroidResourceAnalysisConfig

ビルド アクション `AndroidResourceAnalysisConfig` は、Xamarin Android Designer レイアウト診断ツールの重大度レベルの構成ファイルとしてファイルをマークします。 これは現在、レイアウト エディターでのみ使用され、ビルド メッセージでは使用されません。

詳細については、[Android リソース分析のドキュメント](../../user-interface/android-designer/diagnostics.md)を参照してください。

Xamarin.Android 10.2 で追加されました。

## <a name="content"></a>コンテンツ

通常の `Content` ビルド アクションはサポートされていません (コストのかかる可能性がある最初の実行手順を行わずにサポートする方法が見つかっていないからです)。

Xamarin.Android 5.1 以降では、`@(Content)` ビルド アクションを使用しようとすると、`XA0101` 警告が発生します。

## <a name="linkdescription"></a>LinkDescription

*LinkDescription* ビルド アクションを持つファイルは、[リンカーの動作を制御](~/cross-platform/deploy-test/linker.md)するために使用されます。

## <a name="proguardconfiguration"></a>ProguardConfiguration

*ProguardConfiguration* ビルド アクションを持つファイルは、`proguard` 動作を制御するために使用されます。 このビルド アクションの詳細については、「[ProGuard](~/android/deploy-test/release-prep/proguard.md)」を参照してください。

これらのファイルは、[`$(EnableProguard)`](~/android/deploy-test/building-apps/build-properties.md#enableproguard)
MSBuild プロパティが `True` でない場合は無視されます。