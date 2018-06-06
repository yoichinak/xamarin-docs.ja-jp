---
title: Xamarin.iOS 分析規則
description: このドキュメントより/better optimized 設定が使用可能なかどうかを確認するのに役立つ Xamarin.iOS プロジェクト設定をチェックする分析規則のセットについて説明します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C29B69F5-08E4-4DCC-831E-7FD692AB0886
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: 25d2936f70981ed239626193ba6e4993f1378108
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788899"
---
# <a name="xamarinios-analysis-rules"></a>Xamarin.iOS 分析規則

Xamarin.iOS 分析は、高い/高い最適化された設定が使用可能な場合の判断に役立つプロジェクトの設定をチェックする規則のセットです。

分析ルールを早い段階で品質向上を図るを検索し、開発時間を節約できる限り頻繁に実行します。

実行するには、ルールでは、Visual Studio for Mac のメニューで選択**プロジェクト > コード分析を実行**です。

> [!NOTE]
> Xamarin.iOS 分析は、現在選択されている構成でのみ実行されます。 デバッグ用ツールの実行を強くお勧め**と**リリース構成します。

<a name="XIA0001" />

## <a name="xia0001-disabledlinkerrule"></a>XIA0001: DisabledLinkerRule

- **問題:** リンカーがデバッグ モード用にデバイスで無効になっています。
- **修正:** 予想外の問題を回避するようにリンカーをコードを実行しようとする必要があります。
設定するには、プロジェクトに移動 > iOS ビルド > リンカー動作します。

<a name="XIA0002" />

## <a name="xia0002-testcloudagentreleaserule"></a>XIA0002: TestCloudAgentReleaseRule

- **問題:** プライベート API を使用するときにその Test Cloud エージェントを初期化するアプリのビルドが送信されると、Apple によって拒否されました。
- **修正:** 追加や、必要な #if を修正して、コードで定義します。

<a name="XIA0003" />

## <a name="xia0003-ipadebugbuildsrule"></a>XIA0003: IPADebugBuildsRule

- **問題:** のみを今すぐ発行ウィザードを使用して配布する必要があると、開発者が署名キーを使用してデバッグ構成は、IPA を生成しないようにします。
- **修正:** IPA ビルド プロジェクトのオプションでのデバッグ構成を無効にします。

<a name="XIA0004" />

## <a name="xia0004-missing64bitsupportrule"></a>XIA0004: Missing64BitSupportRule

- **問題:** のアーキテクチャをサポートされている"リリース |デバイス"では、64 ビット互換性のある、ARM64 を見つからないはありません。 これは、Apple は、AppStore で唯一の iOS アプリを 32 ビットを受け入れません問題です。
- **修正:** 倍精度浮動小数点、iOS プロジェクトをクリックし、ビルドを参照してください > iOS をビルドおよび ARM64 があるために、サポートされているアーキテクチャを変更します。

<a name="XIA0005" />

## <a name="xia0005-float32rule"></a>XIA0005: Float32Rule

- **問題:** float32 オプションを使用していない (--aot オプション = O = float32) 倍精度演算が多少遅くなりますが、携帯電話で特にコスト、膨大なパフォーマンスにつながります。 .NET を使用すること倍精度内部的も浮動小数点数、有効桁数と、場合によっては、互換性に影響は、このオプションを有効にするように注意してください。
- **修正:** 倍精度浮動小数点、iOS プロジェクトをクリックし、ビルドを参照してください > iOS をビルドおよびオフにして、「64 ビット浮動小数点として 32 ビット浮動小数点のすべての操作を実行する」です。

<a name="XIA0006" />

## <a name="xia0006-httpclientavoidmanaged"></a>XIA0006: HttpClientAvoidManaged

- **問題:** 実行可能ファイル サイズの小さいパフォーマンス向上のため管理されている 1 つではなくネイティブ HttpClient ハンドラーを使用することをお勧めとを簡単に新しい標準をサポートします。
- **修正:** 倍精度浮動小数点、iOS プロジェクトをクリックし、ビルドを参照してください > iOS をビルドおよびを iOS 7 の前のバージョンをサポートするために NSUrlSession (iOS 7 以降) または CFNetwork HttpClient の実装を変更します。
