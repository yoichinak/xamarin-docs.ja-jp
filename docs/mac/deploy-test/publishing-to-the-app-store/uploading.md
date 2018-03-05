---
title: "Mac App Store へのアップロード"
description: "このガイドでは、Mac App Store に Xamarin.Mac アプリを発行するためのアップロード手順について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 30cd0e47-1b2e-47ef-93f6-4bed20b15c03
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 533031016af68b1eb95a198e28ac40f445ff1579
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="upload-to-mac-app-store"></a>Mac App Store へのアップロード

_このガイドでは、Mac App Store に Xamarin.Mac アプリを発行するためのアップロード手順について説明します。_

承認時、アプリケーションは、[iTunes Connect](http://itunesconnect.apple.com/) から Mac App Store に送信されます。

1. 作成する **macOS アプリ**を選択します。 

    [ ![](uploading-images/image65.png "iTunes Connect")](uploading-images/image65.png)

2. アプリケーション名とその他の詳細を入力します。 開発者は、既に作成されている既存の **Bundle ID** からのみ選択できます。 

    [ ![](uploading-images/image66.png "バンドル ID の選択")](uploading-images/image66.png)

3. 提供開始日と価格を選択します。 アプリは、開発者が選択した提供開始日に関係なく、承認後にのみ販売となります。 開発者が、実際の提供開始日をより制御したい場合、この値はかなり先に設定します。 

    [ ![](uploading-images/image67.png "提供開始日と価格の設定")](uploading-images/image67.png)

4. 属する App Store のカテゴリなど、アプリの情報を入力します。 

    [ ![](uploading-images/image68.png "アプリ情報の入力")](uploading-images/image68.png) 

    該当するレーティングを選択します。 

    [ ![](uploading-images/image69.png "アプリのレーティングの設定")](uploading-images/image69.png) 

    説明、キーワード、連絡先 URL を入力します。 

    [ ![](uploading-images/image70.png "説明、キーワード、連絡先 URL の入力")](uploading-images/image70.png) 

    App Store のレビュー担当者への連絡先情報とアドバイスを入力します。 

    [ ![](uploading-images/image71.png "App Store のレビュー担当者への連絡先情報とアドバイスの編集")](uploading-images/image71.png) 

    最後にはスクリーンショットです。 

    [ ![](uploading-images/image72.png "必要なスクリーンショットの追加")](uploading-images/image72.png) 

    スクリーンショットは、ピクセル サイズ 1280 x 800、1440 x 900、2880 x 1800 または 2560 x 1600 の JPG、TIF または PNG 形式にする必要があります。 **[Save]\(保存\)** を押して完了します。

5. 確認のためにアプリの情報が表示されます。 **[View Details]\(詳細を表示\)** をクリックして状態を変更します。 

    [ ![](uploading-images/image73.png "アプリの詳細の表示")](uploading-images/image73.png)

6. 詳細ビューで、[Ready to Upload Binary]\(バイナリのアップロードの準備ができました\) をクリックして、アプリケーション パッケージ ファイルを送信します。 

    [ ![](uploading-images/image74.png "バイナリのアップロードの準備の選択")](uploading-images/image74.png)

7. 暗号化に関する質問に答えます。 

    [ ![](uploading-images/image75.png "暗号化に関する質問に答える")](uploading-images/image75.png)

8. アプリケーション パッケージ ファイルを受け入れる準備ができたことが、サイトに表示されます。 

    [ ![](uploading-images/image76.png "受け入れの通知")](uploading-images/image76.png)

9. Application Loader を起動し、Apple ID でログインしていることを確認します。
**[Deliver Your App]\(アプリの配信\)** を選択し、続行します。 

    [ ![](uploading-images/image77.png "Application Loader のインターフェイス")](uploading-images/image77.png)

10. **[Ready to Upload Binary]\(バイナリのアップロードの準備ができました\)** の状態のアプリケーション一覧から選択し、**[Next]\(次へ\)** をクリックします。 

    [ ![](uploading-images/image78.png "読み込むアプリの選択")](uploading-images/image78.png)

11. アプリケーションのメタデータを確認し、**[Choose...]\(選択\)** をクリックして、パッケージ ファイルを検索します。 

    [ ![](uploading-images/image79.png "アプリのメタデータの確認")](uploading-images/image79.png)

12. App Store のビルド構成を使用して Visual Studio for Mac でビルドされたパッケージ ファイルを探します。 

    [ ![](uploading-images/image80.png "アップロードするファイルの選択")](uploading-images/image80.png)

13. **[送信]** を押します。 

    [ ![](uploading-images/image81.png "アプリの送信")](uploading-images/image81.png)

14. パッケージが検証され、すべてのエラーが報告されます。 これらのエラーを修正し、再度アップロードします。 アップロードが正常に完了すると、App Store チームが検証するためにアプリは自動送信されます。 

    [ ![](uploading-images/image82.png "アップロード エラーの例")](uploading-images/image82.png)

承認されると、アプリケーションはダウンロード可能になるか、Mac App Store から購入可能となります。

## <a name="related-links"></a>関連リンク

- [インストール](~//mac/get-started/installation.md)
- [Hello Mac のサンプル](~//mac/get-started/hello-mac.md)
- [Mac App Store でアプリを配布する](https://developer.apple.com/devcenter/mac/checklist/)
- [ツール ガイド: アプリのコード署名](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [Developer ID と GateKeeper](https://developer.apple.com/resources/developer-id/)
