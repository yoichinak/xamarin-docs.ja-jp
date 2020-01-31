---
title: 開発者 ID で Xamarin.Mac アプリに署名する
description: このドキュメントでは、Mac App Store の外で配布できるように、開発者 ID で Xamarin.Mac アプリに署名する方法について説明します。 コード署名オプションとビルドについて説明します。
ms.prod: xamarin
ms.assetid: cf7b733b-e08f-4f56-a233-264b29ee4c97
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: f05e763df58e6fafc4d4ed1d4007979859670c28
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725463"
---
# <a name="signing-xamarinmac-apps-with-a-developer-id"></a>開発者 ID で Xamarin.Mac アプリに署名する

開発者が macOS ユーザーに直接アプリを配布する計画をしている場合、**GateKeeper** を有効にして macOS システムにインストールできるよう、Developer ID を使用してコード署名することを Apple では推奨しています。 アプリが署名されていない場合、**GateKeeper** によって警告メッセージが表示され、ユーザーはインストールできなくなります (この制限は、起動時に Ctrl キーを押下することによって回避できます)。

Apple の Web サイトで、「[Developer ID and Gatekeeper](https://developer.apple.com/developer-id/)」 (Developer ID と Gatekeeper) と「[Distributing Outside the Mac App Store](https://developer.apple.com/library/content/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)」 (Mac App Store 外での配布) の詳細を参照してください。

## <a name="code-signing-options"></a>コード署名のオプション

アプリを、(Mac App Store を介してではなく) ユーザーに直接配布するよう構築する場合、 **[Signing Settings]\(署名の設定\)** を設定して **[Developer ID]** を使用します。 **[Release]\(リリース\)** 構成は必ず編集します。

 [![](signing-images/config02.png "The Mac Signing options")](signing-images/config02.png#lightbox)

## <a name="build"></a>ビルド

ビルド前に、正しい構成が選択されていることを確認し、 **[Mac Build]\(Mac ビルド\)** 設定にインストール パッケージを作成するよう選択します。

[![](signing-images/config03.png "The build options")](signing-images/config03.png#lightbox)

アプリを構築する際、開発者は両方の証明書を使用するよう求められます。

 [![](signing-images/image57.png "Allowing keychain access")](signing-images/image57.png#lightbox)

 [![](signing-images/image58.png "Allowing keychain access")](signing-images/image58.png#lightbox)

アプリケーションがビルドされると、開発者はプロジェクトを右クリックし、 **[Open Containing Folder]\(含まれているフォルダーを開く\)** を選択して、パッケージ ファイルを (`bin/Release` ディレクトリから) 検索することができます。 このパッケージ ファイルには、アプリケーションのインストーラーが含まれているので、任意の macOS ユーザーにインストール用に配布することができます。

 [![](signing-images/image59.png "Selecting the app package in Finder")](signing-images/image59.png#lightbox)

## <a name="related-links"></a>関連リンク

- [インストール](~//mac/get-started/installation.md)
- [Hello Mac のサンプル](~//mac/get-started/hello-mac.md)
- [Mac App Store でアプリを配布する](https://developer.apple.com/devcenter/mac/checklist/)
- [ツール ガイド: アプリのコード署名](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [Developer ID と GateKeeper](https://developer.apple.com/developer-id/)
