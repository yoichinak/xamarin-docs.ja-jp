---
title: クロス プラットフォーム アプリケーションの構築
description: このセクションには、プラットフォームを使用して、Xamarin 開発 –、モバイル アプリを設計しテストをさまざまなアプリ ストアへの展開での Xamarin の動作を理解することからアプリケーションを構築する方法が概要に加えて、6 つの部分について説明します。
ms.prod: xamarin
ms.assetid: 442FC40A-84DD-A218-0D15-EAD86594B6D7
author: asb3993
ms.author: amburns
ms.date: 01/28/2016
ms.openlocfilehash: fba13ab921949cd2361e78535d5ffc96952a1336
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/10/2018
---
# <a name="sharing-code-options"></a>コード共有のオプション

クロス プラットフォーム モバイル アプリケーション間でコードを共有するための 2 つのオプションがあります: 共有アセット プロジェクト、ポータブル クラス ライブラリです。 これらのオプションは[ここで説明した](~/cross-platform/app-fundamentals/code-sharing.md); の詳細について[ポータブル クラス ライブラリ](~/cross-platform/app-fundamentals/pcl.md)と[共有プロジェクト](~/cross-platform/app-fundamentals/shared-projects.md)も利用できます。

<a name="Sections" />

## <a name="building-cross-platform-mobile-apps"></a>クロス プラットフォームのモバイル アプリの構築

 [概要](~/cross-platform/app-fundamentals/building-cross-platform-applications/overview.md)

 [パート 1: Xamarin モバイル プラットフォームを理解します。](~/cross-platform/app-fundamentals/building-cross-platform-applications/understanding-the-xamarin-mobile-platform.md)

 [パート 2 – アーキテクチャ](~/cross-platform/app-fundamentals/building-cross-platform-applications/architecture.md)

 [パート 3 – Xamarin クロス プラットフォーム ソリューションを設定します。](~/cross-platform/app-fundamentals/building-cross-platform-applications/setting-up-a-xamarin-cross-platform-solution.md)

 [パート 4: 複数のプラットフォームを処理します。](~/cross-platform/app-fundamentals/building-cross-platform-applications/platform-divergence-abstraction-divergent-implementation.md)

 [パート 5 – 実用的なコードの戦略を共有](~/cross-platform/app-fundamentals/building-cross-platform-applications/practical-code-sharing-strategies.md)

 [パート 6 - テストと App Store の承認](~/cross-platform/app-fundamentals/building-cross-platform-applications/testing-and-app-store-approvals.md)

 <a name="Cross-Platform_Mobile_Application_Case_Studies" />


## <a name="case-studies"></a>ケース スタディ

このドキュメントで説明した原則は、サンプル アプリケーションで実際に配置されます*Tasky*、だけでなく[既成のアプリケーション](https://xamarin.com/prebuilt)など[Xamarin CRM](https://xamarin.com/prebuilt/#xamarincrm)です。

 <a name="Tasky" />


### <a name="tasky"></a>Tasky

Tasky は、iOS、Android および Windows Phone 用の単純なタスク リスト アプリケーションです。
Xamarin を使用したクロス プラットフォーム アプリケーションの作成の基本について説明し、ローカル SQLite データベースを使用します。

 [![tasky リスト](images/iphone-list-sml.png)](images/iphone-list.png#lightbox) [ ![tasky 一覧](images/iphone-list-sml.png)](images/iphone-list.png#lightbox)

読み取り、 [Tasky のケース スタディ](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)です。


## <a name="summary"></a>まとめ

このセクションでは、Xamarin のアプリケーションの開発ツールを紹介し、複数のモバイル プラットフォームを対象とするアプリケーションをビルドする方法について説明します。

複数層のアーキテクチャが、複数のプラットフォーム間で再利用するためには、その構造体コードについて説明し、そのアーキテクチャ内で使用できるさまざまなソフトウェアのパターンについて説明します。

(ファイル サービスおよびネットワーク操作) のような一般的なアプリケーション機能とそれらをビルドしてクロスプラット フォームの方法で方法の例が示されています。

最後に、その簡単なテスト、について説明し、アクションにこれらの原則を置くケース スタディへの参照を提供します。



## <a name="related-links"></a>関連リンク

- [コード共有のオプション](~/cross-platform/app-fundamentals/code-sharing.md)
- [ケース スタディ: Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Tasky サンプル アプリ (github)](https://developer.xamarin.com/samples/mobile/TaskyPortable/)
- [Xamarin のモバイル アプリケーションの開発: クロスプラット フォームの c# および Xamarin.Forms 基礎](http://www.amazon.com/Xamarin-Mobile-Application-Development-Cross-Platform/dp/1484202155/))
- [Greg で c# を使用したモバイル開発足かせを外す (◆ セグ: 前)](http://shop.oreilly.com/product/0636920024002.do)
- [Scott Olson、John ハンター、佐藤さん Horgen、Kenny 行く (Wrox) での c# でのプロフェッショナル向けのクロスプラット フォーム モバイル開発](http://www.wiley.com/WileyCDA/WileyTitle/productCd-1118157702.html)
