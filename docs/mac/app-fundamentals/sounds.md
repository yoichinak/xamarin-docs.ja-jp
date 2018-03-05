---
title: "AVAudioPlayer でサウンドの再生"
description: "この記事では、ヘルパー クラスを使用して、AVAudioPlayer を使用してサウンドの再生を制御する方法を示します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4A683A94-F75D-4EAF-8497-E9443653250B
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/19/2016
ms.openlocfilehash: b3eb3f16f358becb22029cee295ef6064caad85a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="playing-sound-with-avaudioplayer"></a>AVAudioPlayer でサウンドの再生

_この記事では、ヘルパー クラスを使用して、AVAudioPlayer を使用してサウンドの再生を制御する方法を示します。_

## <a name="about-the-avaudioplayer"></a>AVAudioPlayer について

`AVAudioPlayer`クラスは、メモリまたはファイルから再生オーディオ データに使用します。 Apple では、ネットワークのストリーミングを実行しているか、低待機時間のオーディオ I/O を必要としない限り、アプリでオーディオを再生するこのクラスを使用することをお勧めします。

使用することができます、`AVAudioPlayer`クラスを次を行うには。

- 省略可能なループを設定して、期間の長さのサウンドを再生します。
- 省略可能な同期で同時に複数のサウンドを再生します。
- ボリューム、再生レート、および各サウンドの再生のステレオの配置を制御します。
- 早送りまたは巻き戻しなどの機能をサポートします。
- 使用状況測定データの再生レベルを取得します。

`AVAudioPlayer` iOS、tvOS および macOS .aif、.wav または .mp3 などによって提供されるすべてのオーディオ形式でサウンドをサポートします。

## <a name="playing-sounds-in-macos"></a>MacOS でサウンドの再生

MacOS iOS として同じオーディオ ツールボックス クラスをサポートしているため、iOS を参照してください[AVAudioPlayer でサウンドを再生](https://developer.xamarin.com/recipes/ios/media/sound/avaudioplayer/)Xamarin.Mac アプリでのオーディオの再生の詳細については、ドキュメントです。



## <a name="related-links"></a>関連リンク

- [AVAudioPlayer 参照](https://developer.apple.com/documentation/avfoundation/avaudioplayer)
