---
title: Objective-C 開発者向けの Xamarin
description: このドキュメントでは、Objective-C 開発者を対象に Xamarin.iOS について説明しています。 Objective-C から C# に移行する方法、C# で使用するために Objective-C ライブラリをバインドする方法、クロスプラットフォーム モバイル アプリケーションをビルドする方法について説明したガイドにリンクされています。
ms.prod: xamarin
ms.assetid: 9F3C86A3-403E-4025-99CA-99FCA86DC828
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/05/2017
ms.openlocfilehash: 2c5451b379968230d75599601052e3a33f5b5951
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022441"
---
# <a name="xamarin-for-objective-c-developers"></a>Objective-C 開発者向けの Xamarin

iOS を対象とした開発者は Xamarin を使用し、 ユーザーインターフェイス 以外のコードをプラットフォームに依存しないC＃に移行する事で、Xamarin.Android を利用した Android や様々な Windows など、C#が使用可能なプラットフォームであればどこでもそのコードを使用できるようなります。 ただし、Xamarin で C# を使用したからといって、既存のスキルや Objective-C のコードを活用できなくなるわけではありません。 実際、Objective-C を理解することでより優れた Xamarin.iOS の開発者になることができます。Xamarin にはすべてのネイティブ iOS と、UIKit、Core Animation、Core Foundation、Core Graphics といった慣れ親しんだ OS X プラットフォームの API が公開されているためです。 同時に、LINQ や Generics などの機能を含む C# 言語の威力や、ネイティブ アプリケーションで使用できる .NET 基底クラスの豊富なライブラリを活用できます。

さらに、Xamarin ではバインディングを使用して既存の Objective-C のアセットも利用できます。 次の図で説明されているように、Objective-C で静的ライブラリを作成し、バインディングを使用して C# に公開するだけです。

 [![](images/01-bindings.png "A static library in Objective-C exposed to C# via a binding")](images/01-bindings.png#lightbox)

これは非 UI コードに制限する必要はありません。 バインドでは Objective-C で開発されたユーザー インターフェイス コードも公開できます。

## <a name="transitioning-from-objective-c"></a>Objective-C からの移行

ドキュメント サイトには Xamarin への移行を簡単にするための情報が多数用意されており、C# コードを使い慣れたコードに統合する方法が示されています。 以下のような情報は特に使い始めに重要です。

- [Objective-C 開発者向けの C# 入門](primer.md) - Xamarin と C# への移行を検討している Objective-C 開発者向けの簡単な入門。 
- [チュートリアル: Objective-C ライブラリのバインディング](~/ios/platform/binding-objective-c/walkthrough.md) - Xamarin.iOS アプリケーションで既存の Objective-C コードを再利用するための手順に関するチュートリアル。 

## <a name="binding-objective-c"></a>Objective-C のバインディング

C# と Objective-C の違いについて理解し、上記のチュートリアルでバインディングについて確認したなら、Xamarin プラットフォームに移行する準備は整いました。 フォローアップとして、全体的なバインディングの参照を含む Xamarin.iOS のバインディング技術に関する詳細情報を、[Objective-C のバインディング](~/ios/platform/binding-objective-c/index.md)のセクションで確認できます。

## <a name="cross-platform-development"></a>プラットフォーム間の開発

最後に、Xamarin.iOS への移行後は、クロスプラットフォームに関するガイダンスを確認できます。これには、開発した参照アプリケーションのケーススタディや、[クロスプラットフォーム アプリケーションの構築のセクション](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)に含まれている再利用可能なクロスプラットフォーム コードを作成するためのベストプラクティスが含まれています。
