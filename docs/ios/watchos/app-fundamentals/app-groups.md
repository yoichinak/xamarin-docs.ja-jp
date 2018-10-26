---
title: WatchOS アプリ グループが Xamarin での操作
description: このドキュメントでは、アプリのグループおよび watchOS アプリケーションでの使用について説明します。 これには、要件、Entitlements.plist の考慮事項、およびデプロイのプロビジョニング アプリ グループを構成する方法について説明します。
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 6968606B-C287-424F-A321-2492E12BC0BB
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 78f6c03f73f0e4d8a74f826dd7bc25bbe325d545
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103674"
---
# <a name="working-with-watchos-app-groups-in-xamarin"></a>WatchOS アプリ グループが Xamarin での操作


アプリ グループを使用すると、さまざまなアプリケーション (またはアプリケーションとその拡張機能) から共有ファイルの保存場所にアクセスできます。 アプリ グループは次のようなデータに使用できます。

- Apple Watch[設定](~/ios/watchos/app-fundamentals/settings.md)します。
- 共有[NSUserDefaults](~/ios/watchos/app-fundamentals/parent-app.md#nsuserdefaults)します。
- 共有[ファイル](~/ios/watchos/app-fundamentals/parent-app.md#files)します。

## <a name="configure-an-app-group"></a>アプリ グループを構成します。

使用して共有の場所を構成、[アプリ グループ](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)で構成されており、**証明書, Identifiers & Profiles**セクションで[iOS デベロッパー センター](https://developer.apple.com/devcenter/ios/)します。 この値は、各プロジェクトの参照も必要があります**Entitlements.plist**します。

### <a name="provisioning"></a>プロビジョニング

アプリ グループは、バンドル ID は、通常、識別子を持っている、使用、`group.`プレフィックス。 たとえば、バンドル ID を使用してでした`com.xamarin.WatchSettings`およびアプリ グループ`group.com.xamarin.WatchSettings`します。

[![](app-groups-images/app-group-sml.png "バンドル ID com.xamarin.WatchSettings とアプリのグループ group.com.xamarin.WatchSettings を使用します。")](app-groups-images/app-group.png#lightbox)

### <a name="entitlementsplist"></a>Entitlements.plist

にプロビジョニング プロファイルを構成すると**アプリ グループを有効にする**で、 **Entitlements.plist**選択した ID を入力します。

[![](app-groups-images/entitlements-sml.png "Plist を構成し、ID を入力します")](app-groups-images/entitlements.png#lightbox)


### <a name="deployment"></a>配置

正しくアプリ グループを構成することを確認、[展開](~/ios/watchos/deploy-test/index.md#App_Groups)プロビジョニングします。


詳細についてを参照してください、[アプリ グループ機能](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md)ドキュメント。


## <a name="related-links"></a>関連リンク

- [Apple のデータを含むアプリを共有します。](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
- [Apple のアプリ グループのドキュメント](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)
