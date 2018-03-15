---
title: "Objective-C 開発者向けの Xamarin"
description: "Objective-C 開発者は、C# のコードを再利用できる利点を活かしながら、Xamarin プラットフォームでの自分のスキルと既存の Objective-C コードの活用方法について学びます。 このセクションは Xamarin.iOS のエントリ ポイントとして機能し、C# に基づく既存の Objective-C コードの使用に関する豊富な情報を示します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 9F3C86A3-403E-4025-99CA-99FCA86DC828
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 275cce891801cd542d202960efc3da668fa8f07b
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="xamarin-for-objective-c-developers"></a>Objective-C 開発者向けの Xamarin

_Objective-C 開発者は、C# のコードを再利用できる利点を活かしながら、Xamarin プラットフォームでの自分のスキルと既存の Objective-C コードの活用方法について学ぶことになります。このセクションは Xamarin.iOS のエントリ ポイントとして機能し、C# に基づく既存の Objective-C コードの使用に関する豊富な情報を示します。_

Xamarin は Android via Xamarin.Android やさまざまな Windows など、使えるところであればどこでも C# が使用できるように、プラットフォームにとらわれない C# に非ユーザー インターフェイスを移すための iOS を対象とした開発者向けパスを提供します。 ただし、Xamarin で C# を使用したからといって、既存のスキルや Objective-C のコードを活用できなくなるわけではありません。 実際、Objective-C を理解することでより優れた Xamarin.iOS の開発者になることができます。Xamarin にはすべてのネイティブ iOS と、UIKit、Core Animation、Core Foundation、Core Graphics といった慣れ親しんだ OS X プラットフォームの API が公開されているためです。 同時に、LINQ や Generics などの機能を含む C# 言語の威力や、ネイティブ アプリケーションで使用できる .NET 基底クラスの豊富なライブラリを活用できます。

さらに、Xamarin ではバインドの技術を使用して既存の Objective-C のアセットも利用できます。 次の図で説明されているように、Objective-C で静的ライブラリを作成し、バインドを使用して C# に公開するだけです。

 [![](images/01-bindings.png "バインドを使用して C# に公開された Objective-C の静的ライブラリ")](images/01-bindings.png#lightbox)

これは非 UI コードに制限する必要はありません。 バインドでは Objective-C で開発されたユーザー インターフェイス コードも公開できます。

## <a name="transitioning-from-objective-c"></a>Objective-C からの移行

ドキュメント サイトには使い慣れたコードに C# のコードを統合することで Xamarin への移行を簡単にするための豊富な情報が含まれています。 次のような情報は特に使い始めに重要です。

-   [Objective-C 開発者向けの C# 入門](primer.md) - Xamarin と C# 言語への移行を検討している Objective-C 開発者向けの簡単な入門。 
-   [チュートリアル: Objective-C ライブラリのバインド](~/ios/platform/binding-objective-c/walkthrough.md) - Xamarin.iOS アプリケーションで既存の Objective-C コードを再利用するための手順に関するチュートリアル。 


## <a name="binding-objective-c"></a>Objective-C のバインド

C# と Objective-C の違いについて理解し、上記のチュートリアルでバインドについて確認したら、Xamarin プラットフォームに移行する準備ができました。 フォローアップとして、全体的なバインドの参照を含む Xamarin.iOS のバインド技術に関する詳細情報を、[Objective-C のバインド](~/ios/platform/binding-objective-c/index.md)のセクションで確認できます。

## <a name="cross-platform-development"></a>プラットフォーム間の開発

最後に、Xamarin.iOS への移行後、開発した参照アプリケーションのケース スタディ、「[Building Cross Platform Applications section](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)」(クロスプラットフォーム アプリケーションのビルド) のセクションに含まれる再利用可能なクロスプラットフォーム コードの作成に関するベスト プラクティスなどのクロスプラットフォームのガイダンスについて確認できます。
