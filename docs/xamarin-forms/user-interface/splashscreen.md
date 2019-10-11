---
title: Xamarin. フォームスプラッシュスクリーン
description: この記事では、Xamarin アプリケーションのスプラッシュスクリーンを作成する方法について説明します。
ms.prod: xamarin
ms.assetId: 338C8F60-90F2-4B3D-BB47-7F846AE8DC6C
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 10/1/2019
ms.openlocfilehash: 2624c3f35a9cfac122a0b36ea8c1684d30f57824
ms.sourcegitcommit: 5110d1279809a2af58d3d66cd14c78113bb51436
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/08/2019
ms.locfileid: "72035774"
---
# <a name="xamarinforms-splash-screen"></a>Xamarin. フォームスプラッシュスクリーン

アプリケーションの初期化プロセスが完了している間、多くの場合、アプリケーションの起動に遅延が発生します。 開発者は、アプリケーションの起動中に、通常はスプラッシュスクリーンと呼ばれるブランド化されたエクスペリエンスを提供することができます。 この記事では、Xamarin アプリケーションのスプラッシュスクリーンを作成する方法について説明します。

ネイティブスタートアップシーケンスが完了すると、各プラットフォームで Xamarin が初期化されます。 Xamarin. フォームが初期化されます。

- Android の `MainActivity` クラスの `OnCreate` メソッド。
- IOS の `AppDelegate` クラスの `FinishedLaunching` メソッド。
- UWP の `App` クラスの `OnLaunched` メソッド。

スプラッシュスクリーンは、アプリケーションを起動したときにできるだけ早く表示されるはずですが、Xamarin. Forms は起動シーケンスの遅延まで初期化されません。つまり、スプラッシュスクリーンは、各プラットフォーム上の Xamarin とは別に実装する必要があります。 以下のセクションでは、各プラットフォームでスプラッシュスクリーンを作成する方法について説明します。

## <a name="xamarinforms-android-splash-screen"></a>Xamarin. Forms Android スプラッシュスクリーン

Android でスプラッシュスクリーンを作成するには、特別なテーマを持つ `MainLauncher` として `Activity` というスプラッシュを作成する必要があります。 スプラッシュ `Activity` が開始されるとすぐに、通常のアプリケーションテーマを使用してメイン `Activity` を起動します。

Xamarin Android のスプラッシュスクリーンの詳細については、「 [xamarin のスプラッシュスクリーン](~/android/user-interface/splash-screen.md)」を参照してください。

## <a name="xamarinforms-ios-splash-screen"></a>Xamarin. フォーム iOS スプラッシュスクリーン

IOS のスプラッシュスクリーンは、起動画面と呼ばれます。 IOS で起動画面を作成するには、起動画面の UI を定義するストーリーボードを作成し、そのストーリーボードを**情報 plist**で起動画面として設定する必要があります。

Xamarin. iOS の起動画面の詳細については、「 [xamarin の起動画面](~/ios/app-fundamentals/images-icons/launch-screens.md)」を参照してください。

## <a name="xamarinforms-uwp-splash-screen"></a>Xamarin. Forms UWP スプラッシュスクリーン

UWP の**package.appxmanifest**には、**スプラッシュスクリーン**サブメニューを含む **[ビジュアルアセット]** タブがあります。 このメニューでは、スプラッシュスクリーンのグラフィックスを指定できます。

[![Setting のスプラッシュスクリーンの設定](splashscreen-images/uwp-splashscreen-cropped.png)](splashscreen-images/uwp-splashscreen.png#lightbox)

## <a name="related-links"></a>関連リンク

- [Xamarin. Android スプラッシュスクリーン](~/android/user-interface/splash-screen.md)
- [Xamarin. iOS の起動画面](~/ios/app-fundamentals/images-icons/launch-screens.md)
