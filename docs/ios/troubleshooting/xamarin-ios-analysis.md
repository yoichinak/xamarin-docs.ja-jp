---
title: Xamarin. iOS 分析ルール
description: このドキュメントでは、Xamarin. iOS プロジェクトの設定を確認する一連の分析規則について説明します。これにより、より適切に最適化された設定が使用可能かどうかを判断できます。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C29B69F5-08E4-4DCC-831E-7FD692AB0886
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/06/2018
ms.openlocfilehash: 5f968e01cc0b866f94f524728b4bba1e759e8bf8
ms.sourcegitcommit: 9f37dc00c2adab958025ad1cdba9c37f0acbccd0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/14/2019
ms.locfileid: "69012386"
---
# <a name="xamarinios-analysis-rules"></a>Xamarin. iOS 分析ルール

Xamarin. iOS 分析は、プロジェクトの設定を確認する一連の規則です。これにより、より適切な、より最適化された設定が使用可能かどうかを判断できます。

できるだけ早く分析ルールを実行して、開発時間を短縮し、開発時間を節約します。

ルールを実行するには、Visual Studio for Mac のメニューで [プロジェクト] を選択し、[**コード分析の実行] >** クリックします。

> [!NOTE]
> Xamarin。 iOS 分析は、現在選択されている構成でのみ実行されます。 デバッグ構成**と**リリース構成用にツールを実行することを強くお勧めします。

<a name="XIA0001" />

## <a name="xia0001-disabledlinkerrule"></a>XIA0001:DisabledLinkerRule

- **問題:** デバッグモードでは、リンカーはデバイスで無効になっています。
- **ファイル**予想外の事態を避けるために、リンカーを使用してコードを実行する必要があります。
設定するには、[プロジェクト] > [iOS ビルド > リンカーの動作] の各ページにアクセスします。

<a name="XIA0002" />

## <a name="xia0002-testcloudagentreleaserule"></a>XIA0002:TestCloudAgentReleaseRule

- **問題:** Test Cloud エージェントを初期化するアプリビルドは、プライベート API を使用するため、送信時に Apple によって拒否されます。
- **ファイル**必要な #if を追加または修正し、コードでを定義します。

<a name="XIA0003" />

## <a name="xia0003-ipadebugbuildsrule"></a>XIA0003:IPADebugBuildsRule

- **問題:** 開発者署名キーを使用するデバッグ構成では、IPA を生成しないでください。ディストリビューションで必要なのは、公開ウィザードを使用するようになったためです。
- **ファイル**デバッグ構成のプロジェクトオプションで IPA ビルドを無効にします。

<a name="XIA0004" />

## <a name="xia0004-missing64bitsupportrule"></a>XIA0004:Missing64BitSupportRule

- **問題:** "Release |" のサポートされているアーキテクチャデバイス "64 ビット互換性がありません。 ARM64 がありません。 Apple は、AppStore 内の32ビットのみの iOS アプリを受け入れないため、これは問題です。
- **ファイル**IOS プロジェクトをダブルクリックし、[ビルド > iOS ビルド] に移動して、サポートされているアーキテクチャを変更して ARM64 します。

<a name="XIA0005" />

## <a name="xia0005-float32rule"></a>XIA0005:Float32Rule

- **問題:** Float32 オプション (--aot-options =-O = float32) を使用しない場合は、hefty のパフォーマンスコストが生じます。特に mobile では、倍精度の数値演算はある程度までが遅くなります。 .NET では、float の場合でも、内部的に倍精度が使用されるため、このオプションを有効にすると、精度と、場合によっては互換性に影響します。
- **ファイル**IOS プロジェクトをダブルクリックし、[ビルド > iOS ビルド] にアクセスして、[32 ビット浮動小数点演算を64ビット浮動小数点演算で実行する] をオフにします。

<a name="XIA0006" />

## <a name="xia0006-httpclientavoidmanaged"></a>XIA0006:HttpClientAvoidManaged

- **問題:** パフォーマンスを向上させ、実行可能ファイルのサイズを小さくし、新しい標準を簡単にサポートするために、マネージドではなくネイティブの HttpClient ハンドラーを使用することをお勧めします。
- **ファイル**IOS プロジェクトをダブルクリックし、[ビルド > iOS のビルド] に移動して、HttpClient の実装を N、ios 7 以降のバージョンをサポートするように、または CFNetwork に変更します。

<a name="XIA0007" />

## <a name="xia0007-usellvmrule"></a>XIA0007:UseLLVMRule

- **問題:** Release | iPhone 構成では、ビルド時間を犠牲にして実行した方が高速なコードを生成する LLVM コンパイラを有効にすることをお勧めします。
- **ファイル**IOS プロジェクトをダブルクリックし、[Build > iOS Build]、[Release | iPhone] の順に選択して、LLVM 最適化コンパイラオプションを確認します。
