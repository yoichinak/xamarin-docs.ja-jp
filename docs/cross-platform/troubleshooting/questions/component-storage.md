---
title: コンポーネントはマシンのどこに保存されていますか
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
author: davidortinau
ms.author: daortin
ms.date: 05/08/2018
ms.openlocfilehash: 9447cf903c8789078e66082e720eeecfa7bb3e0d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73014236"
---
# <a name="where-are-the-components-stored-on-my-machine"></a>コンポーネントはマシンのどこに保存されていますか

Xamarin コンポーネントをアプリプロジェクトにインストールするたびに、次の2つの場所に配置されます。

1. ソリューションフォルダーのルートレベルにあるコンポーネントフォルダー。 ソリューション内のすべてのプロジェクトからコンポーネントを削除すると、このフォルダーからも削除されます。

2. コピーは次の場所にも格納されます。
    - Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - Mac: `~/Library/Caches/Xamarin/Components`

システムからコンポーネントを完全に削除するには、プロジェクト/ソリューションから、または上記のキャッシュフォルダーからコンポーネントを削除します。
