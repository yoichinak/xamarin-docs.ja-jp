---
title: Xamarin.iOS と Xamarin.Mac での NSString
description: このドキュメントでは、Xamarin.iOS によって NSString オブジェクトが C# の文字列オブジェクトに透過的に変換される方法と、それが行われない場合について説明します。
ms.prod: xamarin
ms.assetid: 785744B3-42E2-4590-8F41-435325E609B9
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: f744f4ed5619e4e7f4a9d85897c4451bf7e5b9bc
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "73022349"
---
# <a name="nsstring-in-xamarinios-and-xamarinmac"></a>Xamarin.iOS と Xamarin.Mac での NSString

Xamarin.iOS と Xamarin.Mac のどちらの設計でも、API を使用して、C# と他の .NET プログラミング言語での文字列操作のためのネイティブの .NET 文字列 `string` が公開され、 `NSString` データ型ではなく API によって公開されるデータ型として文字列が公開されます。

つまり、開発者は、特殊な型 (`Foundation.NSString`) で Xamarin.iOS および Xamarin.Mac の API (統一) の呼び出しに使用するための文字列を保持する必要はなく、すべての操作に Mono の `System.String` を使用し続けることができます。また、Xamarin.iOS または Xamarin.Mac の API で文字列が必要なときはいつでも、API バインドによって情報のマーシャリングが行われます。

たとえば、`NSString` 型の `UILabel` での Objective-C の "text" プロパティは、次のように宣言されます。

```objc
@property(nonatomic, copy) NSString *text
```

これは、Xamarin.iOS では次のように公開されます。

```csharp
class UILabel {
    public string Text { get; set; }
}
```

見えないところで、このプロパティの実装によって C# の文字列が `NSString` にマーシャリングされ、Objective-C と同じ方法で `objc_msgSend` メソッドが呼び出されます。

`NSString` を使う代わりに C の文字列 ("*char*") を使っているサードパーティの Objective-C API がいくつかあります。 そのような場合でも、C# の文字列データ型を使用できますが、この文字列を `NSString` としてマーシャリングするのではなく、C 文字列としてマーシャリングする必要があることを、[[PlainString]](~/cross-platform/macios/binding/objective-c-libraries.md) 属性を使用して、バインド ジェネレーターに通知する必要があります。

 <a name="Exceptions_to_the_Rule" />

## <a name="exceptions-to-the-rule"></a>ルールの例外

Xamarin.iOS と Xamarin.Mac の両方で、このルールに対する例外が設けられています。  `NSString` メソッドがコンテンツ比較ではなくポインター比較を行う可能性がある場合、 `string` を公開するか、例外的に `NSString` を公開するかの決定が行われます。

これは、Objective-C API で、文字列の実際の内容を比較するのではなく、何らかのアクションを表すトークンとしてパブリック `NSString` 定数が使用されている場合に、発生する可能性があります。

そのような場合は、`NSString` API が公開され、これを使用する少数の API があります。 また、NSString プロパティが一部のクラスで公開されていることにも注意してください。 それらの `NSString` プロパティは、通知などの項目に対して公開されます。 それらのプロパティは、通常、次のようになります。

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

`NSString` が API で公開されるもう 1 つの場合は、`NSDictionary` オブジェクトをパラメーターとして受け取る iOS または OS X の特定の API に対するパラメーターとして使用されるトークンです。 通常、ディクショナリには `NSString` キーが含まれます。 Xamarin.iOS では、慣例により、そのような静的 `NSString` プロパティの名前に "Key" が付加されます。
