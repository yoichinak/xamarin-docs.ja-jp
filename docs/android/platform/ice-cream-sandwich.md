---
title: "アイスクリーム サウスサンドウィッチ機能"
description: "この記事では、いくつかの API 使用して、Android 4 - アイスクリーム サウスサンドウィッチ アプリケーション開発者が利用できる新しい機能について説明します。 いくつかの新しいユーザー インターフェイス テクノロジについて説明し、次のさまざまな Android 4 には、アプリケーション間とデバイス間でデータを共有する新しい機能を確認します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 78E18A62-C12F-A699-37FA-44B9F6B44273
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: c2e7f9fabd462faf813299021d5f84ef0a62f790
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="ice-cream-sandwich-features"></a>アイスクリーム サウスサンドウィッチ機能

_この記事では、いくつかの API 使用して、Android 4 - アイスクリーム サウスサンドウィッチ アプリケーション開発者が利用できる新しい機能について説明します。いくつかの新しいユーザー インターフェイス テクノロジについて説明し、次のさまざまな Android 4 には、アプリケーション間とデバイス間でデータを共有する新しい機能を確認します。_

## <a name="overview"></a>概要

Android OS 4.0 (API レベル 14)、メジャーのやり直し Android オペレーティング システムを表し、多数重要な変更とを含むアップグレードにはが含まれています。

-   **ユーザー インターフェイスを更新**: いくつかの新しい UI 機能を複数の電源の開発者に提供し、アプリケーションのユーザーを作成するときの柔軟性のインターフェイスです。 これらの新機能が含まれます: `GridLayout` 、 `PopupMenu` 、`Switch`ウィジェット、および`TextureView`です。 
-   **ハードウェア アクセラレータのより強力な**– 2 D のすべての Android のコントロールの GPU での処理を行うのレンダリングようになりました。 さらに、ハードウェア アクセラレータは、既定では、Android 4.0 用に開発されたすべてのアプリケーションです。 
-   **データの新しい Api** – 以前正式にアクセスできなかった、予定表のデータとデバイスの所有者のユーザー プロファイルなどのデータを新しいアクセス権があります。 
-   **アプリのデータの共有**– 共有アプリケーションとデバイス間でデータが今すぐテクノロジを使用してこれまでより簡単になど、`ShareActionProvider`を簡単に操作バーから共有のアクションを作成および*Android ビーム**近距離通信 (NFC)* 、互いに近接デバイス間でデータを共有するスナップインになります。 


この記事ではこれらの機能と、Android 4.0 API に加えられたその他の変更を探索することと Xamarin.Android で各機能を使用する方法について説明します。

## <a name="user-interface-features"></a>ユーザー インターフェイスの機能

など、Android 4 で使用できるは、さまざまなユーザー インターフェイスの新しいテクノロジです。

-   **[GridLayout](~/android/user-interface/layouts/grid-layout.md)**  – コントロールの 2D グリッド レイアウトをサポートしています。 
-   **[ウィジェットを切り替える](~/android/user-interface/controls/switch.md)** – 間を切り替えることが可能になります ON または OFF です。 
-   **[TextureView](~/android/user-interface/controls/texture-view.md)**  – ビデオと OpenGL のコンテンツ ビュー内で有効にします。 
-   **[ナビゲーション バー](~/android/user-interface/controls/navigation-bar.md)**  – バックアップ、家庭用、およびマルチタスク仮想ボタンが含まれています。 


さらに、他の UI 要素が強化されたなど、`<a href"/guides/android/user_interface/popup_menus">PopupMenu</a>`は今すぐに簡単に扱うとタブより洗練された外観であります。

## <a name="sharing-features"></a>共有機能

Android 4 には、複数のデバイスとアプリケーション間でデータを共有できるいくつかの新しいテクノロジが含まれています。 また、各種の予定表の情報とデバイスの所有者のユーザー プロファイルなど、以前は使用できなかったデータへのアクセスも提供します。 このセクションの内容について見ていきましょうのさまざまな機能によって提供される Android 4 そのアドレスを含むこれらの領域。

-  **[Android ビーム](~/android/platform/android-beam.md)** – 可能データの NFC を使用して共有します。
-   **[ShareActionProvider](~/android/user-interface/controls/action-bar.md)**  – 操作バーから共有のアクションを指定できるように、プロバイダーを作成します。 
-   **[ユーザー プロファイル](~/android/user-interface/user-profile.md)** – デバイスの所有者のプロファイル データにアクセスできるようにします。 
-   **[予定表 API](~/android/user-interface/controls/calendar.md)**  – カレンダー プロバイダーから予定表データへのアクセスを提供します。 

## <a name="x86-emulators"></a>x86 エミュレーター

ICS はまだ、x86 を使用した開発をサポートしていませんエミュレーターです。 x86 エミュレーターは API レベル 10、Android 2.3.3 でのみサポートされます。 参照してください[構成の x86 エミュレーター](~/android/get-started/installation/android-emulator/index.md)詳細についてはします。

## <a name="summary"></a>まとめ

この記事では、さまざまな Android 4 で使用できるようになった新しいテクノロジについて説明します。 新しいユーザー インターフェイスの機能をなどについて確認しました、 *GridLayout*、*ポップアップ メニュー*、および*スイッチ*ウィジェット。 使用する方法として、システム UI を制御するための新しいサポートのいくつかについても説明しました、 *TextureView*です。 新しい共有テクノロジのさまざまなを説明します。 お方法について説明*Android ビーム*を使用するデバイス間で情報を共有しましょう*NFC*、説明、新しい*予定表 API*もの組み込みを使用する方法を紹介し、*ShareActionProvider*です。
最後に、使用する方法を調べるお、 *ContactsContract*ユーザー プロファイル データにアクセスするプロバイダー。



## <a name="related-links"></a>関連リンク

- [アイスクリーム サウスサンドウィッチ サンプル](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/)
- [TextureViewDemo (sample)](https://developer.xamarin.com/samples/monodroid/TextureViewDemo/)
- [CalendarDemo (サンプル)](https://developer.xamarin.com/samples/monodroid/CalendarDemo/)
- [タブ レイアウトのチュートリアル](~/android/user-interface/layouts/tab-layout/index.md)
- [アイスクリーム サウスサンドウィッチ](http://developer.android.com/about/versions/android-4.0-highlights.html)
- [Android 4.0 プラットフォーム](http://developer.android.com/about/versions/android-4.0.html)
