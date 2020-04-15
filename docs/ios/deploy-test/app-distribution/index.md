---
title: Xamarin.iOS アプリの配布の概要
description: ここでは、Xamarin.iOS アプリケーションで使用できる配布手法の概要について説明します。また、このトピックよりも詳細なドキュメントについても紹介します。
ms.prod: xamarin
ms.assetid: 341D36DB-BB07-FA94-BCC9-5F8C0B18C179
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 20126849027f735e9ecd3599c290b4e7a57f837e
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "75886581"
---
# <a name="xamarinios-app-distribution-overview"></a>Xamarin.iOS アプリの配布の概要

_ここでは、Xamarin.iOS アプリケーションで使用できる配布手法の概要について説明します。また、このトピックよりも詳細なドキュメントについても紹介します。_

Xamarin.iOS アプリを開発したら、ソフトウェア開発ライフサイクルの次の手順は、下図で強調表示されているセクションのように、ユーザーにアプリを配布することです。

[![iOS アプリを開発したら、次の手順は下図で強調表示されているセクションのように、ユーザーにアプリを配布することです](images/publishingdiagram.png)](images/publishingdiagram.png#lightbox)

Apple では、iOS アプリケーションを配布するために次の方法が用意されています。

- [**App Store**](#app-store-distribution)
- [**社内 (エンタープライズ)** ](#in-house-distribution)
- [**アドホック**](#ad-hoc-distribution)
- [**ビジネス向けカスタム アプリ**](#custom-apps-for-business)

これらいずれのシナリオでも、適切な*プロビジョニング プロファイル*を使用してアプリケーションをプロビジョニングする必要があります。 プロビジョニング プロファイルは、コード署名情報だけでなく、アプリケーションの ID と使用する配布メカニズムも含むファイルです。 App Store 以外の配布には、アプリを展開できるデバイスに関する情報も含まれています。

## <a name="app-store-distribution"></a>App Store の配布

> [!IMPORTANT]
> Apple は、2019 年 3 月以降に App Store に提出されるすべてのアプリおよび更新プログラムが iOS 12.1 SDK (Xcode 10.1 以降に含まれている) でビルドされる必要があることを[通知しました](https://developer.apple.com/ios/submit/)。
> アプリでは、iPhone XS および 12.9 インチ iPad Pro の画面サイズもサポートされる必要もあります。

iOS デバイスのユーザーに iOS アプリケーションを配布するには、これが主な方法です。 App Store に提出されたすべてのアプリは、Apple の承認を受ける必要があります。

アプリを App Store に提出するには、*iTunes Connect* というポータルを使用します。 このポータルをセットアップし、使用して、App Store で Xamarin.iOS アプリを発行する準備をするには、「[Configure your App in iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)」(iTunes Connect でのアプリの構成) ガイドを参照してください。

**Apple Developer Program** に属する開発者のみが iTunes Connect にアクセスすることができる点に注意してください。 **Apple Developer Enterprise Program** のメンバーはアクセスできません。

詳細については、「[App Store Distribution](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)」(App Store の配布) ガイドを参照してください。

## <a name="in-house-distribution"></a>社内配布

社内配布は、*エンタープライズ配布*とも呼ばれます。社内配布の場合、**Apple Developer Enterprise Program** のメンバーは、同じ組織内の他のメンバーにアプリを配布できます。 社内配布には、App Store のレビューを必要としないという利点があります。また、アプリケーションをインストールできるデバイス数に制限がありません。 ただし、**Apple Developer Enterprise Program** メンバーは iTunes Connect に**アクセスできない**ため、ライセンシーがアプリを配布する必要があります。

社内でアプリケーションをセットアップし、配布する方法については、[社内配布ガイド](~/ios/deploy-test/app-distribution/in-house-distribution.md)のページを参照してください。

## <a name="ad-hoc-distribution"></a>アドホック配布

Xamarin.iOS アプリケーションは、アドホック配布を使用してユーザーテストを実行できます。アドホック配布は、**Apple Developer Program** と **Apple Developer Enterprise Program** の両方で使用できます。また、最大 100 台の iOS デバイスをテストできます。 アドホック配布の最適なユース ケースは、iTunes Connect が選択肢にない社内での配布です。

社内でアプリケーションをセットアップし、配布する方法については、[アドホック配布ガイド](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)のページを参照してください。

## <a name="custom-apps-for-business"></a>ビジネス向けカスタム アプリ

Apple では、アプリの[カスタム配布](https://developer.apple.com/business/custom-apps/)を企業や教育機関に許可しています。 詳細については、「[Apple Business Manager ユーザガイド](https://support.apple.com/guide/apple-business-manager/welcome/web)」を参照してください。

## <a name="related-links"></a>関連リンク

- [App Store の配布](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [iTunes Connect でのアプリの構成](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [App Store への発行](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [社内配布](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [アドホック配布](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [iTunesMetadata.plist ファイル](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [IPA のサポート](~/ios/deploy-test/app-distribution/ipa-support.md)
- [トラブルシューティング](~/ios/deploy-test/troubleshooting.md)
