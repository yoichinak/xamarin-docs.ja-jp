---
title: Xamarin.iOS のテストと展開 - トラブルシューティング
description: このドキュメントでは、コード署名とプロビジョニング、TestFlight、Mac ビルド ホストから Windows への iOS アプリ バンドルのコピーに関連するトラブルシューティングのヒントを示します。
ms.prod: xamarin
ms.assetid: 65286D09-F74D-4F22-B6CD-D1BCD7FC7992
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 08/23/2017
ms.openlocfilehash: b6fbe8ca975100310922240e532b9922e76e4724
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290671"
---
# <a name="xamarinios-testing-and-deployment---troubleshooting"></a>Xamarin.iOS のテストと展開 - トラブルシューティング

## <a name="code-signing--provisioning"></a>コード署名とプロビジョニング

iOS ではコード署名とプロビジョニングは非常に厄介な場合があるため、コード署名証明書とプロビジョニング プロファイルが整理されていることを確認することが重要です。

- 大規模なチームは、Xcode で [Fix issue]\(問題の修正\) ボタン (下図) の使用は控える必要があります。

    [![](troubleshooting-images/fixissue.png "[Fix Issues] ダイアログ")](troubleshooting-images/fixissue.png#lightbox)

    これにより新しいプロビジョニング プロファイルと証明書が作成されます。 最良の場合でも、チーム メンバーがボタンをクリックするたびに、プロビジョニング プロファイルが作成され、プロファイルによる混乱が発生します。 最悪の場合、社内の他の人全員の証明書が無効になり、アプリの停止を引き起こします。

- Keychain Access は整理された状態を保ち、有効期限が切れた証明書とプロファイルを削除します。 Enterprise 証明書は 3 年間有効で、他の証明書は 1 年間有効です。 証明書は更新できないため、古い証明書が期限切れになる直前に新しい証明書を作成する必要があります。 古い証明書を失効させて削除し、新しい証明書でアプリを再署名します。

- 新しいプロビジョニング プロファイルがインストールされたら、古いプロビジョニング プロファイルを削除します。 これは Visual Studio for Mac が使用するプロファイルを決定しなければならない立場にないことを意味します。 これを実現するには、最初に、Apple Developer Center でプロファイルを削除してから、 *[Preferences]\(環境設定\) > [Your Account]\(アカウント\) > [View Details...]\(詳細の表示\)* の順に移動します。プロビジョニング プロファイルを選択し、 **[Show in Finder]\(Finder で表示\)** をクリックします。 Mac ファイル システム内のプロファイルの場所が表示されます。ここで、Finder を使用してプロファイルを削除できます。

- 必要なすべての証明書および対応する秘密キーが使用できることを確認します。 チームごとに開発者の証明書 (独自のデバイスにアプリをインストールするため) と配布証明書 (その他のデバイスにインストールするため) が必要になります。

- 新しいプロビジョニング プロファイルまたは証明書をインストールする場合は、Xcode と、Visual Studio for Mac または Visual Studio を再起動します。

## <a name="testflight"></a>TestFlight

ときには、テストが計画どおりにスムーズに行かない場合があります。  次の手順は、TestFlight での問題の解決に役立つ場合があります。

- TestFlight は iOS 8 以降を対象としたアプリにのみ使用できます。

- ベータ版の権利を持つ *App Store の配布プロファイル*がある必要があります。

- **[New iOS App submission]\(新規 iOS アプリの提出\)** ウィンドウには、アプリの **Info.plist** とまったく同じ情報が含まれる必要があり、すべてのセクションが入力されている必要があります。 TestFlight にアップロードする前に、アプリのアイコンを指定する必要があります。

- 新しいビルドをアップロードするときには、iTunes Connect にビルドが表示されるまでに 1 - 5 分程度かかります。

- アプリの各バージョンに対し、[[TestFlight Beta Testing]\(TestFlight のベータ版のテスト\)](~/ios/deploy-test/testflight.md#beta-testing) スイッチをオンにする必要があります。

- 内部のテスト担当者でもある開発者チームの各メンバーには、 **[Internal Tester]\(内部のテスト担当者\)** スイッチがオンになっている必要です。

- 別の iTunes Connect アカウントに所属しているまたはアカウントを所有しているユーザーは、内部のテスト担当者になることはできません。 これらのユーザーは外部テスト担当者として追加することしかできません。

- 内部ユーザーと外部ユーザーは、別々に追加、選択、招待されます。 各リストは個別に管理する必要があります。

- 外部のテスト担当者に配布される各ビルドは、Apple によって承認される必要があります。 ビルドのバージョンが変更された場合は、Apple による新しいベータ版のレビューが必要です。 ビルド番号が変更された場合、レビューは省略できます。

- メタ データは、外部のテスト担当者に配布されるビルドに追加する必要があります。 これには、 **[My Apps]\(マイ App\) > [Prerelease]\(プレリリース\)** でビルド番号をクリックしてアクセスできます。

- 1 日にレビューのために提出できるのは、2 つのビルドだけです。 バージョンの変更にはレビューが必要になるため、バージョン番号は 1 日に 2 回しか変更できないことを意味します。

<a name="Automatically_copy_app_bundles_back_to_Windows" />

## <a name="automatically-copy-app-bundles-back-to-windows"></a>.app バンドルを自動的に Windows にコピーする

[!include[](~/ios/includes/copy-app-bundle-to-windows.md)]
