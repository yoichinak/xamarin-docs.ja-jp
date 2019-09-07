---
title: Xamarin. Mac 用のターゲットフレームワーク
description: この記事では、Xamarin. Mac で使用できるターゲットフレームワーク (基本クラスライブラリ) と、それらを Xamarin. Mac プロジェクトで使用することによる影響について説明します。
ms.prod: xamarin
ms.assetid: AF21BE16-3F92-4121-AB4C-D51AC863D92D
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 11/10/2017
ms.openlocfilehash: a612c2c23ceff13ea1d602465573514547628e55
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769804"
---
# <a name="target-framework-for-xamarinmac"></a>Xamarin. Mac 用のターゲットフレームワーク

_この記事では、Xamarin. Mac で使用できるターゲットフレームワーク (基本クラスライブラリ) と、それらを Xamarin. Mac プロジェクトで使用することによる影響について説明します。_

![Xamarin. Mac のターゲットフレームワークオプション](target-framework-images/select-target.png "Xamarin. Mac のターゲットフレームワークオプション")

## <a name="background"></a>背景

すべての .NET プログラムまたはライブラリは、基本クラスライブラリ (BCL) によって提供される機能に依存しています。 この BCL には、すべての .NET 言語に組み込まれている共通機能を提供する mscorlib、System、System .Net. Http、System.xml などのアセンブリが含まれています。

長年にわたり、この BCL の複数の異なるバージョンを開発しました。さまざまなユースケースに合わせて最適化されています。 "デスクトップ" BCL には、他のユースケースに対しては重いことがあるライブラリの豊富なセットが含まれていますが、モバイルではリンクに対して Api の安全性を確保することに重点を置いています。

これらの異なるターゲットフレームワークの大きな影響の1つは、特定のプログラム内のすべてのアセンブリが互換性のある BCL アセンブリをターゲットに*する必要*があることです。 そうでない場合は、異なるバージョンの **.dll**にリンクされている2つのアセンブリを、指定した型のシグネチャについて無効にすることができます。 共有ライブラリは、ターゲットフレームワークの共通のサブセットである[.NET Standard 2](https://blog.xamarin.com/share-code-net-standard-2-0/)、または特定のターゲットフレームワークのいずれかをターゲットにすることができます。

Xamarin. Mac には3つのターゲットフレームワークオプションが用意されており、それぞれに異なる長所とトレードオフがあります。

- **モダン**(以前のドキュメントでは Mobile と呼ばれていましたが、パフォーマンスとサイズのために高度にチューニングされた Xamarin. iOS の機能によく似たサブセットです)。 このターゲットフレームワークはリンカーセーフであるため、使用されていないコードを削除することで、これらのプロジェクトの最終的なフットプリントを大幅に削減できます。

- **完全**(以前のドキュメントでは XM 4.5 と呼ばれていました)。小さな削除がいくつかありますが、"デスクトップ" BCL と非常によく似ています。 ターゲットフレームワークは net45 (およびそれ以降) とほぼ同じであるため、netstandard2 または特定の Xamarin. Mac ビルドを提供しない多くの nuget を簡単に使用できます。 ただし、システム構成の使用により、リンクと互換性がありません。

- **サポート**されない(以前のドキュメントではシステムと呼ばれます)-Xamarin. Mac で提供される BCL にリンクするのではなく、現在インストールされている mono を使用します。 これにより、問題があるとわかっているもの (たとえば、システム図) を含むアセンブリのセットが提供されます。 このオプションには "last リゾート" しか存在しないため、使用前に他のオプションを使用しないことを強くお勧めします。 名前が示すように、使用法は公式サポートチャネルでサポートされていません。

## <a name="setting-the-target-framework"></a>ターゲットフレームワークの設定

Xamarin. Mac プロジェクトの [ターゲットフレームワーク] の種類に変更するには、次の手順を実行します。

1. Visual Studio for Mac で Xamarin.Mac プロジェクトを開きます。
2. **ソリューション エクスプローラー**で、プロジェクト ファイルをダブルクリックし、 **[プロジェクト オプション]** ダイアログ ボックスを開きます。
3. **[全般**] タブで、アプリケーションのニーズに適した**ターゲットフレームワーク**の種類を選択します。

    [[![プロジェクトオプション] ウィンドウを使用したターゲットフレームワークの選択][(target-framework-images/select-target-full.png "プロジェクトオプション] ウィンドウを使用したターゲットフレームワークの選択")](target-framework-images/select-target-full-large.png#lightbox)

4. **[OK]** をクリックして変更内容を保存します。

ターゲットフレームワークの種類を切り替えた後、Xamarin. Mac プロジェクトを**クリーン**にしてから**リビルド**する必要があります。

## <a name="summary"></a>Summary

この記事では、Xamarin. Mac アプリケーションで使用できるさまざまな種類のターゲットフレームワーク (基本クラスライブラリ) について簡単に説明し、各種類のフレームワークを使用する必要がある場合について説明しました。

## <a name="related-links"></a>関連リンク

- [iOS と Mac のコード共有](~/cross-platform/macios/index.md)
- [Unified API](~/cross-platform/macios/unified/index.md)
- [ポータブル クラス ライブラリ](~/cross-platform/app-fundamentals/pcl.md)
- [アセンブリ](~/cross-platform/internals/available-assemblies.md)
- [既存の Mac アプリを更新しています](~/cross-platform/macios/unified/updating-mac-apps.md)
