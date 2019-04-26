---
title: AVAudioPlayer で Xamarin で tvOS のサウンドの再生
description: この記事では、Xamarin.iOS アプリケーションで、AVAudioPlayer を使用してサウンドの再生を制御するヘルパー クラスを使用する方法を示します。
ms.prod: xamarin
ms.assetid: E0305572-DC64-48BB-BD97-0A5096E6CA04
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: bc1e9febea9529c3fadbacd689404562952276fe
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61414101"
---
# <a name="playing-sound-in-tvos-with-avaudioplayer-in-xamarin"></a>AVAudioPlayer で Xamarin で tvOS のサウンドの再生

## <a name="about-the-avaudioplayer"></a>AVAudioPlayer について

`AVAudioPlayer`メモリまたはファイルのいずれかからオーディオ データの再生に使用されます。 Apple では、ネットワークのストリーミングを実行しているか、オーディオ I/O の低待機時間を必要としない限り、アプリでオーディオを再生するこのクラスの使用をお勧めします。

使用することができます、`AVAudioPlayer`以下を実行します。

- 省略可能なループを設定して、任意の期間のサウンドを再生します。
- 省略可能な同期で同時に複数のサウンドを再生します。
- ボリューム、再生レート、および各サウンドの再生のステレオの配置を制御します。
- 早送りまたは巻き戻しなどの機能をサポートします。
- 再生レベルの使用状況測定データを取得します。

`AVAudioPlayer` などの iOS、tvOS、および OS X で提供されている任意のオーディオ形式でサウンドをサポートしている`.aif`、`.wav`または`.mp3`します。

## <a name="playing-sounds-in-tvos"></a>TvOS のサウンドの再生

TvOS では、iOS として同じのオーディオ ツールボックス クラスをサポートするため、iOS を参照してください[AVAudioPlayer でのサウンドを再生](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/sound/avaudioplayer)Xamarin.tvOS アプリでのオーディオの再生の詳細については完全なドキュメントです。



## <a name="related-links"></a>関連リンク

- [AVAudioPlayer 参照](https://developer.apple.com/library/ios/documentation/AVFoundation/Reference/AVAudioPlayerClassReference/)
