---
title: fastlane for iOS – sigh
description: このドキュメントでは fastlane の sigh コマンドについて説明します。このコマンドは、すべての Xamarin.iOS ビルド構成用のプロビジョニング プロファイルを作成、更新、および修復するために使用します。
ms.prod: xamarin
ms.assetid: CD17276F-2C8C-4A46-A54C-DD532EBD5720
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/19/2017
ms.openlocfilehash: c9b6f6c29b86ee40c2d7b04dbe6fa4ce24a745ea
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70762626"
---
# <a name="fastlane-for-ios-sigh"></a>fastlane for iOS – sigh

> [!IMPORTANT]
> fastlane では、プロビジョニング プロファイルの作成および維持に [`match`](~/ios/deploy-test/provisioning/fastlane/match.md) の使用を推奨しています。 フル コントロールが必要で、コード署名について十分に理解している場合にのみ、sigh を直接使用します。

## <a name="overview"></a>概要

従来から、デバイスのプロビジョニングは、Xcode または Apple Developer ポータルを通じて、開発チームの各メンバーによって実行されています。 これは次の複数の手順で構成されています。

- 開発証明書を要求する
- デバイスをポータルに追加する
- アプリ ID の作成
- プロビジョニング プロファイルを作成する
- プロファイルと証明書をダウンロードする

これらの各手順には、開発中のアプリケーションの種類に応じて対処する必要がある変数が含まれています。 手動でまたは Xcode を介して開発用にデバイスをセットアップするために必要な手順の詳細については、「[デバイスのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)」ガイドを参照してください。

このガイドでは、Xcode の代替としての fastlane ツールを紹介し、次について説明します。

- [sigh とは](#whatissigh)
- [App ID を作成する](#appid)
- [新しいデバイスを追加する](#newdevices)
- [sigh の使用](#using)
- [その他のオプション](#options)

> [!NOTE]
> アプリのバンドル ID と一致する既存の App ID があり、デバイスが Developer ポータルに存在する場合は、「[アプリ ID の作成](#appid)」と「[新しいデバイスを追加する](#newdevices)」の手順は無視できます。 その場合は、[sigh の使用](#using)に直接進んで開始します。

## <a name="installation"></a>インストール

fastlane のインストールの詳細については、[fastlane](~/ios/deploy-test/provisioning/fastlane/index.md#Installation) の概要に関するガイドを参照してください。

<a name="whatissigh" />

## <a name="what-is-sigh"></a>sigh とは

sigh には、次のすべての構成のプロビジョニング プロファイルを作成および更新できるターミナル インターフェイスが用意されています。開発、App Store 配布、アドホック配布、およびエンタープライズ配布。 さらに、プロビジョニング プロファイルをダウンロードして修復する簡単な方法を提供します。

<a name="appid" />

## <a name="creating-an-app-id"></a>アプリ ID の作成

App ID は、次のコマンドで作成できます。

```
fastlane produce -u your@appleid.com -a com.company.appname --skip_itc
```

ここで `com.company.appname` はアプリのバンドル ID で、次に示すように、Xamarin.iOS アプリケーションの Info.plist ファイルで見つかります。

[![](sigh-images/fastlane-image5.png "Xamarin.iOS アプリケーションの Info.plist ファイル")](sigh-images/fastlane-image5.png#lightbox)

一意の App ID は、逆引き DNS スタイルの文字列である必要があります。 ID が作成されたら、メモします。これは、このガイドで後ほど sigh を使用する場合に必要になります。

アプリを [iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) で作成する必要がある場合は、上記のコマンドから `--skip_itc` フラグを削除します。

<a name="newdevices" />

## <a name="adding-new-devices"></a>新しいデバイスを追加する

コマンドラインから Developer ポータルに 1 つのデバイスを追加するには、次のコマンドを入力します。

```bash
fastlane run register_device name:"Adam iPhone" udid:"abcdeg1234567"
```

複数のデバイスを追加するには、`register_devices` コマンドを使用します。

```bash
    register_devices(
        devices: {
            "iPhone 6" => "1234567890123456789012345678901234567890",
            "iPad Air 2" => "abcdefghijklmnopqrstvuwxyzabcdefghijklmn"
         }
    )
```

<a name="using" />

## <a name="using-sigh"></a>sigh の使用

sigh ユーティリティの使用を開始するには、端末に次のコマンドを入力します。

```bash
fastlane sigh
```

既定ではこれが [App Store Distribution](~/ios/deploy-test/app-distribution/app-store-distribution/index.md) プロビジョニング プロファイルを作成します。 開発用にデバイスを設定するには、`--development` フラグ: 

```bash
fastlane sigh --development
```

fastlane によって求められたら、Apple ID のユーザー名を入力します。 fastlane を初めて使用する場合は、パスワードも求められる場合があります。 それ以外の場合は、キーチェーンからパスワードの環境変数をプルします。

Apple ID が複数のチームに接続されている場合は、ここに表示されます。 使用するチームに対応する番号を選択します。

[![](sigh-images/fastlane-image2.png "使用するチームの選択")](sigh-images/fastlane-image2.png#lightbox)

次の方法で CLI にチーム ID を渡すこともできます。

```bash
fastlane sigh -l 2TU993NY9J
```

アプリの [App ID](#appid) を入力します。 これは、アプリの Info.plist のバンドル ID と一致している必要があります。

自分のアカウントに接続されているすべてのデバイスがプロビジョニング プロファイルに追加されます。

そしてプロビジョニング プロファイルが fastlane によって作成、ダウンロード、およびインストールされます。

Developer Center を参照すると、以下に示すように、新しく作成されたプロビジョニング プロファイルを表示できます。

[![](sigh-images/fastlane-image10.png "新たに作成されたプロビジョニング プロファイルを表示")](sigh-images/fastlane-image10.png#lightbox)

既定では、sigh は現在のフォルダーにプロビジョニング プロファイルを格納します。 出力ディレクトリを変更するには、`output_path` を編集するか、次の手順を実行します。

```bash
fastlane sigh -o "~/Library/MobileDevice/Provisioning Profiles"
```

<a name="options" />

## <a name="sigh-additional-options"></a>sigh のその他のオプション

sigh を使用する際に、次のオプションを使用して追加のサポートを提供できます。

- すべてのプロビジョニング プロファイルをダウンロードするには、次を使用します。

    ```bash
    fastlane sigh download_all
    ```

- プロビジョニング プロファイルに特定の署名 ID を使用するには、次を使用します。

    ```bash
    fastlane sigh -c "Amy cert"
    ```
    
    ここで `Amy cert` はコード署名 ID の名前です。

## <a name="related-links"></a>関連リンク

- [fastlane - sigh](https://github.com/fastlane/fastlane/tree/master/sigh#readme)
