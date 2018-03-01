---
title: "コンポーネントがコンピューターに格納する場所をしますか。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 9549363d7aeb5ff7a5cfa79eb38443aaa80b7019
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="where-are-the-components-stored-on-my-machine"></a>コンポーネントがコンピューターに格納する場所をしますか。

アプリのプロジェクトには、Xamarin コンポーネントをインストールするときに、2 つの場所に配置を取得します。

1. [ソリューション] フォルダーのルート レベルのコンポーネント フォルダー。 ソリューション内のすべてのプロジェクトからコンポーネントを削除する場合はもこのフォルダーから削除を取得します。

2. コピーは、次の場所にも格納されます。
    - Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - Mac: `~/Library/Caches/Xamarin/Components`

ように、コンポーネントをシステムから完全に削除する削除プロジェクト/ソリューションおよび上記のキャッシュ フォルダーから。
