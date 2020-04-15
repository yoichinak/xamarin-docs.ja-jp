---
title: Mac App Store 用のバンドル
description: このドキュメントでは、Mac App Store に公開するために Xamarin.Mac アプリをバンドルする方法について説明します。 コード署名オプションとビルドについて説明します。
ms.prod: xamarin
ms.assetid: 00a36d7c-937d-4657-bf6a-0de9684b8f94
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: f4d38bb66a34257c1e0a27c5fbbfe16f59743e83
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "76725507"
---
# <a name="bundling-for-the-mac-app-store"></a>Mac App Store 用のバンドル

このセクションでは、Visual Studio for Mac を使用して、Mac App Store でリリースするためのアプリケーションを構築する基礎について説明します。 iCloud アクセスやプッシュ通知などの追加の機能によっては、この記事で扱っていない追加のセットアップが必要になる場合があります。

> [!NOTE]
> このセクションの内容を開始する前に、開発者が、Mac App Store 用にビルドするための Production プロビジョニング プロファイルを作成している必要があります。 必要なプロビジョニング プロファイルの作成については、[プロファイルの手順](profiles.md)に関する記事をご覧ください。

## <a name="code-signing-options"></a>コード署名のオプション

コードの署名とパッケージ化のオプションを更新する前に、 **[構成]** を **[リリース]** に変更してください。 開発者は、App Store でのリリース用にアプリケーションに署名するときに、前に作成した **ID** とプロビジョニング ファイルを会社が使用していることを確認する必要があります。

[![コード署名のオプションの編集](bundling-images/sign.png)](bundling-images/sign-large.png#lightbox)

**[Mac ビルド]** の設定で、インストーラー パッケージを作成するオプションがオンになっていることを確認します。

[![ビルド オプションの編集](bundling-images/build.png "ビルド オプションの編集")](bundling-images/build-large.png#lightbox)

## <a name="build"></a>ビルド

ビルドの前に、 **[リリース]** 構成が選択されていることを確認します。 開発者がアプリをビルドすると、プロンプトが "_2 回_" 表示されます (アプリケーションとインストーラーの両方の証明書を使用するため)。

![アプリによる証明書の使用の許可は、2 回表示されます](bundling-images/perms02.png)

アプリケーションがビルドされると、開発者はプロジェクトを右クリックし、 **[Finder で表示]** を選択して、パッケージ ファイルを (下に示す例の `bin/Release/AppStore` ディレクトリから) 検索することができます。  このパッケージ ファイルには、Mac App Store で含めるように Apple に送信できるアプリのインストーラーが含まれています。

> [!div class="mx-imgBorder"]
> ![Finder でのビルド パッケージの選択](bundling-images/path.png)

## <a name="related-links"></a>関連リンク

- [インストール](/visualstudio/mac/installation/)
- [Hello Mac のサンプル](~/mac/get-started/hello-mac.md)
- [Mac App Store でアプリを配布する](https://developer.apple.com/devcenter/mac/checklist/)
- [Developer ID と GateKeeper](https://developer.apple.com/developer-id/)
