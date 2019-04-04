---
title: .NET の Android での埋め込み
ms.prod: xamarin
ms.assetid: EB2F967A-6D95-4448-994B-6D5C7BFAC2C7
author: lobrien
ms.author: laobri
ms.date: 06/15/2018
ms.openlocfilehash: 6917267896cff796af4e5cff095720eaeccc7652
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57671175"
---
# <a name="net-embedding-on-android"></a>.NET の Android での埋め込み

場合によっては、既存のネイティブの Android プロジェクトに Xamarin .NET ライブラリを追加することがあります。 これを行うには、使用することができます、 [Embeddinator 4000](https://www.nuget.org/packages/Embeddinator-4000/)ネイティブ Java ベースの Android アプリに組み込むことができるネイティブ ライブラリに .NET ライブラリを有効にするツール。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="xamarinandroid-requirements"></a>Xamarin.Android の要件

.NET の埋め込みを使用する Xamarin.Android では、次が必要です。

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.5](https://visualstudio.microsoft.com/xamarin/) or later must be installed.

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/)以降をインストールする必要があります。

-   **Java Developer Kit** &ndash; [Java 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)以降をインストールする必要があります。


## <a name="using-embeddinator-4000"></a>Embeddinator 4000 を使用します。

ネイティブの Android プロジェクトでの .NET ライブラリを使用するには、次の手順を使用します。

1.  作成、 C# Android ライブラリ プロジェクト。

2.  インストール[Embeddinator 4000](https://www.nuget.org/packages/Embeddinator-4000/)します。

3.  検索**Embeddinator 4000.exe**に追加し、**パス**します。 例:

    ```cmd
    set PATH=%PATH%;C:\Users\USERNAME\.nuget\packages\embeddinator-4000\0.4.0\tools
    ```

4.  ライブラリ アセンブリに Embeddinator 4000 を実行します。 例:

    ```cmd
    Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5.  Android Studio での Java プロジェクトで生成された AAR ファイルを使用します。


# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="xamarinandroid-requirements"></a>Xamarin.Android の要件

.NET の埋め込みを使用する Xamarin.Android では、次が必要です。

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.5](https://visualstudio.microsoft.com/xamarin/) or later must be installed.

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/)以降をインストールする必要があります。

-   **Java Developer Kit** &ndash; [Java 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)以降をインストールする必要があります。

-   **Mono** &ndash; [Mono 5.0](https://www.mono-project.com/download/)以降をインストールする必要があります (mono がインストールされている Visual studio for Mac)。


## <a name="using-embeddinator-4000"></a>Embeddinator 4000 を使用します。

ネイティブの Android プロジェクトでの .NET ライブラリを使用するには、次の手順を使用します。

1.  作成、 C# Android ライブラリ プロジェクト。

2.  インストール[Embeddinator 4000](https://www.nuget.org/packages/Embeddinator-4000/)します。

3.  検索**Embeddinator 4000.exe**追加**mono**をパスにします。 例:

    ```bash
    export TOOLS=~/.nuget/packages/embeddinator-4000/0.4.0/tools
    export PATH=$PATH:/Library/Frameworks/Mono.framework/Commands
    ```

4.  ライブラリ アセンブリに Embeddinator 4000 を実行します。 例:

    ```bash
    mono $TOOLS/Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5.  Android Studio での Java プロジェクトで生成された AAR ファイルを使用します。

-----

使用状況とコマンド ライン オプションの説明を[Embeddinator 4000](https://github.com/mono/Embeddinator-4000/blob/master/Usage.md#java--c)ドキュメント。


## <a name="callbacks"></a>コールバック

について[間呼び出しを行うC#と Java](callbacks.md)します。

## <a name="samples"></a>サンプル

* [天気のサンプル アプリ](https://github.com/jamesmontemagno/embeddinator-weather)
