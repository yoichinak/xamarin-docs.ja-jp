---
title: コンポーネントはマシンのどこに保存されていますか
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 4152c8ef7eeba3748d9244e27e48f3f9a2c0019b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61359106"
---
# <a name="where-are-the-components-stored-on-my-machine"></a>コンポーネントはマシンのどこに保存されていますか

アプリのプロジェクトには、Xamarin コンポーネントをインストールするときに、2 つの場所に配置を取得します。

1. ソリューション フォルダーのルート レベルのコンポーネント フォルダー。 ソリューション内のすべてのプロジェクトからコンポーネントを削除する場合もこのフォルダーから、削除します。

2. コピーは、次の場所にも格納されます。
    - Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - Mac の場合: `~/Library/Caches/Xamarin/Components`

ので、システムから完全にコンポーネントを削除する削除上のキャッシュ フォルダーとプロジェクト/ソリューションから。
