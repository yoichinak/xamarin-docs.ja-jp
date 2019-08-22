---
title: Xamarin. iOS と Xamarin. Mac の NSString
description: このドキュメントでは、Xamarin. iOS が NSString オブジェクトをC#文字列オブジェクトに透過的に変換する方法について説明します。
ms.prod: xamarin
ms.assetid: 785744B3-42E2-4590-8F41-435325E609B9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 589b584e0526ffdc56dd5ec26aa25a0b61e2141a
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/21/2019
ms.locfileid: "69889902"
---
# <a name="nsstring-in-xamarinios-and-xamarinmac"></a>Xamarin. iOS と Xamarin. Mac の NSString

Xamarin. iOS と xamarin. Mac の両方の設計では、api を使用して、ネイティブ .net 文字列型`string`の、、 C#およびその他の .net プログラミング言語の文字列操作を公開し、文字列を api によって公開されるデータ型として公開します。 `NSString`データ型 。

このため、開発者は xamarin の呼び出しに使用することを意図した文字列を保持する必要がないことに注意してください。 iOS & xamarin.`Foundation.NSString`Mac API (統合) は特殊な`System.String`種類 () では、すべての操作に Mono を使用し続けることができます。Xamarin. iOS または Xamarin. Mac の API には、文字列が必要です。 API バインディングでは、情報のマーシャリングが行われます。

たとえば、型`UILabel` `NSString`のの目的の C "text" プロパティは次のように宣言されています。

```objc
@property(nonatomic, copy) NSString *text
```

これは Xamarin. iOS で次のように公開されています。

```csharp
class UILabel {
    public string Text { get; set; }
}
```

背後では、このプロパティの実装によってC#文字列`NSString`がにマーシャリングされ`objc_msgSend` 、目的の C と同じ方法でメソッドが呼び出されます。

を使用`NSString`せず、代わりに c 文字列 ("*char*") を使用するサードパーティの目的 c api がいくつかあります。 このような場合でも、 C#文字列データ型を使用できますが、この文字列をとして`NSString`マーシャリングし、代わりに C 文字列としてマーシャリングする必要があることをバインディングジェネレーターに通知するには、 [[PlainString]](~/cross-platform/macios/binding/objective-c-libraries.md)属性を使用する必要があります。

 <a name="Exceptions_to_the_Rule" />

## <a name="exceptions-to-the-rule"></a>ルールの例外

Xamarin. iOS と Xamarin. Mac の両方で、このルールの例外を作成しました。 を公開 `string`するときと、を除くと公開 `NSString`するときの決定は、 メソッドがコンテンツ比較では `NSString`なくポインター比較を実行できる場合に作成されます。

これは、目的の C api が、文字列の実際 `NSString`の内容を比較するのではなく、何らかのアクションを表すトークンとしてパブリック 定数を使用する場合に発生する可能性があります。

そのような場合`NSString`  は、api が公開されており、これを持つ少数の api があります。 また、NSString プロパティが一部のクラスで公開されていることにも注意してください。 これら`NSString`のプロパティは、通知などの項目に対して公開されます。 これらのプロパティは、通常、次のようになります。

```csharp
class Foo {
     public NSString FooNotification { get; }
}
```

通知は、ランタイムによってブロードキャスト`NSNotification`される特定のイベントに登録するときに、クラスで使用されるキーです。

通常、キーは次のようになります。

```csharp
class Foo {
     public NSString FooBarKey { get; }
}
```

`NSString`S が API で公開されている別の場所は、パラメーターとしてオブジェクトを受け取る`NSDictionary` iOS または OS X の特定の api のパラメーターとして使用されるトークンです。 ディクショナリには、 `NSString`通常、キーが含まれています。 慣例により、"Key" という名前`NSString`を追加することで、これらの静的プロパティに名前を付けます。
