---
title: 展開して、Xamarin を使用した watchOS アプリのテスト
description: このドキュメントでは、配置、および Xamarin でビルドされた watchOS アプリをテストする方法について説明します。 配置のチェックリストを提供して、明示的なについて説明しますとワイルドカード アプリ Id、し、アプリ グループを確認します。
ms.prod: xamarin
ms.assetid: 98257399-E9B3-4BAB-9204-0E89117DEA6D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 778583456e74bb7ed3a85dce96bcdbc487aef57a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790943"
---
# <a name="deploying-and-testing-watchos-apps-with-xamarin"></a>展開して、Xamarin を使用した watchOS アプリのテスト

## <a name="deployment-checklist"></a>配置のチェックリスト

テスト ウォッチへの展開が、アプリ ストアにアップロードするか、か、このページで手順を完了する必要があります。

- **IOS Dev Center**:
  - [アプリ Id](#App_IDs)が作成されています。
  - [アプリ グループ](#App_Groups)(必要な場合) を構成します。
  - 配布プロビジョニング プロファイルを作成

- で、解決方法。

  - 確認、[プロジェクト参照のバンドル Id と](~/ios/watchos/get-started/installation.md)設定されます。
  - アイコンを確認して[正しく構成されている](~/ios/watchos/app-fundamentals/icons.md)です。
  - すべてのプロジェクトでバンドル バージョン番号の一致を確認してください。
  - 構成、 **Entitlements.plist**アプリ グループ (必要な場合)。

* 次の指示に従います。
  - [Apple Watch をテストするための展開](~/ios/watchos/deploy-test/device.md)、または
  - [アプリ ストアにアップロード](~/ios/watchos/deploy-test/appstore.md)です。

<a name="App_IDs"/>

## <a name="app-ids"></a>アプリ Id

説明したように、[セットアップ手順](~/ios/watchos/get-started/installation.md)、Watch アプリ内のすべての 3 つのプロジェクトに関連するは、バンドル Id など。

- Xamarin.iOS 統合プロジェクト- `com.xamarin.WatchKitCatalog`
- WatchKit 拡張機能プロジェクトの場合- `com.xamarin.WatchKitCatalog.watchkitextension`
- Watch アプリ プロジェクト `com.xamarin.WatchKitCatalog.watchkitapp`

次の 3 つのすべてのプロジェクトに必要な配布プロビジョニング プロファイルが一致する、またはを使用して明示的にアプリ Id ごとに、ワイルドカード アプリ id。

### <a name="explicit-app-ids"></a>明示的なアプリの Id

作成、**アプリ ID**各各プロジェクトのバンドル id (これは、iOS Dev Center で次のようになります)。

![IOS デベロッパー センター内のバンドル Id](images/appids-specific-sml.png)

を作成したり、アプリ Id を構成する場合は、アプリに必要な特定の機能を有効にしてください。 これには、プッシュ通知とアプリ グループを含めることができます。

アプリ ID ごとに、分布のプロビジョニング プロファイルを作成する必要があります。

### <a name="wildcard-app-id"></a>ワイルドカード アプリケーション ID

また、ワイルドカードを作成することができます**アプリ ID**など、3 つすべてのプロジェクトに一致する`com.xamarin.*`です。

プッシュ通知) などのワイルドカード アプリ ID の一部の機能を使用できないことに注意してください。 アプリは、これらの機能を必要とする場合は、アプリは明示的な Id を作成してください。

ディストリビューションの場合のみ必要プロファイルを作成する 1 つの配布のプロビジョニングのワイルドカード アプリ id。

<a name="App_Groups" />

## <a name="app-groups"></a>アプリ グループ

アプリ グループを使用するには、iOS アプリとウォッチ拡張機能の間でデータを共有します。 ソリューションことを確認する必要があります。

- 構成されている、**アプリ グループ**Apple 開発者ポータルで**証明書、識別子、およびプロファイル**セクションです。

- 有効になっている**アプリ グループ**(提供されると、**アプリ グループ ID**) で*両方*iOS アプリと、ウォッチ拡張機能の**アプリ ID**と**Entitlements.plist**です。

### <a name="certificates-identifiers--profiles"></a>証明書、識別子、およびプロファイル

アプリ グループを使用するには、内のエントリを作成、**アプリ グループ**画面。 アプリ Id はよく使用される同じ逆引き DNS スタイルを使用して次の例で、グループの名前は、`group.`プレフィックス (必須)。

![識別子](images/appgroups-new-sml.png)

アプリ グループが一覧に表示されます。

![識別子の一覧](images/appgroups-setup-sml.png)

グループを作成した後で参照できる、**アプリ ID**構成します。 両方の iOS アプリ、およびウォッチ拡張機能を含めることに注意してください**のアプリ Id**です。

![使用可能な consifurations](images/appgroups-sml.png)

**いない**Apple Watch アプリ ID でアプリ グループを有効にします。 ウォッチ自体で有効にする必要はありません。

### <a name="entitlementsplist"></a>Entitlements.plist

一部のアプリ機能 (例です。 アプリ グループ) では、権利を設定する必要があります。
編集するにはダブルクリック、 **Entitlements.plist**これらのプロジェクト内のファイル。

- iOS アプリ プロジェクト
- ウォッチ拡張機能プロジェクト

である必要があります。![Entitlements.plist エディター](images/entitlements-plist-sml.png)

**いない**Watch アプリ プロジェクトで権利を有効にします。 ウォッチ自体で有効にする必要はありません。

## <a name="related-links"></a>関連リンク

- [Apple WatchKit 送信ガイド](https://developer.apple.com/app-store/watch/)
