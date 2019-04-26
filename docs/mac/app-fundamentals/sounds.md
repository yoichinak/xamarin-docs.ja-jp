---
title: Xamarin.Mac で AVAudioPlayer でのサウンドの再生
description: このドキュメントでは、Xamarin.Mac アプリで AVAudioPlayer でのサウンドを再生する方法について説明します。 AVAudioPlayer で高レベルより詳細にについてを説明するその他のドキュメントへのリンクがについて説明します。
ms.prod: xamarin
ms.assetid: 4A683A94-F75D-4EAF-8497-E9443653250B
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 10/19/2016
ms.openlocfilehash: 9aeb7bbfc2fddef1f690b5299ec060c475ea1ce7
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61234865"
---
# <a name="playing-sound-with-avaudioplayer-in-xamarinmac"></a>Xamarin.Mac で AVAudioPlayer でのサウンドの再生

## <a name="about-the-avaudioplayer"></a>AVAudioPlayer について

`AVAudioPlayer`クラスは、メモリまたはファイルのいずれかからオーディオ データの再生に使用されます。 Apple では、ネットワークのストリーミングを実行しているか、オーディオ I/O の低待機時間を必要としない限り、アプリでオーディオを再生するこのクラスの使用をお勧めします。

使用することができます、`AVAudioPlayer`次を実行するために。

- 省略可能なループを設定して、任意の期間のサウンドを再生します。
- 省略可能な同期で同時に複数のサウンドを再生します。
- ボリューム、再生レート、および各サウンドの再生のステレオの配置を制御します。
- 早送りまたは巻き戻しなどの機能をサポートします。
- 再生レベルの使用状況測定データを取得します。

`AVAudioPlayer` iOS、tvOS、および macOS .aif、.wav または .mp3 などによって提供される任意のオーディオ形式でサウンドをサポートしています。

## <a name="playing-sounds-in-macos"></a>MacOS のサウンドの再生

MacOS では、iOS として同じのオーディオ ツールボックス クラスをサポートするため、iOS を参照してください[AVAudioPlayer でのサウンドを再生](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/sound/avaudioplayer)Xamarin.Mac アプリでオーディオの再生の詳細については完全なドキュメントです。

## <a name="related-links"></a>関連リンク

- [AVAudioPlayer 参照](https://developer.apple.com/documentation/avfoundation/avaudioplayer)
