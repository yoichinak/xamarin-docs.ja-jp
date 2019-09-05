---
title: コンポーネントはマシンのどこに保存されていますか
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
author: conceptdev
ms.author: crdun
ms.date: 05/08/2018
ms.openlocfilehash: 7725dbff994ffcef9734ad07c6b506064d9c5b2b
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70285050"
---
# <a name="where-are-the-components-stored-on-my-machine"></a>コンポーネントはマシンのどこに保存されていますか

Xamarin コンポーネントをアプリプロジェクトにインストールするたびに、次の2つの場所に配置されます。

1. ソリューションフォルダーのルートレベルにあるコンポーネントフォルダー。 ソリューション内のすべてのプロジェクトからコンポーネントを削除すると、このフォルダーからも削除されます。

2. コピーは次の場所にも格納されます。
    - Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - Mac`~/Library/Caches/Xamarin/Components`

システムからコンポーネントを完全に削除するには、プロジェクト/ソリューションから、または上記のキャッシュフォルダーからコンポーネントを削除します。
