---
title: 理由 Visual Studio を含まない参照先のライブラリ プロジェクトでビルドしますか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: b9009db8-e716-43aa-b40e-6f28a8eb1b82
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: cb9b3689ab6a12d99f9694583cd0fd50a6f5c72c
ms.sourcegitcommit: 6f7033a598407b3e77914a85a3f650544a4b6339
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
# <a name="why-doesnt-visual-studio-include-my-referenced-library-project-in-my-build"></a>なぜ Visual Studio が含まれていない参照先のライブラリ プロジェクトにはでビルド

Visual Studio を使用して、 **Configuration Manager**先となるプロジェクトをソリューションでは、特定のビルドまたは配置の構成に自動的に追加を決定します。

参照先のライブラリ プロジェクトで生成される一部のテンプレートが、構成に含まれる参照先のライブラリそれ以外の場合、手動で設定する必要があります。

## <a name="how-to-use-the-configuration-manager"></a>構成マネージャーを使用する方法

1. 開いている**ビルド > 構成マネージャー**
2. 例: をカスタマイズする構成を選択**デバッグ | iPhone**
3. 含めるプロジェクトのチェック ボックスを選択します。

> [!NOTE]
> 灰色ボックスが自動的に処理され、変更する必要はありません。

これらの手順のスクリーン キャスト。 [http://screencast.com/t/zLoQOpEn](http://screencast.com/t/zLoQOpEn)
