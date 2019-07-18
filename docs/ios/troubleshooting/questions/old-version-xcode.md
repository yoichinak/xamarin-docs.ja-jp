---
title: 以前のバージョンの Xcode または Xamarin.iOS を使用できます。
description: このガイドでは、(現在の安定版リリース) より Xamarin.iOS または Xcode の以前のバージョンの使用に関する問題について説明します。
ms.prod: xamarin
ms.assetid: 27CF7EB7-9251-435F-BEA5-F20F8DD7DC17
ms.technology: xamarin-ios
author: chamons
ms.author: chhamo
ms.date: 04/16/2019
ms.openlocfilehash: 7cbc14e0a912fe9c55ff672796e839a8dcdfd9b5
ms.sourcegitcommit: 864f47c4f79fa588b65ff7f721367311ff2e8f8e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/24/2019
ms.locfileid: "64347114"
---
# <a name="can-i-use-an-older-version-of-xcode-or-xamarinios"></a>以前のバージョンの Xcode または Xamarin.iOS を使用できますか。

Xamarin ドキュメントには、最新の Xamarin.iOS と、推奨されている Xcode の使用が想定しています。 ただし、一部のお客様は、以前の Xamarin.iOS や Xcode を使用して、結果の詳細します。

リリース ノートには、次の警告が含まれます。

