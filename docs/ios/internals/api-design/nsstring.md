---
title: Xamarin. iOS と Xamarin. Mac の NSString
description: このドキュメントでは、Xamarin. iOS が NSString オブジェクトをC#文字列オブジェクトに透過的に変換する方法について説明します。
ms.prod: xamarin
ms.assetid: 785744B3-42E2-4590-8F41-435325E609B9
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: f744f4ed5619e4e7f4a9d85897c4451bf7e5b9bc
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022349"
---
# <a name="nsstring-in-xamarinios-and-xamarinmac"></a>Xamarin. iOS と Xamarin. Mac の NSString

Xamarin.iOS と xamarin.Mac の両方の設計では、API を使用して、C# およびその他の .NET プログラミング言語の文字列操作のために、文字列を API によって公開するデータ型として、`string` `NSString`データ型の代わりに、ネイティブ .NET 文字列型の を公開します。

このため、開発者は Xamarin の呼び出しに使用することを意図した文字列を保持する必要がないことに注意してください。 特殊な種類の (`Foundation.NSString`) iOS & Xamarin.Mac API (統合) は、すべての操作に Mono の `System.String` を使用し続けることができます。Xamarin. iOS または Xamarin.Mac の API に文字列が必要な場合はいつでも、API バインディングでは情報のマーシャリングが処理されます。

たとえば、`NSString`型の `UILabel` の "text" プロパティの目的は次のように宣言されています。

```objc
@property(nonatomic, copy) NSString *text
```

これは Xamarin. iOS で次のように公開されています。

```csharp
class UILabel {
    public string Text { get; set; }
}
```

背後では、このプロパティの実装によって C# 文字列が `NSString` にマーシャリングされ、Objective-C と同じ方法で `objc_msgSend` メソッドが呼び出されます。

`NSString` を使用せず、代わりに C 文字列 ("*char*") を使用するサードパーティの Objective-C API がいくつかあります。 この場合でも、引き続き C# 文字列データ型を使用できますが、この文字列を [ としてマーシャリングせず、代わりに C 文字列としてマーシャリングする必要があることをバインディング ジェネレーターに通知するには、 ](~/cross-platform/macios/binding/objective-c-libraries.md)[PlainString]`NSString` 属性を使用する必要があります。

 <a name="Exceptions_to_the_Rule" />

## <a name="exceptions-to-the-rule"></a>ルールの例外

Xamarin. iOS と Xamarin. Mac の両方で、このルールの例外を作成しました。  `string`を公開するタイミングと、を除き、 `NSString`を公開するかどうかの決定は、 `NSString` メソッドがコンテンツ比較ではなくポインター比較を行う可能性がある場合に行います。

これは、Objective-C API が、文字列の実際の内容を比較するのではなく、何らかのアクションを表すトークンとしてパブリック  `NSString` 定数を使用する場合に発生する可能性があります。

そのような場合、`NSString`  API が公開されており、これを持つ少数の API があります。 また、NSString プロパティが一部のクラスで公開されていることにも注意してください。 これら `NSString` のプロパティは、通知などの項目に対して公開されます。 これらのプロパティは、通常、次のようになります。

```csharp
class Foo {
     public NSString FooNotification { get; }
}
```

通知は、ランタイムによってブロードキャストされる特定のイベントに登録するときに、`NSNotification` クラスに使用されるキーです。

通常、キーは次のようになります。

```csharp
class Foo {
     public NSString FooBarKey { get; }
}
```

`NSString` が API で公開されている別の場所は、パラメーターとして `NSDictionary` オブジェクトを受け取る iOS または OS X の特定の API のパラメーターとして使用されるトークンです。 このディクショナリには通常、`NSString` キーが含まれています。 Xamarin.iOS では慣例、これらの静的 `NSString` プロパティに "Key" 名を追加することによって名前を付けます。
