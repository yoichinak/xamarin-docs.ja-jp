---
title: "アプリケーションの基礎"
description: "このドキュメントには、Xamarin.Mac アプリケーションを開発する場合に理解するために必要なさまざまな概念について説明したガイドへのリンクがします。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5A36B3A7-F197-4AC3-A40D-B2C49362FF06
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/17/2015
ms.openlocfilehash: 9b64647fdb455978c3833ce1bc37f5e8784b9a78
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="application-fundamentals"></a>アプリケーションの基礎

## <a name="common-patterns-and-idiomsmacapp-fundamentalspatternsmd"></a>[共通のパターンおよび手法](~/mac/app-fundamentals/patterns.md)

C# を使用して公開されている Apple Api、全体で特定の表現形式とパターンつなげて何度もします。 Xamarin.iOS を使用したプログラミングの経験があれば、これらはおなじみです。 ドキュメントは多くの場合を参照してこれらのパターンと表現方法、繰り返しため、それらの実線に理解することはできますが見つかったら、ドキュメントの意味をなします。

## <a name="understanding-mac-apismacapp-fundamentalsmac-apismd"></a>[Understanding Mac Api](~/mac/app-fundamentals/mac-apis.md)

Xamarin.Mac による開発時間の大部分と思われる、読み取り、および基になる Objective C Api と程度を気にせず、c# で記述することができます。 ただし、場合がありますを必要があります、Apple から API のドキュメントを読み取る、問題のソリューションにスタック オーバーフローからの応答を変換または比較する既存のサンプルにします。

## <a name="working-with-xib-filesmacapp-fundamentalsxibmd"></a>[.Xib ファイルの操作](~/mac/app-fundamentals/xib.md)

この記事では、Xcode のインターフェイスのビルダーを作成および維持 Xamarin.Mac アプリケーションのユーザー インターフェイスに作成された .xib ファイルと作業について説明します。

## <a name="storyboardxib-less-user-interface-designmacapp-fundamentalsxibless-uimd"></a>[ユーザー インターフェイス デザイン以下.storyboard/.xib](~/mac/app-fundamentals/xibless-ui.md)

この記事では、.storyboard または .xib ファイルで Xcode のインターフェイスのビルダーを使用せずに、c# コードから直接 Xamarin.Mac アプリケーションのユーザー インターフェイスを作成するについて説明します。

## <a name="working-with-imagesmacapp-fundamentalsimagemd"></a>[画像の操作](~/mac/app-fundamentals/image.md)

この記事では、イメージやアイコン Xamarin.Mac アプリケーションでの操作について説明します。 作成について説明し、アプリケーションのアイコンと c# コードと Xcode のインターフェイスのビルダーの両方でイメージを使用して作成するために必要なイメージを維持します。

## <a name="data-binding-and-key-value-codingmacapp-fundamentalsdatabindingmd"></a>[データのバインドとキー値のコーディング](~/mac/app-fundamentals/databinding.md)

この記事では、キーと値のコーディングおよびキーと値の Xcode のインターフェイスのビルダーでの UI 要素にデータ バインドを許可する確認の使用方法について説明します。 大幅に書き込む必要がある Xamarin.Mac アプリケーションの c# コードの量を減らすこの手法を使用して。 

## <a name="working-with-databasesmacapp-fundamentalsdatabasesmd"></a>[データベースの操作](~/mac/app-fundamentals/databases.md)

この記事では、キーと値のコーディングおよびキーと値の Xcode のインターフェイスのビルダーでの UI 要素に SQLite データベースに直接アクセスできるデータのバインドを許可する確認の使用方法について説明します。 また、SQLite.NET ORM を使用して、SQLite データへのアクセスを提供するについても説明します。

## <a name="working-with-copy-and-pastemacapp-fundamentalscopy-pastemd"></a>[コピーと貼り付けの操作](~/mac/app-fundamentals/copy-paste.md)

この記事では、コピーを提供し、Xamarin.Mac アプリケーションに貼り付けますペーストの扱いについて説明します。 作業する方法を示しますによりアプリ内でカスタム データをサポートする方法と複数のアプリ間で共有できる標準のデータ型にします。

## <a name="sandboxing-a-xamarinmac-appmacapp-fundamentalssandboxingmd"></a>[サンド ボックス Xamarin.Mac アプリ](~/mac/app-fundamentals/sandboxing.md)

この記事では、サンド ボックスでは、アプリ ストアのリリースの Xamarin.Mac アプリケーションについて説明します。 サンド ボックスに移動した要素のすべてに対応します。 コンテナー ディレクトリ、資格、ユーザー指定のアクセス許可、特権の分離、およびカーネル実施します。

## <a name="playing-sound-with-avaudioplayermacapp-fundamentalssoundsmd"></a>[AVAudioPlayer でサウンドの再生](~/mac/app-fundamentals/sounds.md)

この記事では、ヘルパー クラスを使用して、AVAudioPlayer を使用してサウンドの再生を制御する方法を示します。

## <a name="reporting-bugsmacapp-fundamentalstroubleshootingmd"></a>[バグの報告](~/mac/app-fundamentals/troubleshooting.md)

場合があります、希望方法を使用する API を取得することができない、またはバグを回避しようとしています。 で、プロジェクトで作業中にスタックしている全員を取得します。 Xamarin の目標は、ユーザーが、モバイル、デスクトップ アプリケーションの作成に成功して、支援何らかのリソースが提供されています。
