---
title: Xamarin でビルドされたアプリを tvOS のトラブルシューティング
description: この記事では、Xamarin でビルドされた tvOS アプリの開発時にトラブルシューティングに役立つさまざまなヒントを提供します。 これは、特定のエラーと既知の問題について説明します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 124E4953-4DFA-42B0-BCFC-3227508FE4A6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: e69157cf9c8a9b9405e31edb2906754328653ccb
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789204"
---
# <a name="troubleshooting-tvos-apps-built-with-xamarin"></a>Xamarin でビルドされたアプリを tvOS のトラブルシューティング

_この記事カバー既知の問題点の Xamarin の tvOS のサポートとやり取りしているときに発生する可能性があります。_

<a name="Known-Issues" />

## <a name="known-issues"></a>既知の問題

Xamarin の tvOS のサポートの現在のリリースでは、次の既知の問題があります。

- **Mono フレームワーク**– モノラル 4.3 Cryptography.ProtectedData がモノラル 4.2 からデータを復号化に失敗します。 NuGet パッケージは、エラーのため、復元に失敗する結果として、`Data unprotection failed`保護されている NuGet のソースが構成されている場合。
    - **回避策**– Visual Studio for Mac のいずれかの NuGet パッケージ ソースにパッケージを復元して再試行する前に使用するパスワード認証を再度追加する必要があります。
- **Visual Studio を使用した f# アドインで Mac を**– Windows に f# Android テンプレートを作成するとエラーが発生します。 これは引き続き正常に機能 mac
- **Xamarin.Mac**を設定すると、ターゲット フレームワーク Xamarin.Mac 統合テンプレート プロジェクトを実行する – `Unsupported`、ポップアップ`Could not connect to the debugger`場合があります。
    - **潜在的な回避策**– 安定した当社のチャネルで使用可能なの Mono フレームワーク バージョンにダウン グレードします。
- **Visual Studio の Xamarin & Xamarin.iOS** – Visual studio で、エラー WatchKit アプリケーションの配置と`The file ‘bin\iPhoneSimulator\Debug\WatchKitApp1WatchKitApp.app\WatchKitApp1WatchKitApp’ does not exist`場合があります。

