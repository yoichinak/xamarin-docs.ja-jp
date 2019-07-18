---
title: Xamarin でビルドされた tvOS アプリのトラブルシューティング
description: この記事では、Xamarin でビルドされた tvOS アプリの開発中のトラブルシューティングに役立つさまざまなヒントを提供します。 これは、特定のエラーと既知の問題について説明します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 124E4953-4DFA-42B0-BCFC-3227508FE4A6
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 6830df267aa0b9c4f12fbd53520206ea94fc8a38
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67831890"
---
# <a name="troubleshooting-tvos-apps-built-with-xamarin"></a>Xamarin でビルドされた tvOS アプリのトラブルシューティング

_この記事では既知の問題が Xamarin の tvOS のサポートの使用中に発生する可能性があります。_

<a name="Known-Issues" />

## <a name="known-issues"></a>既知の問題

Xamarin の tvOS のサポートの現在のリリースには、次の既知の問題があります。

- **Mono フレームワーク**– Mono 4.3 Cryptography.ProtectedData は Mono 4.2 からのデータを復号化に失敗します。 NuGet パッケージは、エラーのため、復元に失敗する結果として、`Data unprotection failed`保護された NuGet ソースが構成されている場合。
    - **回避策**– Visual Studio for Mac は、NuGet パッケージ ソースの再パッケージの復元を試みる前にパスワード認証を使用するを追加する必要があります。
- **Visual Studio for Mac を使用したF#アドイン**– エラーの作成時に、 F# Windows での Android のテンプレート。 これは引き続き正常に機能 mac
- **Xamarin.Mac**を設定するターゲット フレームワークと Xamarin.Mac 統合テンプレート プロジェクトを実行する – `Unsupported`、ポップアップ`Could not connect to the debugger`場合があります。
    - **潜在的な回避策**–、安定チャネルで使用可能な Mono フレームワークのバージョンをダウン グレードします。
- **Xamarin Visual Studio および Xamarin.iOS** – Visual studio で、エラー WatchKit アプリケーションの配置と`The file ‘bin\iPhoneSimulator\Debug\WatchKitApp1WatchKitApp.app\WatchKitApp1WatchKitApp’ does not exist`場合があります。

