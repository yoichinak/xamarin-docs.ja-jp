---
title: Android での埋め込み .NET
ms.prod: xamarin
ms.assetid: EB2F967A-6D95-4448-994B-6D5C7BFAC2C7
author: topgenorth
ms.author: toopge
ms.date: 06/15/2018
ms.openlocfilehash: e90d1e6258d4cfd9c918c566c9e18c358ee7668a
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/28/2018
ms.locfileid: "37067353"
---
# <a name="net-embedding-on-android"></a>Android での埋め込み .NET

場合によっては、既存のネイティブの Android プロジェクトに Xamarin .NET ライブラリを追加することがあります。 これを行うには、使用することができます、 [Embeddinator 4000](https://www.nuget.org/packages/Embeddinator-4000/)ネイティブ Java ベースの Android アプリに組み込むことがネイティブ ライブラリに .NET ライブラリを有効にするツールです。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="xamarinandroid-requirements"></a>Xamarin.Android 要件

.NET の埋め込みを使用する Xamarin.Android、次が必要です。

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.5](https://visualstudio.microsoft.com/xamarin/) or later must be installed.

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/)以降、インストールする必要があります。

-   **Java Developer Kit** &ndash; [Java 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)以降、インストールする必要があります。


## <a name="using-embeddinator-4000"></a>Embeddinator 4000 を使用します。

ネイティブの Android プロジェクトでの .NET ライブラリを使用するには、次の手順を使用します。

1.  C# での Android ライブラリ プロジェクトを作成します。

2.  インストール[Embeddinator 4000](https://www.nuget.org/packages/Embeddinator-4000/)です。

3.  検索**Embeddinator 4000.exe**に追加し、**パス**です。 例えば:

    ```cmd
    set PATH=%PATH%;C:\Users\USERNAME\.nuget\packages\embeddinator-4000\0.4.0\tools
    ```

4.  ライブラリのアセンブリで Embeddinator 4000 を実行します。 例えば:

    ```cmd
    Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5.  Android Studio での Java プロジェクトで生成された AAR ファイルを使用します。


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="xamarinandroid-requirements"></a>Xamarin.Android 要件

.NET の埋め込みを使用する Xamarin.Android、次が必要です。

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.5](https://visualstudio.microsoft.com/xamarin/) or later must be installed.

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/)以降、インストールする必要があります。

-   **Java Developer Kit** &ndash; [Java 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)以降、インストールする必要があります。

-   **モノラル** &ndash; [モノラル 5.0](http://www.mono-project.com/download/)以降、インストールする必要があります (for Mac モノラルが Visual Studio と共にインストールされます)。


## <a name="using-embeddinator-4000"></a>Embeddinator 4000 を使用します。

ネイティブの Android プロジェクトでの .NET ライブラリを使用するには、次の手順を使用します。

1.  C# での Android ライブラリ プロジェクトを作成します。

2.  インストール[Embeddinator 4000](https://www.nuget.org/packages/Embeddinator-4000/)です。

3.  検索**Embeddinator 4000.exe**追加**モノラル**をパスにします。 例えば:

    ```bash
    export TOOLS=~/.nuget/packages/embeddinator-4000/0.4.0/tools
    export PATH=$PATH:/Library/Frameworks/Mono.framework/Commands
    ```

4.  ライブラリのアセンブリで Embeddinator 4000 を実行します。 例えば:

    ```bash
    mono $TOOLS/Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5.  Android Studio での Java プロジェクトで生成された AAR ファイルを使用します。

-----

使用法とコマンド ライン オプションの説明を[Embeddinator 4000](https://github.com/mono/Embeddinator-4000/blob/master/Usage.md#java--c)ドキュメント。


## <a name="callbacks"></a>コールバック

について学習[c# や Java 間の呼び出しを行う](callbacks.md)です。

## <a name="samples"></a>サンプル

* [天気のサンプル アプリ](https://github.com/jamesmontemagno/embeddinator-weather)