> [!WARNING]
> **古い Xcode バージョンを使用します。**
>
> 古い Xcode バージョンを使用して (上記で説明されているものよりも[要件](https://docs.microsoft.com/xamarin/ios/release-notes/12/12.8#requirements)) が可能であれば、多くの場合がいくつかの機能を利用できない可能性があります。 いくつかの制限が必要です、回避策など。
>
> - 静的なレジストラーにつながる、アプリケーションをビルドする Xcode ヘッダー ファイルを必要があります[ `MT0091` ](https://docs.microsoft.com/xamarin/ios/troubleshooting/mtouch-errors#MT0091)または[ `MT4109` ](https://docs.microsoft.com/xamarin/ios/troubleshooting/mtouch-errors#MT4109) Api が存在しない場合のエラー。 ほとんどの場合、管理対象のリンカーを有効にするとは (を API を削除する) のに役立ちます。
> - Bitcode ビルド (tvOS と watchOS) は、Xcode 9.0 以降のツール チェーンを使用しない場合、アプリ ストアに送信するをフェールバックできます。

## <a name="further-information"></a>詳細情報

Microsoft では、最新の Xcode との開発とアプリケーションを送信するときに、この最新の Xamarin.iOS リリースを使用して強くお勧めします。 Apple では、アプリケーションを送信するときに、最新の Xcode を使用する必要があります。

最新の Xcode を使用しても、アプリケーションから、以前の iOS バージョンを対象とするはできないは注意してください。 サポートする iOS のバージョンがに基づいて、 **Info.plist**エントリと、Api アプリケーションで使用します。

など、異なる名前を持つ複数のバージョンの Xcode サイド バイ サイドをインストールすることは**Xcode101.app**と**Xcode102.app**します。 複数のバージョンを使用する場合でアクティブな Xcode を設定することを確認してください[Visual Studio for Mac 設定](~/ios/troubleshooting/questions/ios-sdk.md)を使用して、 [ `xcode-select` ](https://developer.apple.com/library/archive/technotes/tn2339/_index.html#//apple_ref/doc/uid/DTS40014588-CH1-HOW_DO_I_SELECT_THE_DEFAULT_VERSION_OF_XCODE_TO_USE_FOR_MY_COMMAND_LINE_TOOLS_) [コマンド ライン ツール](https://developer.apple.com/library/archive/technotes/tn2339/_index.html#//apple_ref/doc/uid/DTS40014588-CH1-HOW_DO_I_SELECT_THE_DEFAULT_VERSION_OF_XCODE_TO_USE_FOR_MY_COMMAND_LINE_TOOLS_)します。

ただし、まれな状況では、古いコンポーネントの使用を必要があります。 このドキュメントはときに直面する可能性のある一般的な課題について説明して最新のよりも古いバージョンを使用します。

Apple からの各リリースは、一意とは、ここに記載されていないその他の落とし穴が遭遇する可能性があります。

これらの課題は、最新の Xcode と最新の Xamarin.iOS のサポートされる構成に引き続き使用可能なときにこれを解決する重要な場合があります。

## <a name="use-of-an-old-xamarinios-with-an-old-xcode"></a>古い Xcode と古い Xamarin.iOS の使用

Xamarin.iOS と Xcode のない更新が可能であれば、以上の一定の時間です。 制限は、ある時点で、Apple を必要とするアプリケーションを送信する Xcode の最小バージョンです。 この時点で最新バージョン (または、新しい最小バージョンの Xcode に Apple と一致する Xamarin.iOS リリースに必要な) に、すべてのコンポーネント (macOS で Xcode と Xamarin.iOS) を更新する必要があります。

通常を段階的に更新し、小規模な変更の方が簡単になります。 大規模なプロジェクトを更新できますが困難になるのでは、既知のワーキング セットには、妥当な結果に指定できます。

## <a name="use-of-new-xamarinios-with-older-xcode"></a>新しい Xamarin.iOS 古い Xcode との使用

一般に、Xamarin.iOS には、ある程度可能であれば、古い Xcode リリースがサポートされています。 いくつかの潜在的な課題は次のとおりです。

- 新しい Xamarin.iOS は、一部の機能をサポート可能性があり、選択した Xcode での Api が存在しません。 
- **静的レジストラー**に先行するアプリケーションをビルドする Xcode ヘッダー ファイルを必要と[ `MT0091` ](~/ios/troubleshooting/mtouch-errors.md#MT0091)または[ `MT4109` ](~/ios/troubleshooting/mtouch-errors.md#MT4109) Api が存在しない場合のエラー。
  - ほとんどの場合 (新しい API の管理対象のバインドを削除する) を管理対象のリンカーを有効にするはにより使用されていない場合。
- Bitcode ビルド (tvOS と watchOS) は、Xcode 9.0 以降のツール チェーンを使用しない場合、アプリ ストアに送信するをフェールバックできます。

## <a name="use-of-new-xcode-with-older-xamarinios"></a>古い Xamarin.iOS で新しい Xcode の使用

このユース ケースは大幅に難しく、Xamarin.iOS は新しい Xcode の要件の変化を予測できるいないようです。 MacOS の更新プログラムでは、問題も導入し、互換性修正プログラムなし Xamarin.iOS の多くの部分に影響する可能性があります。 

さまざまな場所は問題が生じたなど潜在的な領域があります。

- 非互換性`mlaunch`:
  - シミュレーターのサポートが正しく (またはまったく) は機能しません。
  - デバイスのサポートが正しく (またはまったく) は機能しません。
- 不明なサポート `mtouch` 
  - 新しいフレームワークはサポートされていません
  - 新しい権利はサポートされていません
  - 新しいまたは更新されたツールはサポートされていません
    - これは、コードも署名に影響を与えることができます。

### <a name="new-appstore-submission-rules"></a>新しい AppStore 送信ルール

Apple では、いつでも、AppStore 送信規則を更新する権利を有します。 これらの規則の変更が事前に発表されたのみ場合があります。 これらの変更の一部は、Xamarin.iOS の更新されたコンポーネントを必要となります変更をサポートするためのツールが必要です。

規則の変更だけでなく Apple は多くの場合、送信されたアプリに追加の検証を追加します。 または、既存の高めることができます。 これらの一部には、当社のツール (例: 新しいブラック リストにあるシンボルを) の変更が必要です。 これらの多く最初に検出された顧客を送信して、規則のないお知らせ (または一覧) があるとします。

## <a name="summary"></a>まとめ

可能であれば、安全では、次の Apple のガイダンスとでの開発と、App Store にリリースされた最新の Xcode と送信に再生します。

さらに、リリースされた最新の Xamarin.iOS を使用します。 これにより、送信されたアプリケーションに影響する規則の最新の変更に従っている可能性のある最新の修正プログラムを追跡します。

可能でない、一致した古い Xcode と Xamarin.iOS リリースを使用してを検討します。 期間、この作業できますが、ある時点で Apple では新しいツールによって要求適切に計画します。
