---
title: 参照のネイティブの iOS、Mac、およびバインド プロジェクト
description: ネイティブ参照では、Xamarin.iOS、Xamarin.Mac、またはバインド プロジェクトにネイティブ フレームワークを埋め込むために機能を提供します。
ms.prod: xamarin
ms.assetid: E53185FB-CEF5-4AB5-94F9-CC9B57C52300
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 3a497d0bb4674014b8063cb1fbc91eec6e7ae5ea
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61201562"
---
# <a name="native-references-in-ios-mac-and-bindings-projects"></a>IOS、Mac、およびバインド プロジェクトでネイティブ参照

_ネイティブ参照では、ネイティブ フレームワークに埋め込むか、Xamarin.iOS、Xamarin.Mac プロジェクトまたはバインド プロジェクトに機能を提供します。_

IOS 8.0 以降は、アプリ拡張機能と Xcode でメイン アプリケーションの間のコードを共有する埋め込みのフレームワークを作成することがされました。 ネイティブ参照機能を使用することは Xamarin.iOS でこれらの組み込みフレームワーク (Xcode で作成された) を使用します。
 
> [!IMPORTANT]
> Xamarin.iOS または Xamarin.Mac プロジェクトの任意の型から組み込みフレームワークを作成することはできません、ネイティブ参照のみ許可の既存のネイティブ (OBJECTIVE-C) フレームワークの利用します。

<a name="Terminology" />

## <a name="terminology"></a>用語

IOS 8 (およびそれ以降) で**埋め込みフレームワーク**どちらも静的にリンクされたおよび動的にリンクされたフレームワークが埋め込まれたことができます。 正しくこれらを配布する必要がありますようにするすべての含まれる"fat"フレームワークにその_スライス_アプリでサポートするデバイス アーキテクチャごとにします。

<a name="Static-vs-Dynamic-Frameworks" />

### <a name="static-vs-dynamic-frameworks"></a>静的ポートとします。動的なフレームワーク

**静的なフレームワーク**コンパイル時にリンクされている**動的フレームワーク**ランタイムにリンクされているし、されるため、再リンクすることがなく変更できます。 使用していた iOS 8 の前に、サード パーティのフレームワークを使用している場合、**静的 Framework**アプリにコンパイルします。 Apple を参照してください。[動的ライブラリ プログラミング](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/DynamicLibraries/100-Articles/OverviewOfDynamicLibraries.html#//apple_ref/doc/uid/TP40001873-SW1)詳細についてはドキュメントです。

<a name="Embedded-vs-System-Frameworks" />

### <a name="embedded-vs-system-frameworks"></a>埋め込まれたとします。システムのフレームワーク

**埋め込みフレームワーク**は、アプリ バンドルに含まれはのみ、サンド ボックスを使用して、特定のアプリにアクセスできます。 **システム フレームワーク**はオペレーティング システム レベルで格納され、デバイス上のすべてのアプリを利用できます。 現時点では、Apple だけでは、オペレーティング システム レベルのフレームワークを作成する機能があります。

<a name="Thin-vs-Fat-Frameworks" />

### <a name="thin-vs-fat-frameworks"></a>シンとします。Fat フレームワーク

**フレームワークのシン**特定のシステムのアーキテクチャ用コンパイル済みコードのみを含めることが、 **Fat フレームワーク**複数のアーキテクチャ用のコードが含まれます。 フレームワークにコンパイルされた各アーキテクチャに固有のコードベースと呼びます、_スライス_します。 そのため、たとえば、フレームワークを 2 つの iOS シミュレーターのアーキテクチャ (i386 および X86_64) 用にコンパイルされましたは含めることが 2 つのスライス。

アプリでこのサンプル フレームワークを配布しようとした場合は、シミュレーターでは正しく動作しますが、フレームワークには、iOS デバイスの場合は、任意コード固有スライスが含まれていないために、デバイスで失敗します。 フレームワークは、すべてのインスタンスで動作することを確認するにもすることは arm64、armv7 と armv7s などのデバイスに固有のスライスが含まれます。

<a name="Working-with-Embedded-Frameworks" />

## <a name="working-with-embedded-frameworks"></a>埋め込みフレームワークの使用

Xamarin.iOS または Xamarin.Mac アプリで埋め込みフレームワークを使用する完了する必要がある 2 つの手順があります。Fat フレームワークを作成し、フレームワークを埋め込みます。

<a name="Overview" />

### <a name="creating-a-fat-framework"></a>Fat フレームワークを作成します。

前述のように、アプリでは、埋め込みのフレームワークを使用できるする必要を含むすべてのアプリを実行するデバイスのシステム アーキテクチャのスライスの Fat フレームワークです。

フレームワークとアプリを使うには、同じの Xcode プロジェクトでは、ときにこの問題には Xcode は、フレームワークと同じビルド設定を使用してアプリの両方を構築されます。 Xamarin アプリでは、埋め込みフレームワークを作成できないために、この手法は使用できません。

この問題を解決するために、`lipo`コマンド ライン ツールは、すべての必要なスライスを含む 1 つの Fat フレームワークに 2 つまたは複数のフレームワークをマージするために使用できます。 操作の詳細については、`lipo`コマンドを参照してください、[ネイティブ ライブラリのリンク](~/ios/platform/native-interop.md)ドキュメント。

<a name="Embedding-a-Framework" />

### <a name="embedding-a-framework"></a>埋め込みフレームワーク

ネイティブ参照を使用して Xamarin.iOS または Xamarin.Mac プロジェクトに、フレームワークを埋め込むには、次の手順が必要です。

1. 新規作成するか、既存の Xamarin.iOS、Xamarin.Mac、またはバインド プロジェクトを開きます。
2. **ソリューション エクスプ ローラー**プロジェクト名を右クリックし、選択、**追加** > **ネイティブ参照の追加**: 

    [![](native-references-images/ref01.png "ソリューション エクスプ ローラーでプロジェクト名を右クリックし、ネイティブ参照の追加を選択します。")](native-references-images/ref01.png#lightbox)
3. **オープン** ダイアログ ボックスで、埋め込み をクリックするネイティブ フレームワークの名前を選択、**オープン**ボタン。 

    [![](native-references-images/ref02.png "埋め込みを開く をクリックするネイティブ フレームワークの名前を選択します。")](native-references-images/ref02.png#lightbox)
4. フレームワークは、プロジェクトのツリーに追加されます。 

    [![](native-references-images/ref03.png "フレームワークは、プロジェクト ツリーに追加されます。")](native-references-images/ref03.png#lightbox)

プロジェクトがコンパイルされると、ネイティブ フレームワークは、アプリのバンドルに埋め込まれます。

<a name="App-Extensions-and-Embedded-Frameworks" />

## <a name="app-extensions-and-embedded-frameworks"></a>アプリの拡張機能と埋め込みフレームワーク

内部的には Xamarin.iOS がフレームワークとして Mono ランタイムとリンクするには、この機能をかかる場合があります (ときに、展開対象が > = iOS 8.0)、(Mono ランタイムはの 1 回だけ含まれるために、大幅に拡張機能を使用したアプリのアプリのサイズを減らすアプリ全体、バンドル、コンテナー アプリと各拡張機能の 1 回に 1 回ではなく)。

拡張機能は、すべての拡張機能には、iOS 8.0 が必要とするため、フレームワークとして、Mono ランタイムとリンクされます。

ない拡張機能とアプリを対象の iOS アプリ 

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、ネイティブ フレームワークに埋め込むか、Xamarin.iOS、Xamarin.Mac アプリケーションについて詳しく説明をしました。

