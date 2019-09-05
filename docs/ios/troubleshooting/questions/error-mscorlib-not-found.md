---
title: 'ランタイム エラー: The assembly mscorlib.dll was not found or could not be loaded (アセンブリ mscorlib.dll が見つからないか、読み込めませんでした)'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1027E16C-2C14-4BB5-AAAB-342F3E28E22E
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 04/03/2018
ms.openlocfilehash: 4a8e3827deadd5c5d183c61c53cbe8346949759b
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290502"
---
# <a name="runtime-error-the-assembly-mscorlibdll-was-not-found-or-could-not-be-loaded"></a>ランタイム エラー: The assembly mscorlib.dll was not found or could not be loaded (アセンブリ mscorlib.dll が見つからないか、読み込めませんでした)

```
<Warning>: The assembly mscorlib.dll was not found or could not be loaded.
<Warning>: It should have been installed in the `/Developer/MonoTouch/Source/monotouch/builds/install/target64/lib/mono/2.0/mscorlib.dll' directory.
<Warning>: Service exited with abnormal code: 1
```

この問題が発生するのは`.monotouch-64` 、署名/IPA 作成`.xcarchive`のためにで*非表示* `.monotouch-32`のおよびフォルダーが不足している場合に、ランタイムエラーがトリガーされることです。

