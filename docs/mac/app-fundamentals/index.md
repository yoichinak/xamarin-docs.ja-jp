---
title: Xamarin.Mac アプリケーションの基礎
description: このドキュメントは、Xamarin.Mac アプリケーションを開発する際に理解するために必要なさまざまな概念について説明したガイドにリンクしています。
ms.prod: xamarin
ms.assetid: 5A36B3A7-F197-4AC3-A40D-B2C49362FF06
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 12/17/2015
ms.openlocfilehash: 376286b73c92cba40de183043b86cb4ffb5e699d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108718"
---
# <a name="xamarinmac-application-fundamentals"></a>Xamarin.Mac アプリケーションの基礎

## <a name="common-patterns-and-idiomsmacapp-fundamentalspatternsmd"></a>[共通のパターンと成句](~/mac/app-fundamentals/patterns.md)

C# を使用して公開されている Apple Api、全体で特定の表現形式とパターンが何度も。 Xamarin.iOS を使用したプログラミングの経験がある場合は、これらが使い慣れた見えることがあります。 ドキュメントは多くの場合を参照してこれらのパターンと成句、繰り返しがそれらを確実に理解に役立つ検索するドキュメントを理解するため。

## <a name="understanding-mac-apismacapp-fundamentalsmac-apismd"></a>[Understanding Mac Api](~/mac/app-fundamentals/mac-apis.md)

Xamarin.Mac を使って開発時間の大部分を考えて、読み取り、および基になる Objective C Api を使用した量に関係なく、c# で記述することができます。 ただし、場合があります必要がありますを apple の API のドキュメントを読み取る、問題のソリューションには、Stack Overflow から回答を変換したりする既存のサンプルと比較します。

## <a name="console-appsmacapp-fundamentalsconsolemd"></a>[コンソール アプリ](~/mac/app-fundamentals/console.md)

ネイティブ macOS Api にアクセスする「ヘッドレス」コンソール アプリケーションを構築することもできます。 Xamarin.Mac を使用します。

## <a name="working-with-xib-filesmacapp-fundamentalsxibmd"></a>[.Xib ファイルの使用](~/mac/app-fundamentals/xib.md)

この記事では、.xib ファイルを作成して、Xamarin.Mac アプリケーションのユーザー インターフェイスを管理する Xcode の Interface Builder で作成した作業について説明します。

## <a name="storyboardxib-less-user-interface-designmacapp-fundamentalsxibless-uimd"></a>[ユーザー インターフェイスの設計より.storyboard/.xib](~/mac/app-fundamentals/xibless-ui.md)

この記事では、c# コードから直接 .storyboard または .xib ファイルと Xcode の Interface Builder を使用せず、Xamarin.Mac アプリケーションのユーザー インターフェイスを作成するについて説明します。

## <a name="working-with-imagesmacapp-fundamentalsimagemd"></a>[画像の操作](~/mac/app-fundamentals/image.md)

この記事では、イメージとアイコンは、Xamarin.Mac アプリケーションでの操作について説明します。 作成について説明し、アプリケーションのアイコンと c# コードと Xcode の Interface Builder の両方でイメージを使用して作成するために必要なイメージを維持します。

## <a name="data-binding-and-key-value-codingmacapp-fundamentalsdatabindingmd"></a>[データ バインディングとキー値コーディング](~/mac/app-fundamentals/databinding.md)

この記事では、キーと値のコーディングと観察の Xcode の Interface Builder での UI 要素へのデータ バインディングを許可するキーと値を使用してについて説明します。 大幅に書き込む必要がある、Xamarin.Mac アプリケーションの c# コードの量を減らすこの手法を使用して。 

## <a name="working-with-databasesmacapp-fundamentalsdatabasesmd"></a>[データベースの操作](~/mac/app-fundamentals/databases.md)

この記事では、キーと値のコーディングと Xcode の Interface Builder での UI 要素に SQLite データベースに直接アクセスによるデータ バインディングのために観察キーと値を使用してについて説明します。 SQLite.NET ORM を使用して SQLite データへのアクセスを提供することも説明します。

## <a name="working-with-copy-and-pastemacapp-fundamentalscopy-pastemd"></a>[コピーと貼り付けの操作](~/mac/app-fundamentals/copy-paste.md)

この記事では、コピーを提供して、Xamarin.Mac アプリケーションで貼り付けるクリップボードの操作について説明します。 作業する方法を示しますによりアプリ内でカスタム データをサポートする方法と複数のアプリ間で共有できる標準的なデータ型。

## <a name="sandboxing-a-xamarinmac-appmacapp-fundamentalssandboxingmd"></a>[Xamarin.Mac アプリのサンド ボックス化](~/mac/app-fundamentals/sandboxing.md)

この記事では、App Store でのリリース用の Xamarin.Mac アプリケーションのサンド ボックス化について説明します。 すべてのサンド ボックスに移動する要素について説明します。 コンテナー ディレクトリ、権利、ユーザー指定のアクセス許可、特権の分離、およびカーネルの強制実行します。

## <a name="playing-sound-with-avaudioplayermacapp-fundamentalssoundsmd"></a>[AVAudioPlayer でのサウンドの再生](~/mac/app-fundamentals/sounds.md)

この記事では、ヘルパー クラスを使用して、AVAudioPlayer を使用してサウンドの再生を制御する方法を示します。

## <a name="reporting-bugsmacapp-fundamentalstroubleshootingmd"></a>[バグの報告](~/mac/app-fundamentals/troubleshooting.md)

希望方法を操作する API を取得することができない、またはバグを回避しようとしています。 で、プロジェクトで作業中にスタックでしょう。 Xamarin の目標は、モバイルおよびデスクトップ アプリケーションの作成に成功することですし、リソースについてご紹介します。
