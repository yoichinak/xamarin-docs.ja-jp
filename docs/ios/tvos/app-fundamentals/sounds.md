---
title: Xamarin で AVAudioPlayer で tvOS でサウンドの再生
description: この記事では、Xamarin.iOS アプリケーションで、AVAudioPlayer を使用してサウンドの再生を制御するためのヘルパー クラスを使用する方法を示します。
ms.prod: xamarin
ms.assetid: E0305572-DC64-48BB-BD97-0A5096E6CA04
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 7d95a8ea6c22c0d897d8ccfe0c2ca401f6523783
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788635"
---
# <a name="playing-sound-in-tvos-with-avaudioplayer-in-xamarin"></a>Xamarin で AVAudioPlayer で tvOS でサウンドの再生

## <a name="about-the-avaudioplayer"></a>AVAudioPlayer について

`AVAudioPlayer`は、メモリまたはファイルから再生オーディオ データを使用します。 Apple では、ネットワークのストリーミングを実行しているか、低待機時間のオーディオ I/O を必要としない限り、アプリでオーディオを再生するこのクラスを使用することをお勧めします。

使用することができます、`AVAudioPlayer`を次を行うには。

- 省略可能なループを設定して、期間の長さのサウンドを再生します。
- 省略可能な同期で同時に複数のサウンドを再生します。
- ボリューム、再生レート、および各サウンドの再生のステレオの配置を制御します。
- 早送りまたは巻き戻しなどの機能をサポートします。
- 使用状況測定データの再生レベルを取得します。

`AVAudioPlayer` 任意のオーディオ形式など、iOS、tvOS および OS X によって提供されるサウンドをサポートしている`.aif`、`.wav`または`.mp3`です。

## <a name="playing-sounds-in-tvos"></a>TvOS のサウンドの再生

TvOS iOS として同じオーディオ ツールボックス クラスをサポートしているため、iOS を参照してください[AVAudioPlayer でサウンドを再生](http://developer.xamarin.com/recipes/ios/media/sound/avaudioplayer/)Xamarin.tvOS アプリでのオーディオの再生の詳細については、ドキュメントです。



## <a name="related-links"></a>関連リンク

- [AVAudioPlayer 参照](https://developer.apple.com/library/ios/documentation/AVFoundation/Reference/AVAudioPlayerClassReference/)
