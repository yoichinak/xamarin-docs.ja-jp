---
title: コンポーネントがコンピューターに格納する場所をしますか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
author: asb3993
ms.author: amburns
ms.openlocfilehash: a53045a6179a26b30d824976d11fd2769a84811e
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
ms.locfileid: "33919398"
---
# <a name="where-are-the-components-stored-on-my-machine"></a>コンポーネントがコンピューターに格納する場所をしますか。

アプリのプロジェクトには、Xamarin コンポーネントをインストールするときに、2 つの場所に配置を取得します。

1. [ソリューション] フォルダーのルート レベルのコンポーネント フォルダー。 ソリューション内のすべてのプロジェクトからコンポーネントを削除する場合はもこのフォルダーから削除を取得します。

2. コピーは、次の場所にも格納されます。
    - Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - Mac: `~/Library/Caches/Xamarin/Components`

ように、コンポーネントをシステムから完全に削除する削除プロジェクト/ソリューションおよび上記のキャッシュ フォルダーから。
