---
title: Xamarin で AVAudioPlayer を使用して tvOS でサウンドを再生する
description: この記事では、ヘルパークラスを使用して、Xamarin. iOS アプリケーションで AVAudioPlayer を使用してサウンドの再生を制御する方法について説明します。
ms.prod: xamarin
ms.assetid: E0305572-DC64-48BB-BD97-0A5096E6CA04
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 4dddde8d4408df6a9b9d73c0a3efff62f563591a
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030770"
---
# <a name="playing-sound-in-tvos-with-avaudioplayer-in-xamarin"></a>Xamarin で AVAudioPlayer を使用して tvOS でサウンドを再生する

## <a name="about-the-avaudioplayer"></a>AVAudioPlayer について

`AVAudioPlayer` は、メモリまたはファイルからオーディオデータを再生するために使用されます。 Apple では、ネットワークストリーミングを行ったり、低待機時間のオーディオ i/o を必要としない限り、アプリでオーディオの再生にこのクラスを使用することを推奨しています。

`AVAudioPlayer` を使用すると、次の操作を実行できます。

- 任意のループを使用して、任意の期間のサウンドを再生します。
- オプションの同期を使用して、複数のサウンドを同時に再生します。
- 再生する各サウンドのボリューム、再生速度、およびステレオ位置を制御します。
- 高速転送や巻き戻しなどの機能をサポートします。
- 再生レベルの使用状況データを取得します。

`AVAudioPlayer` は、iOS、tvOS、OS X (`.aif`、`.wav`、`.mp3`など) によって提供されるオーディオ形式のサウンドをサポートしています。

## <a name="playing-sounds-in-tvos"></a>TvOS でのサウンドの再生

TvOS は iOS と同じオーディオツールボックスクラスをサポートしているため、tvOS アプリでのオーディオの再生の詳細については、 [AVAudioPlayer のドキュメントを使用した ios の再生に](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/sound/avaudioplayer)関するドキュメントを参照してください。

## <a name="related-links"></a>関連リンク

- [AVAudioPlayer リファレンス](https://developer.apple.com/library/ios/documentation/AVFoundation/Reference/AVAudioPlayerClassReference/)
