---
title: fastlane for iOS – cert
description: このドキュメントでは fastlane について説明します。このツールでは、証明書の要求、Apple の Developer Portal へのデバイスの追加、App ID の作成など、iOS アプリケーション プロビジョニング プロセスの多くの部分を自動化します。
ms.prod: xamarin
ms.assetid: 900FA6FF-F3C9-4D35-993E-B0D88E6B1883
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: ba0348ff0cf6dc394f67b3c5779fd49eb852673f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028524"
---
# <a name="fastlane-for-ios--cert"></a>fastlane for iOS – cert

> [!IMPORTANT]
> fastlane では、証明書の作成および維持に [`match`](~/ios/deploy-test/provisioning/fastlane/match.md) ツールの使用を推奨しています。 フル コントロールが必要で、コード署名について十分に理解している場合にのみ、`cert` を直接使用します。 最新のコード署名 ID をダウンロードするには、このアクションを使用します。

## <a name="overview"></a>概要

従来から、デバイスのプロビジョニングは、Xcode または Apple Developer ポータルを通じて、開発チームの各メンバーによって実行されています。 これは次の複数の手順で構成されています。

- 開発証明書を要求する
- デバイスをポータルに追加する
- アプリ ID の作成
- プロビジョニング プロファイルを作成する
- プロファイルと証明書をダウンロードする

これらの各手順には、開発中のアプリケーションの種類に応じて対処する必要がある変数が含まれています。 手動でまたは Xcode を介して開発用にデバイスをセットアップするために必要な手順の詳細については、「[デバイスのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)」ガイドを参照してください。

このガイドでは、Xcode の代替としての fastlane ツールを紹介し、次について説明します。

- [cert とは](#whatiscert)
- [cert の使用](#using)
- [その他のオプション](#options)

## <a name="installation"></a>インストール

fastlane のインストールと更新の詳細については、[fastlane](~/ios/deploy-test/provisioning/fastlane/index.md#Installation) の概要に関するガイドを参照してください。

<a name="whatiscert" />

## <a name="what-is-cert"></a>cert とは

cert は、開発環境と配布環境の両方に、新しいコード署名 ID (開発者_証明書_とも呼ばれます) を作成するターミナル インターフェイスを提供します。

<a name="using" />

## <a name="using-cert"></a>cert の使用

cert ユーティリティを使用するには、ターミナルの CLI に次のコマンドを入力します。

```
fastlane cert
```

既定では、これにより配布の証明書が作成されます。 開発の証明書を作成するには、`--development` フラグを渡します。

```
fastlane cert --development
```

cert により Apple ID とパスワードの入力が求められるので、ここで入力します。

[![](cert-images/fastlane-image1.png "cert will prompt for your Apple ID and password")](cert-images/fastlane-image1.png#lightbox)

> [!IMPORTANT]
> 初めてパスワードを入力すると、パスワードがローカル macOS キーチェーンに保存されます。 または、環境変数を使用してユーザー名とパスワードを格納することも、パスワードをキーチェーンに保存したくない場合は、`export fastlane_DONT_STORE_PASSWORD=1` を使用することもできます。 fastlane で資格情報を管理する詳細については、fastlane の[資格情報マネージャー ガイド](https://github.com/fastlane/fastlane/blob/master/credentials_manager/README.md)を参照してください。

次のコマンドを使用して、Apple ID を引数として渡すこともできます。

```
fastlane cert -u myemailadress@domain.com
```

Apple ID が複数のチームに接続されている場合は、ここに表示されます。 使用するチームに対応する番号を選択します。

[![](cert-images/fastlane-image2.png "Select the team that you wish to use")](cert-images/fastlane-image2.png#lightbox)

次のフラグを使用してチーム ID を渡すこともできます。

```
fastlane cert -l 2TU993NY9J
```

fastlane は使用可能な署名証明書がローカル コンピューターにインストールされているかどうかを確認し、されている場合はそれを使用します。

署名証明書がない場合は、cert は次のことを行います。

- 新しい秘密キーと署名要求を作成する
- 証明書を生成、ダウンロード、およびインストールする
- 証明書と秘密キーをキーチェーンにインポートする

アカウントに許可されている署名 ID の最大数に達している場合は、例外が生成されます。 新しい署名 ID を作成する場合は、Developer Center を通じて既存の証明書のいずれかを手動で失効させ、もう一度実行する必要があります。

> [!NOTE]
> 署名 ID が自分のキーチェーンにまだない場合は、fastlane は Developer Center から既存の署名 ID をダウンロードできません。 これは、秘密キーが自分のコンピューター、または証明書の exported(*.p12) バージョンにしか存在せず、Developer Center に存在しないためです。

<a name="options" />

## <a name="additional-options"></a>その他のオプション

cert を使用する際に、次のオプションを使用して追加のサポートを提供できます。

- 使用可能なすべてのコマンドのリストに `-–help` フラグを使用します。

    ```
    fastlane cert --help
    ```

- 出力の詳細レベルを上げるには、`-–verbose` フラグを使用します。

    ```
    fastlane cert --development --verbose
    ```

## <a name="related-links"></a>関連リンク

- [fastlane – cert](https://github.com/fastlane/fastlane/blob/master/cert/README.md)
