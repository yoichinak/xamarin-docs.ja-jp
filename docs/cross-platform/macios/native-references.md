---
title: ネイティブは、iOS、Mac、およびバインドプロジェクトを参照します
description: ネイティブ参照を使用すると、ネイティブフレームワークを Xamarin. iOS、Xamarin、またはバインドプロジェクトに埋め込むことができます。
ms.prod: xamarin
ms.assetid: E53185FB-CEF5-4AB5-94F9-CC9B57C52300
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: e2f874446b48726afc2218e5cdcac9b8736e1681
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86930443"
---
# <a name="native-references-in-ios-mac-and-bindings-projects"></a>IOS、Mac、およびバインドプロジェクトでのネイティブ参照

_ネイティブ参照を使用すると、ネイティブフレームワークを Xamarin または Xamarin のプロジェクトまたはバインドプロジェクトに埋め込むことができます。_

IOS 8.0 以降、アプリ拡張機能と Xcode のメインアプリ間でコードを共有するための埋め込みフレームワークを作成できるようになりました。 ネイティブ参照機能を使用すると、Xcode で作成されたこれらの埋め込みフレームワークを使用することができます。

> [!IMPORTANT]
> 任意の種類の Xamarin. iOS プロジェクトまたは Xamarin. Mac プロジェクトから埋め込みフレームワークを作成することはできません。ネイティブ参照では、既存のネイティブ (目標 C) フレームワークの使用のみが許可されます。

<a name="Terminology"></a>

## <a name="terminology"></a>用語

IOS 8 以降では、**埋め込みフレームワーク**は、静的にリンクされた静的フレームワークと動的にリンクされたフレームワークの両方にすることができます。 これらを適切に配布するには、アプリでサポートする各デバイスアーキテクチャのすべての_スライス_を含む "Fat" フレームワークにする必要があります。

<a name="Static-vs-Dynamic-Frameworks"></a>

### <a name="static-vs-dynamic-frameworks"></a>静的フレームワークと動的フレームワーク