いずれかのバグ レポートを参照してください[GitHub](https://github.com/xamarin/xamarin-macios/issues/new)します。

## <a name="troubleshooting"></a>トラブルシューティング

次のセクションでは、Xamarin.tvOS とそれらの問題の解決策を tvOS 9 を使用するときに発生する可能性がある既知の問題を一覧表示します。

### <a name="invalid-executable---the-executable-does-not-contain-bitcode"></a>無効な実行可能ファイルの実行可能ファイルは bitcode 含まれていません。

Xamarin.tvOS アプリを Apple TV App Store に送信しようとすると、フォームのエラー メッセージを表示可能性があります _「無効な実行可能ファイルの実行可能ファイルを含まない bitcode」_ します。

この問題を解決するには、次の操作を行います。

1. Visual Studio for Mac で Xamarin.tvOS プロジェクト ファイルを右クリックし、**ソリューション エクスプ ローラー**選択と**オプション**します。
2. 選択**tvOS ビルド**であることを確認し、**リリース**構成。 

    [![](troubleshooting-images/ts01.png "TvOS ビルド オプションを選択します。")](troubleshooting-images/ts01.png#lightbox)
3. 追加`--bitcode=asmonly`を**追加 mtouch 引数**フィールドでをクリックし、 **OK**ボタン。
4. アプリを再構築、**リリース**構成します。

### <a name="verifying-that-your-tvos-app-contains-bitcode"></a>確認、tvOS アプリを含む Bitcode

Xamarin.tvOS アプリ ビルドにはでビットコードが含まれていることを確認するには、ターミナル アプリを開き、次を入力します。

```csharp
otool -l /path/to/your/tv.app/tv
```

出力では、次のようなになります。

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

`addr` `size`異なるものになりますが、他のフィールドが同じであります。

確認する必要がありますも静的、サード パーティ (`.a`) tvOS ライブラリ (iOS ライブラリではない) に対してビルドされたライブラリを使用して、bitcode 情報も含まれますが、します。

アプリまたは有効なビットコードが含まれているライブラリ、 `size` 1 よりも大きくなります。 場所ライブラリできます bitcode マーカーをまだ有効なビットコードが含まれていない場合があります。 例えば:

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

違いに注意してください`size`で一覧表示されている例では、2 つのライブラリとの間の上を実行します。 Bitcode を有効になっていると Xcode のアーカイブ ビルドからライブラリを生成する必要があります (Xcode 設定`ENABLE_BITCODE`) このサイズの問題へのソリューションとして。

### <a name="apps-that-only-contain-the-arm64-slice-must-also-have-arm64-in-the-list-of-uirequireddevicecapabilities-in-infoplist"></a>Arm64 スライスにはのみが含まれているアプリでは、Info.plist で UIRequiredDeviceCapabilities の一覧では、"arm64"必要があります。

パブリケーションの Apple TV App Store にアプリを送信するときに、フォームでエラーが発生する可能性があります。

_"アプリのみ arm64 スライスが含まれている必要があります"arm64"info.plist UIRequiredDeviceCapabilities の一覧で"_

このような場合は、編集、`Info.plist`ファイルを開き、次のキーを使用していることを確認します。

```xml
<key>UIRequiredDeviceCapabilities</key>
<array>
    <string>arm64</string>
</array>
```

リリース用のアプリを再コンパイルし、iTunes Connect を再実行してください。

### <a name="task-mtouch-execution----failed"></a>タスクの"MTouch"の実行 - 失敗しました

MonoGame) などのサード パーティ ライブラリを使用しているし、長い一連ので終わるエラー メッセージが、リリースのコンパイルが失敗した場合`Task "MTouch" execution -- FAILED`を追加してみてください`-gcc_flags="-framework OpenAL"`を**追加タッチ引数**:

[![](troubleshooting-images/mtouch01.png "タスクの MTouch の実行")](troubleshooting-images/mtouch01.png#lightbox)

必要があります`--bitcode=asmonly`で、**追加タッチ引数**、リンカー オプション設定**リンクすべて**クリーン コンパイルを実行します。

### <a name="itms-90471-error-the-large-icon-is-missing"></a>ITMS 90471 エラーです。 大きいアイコンがありません。

"ITMS 90471 エラー形式でメッセージが発生した場合。 次チェック大きいアイコンがありません"リリースでは、Apple TV App Store に Xamarin.tvOS アプリを送信しているときにしてください。

1. 大きいアイコンのアセットが含まれていることを確認、`Assets.car`を使用して作成したファイルを[アプリ アイコン](~/ios/tvos/app-fundamentals/icons-images.md#App-Icons)ドキュメント。
2. 含まれるように、`Assets.car`からファイル、[アイコンとイメージを使用する](~/ios/tvos/app-fundamentals/icons-images.md)最終的なアプリケーション バンドルのドキュメント。

### <a name="invalid-bundle--an-app-that-supports-game-controllers-must-also-support-the-apple-tv-remote"></a>無効なバンドル-ゲーム コント ローラーをサポートするアプリを Apple TV をリモートにする必要がありますもはサポート

または 

### <a name="invalid-bundle--apple-tv-apps-with-the-gamecontroller-framework-must-include-the-gcsupportedgamecontrollers-key-in-the-apps-infoplist"></a>無効なバンドル-GameController フレームワークを使用した Apple TV のアプリは、アプリの Info.plist で GCSupportedGameControllers キーを含める必要があります。

ゲーム コント ローラーは、ゲームプレイを強化し、ゲームで immersion 感の提供に使用できます。 また、ユーザーがリモート インスタンスと、コント ローラー間で切り替えがあるないために、Apple TV の標準のインターフェイスを制御するためも使用できます。

Apple TV のアプリ ストアにゲーム コント ローラーのサポートと Xamarin.tvOS アプリを送信するエラー メッセージの形式で取得する場合、します。


_「アプリ名」の最近の配信を使用した 1 つまたは複数の問題が見つかりました。配信が成功した場合が、次の配信では、次の問題を修正することができます。_

_無効なバンドル-ゲーム コント ローラーをサポートするアプリには、Apple TV のリモート サポートもする必要があります。_

または 

_無効なバンドル-GameController フレームワークを使用した Apple TV のアプリは、アプリの Info.plist で GCSupportedGameControllers キーを含める必要があります。_

Siri のリモートのサポートを追加するソリューションは、(`GCMicroGamepad`) アプリの`Info.plist`ファイル。 マイクロ ゲーム コント ローラー プロファイルは apple Siri のリモートの対象に追加されました。 たとえば、次のキーを含めます。

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
> Bluetooth のゲーム コント ローラーには、エンドユーザーを使用する省略可能で購入した、アプリは、いずれかを購入するユーザーを強制できません。 アプリは、ゲーム コント ローラーをサポートする場合は、Siri のリモートもサポート ゲームが Apple TV のすべてのユーザーが使用できるようにする必要があります。

詳細についてを参照してください、[ゲーム コント ローラーの操作](~/ios/tvos/platform/remote-bluetooth.md#Working-with-Game-Controllers)のセクション、 [Siri のリモートと Bluetooth コント ローラー](~/ios/tvos/platform/remote-bluetooth.md)ドキュメント。

### <a name="incompatible-target-framework-netportable-versionv45-profileprofile78"></a>互換性のないターゲット フレームワーク: です。NetPortable、バージョン = v4.5、プロファイル Profile78 を =

Xamarin.tvOS プロジェクトにポータブル クラス ライブラリ (PCL) が含まれているフォームをメッセージを表示可能性があります。

_互換性のないターゲット フレームワーク: です。NetPortable、バージョン = v4.5、プロファイル Profile78 を =_

この問題を解決すると呼ばれる XML ファイルを追加`Xamarin.TVOS.xml`次の内容。

```xml
<Framework Identifier="Xamarin.TVOS" MinimumVersion="1.0" Profile="*" DisplayName="Xamarin.TVOS"/>
```

次のパス。

```csharp
/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/xbuild-frameworks/.NETPortable/v4.5/Profile/Profile259/SupportedFrameworks/

```

パス内のプロファイル数が、PCL のプロファイルの数を一致する必要がありますに注意してください。

このファイルに正常に Xamarin.tvOS プロジェクトを PCL ファイルを追加できる必要があります。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 用のアプリのプログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
