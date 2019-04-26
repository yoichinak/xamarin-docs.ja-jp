---
title: Visual Studio に自分のビルドの参照先ライブラリ プロジェクトが含まれないのはなぜですか
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: b9009db8-e716-43aa-b40e-6f28a8eb1b82
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: d7aeac2f433e8fdf231f5887f1537f15e2bd1976
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61341309"
---
# <a name="why-doesnt-visual-studio-include-my-referenced-library-project-in-my-build"></a>Visual Studio に自分のビルドの参照先ライブラリ プロジェクトが含まれないのはなぜですか

Visual Studio を使用して、 **Configuration Manager**をソリューションにプロジェクトが特定のビルドまたは配置構成で自動的に含まれているかを判断します。

参照先のライブラリ プロジェクトを使用して生成された一部のテンプレートは、構成に含まれる参照先のライブラリが既に手動で設定する必要がそれ以外の場合。

## <a name="how-to-use-the-configuration-manager"></a>Configuration Manager を使用する方法

1. 開いている**ビルド > Configuration Manager**
2. 例: をカスタマイズする構成を選択します**デバッグ | iPhone。**
3. 含めるプロジェクトのチェック ボックスを選択します。

> [!NOTE]
> 灰色ボックスは、自動的に処理され、すべての変更は必要ありません。

次の手順のスクリーン キャスト: [http://screencast.com/t/zLoQOpEn](http://screencast.com/t/zLoQOpEn)
