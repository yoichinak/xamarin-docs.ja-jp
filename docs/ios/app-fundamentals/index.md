---
title: Xamarin iOS アプリケーションの基礎
description: このドキュメントでは、アプリトランスポートセキュリティ、バックグラウンド処理、イベント、スレッド処理など、Xamarin の開発に関する基本概念を説明するさまざまなガイドにリンクしています。
ms.prod: xamarin
ms.assetid: 608403AE-B09F-4D9C-8F59-F9DE9F0B1CF1
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/21/2017
ms.openlocfilehash: 59257dafc1d92756feb85046df43de7b9da0cc42
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290150"
---
# <a name="xamarinios-application-fundamentals"></a>Xamarin iOS アプリケーションの基礎

このセクションでは、開発者がXamarin.iOS (旧称 MonoTouch) アプリケーションを開発するときに注意する必要がある一般的なタスクや概念についてガイドを提供します。

## <a name="accessibilityiosapp-fundamentalsaccessibilitymd"></a>[ユーザー補助](~/ios/app-fundamentals/accessibility.md)

このドキュメントでは、できるだけ多くのユーザーがアクセスできるアプリケーションを構築するために使用できるさまざまな Api とツールについて説明します。

## <a name="app-transport-securityiosapp-fundamentalsatsmd"></a>[アプリケーション トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)

この記事はiOS 9アプリにおいてApp Transport Securityによって必要となるセキュリティに関する変更とそれがあなたのXamarin.iOSプロジェクトにもたらす意味を紹介します。 この記事はATS構成オプションと必要な場合にATSからオプトアウトする方法を説明します。ATSは既定で有効となっているため、iOS 9アプリでは(明示的に許可しない限り)あらゆるセキュアでないインターネット接続によって例外が発生します。

## <a name="backgroundingiosapp-fundamentalsbackgroundingindexmd"></a>[バックグラウンド処理](~/ios/app-fundamentals/backgrounding/index.md)

バック グラウンド処理または backgrounding は、別のアプリケーションがフォア グラウンドで実行中に、アプリケーションにバック グラウンドでタスクを実行させる処理です。 このガイドは、iOSにおけるバック グラウンド処理の概要を説明します。

## <a name="creating-ios-applications-in-codeiosapp-fundamentalsios-code-onlymd"></a>[コードでの iOS アプリケーションの作成](~/ios/app-fundamentals/ios-code-only.md)

この記事では、Visual Studio と Visual Studio for Mac を使用して、コード内で iOS アプリケーションを完全に作成する方法について説明します。 この例では、空のプロジェクトテンプレートから開始して、UIKit からビューの階層を作成することによって、コントローラーでアプリケーション画面を構築する方法を示します。 次に、コントローラーに読み込むことができるカスタムビューを作成する方法について説明します。

## <a name="exception-marshalingiosplatformexception-marshalingmd"></a>[例外のマーシャリング](~/ios/platform/exception-marshaling.md)

ネイティブフレームとマネージフレームとの間で、目標 C とマネージ例外をマーシャリングする方法について説明します。

## <a name="events-protocols-and-delegatesiosapp-fundamentalsdelegates-protocols-and-eventsmd"></a>[イベント、プロトコル、およびデリゲート](~/ios/app-fundamentals/delegates-protocols-and-events.md)

この記事では、コールバックを受信して、ユーザー インターフェイス コントロールにデータを設定する場合に鍵となるiOS テクノロジを示します。 具体的には、イベント、プロトコル、およびデリゲートです。この記事では、それぞれどのようなものであるか、それぞれの C# からの使用方法を説明します。 この記事は Xamarin.iOSが iOSコントロールを使って使い慣れた .NET イベントを公開する方法だけでなく、Xamarin.iOSが プロトコルやデリゲートといった Objective-Cの概念をサポートする方法を示します。 (Objective-C デリゲートとC# デリゲートを混同しないように) この記事は プロトコルがObjective-C デリゲートと非デリゲートシナリオの両方の基盤としてどのように使うのかについても例を示します。

