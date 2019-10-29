---
title: .NET 埋め込みのベストプラクティス-C
description: このドキュメントでは、.NET 埋め込みを使用して、C を使用するためのさまざまなベストプラクティスについて説明します。 ここでは、マネージコードのサブセットの公開、chunkier API の公開、名前付けなどについて説明します。
ms.prod: xamarin
ms.assetid: 63C7F5D2-8933-4D4A-8348-E9CBDA45C472
author: davidortinau
ms.author: daortin
ms.date: 11/14/2017
ms.openlocfilehash: 2f632e3218d817aa0162a63ea81c61ca18c52b93
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73006773"
---
# <a name="net-embedding-best-practices-for-objective-c"></a>.NET 埋め込みのベストプラクティス-C

これはドラフトであり、ツールで現在サポートされている機能と同期されていない可能性があります。 このドキュメントが個別に進化し、最終的に最終的なツールと一致していることを願っています。つまり、短期的なベストアプローチを提案します。即時の回避策ではありません。

このドキュメントの大部分は、サポートされている他の言語にも適用されます。 ただし、指定されたC#すべての例は、との C に含まれています。

## <a name="exposing-a-subset-of-the-managed-code"></a>マネージコードのサブセットの公開

生成されたネイティブライブラリ/フレームワークには、公開されている各マネージ Api を呼び出すための目的の C コードが含まれています。 より多くの API (パブリック) を使用すると、ネイティブ_グルー_ライブラリが大きくなります。

必要な Api のみをネイティブ開発者に公開するために、別の小さなアセンブリを作成することをお勧めします。 このファサードを使用すると、可視性、名前付け、エラーチェックをより細かく制御することもできます。生成されたコードの。

## <a name="exposing-a-chunkier-api"></a>Chunkier API の公開

ネイティブから管理 (およびその逆) への移行には料金が発生します。 そのため、_高い api の代わりに chunky_をネイティブ開発者に公開することをお勧めします。たとえば、

**高い**

```csharp
public class Person {
  public string FirstName { get; set; }
  public string LastName { get; set; }
}
```

```objc
// this requires 3 calls / transitions to initialize the instance
Person *p = [[Person alloc] init];
p.firstName = @"Sebastien";
p.lastName = @"Pouliot";
```

**Chunky**

```csharp
public class Person {
  public Person (string firstName, string lastName) {}
}
```

```objc
// a single call / transition will perform better
Person *p = [[Person alloc] initWithFirstName:@"Sebastien" lastName:@"Pouliot"];
```

移行の回数が少ないため、パフォーマンスが向上します。 また、生成されるコードが少なくなるため、ネイティブライブラリも小さくなります。

## <a name="naming"></a>名前付け

名前付けは、コンピューターサイエンスで最も困難な2つの問題の1つであり、キャッシュの無効化と1回目のエラーの発生を防ぎます。 .NET 埋め込みは、名前を付けるだけではなく、すべてのことを防ぐことができます。

### <a name="types"></a>種類

目標 C は名前空間をサポートしていません。 一般に、この型には、フレームワークを示す UIKit のビューの `UIView` のように、2 (Apple の場合) または 3 (サードパーティの場合) という文字プレフィックスが付いています。

名前空間の重複や混乱を招く可能性があるため、.NET 型では名前空間を省略できません。 これにより、既存の .NET 型が非常に長くなります。たとえば、

```csharp
namespace Xamarin.Xml.Configuration {
  public class Reader {}
}
```

次のように使用されます。

```objc
id reader = [[Xamarin_Xml_Configuration_Reader alloc] init];
```

ただし、型は次のように再公開できます。

```csharp
public class XAMXmlConfigReader : Xamarin.Xml.Configuration.Reader {}
```

次のように、使用する目的をさらに C でわかりやすくします。

```objc
id reader = [[XAMXmlConfigReader alloc] init];
```

### <a name="methods"></a>メソッド

優れた .NET 名でも、目的の C API には適していない場合があります。

目的 C の名前付け規則は .NET とは異なります (pascal 形式ではなく camel 形式の場合は、より詳細です)。
[Cocoa のコーディングガイドライン](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingMethods.html#//apple_ref/doc/uid/20001282-BCIGIJJF)をお読みください。

C 開発者の観点から見ると、`Get` プレフィックスを持つメソッドは、インスタンス ( [get ルール](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-SW1)) を所有していないことを意味します。

この名前付け規則は、.NET GC の世界では一致しません。`Create` プレフィックスを持つ .NET メソッドは、.NET でも同じように動作します。 ただし、C の開発者にとっては、通常、返されたインスタンス (つまり、[作成規則](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-103029)) を所有していることを意味します。

## <a name="exceptions"></a>例外

.NET では、例外を頻繁に使用してエラーを報告することが非常に一般的です。 ただし、これらは低速であり、目標 C ではまったく同じではありません。 可能な限り、C 開発者からは非表示にする必要があります。

たとえば、.NET `Try` パターンは、目標 C コードからはるかに簡単に使用できます。

```csharp
public int Parse (string number)
{
  return Int32.Parse (number);
}
```

比べ

```csharp
public bool TryParse (string number, out int value)
{
  return Int32.TryParse (number, out value);
}
```

### <a name="exceptions-inside-init"></a>`init*` 内の例外

.NET では、コンストラクターは成功し、(_できれ_ば) 有効なインスタンスを返すか、または例外をスローする必要があります。

これに対し、目標-C を使用すると、インスタンスを作成できない場合に `nil` を返すことが `init*` できます。 これは一般的ではありませんが、多くの Apple のフレームワークで使用される一般的なパターンです。 他のケースでは、`assert` が発生する場合があります (および現在のプロセスを終了します)。

ジェネレーターは、生成された `init*` メソッドに対して同じ `return nil` パターンに従います。 マネージ例外がスローされた場合は、(`NSLog`を使用して) 出力され、`nil` 呼び出し元に返されます。

## <a name="operators"></a>演算子

目標 C では、演算子をそのようC#にオーバーロードすることはできないため、これらはクラスセレクターに変換されます。

["わかりやすい"](https://docs.microsoft.com/dotnet/standard/design-guidelines/operator-overloads)名前付きメソッドは、見つかった場合は演算子のオーバーロードに優先して生成され、API を簡単に使用できるようになります。

施し `!=` `==` 演算子をオーバーライドするクラスは、標準の Equals (Object) メソッドもオーバーライドする必要があります。
