---
title: Azure Active Directory
description: "Azure Active Directory を使用するアプリを登録します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 70B3C2AB-CB4D-420C-9CFA-20CCFA0E3C78
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: bf39b394903c3322ee617289ffe583a22a39fb20
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="azure-active-directory"></a>Azure Active Directory

_Azure Active Directory を使用するアプリを登録します。_

Azure Active Directory では、セキュリティで保護されたリソース ファイル、リンク、および従業員のシステムにサインインしたり、電子メールをチェックを使用して同じ組織アカウントを使用して Web Api などをすることができます。

Azure Active Directory で認証できますが、モバイル アプリケーションの開発には、次の 3 つの手順が含まれます。
最初の 2 つの手順は、一般にどのようなサービスを使用する予定に関係なく同じです。 3 番目の手順は、サービスの種類ごとに異なります。

  1. [Azure Active Directory に登録する](~/cross-platform/data-cloud/active-directory/get-started/register.md)上、 *windowsazure.com*ポータルで、
  2. [サービスを構成する](~/cross-platform/data-cloud/active-directory/get-started/configure.md)です。
  3. サービスを使用してモバイル アプリを開発します。

アクセスできる別のサービスの例としては次のとおりです。

- [Graph API](~/cross-platform/data-cloud/active-directory/graph.md)
- Web API
- Office365


## <a name="conclusion"></a>まとめ

上記の手順を使用して Azure Active Directory に対してモバイル アプリを認証できます。 Active Directory 認証ライブラリ (ADAL) はより簡単により少ない行のコード、コードのほとんどを維持しながらとなり、共有可能なプラットフォーム間で同じです。



## <a name="related-links"></a>関連リンク

- [Microsoft NativeClient サンプル](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
