---
title: 手順 2. モバイルアプリケーションのサービスアクセスを構成する
description: このドキュメントでは、Azure Active Directory によって保護された Azure アプリケーションへのアクセスを Xamarin アプリケーションに提供する方法について説明します。
ms.prod: xamarin
ms.assetid: 8A14A457-F72E-4B08-B4B6-801F7619F893
author: conceptdev
ms.author: crdun
ms.date: 03/23/2017
ms.openlocfilehash: 1f0cdec005dc210600977d5c8f5606cff6570989
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290013"
---
# <a name="step-2-configure-service-access-for-mobile-application"></a>手順 2. モバイルアプリケーションのサービスアクセスを構成する

Web アプリケーション、web サービスなどのリソースを Azure Active Directory によって保護する必要がある場合は、必ず登録する必要があります。 すべてのセキュリティで保護されたアプリケーションまたはサービスが **[アプリケーション]** タブに表示されます。ここでは、モバイルアプリケーションからアクセスする必要があるアプリケーションを選択し、アクセス権を付与することができます。

1. **[構成]** タブで、 **[他のアプリケーションに対するアクセス許可]** セクションを探します。

   ![](configure-images/2.1-configure.png "[構成] タブで、[他のアプリケーションに対するアクセス許可] セクションを探します。")

2. **[アプリケーションの追加]** ボタンをクリックします。 次の画面のポップアップに、Azure Active Directory によって保護されているすべてのアプリケーションの一覧が表示されます。 モバイルアプリケーションからアクセスする必要があるアプリケーションを選択します。

   ![](configure-images/2.2-add-application.png "モバイルアプリケーションからアクセスする必要があるアプリケーションを選択します")

3. アプリケーションを選択した後、もう一度、 **[他のアプリケーションに対するアクセス許可]** セクションで新しく追加したアプリケーションを選択し、適切な権限を付与します。

   ![](configure-images/2.3-permissions.png "アプリケーションを選択した後、もう一度、[他のアプリケーションに対するアクセス許可] セクションで新しく追加したアプリケーションを選択し、適切な権限を付与します。")

4. 最後に、構成を**保存**します。 これらのサービスは、モバイルアプリケーションで使用できるようになりました。



## <a name="related-links"></a>関連リンク

- [Microsoft NativeClient のサンプル](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
