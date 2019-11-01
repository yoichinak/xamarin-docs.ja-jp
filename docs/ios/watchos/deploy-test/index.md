---
title: Xamarin を使用した watchOS アプリのデプロイとテスト
description: このドキュメントでは、Xamarin でビルドされた watchOS アプリをデプロイしてテストする方法について説明します。 デプロイチェックリストを提供し、明示的およびワイルドカードアプリ Id について説明し、アプリグループについて確認します。
ms.prod: xamarin
ms.assetid: 98257399-E9B3-4BAB-9204-0E89117DEA6D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: b9b4d201e02d60bd6131c8693d9ac6a233e4fe10
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028339"
---
# <a name="deploying-and-testing-watchos-apps-with-xamarin"></a>Xamarin を使用した watchOS アプリのデプロイとテスト

## <a name="deployment-checklist"></a>配置のチェックリスト

テストウォッチにデプロイするか、アプリストアにアップロードするかにかかわらず、このページの手順を完了する必要があります。

- **IOS デベロッパーセンター**で、次のようにします。
  - [アプリ id](#App_IDs)が作成されました。
  - [アプリグループ](#App_Groups)が構成されている (必要な場合)。
  - 配布プロビジョニングプロファイルが作成されました

- ソリューションで次のようにします。

  - [バンドル id とプロジェクト参照](~/ios/watchos/get-started/installation.md)が設定されていることを確認します。
  - アイコンが[正しく構成](~/ios/watchos/app-fundamentals/icons.md)されていることを確認します。
  - すべてのプロジェクトでバンドルバージョン番号が一致していることを確認します。
  - アプリグループの**権利**を構成します (必要な場合)。

- 次に、指示に従って操作します。
  - [テストのために Apple Watch に配置](~/ios/watchos/deploy-test/device.md)します。
  - [App Store にアップロード](~/ios/watchos/deploy-test/appstore.md)します。

<a name="App_IDs"/>

## <a name="app-ids"></a>アプリ Id

[セットアップ手順](~/ios/watchos/get-started/installation.md)で説明したように、Watch アプリ内の3つのプロジェクトには、次のような関連するバンドル id があります。

- Xamarin. iOS 統合プロジェクト-`com.xamarin.WatchKitCatalog`
- WatchKit 拡張機能プロジェクト-`com.xamarin.WatchKitCatalog.watchkitextension`
- アプリプロジェクトを見る-`com.xamarin.WatchKitCatalog.watchkitapp`

3つのプロジェクトでは、それぞれに対して明示的なアプリ Id を使用するか、ワイルドカードアプリ ID を使用して、一致するディストリビューションプロビジョニングプロファイルが必要です。

### <a name="explicit-app-ids"></a>明示的なアプリ Id

各プロジェクトのバンドル ID (iOS デベロッパーセンターでは、次のように表示されます) の**アプリ id**を作成します。

![IOS デベロッパーセンターのバンドル Id](images/appids-specific-sml.png)

アプリ Id を作成または構成するときは、アプリに必要な特定の機能を必ず有効にしてください。 これには、プッシュ通知やアプリグループなどがあります。

各アプリ ID の配布プロビジョニングプロファイルを作成する必要があります。

### <a name="wildcard-app-id"></a>ワイルドカードアプリ ID

または、`com.xamarin.*`など、3つのプロジェクトすべてに一致するワイルドカード**アプリ ID**を作成することもできます。

一部の機能は、ワイルドカードアプリ ID (プッシュ通知など) では使用できないことに注意してください。 アプリがこれらの機能を必要とする場合は、明示的なアプリ Id を作成する必要があります。

配布については、ワイルドカードアプリ ID の配布プロビジョニングプロファイルを1つだけ作成する必要があります。

<a name="App_Groups" />

## <a name="app-groups"></a>アプリ グループ

アプリグループを使用して、iOS アプリと Watch 拡張機能の間でデータを共有できます。 次のソリューションがあることを確認する必要があります。

- **アプリグループ**は、Apple Developer Portal の**証明書、識別子 & プロファイル**セクションで構成されています。

- IOS アプリと Watch 拡張機能の**アプリ id**および**権利 plist**の*両方*で有効にされた**アプリグループ**(および**アプリグループ ID**)。

### <a name="certificates-identifiers--profiles"></a>証明書、識別子 & プロファイル

アプリグループを使用するには、 **[アプリグループ]** 画面でエントリを作成します。 次の例では、グループの名前は、アプリ Id に対して一般的に使用されるのと同じ逆引き DNS スタイルで指定されていますが、`group.` のプレフィックス (必須) です。

![識別子](images/appgroups-new-sml.png)

アプリグループが一覧に表示されます。

![識別子の一覧](images/appgroups-setup-sml.png)

作成したグループは、**アプリ ID**構成で参照できます。 IOS アプリと Watch 拡張機能の両方の**アプリ id**を忘れずに含めてください。

![使用可能な構成](images/appgroups-sml.png)

Apple Watch アプリ ID ではアプリグループ**を有効にしないでください。** ウォッチ自体で有効にする必要はありません。

### <a name="entitlementsplist"></a>Entitlements.plist

一部のアプリ機能 ( アプリグループ) では、権利を設定する必要があります。
をダブルクリックして、次のプロジェクトで**権利の plist**ファイルを編集します。

- iOS アプリプロジェクト
- 拡張機能プロジェクトのウォッチ

である必要があります。![権利の plist エディター](images/entitlements-plist-sml.png)

Watch アプリプロジェクトでは、権利**を有効にしないでください。** ウォッチ自体で有効にする必要はありません。

## <a name="related-links"></a>関連リンク

- [Apple WatchKit 提出ガイド](https://developer.apple.com/app-store/watch/)
