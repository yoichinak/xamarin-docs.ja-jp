---
title: Xamarin でビルドされた tvOS アプリのトラブルシューティング
description: この記事では、Xamarin でビルドされた tvOS アプリの開発中にトラブルシューティングを行うためのさまざまなヒントを提供します。 既知の問題と特定のエラーについて説明します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 124E4953-4DFA-42B0-BCFC-3227508FE4A6
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/16/2017
ms.openlocfilehash: 3fb479321686e4b956fc6ffee90dd5b0b2c16d9c
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291191"
---
# <a name="troubleshooting-tvos-apps-built-with-xamarin"></a>Xamarin でビルドされた tvOS アプリのトラブルシューティング

_この記事では、Xamarin の tvOS サポートの使用中に発生する可能性がある問題について説明します。_

<a name="Known-Issues" />

## <a name="known-issues"></a>既知の問題

Xamarin の tvOS サポートの現在のリリースには、次の既知の問題があります。

- Mono**フレームワーク**– Mono 4.3 ProtectedData は、mono 4.2 からのデータの暗号化解除に失敗します。 その結果、保護された nuget ソースが構成され`Data unprotection failed`ていると、nuget パッケージはエラーが発生して復元に失敗します。
  - **回避策**: Visual Studio for Mac、パッケージの復元を再試行する前に、パスワード認証を使用するすべての NuGet パッケージソースを追加し直す必要があります。
- **Visual Studio for Mac w/ F#アドイン**-Windows で Android テンプレートをF#作成するときにエラーが発生します。 これは Mac でも正常に機能します。
- **Xamarin. mac** –ターゲットフレームワークがに`Unsupported`設定された xamarin の統合テンプレートプロジェクトを実行すると、ポップアップ`Could not connect to the debugger`が表示される場合があります。
  - **考えられる回避策**–安定したチャネルで利用できる Mono フレームワークのバージョンをダウングレードします。
- **Xamarin visual studio & xamarin. iOS** – WatchKit アプリケーションを visual studio にデプロイすると、 `The file ‘bin\iPhoneSimulator\Debug\WatchKitApp1WatchKitApp.app\WatchKitApp1WatchKitApp’ does not exist`エラーが表示されることがあります。

