---
title: Visual Studio に自分のビルドの参照先ライブラリ プロジェクトが含まれないのはなぜですか
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: b9009db8-e716-43aa-b40e-6f28a8eb1b82
author: conceptdev
ms.author: crdun
ms.date: 12/02/2016
ms.openlocfilehash: 37fa93ef7377456d61d1a5f5de56d5de6b0f3c7f
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70282908"
---
# <a name="why-doesnt-visual-studio-include-my-referenced-library-project-in-my-build"></a>Visual Studio に自分のビルドの参照先ライブラリ プロジェクトが含まれないのはなぜですか

Visual Studio は、 **Configuration Manager**を使用して、ソリューション内のどのプロジェクトが特定のビルドまたは配置構成に自動的に含まれるかを判断します。

参照ライブラリプロジェクトで生成されたテンプレートの中には、既に構成に含まれている参照ライブラリが含まれているものがあります。それ以外の場合は、手動で設定する必要があります。

## <a name="how-to-use-the-configuration-manager"></a>Configuration Manager の使用方法

1. **ビルド > Configuration Manager**を開く
2. カスタマイズする構成を選択します。例: **Debug | iPhone**
3. 含めるプロジェクトのチェックボックスをオンにします。

> [!NOTE]
> グレー表示のボックスは自動的に処理されるため、変更を必要としません。

これらの手順のスクリーンキャスト:[http://screencast.com/t/zLoQOpEn](http://screencast.com/t/zLoQOpEn)
