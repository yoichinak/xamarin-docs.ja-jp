---
title: "Mac App Store 用のバンドル"
description: "このガイドでは、Mac App Store に Xamarin.Mac アプリを発行するためのバンドルの手順について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 00a36d7c-937d-4657-bf6a-0de9684b8f94
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 77365d5ed62b2ef2e81407ab1fa5aef55c592d0b
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="bundle-for-mac-app-store"></a>Mac App Store 用のバンドル

このセクションでは、Visual Studio for Mac を使用して、Mac App Store でリリースするためのアプリケーションを構築する基礎について説明します。 iCloud アクセスやプッシュ通知などの追加の機能によっては、この記事で扱っていない追加のセットアップが必要になる場合があります。

> [!NOTE]
>  **注**: このセクションの内容を開始する前に、開発者が、Mac App Store 用にビルドするための Production プロビジョニング プロファイルを作成している必要があります。 必要なプロビジョニング プロファイルの作成方法については、このドキュメントの前の手順を参照してください。

## <a name="code-signing-options"></a>コード署名のオプション

コードの署名とパッケージ化のオプションを更新する前に、**[構成]** を **[リリース]** に変更してください。 開発者は、App Store でのリリース用にアプリケーションに署名するときに、前に作成した **ID** とプロビジョニング ファイルを会社が使用していることを確認する必要があります。

 [![コード署名オプションの編集](bundling-images/config02.png "コード署名オプションの編集")](bundling-images/config02-large.png#lightbox)

**[Mac ビルド]** の設定で、インストーラー パッケージを作成するオプションがオンになっていることを確認します。

[![ビルド オプションの編集](bundling-images/config03.png "ビルド オプションの編集")](bundling-images/config03-large.png#lightbox)

## <a name="build"></a>ビルド

ビルドの前に、**[リリース]** 構成が選択されていることを確認します。 開発者がアプリをビルドする際、両方の証明書を使用するよう求められます。

 ![アプリによる証明書の使用の許可](bundling-images/image62.png "アプリによる証明書の使用の許可")

 ![アプリによる証明書の使用の許可](bundling-images/image63.png "アプリによる証明書の使用の許可")

アプリケーションがビルドされると、開発者はプロジェクトを右クリックし、**[Open Containing Folder]\(含まれているフォルダーを開く\)** を選択して、パッケージ ファイルを (下に示す例の `bin/x86/AppStore` ディレクトリから) 検索することができます。  このパッケージ ファイルには、Mac App Store で含めるように Apple に送信できるアプリのインストーラーが含まれています。

 ![Finder でのビルド パッケージの選択](bundling-images/image64.png "Finder でのビルド パッケージの選択")


## <a name="related-links"></a>関連リンク

- [インストール](/visualstudio/mac/installation/)
- [Hello Mac のサンプル](~/mac/get-started/hello-mac.md)
- [Mac App Store でアプリを配布する](https://developer.apple.com/devcenter/mac/checklist/)
- [Developer ID と GateKeeper](https://developer.apple.com/resources/developer-id/)