**静的フレームワーク**はコンパイル時にリンクされます。**動的フレームワーク**は実行時にリンクされ、れるためは再リンクせずに変更できます。 IOS 8 より前のサードパーティ製のフレームワークを使用していた場合は、アプリにコンパイルされた**静的フレームワーク**を使用していました。 詳細については、Apple の[ダイナミックライブラリプログラミング](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/DynamicLibraries/100-Articles/OverviewOfDynamicLibraries.html#//apple_ref/doc/uid/TP40001873-SW1)に関するドキュメントを参照してください。

<a name="Embedded-vs-System-Frameworks"></a>

### <a name="embedded-vs-system-frameworks"></a>埋め込みとシステムフレームワーク

**埋め込みフレームワーク**はアプリバンドルに含まれており、そのサンドボックスを介して特定のアプリにのみアクセスできます。 **システムフレームワーク**はオペレーティングシステムレベルで格納され、デバイス上のすべてのアプリで使用できます。 現時点では、オペレーティングシステムレベルのフレームワークを作成できるのは Apple だけです。

<a name="Thin-vs-Fat-Frameworks"></a>

### <a name="thin-vs-fat-frameworks"></a>シンおよび Fat のフレームワーク

**シンフレームワーク**には、特定のシステムアーキテクチャ用にコンパイルされたコードのみが含まれており、 **Fat フレームワーク**には複数のアーキテクチャ用のコードが含まれています。 フレームワークにコンパイルされたアーキテクチャ固有のコードベースは、それぞれ_スライス_と呼ばれます。 たとえば、2つの iOS シミュレーターアーキテクチャ (i386 と X86_64) 用にコンパイルされたフレームワークがある場合、2つのスライスが含まれています。

このサンプルフレームワークをアプリケーションと共に配布しようとすると、シミュレーターでは正常に実行されますが、iOS デバイスのコード固有のスライスがフレームワークに含まれていないため、デバイスでは失敗します。 フレームワークがすべてのインスタンスで動作するようにするには、arm64、armv7、armv7s などのデバイス固有のスライスも含める必要があります。

<a name="Working-with-Embedded-Frameworks"></a>

## <a name="working-with-embedded-frameworks"></a>埋め込みフレームワークの操作

Xamarin または Xamarin. Mac アプリで埋め込みフレームワークを操作するには、次の2つの手順を実行する必要があります。 Fat フレームワークの作成とフレームワークの埋め込み。

<a name="Overview"></a>

### <a name="creating-a-fat-framework"></a>Fat フレームワークの作成

前述のように、アプリで埋め込まれたフレームワークを使用できるようにするには、アプリが実行されるデバイスのすべてのシステムアーキテクチャスライスを含む Fat フレームワークである必要があります。

フレームワークとコンシューマーアプリが同じ Xcode プロジェクト内にある場合、Xcode は同じビルド設定を使用してフレームワークとアプリの両方をビルドするので、これは問題にはなりません。 Xamarin アプリは埋め込みフレームワークを作成できないため、この手法は使用できません。

この問題を解決するには、 `lipo` コマンドラインツールを使用して、2つ以上のフレームワークを、必要なスライスをすべて含む1つの Fat フレームワークにマージします。 コマンドの使用方法の詳細については `lipo` 、[ネイティブライブラリのリンク](~/ios/platform/native-interop.md)に関するドキュメントを参照してください。

<a name="Embedding-a-Framework"></a>

### <a name="embedding-a-framework"></a>フレームワークの埋め込み

ネイティブ参照を使用して Xamarin または Xamarin. Mac プロジェクトにフレームワークを埋め込むには、次の手順を実行する必要があります。

1. 新しいを作成するか、既存の Xamarin、iOS、Xamarin、またはバインドプロジェクトを開きます。
2. **ソリューションエクスプローラー**で、プロジェクト名を右クリックし、[**追加**] [  >  **ネイティブ参照**の追加] の順に選択します。 

    [![ソリューションエクスプローラーで、プロジェクト名を右クリックし、[ネイティブ参照の追加] を選択します。](native-references-images/ref01.png)](native-references-images/ref01.png#lightbox)
3. [**開く**] ダイアログボックスで、埋め込むネイティブフレームワークの名前を選択し、[**開く**] ボタンをクリックします。 

    [![埋め込むネイティブフレームワークの名前を選択し、[開く] ボタンをクリックします。](native-references-images/ref02.png)](native-references-images/ref02.png#lightbox)
4. フレームワークがプロジェクトのツリーに追加されます。 

    [![フレームワークがプロジェクトツリーに追加されます](native-references-images/ref03.png)](native-references-images/ref03.png#lightbox)

プロジェクトをコンパイルすると、ネイティブフレームワークはアプリのバンドルに埋め込まれます。

<a name="App-Extensions-and-Embedded-Frameworks"></a>

## <a name="app-extensions-and-embedded-frameworks"></a>アプリ拡張機能と埋め込みフレームワーク

内部 Xamarin。 iOS では、この機能を利用して Mono ランタイムをフレームワークとしてリンクすることができます (デプロイターゲットが >= iOS 8.0 の場合)。そのため、拡張機能を使用するアプリでは、アプリのサイズを大幅に削減できます (Mono ランタイムは、コンテナーアプリにつき1回ではなく、拡張機能ごとに1回

拡張機能は、すべての拡張機能が iOS 8.0 を必要とするため、フレームワークとして Mono ランタイムとリンクされます。

IOS を対象とする拡張機能とアプリがないアプリ 

<a name="Summary"></a>

## <a name="summary"></a>まとめ

この記事では、ネイティブフレームワークを Xamarin iOS または Xamarin. Mac アプリケーションに埋め込む方法について詳しく説明しました。
