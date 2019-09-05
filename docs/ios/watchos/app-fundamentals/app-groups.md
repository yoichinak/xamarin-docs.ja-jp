---
title: Xamarin での watchOS App Groups の使用
description: このドキュメントでは、watchOS アプリケーションでのアプリグループとその使用について説明します。 アプリグループの構成方法、プロビジョニングの要件、権利の考慮事項、および展開について説明します。
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 6968606B-C287-424F-A321-2492E12BC0BB
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: c75db8bd29b7a57c46610abdd5e4024938fc9e1b
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70280330"
---
# <a name="working-with-watchos-app-groups-in-xamarin"></a>Xamarin での watchOS App Groups の使用


アプリ グループを使用すると、さまざまなアプリケーション (またはアプリケーションとその拡張機能) から共有ファイルの保存場所にアクセスできます。 アプリ グループは次のようなデータに使用できます。

- Apple Watch[設定](~/ios/watchos/app-fundamentals/settings.md)。
- 共有[Nsuserdefaults](~/ios/watchos/app-fundamentals/parent-app.md#nsuserdefaults)。
- 共有[ファイル](~/ios/watchos/app-fundamentals/parent-app.md#files)。

## <a name="configure-an-app-group"></a>アプリグループを構成する

共有の場所は、 [IOS デベロッパーセンター](https://developer.apple.com/devcenter/ios/)の「**証明書、識別子 & プロファイル**」セクションで構成されている[アプリグループ](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)を使用して構成されます。 この値は、各プロジェクトの権利でも参照される必要があります。 **plist**です。

### <a name="provisioning"></a>プロビジョニング

アプリグループには識別子があります。これは通常、 `group.`プレフィックスを持つバンドル ID です。 たとえば、バンドル ID `com.xamarin.WatchSettings`とアプリグループ`group.com.xamarin.WatchSettings`を使用できます。

[![](app-groups-images/app-group-sml.png "バンドル ID WatchSettings とアプリグループ WatchSettings を使用します。この場合、")](app-groups-images/app-group.png#lightbox)

### <a name="entitlementsplist"></a>Entitlements.plist

プロビジョニングプロファイルを構成するだけでなく、 **[権利]** の [**アプリグループ] を有効に**し、選択した ID を入力します。

[![](app-groups-images/entitlements-sml.png "Plist を構成して ID を入力する")](app-groups-images/entitlements.png#lightbox)


### <a name="deployment"></a>展開

[デプロイ](~/ios/watchos/deploy-test/index.md#App_Groups)のプロビジョニングで、アプリグループが正しく構成されていることを確認します。


詳細についてを参照してください、[アプリ グループ機能](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md)ドキュメント。


## <a name="related-links"></a>関連リンク

- [Apple のデータを含むアプリを共有します。](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
- [Apple の App Group ドキュメント](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)
