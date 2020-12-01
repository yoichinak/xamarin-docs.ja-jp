---
title: Xamarin を使用したユーザーインターフェイスの構築
description: このドキュメントでは、Xamarin iOS アプリでユーザーインターフェイスを構築する方法について説明します。 IOS designer、storyboard、一般的な iOS インターフェイスの概念、iOS ユーザーインターフェイスコントロールに関するガイドへのリンクを提供します。
ms.prod: xamarin
ms.assetid: 2B3E45FA-C30F-D708-0E8F-3EE02BD1A867
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/21/2017
ms.openlocfilehash: 4d98e061ae935605613eb0d2a9c10b3866b610c9
ms.sourcegitcommit: d1f0e0a9100548cfe0960ed2225b979cc1d7c28f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/01/2020
ms.locfileid: "96439415"
---
# <a name="building-user-interfaces-with-xamarinios"></a>Xamarin を使用したユーザーインターフェイスの構築

## <a name="storyboards"></a>[ストーリーボード](~/ios/user-interface/storyboards/index.md)

ストーリーボードは、アプリケーションの外観とフローを視覚的に表現したものです。 Visual Studio for Mac を使用すると、Xcode Interface Builder と対話して、アプリケーション画面を視覚的にデザインできるだけでなく、ビュー、コントローラー、および C# のセグエにアクセスして、より詳細な制御を行うことができます。 

## <a name="ios-designer"></a>[iOS Designer](~/ios/user-interface/designer/index.md)

> [!WARNING]
> IOS Designer は、Visual Studio 2019 バージョン16.8 と Visual Studio 2019 for Mac バージョン8.8 で段階的に廃止される予定です。
> IOS ユーザーインターフェイスを構築する場合は、Xcode を実行している Mac 上で直接作成することをお勧めします。 詳細については、「 [Xcode を使用したユーザーインターフェイスの設計](~/ios/user-interface/storyboards/index.md)」を参照してください。 

Visual Studio for Mac に完全に統合された iOS ストーリーボード形式のデザイナーを構築しました。 IOS デザイナーは、ストーリーボード形式との完全な互換性を維持し、Xcode または Visual Studio for Mac でファイルを編集できるようにします。 また、エディターでは、デザイン時にレンダリングするカスタムコントロールなどの高度な機能がサポートされています。

## <a name="user-interface-in-ios"></a>[iOS のユーザー インターフェイス](~/ios/user-interface/ios-ui/index.md)

Xamarin. iOS アプリで iOS ユーザーインターフェイスを使用する方法について説明します。これには、表示 API、ユーザーインターフェイスオブジェクトの作成、レイアウトオプション、Haptic フィードバックの提供、UI スレッドの操作などが含まれます。

## <a name="user-interface-controls"></a>[ユーザーインターフェイスコントロール](~/ios/user-interface/controls/index.md)

Xamarin は、Apple によって提供されるすべてのネイティブユーザーインターフェイスオブジェクトを公開します。 IOS Designer、Xcode の Interface Builder、プログラムを使用して、Xamarin iOS アプリケーションに簡単に追加できます。 選択する方法に関係なく、Xamarin では、すべてのユーザーインターフェイスオブジェクトのプロパティとメソッドが C# で公開されます。
