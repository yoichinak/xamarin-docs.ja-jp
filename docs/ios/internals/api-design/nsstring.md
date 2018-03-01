---
title: NSString
ms.topic: article
ms.prod: xamarin
ms.assetid: BB99EBD7-308A-C865-1829-4DFFDB1BBCA4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: b52a9e926f5ec907746d2a2dd8ee7a6a6212742f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="nsstring"></a>NSString

ネイティブ .NET 文字列型の公開 API を使用して呼び出す Xamarin.iOS と Xamarin.Mac の両方の設計`string`、c# での文字列操作や他の .NET プログラミング言語におよび、ではなくAPIによって公開されるデータ型として文字列を公開するには`NSString`データ型。


これは意味を開発者は、Xamarin.iOS と Xamarin.Mac API (統合) を呼び出すことに使用するためのものでは、文字列を保持する必要がありますいない特殊な種類の (`Foundation.NSString`) を使用してモノラルを保持できます`System.String`のすべての操作をときにいつ当社の API バインドはマーシャ リング、情報の処理、Xamarin.iOS または Xamarin.Mac API に文字列が必要です。

たとえば、OBJECTIVE-C"text"のプロパティ、`UILabel`型の`NSString`が次のように宣言されています。

```csharp
@property(nonatomic, copy) NSString *text
```

これは、として Xamarin.iOS で公開されています。

```csharp
class UILabel {
    public string Text { get; set; }
}
```

背後では、このプロパティの実装に c# の文字列をマーシャ リング、`NSString`を呼び出すと、 `objc_msgSend` Objective C と同じ方法でメソッドです。

いくつかのサード パーティ製 Objective C Api を使用しませんが、`NSString`は代わりに、C の文字列を使用 (、"*char*") です。 ような場合は、c# の文字列データ型も使用できますが、使用する必要があります、 [[PlainString]](~/cross-platform/macios/binding/objective-c-libraries.md)属性としてこの文字列をマーシャ リングする必要がありますいないバインド ジェネレーターを通知するために、 `NSString`、ですが、C の文字列として代わりにします。

 <a name="Exceptions_to_the_Rule" />


## <a name="exceptions-to-the-rule"></a>規則の例外

Xamarin.iOS と Xamarin.Mac の両方では、この規則の例外を行いました。 公開おとの間で意思決定`string`s を作成する場合と、except および公開`NSString`場合、s が行われます、`NSString`メソッドが、コンテンツの比較ではなくポインターの比較を行うことができます。


これは、OBJECTIVE-C Api がパブリックで使用する場合に発生する可能性があります`NSString`を文字列の実際の内容の比較ではなく、いくつかの操作を表すトークンとして定数。


ような場合、 `NSString` Api が公開され、このを持つ Api の少数派があります。 NSString プロパティがいくつかのクラスで公開されることもわかります。 もの`NSString`通知などの項目のプロパティが公開されます。 それらはプロパティは、通常次のようになります。

```csharp
class Foo {
     public NSString FooNotification { get; }
}
```

通知は、キーに使用される、`NSNotification`クラスに、ランタイムによって配信している特定のイベントを登録するときにします。

キーでは、次のような通常なります。

```csharp
class Foo {
     public NSString FooBarKey { get; }
}
```

別の場所で`NSString`で公開されている s、API は、iOS での特定の Api または OS X を受け取るパラメーターとして使用されるトークンとして`NSDictionary`パラメーターとしてオブジェクト。 ディクショナリに通常含まれる`NSString`キー。 Xamarin.iOS、規則では、名前、静的`NSString`「キー」名を追加してプロパティです。
