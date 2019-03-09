---
title: Apple Watch デバイスでのテスト
description: このドキュメントでは、実際の Apple Watch でのテスト、Xamarin でビルドされた watchOS アプリをデプロイする方法について説明します。 プロビジョニング プロファイルをテストするには、デバイスについて説明し、トラブルシューティングのヒントを提供します。
ms.prod: xamarin
ms.assetid: A72A7D38-FAE8-4DD2-843D-54B74C5078D7
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 9c15e9205b96a02caa182e47b71c6d36c8bff1aa
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57671053"
---
# <a name="testing-on-apple-watch-devices"></a>Apple Watch デバイスでのテスト

従った後、[展開手順](~/ios/watchos/deploy-test/index.md)、このページに指示を使用 (必要な場合)、アプリ Id とアプリのグループを作成します。

- [セットアップ デバイス](#devices)Apple のデベロッパー センターで、
- [開発プロビジョニング プロファイルを作成する](#profiles)、し、
- [配置およびテスト](#testing)Apple Watch にします。

<a name="devices" />

## <a name="devices"></a>デバイス

実際の iPhone または iPad で iOS アプリをテストすると、デベロッパー センターに登録するデバイスが常に必要です。 次のようなデバイスの一覧 (プラス記号をクリックします。 **+** 新しいデバイスを追加する)。

![](device-images/devices-sml.png "次のようなデバイスの一覧")

ウォッチではありません - 今すぐアプリをデプロイする前に、Apple Watch デバイスを追加する必要があります。 ウォッチの UDID を使用して検索**Xcode** (**Windows > デバイス**リスト)。 ペアになっている電話が接続されている場合、ウォッチの情報も表示されます。

[![](device-images/xcode-devices-sml.png "Watch のペアリング情報")](device-images/xcode-devices.png#lightbox)

わかっている場合、ウォッチの UDID、デベロッパー センターでデバイスの一覧に追加します。

![](device-images/devices-watch-sml.png "ウォッチのデバイスの一覧で UDID")

ウォッチ デバイスが追加されると、新規または既存の開発や、アドホックのプロビジョニング プロファイルを作成するで選択されているを確認します。

![](device-images/devices-provisioning.png "使用可能なデバイスの一覧")

ダウンロードし、再インストールする既存のプロビジョニング プロファイルを編集するかどうかは忘れないでください。

<a name="profiles" />

## <a name="development-provisioning-profiles"></a>開発プロビジョニング プロファイル

作成する必要があるデバイスでテスト用にビルドする、**開発プロビジョニング プロファイル**ソリューション内のアプリ ID ごとにします。

ワイルドカード アプリ ID がある場合*プロビジョニング プロファイルを 1 つだけが必要になります*; が、プロジェクトごとに個別のアプリ ID があるかどうかは、各アプリ ID のプロビジョニング プロファイルを必要があります。

![](device-images/provisioningprofile-development.png "開発プロビジョニング プロファイル")

次の 3 つのすべてのプロファイルを作成したら、一覧に表示されます。 ダウンロードし、それぞれのインストールに注意してください。

![](device-images/provisioningprofiles.png "使用可能な開発プロビジョニング プロファイル")

プロビジョニング プロファイルを確認することができます、**プロジェクト オプション**を選択して、**ビルド > iOS バンドル署名**画面とを選択すると、**リリース**または**IPhone デバッグ**構成します。

**プロビジョニング プロファイル**表示されるすべての一致するプロファイル - このドロップダウン リストで作成した一致するプロファイルを表示する必要があります。

![](device-images/options-selectprofile.png "プロビジョニング プロファイルの一覧")


<a name="testing" />

## <a name="testing-on-a-watch-device"></a>Watch デバイスでのテスト

デバイス、アプリの Id とプロビジョニング プロファイルを構成した後をテストする準備が整いました。

1. IPhone が接続されているし、監視は、iPhone と既にペアリングしてください。

2. 構成設定されている確認**リリース**または**デバッグ**します。

3. 接続している iPhone デバイスがターゲットの一覧で選択されていることを確認します。

4. IOS アプリのプロジェクト (、ウォッチや拡張機能) を右クリックし、選択**スタートアップ プロジェクトとして設定**します。

5. をクリックして、**実行**ボタン (または選択を**開始**オプションを**実行**メニュー)。

6. ソリューションがビルドされ、iOS アプリを iPhone に配置されます。
  IOS アプリまたはウォッチ拡張機能のプロビジョニングを正しく設定しない場合は、iPhone へのデプロイは失敗します。

7. 展開が正常に完了すると、ペアになっているウォッチを watch アプリを送信する、iPhone が自動的に試みます。 アプリのアイコンが循環にウォッチ画面に表示されます*インストール*進行状況インジケーター。

8. Watch アプリが正常にインストールされている場合、アイコンは引き続き [ウォッチ] 画面のタッチ、アプリのテストを開始するように!


## <a name="troubleshooting"></a>トラブルシューティング

展開の使用中にエラーが発生した場合、**ビュー > パッド > デバイス ログ**エラーに関する詳細を表示します。 一部のエラーとその原因は、以下に示します。

### <a name="error-mt3001-could-not-aot-the-assembly"></a>エラー MT3001:AOT アセンブリではない可能性があります。

これは、Apple Watch デバイスへの展開をデバッグ モードでビルドする場合に発生する可能性があります。

*一時的に*この問題を回避するには、無効にする**インクリメンタル ビルド**ウォッチ拡張機能で**プロジェクト オプション > ビルド > watchOS ビルド**ウィンドウ。

[![](device-images/disable-incremental-sml.png "インクリメンタル ビルドのチェック ボックス")](device-images/disable-incremental.png#lightbox)

これは後インクリメンタル ビルドを有効にできます再ビルド時間の短縮を活用するために、将来のリリースで修正されます。


### <a name="watch-app-fails-to-start-while-debugging-on-device"></a>Watch アプリのデバイスでのデバッグ中に起動に失敗します。

表示アイコン & 読み込みスピナーの物理デバイス上の watch アプリをデバッグしようとしたときに、最終的にタイムアウト。 これは、将来のリリースで解決されます。(これはデバッグはできません)、リリース ビルドを実行する問題を回避することです。


### <a name="invalid-application-executable-or-application-verification-failed"></a>無効なアプリケーション実行可能ファイルまたはアプリケーションの検証に失敗しました

```csharp
Failed to install [APPNAME]
Invalid executable/Application Verification Failed
```

![](device-images/invalid-application-executable.png "無効なアプリケーション実行可能ファイルのアラート")

これらのメッセージが表示されない場合*ウォッチ画面*いくつかの問題がありますが、アプリをインストールしようとしましたが後、。

- ウォッチ デバイス自体は、Apple のデベロッパー センターでのデバイスとして追加されていません。 指示に従って、[デバイスを正しく構成](#devices)します。

- テストに使用される開発プロビジョニング プロファイルに含まれる; ウォッチ デバイスはありませんでした。または、ウォッチがプロビジョニング プロファイルに追加された後は、再ダウンロードし、再インストール、でした。 指示に従って、[プロビジョニング プロファイルの構成を正しく](#profiles)します。

- 場合、 **iOS デバイスのログ**を含む`The system version is lower than the minimum OS version specified for bundle...Have 8.2; need 8.3`し、Watch アプリの**Info.plist**間違った**MinimumOSVersion**値。
  必要があります**8.2** - ソースを手動で編集する必要があります Xcode 6.3 がインストールされている場合は、insert 8.2 に設定します。

- Watch アプリの**Entitlements.plist**正しくが権利を有効にできません (アプリ グループ) などはありません。

- Watch アプリの**アプリ ID** (アプリ グループ) などを有効にする権利を持つことはできませんが、デベロッパー センターでは。



### <a name="install-never-finished"></a>インストールが完了しません。

```csharp
SPErrorGizmoInstallNeverFinishedErrorMessage
```

このエラーは、Watch アプリの上の不要な (と無効な) キーである可能性があります**Info.plist**ファイル。 Watch アプリで iOS アプリまたはウォッチ拡張機能用のキーを含めないでください。

<!--eg. NSLocationAlwaysUsageDescription -->


### <a name="waiting-for-debugger-to-connect"></a>「デバッガーの接続を待機しています」

場合、**アプリケーション出力**表示ウィンドウのままになります。

```csharp
waiting for debugger to connect
```

依存しているかどうか、プロジェクトに含まれている Nuget のいずれかを確認**Microsoft.Bcl.Build**します。 など、人気のあるいくつかの Microsoft の発行したライブラリにこれが自動的に追加[Microsoft Http Client Libraries](https://www.nuget.org/packages/Microsoft.Net.Http/)します。

**Microsoft.Bcl.Build.targets**ファイルに追加される、 **.csproj** iOS 拡張機能のデプロイ時にパッケージ化に干渉することができます。 追跡することができます、[バグ](https://bugzilla.xamarin.com/show_bug.cgi?id=29912)します。
回避策は、.csproj ファイルを編集し、手動で移動する、 **Microsoft.Bcl.Build.targets**最後の要素でなければなりません。

