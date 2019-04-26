---
title: Ice Cream Sandwich 機能
description: この記事では、いくつかの Android 4 api - Ice Cream Sandwich アプリケーション開発者が利用できる新しい機能について説明します。 いくつかの新しいユーザー インターフェイス テクノロジについて説明し、次のさまざまな Android 4 がアプリケーション間およびデバイス間のデータを共有するために提供する新しい機能を確認します。
ms.prod: xamarin
ms.assetid: 78E18A62-C12F-A699-37FA-44B9F6B44273
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 00b553ae8de0dfcd86d57d1d5e3e2a892d6b5463
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61091180"
---
# <a name="ice-cream-sandwich-features"></a>Ice Cream Sandwich 機能

_この記事では、いくつかの Android 4 api - Ice Cream Sandwich アプリケーション開発者が利用できる新しい機能について説明します。いくつかの新しいユーザー インターフェイス テクノロジについて説明し、次のさまざまな Android 4 がアプリケーション間およびデバイス間のデータを共有するために提供する新しい機能を確認します。_

## <a name="overview"></a>概要

Android OS バージョン 4.0 (API レベル 14) は、主要な手直し Android オペレーティング システムを表し、多数重要な変更やアップグレードにはが含まれています。

-   **ユーザー インターフェイスを更新**: いくつかの新しい UI 機能を複数の電源の開発者に提供し、アプリケーションのユーザーを作成するときの柔軟性のインターフェイスします。 これらの新機能が含まれます: `GridLayout` 、 `PopupMenu` 、`Switch`ウィジェット、および`TextureView`します。 
-   **優れたハードウェア アクセラレータ**– 2 D rendering を今すぐすべての Android のコントロールの GPU 上で実行します。 さらに、ハードウェア アクセラレーションは、既定では、Android 4.0 用に開発されたすべてのアプリケーションでは。 
-   **新しいデータ Api** – 以前正式にアクセスできなかった、予定表のデータとデバイスの所有者のユーザー プロファイルなどのデータへの新しいアクセスがあります。 
-   **アプリ データの共有**– アプリケーションとデバイス間のデータを共有するが、テクノロジ経由でこれまでよりも簡単になど、 `ShareActionProvider` 、しやすく、操作バーから共有アクションを作成して*Android ビーム*  *近距離通信 (NFC)* 、相互に近接デバイス間でデータを共有する簡単になります。 


この記事では、これらの機能と、Android 4.0 API に加えられたその他の変更について説明していき、Xamarin.Android で各機能を使用する方法を説明しましょう。

## <a name="user-interface-features"></a>ユーザー インターフェイスの機能

さまざまな新しいユーザー インターフェイス テクノロジを含む、Android 4 で利用できます。

-   **[GridLayout](~/android/user-interface/layouts/grid-layout.md)**  – コントロールの 2 次元グリッド レイアウトをサポートしています。 
-   **[ウィジェットを切り替える](~/android/user-interface/controls/switch.md)** – の切り替えが許可 on または OFF にします。 
-   **[TextureView](~/android/user-interface/controls/texture-view.md)**  – ビデオと OpenGL のコンテンツ ビュー内で有効にします。 
-   **[ナビゲーション バー](~/android/user-interface/controls/navigation-bar.md)**  – バックアップ、home、およびマルチタスク仮想ボタンが含まれています。 


さらに、他の UI 要素が強化されなど、`<a href"/guides/android/user_interface/popup_menus">PopupMenu</a>`は今すぐに簡単に扱うとタブより洗練された外観であります。

## <a name="sharing-features"></a>共有機能

Android 4 には、複数のデバイスおよびアプリケーション間でデータを共有するいくつかの新しいテクノロジが含まれています。 また、さまざまな種類の予定表の情報とデバイスの所有者のユーザー プロファイルなど、以前使用できなかったデータへのアクセスも提供します。 取り上げるこのセクションではさまざまな機能によって提供される Android 4 そのアドレスを含むこれらの領域。

-  **[Android ビーム](~/android/platform/android-beam.md)** – NFC を使用した共有可能データ。
-   **[ShareActionProvider](~/android/user-interface/controls/action-bar.md)**  – により、開発者は、操作バーから共有の動作を指定するプロバイダーを作成します。 
-   **[ユーザー プロファイル](~/android/user-interface/user-profile.md)** – デバイスの所有者のプロファイル データへのアクセスを提供します。 
-   **[API のカレンダー](~/android/user-interface/controls/calendar.md)**  – カレンダー プロバイダーから予定表データへのアクセスを提供します。 

## <a name="x86-emulators"></a>x86 エミュレーター

ICS は x86 を使用した開発をまだサポートしていませんエミュレーター。 x86 エミュレーター、Android 2.3.3、API レベル 10 でのみサポートされます。 参照してください[構成、x86 エミュレーター](~/android/get-started/installation/android-emulator/index.md)詳細についてはします。

## <a name="summary"></a>まとめ

この記事では、さまざまな Android 4 には今すぐ利用できる新しいテクノロジについて説明します。 など、新しいユーザー インターフェイス機能を確認しました、 *GridLayout*、*ポップアップ メニュー*、および*スイッチ*ウィジェット。 新しいサポートを使用する方法についても、システム UI を制御するためのいくつかについても調査しました、 *TextureView*します。 次のさまざまな新しい共有テクノロジを説明します。 私たち方法について説明*Android ビーム*を使用して、デバイス間で情報を共有しましょう*NFC*、説明、新しい*予定表 API*の組み込みを使用する方法も示しましたと*ShareActionProvider*します。
最後に、使用する方法を調べる、 *ContactsContract*ユーザー プロファイル データにアクセスするプロバイダー。



## <a name="related-links"></a>関連リンク

- [Ice Cream Sandwich サンプルします。](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/)
- [TextureViewDemo (サンプル)](https://developer.xamarin.com/samples/monodroid/TextureViewDemo/)
- [CalendarDemo (サンプル)](https://developer.xamarin.com/samples/monodroid/CalendarDemo/)
- [タブ レイアウトのチュートリアル](~/android/user-interface/layouts/tab-layout/index.md)
- [Ice Cream Sandwich](https://developer.android.com/about/versions/android-4.0-highlights.html)
- [Android 4.0 プラットフォーム](https://developer.android.com/about/versions/android-4.0.html)
