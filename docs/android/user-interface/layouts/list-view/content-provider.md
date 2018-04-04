---
title: 使用して、ContentProvider
ms.prod: xamarin
ms.assetid: 251F7557-328D-0132-F39D-595920A28B87
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: b9b6340d4aaf386c7b4be8ebf366589582771be2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="using-a-contentprovider"></a>使用して、ContentProvider

CursorAdapters は、ContentProvider からデータを表示することもできます。
他のアプリケーションによって公開されているデータにアクセスすることは ContentProviders (連絡先のように Android のシステム データを含むメディアと予定表の情報)。

LoaderManager を使用して CursorLoader では、ContentProvider にアクセスすることをお勧めします。 LoaderManager は、メイン スレッドをブロックしているタスクを移動する Android の 3.0 (API レベル 11、Honeycomb) で導入されましたでき、CursorLoader を使用して、データを表示するための ListView にバインドされる前に、スレッドに読み込まれます。

参照してください[ContentProviders 入門](~/android/platform/content-providers/index.md)詳細についてはします。