[GitHub](https://github.com/xamarin/xamarin-macios/issues/new)で見つかったすべてのバグを報告してください。

## <a name="troubleshooting"></a>トラブルシューティング

次のセクションでは、tvOS 9 と tvOS を使用する場合に発生する可能性があるいくつかの既知の問題と、それらの問題の解決策を示します。

### <a name="invalid-executable---the-executable-does-not-contain-bitcode"></a>無効な実行可能ファイル-実行可能ファイルにビットコードが含まれていません

TvOS アプリを Apple TV App Store に送信しようとすると、 _"無効な実行可能ファイル-実行可能ファイルにビットコードが含まれていません"_ という形式のエラーメッセージが表示されることがあります。

この問題を解決するには、次の手順を実行します。

1. Visual Studio for Mac で、**ソリューションエクスプローラー**で TvOS プロジェクトファイルを右クリックし、 **[オプション]** を選択します。
2. **[TvOS Build]** を選択し、**リリース**構成になっていることを確認します。 

    [![](troubleshooting-images/ts01.png "TvOS ビルドオプションの選択")](troubleshooting-images/ts01.png#lightbox)
3. [ `--bitcode=asmonly`追加の**mtouch 引数**] フィールドにを追加し、 **[OK]** ボタンをクリックします。
4. **リリース**構成でアプリをリビルドします。

### <a name="verifying-that-your-tvos-app-contains-bitcode"></a>TvOS アプリにビットコードが含まれていることを確認しています

TvOS アプリのビルドに Bitcode が含まれていることを確認するには、ターミナルアプリを開き、次のように入力します。

```csharp
otool -l /path/to/your/tv.app/tv
```

出力で、次のことを確認します。

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

`addr`と`size`は異なりますが、他のフィールドは同じである必要があります。

使用しているサードパーティの静的 (`.a`) ライブラリが (iOS ライブラリではなく) tvOS ライブラリに対してビルドされていること、およびビットコード情報も含まれていることを確認する必要があります。

有効な bitcode を含むアプリまたはライブラリ`size`の場合、は1より大きくなります。 ライブラリが bitcode マーカーを持つことができる状況もありますが、有効な bitcode は含まれていません。 例えば:

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

上記の例の`size` 2 つのライブラリの違いに注意してください。 このサイズの問題の解決策として、bitcode が有効になっ`ENABLE_BITCODE`ている Xcode archive ビルド (Xcode 設定) からライブラリを生成する必要があります。

### <a name="apps-that-only-contain-the-arm64-slice-must-also-have-arm64-in-the-list-of-uirequireddevicecapabilities-in-infoplist"></a>Arm64 スライスだけを含むアプリも、UIRequiredDeviceCapabilities の一覧に "arm64" が含まれている必要があります。

アプリを発行用の Apple TV App Store に送信すると、次の形式でエラーが表示されることがあります。

_"Arm64 スライスだけを含むアプリには、UIRequiredDeviceCapabilities の一覧に" arm64 "が含まれている必要もあります。_

この問題が発生した`Info.plist`場合は、ファイルを編集し、次のキーが含まれていることを確認します。

```xml
<key>UIRequiredDeviceCapabilities</key>
<array>
  <string>arm64</string>
</array>
```

アプリをリリース用に再コンパイルし、iTunes Connect に再送信します。

### <a name="task-mtouch-execution----failed"></a>タスク "MTouch" の実行-失敗

サードパーティ製のライブラリ (モノゲームなど) を使用していて、で`Task "MTouch" execution -- FAILED`終了する長い一連のエラーメッセージがリリースコンパイルに失敗した場合は、追加の**タッチ引数**を追加`-gcc_flags="-framework OpenAL"`してみてください。

[![](troubleshooting-images/mtouch01.png "タスクの MTouch 実行")](troubleshooting-images/mtouch01.png#lightbox)

また、追加の`--bitcode=asmonly` **タッチ引数**にを含め、リンカーオプションを **[すべてリンク]** に設定して、クリーンコンパイルを実行する必要があります。

### <a name="itms-90471-error-the-large-icon-is-missing"></a>ITMS-90471 エラー。 大きいアイコンがありません

"ITMS-90471 エラー" という形式のメッセージが表示された場合。 TvOS アプリをリリース用の Apple TV App Store に送信しようとしているときに、大きいアイコンがありません。次のことを確認してください。

1. [アプリアイコン](~/ios/tvos/app-fundamentals/icons-images.md#App-Icons)のドキュメントを使用して、作成し`Assets.car`たファイルに大きなアイコン資産が含まれていることを確認します。
2. 最終的なアプリケーションバンドルで`Assets.car` 、[アイコンとイメージの操作](~/ios/tvos/app-fundamentals/icons-images.md)に関するドキュメントに記載されているファイルが含まれていることを確認します。

### <a name="invalid-bundle--an-app-that-supports-game-controllers-must-also-support-the-apple-tv-remote"></a>無効なバンドル–ゲームコントローラーをサポートするアプリは、Apple TV リモコンもサポートする必要があります。

または 

### <a name="invalid-bundle--apple-tv-apps-with-the-gamecontroller-framework-must-include-the-gcsupportedgamecontrollers-key-in-the-apps-infoplist"></a>バンドルが無効です– GameController フレームワークを使用する Apple TV アプリでは、GCSupportedGameControllers キーをアプリの情報に含める必要があります。 plist

ゲームコントローラーを使用してゲームプレイを強化し、ゲームで immersion を実現できます。 また、ユーザーがリモートとコントローラーを切り替える必要がないように、標準の Apple TV インターフェイスを制御するために使用することもできます。

ゲームコントローラーがサポートされている tvOS アプリを Apple TV App store に送信しているときに、次の形式でエラーメッセージが表示される場合。


_"アプリ名" の最新の配信に関する1つ以上の問題が検出されました。配信は成功しましたが、次の配信で次の問題を修正する必要があります:_

_無効なバンドル–ゲームコントローラーをサポートするアプリは、Apple TV リモコンもサポートする必要があります。_

または 

_バンドルが無効です– GameController フレームワークを使用した Apple TV アプリでは、GCSupportedGameControllers キーをアプリの情報 plist に含める必要があります。_

この問題を解決するには、アプリケーションの`GCMicroGamepad` `Info.plist`ファイルに siri リモート () のサポートを追加します。 Apple は、Siri リモートをターゲットにするために、マイクロゲームコントローラーのプロファイルを追加しました。 たとえば、次のキーを含めます。

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
> Bluetooth ゲームコントローラーは、エンドユーザーが行うことができるオプションの購入で、アプリはユーザーに購入を強制することはできません。 アプリでゲームコントローラーがサポートされている場合は、すべての Apple TV ユーザーがゲームを利用できるように、Siri リモートもサポートする必要があります。

詳細については、 [Siri リモートおよび Bluetooth コントローラー](~/ios/tvos/platform/remote-bluetooth.md)のドキュメントの「[ゲームコントローラーの操作](~/ios/tvos/platform/remote-bluetooth.md#Working-with-Game-Controllers)」セクションを参照してください。

### <a name="incompatible-target-framework-netportable-versionv45-profileprofile78"></a>互換性のないターゲットフレームワーク:。NetPortable、Version = v1.0、Profile = Profile78

ポータブルクラスライブラリ (PCL) を tvOS プロジェクトに追加しようとすると、次のようなメッセージが表示される場合があります。

_互換性のないターゲットフレームワーク:。NetPortable、Version = v1.0、Profile = Profile78_

この問題を解決するには、という`Xamarin.TVOS.xml`名前の XML ファイルを次の内容で追加します。

```xml
<Framework Identifier="Xamarin.TVOS" MinimumVersion="1.0" Profile="*" DisplayName="Xamarin.TVOS"/>
```

次のパスに対して実行します。

```csharp
/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/xbuild-frameworks/.NETPortable/v4.5/Profile/Profile259/SupportedFrameworks/

```

パス内のプロファイル番号が PCL のプロファイル番号と一致している必要があることに注意してください。

このファイルを配置すると、tvOS プロジェクトに PCL ファイルを正常に追加できます。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマンインターフェイスガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリプログラミングガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
