---
title: Xamarin.iOS アプリケーションの基礎
description: このドキュメント ガイドへのリンクさまざまなアプリのトランスポート セキュリティなどの Xamarin.iOS の開発の基礎となる概念について説明したバック グラウンド処理は、イベント、およびスレッド処理します。
ms.prod: xamarin
ms.assetid: 608403AE-B09F-4D9C-8F59-F9DE9F0B1CF1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/21/2017
ms.openlocfilehash: a40227454b597578ff1c1c247b326e523c23493b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110473"
---
# <a name="xamarinios-application-fundamentals"></a>Xamarin.iOS アプリケーションの基礎

このセクションでは、開発者がXamarin.iOS (旧称 MonoTouch) アプリケーションを開発するときに注意する必要がある一般的なタスクや概念についてガイドを提供します。

## <a name="accessibilityiosapp-fundamentalsaccessibilitymd"></a>[ユーザー補助](~/ios/app-fundamentals/accessibility.md)

このドキュメントでは、さまざまな Api や、できるだけ多くのユーザーにアクセスできるアプリケーションの構築に役立つ使用できるツールについて説明します。

## <a name="app-transport-securityiosapp-fundamentalsatsmd"></a>[アプリケーション トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)

この記事はiOS 9アプリにおいてApp Transport Securityによって必要となるセキュリティに関する変更とそれがあなたのXamarin.iOSプロジェクトにもたらす意味を紹介します。 この記事はATS構成オプションと必要な場合にATSからオプトアウトする方法を説明します。ATSは既定で有効となっているため、iOS 9アプリでは(明示的に許可しない限り)あらゆるセキュアでないインターネット接続によって例外が発生します。

## <a name="backgroundingiosapp-fundamentalsbackgroundingindexmd"></a>[バックグラウンド処理](~/ios/app-fundamentals/backgrounding/index.md)

バック グラウンド処理または backgrounding は、別のアプリケーションがフォア グラウンドで実行中に、アプリケーションにバック グラウンドでタスクを実行させる処理です。 このガイドは、iOSにおけるバック グラウンド処理の概要を説明します。

## <a name="creating-ios-applications-in-codeiosapp-fundamentalsios-code-onlymd"></a>[コードで iOS アプリケーションの作成](~/ios/app-fundamentals/ios-code-only.md)

この記事では、Visual Studio と Visual Studio for mac を使用するコードだけで iOS アプリケーションを作成する方法を説明します。 UIKit からビューの階層を作成して、コント ローラーで、アプリケーションの画面を構築する空のプロジェクト テンプレートから開始する方法を示します。 次に、コント ローラーで読み込むことができるカスタム ビューを作成する方法について説明します。

## <a name="events-protocols-and-delegatesiosapp-fundamentalsdelegates-protocols-and-eventsmd"></a>[イベント、プロトコル、およびデリゲート](~/ios/app-fundamentals/delegates-protocols-and-events.md)

この記事では、コールバックを受信して、ユーザー インターフェイス コントロールにデータを設定する場合に鍵となるiOS テクノロジを示します。 具体的には、イベント、プロトコル、およびデリゲートです。この記事では、それぞれどのようなものであるか、それぞれの C# からの使用方法を説明します。 この記事は Xamarin.iOSが iOSコントロールを使って使い慣れた .NET イベントを公開する方法だけでなく、Xamarin.iOSが プロトコルやデリゲートといった Objective-Cの概念をサポートする方法を示します。 (Objective-C デリゲートとC# デリゲートを混同しないように) この記事は プロトコルがObjective-C デリゲートと非デリゲートシナリオの両方の基盤としてどのように使うのかについても例を示します。

## <a name="working-with-the-file-systemiosapp-fundamentalsfile-systemmd"></a>[ファイル システムの操作](~/ios/app-fundamentals/file-system.md)

Xamarin.iOS は、ファイルとディレクトリの任意の .NET アプリケーションで使用する iOS を使用する同じ System.IO クラスを使用することができます。 ただし、使い慣れたクラスとメソッドに関係なく、iOS は作成またはアクセスできるファイルのいくつかの制限を実装し、機能も提供します特別な特定のディレクトリ。 この記事では、これらの制限と機能の概要を説明します。 し、Xamarin.iOS アプリケーションでのファイル アクセスのしくみを示します。

## <a name="working-with-imagesiosapp-fundamentalsimages-iconsindexmd"></a>[画像の操作](~/ios/app-fundamentals/images-icons/index.md)

この記事では、Xamarin.iOS、(このアイコンを使用して、画像などの読み込み) などのアプリケーションのサポート イメージとイメージがコントロールに適用) などのアプリケーション内のイメージの両方でイメージを使用する方法について説明します。 Visual Studio for Mac を使用して画像を組み合わせたりする方法とコードからイメージを操作する方法についても説明します。

## <a name="localizationiosapp-fundamentalslocalizationindexmd"></a>[ローカリゼーション](~/ios/app-fundamentals/localization/index.md)

このガイドでは、エンコーディングを国際化をサポートするために Xamarin.iOS アプリケーションの追加について説明します。

## <a name="working-with-property-listsiosapp-fundamentalsindexmd"></a>[プロパティ リストを使用します。](~/ios/app-fundamentals/index.md)

このドキュメントは、Info.plist と Entitlements.plist を操作するため、Mac のグラフィックと高度なプロパティ一覧 (.plist) のエディターの Visual Studio を紹介します。 アイコンを設定する方法について説明し、起動、iOS アプリケーションのイメージしアプリの機能 (権利) を指定してから例を示します Visual Studio for mac 内

## <a name="working-with-security-and-privacyiosapp-fundamentalssecurity-privacymd"></a>[セキュリティとプライバシーの使用](~/ios/app-fundamentals/security-privacy.md)

Apple は iOS 10 (以降) でのプライバシーとセキュリティの両方に、開発者がアプリのセキュリティを強化し、エンドユーザーのプライバシーを確保に役立ついくつかの強化されています。 この記事では、Xamarin.iOS アプリでこれらの機能の実装について説明します。

## <a name="threadingiosapp-fundamentalsthreadingmd"></a>[スレッド化](~/ios/app-fundamentals/threading.md)

この記事では、Xamarin.iOS アプリケーションでのスレッドについて説明し、.NET スレッド プール、応答性の高いアプリケーション、およびガベージ コレクションについて少し説明。

## <a name="touchiosapp-fundamentalstouchindexmd"></a>[タッチ](~/ios/app-fundamentals/touch/index.md)

現在のデバイスの多くのタッチ スクリーンでは、迅速かつ効率的に、自然で直感的な方法でデバイスと対話するようにします。 この相互作用は簡単なタッチの検出だけに制限されています – もジェスチャを使用することができます。 たとえば、ピンチ ズーム ジェスチャは – この非常に一般的な例、ユーザーは拡大または縮小する 2 本の指で画面の一部をピンチしています。このガイドでは、タッチとジェスチャで iOS を調べます。

## <a name="working-with-user-defaultsiosapp-fundamentalsuser-defaultsmd"></a>[ユーザーの既定値の操作](~/ios/app-fundamentals/user-defaults.md)

`NSUserDefaults`クラスは、ios アプリと拡張機能は、システム全体の既定のシステムとの対話方法を提供します。 既定でシステムを使用して、ユーザーはアプリの動作や、ユーザー設定 (アプリのデザインに基づく) を満たすためにスタイル設定を構成できます。 たとえば、ヤード メトリックの vs でのデータを表示または指定した UI テーマを選択します。
