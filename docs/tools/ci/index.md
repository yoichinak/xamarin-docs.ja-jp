---
title: Xamarin を使用した継続的インテグレーションの概要
description: このドキュメントには、Xamarin を使用した継続的インテグレーションを説明するガイドへのリンクがします。 リンクされたコンテンツは、継続的インテグレーションの概要を説明し、Center ビルドのアプリ、TeamCity、および Jenkins を説明します。
ms.prod: xamarin
ms.assetid: 99484E96-DC69-4697-8BBB-1B44C5CBB5ED
author: topgenorth
ms.author: toopge
ms.date: 05/04/2017
ms.openlocfilehash: 1a38160b4f1536c0e0e393cb9ec3482624a40681
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793745"
---
# <a name="introduction-to-continuous-integration-with-xamarin"></a>Xamarin を使用した継続的インテグレーションの概要

> [!Video https://youtube.com/embed/wXgnh2Q7Uv8]

##  <a name="introduction-to-continuous-integrationtoolsciintro-to-cimd"></a>[継続的インテグレーションの概要](~/tools/ci/intro-to-ci.md)

このセクションでは、継続的インテグレーションとそのリレーションシップに関連するさまざまなコンポーネントについて説明します。 それには、次の特定のセクションに記載されている継続的インテグレーション環境について概説されています。

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="working-with-continuous-integration-environments"></a>継続的インテグレーション環境での作業

### <a name="using-app-center-build-with-xamarinappcenterbuildxamarin"></a>[Xamarin での App Center ビルドの使用](/appcenter/build/xamarin/)

GitHub、VSTS、または Bitbucket から直接には、アプリ中心で、Xamarin.iOS および Xamarin.Android のソリューションを構築します。

### <a name="using-teamcity-with-xamarintoolsciteamcitymd"></a>[Xamarin での TeamCity の使用](~/tools/ci/teamcity.md)

このガイドを使って、TeamCity するモバイル アプリをコンパイルし、アプリ中心のテストに送信するには必要な手順について説明します。

### <a name="using-jenkins-with-xamarintoolscijenkins-walkthroughmd"></a>[Xamarin での Jenkins の使用](~/tools/ci/jenkins-walkthrough.md)

このガイドでは、Jenkins 継続的インテグレーション サーバーとして設定して、Xamarin で作成されたモバイル アプリのコンパイルを自動化する方法を示します。 これには、OS X 上の Jenkins のインストール、構成、および変更がバージョン管理システムにコミットされたときに、Xamarin.iOS および Xamarin.Android アプリをコンパイルするジョブをセットアップする方法について説明します。
