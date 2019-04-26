---
title: Xamarin.iOS の分析規則
description: このドキュメントより/better optimized 設定があるかを判断するための Xamarin.iOS プロジェクトの設定をチェックする分析規則のセットについて説明します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C29B69F5-08E4-4DCC-831E-7FD692AB0886
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/06/2018
ms.openlocfilehash: 8a4990ce7b2bcacbd4b97b214458531b3d94122e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61416384"
---
# <a name="xamarinios-analysis-rules"></a>Xamarin.iOS の分析規則

Xamarin.iOS の分析より優れた/より最適化された設定が使用可能な場合を判断するのに役立つプロジェクトの設定をチェックする規則のセットです。

考えられる改善点を早い段階で見つけて開発時間を節約する限り頻繁に分析規則を実行します。

Visual Studio for Mac のメニューで、ルールを実行する選択**プロジェクト > コード分析を実行**します。

> [!NOTE]
> Xamarin.iOS の分析は、現在選択されている構成でのみ実行されます。 デバッグ用ツールの実行を強くお勧め**と**リリース構成します。

<a name="XIA0001" />

## <a name="xia0001-disabledlinkerrule"></a>XIA0001:DisabledLinkerRule

- **問題:** デバイスをデバッグ モードでは、リンカーが無効です。
- **修正:** 事態を回避するために、リンカー、コードを実行しようとする必要があります。
それを設定するには、プロジェクトに移動 > iOS ビルド > リンカーの動作。

<a name="XIA0002" />

## <a name="xia0002-testcloudagentreleaserule"></a>XIA0002:TestCloudAgentReleaseRule

- **問題:** Test Cloud agent を初期化するアプリのビルドはプライベート API を使用した、送信されたときに Apple によって拒否されます。
- **修正:** 追加、#if のために必要な修正やコードで定義します。

<a name="XIA0003" />

## <a name="xia0003-ipadebugbuildsrule"></a>XIA0003:IPADebugBuildsRule

- **問題:** 開発者署名キーを使用するデバッグ構成はこれで、公開ウィザードを使用して、配布、必要に応じてのみ、IPA を生成しません。
- **修正:** デバッグ構成のプロジェクト オプションで IPA のビルドを無効にします。

<a name="XIA0004" />

## <a name="xia0004-missing64bitsupportrule"></a>XIA0004:Missing64BitSupportRule

- **問題:** サポートされているアーキテクチャを"リリース |デバイス"では、64 ビット ARM64 不足している、互換性はありません。 これは、Apple AppStore で、32 ビットのみの iOS アプリを受け入れない問題です。
- **修正:** IOS プロジェクトをダブルクリック、ビルドに移動 > iOS をビルドおよび ARM64 があるために、サポートされているアーキテクチャを変更します。

<a name="XIA0005" />

## <a name="xia0005-float32rule"></a>XIA0005:Float32Rule

- **問題:** Float32 型のオプションを使用していない (--aot オプション = O = float32) コスト、モバイル、特にでおそらく多くのパフォーマンスに潜在顧客、倍精度演算が多少遅くなります。 .NET では倍精度内部的には、float の場合でもため、このオプションを有効にすると、精度と、場合によっては、互換性に影響しますに注意してください。
- **修正:** IOS プロジェクトをダブルクリック、ビルドに移動 > iOS をビルドし、オフにします、「64 ビット浮動小数点数と 32 ビット浮動小数点のすべての操作を実行する」。

<a name="XIA0006" />

## <a name="xia0006-httpclientavoidmanaged"></a>XIA0006:HttpClientAvoidManaged

- **問題:** パフォーマンスの向上のため、実行可能ファイルのサイズより小さい、管理対象ではなく、ネイティブの HttpClient ハンドラーを使用することをお勧めします。 簡単に新しい標準をサポートするとします。
- **修正:** IOS プロジェクトをダブルクリック、ビルドに移動 > iOS のビルドし、NSUrlSession (iOS 7 以降) または CFNetwork iOS 7 の前のバージョンをサポートする HttpClient 実装を変更します。
