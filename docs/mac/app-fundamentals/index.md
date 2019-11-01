---
title: Xamarin. Mac アプリケーションの基礎
description: このドキュメントでは、Xamarin アプリケーションを開発するときに理解しておく必要があるさまざまな概念について説明しているガイドにリンクしています。
ms.prod: xamarin
ms.assetid: 5A36B3A7-F197-4AC3-A40D-B2C49362FF06
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 12/17/2015
ms.openlocfilehash: 2603360162ee9918e83b9f5c74b8086f71d02df8
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030096"
---
# <a name="xamarinmac-application-fundamentals"></a>Xamarin. Mac アプリケーションの基礎

## <a name="common-patterns-and-idiomsmacapp-fundamentalspatternsmd"></a>[一般的なパターンと表現](~/mac/app-fundamentals/patterns.md)

を通じて公開されC#ている Apple api 全体を通じて、特定の表現とパターンが繰り返し使用されます。 Xamarin. iOS を使用したプログラミングの経験がある場合は、これらの機能がよく見られます。 ドキュメントでは、これらのパターンや表現を繰り返し参照することがよくあるため、これらを十分に理解しておくと、見つけたドキュメントを理解するのに役立ちます。

## <a name="understanding-mac-apismacapp-fundamentalsmac-apismd"></a>[Mac Api について](~/mac/app-fundamentals/mac-apis.md)

Xamarin. Mac を使用した開発時間の大部分では、基礎となる目標 C C# api に関してあまり心配をすることなく、お客様が検討、読み取り、および書き込みを行うことができます。 ただし、場合によっては、Apple から API ドキュメントを読み取り、Stack Overflow からの回答を問題の解決策に変換するか、既存のサンプルと比較する必要があります。

## <a name="console-appsmacapp-fundamentalsconsolemd"></a>[コンソール アプリ](~/mac/app-fundamentals/console.md)

また、Xamarin を使用してネイティブ macOS Api にアクセスする "ヘッドレス" コンソールアプリを構築することもできます。

## <a name="working-with-xib-filesmacapp-fundamentalsxibmd"></a>[Xib ファイルの操作](~/mac/app-fundamentals/xib.md)

この記事では、Xcode の Interface Builder で作成された xib ファイルを使用して、Xamarin アプリケーションのユーザーインターフェイスを作成および管理する方法について説明します。

## <a name="storyboardxib-less-user-interface-designmacapp-fundamentalsxibless-uimd"></a>[. storyboard/xib less ユーザーインターフェイスの設計](~/mac/app-fundamentals/xibless-ui.md)

この記事では、Xcode の Interface Builder を storyboard または xib ファイルC#と共に使用せずに、コードから直接 Xamarin. Mac アプリケーションのユーザーインターフェイスを作成する方法について説明します。

## <a name="working-with-imagesmacapp-fundamentalsimagemd"></a>[イメージの操作](~/mac/app-fundamentals/image.md)

この記事では、Xamarin. Mac アプリケーションでのイメージとアイコンの使用について説明します。 この記事では、アプリケーションのアイコンを作成し、コードと Xcode の両方C#の Interface Builder でイメージを使用するために必要なイメージの作成と管理について説明します。

## <a name="data-binding-and-key-value-codingmacapp-fundamentalsdatabindingmd"></a>[データバインディングとキー値のコーディング](~/mac/app-fundamentals/databinding.md)

この記事では、キーと値のコードを使用して、Xcode の Interface Builder の UI 要素にデータをバインドできるようにするためのキーと値の監視について説明します。 この手法を使用すると、Xamarin. Mac C#アプリケーション用に書き込む必要があるコードの量を大幅に削減できます。 

## <a name="working-with-databasesmacapp-fundamentalsdatabasesmd"></a>[データベースの操作](~/mac/app-fundamentals/databases.md)

この記事では、キー値のコードとキー値の監視を使用して、データバインディングで Xcode の Interface Builder の UI 要素に対して SQLite データベースへの直接アクセスを許可する方法について説明します。 また、SQLite.NET ORM を使用して SQLite データへのアクセスを提供する方法についても説明します。

## <a name="working-with-copy-and-pastemacapp-fundamentalscopy-pastemd"></a>[コピーと貼り付けの操作](~/mac/app-fundamentals/copy-paste.md)

この記事では、ペーストボードを使用して、Xamarin. Mac アプリケーションにコピーと貼り付けを行う方法について説明します。 ここでは、複数のアプリ間で共有できる標準データ型の操作方法と、アプリ内でカスタムデータをサポートする方法を示します。

## <a name="sandboxing-a-xamarinmac-appmacapp-fundamentalssandboxingmd"></a>[Xamarin. Mac アプリのサンドボックス化](~/mac/app-fundamentals/sandboxing.md)

この記事では、App Store でリリースするための Xamarin. Mac アプリケーションのサンドボックスについて説明します。 ここでは、サンドボックスに含まれるすべての要素 (コンテナーディレクトリ、権利、ユーザーによって決定されるアクセス許可、特権の分離、およびカーネルの強制) について説明します。

## <a name="playing-sound-with-avaudioplayermacapp-fundamentalssoundsmd"></a>[AVAudioPlayer でのサウンドの再生](~/mac/app-fundamentals/sounds.md)

この記事では、ヘルパークラスを使用して、AVAudioPlayer を使用してサウンドの再生を制御する方法について説明します。

## <a name="reporting-bugsmacapp-fundamentalstroubleshootingmd"></a>[バグの報告](~/mac/app-fundamentals/troubleshooting.md)

場合によっては、API を手に入れたり、バグに対処したりすることができないときに、プロジェクトの作業中に行き詰まってしまうことがあります。 Xamarin での目標は、モバイルアプリケーションとデスクトップアプリケーションの作成に成功し、役に立ちます。
