---
title: クロスプラットフォーム アプリケーションの構築
description: 'このセクションには、Xamarin 開発プラットフォームのモバイル アプリを設計して、テストし、さまざまなアプリ ストアに展開する Xamarin のしくみを理解する: を使用してアプリケーションを構築する方法が概要と、6 つの部分について説明します。'
ms.prod: xamarin
ms.assetid: 442FC40A-84DD-A218-0D15-EAD86594B6D7
author: asb3993
ms.author: amburns
ms.date: 01/28/2016
ms.openlocfilehash: 683400e24844308769f0562552641216d45e7d11
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61276368"
---
# <a name="building-cross-platform-applications"></a>クロスプラットフォーム アプリケーションの構築

クロス プラットフォーム モバイル アプリケーション間でコードを共有するための 2 つのオプションがあります。資産のプロジェクトとポータブル クラス ライブラリを共有します。 これらのオプションは[ここで説明した](~/cross-platform/app-fundamentals/code-sharing.md); の詳細について[ポータブル クラス ライブラリ](~/cross-platform/app-fundamentals/pcl.md)と[共有プロジェクト](~/cross-platform/app-fundamentals/shared-projects.md)も使用します。

<a name="Sections" />

 [概要](~/cross-platform/app-fundamentals/building-cross-platform-applications/overview.md)

 [パート 1-Xamarin モバイル プラットフォームを理解します。](~/cross-platform/app-fundamentals/building-cross-platform-applications/understanding-the-xamarin-mobile-platform.md)

 [パート 2-アーキテクチャ](~/cross-platform/app-fundamentals/building-cross-platform-applications/architecture.md)

 [パート 3-Xamarin クロス プラットフォーム ソリューションの設定](~/cross-platform/app-fundamentals/building-cross-platform-applications/setting-up-a-xamarin-cross-platform-solution.md)

 [パート 4 – 複数のプラットフォームを処理します。](~/cross-platform/app-fundamentals/building-cross-platform-applications/platform-divergence-abstraction-divergent-implementation.md)

 [パート 5 – 実用的なコード共有戦略](~/cross-platform/app-fundamentals/building-cross-platform-applications/practical-code-sharing-strategies.md)

 [パート 6 - テストと App Store の承認](~/cross-platform/app-fundamentals/building-cross-platform-applications/testing-and-app-store-approvals.md)

 <a name="Cross-Platform_Mobile_Application_Case_Studies" />

## <a name="case-studies"></a>ケース スタディ

このドキュメントで概説する原則は、サンプル アプリケーションで実際に配置されます*Tasky*、だけでなく[既成のアプリケーション](https://xamarin.com/prebuilt)など[Xamarin CRM](https://xamarin.com/prebuilt/#xamarincrm)します。

 <a name="Tasky" />

### <a name="tasky"></a>Tasky

Tasky は、iOS、Android、Windows Phone 向けの簡単な to do リスト アプリケーションです。
Xamarin によるクロス プラットフォーム アプリケーションの作成の基本について説明し、ローカルの SQLite データベースを使用します。

 [![tasky 一覧](images/iphone-list-sml.png)](images/iphone-list.png#lightbox) [ ![tasky 一覧](images/iphone-list-sml.png)](images/iphone-list.png#lightbox)

読み取り、 [Tasky のケース スタディ](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)します。

## <a name="summary"></a>まとめ

このセクションでは、Xamarin のアプリケーションの開発ツールを紹介し、複数のモバイル プラットフォームを対象とするアプリケーションを構築する方法について説明します。

複数のプラットフォームで、階層型アーキテクチャが再利用のためには、その構造体コードについて説明し、そのアーキテクチャ内で使用できるさまざまなソフトウェア パターンについて説明します。

(ファイルおよびネットワークの操作) のような一般的なアプリケーション機能とそれらをクロスプラット フォーム対応の方法で構築する方法の例が提供されます。

最後に、その簡単なテスト、について説明し、アクションにこれらの原則を出力するケース スタディへの参照を提供します。

## <a name="related-links"></a>関連リンク

- [コード共有のオプション](~/cross-platform/app-fundamentals/code-sharing.md)
- [ケース スタディ:Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Tasky サンプル アプリ (github)](https://developer.xamarin.com/samples/mobile/TaskyPortable/)
- [Xamarin モバイル アプリケーションの開発:クロス プラットフォームC#と Xamarin.Forms の基礎 (Amazon)](http://www.amazon.com/Xamarin-Mobile-Application-Development-Cross-Platform/dp/1484202155/)
- [によるモバイル開発C#によって Greg 足かせを外す (o ' reilly)](http://shop.oreilly.com/product/0636920024002.do)
- [プロフェッショナルなクロス プラットフォーム モバイル開発C#Scott Olson、John Hunter、Ben Horgen、Kenny お客さん (Wrox)](http://www.wrox.com/WileyCDA/WroxTitle/Professional-Cross-Platform-Mobile-Development-in-C-.productCd-1118157702.html)
