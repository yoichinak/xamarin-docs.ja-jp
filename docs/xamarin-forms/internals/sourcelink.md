---
title: ソースリンクXamarin.Forms
description: この記事では、ソースリンクを使用してにデバッグする方法について説明 Xamarin.Forms します。
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetId: 1E13FCD9-5607-46E8-80E4-87A58B389BEB
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 09/26/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 57db314538c42ef9d58691ba16ab68371ff092b7
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138314"
---
# <a name="source-link-with-xamarinforms"></a>ソースリンクXamarin.Forms

Xamarin.FormsNuGet パッケージには、ソースリンクマッピングが含まれます。 ソースリンクは、NuGet パッケージに含まれるコンパイル済みライブラリをソースコードリポジトリにマップします。 Visual Studio では、デバッグ中にソースコードファイルをダウンロードし、開発者がコードをステップ実行して、ソースからビルドしなくてもパッケージのデバッグを有効にすることができます。

ソースリンクの使用の詳細については、[ソースリンクのドキュメント](/dotnet/standard/library-guidance/sourcelink)を参照してください。

::: zone pivot="windows"

> [!WARNING]
> Visual Studio 2019 では、 **.net デバッガー**のソースリンクがサポートされていますが、現在**Mono デバッガー**のソースリンクはサポートされていません。 そのため、ソースリンクを使用して UWP アプリをデバッグできますが、Android や iOS のアプリはデバッグできません。 UWP アプリのデバッグ時には、デバッグするライブラリの PDB ファイルが、アプリがコンパイルされる**bin**ディレクトリ内の**AppX**フォルダーにコピーされていることを確認する必要があります。

## <a name="enable-source-link"></a>ソース リンクを有効にする

ソースリンクを使用するには、外部コードのデバッグを有効にする必要があります。そうしないと、デバッガーは、現在のソリューションに含まれていないコードの呼び出しをステップ実行します。 Visual Studio 2019 では、これは [**デバッグ**] セクションの [**オプション**] メニューにあります。

[![Visual Studio 2019 でソースリンクを有効にする](sourcelink-images/sourcelink-enable-pc-cropped.png)](sourcelink-images/sourcelink-enable-pc.png#lightbox)

**[マイコードのみを有効**にする] が無効で、[**ソースリンクのサポート**を有効にする] が有効になっていることを確認します。

::: zone-end
::: zone pivot="macos"

## <a name="enable-source-link"></a>ソース リンクを有効にする

ソースリンクを使用するには、外部コードのデバッグを有効にする必要があります。そうしないと、デバッガーは、現在のソリューションに含まれていないコードの呼び出しをステップ実行します。 このオプションは、[**デバッガー** ] セクションの [**基本設定**] ウィンドウにあります。

[![Visual Studio for Mac でソースリンクを有効にする](sourcelink-images/sourcelink-enable-mac-cropped.png)](sourcelink-images/sourcelink-enable-mac.png#lightbox)

**外部コードへのステップイン**が有効になっていることを確認します。

::: zone-end

## <a name="debug-xamarinforms-using-source-link"></a>Xamarin.Formsソースリンクを使用したデバッグ

外部パッケージのデバッグが有効になっている場合、Visual Studio は、NuGet パッケージに含まれているソースリンクマッピングを使用して、外部ソースコードをダウンロードしてステップ実行します。 これをテストするには、によって提供されるメソッドの呼び出しにブレークポイントを設定し Xamarin.Forms ます。

[![メソッドに設定されるブレークポイント Xamarin.Forms](sourcelink-images/breakpoint-cropped.png)](sourcelink-images/external-code-available.png#lightbox)

**デバッガー**オプションで指定した設定に応じて、Visual Studio によってソースファイルがダウンロードされていることが警告されます。

[![Visual Studio 外部コードの警告](sourcelink-images/external-code-cropped.png)](sourcelink-images/external-code-available.png#lightbox)

Visual Studio によるファイルのダウンロードを許可すると、デバッガーは外部コードにステップインします。

::: zone pivot="windows"

## <a name="source-link-caching"></a>ソースリンクのキャッシュ

ソースリンクは、パフォーマンスのためにキャッシュを使用します。 ソースリンクのキャッシュディレクトリは、**シンボル**セクションの [**デバッグ**] の [**オプション**] メニューで定義されています。

[![Visual Studio ソースリンクのキャッシュ](sourcelink-images/sourcelink-caching-pc-cropped.png)](sourcelink-images/sourcelink-caching-pc.png#lightbox)

このメニューでは、すべてのデバッグシンボルのキャッシュディレクトリを指定したり、キャッシュされたシンボルで問題が発生した場合にキャッシュをクリアしたりできます。

::: zone-end
::: zone pivot="macos"

## <a name="source-link-caching"></a>ソースリンクのキャッシュ

ソースリンクは、パフォーマンスのためにキャッシュを使用します。 MacOS のソースリンクのキャッシュディレクトリは `/Users/<username>/Library/Caches/VisualStudio/8.0/Symbols` です。 このフォルダーには、ソースファイルのダウンロードに使用されるリポジトリを格納するサブフォルダーが含まれています。 NuGet パッケージのバッキングリポジトリが変更されている場合は、これらのフォルダーを手動で削除してキャッシュを更新することが必要になる場合があります。

::: zone-end

## <a name="related-links"></a>関連リンク

- [ソースリンクのドキュメント](/dotnet/standard/library-guidance/sourcelink)
- [GitHub のソースリンク](https://github.com/dotnet/sourcelink)