## <a name="working-with-the-file-systemiosapp-fundamentalsfile-systemmd"></a>[ファイルシステムの操作](~/ios/app-fundamentals/file-system.md)

Xamarin では、同じ System.IO クラスを使用して、任意の .NET アプリケーションで使用する iOS のファイルとディレクトリを操作できます。 ただし、使い慣れたクラスとメソッドにもかかわらず、iOS では、作成またはアクセス可能なファイルに対していくつかの制限が実装されています。また、特定のディレクトリに対して特別な機能を提供します。 この記事では、これらの制限と機能について概説し、Xamarin. iOS アプリケーションでのファイルアクセスのしくみについて説明します。

## <a name="working-with-imagesiosapp-fundamentalsimages-iconsindexmd"></a>[イメージの操作](~/ios/app-fundamentals/images-icons/index.md)

この記事では、Xamarin. iOS のイメージを使用する方法について説明します。これは、アプリケーションサポートイメージ (アイコン、イメージの読み込みなど)、アプリケーション内のイメージ (コントロールに適用されるイメージなど) の両方に対応しています。 また、Visual Studio for Mac を使用してイメージを組み込む方法と、コードからイメージを操作する方法についても説明します。

## <a name="localizationiosapp-fundamentalslocalizationindexmd"></a>[ローカリゼーション](~/ios/app-fundamentals/localization/index.md)

このガイドでは、国際化をサポートするための Xamarin. iOS アプリケーションへのエンコーディングの追加について説明します。

## <a name="working-with-property-listsiosapp-fundamentalsindexmd"></a>[プロパティリストの操作](~/ios/app-fundamentals/index.md)

このドキュメントでは、Visual Studio for Mac のグラフィカルおよび詳細プロパティリスト (plist) エディターについて説明します。このエディターでは、情報 plist と権利 plist を操作できます。 ここでは、iOS アプリケーションのアイコンと起動イメージを設定する方法と、Visual Studio for Mac 内部からアプリの機能 (権利) を指定する方法について説明します。

## <a name="working-with-security-and-privacyiosapp-fundamentalssecurity-privacymd"></a>[セキュリティとプライバシーの使用](~/ios/app-fundamentals/security-privacy.md)

Apple は、iOS 10 (およびそれ以降) のセキュリティとプライバシーの両方に対していくつかの機能強化を行っており、開発者がアプリのセキュリティを向上させ、エンドユーザーのプライバシーを確保するのに役立ちます。 この記事では、これらの機能を Xamarin iOS アプリに実装する方法について説明します。

## <a name="threadingiosapp-fundamentalsthreadingmd"></a>[スレッド化](~/ios/app-fundamentals/threading.md)

この記事では、Xamarin iOS アプリケーションのスレッド処理について説明し、.NET スレッドプール、応答性の高いアプリケーション、およびガベージコレクションについて説明します。

## <a name="touchiosapp-fundamentalstouchindexmd"></a>[タッチ](~/ios/app-fundamentals/touch/index.md)

今日の多くのデバイスでタッチスクリーンを使用すると、ユーザーは、自然で直感的な方法でデバイスとすばやく効率的に対話できます。 この相互作用は、単純なタッチ検出だけに限定されるわけではありません。ジェスチャを使用することもできます。 たとえば、ピンチとズームのジェスチャは、ユーザーが拡大または縮小できる2本の指で画面の一部をピンチすることで、非常に一般的な例です。このガイドでは、iOS のタッチとジェスチャについて説明します。

## <a name="working-with-user-defaultsiosapp-fundamentalsuser-defaultsmd"></a>[ユーザーの既定値の使用](~/ios/app-fundamentals/user-defaults.md)

クラス`NSUserDefaults`を使用すると、iOS アプリと拡張機能がシステム全体の既定のシステムとプログラムを使用して対話することができます。 ユーザーは、既定のシステムを使用して、アプリの動作またはスタイル設定 (アプリの設計に基づく) を構成できます。 たとえば、メトリックと英国の測定値にデータを表示したり、特定の UI テーマを選択したりすることができます。
