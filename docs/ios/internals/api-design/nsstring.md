---
title: Xamarin.iOS および Xamarin.Mac NSString
description: このドキュメントでは、Xamarin.iOS が透過的を NSString オブジェクトを変換する方法について説明しますC#そうでないときに、オブジェクトの文字列します。
ms.prod: xamarin
ms.assetid: 785744B3-42E2-4590-8F41-435325E609B9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: cc9e3f992642f3cdc3d16fe6f829b6a6c06b50fc
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61277815"
---
# <a name="nsstring-in-xamarinios-and-xamarinmac"></a>Xamarin.iOS および Xamarin.Mac NSString

API を使用して、ネイティブの .NET の文字列型の公開するために Xamarin.iOS および Xamarin.Mac の両方の設計`string`での文字列操作のC#とプログラミング言語、他の .NET との代わりに、API によって公開されるデータ型として文字列を公開するには `NSString` データ型。

つまり、こと、開発者は、Xamarin.iOS および Xamarin.Mac API (統合) を呼び出すことに使用することを意図した文字列を保持する必要はありません、特殊な種類の (`Foundation.NSString`) を使用して Mono を保持できます`System.String`のすべての操作、や、常にXamarin.iOS、Xamarin.Mac の API には、文字列が必要です。 は、マーシャ リング、情報の当社の API バインドを行います。

Objective C"text"プロパティなど、`UILabel`型の`NSString`、ように宣言します。

```objc
@property(nonatomic, copy) NSString *text
```

これは、として Xamarin.iOS で公開されます。

```csharp
class UILabel {
    public string Text { get; set; }
}
```

背後では、このプロパティの実装をマーシャ リング、C#文字列に、`NSString`を呼び出すと、 `objc_msgSend` Objective C と同じ方法でメソッド。

いくつかのサード パーティ製の Objective C Api を使用しない、`NSString`が代わりに、C の文字列を使用 (、"*char*")。 ような場合、引き続き使用できます、C#文字列データ型では、使用する必要がありますが、 [[PlainString]](~/cross-platform/macios/binding/objective-c-libraries.md)属性としてこの文字列をマーシャ リングしないバインド ジェネレーターを通知するために、 `NSString`、C 文字列として代わりにします。

 <a name="Exceptions_to_the_Rule" />

## <a name="exceptions-to-the-rule"></a>ルールの例外

Xamarin.iOS および Xamarin.Mac では、この規則の例外を行いました。 公開との間での意思決定 `string`s を作成する場合と、except、および公開 `NSString`場合、s が行われます、 `NSString` メソッドが、コンテンツの比較ではなくポインターの比較を行うことができます。

これは、Objective C Api がパブリックで使用する場合に発生する可能性があります `NSString` 定数として文字列の実際の内容を比較する代わりに、何らかのアクションを表すトークン。

その場合、 `NSString`  Api が公開され、少数の Api をこれがあります。 NSString プロパティがいくつかのクラスで公開されることもわかります。 これは、`NSString`通知などの項目のプロパティが公開されます。 これらはプロパティが通常ようになります。

```csharp
class Foo {
     public NSString FooNotification { get; }
}
```
通知に使用されるキーでは、`NSNotification`クラスのランタイムで配信している特定のイベントに登録する場合。

キーを使用して、このようなものは、通常なります。

```csharp
class Foo {
     public NSString FooBarKey { get; }
}
```

別の場所、`NSString`で s が公開されている API はトークンを受け取る iOS で特定の Api または OS X をパラメーターとして使用されると、`NSDictionary`パラメーターとしてのオブジェクト。 ディクショナリに通常含まれる`NSString`キー。 Xamarin.iOS では、慣例により、名前、静的`NSString`「キー」の名前を追加することでプロパティ。
