---
title: レイアウト
description: Xamarin Android アプリのビジュアル構造の定義
ms.prod: xamarin
ms.assetid: 2BA72B0E-230D-4F98-B4D5-4EFB0D479789
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/18/2017
ms.openlocfilehash: 60752760415bd416d339cc2a3729075b4fca0d32
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70764474"
---
# <a name="xamarinandroid-layouts"></a>Xamarin Android のレイアウト

レイアウトは、画面の UI インターフェイスを構成する要素 (アクティビティなど) を配置するために使用されます。 以下のセクションでは、Xamarin Android アプリで最もよく使用されるレイアウトを使用する方法について説明します。

- [LinearLayout](~/android/user-interface/layouts/linear-layout.md)は、垂直方向または水平方向の線形方向に子ビュー要素を表示するビューグループです。

    ![線形レイアウトの例](images/linear-layout.png)

- [RelativeLayout](~/android/user-interface/layouts/relative-layout.md)は、子ビューの要素を相対的な位置に表示するビューグループです。 ビューの位置は、兄弟要素に対する相対値として指定できます。

    ![相対レイアウトの例](images/relative-layout.png)

- [TableLayout](~/android/user-interface/layouts/table-layout.md)は、子ビューの要素を行と列に表示するビューグループです。

    ![テーブルのレイアウトの例](images/table-layout.png)

- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)は、リストまたはグリッド内の項目のコレクションを表示し、ユーザーがコレクションをスクロールできるようにする UI 要素です。

    ![リサイクラービューの例](images/recycler-view.png)

- [ListView](~/android/user-interface/layouts/list-view/index.md)は、スクロール可能な項目の一覧を作成するビューグループです。 リストの項目は、リストアダプターを使用してリストに自動的に挿入されます。 は`ListView` Android アプリケーションの重要な UI コンポーネントです。これは、メニューオプションの短い一覧から、連絡先やインターネットのお気に入りの一覧に至るまで、あらゆる場所で使用されるためです。 この機能を使用すると、組み込みスタイルで書式設定したり、広範にカスタマイズしたりできる行のスクロールリストを簡単に表示できます。 ListView インスタンスには、行ビューに含まれるデータをフィードするアダプターが必要です。

    ![リストビューの例](images/list-view.png)

- [GridView](~/android/user-interface/layouts/grid-view.md)は、スクロール可能な2次元グリッドに項目を表示する UI 要素です。

    ![グリッドビューの例](images/grid-view.png)

- [GridLayout](~/android/user-interface/layouts/grid-layout.md)は、HTML テーブルと同様に、2d グリッドでのビューのレイアウトをサポートするビューグループです。

    ![グリッドレイアウトの例](images/grid-layout.png)

- [タブ付きレイアウト](~/android/user-interface/layouts/tab-layout/index.md)は、単純さと使いやすさが理由で、モバイルアプリケーションの一般的なユーザーインターフェイスパターンです。 アプリケーションのさまざまな画面間を移動するための一貫した簡単な方法を提供します。

    ![タブ付きレイアウトの例](images/tabbed-layout.png)
