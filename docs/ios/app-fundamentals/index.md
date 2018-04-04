---
title: アプリケーションの基礎
description: 中核となるアプリケーションの概念
ms.prod: xamarin
ms.assetid: 608403AE-B09F-4D9C-8F59-F9DE9F0B1CF1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/21/2017
ms.openlocfilehash: d8dc1e25de527357fe6ad3ad1328a930e0e4dc70
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="application-fundamentals"></a>アプリケーションの基礎

このセクションでは、いくつかの一般的な処理タスクまたは開発者は、Xamarin.iOS (旧称 MonoTouch) アプリケーションを開発するときに注意する必要がある概念にガイドを提供します。

## <a name="app-transport-securityiosapp-fundamentalsatsmd"></a>[アプリケーション トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)

この記事で iOS 9 アプリおよびこれが意味、Xamarin.iOS プロジェクト用のアプリのトランスポート セキュリティを適用するセキュリティの変更を導入する ATS 構成オプションに適用されます、必要な場合は、ATS の脱退する方法はについて説明します。 (明示的に許可した) 場合を除き、ATS が既定で有効になっているためには、すべてのセキュリティ保護されていないインターネット接続で iOS 9 のアプリで例外が発生します。


## <a name="backgroundingiosapp-fundamentalsbackgroundingindexmd"></a>[バックグラウンド処理](~/ios/app-fundamentals/backgrounding/index.md)

バック グラウンド処理または backgrounding は、別のアプリケーションがフォア グラウンドで実行中に、バック グラウンドでタスクを実行するアプリケーションのプロセスです。 このガイドは、バック グラウンドで iOS 処理の概要として機能します。


## <a name="events-protocols-and-delegatesiosapp-fundamentalsdelegates-protocols-and-eventsmd"></a>[イベント、プロトコル、およびデリゲート](~/ios/app-fundamentals/delegates-protocols-and-events.md)

この記事では、コールバックを受信して、ユーザー インターフェイス コントロールにデータを設定するために使用するキー iOS テクノロジを表示します。 これらのテクノロジは、イベント、プロトコル、およびデリゲートです。この記事では、新機能について説明しますがこれらの各および c# から使用は、各方法です。 Xamarin.iOS が iOS コントロールを使用して、Xamarin.iOS Objective C の概念のプロトコルおよびデリゲートなどのサポートを提供する方法と、使い慣れた .NET イベントもを公開する方法を示しています (OBJECTIVE-C デリゲート混同しないように c# デリゲートを使用)。 この記事では、使用方法を示すプロトコルは、両方の基礎として OBJECTIVE-C デリゲート用とデリゲート以外のシナリオで例も提供します。

## <a name="threadingiosapp-fundamentalsthreadingmd"></a>[スレッド化](~/ios/app-fundamentals/threading.md)

この記事は、Xamarin.iOS アプリケーションでスレッド処理について説明し、について少し説明、.NET スレッド プール、応答性の高いアプリケーション、およびガベージ コレクション。&nbsp;

## <a name="working-with-imagesiosapp-fundamentalsimages-iconsindexmd"></a>[イメージの処理](~/ios/app-fundamentals/images-icons/index.md)

この記事では、Xamarin.iOS、(画像などの読み込みのアイコン) などのアプリケーションのサポートのイメージと (イメージなどのコントロールに適用) アプリケーション内のイメージの両方でイメージを使用する方法について説明します。 Visual Studio for Mac を使用してイメージを組み込む方法と、コードからのイメージと対話する方法についても説明します。

## <a name="working-with-property-listsiosapp-fundamentalsindexmd"></a>[プロパティ リストの使用](~/ios/app-fundamentals/index.md)

このドキュメントでは、Info.plist と Entitlements.plist の操作に Mac のグラフィックと高度なプロパティ一覧 (.plist) のエディターの Visual Studio を紹介します。 設定アイコンが示されていると、起動の iOS アプリケーションのイメージおよびからアプリの機能 (権限) を指定する例を示します Visual Studio for mac 内

## <a name="working-with-the-file-systemiosapp-fundamentalsfile-systemmd"></a>[ファイル システムの処理](~/ios/app-fundamentals/file-system.md)

Xamarin.iOS は、任意の .NET アプリケーションで使用する iOS でファイルおよびディレクトリを使用する同じ System.IO クラスを使用することができます。 ただし、使い慣れたクラスとメソッドに関係なく、iOS は作成またはアクセスできるファイルのいくつかの制限を実装し、機能も提供特別な特定のディレクトリ。 この記事では、これらの制限およびの機能について説明し、Xamarin.iOS アプリケーションでファイルへのアクセスがどのように動作するかを示します。

## <a name="creating-ios-applications-in-codeiosapp-fundamentalsios-code-onlymd"></a>[コードで iOS アプリケーションの作成](~/ios/app-fundamentals/ios-code-only.md)

この記事は、for mac Visual Studio および Visual Studio を使用してコードを完全に iOS アプリケーションを作成する方法を説明します。 UIKit からビューの階層を作成することでコント ローラーでアプリケーションの画面を構築する空のプロジェクト テンプレートからを起動する方法を示します。 その後、コント ローラーで読み込むことができるカスタム ビューを作成する方法について説明します。

## <a name="working-with-user-defaultsiosapp-fundamentalsuser-defaultsmd"></a>[ユーザーの既定値の操作](~/ios/app-fundamentals/user-defaults.md)

`NSUserDefaults`クラスは、ios アプリと拡張機能は、システム全体の既定のシステムとの対話方法を提供します。 既定でシステムを使用すると、ユーザーはアプリの動作または (アプリのデザインに基づく) の設定に応じたスタイル設定を構成できます。 たとえば、ヤード メトリックの vs でのデータを表示または指定した UI のテーマを選択します。

## <a name="touchiosapp-fundamentalstouchindexmd"></a>[タッチ](~/ios/app-fundamentals/touch/index.md)

現在のデバイスの多くのタッチ スクリーンでは、迅速かつ効率的に、自然で直感的な方法でデバイスと対話するようにします。 この連携では簡単なタッチの検出にのみ限定されません – もジェスチャを使用して行うことができます。 たとえば、ピンチ、ズーム ジェスチャは、はさむ、ユーザーは拡大または縮小する 2 本の指で画面の一部で – この非常に一般的な例を示します。このガイドでは、タッチとジェスチャで iOS を調べます。

## <a name="working-with-security-and-privacyiosapp-fundamentalssecurity-privacymd"></a>[セキュリティとプライバシーの扱い](~/ios/app-fundamentals/security-privacy.md)

Apple には、セキュリティとプライバシーに iOS 10 (以降) の両方に、開発者がアプリのセキュリティを強化し、エンドユーザーのプライバシーを確保に役立ついくつかの強化が行われます。 この記事では、Xamarin.iOS アプリでこれらの機能を実装することを説明します。

##  <a name="localizationiosapp-fundamentalslocalizationindexmd"></a>[ローカリゼーション](~/ios/app-fundamentals/localization/index.md)

ここでは、国際化をサポートするために、Xamarin.iOS アプリケーションへのエンコーディングを追加します。