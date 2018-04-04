---
title: コンポーネントがコンピューターに格納する場所をしますか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: f0dad6e6219d373eaa9f8410aea7d96c81eceb6b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="where-are-the-components-stored-on-my-machine"></a>コンポーネントがコンピューターに格納する場所をしますか。

アプリのプロジェクトには、Xamarin コンポーネントをインストールするときに、2 つの場所に配置を取得します。

1. [ソリューション] フォルダーのルート レベルのコンポーネント フォルダー。 ソリューション内のすべてのプロジェクトからコンポーネントを削除する場合はもこのフォルダーから削除を取得します。

2. コピーは、次の場所にも格納されます。
    - Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - Mac: `~/Library/Caches/Xamarin/Components`

ように、コンポーネントをシステムから完全に削除する削除プロジェクト/ソリューションおよび上記のキャッシュ フォルダーから。
