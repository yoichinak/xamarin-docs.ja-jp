---
title: 手順 2. モバイル アプリケーションのサービスへのアクセスを構成します。
description: このドキュメントでは、Azure Active Directory によって保護された Azure のアプリケーションにアクセスできる Xamarin アプリケーションを提供する方法について説明します。
ms.prod: xamarin
ms.assetid: 8A14A457-F72E-4B08-B4B6-801F7619F893
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 2a9baab9215ae2d30e4daf6800a116c95165da42
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61188302"
---
# <a name="step-2-configure-service-access-for-mobile-application"></a>手順 2. モバイル アプリケーションのサービスへのアクセスを構成します。

任意のリソースなどの web アプリケーション、web サービスなどは、Azure Active Directory によってセキュリティで保護する必要がある、ときに、登録が必要です。 すべてのセキュリティで保護されたアプリケーションまたはサービスを確認できます**アプリケーション**タブ。ここでモバイル アプリケーションからアクセスし、それにアクセスできるようにする必要があるアプリケーションを選択できます。

1. **構成**タブで、**他のアプリケーションに対するアクセス許可**セクション。

  ![](configure-images/2.1-configure.png "[構成] タブで、他のアプリケーション セクションへのアクセス許可を見つける")

2.  をクリックして**アプリケーションを追加**ボタンをクリックします。 ポップアップの次の画面では、Azure Active Directory によってセキュリティで保護されているすべてのアプリケーションの一覧が表示されます。 モバイル アプリケーションからアクセスする必要があるアプリケーションを選択します。

  ![](configure-images/2.2-add-application.png "モバイル アプリケーションからアクセスする必要があるアプリケーションを選択します。")

3. アプリケーションを選択するで新しく追加されたアプリケーションをもう一度選択**他のアプリケーションに対するアクセス許可**セクションし、適切な権限を付与します。

  ![](configure-images/2.3-permissions.png "アプリケーションを選択すると、もう一度その他のアプリケーション セクションへのアクセス許可で新しく追加されたアプリケーションを選択し、適切な権限を与える")

4. 最後に、**保存**構成します。 これらのサービスがモバイル アプリケーションで使用可能になります。



## <a name="related-links"></a>関連リンク

- [Microsoft NativeClient サンプル](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
