---
title: Xamarin を使用した継続的インテグレーションの概要
description: このドキュメントは、Xamarin を使用した継続的インテグレーションを記述するためのガイドにリンクしています。 リンクされたコンテンツは、継続的インテグレーションの概要を説明し、App Center Build、TeamCity と Jenkins について説明します。
ms.prod: xamarin
ms.assetid: 99484E96-DC69-4697-8BBB-1B44C5CBB5ED
author: lobrien
ms.author: laobri
ms.date: 10/23/2018
---

# <a name="continuous-integration-with-xamarin"></a>Xamarin を使用した継続的インテグレーション

> [!Video https://youtube.com/embed/wXgnh2Q7Uv8]

## <a name="introduction-to-continuous-integrationtoolsciintro-to-cimd"></a>[継続的な統合の概要](~/tools/ci/intro-to-ci.md)

このセクションでは、継続的インテグレーションとそのリレーションシップに関連するさまざまなコンポーネントについて説明します。 これには、以下の特定のセクションで説明されている継続的インテグレーション環境について説明します。

## <a name="devops-with-xamarintoolscidevopsmd"></a>[Xamarin を使用した DevOps](~/tools/ci/devops.md)

このセクションでは、Azure と Xamarin プロジェクトで機能する Visual Studio での DevOps 機能を識別します。

## <a name="working-with-continuous-integration-environments"></a>継続的インテグレーション環境の使用

### <a name="build-xamarin-apps-with-azure-pipelineshttpsdocsmicrosoftcomazuredevopspipelineslanguagesxamarin"></a>[Azure のパイプラインを使用した Xamarin アプリを構築します。](https://docs.microsoft.com/azure/devops/pipelines/languages/xamarin/)

Android および iOS 用の Xamarin アプリを自動的にビルドするのにには、Azure のパイプラインを使用します。

### <a name="build-xamarin-apps-using-app-centerhttpsdocsmicrosoftcomappcenterbuildxamarin"></a>[App Center を使用して Xamarin アプリを構築します。](https://docs.microsoft.com/appcenter/build/xamarin/)

GitHub、Azure DevOps、または Bitbucket から直接には、App center では、Xamarin.iOS と Xamarin.Android のソリューションを構築します。

### <a name="build-xamarin-apps-with-teamcitytoolsciteamcitymd"></a>[TeamCity による Xamarin アプリを構築します。](~/tools/ci/teamcity.md)

このガイドでは、モバイル アプリをコンパイルし、App Center Test に送信する TeamCity の使用するための手順について説明します。

### <a name="build-xamarin-apps-with-jenkinstoolscijenkins-walkthroughmd"></a>[Jenkins での Xamarin アプリを構築します。](~/tools/ci/jenkins-walkthrough.md)

このガイドでは、Jenkins 継続的インテグレーション サーバーとしてセットアップし、Xamarin で作成したモバイル アプリのコンパイルを自動化する方法を示します。 これには、OS X で Jenkins をインストール、構成、および変更がバージョン コントロール システムにコミットされるときに、Xamarin.iOS および Xamarin.Android アプリをコンパイルするジョブを設定する方法について説明します。
