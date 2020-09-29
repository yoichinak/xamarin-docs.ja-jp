---
title: 古いバージョンの Xcode または Xamarin を使用できます。
description: このガイドでは、以前のバージョンの Xamarin または Xcode を使用する場合の問題の概要を示します (現在の安定したリリースより)。
ms.prod: xamarin
ms.assetid: 27CF7EB7-9251-435F-BEA5-F20F8DD7DC17
ms.technology: xamarin-ios
author: chamons
ms.author: chhamo
ms.date: 04/16/2019
ms.openlocfilehash: 6c6a0491f7989f2f76afabc3412e38be74a06da4
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91431580"
---
# <a name="can-i-use-an-older-version-of-xcode-or-xamarinios"></a>古いバージョンの Xcode または Xamarin を使用できますか。

Xamarin ドキュメントでは、最新の Xamarin. iOS と Xcode を使用することを前提としています。この方法をお勧めします。 ただし、一部のお客様は以前の Xamarin. iOS や Xcode を使用することを希望しており、その結果について詳しく説明します。

リリースノートには、次の警告が含まれています。

> [!WARNING]
> **古いバージョンの Xcode を使用する**
>
> 多くの場合、以前の Xcode バージョン (上記の [要件](/xamarin/ios/release-notes/12/12.8#requirements)に記載されていないバージョン) を使用できますが、一部の機能が使用できないことがあります。 また、いくつかの制限事項を回避する必要があります。次に例を示します。
>
> - 静的レジストラーでは、アプリケーションをビルドするために Xcode headers ファイルが必要です [`MT0091`](../mtouch-errors.md#MT0091) [`MT4109`](../mtouch-errors.md#MT4109) 。 api がない場合は、エラーまたはエラーが発生します。 ほとんどの場合、(API を削除することによって) マネージリンカーを有効にすることができます。
> - Xcode 9.0 + ツールチェーンが使用されていない場合、bitcode ビルド (tvOS と watchOS の場合) は、App Store への送信に失敗する可能性があります。

## <a name="further-information"></a>詳細情報

Microsoft では、アプリケーションを開発および送信するときに最新の Xcode と最新の Xamarin. iOS リリースを使用することを強くお勧めします。 Apple では、アプリケーションを送信するときに最新の Xcode を使用する必要があります。

最新の Xcode を使用しても、アプリケーションで古いバージョンの iOS を対象にすることはできないことに注意してください。 サポートされている iOS のバージョンは、お使いのアプリケーションで使用する **情報 plist** エントリと api に基づいています。

Xcode の複数のバージョンをサイドバイサイドでインストールすることができます。 **Xcode101** や **Xcode102**などの名前が異なります。 複数のバージョンを使用する場合は、 [Visual Studio for Mac 設定](~/ios/troubleshooting/questions/ios-sdk.md) で active Xcode を、 [`xcode-select`](https://developer.apple.com/library/archive/technotes/tn2339/_index.html#//apple_ref/doc/uid/DTS40014588-CH1-HOW_DO_I_SELECT_THE_DEFAULT_VERSION_OF_XCODE_TO_USE_FOR_MY_COMMAND_LINE_TOOLS_) [コマンドラインツール](https://developer.apple.com/library/archive/technotes/tn2339/_index.html#//apple_ref/doc/uid/DTS40014588-CH1-HOW_DO_I_SELECT_THE_DEFAULT_VERSION_OF_XCODE_TO_USE_FOR_MY_COMMAND_LINE_TOOLS_)で必ず設定してください。

ただし、まれな状況では、古いコンポーネントの使用が必要になる場合があります。 このドキュメントでは、最新バージョンより古いバージョンを使用する場合に直面する可能性がある一般的な課題について説明します。

Apple からの各リリースは一意ですが、ここに記載されていない他の落とし穴が発生する可能性があります。

これらの課題は、問題の解決にはあまり重要ではない場合があるため、可能な限り最新の Xcode と最新の Xamarin. iOS の構成がサポートされています。

## <a name="use-of-an-old-xamarinios-with-an-old-xcode"></a>古い Xamarin. iOS と古い Xcode の使用

Xamarin を更新していません。少なくともしばらくの間、iOS と Xcode が可能です。 上限として、Apple はアプリケーションを送信するために Xcode の最小バージョンを必要とします。 この時点で、すべてのコンポーネント (macOS、Xcode、および Xamarin) を最新バージョンに更新する必要があります (または、Apple によって必要とされる Xcode の新しい最小バージョンと、対応する Xamarin. iOS リリース)。

通常、少しずつ更新して、小さな変更に対応できます。 更新の処理が困難になる可能性がある大規模なプロジェクトの場合、既知の作業セットを保持することが、適切な妥協になる可能性があります。

## <a name="use-of-new-xamarinios-with-older-xcode"></a>新しい Xamarin. iOS と古い Xcode の使用

Xamarin. iOS は一般に、可能な限り古い Xcode のリリースをサポートしています。 次のようないくつかの問題が考えられます。

- 新しい Xamarin. iOS では、選択した Xcode に存在しない一部の機能と Api がサポートされている場合があります。 
- **静的レジストラー**では、アプリケーションをビルドするために Xcode headers ファイルが必要です [`MT0091`](~/ios/troubleshooting/mtouch-errors.md#MT0091) [`MT4109`](~/ios/troubleshooting/mtouch-errors.md#MT4109) 。 api がない場合は、エラーまたはエラーが発生します。
  - ほとんどの場合、マネージリンカーを有効にすると (新しい API のマネージバインドを削除することによって)、使用されなくなった場合に役立ちます。
- Xcode 9.0 + ツールチェーンが使用されていない場合、bitcode ビルド (tvOS と watchOS の場合) は、App Store への送信に失敗する可能性があります。

## <a name="use-of-new-xcode-with-older-xamarinios"></a>古い Xamarin での new Xcode の使用

このユースケースは、Xamarin と比べて大幅に困難です。新しい Xcode の変更要件を予測することはできません。 MacOS の更新により、問題が発生する可能性があります。また、互換性修正プログラムが Xamarin の多くの部分に含まれていない場合もあります。 

次のような問題が発生する可能性のある領域がいくつかあります。

- 非互換性 `mlaunch` :
  - シミュレーターのサポートが正しく機能しない可能性があります (またはすべて)。
  - デバイスのサポートが正しく機能しない (またはまったく)
- 不明なサポート `mtouch` 
  - 新しいフレームワークはサポートしていません
  - 新しい権利はサポートしていません
  - 新規または更新されたツールはサポートされていません
    - これはコード署名にも影響を与える可能性があります

### <a name="new-appstore-submission-rules"></a>新しい AppStore の提出ルール

Apple は、いつでも AppStore の提出ルールを更新する権利を留保します。 これらの規則の変更は、事前に発表されている場合があります。 これらの変更の一部では、のサポートに対するツールの変更が必要になります。そのためには、更新された Xamarin. iOS コンポーネントが必要です。

Apple では、ルールの変更に加えて、送信されたアプリまたは既存の強化に対して、追加の検証が追加されることがよくあります。 これらの一部は、ツールの変更を必要とします (たとえば、新しいブラックリストシンボル)。 これらの多くは、ルールのアナウンス (または一覧) がないため、お客様がを送信するときに最初に検出されます。

## <a name="summary"></a>まとめ

可能な限り、Apple のガイダンスに従って、アプリストアでリリースされた最新の Xcode を使用して開発し、提出してください。

さらに、最新の Xamarin. iOS がリリースされたを使用します。 これは、送信されたアプリケーションに影響する可能性がある最新の修正プログラムを追跡し、最新のルール変更に準拠します。

それが不可能な場合は、一致した古い Xcode と Xamarin の iOS リリースを使用することを検討してください。 これは、しばらくの間機能しますが、ある時点では、Apple は新しいツールに同意するため、それに応じて計画を立てることができます。