レポートのいずれかのバグを検索してください[Bugzilla](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

## <a name="troubleshooting"></a>トラブルシューティング

次のセクションでは、tvOS 9 を Xamarin.tvOS とそれらの問題の解決策を使用する場合に発生する可能性がある既知の問題を一覧表示します。

### <a name="invalid-executable---the-executable-does-not-contain-bitcode"></a>無効な実行可能ファイルの実行可能ファイルを含まない bitcode

Apple TV の App Store に Xamarin.tvOS アプリを送信しようとすると、フォームのエラー メッセージをする可能性があります _「無効な実行可能ファイルには実行可能ファイルが含まれていない bitcode には」_ です。

この問題を解決するには、次の操作を行います。

1. Mac 用 Visual Studio で、Xamarin.tvOS プロジェクト ファイルでを右クリックし、**ソリューション エクスプ ローラー**選択**オプション**です。
2. 選択**tvOS ビルド**にあることを確認して、**リリース**構成。 

    [![](troubleshooting-images/ts01.png "TvOS ビルド オプションを選択します。")](troubleshooting-images/ts01.png#lightbox)
3. 追加`--bitcode=asmonly`を**追加 mtouch 引数**フィールドでをクリックし、 **[ok]** ボタンをクリックします。
4. アプリを再構築、**リリース**構成します。

### <a name="verifying-that-your-tvos-app-contains-bitcode"></a>ことを確認、tvOS アプリが含まれています Bitcode

Bitcode が、Xamarin.tvOS アプリのビルドに含まれていることを確認するには、ターミナル アプリを開く」と入力します。

```csharp
otool -l /path/to/your/tv.app/tv
```

出力では、次の検索します。

```csharp
Section
  sectname __bundle
   segname __LLVM
      addr 0x0000000100001000
      size 0x000000000000124f
    offset 4096
     align 2^0 (1)
    reloff 0
    nreloc 0
     flags 0x00000000
 reserved1 0
 reserved2 0
```

`addr` および`size`異なりますが、その他のフィールドは、同一である必要があります。

確認する必要がありますも静的、サード パーティ (`.a`) tvOS ライブラリ (iOS ライブラリではない) を使用しているライブラリが構築されし、それらもが含まれている bitcode 情報には。

アプリまたはライブラリを含む有効な bitcode、 `size` 1 より大きい値になります。 状況によっては、ライブラリが bitcode マーカーがあるまだ有効な bitcode が含まれていない場所です。 例えば:

**無効な Bitcode**

```csharp
 $ otool -arch arm64 libLibrary.a | grep __bitcode -A 3
   sect name __bitcode
   segname __LLVM
      add 0x0000000000000670
      size 0x0000000000000001
```

**有効な Bitcode** 

```csharp
$ otool -l -arch arm64 libDownloadableAgent-tvos.a |grep __bitcode -A 3
   sectname __bitcode
   segname __LLVM
      addr 0x000000000001d2d0
      size 0x0000000000045440
```

違いに注意してください`size`一覧の例では 2 つのライブラリとの間は、上で実行します。 Bitcode が有効になっていると Xcode アーカイブ ビルドからライブラリを生成する必要があります (Xcode 設定`ENABLE_BITCODE`) このサイズの問題の解決策として。

### <a name="apps-that-only-contain-the-arm64-slice-must-also-have-arm64-in-the-list-of-uirequireddevicecapabilities-in-infoplist"></a>Arm64 スライスにはのみが含まれているアプリでは、Info.plist で UIRequiredDeviceCapabilities の一覧では、"arm64"必要があります。

パブリケーションの Apple TV のアプリ ストアにアプリを送信するときに、フォームでエラーが発生する可能性があります。

_「アプリのみ arm64 スライスを含むことも必要があります"arm64"Info.plist で UIRequiredDeviceCapabilities の一覧で、」_

この場合、編集、`Info.plist`ファイルを次のキーがあることを確認してください。

```xml
<key>UIRequiredDeviceCapabilities</key>
<array>
    <string>arm64</string>
</array>
```

リリース用のアプリを再コンパイルし、iTunes Connect を再実行してください。

### <a name="task-mtouch-execution----failed"></a>タスク"MTouch"の実行 - 失敗

(MonoGame) などのサード パーティ ライブラリを使用するいるし、長い一連ので終わるエラー メッセージが、リリース コンパイルが失敗`Task "MTouch" execution -- FAILED`を追加してみてください`-gcc_flags="-framework OpenAL"`を**追加タッチ引数**:

[![](troubleshooting-images/mtouch01.png "タスクの MTouch 実行")](troubleshooting-images/mtouch01.png#lightbox)

含めます`--bitcode=asmonly`で、**追加タッチ引数**、リンカー オプション設定**リンクすべて**クリーン コンパイルを行うとします。

### <a name="itms-90471-error-the-large-icon-is-missing"></a>ITMS 90471 エラーです。 大きいアイコンがありません。

"ITMS 90471 エラー形式でメッセージを取得する場合。 大きいアイコンは不足している"リリースでは、Apple TV の App Store に Xamarin.tvOS アプリを送信しようとしているときに確認してください以下。

1. 大きいアイコン アセットが含まれていることを確認、`Assets.car`を使用して作成したファイルを[アプリ アイコン](~/ios/tvos/app-fundamentals/icons-images.md#App-Icons)ドキュメント。
2. 含まれていることを確認、`Assets.car`ファイルから、[アイコンとイメージ処理](~/ios/tvos/app-fundamentals/icons-images.md)最終的なアプリケーション バンドルのドキュメントです。

### <a name="invalid-bundle--an-app-that-supports-game-controllers-must-also-support-the-apple-tv-remote"></a>無効なバンドル – ゲーム コント ローラーをサポートするアプリをリモート Apple TV にする必要がありますもはサポート

または 

### <a name="invalid-bundle--apple-tv-apps-with-the-gamecontroller-framework-must-include-the-gcsupportedgamecontrollers-key-in-the-apps-infoplist"></a>無効なバンドル – GameController フレームワークと Apple TV のアプリは、アプリの Info.plist で GCSupportedGameControllers キーを含める必要があります。

ゲームを拡張し、ゲームの世界の意味を提供するゲーム コント ローラーを使用できます。 また、ユーザーは、リモート コンピューターと、コント ローラーの間で切り替える必要はありませんので、Apple TV の標準のインターフェイスを制御するも使用できます。

Apple TV のアプリ ストアへのゲーム コント ローラーのサポートと Xamarin.tvOS アプリを送信している場合の形式で、エラー メッセージを取得します。


_私たちには、「アプリケーション名」の最新の配信で 1 つまたは複数の問題が発見しました。配信が成功した場合が、次の配信では、次の問題を解決することができます。_

_無効なバンドル – ゲーム コント ローラーをサポートするアプリには、Apple TV のリモート サポートもあります。_

または 

_無効なバンドル – GameController フレームワークと Apple TV のアプリは、アプリの Info.plist で GCSupportedGameControllers キーを含める必要があります。_

解決 Siri リモートのサポートを追加するには (`GCMicroGamepad`) をアプリの`Info.plist`ファイル。 Siri リモートの対象とする apple マイクロ ゲーム コント ローラー プロファイルが追加されました。 たとえば、次のキーを追加します。

```xml
<key>GCSupportedGameControllers</key>  
  <array>  
    <dict>  
      <key>ProfileName</key>  
      <string>ExtendedGamepad</string>  
    </dict>  
    <dict>  
      <key>ProfileName</key>  
      <string>MicroGamepad</string>  
    </dict>  
  </array>  
<key>GCSupportsControllerUserInteraction</key>  
<true/>
```

> [!IMPORTANT]
> Bluetooth ゲーム コント ローラーは、エンドユーザーに対して行うことが省略可能な購買、アプリは、いずれかを購入するユーザーを強制できません。 アプリは、ゲーム コント ローラーをサポートしている場合必要がありますもサポート Siri リモート ゲームは、Apple TV のすべてのユーザーによって使用できるようにします。

詳細についてを参照してください、[ゲーム コントローラを備えた作業](~/ios/tvos/platform/remote-bluetooth.md#Working-with-Game-Controllers)のセクション、 [Siri リモート コンピューターと Bluetooth コント ローラー](~/ios/tvos/platform/remote-bluetooth.md)ドキュメント。

### <a name="incompatible-target-framework-netportable-versionv45-profileprofile78"></a>互換性のないターゲット フレームワーク: です。NetPortable, Version = v4.5、プロファイル Profile78 を =

Xamarin.tvOS プロジェクトにポータブル クラス ライブラリ (PCL) が含まれている場合は、フォームをメッセージを取得する可能性があります。

_互換性のないターゲット フレームワーク: です。NetPortable, Version = v4.5、プロファイル Profile78 を =_

この問題を解決すると呼ばれる XML ファイルを追加` Xamarin.TVOS.xml`次の内容。

```xml
<Framework Identifier="Xamarin.TVOS" MinimumVersion="1.0" Profile="*" DisplayName="Xamarin.TVOS"/>
```

次のパス。

```csharp
/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/xbuild-frameworks/.NETPortable/v4.5/Profile/Profile259/SupportedFrameworks/

```

パスのプロファイル数が、PCL のプロファイル数に一致する必要があることに注意してください。

このファイルには、Xamarin.tvOS プロジェクトに PCL ファイルを正常に追加することができます。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリケーション プログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
