---
title: "ネイティブ参照"
description: "ネイティブ参照では、ネイティブ フレームワークに埋め込む Xamarin.iOS または Xamarin.Mac プロジェクトまたはプロジェクトのバインド機能を提供します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: E53185FB-CEF5-4AB5-94F9-CC9B57C52300
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 29b405c61b745c26c74318243f75e5809ecfdd7d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="native-references"></a>ネイティブ参照

_ネイティブ参照では、ネイティブ フレームワークに埋め込む Xamarin.iOS または Xamarin.Mac プロジェクトまたはプロジェクトのバインド機能を提供します。_


IOS 8.0 以降が経過しているアプリの拡張機能と Xcode でメインのアプリ間でコードを共有するために埋め込みのフレームワークを作成することです。 ネイティブ参照機能を使用することができます Xamarin.iOS でこれらの組み込みフレームワーク (Xcode で作成) を使用します。
 
> [!IMPORTANT]
> **注:** Xamarin.iOS または Xamarin.Mac のプロジェクトの任意の型から組み込みフレームワークを作成することはできません、ネイティブ参照のみを許可する既存のネイティブ (OBJECTIVE-C) フレームワークの消費をします。




<a name="Terminology" />

## <a name="terminology"></a>用語

IOS 8 (およびそれ以降) で**埋め込みフレームワーク**静的にリンクされると、動的にリンクされたフレームワークを埋め込み両方を指定できます。 適切に分散して、する必要がありますに含まれるすべての"fat"フレームワークにその_スライス_アプリをサポートするデバイス アーキテクチャごとにします。

<a name="Static-vs-Dynamic-Frameworks" />

### <a name="static-vs-dynamic-frameworks"></a>静的ポートとします。動的なフレームワーク

**静的なフレームワーク**コンパイル時にリンクされている**動的フレームワーク**は実行時にリンクされてを下げて再リンクさせずに変更できます。 使用していた iOS 8 の前に、サード パーティ製のフレームワークを使用している場合、**静的 Framework**アプリにコンパイルされたことです。 Apple を参照してください[ダイナミック ライブラリ プログラミング](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/DynamicLibraries/100-Articles/OverviewOfDynamicLibraries.html#//apple_ref/doc/uid/TP40001873-SW1)詳細についてはドキュメントです。

<a name="Embedded-vs-System-Frameworks" />

### <a name="embedded-vs-system-frameworks"></a>埋め込まれたとします。システムのフレームワーク

**フレームワークの埋め込み**アプリ バンドルに含まれ、は、サンド ボックスを使用して特定のアプリにアクセスできるのみです。 **システム フレームワーク**オペレーティング システム レベルで保存され、デバイス上のすべてのアプリで利用できます。 現時点では、Apple だけでは、オペレーティング システム レベルのフレームワークを作成する権限を持ちます。

<a name="Thin-vs-Fat-Frameworks" />

### <a name="thin-vs-fat-frameworks"></a>シン vs です。Fat フレームワーク

**フレームワークのシン**特定のシステムのアーキテクチャ用コンパイル済みコードのみを含む場所**Fat フレームワーク**複数のアーキテクチャ用のコードが含まれます。 各アーキテクチャ固有コードベースをフレームワークにコンパイルと呼びます、_スライス_です。 そのため、たとえば、2 つの iOS シミュレーター アーキテクチャ (i386 と X86_64) 用にコンパイルされたフレームワークがあればが含まれて 2 つのスライスします。

このサンプル Framework アプリを配布しようとした場合に、シミュレーターに正常に実行けれども、デバイスで失敗する場合、フレームワークには、iOS デバイスの特定のコード スライスが含まれていないために、 フレームワークは、すべてのインスタンスで動作することを確認するにもすることは arm64、常に armv7 armv7s などのデバイスに固有のスライスが含まれます。

<a name="Working-with-Embedded-Frameworks" />

## <a name="working-with-embedded-frameworks"></a>埋め込みのフレームワークでの作業

2 つの手順 Xamarin.iOS または Xamarin.Mac アプリで埋め込みフレームワークで作業を完了する必要がある: Fat フレームワークを作成して、フレームワークの埋め込み。

<a name="Overview" />

### <a name="creating-a-fat-framework"></a>Fat フレームワークを作成します。

前述のように、アプリでは、埋め込みのフレームワークを使用できるようにする必要を含むすべてのアプリを実行するデバイスのシステム アーキテクチャのスライスの Fat フレームワークです。

フレームワークと使用のアプリは、同じの Xcode プロジェクトでは、これはない問題 Xcode は、フレームワークと同じビルド設定を使用して、アプリの両方をビルドは、です。 Xamarin アプリでは、埋め込みフレームワークを作成できないために、この手法を使用できません。

この問題を解決するために、`lipo`コマンド ライン ツールは、すべての必要なスライスを含む 1 つの Fat フレームワークに 2 つまたは複数のフレームワークをマージするために使用できます。 操作の詳細については、`lipo`コマンドを参照してください、[ネイティブ ライブラリのリンク](~/ios/platform/native-interop.md)ドキュメント。

<a name="Embedding-a-Framework" />

### <a name="embedding-a-framework"></a>フレームワークの埋め込み

ネイティブ参照を使用して、Xamarin.iOS または Xamarin.Mac プロジェクトに、フレームワークを埋め込むには、次の手順が必要です。

1. 新規作成または既存 Xamarin.iOS、Xamarin.Mac またはバインド プロジェクトを開きます。
2. **ソリューション エクスプ ローラー**、プロジェクト名を右クリックし **追加** > **ネイティブ参照の追加**: 

    [ ![](native-references-images/ref01.png "ソリューション エクスプ ローラーでプロジェクト名を右クリックし、ネイティブ参照の追加")](native-references-images/ref01.png)
3. **開く** ダイアログ ボックスで、埋め込み をクリックするネイティブ フレームワークの名前を選択、**開く**ボタン。 

    [ ![](native-references-images/ref02.png "埋め込むし、[開く] ボタンをクリックするためにネイティブ フレームワークの名前を選択します。")](native-references-images/ref02.png)
4. フレームワークは、プロジェクトのツリーに追加されます。 

    [ ![](native-references-images/ref03.png "フレームワークは、プロジェクト ツリーに追加されます。")](native-references-images/ref03.png)

プロジェクトがコンパイルされると、ネイティブのフレームワークは、アプリのバンドルに埋め込まれます。

<a name="App-Extensions-and-Embedded-Frameworks" />

## <a name="app-extensions-and-embedded-frameworks"></a>アプリの拡張機能と埋め込みフレームワーク

Xamarin.iOS がフレームワークとしてモノラル ランタイムとリンクするには、この機能に内部的にかかる場合があります (配置ターゲットの場合は > = iOS 8.0)、(以降の Mono ランタイムが 1 回だけ含まれますが大幅に拡張機能を使ったアプリのアプリのサイズを減らすアプリ全体をバンドル、コンテナーのアプリと拡張機能ごとに 1 回に 1 回ではなく)。

拡張機能は、すべての拡張機能には、iOS 8.0 が必要とするため、フレームワークでは、Mono ランタイムとリンクされます。

ない拡張機能とアプリを対象の iOS アプリ 

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、Xamarin.iOS や Xamarin.Mac アプリケーションにネイティブ フレームワークの埋め込みについての詳細を取得しました。

