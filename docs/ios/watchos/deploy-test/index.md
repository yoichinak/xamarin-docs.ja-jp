---
title: 展開して、Xamarin で watchOS アプリのテスト
description: このドキュメントでは、配置、および Xamarin でビルドされた watchOS アプリをテストする方法について説明します。 展開チェックリストを提供します、明示的な説明とワイルドカード アプリ Id を受け取ってアプリ グループを参照してください。
ms.prod: xamarin
ms.assetid: 98257399-E9B3-4BAB-9204-0E89117DEA6D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 7d626b8a968835813d87c93e3cead57a00c14000
ms.sourcegitcommit: 849bf6d1c67df943482ebf3c80c456a48eda1e21
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2018
ms.locfileid: "51528599"
---
# <a name="deploying-and-testing-watchos-apps-with-xamarin"></a>展開して、Xamarin で watchOS アプリのテスト

## <a name="deployment-checklist"></a>展開のチェックリスト

テスト Watch へのデプロイまたは、App Store にアップロードするかどうかは、このページで手順を完了する必要があります。

- **IOS デベロッパー センター**:
  - [アプリ Id](#App_IDs)が作成されています。
  - [アプリ グループ](#App_Groups)(必要な場合) を構成します。
  - 配布プロビジョニング プロファイルを作成

- で、解決方法。

  - 確認、[プロジェクト参照のバンドル Id と](~/ios/watchos/get-started/installation.md)設定されます。
  - チェック アイコン[正しく構成されている](~/ios/watchos/app-fundamentals/icons.md)します。
  - すべてのプロジェクトでバンドル バージョン番号の一致を確認します。
  - 構成、 **Entitlements.plist**アプリ グループ (必要な場合)。

* 次の手順。
  - [テストのために、Apple Watch デプロイ](~/ios/watchos/deploy-test/device.md)、または
  - [App Store にアップロード](~/ios/watchos/deploy-test/appstore.md)します。

<a name="App_IDs"/>

## <a name="app-ids"></a>アプリ Id

説明したように、[セットアップ手順](~/ios/watchos/get-started/installation.md)、Watch アプリ内のすべての 3 つのプロジェクトがバンドル Id をなどに関連します。

- Xamarin.iOS 統合プロジェクト- `com.xamarin.WatchKitCatalog`
- WatchKit 拡張機能プロジェクト- `com.xamarin.WatchKitCatalog.watchkitextension`
- Watch アプリ プロジェクトの場合- `com.xamarin.WatchKitCatalog.watchkitapp`

次の 3 つのすべてのプロジェクトに必要な配布プロビジョニング プロファイルが一致する、いずれかを使用して明示的にアプリ Id ごとに、またはワイルドカード アプリ id。

### <a name="explicit-app-ids"></a>明示的なアプリ Id

作成、**アプリ ID** (iOS デベロッパー センターで次のようになりますが) を各プロジェクトのバンドル id:

![IOS デベロッパー センター内のバンドル Id](images/appids-specific-sml.png)

を作成またはアプリ Id を構成するときに、アプリに必要な特定の機能を有効にしてください。 これには、プッシュ通知とアプリ グループを含めることができます。

アプリ ID ごとに配布プロビジョニング プロファイルを作成する必要があります。

### <a name="wildcard-app-id"></a>ワイルドカード アプリ ID

ワイルドカードを作成する代わりに、**アプリ ID**などすべての 3 つのプロジェクトに一致する`com.xamarin.*`します。

プッシュ通知) などのワイルドカード アプリ ID の一部の機能を使用できないことに注意してください。 アプリがこれらの機能が必要な場合は、明示的なアプリ Id を作成してください。

配布、だけする必要があります 1 つの配布プロビジョニング プロファイルを作成、ワイルドカード アプリ id。

<a name="App_Groups" />

## <a name="app-groups"></a>アプリ グループ

アプリ グループを使用して、iOS アプリと Watch Extension 間でデータを共有することができます。 ソリューションが持つことを確認してください。

- 構成されている、**アプリ グループ**Apple Developer Portal で**証明書, Identifiers & Profiles**セクション。

- 有効になっている**アプリ グループ**(提供されていると、**アプリ グループ ID**) で*両方*iOS App と Watch Extension の**アプリ ID**と**Entitlements.plist**します。

### <a name="certificates-identifiers--profiles"></a>証明書, Identifiers & Profiles

アプリ グループを使用するには、内のエントリを作成、**アプリ グループ**画面。 アプリの id がよく使用される同じ逆引き DNS スタイルを使用して次の例で、グループの名前、`group.`プレフィックス (これは必須)。

![識別子](images/appgroups-new-sml.png)

アプリ グループは、一覧に表示されます。

![識別子の一覧](images/appgroups-setup-sml.png)

グループを作成した後で参照できる、**アプリ ID**構成します。 両方、iOS App と Watch Extension を含めることに注意してください**アプリ Id**します。

![使用可能な構成](images/appgroups-sml.png)

**いない**Apple Watch アプリ ID でアプリ グループを有効にします。 Watch 自体で有効にする必要はありません。

### <a name="entitlementsplist"></a>Entitlements.plist

一部のアプリ機能 (例: アプリ グループ) では、権利を設定する必要があります。
編集する をダブルクリック、 **Entitlements.plist**これらのプロジェクト ファイル。

- iOS アプリ プロジェクト
- ウォッチ拡張機能プロジェクト

.![Entitlements.plist エディター](images/entitlements-plist-sml.png)

**いない**Watch アプリ プロジェクトで権利を有効にします。 Watch 自体で有効にする必要はありません。

## <a name="related-links"></a>関連リンク

- [Apple WatchKit 送信ガイド](https://developer.apple.com/app-store/watch/)
