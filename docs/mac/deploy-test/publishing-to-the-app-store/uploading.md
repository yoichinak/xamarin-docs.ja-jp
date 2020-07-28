---
title: Mac App Store へのアップロード
description: このドキュメントでは、iTunes Connect を使用し、Mac App Store に Xamarin.Mac アプリをアップロードする方法について説明します。 プロセスを完了する目的で iTunes Connect から要求される情報について説明します。
ms.prod: xamarin
ms.assetid: 30cd0e47-1b2e-47ef-93f6-4bed20b15c03
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 57591a6e9fcaa3c8271ab27756160ee1c46a4fa3
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86935929"
---
# <a name="upload-to-mac-app-store"></a>Mac App Store へのアップロード

_このガイドでは、Mac App Store に Xamarin.Mac アプリを発行するためのアップロード手順について説明します。_

承認時、アプリケーションは、[iTunes Connect](https://itunesconnect.apple.com/) から Mac App Store に送信されます。 さらに、App Store から [**Transporter**](https://apps.apple.com/us/app/transporter/id1450874784?mt=12) ツールを入手する必要もあります。

1. 作成する **macOS アプリ**を選択します。

    [![iTunes Connect](uploading-images/image65.png)](uploading-images/image65.png#lightbox)

2. アプリケーション名とその他の詳細を入力します。 開発者は、既に作成されている既存の **Bundle ID** からのみ選択できます。

    [![バンドル ID の選択](uploading-images/image66.png)](uploading-images/image66.png#lightbox)

3. 提供開始日と価格を選択します。 アプリは、開発者が選択した提供開始日に関係なく、承認後にのみ販売となります。 開発者が、実際の提供開始日をより制御したい場合、この値はかなり先に設定します。

    [![提供開始日と価格の設定](uploading-images/image67.png)](uploading-images/image67.png#lightbox)

4. 属する App Store のカテゴリなど、アプリの情報を入力します。

    [![アプリ情報の入力](uploading-images/image68.png)](uploading-images/image68.png#lightbox)

    該当するレーティングを選択します。

    [![アプリのレーティングの設定](uploading-images/image69.png)](uploading-images/image69.png#lightbox)

    説明、キーワード、連絡先 URL を入力します。

    [![説明、キーワード、連絡先 URL の入力](uploading-images/image70.png)](uploading-images/image70.png#lightbox)

    App Store のレビュー担当者への連絡先情報とアドバイスを入力します。

    [![App Store のレビュー担当者への連絡先情報とアドバイスの編集](uploading-images/image71.png)](uploading-images/image71.png#lightbox)

    最後にはスクリーンショットです。

    [![必要なスクリーンショットの追加](uploading-images/image72.png)](uploading-images/image72.png#lightbox)

    スクリーンショットは、ピクセル サイズ 1280 x 800、1440 x 900、2880 x 1800 または 2560 x 1600 の JPG、TIF または PNG 形式にする必要があります。 **[Save]\(保存\)** を押して完了します。

5. 確認のためにアプリの情報が表示されます。 **[View Details]\(詳細を表示\)** をクリックして状態を変更します。

    [![アプリの詳細の表示](uploading-images/image73.png)](uploading-images/image73.png#lightbox)

6. 詳細ビューで、[Ready to Upload Binary]\(バイナリのアップロードの準備ができました\) をクリックして、アプリケーション パッケージ ファイルを送信します。

    [![バイナリのアップロードの準備の選択](uploading-images/image74.png)](uploading-images/image74.png#lightbox)

7. 暗号化に関する質問に答えます。

    [![暗号化に関する質問に答える](uploading-images/image75.png)](uploading-images/image75.png#lightbox)

8. アプリケーション パッケージ ファイルを受け入れる準備ができたことが、サイトに表示されます。

    [![受け入れの通知](uploading-images/image76.png)](uploading-images/image76.png#lightbox)

9. **Transporter** を開始し、Apple ID でログインした後、 **[アプリの追加]** を選択します。

    [![Application Loader のインターフェイス](uploading-images/transporter01-sml.png)](uploading-images/transporter01.png#lightbox)

    手順に従ってアプリ パッケージを iTunes Connect にアップロードします。

    > [!NOTE]
    > [**Transporter**](https://apps.apple.com/us/app/transporter/id1450874784?mt=12) は、Xcode 10 以前で使用されていた**Application Loader** ツールに代わるものです。
    > Application Loader は、Xcode 11 以降では使用できなくなります。

承認されると、アプリケーションはダウンロード可能になるか、Mac App Store から購入可能となります。

## <a name="related-links"></a>関連リンク

- [インストール](~//mac/get-started/installation.md)
- [Hello Mac のサンプル](~/mac/get-started/hello-mac.md)
- [Mac App Store でアプリを配布する](https://developer.apple.com/devcenter/mac/checklist/)
- [ツール ガイド: アプリのコード署名](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [Developer ID と GateKeeper](https://developer.apple.com/developer-id/)
