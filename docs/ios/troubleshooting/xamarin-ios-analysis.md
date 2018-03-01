---
title: "Xamarin.iOS 分析規則"
ms.topic: article
ms.prod: xamarin
ms.assetid: C29B69F5-08E4-4DCC-831E-7FD692AB0886
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/26/2017
ms.openlocfilehash: 7cf627f369b666bb54d0f512dc1361d2a685a057
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinios-analysis-rules"></a>Xamarin.iOS 分析規則


## <a name="a-namexia0001xia0001-disabledlinkerrule"></a><a name="XIA0001"/>XIA0001: DisabledLinkerRule

- **問題:**リンカーがデバッグ モード用にデバイスで無効になっています。
- **修正:**予想外の問題を回避するようにリンカーをコードを実行しようとする必要があります。
設定するには、プロジェクトに移動 > iOS ビルド > リンカー動作します。

## <a name="a-namexia0002xia0002-testcloudagentreleaserule"></a><a name="XIA0002"/>XIA0002: TestCloudAgentReleaseRule

- **問題:**プライベート API を使用するときにその Test Cloud エージェントを初期化するアプリのビルドが送信されると、Apple によって拒否されました。
- **修正:**追加や、必要な #if を修正して、コードで定義します。

## <a name="a-namexia0003xia0003-ipadebugbuildsrule"></a><a name="XIA0003"/>XIA0003: IPADebugBuildsRule

- **問題:**のみを今すぐ発行ウィザードを使用して配布する必要があると、開発者が署名キーを使用してデバッグ構成は、IPA を生成しないようにします。
- **修正:** IPA ビルド プロジェクトのオプションでのデバッグ構成を無効にします。

## <a name="a-namexia0004xia0004-missing64bitsupportrule"></a><a name="XIA0004"/>XIA0004: Missing64BitSupportRule

- **問題:**のアーキテクチャをサポートされている"リリース |デバイス"では、64 ビット互換性のある、ARM64 を見つからないはありません。 これは、Apple は、AppStore で唯一の iOS アプリを 32 ビットを受け入れません問題です。
- **修正:**倍精度浮動小数点、iOS プロジェクトをクリックし、ビルドを参照してください > iOS をビルドおよび ARM64 があるために、サポートされているアーキテクチャを変更します。

## <a name="a-namexia0005xia0005-float32rule"></a><a name="XIA0005"/>XIA0005: Float32Rule

- **問題:** float32 オプションを使用していない (--aot オプション = O = float32) 倍精度演算が多少遅くなりますが、コスト、携帯電話 に特別に膨大なパフォーマンスにつながります。 .NET を使用すること倍精度内部的も浮動小数点数、有効桁数と、場合によっては、互換性に影響は、このオプションを有効にするように注意してください。
- **修正:**倍精度浮動小数点、iOS プロジェクトをクリックし、ビルドを参照してください > iOS をビルドおよびオフにして、「64 ビット浮動小数点として 32 ビット浮動小数点のすべての操作を実行する」です。
