---
title: ContentProvider の使用
ms.prod: xamarin
ms.assetid: 251F7557-328D-0132-F39D-595920A28B87
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 142541dcc35b55e43b54eeb729c486ac9fc88b54
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510067"
---
# <a name="using-a-contentprovider-with-xamarinandroid"></a>コンテンツプロバイダーと Xamarin Android の使用

カーソルアダプターを使用して、ContentProvider のデータを表示することもできます。
ContentProviders を使用すると、他のアプリケーションによって公開されているデータにアクセスできます (連絡先、メディア、予定表の情報などの Android システムデータを含む)。

ContentProvider にアクセスするには、LoaderManager を使用するカーソルローダーを使用することをお勧めします。 LoaderManager は、Android 3.0 (API レベル11、Honeycomb) で、ブロックしているタスクをメインスレッドから移動するために導入されました。カーソルローダーを使用すると、ListView にバインドされる前に、データをスレッドに読み込むことができます。

詳細については、「 [ContentProviders の概要](~/android/platform/content-providers/index.md)」を参照してください。

