---
title: アイスクリームサンドイッチの機能
description: この記事では、Android 4 の API アイスクリームを使用して、アプリケーション開発者が利用できるいくつかの新機能について説明します。 新しいユーザーインターフェイステクノロジをいくつか取り上げ、Android 4 がアプリケーション間およびデバイス間でデータを共有するために提供するさまざまな新機能について説明します。
ms.prod: xamarin
ms.assetid: 78E18A62-C12F-A699-37FA-44B9F6B44273
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 382315f755102d7111db1a5c0f71d43bdea97a10
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020181"
---
# <a name="ice-cream-sandwich-features"></a>アイスクリームサンドイッチの機能

_この記事では、Android 4 の API アイスクリームを使用して、アプリケーション開発者が利用できるいくつかの新機能について説明します。新しいユーザーインターフェイステクノロジをいくつか取り上げ、Android 4 がアプリケーション間およびデバイス間でデータを共有するために提供するさまざまな新機能について説明します。_

## <a name="overview"></a>概要

Android OS バージョン 4.0 (API レベル 14) は、Android オペレーティングシステムの主要な作り替えるを表し、次のような重要な変更とアップグレードが多数含まれています。

- **ユーザーインターフェイスが更新されまし**た。新しい UI 機能によって、開発者はアプリケーションユーザーインターフェイスを作成するときに、より強力で柔軟性を高めることができます。 これらの新機能には、`GridLayout`、`PopupMenu`、`Switch` ウィジェット、`TextureView` があります。 
- **ハードウェアアクセラレータの向上**–2d レンダリングは、すべての Android コントロールの GPU で実行されるようになりました。 また、既定では、Android 4.0 用に開発されたすべてのアプリケーションでハードウェアアクセラレータがオンになっています。 
- **新しいデータ api** –カレンダーデータやデバイス所有者のユーザープロファイルなど、以前は正式にアクセスできなかったデータへの新しいアクセス権があります。 
- **アプリデータの共有**–アプリケーションとデバイス間でのデータの共有は、`ShareActionProvider` などのテクノロジにより、これまで以上に簡単になりました。これにより、操作バーからの共有アクションを簡単に作成でき、近距離通信用の*Android ビーム* *(NFC)* を使用すると、互いに近接しているデバイス間でデータを共有するためのスナップができます。 

この記事では、Android 4.0 API に加えられたこれらの機能とその他の変更について説明します。また、各機能を Xamarin Android で使用する方法についても説明します。

## <a name="user-interface-features"></a>ユーザーインターフェイスの機能

Android 4 では、次のようなさまざまな新しいユーザーインターフェイステクノロジを利用できます。

- **[GridLayout](~/android/user-interface/layouts/grid-layout.md)** –コントロールの2d グリッドレイアウトをサポートします。 
- **[ウィジェットの切り替え](~/android/user-interface/controls/switch.md)** –オンとオフを切り替えることができます。 
- **[Textureview](~/android/user-interface/controls/texture-view.md)** –ビュー内でビデオと OpenGL のコンテンツを有効にします。 
- **[ナビゲーションバー](~/android/user-interface/controls/navigation-bar.md)** – [戻る]、[ホーム]、および [マルチタスク] の仮想ボタンが表示されます。 

さらに、他の UI 要素も強化されています。たとえば、`<a href"/guides/android/user_interface/popup_menus">PopupMenu</a>`が使いやすくなり、より洗練された外観を備えたタブが使用できるようになりました。

## <a name="sharing-features"></a>機能の共有

Android 4 には、デバイス間およびアプリケーション間でデータを共有できる新しいテクノロジがいくつか含まれています。 また、カレンダー情報やデバイス所有者のユーザープロファイルなど、以前は利用できなかったさまざまな種類のデータへのアクセスも提供します。 このセクションでは、Android 4 で提供される次のようなさまざまな機能について説明します。

- **[Android ビーム](~/android/platform/android-beam.md)** – NFC 経由でのデータ共有を許可します。
- 共有 **[actionprovider](~/android/user-interface/controls/action-bar.md)** –開発者が操作バーから共有操作を指定できるようにするプロバイダーを作成します。 
- **[ユーザープロファイル](~/android/user-interface/user-profile.md)** –デバイス所有者のプロファイルデータへのアクセスを提供します。 
- **[CALENDAR API](~/android/user-interface/controls/calendar.md)** –カレンダープロバイダーから予定表データへのアクセスを提供します。 

## <a name="x86-emulators"></a>x86 エミュレーター

ICS は、x86 エミュレーターを使用した開発をまだサポートしていません。 x86 エミュレーターは、Android 2.3.3、API レベル10でのみサポートされています。 詳細について[は、「X86 Emulator の構成](~/android/get-started/installation/android-emulator/index.md)」を参照してください。

## <a name="summary"></a>まとめ

この記事では、Android 4 で利用できるようになったさまざまな新しいテクノロジについて説明しました。 *GridLayout*、 *PopupMenu*、 *Switch*の各ウィジェットなどの新しいユーザーインターフェイス機能を確認しています。 また、システム UI の制御に関する新しいサポートと、 *Textureview*の使用方法についても説明しました。 次に、さまざまな新しい共有テクノロジについて説明しました。 ここでは、 *Android ビーム*で、 *NFC*を使用するデバイス間で情報を共有する方法、新しい*Calendar API*について説明し、組み込みの共有*actionprovider*の使用方法について説明しました。
最後に、 *ContactsContract*プロバイダーを使用してユーザープロファイルデータにアクセスする方法を説明します。

## <a name="related-links"></a>関連リンク

- [アイスクリームサンドイッチのサンプル](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-ics-samples)
- [TextureViewDemo (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/textureviewdemo)
- [CalendarDemo (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/calendardemo)
- [タブレイアウトのチュートリアル](~/android/user-interface/layouts/tab-layout/index.md)
- [アイスクリームサンドイッチ](https://developer.android.com/about/versions/android-4.0-highlights.html)
- [Android 4.0 プラットフォーム](https://developer.android.com/about/versions/android-4.0.html)
