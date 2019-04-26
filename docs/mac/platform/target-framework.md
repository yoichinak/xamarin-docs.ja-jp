---
title: Xamarin.Mac 向けのターゲット フレームワーク
description: この記事では、Xamarin.Mac の使用可能なターゲット フレームワーク (基本クラス ライブラリ) および Xamarin.Mac プロジェクトで使用する場合の影響について説明します。
ms.prod: xamarin
ms.assetid: AF21BE16-3F92-4121-AB4C-D51AC863D92D
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 11/10/2017
ms.openlocfilehash: 15c93126f80917df45a5b80fb84397dc6ef0d5fd
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61075629"
---
# <a name="target-framework-for-xamarinmac"></a>Xamarin.Mac 向けのターゲット フレームワーク

_この記事では、Xamarin.Mac の使用可能なターゲット フレームワーク (基本クラス ライブラリ) および Xamarin.Mac プロジェクトで使用する場合の影響について説明します。_

![Xamarin.Mac 向けのオプションのフレームワークをターゲット](target-framework-images/select-target.png "-target Xamarin.Mac 向けフレームワーク オプション")

## <a name="background"></a>背景

すべての .NET プログラムまたはライブラリは、基本クラス ライブラリ (BCL) によって提供される機能に依存します。 この BCL には、すべての .NET 言語に組み込まれている一般的な機能を提供するアセンブリ、mscorlib、System、System.Net.Http、および System.Xml などが含まれています。

長年にわたって開発には、さまざまなユース ケース用に最適化された、この BCL の複数の異なるバージョンがあります。 [デスクトップ] の BCL には、アプリケーションのフット プリントを削減の未使用のコードを削除する可能性のある他のユース ケースのすぎる重い紙 mobile Api は安全にリンクすると、使用するようにすることに重点を置いています中にライブラリの豊富なセットが含まれています。

これらの別のターゲット フレームワークの重要な影響の 1 つのすべてのプログラムでアセンブリ*する必要があります*互換性のある BCL アセンブリのターゲットします。 2 つのアセンブリに対してさまざまなバージョンのリンクがある可能性がありますがない場合は場合、 **System.dll** disagreeing について、指定された型のシグネチャ。 共有ライブラリはいずれかのターゲット[.NET Standard の 2](https://blog.xamarin.com/share-code-net-standard-2-0/)、共通のターゲット フレームワーク、または特定のターゲット フレームワークのサブセットであります。

ターゲット フレームワークの 3 つのオプションは Xamarin.Mac、それぞれ異なる利点とトレードオフに使用できます。

- **最新**(以前のドキュメントで Mobile と呼ばれます) – どのような高パフォーマンスとサイズを調整べき乗 Xamarin.iOS、サブセットとよく似ています。 このターゲット フレームワークは安全でリンカーは、これらのプロジェクトは、未使用のコードを削除することで大幅に減少しました、最後のフット プリントを持つことができます。

- **完全な**(以前のドキュメントで XM 4.5 と呼ばれます) –、いくつかの小規模な削除を使用して、「デスクトップ」BCL にサブセットとよく似ています。 Net45 (とそれ以降)、ターゲット フレームワークはほぼ同じですが、netstandard2 いずれかを指定しない多くの nuget を簡単に使用することができます。 または特定 Xamarin.Mac をビルドします。 ただし、System.Configuration 使用率に起因リンクと互換性がありません。

- **サポートされていない**(システムでは以前のドキュメント) – 代わりに Xamarin.Mac によって提供される BCL にリンクするのには、現在のシステムがインストールされている mono を使用します。 これは、アセンブリ、既知の問題があるものも含めて (たとえば System.Drawing) の機能を最大限のセットを提供します。 このオプションはのみが存在する「最後の手段」があり、その他のオプションを使用する前に使い果たしてしまうことを強くお勧めします。 名前が示すように、使用状況は公式なサポート チャネルによってサポートされていません。

## <a name="setting-the-target-framework"></a>ターゲット フレームワークを設定します。

Xamarin.Mac プロジェクトのターゲット フレームワークの型に変更するには、次の操作を行います。

1. Visual Studio for Mac で Xamarin.Mac プロジェクトを開きます。
2. **ソリューション エクスプローラー**で、プロジェクト ファイルをダブルクリックし、**[プロジェクト オプション]** ダイアログ ボックスを開きます。
3. **全般**の種類を選択 タブで、**ターゲット フレームワーク**アプリケーションのニーズに合った。

  [![プロジェクト オプション ウィンドウを使用して、ターゲット フレームワークを選択する](target-framework-images/select-target-full.png "プロジェクト オプション ウィンドウを使用して、ターゲット フレームワークを選択するには")](target-framework-images/select-target-full-large.png#lightbox)

4. **[OK]** をクリックして変更内容を保存します。

必要があります**クリーン**し**リビルド**ターゲット フレームワークの型を切り替えた後、Xamarin.Mac プロジェクト。

## <a name="summary"></a>まとめ

この記事には、Xamarin.Mac アプリケーションをおよび各種のフレームワークを使用する必要があります、使用可能なターゲット フレームワーク (基本クラス ライブラリ) のさまざまな種類について簡単にについて説明しました。


## <a name="related-links"></a>関連リンク

- [iOS および Mac のコードの共有](~/cross-platform/macios/index.md)
- [Unified API](~/cross-platform/macios/unified/index.md)
- [ポータブル クラス ライブラリ](~/cross-platform/app-fundamentals/pcl.md)
- [アセンブリ](~/cross-platform/internals/available-assemblies.md)
- [既存の Mac アプリを更新します。](~/cross-platform/macios/unified/updating-mac-apps.md)
