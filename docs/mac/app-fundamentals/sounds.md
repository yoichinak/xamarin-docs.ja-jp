---
title: Xamarin.Mac で AVAudioPlayer でサウンドの再生
description: このドキュメントでは、Xamarin.Mac アプリで AVAudioPlayer でサウンドを再生する方法について説明します。 高レベルより詳細にについてを説明するその他のドキュメントへのリンクで AVAudioPlayer についても説明します。
ms.prod: xamarin
ms.assetid: 4A683A94-F75D-4EAF-8497-E9443653250B
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/19/2016
ms.openlocfilehash: 9e5b9ec43189999f8a0aee29eb50221b494e2133
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791856"
---
# <a name="playing-sound-with-avaudioplayer-in-xamarinmac"></a>Xamarin.Mac で AVAudioPlayer でサウンドの再生

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
