---
title: Objective C の .NET の埋め込みのベスト プラクティス
description: このドキュメントは、OBJECTIVE-C と .NET の埋め込みを使用するためのさまざまなベスト プラクティスを説明します これは、マネージ コードのサブセットを公開する、chunkier API を公開する、名前付け、および詳細について説明します。
ms.prod: xamarin
ms.assetid: 63C7F5D2-8933-4D4A-8348-E9CBDA45C472
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: 33138b7858b8bc04a5be30f9fad1709e916f5575
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105403"
---
# <a name="net-embedding-best-practices-for-objective-c"></a>Objective C の .NET の埋め込みのベスト プラクティス

これは下書きであり、可能性がありますとの同期機能を現時点でサポートされていないツール。 お役に長期的な最適な方法でない即時の回避策を提案いたしますつまりこと、このドキュメントはそれぞれ別々 に進化し、最終的に、最終的なツールと一致します。

このドキュメントの大部分は、サポートされているその他の言語にも適用されます。 例としてで提供されているすべてただしC#と OBJECTIVE-C

## <a name="exposing-a-subset-of-the-managed-code"></a>マネージ コードのサブセットを公開します。

生成されたネイティブ ライブラリ/フレームワークには、Objective C コード各公開されているマネージ Api を呼び出すことにはが含まれています。 サーフェスを複数の API (パブリックこと) が拡大し、ネイティブ_グルー_ライブラリになります。

ネイティブの開発者に必要な Api のみを公開する、異なるより小さなアセンブリを作成することをお勧めが考えられます。 そのファサードはまた、可視性、名前付け、エラーが生成されたコードの確認... より詳細に制御できます。

## <a name="exposing-a-chunkier-api"></a>Chunkier API を公開します。

ネイティブ コードからの移行を支払う料金があるにマネージ コード (とバック)。 そのため、公開するほうが_chatty ではなく chunky_ネイティブの開発者に Api 例。

**Chatty です**

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

**チャンク化**

```csharp
public class Person {
  public Person (string firstName, string lastName) {}
}
```

```objc
// a single call / transition will perform better
Person *p = [[Person alloc] initWithFirstName:@"Sebastien" lastName:@"Pouliot"];
```

遷移の数が小さいため、パフォーマンスが向上されます。 小規模なネイティブ ライブラリも生成されますので、生成されるコードも必要です。

## <a name="naming"></a>名前付け

名前付けすると、コンピューター サイエンス、他のキャッシュの無効化と 1 ではオフのエラーをされている 2 つの最も困難な問題の 1 つです。 うまくいけば .NET を埋め込むことができますからも保護以外のすべての名前付けします。

### <a name="types"></a>種類

Objective C では、名前空間はサポートされていません。 一般に、その型が付いて、2 (Apple) の (サード パーティ) の 3 文字のようなプレフィックス、または`UIView`UIKit の表示を示すフレームワーク。

.NET 型の複製、または、混乱を招く名をもたらす可能性があると、名前空間をスキップはことではできません。 これは、ため、既存の .NET 型が非常に長い例。

```csharp
namespace Xamarin.Xml.Configuration {
  public class Reader {}
}
```

ように使用されます。

```objc
id reader = [[Xamarin_Xml_Configuration_Reader alloc] init];
```

ただし、種類としてを再公開できます。

```csharp
public class XAMXmlConfigReader : Xamarin.Xml.Configuration.Reader {}
```

Objective C のよりわかりやすいように例を使用します。

```objc
id reader = [[XAMXmlConfigReader alloc] init];
```

### <a name="methods"></a>メソッド

.NET にも適した名前を Objective C API の理想的なことができない可能性があります。

Objective C での名前付け規則は、.NET (より詳細なパスカル ケースではなくキャメル ケース) と異なります。
参照してください、 [Cocoa のコーディング ガイドライン](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingMethods.html#//apple_ref/doc/uid/20001282-BCIGIJJF)します。

OBJECTIVE-C 開発者の観点、使用してメソッドからを`Get`プレフィックスは、つまり、インスタンスを所有していないことを意味、[規則の取得](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-SW1)します。

この名前付け規則、.NET GC における; 一致はありません.NET メソッドを`Create`.NET にプレフィックスが同じ動作です。 ただし、開発者は Objective C、という意味など、返されたインスタンスを所有する、[ルールを作成](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-103029)です。

## <a name="exceptions"></a>例外

.Net ではエラーを報告する広範な例外を使用する非常に一般的です。 ただし、これらは低速と OBJECTIVE-C でまったく同じです。 可能であれば、OBJECTIVE-C 開発者から非表示にする必要があります。

たとえば、.NET`Try`パターンは Objective C コードから使用するはるかに簡単になります。

```csharp
public int Parse (string number)
{
  return Int32.Parse (number);
}
```

比較

```csharp
public bool TryParse (string number, out int value)
{
  return Int32.TryParse (number, out value);
}
```

### <a name="exceptions-inside-init"></a>内部例外 `init*`

.NET でのコンス トラクターをする必要がありますか、成功し、返します、、(_できれば_) 有効なインスタンスまたは例外をスローします。

OBJECTIVE-C では、これに対し、`init*`を返す`nil`インスタンスを作成できません。 これは、Apple のフレームワークの多くで使用される一般的なしかしされません一般的なパターンです。 その他のいくつかの場合、`assert`発生することができます (および現在のプロセスを強制終了)。

コード ジェネレーターがに従って同じ`return nil`生成パターン`init*`メソッド。 マネージ例外がスローされたかどうかは、印刷される (を使用して`NSLog`) と`nil`が呼び出し元に返されます。

## <a name="operators"></a>演算子

Objective C としては、オーバー ロードする演算子を許可しないC#ため、これらは、クラス セレクターに変換します。

[「わかりやすい」](https://docs.microsoft.com/dotnet/standard/design-guidelines/operator-overloads)演算子のオーバー ロード方が優先的に名前付きメソッドが生成されるときに検出されより簡単に API を利用して生成できます。

演算子をオーバーライドするクラス`==`施した`!=`も標準 Equals (Object) メソッドをオーバーライドする必要があります。
