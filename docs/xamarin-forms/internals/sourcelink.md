---
title: Xamarin. Forms を使用したソースリンク
description: この記事では、ソースリンクを使用して Xamarin. Forms にデバッグする方法について説明します。
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetId: 1E13FCD9-5607-46E8-80E4-87A58B389BEB
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 09/26/2019
ms.openlocfilehash: 7a3fe70c8ac29f9e84b5d071a0ba1ef73afbbe11
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697807"
---
# <a name="source-link-with-xamarinforms"></a>Xamarin. Forms を使用したソースリンク

Xamarin. フォーム NuGet パッケージには、ソースリンクマッピングが含まれます。 ソースリンクは、NuGet パッケージに含まれるコンパイル済みライブラリをソースコードリポジトリにマップします。 Visual Studio では、デバッグ中にソースコードファイルをダウンロードし、開発者がコードをステップ実行して、ソースからビルドしなくてもパッケージのデバッグを有効にすることができます。

ソースリンクの使用の詳細については、[ソースリンクのドキュメント](/dotnet/standard/library-guidance/sourcelink)を参照してください。

::: zone pivot="windows"

> [!WARNING]
> Visual Studio 2019 では、 **.net デバッガー**のソースリンクがサポートされていますが、現在**Mono デバッガー**のソースリンクはサポートされていません。 そのため、ソースリンクを使用して UWP アプリをデバッグできますが、Android や iOS のアプリはデバッグできません。 UWP アプリのデバッグ時には、デバッグするライブラリの PDB ファイルが、アプリがコンパイルされる**bin**ディレクトリ内の**AppX**フォルダーにコピーされていることを確認する必要があります。

## <a name="enable-source-link"></a>ソースリンクを有効にする

ソースリンクを使用するには、外部コードのデバッグを有効にする必要があります。そうしないと、デバッガーは、現在のソリューションに含まれていないコードの呼び出しをステップ実行します。 Visual Studio 2019 では、これは **[デバッグ]** セクションの **[オプション]** メニューにあります。

[Visual Studio 2019 での ![Enable ソースリンク](sourcelink-images/sourcelink-enable-pc-cropped.png)](sourcelink-images/sourcelink-enable-pc.png#lightbox)

**[マイコードのみを有効**にする] が無効で、[**ソースリンクのサポート**を有効にする] が有効になっていることを確認します。

::: zone-end
::: zone pivot="macos"

## <a name="enable-source-link"></a>ソースリンクを有効にする

ソースリンクを使用するには、外部コードのデバッグを有効にする必要があります。そうしないと、デバッガーは、現在のソリューションに含まれていないコードの呼び出しをステップ実行します。 このオプションは、 **[デバッガー]** セクションの **[基本設定]** ウィンドウにあります。

[Visual Studio for Mac 内の ![Enable ソースリンク](sourcelink-images/sourcelink-enable-mac-cropped.png)](sourcelink-images/sourcelink-enable-mac.png#lightbox)

**外部コードへのステップイン**が有効になっていることを確認します。

::: zone-end

## <a name="debug-xamarinforms-using-source-link"></a>ソースリンクを使用して Xamarin. フォームをデバッグする

外部パッケージのデバッグが有効になっている場合、Visual Studio は、NuGet パッケージに含まれているソースリンクマッピングを使用して、外部ソースコードをダウンロードしてステップ実行します。 これをテストするには、Xamarin によって提供されるメソッドの呼び出しにブレークポイントを設定します。

[Xamarin. Forms メソッドに設定された ![Breakpoint](sourcelink-images/breakpoint-cropped.png)](sourcelink-images/external-code-available.png#lightbox)

**デバッガー**オプションで指定した設定に応じて、Visual Studio によってソースファイルがダウンロードされていることが警告されます。

[![Visual Studio の外部コードの警告](sourcelink-images/external-code-cropped.png)](sourcelink-images/external-code-available.png#lightbox)

Visual Studio によるファイルのダウンロードを許可すると、デバッガーは外部コードにステップインします。

::: zone pivot="windows"

## <a name="source-link-caching"></a>ソースリンクのキャッシュ

ソースリンクは、パフォーマンスのためにキャッシュを使用します。 ソースリンクのキャッシュディレクトリは、**シンボル**セクションの **[デバッグ]** の **[オプション]** メニューで定義されています。

[![Visual Studio のソースリンクキャッシュ](sourcelink-images/sourcelink-caching-pc-cropped.png)](sourcelink-images/sourcelink-caching-pc.png#lightbox)

このメニューでは、すべてのデバッグシンボルのキャッシュディレクトリを指定したり、キャッシュされたシンボルで問題が発生した場合にキャッシュをクリアしたりできます。

::: zone-end
::: zone pivot="macos"

## <a name="source-link-caching"></a>ソースリンクのキャッシュ

ソースリンクは、パフォーマンスのためにキャッシュを使用します。 MacOS のソースリンクのキャッシュディレクトリが `/Users/<username>/Library/Caches/VisualStudio/8.0/Symbols` ます。 このフォルダーには、ソースファイルのダウンロードに使用されるリポジトリを格納するサブフォルダーが含まれています。 NuGet パッケージのバッキングリポジトリが変更されている場合は、これらのフォルダーを手動で削除してキャッシュを更新することが必要になる場合があります。

::: zone-end

## <a name="related-links"></a>関連リンク

- [ソースリンクのドキュメント](/dotnet/standard/library-guidance/sourcelink)
- [GitHub のソースリンク](https://github.com/dotnet/sourcelink)
