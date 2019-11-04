---
title: Mac App Store へのアップロード
description: このドキュメントでは、iTunes Connect を使用し、Mac App Store に Xamarin.Mac アプリをアップロードする方法について説明します。 プロセスを完了する目的で iTunes Connect から要求される情報について説明します。
ms.prod: xamarin
ms.assetid: 30cd0e47-1b2e-47ef-93f6-4bed20b15c03
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: fede666071aad8702eda17a94a06c7de49c7397e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029475"
---
# <a name="upload-to-mac-app-store"></a>Mac App Store へのアップロード

_このガイドでは、Mac App Store に Xamarin.Mac アプリを発行するためのアップロード手順について説明します。_

承認時、アプリケーションは、[iTunes Connect](https://itunesconnect.apple.com/) から Mac App Store に送信されます。

1. 作成する **macOS アプリ**を選択します。 

    [![](uploading-images/image65.png "iTunes Connect")](uploading-images/image65.png#lightbox)

2. アプリケーション名とその他の詳細を入力します。 開発者は、既に作成されている既存の **Bundle ID** からのみ選択できます。 

    [![](uploading-images/image66.png "Selecting the bundle ID")](uploading-images/image66.png#lightbox)

3. 提供開始日と価格を選択します。 アプリは、開発者が選択した提供開始日に関係なく、承認後にのみ販売となります。 開発者が、実際の提供開始日をより制御したい場合、この値はかなり先に設定します。 

    [![](uploading-images/image67.png "Setting the available date and price")](uploading-images/image67.png#lightbox)

4. 属する App Store のカテゴリなど、アプリの情報を入力します。 

    [![](uploading-images/image68.png "Entering the app information")](uploading-images/image68.png#lightbox) 

    該当するレーティングを選択します。 

    [![](uploading-images/image69.png "Setting the app ratings")](uploading-images/image69.png#lightbox) 

    説明、キーワード、連絡先 URL を入力します。 

    [![](uploading-images/image70.png "Editing the Description, keywords and contact URLs")](uploading-images/image70.png#lightbox) 

    App Store のレビュー担当者への連絡先情報とアドバイスを入力します。 

    [![](uploading-images/image71.png "Editing the contact information and advice for the App Store reviewers")](uploading-images/image71.png#lightbox) 

    最後にはスクリーンショットです。 

    [![](uploading-images/image72.png "Adding the required screenshots")](uploading-images/image72.png#lightbox) 

    スクリーンショットは、ピクセル サイズ 1280 x 800、1440 x 900、2880 x 1800 または 2560 x 1600 の JPG、TIF または PNG 形式にする必要があります。 **[Save]\(保存\)** を押して完了します。

5. 確認のためにアプリの情報が表示されます。 **[View Details]\(詳細を表示\)** をクリックして状態を変更します。 

    [![](uploading-images/image73.png "Viewing the app details")](uploading-images/image73.png#lightbox)

6. 詳細ビューで、[Ready to Upload Binary]\(バイナリのアップロードの準備ができました\) をクリックして、アプリケーション パッケージ ファイルを送信します。 

    [![](uploading-images/image74.png "Selecting Ready to Upload Binary")](uploading-images/image74.png#lightbox)

7. 暗号化に関する質問に答えます。 

    [![](uploading-images/image75.png "Answering the cryptography question")](uploading-images/image75.png#lightbox)

8. アプリケーション パッケージ ファイルを受け入れる準備ができたことが、サイトに表示されます。 

    [![](uploading-images/image76.png "The acceptance notification")](uploading-images/image76.png#lightbox)

9. Application Loader を起動し、Apple ID でログインしていることを確認します。
**[Deliver Your App]\(アプリの配信\)** を選択し、続行します。 

    [![](uploading-images/image77.png "The Application Loader interface")](uploading-images/image77.png#lightbox)

10. **[Ready to Upload Binary]\(バイナリのアップロードの準備ができました\)** の状態のアプリケーション一覧から選択し、 **[Next]\(次へ\)** をクリックします。 

    [![](uploading-images/image78.png "Selecting the app to load")](uploading-images/image78.png#lightbox)

11. アプリケーションのメタデータを確認し、 **[Choose...]\(選択\)** をクリックして、パッケージ ファイルを検索します。 

    [![](uploading-images/image79.png "Reviewing the app metadata")](uploading-images/image79.png#lightbox)

12. App Store のビルド構成を使用して Visual Studio for Mac でビルドされたパッケージ ファイルを探します。 

    [![](uploading-images/image80.png "Selecting the file to upload")](uploading-images/image80.png#lightbox)

13. **[送信]** を押します。 

    [![](uploading-images/image81.png "Sending the app")](uploading-images/image81.png#lightbox)

14. パッケージが検証され、すべてのエラーが報告されます。 これらのエラーを修正し、再度アップロードします。 アップロードが正常に完了すると、App Store チームが検証するためにアプリは自動送信されます。 

    [![](uploading-images/image82.png "An example of upload errors")](uploading-images/image82.png#lightbox)

承認されると、アプリケーションはダウンロード可能になるか、Mac App Store から購入可能となります。

## <a name="related-links"></a>関連リンク

- [インストール](~//mac/get-started/installation.md)
- [Hello Mac のサンプル](~//mac/get-started/hello-mac.md)
- [Mac App Store でアプリを配布する](https://developer.apple.com/devcenter/mac/checklist/)
- [ツール ガイド: アプリのコード署名](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [Developer ID と GateKeeper](https://developer.apple.com/resources/developer-id/)
