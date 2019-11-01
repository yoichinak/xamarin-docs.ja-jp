---
title: マルチコア デバイスと Xamarin.Android
description: Android は、複数の異なるコンピューター アーキテクチャで実行できます。 このドキュメントでは、Xamarin.Android アプリケーションに利用できるさまざまな CPU アーキテクチャについて説明します。 このドキュメントでは、さまざまな CPU アーキテクチャをサポートするために Android アプリケーションがどのようにパッケージ化されているかについても説明します。 アプリケーション バイナリ インターフェイス (ABI) が導入され、Xamarin.Android アプリケーションで使用する ABI に関するガイダンスが提供される予定です。
ms.prod: xamarin
ms.assetid: D812883C-A14A-E74B-0F72-E50071E96328
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/30/2019
ms.openlocfilehash: 1141b96151df0adda755b7c6d60019c18825cc76
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028015"
---
# <a name="multi-core-devices--xamarinandroid"></a>マルチコア デバイスと Xamarin.Android

_Android は、複数の異なるコンピューター アーキテクチャで実行できます。このドキュメントでは、Xamarin.Android アプリケーションに利用できるさまざまな CPU アーキテクチャについて説明します。このドキュメントでは、さまざまな CPU アーキテクチャをサポートするために Android アプリケーションがどのようにパッケージ化されているかについても説明します。アプリケーション バイナリ インターフェイス (ABI) が導入され、Xamarin.Android アプリケーションで使用する ABI に関するガイダンスが提供される予定です。_

## <a name="overview"></a>概要

Android では、"FAT バイナリ" を作成することができます。これは、複数の異なる CPU アーキテクチャをサポートするマシン コードを含む 1 つの `.apk` ファイルです。 これは、マシン コードの各部分を*アプリケーション バイナリ インターフェイス*と関連付けることで行います。 ABI は、特定のハードウェア デバイスでどのマシン コードを実行するかを制御するために使用されます。 たとえば、x86 デバイス上で実行する Android アプリケーションには、アプリケーションのコンパイル時に x86 ABI のサポートを含める必要があります。

具体的には、各 Android アプリケーションは 1 つ以上の*埋め込みアプリケーション バイナリ インターフェイス* (EABI) をサポートします。 EABI とは、埋め込みソフトウェア プログラムに固有の規則です。 一般的な EABI は次のようなことを記述します。

- MMX 命令セット。

- 実行時に格納および読み込むメモリのエンディアン。

- オブジェクト ファイルとプログラム ライブラリのバイナリ形式、およびこれらのファイルとライブラリで許可またはサポートされるコンテンツの種類。

- アプリケーション コードとシステム間でデータを渡すために使用されるさまざまな規則 (例: 関数が呼び出されるときのレジスタまたはスタックの使用方法、配置制約など)。

- 列挙型、構造体、フィールド、および配列の配置とサイズの制約。

- 実行時にマシン コードで使用できる (通常は厳選されたライブラリのセットからの) 関数シンボルの一覧。

### <a name="armeabi-and-thread-safety"></a>armeabi とスレッド セーフ

アプリケーション バイナリ インターフェイスについては以降で詳しく説明しますが、Xamarin.Android で使用される `armeabi` ランタイムは、*スレッド セーフではない*ことに注意してください。 `armeabi` をサポートしているアプリケーションを `armeabi-v7a` デバイスに展開する場合、変わった予期しない例外が数多く発生します。

Android 4.0.0、4.0.1、4.0.2、および 4.0.3 のバグにより、`armeabi-v7a` ディレクトリが存在し、デバイスが `armeabi-v7a` デバイスであっても、`armeabi` からネイティブ ライブラリが取得されます。

> [!NOTE]
> Xamarin.Android では、`.so` が正しい順序で APK に追加されます。 このバグは Xamarin.Android のユーザーにとっては問題にはなりません。

### <a name="abi-descriptions"></a>ABI の説明

Android でサポートされている各 ABI は、一意の名前で識別されます。

#### <a name="armeabi"></a>armeabi

これは、最低限 ARMv5TE 命令セットをサポートする ARM ベースの CPU の EABI の名前です。 Android では、リトル エンディアン ARM GNU/Linux ABI に従います。 この ABI は、ハードウェア依存の浮動小数点演算をサポートしていません。 すべての FP 演算は、コンパイラの `libgcc.a` スタティック ライブラリに由来するソフトウェア ヘルパー関数で実行されます。 SMP デバイスは `armeabi` でサポートされていません。

> [!IMPORTANT]
> Xamarin.Android の `armeabi` コードはスレッド セーフではないため、マルチ CPU の `armeabi-v7a` デバイスでは使用しないでください (以下で説明)。 シングル コアの `armeabi-v7a` デバイスで `armeabi` コードを使用するのは安全です。

