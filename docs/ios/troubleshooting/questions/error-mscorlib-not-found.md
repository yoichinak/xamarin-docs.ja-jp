---
title: 'ランタイムエラー: アセンブリ mscorlib.dll が見つからなかったか、または読み込めませんでした'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1027E16C-2C14-4BB5-AAAB-342F3E28E22E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: 1ca6aa8c08d51b91f6a31407328f8949062bbede
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031163"
---
# <a name="runtime-error-the-assembly-mscorlibdll-was-not-found-or-could-not-be-loaded"></a>ランタイムエラー: アセンブリ mscorlib.dll が見つからなかったか、または読み込めませんでした

```
<Warning>: The assembly mscorlib.dll was not found or could not be loaded.
<Warning>: It should have been installed in the `/Developer/MonoTouch/Source/monotouch/builds/install/target64/lib/mono/2.0/mscorlib.dll' directory.
<Warning>: Service exited with abnormal code: 1
```

この問題は、署名/IPA 作成のために*非表示*の `.monotouch-32` と `.monotouch-64` フォルダーが `.xcarchive` にない場合に発生し、ランタイムエラーをトリガーします。
