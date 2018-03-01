---
title: "ウォッチ デバイスでのテスト"
description: "Apple Watch でテストするアプリの展開"
ms.topic: article
ms.prod: xamarin
ms.assetid: A72A7D38-FAE8-4DD2-843D-54B74C5078D7
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: fab2092837f9b9ca8ada53274c9644f131fb5659
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="testing-on-watch-devices"></a>ウォッチ デバイスでのテスト

後[配置手順](~/ios/watchos/deploy-test/index.md)する (必要な場合) のアプリ Id とアプリ グループを作成するには、このページで指示を使用してください。

- [セットアップ デバイス](#devices)Apple のデベロッパー センターで、
- [開発のプロビジョニング プロファイルを作成する](#profiles)、し、
- [配置およびテスト](#testing)Apple Watch でします。

<a name="devices" />

## <a name="devices"></a>デバイス

実際の iPhone または iPad で iOS アプリをテストすると、デベロッパー センターに登録するデバイスは常に必要です。 次のようなデバイスの一覧 (プラス記号をクリックして **+** 新しいデバイスを追加する)。

![](device-images/devices-sml.png "次のようなデバイスの一覧")

監視ではありません - 今すぐアプリを展開する前に、Apple Watch デバイスを追加する必要があります。 ウォッチの UDID を使用して検索**Xcode** (**Windows > デバイス**リスト)。 ペアの電話が接続されている場合、ウォッチの情報も表示されます。

[ ![](device-images/xcode-devices-sml.png "ウォッチ式のペアについて")](device-images/xcode-devices.png)

わかっている場合、ウォッチの UDID、デベロッパー センター内のデバイスの一覧に追加します。

![](device-images/devices-watch-sml.png "ウォッチのデバイスの一覧で UDID")

ウォッチ デバイスが追加されると、新規または既存の開発やを作成するアドホック プロビジョニング プロファイルで選択されていることを確認します。

![](device-images/devices-provisioning.png "使用可能なデバイスの一覧")

ダウンロードし、再インストールする既存のプロビジョニング プロファイルを編集するかどうかは注意してください。

<a name="profiles" />

## <a name="development-provisioning-profiles"></a>プロビジョニング プロファイルの開発

作成する必要があります。 デバイス上でのテストを構築する、**開発プロビジョニング プロファイル**ソリューションでは、各アプリ id。

ワイルドカード アプリ ID を使用していれば*プロビジョニング プロファイルを 1 つだけが必要になります*; が、プロジェクトごとに個別のアプリ ID があるかどうかは、各アプリ ID のプロビジョニング プロファイルが必要があります。

![](device-images/provisioningprofile-development.png "開発のプロビジョニング プロファイル")

3 つすべてのプロファイルを作成したら、一覧に表示されます。 ダウンロードしてインストールの 1 つずつに注意してください。

![](device-images/provisioningprofiles.png "使用可能な開発プロビジョニング プロファイル")

プロビジョニング プロファイルを確認することができます、**プロジェクト オプション**を選択して、**ビルド > iOS バンドル署名 ***画面を選択して、**リリース**または**IPhone のデバッグ**構成します。

**プロビジョニング プロファイル**リストが一致するすべてのプロファイルを表示する - このドロップダウン リストで作成した一致のプロファイルが表示されます。

![](device-images/options-selectprofile.png "プロビジョニング プロファイルの一覧")


<a name="testing" />

## <a name="testing-on-a-watch-device"></a>ウォッチ デバイスでのテスト

デバイス、アプリの Id およびプロビジョニング プロファイルを構成した後は、テストする準備ができたらです。

1. IPhone が差し込まれて、ウォッチは既にペアになって、iPhone で確認してください。

2. 構成に設定されていることを確認**リリース**または**デバッグ**です。

3. ターゲットの一覧で接続している iPhone デバイスが選択されていることを確認します。

4. (ウォッチまたは拡張いない) iOS アプリのプロジェクトを右クリックし、選択**スタートアップ プロジェクトとして設定**です。

5. をクリックして、**実行**ボタン (かを選択して、**開始**オプションを**実行**メニュー)。

6. ソリューションがビルドされ、iPhone に iOS アプリが展開されます。
  IOS アプリまたはウォッチ拡張機能のプロビジョニングが正しく設定されていない場合は、iPhone への展開は失敗します。

7. 展開が正常に完了すると、ペアのウォッチを watch アプリを送信する iPhone は自動的に試行します。 アプリのアイコンは、循環をウォッチ画面に表示されます*インストール*進行状況インジケーター。

8. Watch アプリが正常にインストールされている場合、アイコンは残ります [ウォッチ] 画面での touch アプリのテストを開始するようにします。


## <a name="troubleshooting"></a>トラブルシューティング

展開の使用時にエラーが発生した場合、**ビュー > パッド > デバイス ログ**エラーに関する詳細を表示します。 いくつかのエラーとその原因は、次に示します。

### <a name="error-mt3001-could-not-aot-the-assembly"></a>エラー MT3001: でした AOT アセンブリではありません。

これは、Apple Watch デバイスに展開するデバッグ モードでのビルド時に発生する可能性があります。

*一時的に*この問題を回避するには、無効にする**インクリメンタル ビルド**ウォッチ拡張機能で**プロジェクトのオプション > ビルド > watchOS ビルド**ウィンドウ。

[ ![](device-images/disable-incremental-sml.png "インクリメンタル ビルドのチェック ボックス")](device-images/disable-incremental.png)

これは、その後インクリメンタル ビルドを有効にできます活用ビルド時間を短縮するために将来のリリースで修正されます。


#<a name="3-watch-app-fails-to-start-while-debugging-on-device"></a>&#3; watch アプリ起動が失敗するデバイスでのデバッグ中には

ときに表示されるアイコンと読み込みのスピン ボタンのみ、物理デバイスで watch アプリをデバッグしようとしています (と最終的にタイムアウト)。 これは今後のリリースでアドレス指定すること回避策では、(デバッグを許可しないされます) をリリース ビルドを実行します。


### <a name="invalid-application-executable-or-application-verification-failed"></a>無効なアプリケーションの実行可能ファイルまたはアプリケーションの検証に失敗しました

```csharp
Failed to install [APPNAME]
Invalid executable/Application Verification Failed
```

![](device-images/invalid-application-executable.png "無効なアプリケーション実行可能ファイルのアラート")

これらのメッセージが表示されない場合は*[ウォッチ] 画面で*いくつかの問題がある可能性がありますアプリをインストールしようとしましたが後、。

- ウォッチ デバイス自体は、Apple のデベロッパー センター上のデバイスとして追加されていません。 指示に従って[デバイスを正しく構成](#devices)です。

- 開発のプロビジョニング プロファイルをテストするために使用されているが含まれている; ウォッチ デバイスがありません。または、ウォッチがプロビジョニング プロファイルに追加された後は、再ダウンロードし、再インストール、されませんでした。 指示に従って[プロビジョニング プロファイルを正しく構成](#profiles)です。

- 場合、 **iOS デバイス ログ**が含まれています`The system version is lower than the minimum OS version specified for bundle...Have 8.2; need 8.3`し、Watch アプリの**Info.plist**間違った**MinimumOSVersion**値。
  これは、値には**8.2** - には、ソースを手動で編集する必要があります Xcode 6.3 がインストールされている場合は、insert 8.2 に設定します。

- Watch アプリの**Entitlements.plist**正しくが権利有効になっていません (アプリ グループ) などはずことです。

- Watch アプリの**アプリ ID** (アプリ グループ) などに有効な権利正しくないデベロッパー センター内がします。



### <a name="install-never-finished"></a>インストールが完了しません。

```csharp
SPErrorGizmoInstallNeverFinishedErrorMessage
```

このエラーは、Watch アプリの不要な (と無効な) キーである可能性があります**Info.plist**ファイル。 Watch アプリに iOS アプリまたはウォッチ拡張機能のことを意図したキーを含める必要はありません。

<!--eg. NSLocationAlwaysUsageDescription -->


### <a name="waiting-for-debugger-to-connect"></a>「デバッガーの接続を待機しています」

場合、**アプリケーション出力**ウィンドウは表示行き詰まって

```csharp
waiting for debugger to connect
```

プロジェクトに含まれている NuGets のものがあるかどうか、依存関係確認**Microsoft.Bcl.Build**です。 自動的には、人気の高いなど、いくつか Microsoft 公開ライブラリで追加[Microsoft Http Client Libraries](http://www.nuget.org/packages/Microsoft.Net.Http/)です。

**Microsoft.Bcl.Build.targets**ファイルに追加される、 **.csproj** iOS の拡張機能の配置時にパッケージ化に干渉することができます。 追跡することができます、[バグ](https://bugzilla.xamarin.com/show_bug.cgi?id=29912)です。
考えられる回避策は .csproj ファイルを編集し、手動で移動する、 **Microsoft.Bcl.Build.targets**最後の要素でなければなりません。

