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

Xamarin. iOS と Xamarin. Mac の両方の設計では、API を使用して、ネイティブ .NET 文字列型の公開、`string`、 C#およびとその他の .net プログラミング言語の文字列操作を行い、文字列を api によって公開されるデータ型として公開します。 `NSString` データ型です。

つまり、開発者は Xamarin の呼び出しに使用するための文字列を保持する必要がないことを意味します。 iOS & Xamarin. Mac API (統合) は特殊な種類 (`Foundation.NSString`) で使用でき、すべての操作に Mono の `System.String` を使用したままにすることができます。Xamarin. iOS または Xamarin. Mac には文字列が必要です。 API バインディングでは、情報のマーシャリングが行われます。

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

背後では、このプロパティの実装によってC#文字列が`NSString`にマーシャリングされ、目的の C と同じ方法で`objc_msgSend`メソッドが呼び出されます。

`NSString`を使用せず、代わりに C 文字列 ("*char*") を使用するサードパーティの目的 c api がいくつかあります。 このような場合でも、 C#文字列データ型を使用できますが、この文字列を`NSString`としてマーシャリングし、代わりに C 文字列としてマーシャリングしないことをバインドジェネレーターに通知するには、 [[PlainString]](~/cross-platform/macios/binding/objective-c-libraries.md)属性を使用する必要があります。

 <a name="Exceptions_to_the_Rule" />

## <a name="exceptions-to-the-rule"></a>ルールの例外

Xamarin. iOS と Xamarin. Mac の両方で、このルールの例外を作成しました。  `string`を公開するタイミングと、を除き、 `NSString`を公開するかどうかの決定は、 `NSString` メソッドがコンテンツ比較ではなくポインター比較を行う可能性がある場合に行います。

これは、目的の C Api が、文字列の実際の内容を比較するのではなく、何らかのアクションを表すトークンとしてパブリック `NSString` 定数を使用する場合に発生する可能性があります。

このような場合、`NSString` Api が公開されており、これを持つ少数の Api があります。 また、NSString プロパティが一部のクラスで公開されていることにも注意してください。 これらの `NSString` のプロパティは、通知などの項目に対して公開されます。 これらのプロパティは、通常、次のようになります。

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

`NSString`s が API で公開されるもう1つの場所は、`NSDictionary` オブジェクトをパラメーターとして受け取る、iOS または OS X の特定の Api のパラメーターとして使用されるトークンです。 通常、ディクショナリには `NSString` キーが含まれます。 慣例により、"Key" という名前を追加することによって、これらの静的 `NSString` プロパティに名前を付けます。
