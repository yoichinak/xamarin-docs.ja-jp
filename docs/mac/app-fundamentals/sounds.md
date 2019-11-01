---
title: Xamarin. Mac で AVAudioPlayer を使用してサウンドを再生する
description: このドキュメントでは、Xamarin. Mac アプリで AVAudioPlayer を使用してサウンドを再生する方法について説明します。 AVAudioPlayer の概要について説明し、その他のドキュメントへのリンクを示します。詳細については、こちらを参照してください。
ms.prod: xamarin
ms.assetid: 4A683A94-F75D-4EAF-8497-E9443653250B
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 10/19/2016
ms.openlocfilehash: 18043a88a129d48a1cad3b9ee15b6989d50ad126
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030044"
---
# <a name="playing-sound-with-avaudioplayer-in-xamarinmac"></a>Xamarin. Mac で AVAudioPlayer を使用してサウンドを再生する

## <a name="about-the-avaudioplayer"></a>AVAudioPlayer について

`AVAudioPlayer` クラスは、メモリまたはファイルからオーディオデータを再生するために使用されます。 Apple では、ネットワークストリーミングを行ったり、低待機時間のオーディオ i/o を必要としない限り、アプリでオーディオの再生にこのクラスを使用することを推奨しています。

`AVAudioPlayer` クラスを使用して、次の操作を行うことができます。

- 任意のループを使用して、任意の期間のサウンドを再生します。
- オプションの同期を使用して、複数のサウンドを同時に再生します。
- 再生する各サウンドのボリューム、再生速度、およびステレオ位置を制御します。
- 高速転送や巻き戻しなどの機能をサポートします。
- 再生レベルの使用状況データを取得します。

`AVAudioPlayer` は、iOS、tvOS、macOS (たとえば、aif、.wav、.mp3) によって提供されるオーディオ形式のサウンドをサポートしています。

## <a name="playing-sounds-in-macos"></a>MacOS でのサウンドの再生

MacOS では iOS と同じオーディオツールボックスクラスがサポートされているため、Xamarin. Mac アプリでオーディオを再生する方法の詳細については、 [AVAudioPlayer のドキュメントで](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/sound/avaudioplayer)Ios のサウンドを再生してください。

## <a name="related-links"></a>関連リンク

- [AVAudioPlayer リファレンス](https://developer.apple.com/documentation/avfoundation/avaudioplayer)
