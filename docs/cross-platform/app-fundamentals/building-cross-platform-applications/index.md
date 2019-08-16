---
title: クロスプラットフォーム アプリケーションの構築
description: このセクションでは、概要と6つの部分について説明します。 xamarin の開発プラットフォームを使用してアプリケーションを構築する方法については、Xamarin の機能を理解してモバイルアプリを設計し、さまざまなアプリストアにテストして展開する方法について説明します。
ms.prod: xamarin
ms.assetid: 442FC40A-84DD-A218-0D15-EAD86594B6D7
author: asb3993
ms.author: amburns
ms.date: 01/28/2016
ms.openlocfilehash: 4485f25ac2f81747132ccd92d73c52118f70e545
ms.sourcegitcommit: f255aa286bd52e8a80ffa620c2e93c97f069f8ec
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/31/2019
ms.locfileid: "68680927"
---
# <a name="building-cross-platform-applications"></a>クロスプラットフォーム アプリケーションの構築

クロスプラットフォームモバイルアプリケーション間でコードを共有するには、次の2つのオプションがあります。共有アセットプロジェクトとポータブルクラスライブラリ。 これらのオプションについては、[こちら](~/cross-platform/app-fundamentals/code-sharing.md)を参照してください。[ポータブルクラスライブラリ](~/cross-platform/app-fundamentals/pcl.md)と[共有プロジェクト](~/cross-platform/app-fundamentals/shared-projects.md)の詳細については、こちらも参照してください。

<a name="Sections" />

 [概要](~/cross-platform/app-fundamentals/building-cross-platform-applications/overview.md)

 [パート 1-Xamarin Mobile Platform について](~/cross-platform/app-fundamentals/building-cross-platform-applications/understanding-the-xamarin-mobile-platform.md)

 [パート 2-アーキテクチャ](~/cross-platform/app-fundamentals/building-cross-platform-applications/architecture.md)

 [パート3– Xamarin クロスプラットフォームソリューションの設定](~/cross-platform/app-fundamentals/building-cross-platform-applications/setting-up-a-xamarin-cross-platform-solution.md)

 [パート 4-複数のプラットフォームを扱う](~/cross-platform/app-fundamentals/building-cross-platform-applications/platform-divergence-abstraction-divergent-implementation.md)

 [パート5–実際のコード共有戦略](~/cross-platform/app-fundamentals/building-cross-platform-applications/practical-code-sharing-strategies.md)

 [パート 6 - テストと App Store の承認](~/cross-platform/app-fundamentals/building-cross-platform-applications/testing-and-app-store-approvals.md)

 <a name="Cross-Platform_Mobile_Application_Case_Studies" />

## <a name="case-studies"></a>ケーススタディ

このドキュメントで説明されている原則は、サンプルアプリケーション*Tasky*と、 [Xamarin CRM](https://xamarin.com/prebuilt/#xamarincrm)のような[事前に構築](https://xamarin.com/prebuilt)されたアプリケーションについて説明します。

 <a name="Tasky" />

### <a name="tasky"></a>Tasky

Tasky は、iOS、Android、および Windows Phone 用の単純な to do list アプリケーションです。
Xamarin を使用してクロスプラットフォームアプリケーションを作成し、ローカルの SQLite データベースを使用する方法の基本について説明します。

 tasky リスト[ ![](images/iphone-list-sml.png)](images/iphone-list.png#lightbox) tasky リスト[ ![](images/iphone-list-sml.png)](images/iphone-list.png#lightbox)

[Tasky のケーススタディ](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)を参照してください。

## <a name="summary"></a>Summary

このセクションでは、Xamarin のアプリケーション開発ツールについて説明し、複数のモバイルプラットフォームを対象とするアプリケーションを構築する方法について説明します。

複数のプラットフォームにわたって再利用するためのコードを構成するレイヤーアーキテクチャについて説明し、そのアーキテクチャで使用できるさまざまなソフトウェアパターンについて説明します。

一般的なアプリケーション機能 (ファイル操作やネットワーク操作など) と、それらをクロスプラットフォーム方式で構築する方法については、例を挙げて説明します。

最後に、テストについて簡単に説明し、これらの原則が動作するケーススタディへの参照を提供します。

## <a name="related-links"></a>関連リンク

- [コード共有のオプション](~/cross-platform/app-fundamentals/code-sharing.md)
- [ケーススタディ:Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Tasky サンプルアプリ (github)](https://docs.microsoft.com/samples/xamarin/mobile-samples/taskyportable/)
- [Xamarin モバイルアプリケーションの開発:クロスプラットフォームC#および Xamarin.Forms の基礎 (Amazon)](http://www.amazon.com/Xamarin-Mobile-Application-Development-Cross-Platform/dp/1484202155/)
- [Greg Shackles C#によるモバイル開発 (O'Reilly)](http://shop.oreilly.com/product/0636920024002.do)
- [Scott Olson、John Hunter、 C# Ben Horgen、Kenny Goers (Wrox) によるプロフェッショナルなクロスプラットフォームモバイル開発](http://www.wrox.com/WileyCDA/WroxTitle/Professional-Cross-Platform-Mobile-Development-in-C-.productCd-1118157702.html)