#### <a name="armeabi-v7a"></a>armeabi-v7a

これは、上記で説明した `armeabi` EABI を拡張する別の ARM ベースの CPU 命令セットです。 `armeabi-v7a` EABI は、ハードウェアの浮動小数点演算と複数の CPU (SMP) デバイスをサポートしています。 `armeabi-v7a` EABI を使用するアプリケーションは、`armeabi` を使用するアプリケーションに比べて大幅なパフォーマンスの向上が期待できます。

> [!NOTE]
> `armeabi-v7a` マシン コードは、ARMv5 デバイスでは実行できません。

#### <a name="arm64-v8a"></a>arm64-v8a

これは、ARMv8 CPU アーキテクチャに基づく 64 ビット命令セットです。 このアーキテクチャは、*Nexus 9* で使用されています。
Xamarin.Android 5.1 ではこのアーキテクチャのサポートが導入されています (詳細については「[64-bit runtime support (64 ビット ランタイムのサポート)](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1/index.md#64-bit-runtime-support)」を参照)。

#### <a name="x86"></a>x86

これは、一般的な *x86* または *IA-32* という名前の命令セットをサポートする CPU の ABI の名前です。 この ABI は、Pentium Pro 命令セット (MMX、SSE、SSE2、および SSE3 の命令セットを含む) の命令に対応します。 次のような、その他のオプションの IA-32 命令セットの拡張機能は含まれません。

- MOVBE 命令。
- 追加の SSE3 拡張機能 (SSSE3)。
- SSE4 の任意のバリアント。

> [!NOTE]
> Google TV は x86 上で実行されますが、Android の NDK ではサポートされていません。

#### <a name="x86_64"></a>x86_64

これは、64 ビット x86 命令セット (*x64* または *AMD64* とも呼ばれます) をサポートする CPU の ABI の名前です。 Xamarin.Android 5.1 ではこのアーキテクチャのサポートが導入されています (詳細については「[64-bit runtime support (64 ビット ランタイムのサポート)](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1/index.md#64-bit-runtime-support)」を参照)。

#### <a name="apk-file-format"></a>APK ファイル形式

Android アプリケーション パッケージは、Android のアプリケーションに必要なコード、アセット、リソース、および証明書をすべて保持するファイル形式です。 これは `.zip` ファイルですが、`.apk` ファイル名拡張子を使用します。 展開すると、次のスクリーンショットのような、Xamarin.Android によって作成された `.apk` のコンテンツが表示されます。

[![.apk のコンテンツ](multicore-devices-images/00.png)](multicore-devices-images/00.png#lightbox)

`.apk` ファイルのコンテンツを簡単に説明します。

- **AndroidManifest.xml** &ndash; バイナリ XML 形式の `AndroidManifest.xml` ファイルです。

- **classes.dex** &ndash; Android ランタイム VM によって使用される `dex` ファイル形式にコンパイルされたアプリケーション コードが含まれます。

- **resources.arsc** &ndash; このファイルには、アプリケーションのプリコンパイル済みリソースがすべて含まれています。

- **lib** &ndash; このディレクトリには、各 ABI のコンパイル済みコードが保持されています。 前のセクションで説明した各 ABI に対して 1 つのサブフォルダーが格納されます。 上記のスクリーンショットでは、該当する `.apk` には `armeabi-v7a` と `x86` の両方のネイティブ ライブラリがあります。

- **META-INF** &ndash; このディレクトリ (存在する場合) は、署名情報、パッケージ、および拡張機能の構成データを格納するために使用されます。

- **res** &ndash; このディレクトリには、`resources.arsc` にコンパイルされなかったリソースが保持されます。

> [!NOTE]
> ファイル `libmonodroid.so` は、すべての Xamarin.Android アプリケーションで必要なネイティブ ライブラリです。

#### <a name="android-device-abi-support"></a>Android デバイスの ABI のサポート

各 Android デバイスは、最大 2 つの ABI でネイティブ コードの実行をサポートします。

- **"プライマリ" ABI** &ndash; これは、システム イメージで使用されるマシン コードに対応します。

- **"セカンダリ" ABI** &ndash; これも、システム イメージでサポートされるオプションの ABI です。

たとえば、典型的な ARMv5TE デバイスでは、`armeabi` のプライマリ ABI しかないのに対し、ARMv7 デバイスでは、`armeabi-v7a` のプライマリ ABI と `armeabi` のセカンダリ ABI を指定します。 典型的な x86 デバイスでは、`x86` のプライマリ ABI のみを指定します。

### <a name="android-native-library-installation"></a>Android のネイティブ ライブラリのインストール

パッケージのインストール時に、`.apk` 内のネイティブ ライブラリがアプリのネイティブ ライブラリのディレクトリに抽出されます。このディレクトリは、通常、`/data/data/<package-name>/lib` で、以降は `$APP/lib` とします。

Android のネイティブ ライブラリのインストール動作は、Android のバージョンによって大幅に異なります。

#### <a name="installing-native-libraries-pre-android-40"></a>ネイティブ ライブラリのインストール: Android 4.0 より前

Android 4.0 Ice Cream Sandwich より前のバージョンでは、`.apk` 内の *1 つの ABI* からのみネイティブ ライブラリを抽出します。 この古い Android アプリが最初にプライマリ ABI のすべてのネイティブ ライブラリの抽出を試みて、ライブラリが存在しない場合は、次に Android がセカンダリ ABI のすべてのネイティブ ライブラリを抽出します。 "マージ" は行われません。

たとえば、アプリケーションが `armeabi-v7a` デバイスにインストールされている状況を考えてみましょう。 `armeabi` と `armeabi-v7a` の両方をサポートする `.apk,` の中には、次の ABI `lib` ディレクトリとファイルがあります。

```shell
lib/armeabi/libone.so
lib/armeabi/libtwo.so
lib/armeabi-v7a/libtwo.so
```

インストール後、ネイティブ ライブラリ ディレクトリに次のものが含まれます。

```shell
$APP/lib/libtwo.so # from the armeabi-v7a directory in the apk
```

つまり、`libone.so` はインストールされません。 これは、実行時に読み込むアプリケーションの `libone.so` が存在しないため、問題が発生します。 この動作は予期しないものですが、バグとしてログに記録され、"[目的どおりに動作](https://code.google.com/p/android/issues/detail?id=9089)" として再分類されています。

その結果、Android 4.0 より前のバージョンを対象とする場合には、アプリケーションがサポートする*各* ABI に対して、*すべて*のネイティブ ライブラリを提供する必要があります。つまり、`.apk` を含める必要があります。

```shell
lib/armeabi/libone.so
lib/armeabi/libtwo.so
lib/armeabi-v7a/libone.so
lib/armeabi-v7a/libtwo.so
```

#### <a name="installing-native-libraries-android-40-ndash-android-403"></a>ネイティブ ライブラリのインストール: Android 4.0 &ndash; Android 4.0.3

Android 4.0 Ice Cream Sandwich では抽出ロジックが変更されています。 すべてのネイティブ ライブラリが列挙され、ファイルのベース名が既に抽出されているかどうか、および次の両方の条件が満たされているかどうかを確認してから、ライブラリが抽出されます。

- まだ抽出されていない。

- ネイティブ ライブラリの ABI がターゲットのプライマリまたはセカンダリ ABI と一致している。

これらの条件を満たすと、"マージ" 動作が可能になります。つまり、`.apk` と次のコンテンツがある場合、

```shell
lib/armeabi/libone.so
lib/armeabi/libtwo.so
lib/armeabi-v7a/libtwo.so
```

インストール後、ネイティブ ライブラリ ディレクトリに次のものが含まれます。

```shell
$APP/lib/libone.so
$APP/lib/libtwo.so
```

ただし、この動作は次のドキュメントで説明されているように順序依存です: [問題 24321: apk に armeabi と armeabi v7a の両方が含まれている場合、Galaxy Nexus 4.0.2 では armeabi ネイティブ コードが使用される](https://code.google.com/p/android/issues/detail?id=25321)

ネイティブ ライブラリは、(たとえば、unzip で一覧表示されているように) "順番に" 処理され、*最初の一致*が抽出されます。 `.apk` には `libtwo.so` の `armeabi` バージョンと `armeabi-v7a` バージョンが含まれており、`armeabi` が最初にリストされているため、`armeabi-v7a` バージョン*ではなく*、`armeabi` バージョンが抽出されます。

```shell
$APP/lib/libone.so # armeabi
$APP/lib/libtwo.so # armeabi, NOT armeabi-v7a!
```

さらに、(後述のセクション「*サポートされる ABI の宣言*」で説明されているように) `armeabi` と `armeabi-v7a` の両方の ABI が指定されている場合でも、Xamarin.Android では以下に含まれる次の要素が作成されます。
`csproj`:

```xml
<AndroidSupportedAbis>armeabi,armeabi-v7a</AndroidSupportedAbis>
```

その結果、`armeabi` `libmonodroid.so` は最初に `.apk` 内で見つかり、`armeabi-v7a` `libmonodroid.so`が存在し、ターゲット用に最適化されていても、抽出されるのは `armeabi` `libmonodroid.so` になります。 `armeabi` が SMP セーフではないため、あいまいな実行時エラーが発生する可能性もあります。

##### <a name="installing-native-libraries-android-404-and-later"></a>ネイティブ ライブラリのインストール: Android 4.0.4 以降

Android 4.0.4 では、抽出ロジックが変更されています。すべてのネイティブ ライブラリが列挙され、ファイルのベース名の読み取り後、プライマリ ABI バージョン (存在する場合)、またはセカンダリ ABI (存在する場合) が抽出されます。 これにより、"マージ" 動作が可能になります。つまり、`.apk` と次のコンテンツがある場合、

```shell
lib/armeabi/libone.so
lib/armeabi/libtwo.so
lib/armeabi-v7a/libtwo.so
```

インストール後、ネイティブ ライブラリ ディレクトリに次のものが含まれます。

```shell
$APP/lib/libone.so # from armeabi
$APP/lib/libtwo.so # from armeabi-v7a
```

### <a name="xamarinandroid-and-abis"></a>Xamarin.Android と ABI

Xamarin.Android では、次の "_64 ビット_" アーキテクチャがサポートされています。

- `arm64-v8a`
- `x86_64`

> [!NOTE]
> 2018 年 8 月から、新しいアプリは API レベル 26 をターゲットにすることが必須となります。また、2019 年 8 月から、アプリは 32 ビット バージョンに加えて [64 ビット バージョンを提供することが必須となります](https://android-developers.googleblog.com/2017/12/improving-app-security-and-performance.html)。

Xamarin.Android では、次の 32 ビット アーキテクチャがサポートされています。

- `armeabi` ^
- `armeabi-v7a`
- `x86`

> [!NOTE]
> **^** [Xamarin.Android 9.2](https://docs.microsoft.com/xamarin/android/release-notes/9/9.2#removal-of-support-for-armeabi-cpu-architecture) 以降、`armeabi` はサポート対象から除外されました。

Xamarin.Android では現在、`mips` をサポートしていません。

### <a name="declaring-supported-abis"></a>サポートされる ABI の宣言

既定では、Xamarin.Android の**リリース** ビルドには `armeabi-v7a` が既定となり、**デバッグ** ビルドには `armeabi-v7a` と `x86` が既定となります。 さまざまな ABI のサポートは、Xamarin.Android プロジェクトのプロジェクト オプションから設定できます。 Visual Studio では、次のスクリーンショットで示すように、これらの値は、プロジェクトの **[プロパティ]** の **[Android オプション]** ページの **[詳細設定]** タブの下で設定できます。

![Android オプションの詳細プロパティ](multicore-devices-images/vs-abi-selections.png)

Visual Studio for Mac では、次のスクリーンショットで示すように、サポートされるアーキテクチャは、 **[プロジェクト オプション]** の **[Android のビルド]** ページの **[詳細設定]** タブの下で選択できます。

[![Android のビルドのサポートされる ABI](multicore-devices-images/xs-abi-selections-sml.png)](multicore-devices-images/xs-abi-selections.png#lightbox)

状況によっては、追加の ABI のサポートを宣言する必要があります。たとえば次のような状況が考えられます。

- アプリケーションを `x86` デバイスに展開する。

- スレッド セーフを保証するためにアプリケーションを `armeabi-v7a` デバイスに展開する。

## <a name="summary"></a>まとめ

このドキュメントでは、Android アプリケーションを実行できるさまざまな CPU アーキテクチャについて説明しました。 アプリケーション バイナリ インターフェイスと、それが異なる CPU アーキテクチャをサポートするために Android でどのように使用されているかについて説明しました。
また、Xamarin.Android アプリケーションでの ABI サポートを指定する方法について説明し、Xamarin.Android アプリケーションを `armeabi` のみを対象とした `armeabi-v7a` デバイスで使用するときに発生する問題を明らかにしました。

## <a name="related-links"></a>関連リンク

- [ARM アーキテクチャの ABI (PDF)](http://infocenter.arm.com/help/topic/com.arm.doc.ihi0036b/IHI0036B_bsabi.pdf)
- [Android NDK](https://developer.android.com/tools/sdk/ndk/index.html)
- [問題 9089: Nexus One - armeabi v7a に 1 つ以上のライブラリがある場合、armeabi からどのネイティブ ライブラリも読み込まれない](https://code.google.com/p/android/issues/detail?id=9089)
- [問題 24321: apk に armeabi と armeabi v7a の両方が含まれている場合、Galaxy Nexus 4.0.2 では armeabi ネイティブ コードが使用される](https://code.google.com/p/android/issues/detail?id=25321)
