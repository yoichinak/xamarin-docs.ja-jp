---
title: "アプリ グループの操作"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 27ce6c48c5bca69605773eb5ef5637201b9ce6c5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-app-groups"></a>アプリ グループの操作


アプリ グループを使用すると、さまざまなアプリケーション (またはアプリケーションとその拡張機能) から共有ファイルの保存場所にアクセスできます。 アプリ グループは次のようなデータに使用できます。

- Apple Watch[設定](~/ios/watchos/app-fundamentals/settings.md)です。
- 共有[NSUserDefaults](~/ios/watchos/app-fundamentals/parent-app.md#nsuserdefaults)です。
- 共有[ファイル](~/ios/watchos/app-fundamentals/parent-app.md#files)です。

## <a name="configure-an-app-group"></a>アプリ グループを構成します。

使用して共有の場所を構成、[アプリ グループ](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)で構成されて、**証明書、識別子、およびプロファイル**セクションで[iOS Dev Center](https://developer.apple.com/devcenter/ios/)です。 この値は、各プロジェクトの参照も必要があります**Entitlements.plist**です。

### <a name="provisioning"></a>プロビジョニング

アプリ グループは、バンドル ID は、通常、識別子を持っているはで、`group.`プレフィックス。 たとえば、バンドル ID を使用しておでした`com.xamarin.WatchSettings`とアプリ グループ`group.com.xamarin.WatchSettings`です。

[ ![](app-groups-images/app-group-sml.png "バンドル ID com.xamarin.WatchSettings とアプリ グループ group.com.xamarin.WatchSettings を使用します。")](app-groups-images/app-group.png)

### <a name="entitlementsplist"></a>Entitlements.plist

プロビジョニング プロファイルを構成するだけでなく**アプリ グループを有効にする**で、 **Entitlements.plist**し、選択した ID を入力してください。

[ ![](app-groups-images/entitlements-sml.png "Plist を構成し、ID を入力")](app-groups-images/entitlements.png)


### <a name="deployment"></a>配置

正しくアプリ グループを構成することを確認、[展開](~/ios/watchos/deploy-test/index.md#app-groups)プロビジョニングします。


詳細についてを参照してください、[アプリ グループ機能](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md)ドキュメント。


## <a name="related-links"></a>関連リンク

- [Apple のアプリを含むデータを共有します。](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
- [Apple の App グループ ドキュメント](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)
