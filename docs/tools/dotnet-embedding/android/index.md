---
title: Android での .NET の埋め込み
ms.prod: xamarin
ms.assetid: EB2F967A-6D95-4448-994B-6D5C7BFAC2C7
author: davidortinau
ms.author: daortin
ms.date: 06/15/2018
ms.openlocfilehash: fef422b799ab5280aef205f4d5e55fd91050da39
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73007339"
---
# <a name="net-embedding-on-android"></a>Android での .NET の埋め込み

場合によっては、既存のネイティブ Android プロジェクトに Xamarin .NET ライブラリを追加する必要があります。 これを行うには、 [Embeddinator-4000](https://www.nuget.org/packages/Embeddinator-4000/)ツールを使用して、.net ライブラリをネイティブの Java ベースの Android アプリに組み込むことができるネイティブライブラリに変換します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="xamarinandroid-requirements"></a>Xamarin. Android の要件

Xamarin Android で .NET 埋め込みを使用するには、次のものが必要です。

- Xamarin **. android** &ndash;[7.5](https://visualstudio.microsoft.com/xamarin/)以降がインストールされている必要があります。

- **Android Studio** &ndash;   [Android Studio](https://developer.android.com/studio/) 3.x 以降がインストールされている必要があります。

- Java **Developer Kit** &ndash;[java 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)以降がインストールされている必要があります。

## <a name="using-embeddinator-4000"></a>Embeddinator-4000 の使用

ネイティブ Android プロジェクトで .NET ライブラリを使用するには、次の手順を実行します。

1. Android ライブラリC#プロジェクトを作成します。

2. [Embeddinator-4000](https://www.nuget.org/packages/Embeddinator-4000/)をインストールします。

3. **Embeddinator-4000**を見つけて、**パス**に追加します。 (例:

    ```cmd
    set PATH=%PATH%;C:\Users\USERNAME\.nuget\packages\embeddinator-4000\0.4.0\tools
    ```

4. ライブラリアセンブリで Embeddinator-4000 を実行します。 (例:

    ```cmd
    Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5. Android Studio の Java プロジェクトで生成された AAR ファイルを使用します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="xamarinandroid-requirements"></a>Xamarin. Android の要件

Xamarin Android で .NET 埋め込みを使用するには、次のものが必要です。

- Xamarin **. android** &ndash;[7.5](https://visualstudio.microsoft.com/xamarin/)以降がインストールされている必要があります。

- **Android Studio** &ndash;   [Android Studio](https://developer.android.com/studio/) 3.x 以降がインストールされている必要があります。

- Java **Developer Kit** &ndash;[java 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)以降がインストールされている必要があります。

- **Mono** &ndash;   [mono 5.0](https://www.mono-project.com/download/)以降がインストールされている必要があります (mono は Visual Studio for Mac と共にインストールされます)。

## <a name="using-embeddinator-4000"></a>Embeddinator-4000 の使用

ネイティブ Android プロジェクトで .NET ライブラリを使用するには、次の手順を実行します。

1. Android ライブラリC#プロジェクトを作成します。

2. [Embeddinator-4000](https://www.nuget.org/packages/Embeddinator-4000/)をインストールします。

3. **Embeddinator-4000**を見つけて、パスに**mono**を追加します。 (例:

    ```bash
    export TOOLS=~/.nuget/packages/embeddinator-4000/0.4.0/tools
    export PATH=$PATH:/Library/Frameworks/Mono.framework/Commands
    ```

4. ライブラリアセンブリで Embeddinator-4000 を実行します。 (例:

    ```bash
    mono $TOOLS/Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5. Android Studio の Java プロジェクトで生成された AAR ファイルを使用します。

-----

使用法とコマンドラインのオプションについては、 [Embeddinator-4000](https://github.com/mono/Embeddinator-4000/blob/master/Usage.md#java--c)のドキュメントを参照してください。

## <a name="callbacks"></a>関数

[と Java の間でC#呼び出しを行う](callbacks.md)方法について説明します。

## <a name="samples"></a>サンプル

- [Weather サンプルアプリ](https://github.com/jamesmontemagno/embeddinator-weather)
