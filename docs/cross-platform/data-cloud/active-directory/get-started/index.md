---
title: Azure Active Directory
description: このドキュメントでは、モバイルアプリが Azure Active Directory で認証できるようにするために従う必要がある手順について説明します。
ms.prod: xamarin
ms.assetid: 70B3C2AB-CB4D-420C-9CFA-20CCFA0E3C78
author: conceptdev
ms.author: crdun
ms.date: 03/23/2017
ms.openlocfilehash: f51c6af9ed38910f44db2c0886c474deafa096d7
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70289993"
---
# <a name="azure-active-directory"></a>Azure Active Directory

_アプリを登録して Azure Active Directory を使用する_

Azure Active Directory を使用すると、開発者は、従業員がシステムにサインインしたり、電子メールをチェックしたりするときに使用するのと同じ組織アカウントを使用して、ファイル、リンク、Web Api などのリソースをセキュリティで保護することができます。

Azure Active Directory で認証できるモバイルアプリケーションを開発するには、3つの手順が必要です。
最初の2つの手順は、使用する予定のサービスに関係なく、通常は同じです。 3番目の手順は、サービスの種類ごとに異なります。

  1. *Windowsazure.com* portal での Azure Active Directory への[登録](~/cross-platform/data-cloud/active-directory/get-started/register.md)、
  2. [サービスを構成](~/cross-platform/data-cloud/active-directory/get-started/configure.md)します。
  3. サービスを使用してモバイルアプリを開発します。

アクセスできるさまざまなサービスの例を次に示します。

- [Graph API](~/cross-platform/data-cloud/active-directory/graph.md)
- Web API
- Office365


## <a name="conclusion"></a>まとめ

上記の手順を使用して、Azure Active Directory に対してモバイルアプリを認証できます。 Active Directory 認証ライブラリ (ADAL) を使用すると、コードの大部分を維持しながら、プラットフォーム間で共有できるようにするだけで、コードがより簡単になります。



## <a name="related-links"></a>関連リンク

- [Microsoft NativeClient のサンプル](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
