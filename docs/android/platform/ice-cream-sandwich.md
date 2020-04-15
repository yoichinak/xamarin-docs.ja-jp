---
title: Ice Cream Sandwich の機能
description: この記事では、Android 4 API - Ice Cream Sandwich でアプリケーション開発者が利用できるいくつかの新機能について説明します。 ここでは新しいユーザー インターフェイス テクノロジをいくつか取り上げた後、アプリケーション間およびデバイス間でデータを共有するために Android 4 で提供されるさまざまな新機能について説明します。
ms.prod: xamarin
ms.assetid: 78E18A62-C12F-A699-37FA-44B9F6B44273
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 4fbbe1bec317e66166d5218ef0ed54247aa4f6dd
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "76724376"
---
# <a name="ice-cream-sandwich-features"></a>Ice Cream Sandwich の機能

_この記事では、Android 4 API - Ice Cream Sandwich でアプリケーション開発者が利用できるいくつかの新機能について説明します。ここでは新しいユーザー インターフェイス テクノロジをいくつか取り上げた後、アプリケーション間およびデバイス間でデータを共有するために Android 4 で提供されるさまざまな新機能について説明します。_

## <a name="overview"></a>概要

Android OS バージョン 4.0 (API レベル 14) は、Android オペレーティング システムの大幅な再構成に加えて、次のような重要な変更やアップグレードが多数含まれています。

- **ユーザー インターフェイスの更新** - いくつかの新しい UI 機能によって、開発者はアプリケーション ユーザー インターフェイスを作成するときに、より強力かつ柔軟に行うことができます。 これらの新機能には、`GridLayout`、`PopupMenu`、`Switch` ウィジェット、`TextureView` が含まれます。
- **ハードウェアのさらなる高速化** - すべての Android コントロールについて、2D レンダリングが GPU で行われるようになりました。 また、Android 4.0 用に開発されたすべてのアプリケーションで、ハードウェアの高速化が既定でオンになっています。
- **新しいデータ API** - カレンダー データやデバイス所有者のユーザー プロファイルなど、以前は公式にはアクセスできなかったデータへの新しいアクセスが可能になりました。
- **アプリ データの共有** - アプリケーションとデバイス間のデータの共有は、操作バーから共有アクションを簡単に作成できる `ShareActionProvider` や、近接するデバイス間でデータを簡単に共有できる *近距離無線通信 (NFC)* 用の *Android ビーム*などのテクノロジによって、かつてないほどに容易になりました。

この記事では、Android 4.0 API に加えられたこれらの機能とその他の変更について説明します。また、各機能を Xamarin.Android で使用する方法についても説明します。

## <a name="user-interface-features"></a>ユーザー インターフェイスの機能

Android 4 では、次のようなさまざまな新しいユーザー インターフェイス テクノロジを利用できます。

- **[GridLayout](~/android/user-interface/layouts/grid-layout.md)** - コントロールの 2D グリッド レイアウトをサポートします。
- **[Switch ウィジェット](~/android/user-interface/controls/switch.md)** - オンとオフを切り替えることができます。
- **[TextureView](~/android/user-interface/controls/texture-view.md)** - ビュー内の動画と OpenGL コンテンツを有効にします。
- **[ナビゲーション バー](~/android/user-interface/controls/navigation-bar.md)** - 戻る、ホーム、およびマルチタスク用の仮想ボタンが含まれています。

さらに、操作が容易になった `<a href"/guides/android/user_interface/popup_menus">PopupMenu</a>` や、外観がより洗練されたタブなど、その他の UI 要素も強化されました。

## <a name="sharing-features"></a>共有機能

Android 4 には、デバイス間およびアプリケーション間でデータを共有できる新しいテクノロジがいくつか含まれています。 また、カレンダー情報やデバイス所有者のユーザー プロファイルなど、以前は利用できなかったさまざまな種類のデータにもアクセスできます。 このセクションでは、Android 4 で提供される、次のような領域に対応するさまざまな機能について説明します。

- **[Android ビーム](~/android/platform/android-beam.md)** – NFC 経由でのデータ共有を可能にします。
- **[ShareActionProvider](~/android/user-interface/controls/action-bar.md)** – 開発者が操作バーから共有アクションを指定できるようにするプロバイダーを作成します。
- **[ユーザー プロファイル](~/android/user-interface/user-profile.md)** – デバイス所有者のプロファイル データへのアクセスを提供します。
- **[カレンダー API](~/android/user-interface/controls/calendar.md)** – カレンダー プロバイダーからのカレンダー データへのアクセスを提供します。

## <a name="x86-emulators"></a>x86 エミュレーター

ICS は、x86 エミュレーターを使用した開発をまだサポートしていません。 x86 エミュレーターは、Android 2.3.3、API レベル 10 でのみサポートされています。 詳細については、[x86 エミュレーターの構成](~/android/get-started/installation/android-emulator/index.md)に関する記事を参照してください。

## <a name="summary"></a>まとめ

この記事では、Android 4 で利用できるようになったさまざまな新しいテクノロジについて説明しました。 *GridLayout*、*PopupMenu*、*Switch* ウィジェットなどの新しいユーザー インターフェイス機能について確認しました。 また、システム UI を制御するための新しいサポートや、*TextureView* を操作する方法についても確認しました。 次に、さまざまな新しい共有テクノロジについて説明しました。 *Android ビーム*によって、*NFC* を使用するデバイス間で情報を共有する方法について説明し、新しい *Calendar API* について確認し、組み込みの *ShareActionProvider* の使用方法も示しました。
最後に、ユーザー プロファイル データにアクセスするために、*ContactsContract* プロバイダーを使用する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [TextureViewDemo (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/textureviewdemo)
- [CalendarDemo (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/calendardemo)
- [タブ レイアウトのチュートリアル](~/android/user-interface/layouts/tab-layout/index.md)
- [Ice Cream Sandwich](https://developer.android.com/about/versions/android-4.0-highlights.html)
- [Android 4.0 プラットフォーム](https://developer.android.com/about/versions/android-4.0.html)
