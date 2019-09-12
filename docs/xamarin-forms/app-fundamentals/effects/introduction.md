---
title: エフェクトの概要
description: 効果を使用すると、各プラットフォーム上のネイティブ コントロールをカスタマイズできます。通常は、小規模なスタイル変更のために使用します。 この記事では、効果の概要、および効果とカスタム レンダラーの境界の概要について説明するほか、PlatformEffect クラスについて説明します。
ms.prod: xamarin
ms.assetid: 30CB8615-8F39-4762-BDB7-333D2B57D112
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: e9325c34c645b75f28c7e2070f6bb095780ddb02
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70771460"
---
# <a name="introduction-to-effects"></a>エフェクトの概要

_効果を使用すると、各プラットフォーム上のネイティブ コントロールをカスタマイズできます。通常は、小規模なスタイル変更のために使用します。この記事では、効果の概要、および効果とカスタム レンダラーの境界の概要について説明するほか、PlatformEffect クラスについて説明します。_

Xamarin.Forms の [Pages、Layouts、Controls](~/xamarin-forms/user-interface/controls/index.md) には、クロスプラットフォームのモバイル ユーザー インターフェイスを記述するための共通 API が用意されています。 各ページ、レイアウトおよびコントロールは、`Renderer` クラスを使用してプラットフォームごとに異なる方法でレンダリングされます。次に (Xamarin.Forms の処理形式に対応する) ネイティブ コントロールが作成され、画面に配置され、共有コードに指定された動作が追加されます。

開発者は独自の `Renderer` クラスを実装して、コントロールの外観や動作をカスタマイズできます。 ただし、カスタム レンダラー クラスを実装して簡単なコントロールのカスタマイズを実行すると、多くの場合、応答はヘビーウェイトになります。 効果はこのプロセスを簡略化し、各プラットフォーム上のネイティブ コントロールをより簡単にカスタマイズできるようにします。

効果は、プラットフォーム固有のプロジェクトで `PlatformEffect` コントロールをサブクラスかすることによって作成され、適切な Xamarin.Forms .NET Standard ライブラリまたは共有ライブラリ プロジェクトの適切なコントロールに添付することによって使用されます。

## <a name="why-use-an-effect-over-a-custom-renderer"></a>カスタム レンダラーを使用せずに効果を使用する理由

効果はコントロールのカスタマイズを簡略化し、再利用可能で、さらに再利用しやすくするためにパラメーター化できます。

効果で実現できることは、すべてカスタム レンダラーでも実現できます。 ただし、カスタム レンダラーは効果よりも柔軟性があり、よりカスタマイズできます。 次のガイドラインは、カスタム レンダラーよりも効果を選択する状況の一覧を示します。

- プラットフォーム固有のコントロールのプロパティを変更するときは、望みどおりの結果を実現するには効果を使用することをお勧めします。
- プラットフォーム固有のコントロールのメソッドをオーバーライドする必要があるときは、カスタム レンダラーが必要です。
- Xamarin.Forms コントロールを実装するプラットフォーム固有のコントロールを置換する必要があるときは、カスタム レンダラーが必要です。

## <a name="subclassing-the-platformeffect-class"></a>PlatformEffect クラスのサブクラス化

次の表に、各プラットフォーム上の `PlatformEffect` の名前空間と、そのプロパティの型の一覧を示します。

|プラットフォーム|名前空間|コンテナー|Control|
|--- |--- |--- |--- |
|iOS|Xamarin.Forms.Platform.iOS|UIView|UIView|
|Android|Xamarin.Forms.Platform.Android|ViewGroup|View|
|ユニバーサル Windows プラットフォーム (UWP)|Xamarin.Forms.Platform.UWP|FrameworkElement|FrameworkElement|

プラットフォーム固有の各 `PlatformEffect` クラスは次のプロパティを公開します。

- `Container` - レイアウトの実装に使用されるプラットフォーム固有のコントロールを参照します。
- `Control` - Xamarin.Forms コントロールの実装に使用される プラットフォーム固有のコントロールを参照します。
- `Element` - レンダリングされる Xamarin.Forms コントロールを参照します。

効果はあらゆる要素に添付できるため、添付されているコンテナー、コントロール、または要素に関する型情報がありません。 このため、効果がサポート外の要素に添付されるときは、サービス低下を許容するか、例外をスローするようにしてください。 ただし、`Container`、`Control`、`Element` の各プロパティは、実装する型にキャストすることができます。 これらの型について詳しくは、「[Renderer Base Classes and Native Controls](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)」(レンダラーの基底クラスとネイティブ コントロール) をご覧ください。

プラットフォーム固有の各 `PlatformEffect` クラスは、効果を実装するためにオーバーライドする必要がある、次のメソッドを公開します。

- [`OnAttached`](xref:Xamarin.Forms.Effect.OnAttached) - 効果が Xamarin.Forms コントロールに添付されると呼び出されます。 各プラットフォーム固有の効果クラス内でオーバーライドされたバージョンのこのメソッドは、コントロールのカスタマイズを実行する場所である一方で、その効果を指定された Xamarin.Forms コントロールに提供できない場合に例外を処理する場所です。
- [`OnDetached`](xref:Xamarin.Forms.Effect.OnDetached) - 効果が Xamarin.Forms コントロールからデタッチされると呼び出されます。 各プラットフォーム固有の効果クラス内でオーバーライドされたバージョンのこのメソッドは、イベント ハンドラーの登録解除など、あらゆる効果のクリーンアップを行う場所です。

さらに、`PlatformEffect` は [`OnElementPropertyChanged`](xref:Xamarin.Forms.PlatformEffect`2.OnElementPropertyChanged(System.ComponentModel.PropertyChangedEventArgs)) メソッドを公開します。こちらもオーバーライドできます。 このメソッドは要素のプロパティが変更されると呼び出されてます。 各プラットフォーム固有の効果クラス内でオーバーライドされたバージョンのこのメソッドは、Xamarin.Forms コントロール上でバインド可能なプロパティの変更に応答する場所です。 このオーバーライドは何度も呼び出されることがあるため、変更になったプロパティのチェックは常に行われる必要があります。

## <a name="related-links"></a>関連リンク

- [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
