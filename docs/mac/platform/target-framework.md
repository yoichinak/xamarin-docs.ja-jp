---
title: '[対象とする Framework]'
description: この記事では、Xamarin.Mac の使用可能なターゲット フレームワーク (基本クラス ライブラリ) および Xamarin.Mac プロジェクトで使用する場合の影響について説明します。
ms.prod: xamarin
ms.assetid: AF21BE16-3F92-4121-AB4C-D51AC863D92D
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: 053cdd2dbfc7741257e6630e5b11b77b1055428e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="target-framework"></a>[対象とする Framework]

_この記事では、Xamarin.Mac の使用可能なターゲット フレームワーク (基本クラス ライブラリ) および Xamarin.Mac プロジェクトで使用する場合の影響について説明します。_

![-Target Xamarin.Mac のフレームワーク オプション](target-framework-images/select-target.png "-target Xamarin.Mac のフレームワーク オプション")

## <a name="background"></a>背景

すべての .NET プログラムまたはライブラリは、基本クラス ライブラリ (BCL) によって提供される機能に依存します。 この BCL には、すべての .NET 言語に組み込まれている一般的な機能を提供するアセンブリ、mscorlib、システム、System.Net.Http、および System.Xml などが含まれています。

長年にわたって開発には、複数の異なるバージョンの異なるユース ケース用に最適化された、この BCL がしました。 「デスクトップ」BCL には、アプリケーションの使用量を削減未使用のコードを削除する可能性のある他のユース ケースのすぎる重い紙 mobile について重点的に Api は、リンクすると、に対して安全を確保するときにライブラリの豊富なセットが含まれています。

これらさまざまなターゲット フレームワークのより重要な影響のいずれかがいるすべてのプログラムでアセンブリ*必要があります*BCL の互換性のあるアセンブリのターゲットです。 ない場合は場合のさまざまなバージョンに対してリンクされている 2 つのアセンブリを割り当てることも、 **System.dll** disagreeing について、指定された型のシグネチャ。 共有ライブラリできますいずれかのターゲット[.NET 標準 2](https://blog.xamarin.com/share-code-net-standard-2-0/)、共通のターゲット フレームワークまたは特定のターゲット フレームワークのサブセットであります。

次の 3 つのターゲット フレームワーク オプションはそれぞれ異なる利点とトレードオフ Xamarin.Mac できます。

- **最新**(以前のドキュメントで Mobile と呼ばれます): どのような高パフォーマンスとサイズを調整べき乗 Xamarin.iOS、サブセットとよく似ています。 このターゲット フレームワークとは安全でリンカーのため、これらのプロジェクトは、未使用のコードを削除することで大幅に軽減、最終のフット プリントを持つことができます。

- **完全**(以前のドキュメントで XM 4.5 と呼ばれる) – いくつかの小さな削除で、[デスクトップ] BCL をサブセットとよく似ています。 ターゲット フレームワークが、net45 (とそれ以降) とほぼ同じ、か netstandard2 を提供しない多く nugets を簡単に使用することができますか、特定 Xamarin.Mac を構築します。 ただし、System.Configuration 使用状況のため、リンクと互換性がありません。

- **サポートされていない**(と呼ばれるシステム以前のドキュメントで) – 代わりに Xamarin.Mac によって提供される BCL にリンクするのには、現在インストールされているシステム モノラルを使用します。 これは、アセンブリ、既知の問題があることなど (たとえば System.Drawing) の機能を最大限のセットを提供します。 このオプションはのみが存在する「最後の手段」があり、その他のオプションを使用する前に使い果たすを推奨します。 名前が示すように、使用状況は公式サポート チャネルによってサポートされていません。

## <a name="setting-the-target-framework"></a>ターゲット フレームワークを設定

Xamarin.Mac プロジェクトのターゲット フレームワークの種類に変更するには、次の操作を行います。

1. Visual Studio for Mac で Xamarin.Mac プロジェクトを開きます。
2. **ソリューション エクスプローラー**で、プロジェクト ファイルをダブルクリックし、**[プロジェクト オプション]** ダイアログ ボックスを開きます。
3. **全般**の種類の選択 タブで、**ターゲット フレームワーク**アプリケーションのニーズに適した。

  [![プロジェクトのオプション ウィンドウを使用してターゲット フレームワークを選択する](target-framework-images/select-target-full.png "プロジェクトのオプションのウィンドウを使用してターゲット フレームワークを選択するには")](target-framework-images/select-target-full-large.png#lightbox)

4. **[OK]** をクリックして変更内容を保存します。

行う必要があります**クリーン**し**を再構築**Xamarin.Mac プロジェクトのターゲット フレームワークの種類の切り替え後です。

## <a name="summary"></a>まとめ

この記事では、さまざまな種類の Xamarin.Mac アプリケーションと各種のフレームワークを使用する必要がある時に使用可能なターゲット フレームワーク (基本クラス ライブラリ) をカバーされてについて簡単にします。


## <a name="related-links"></a>関連リンク

- [iOS および Mac のコードの共有](~/cross-platform/macios/index.md)
- [Unified API](~/cross-platform/macios/unified/index.md)
- [ポータブル クラス ライブラリ](~/cross-platform/app-fundamentals/pcl.md)
- [アセンブリ](~/cross-platform/internals/available-assemblies.md)
- [既存の Mac アプリケーションの更新](~/cross-platform/macios/unified/updating-mac-apps.md)
