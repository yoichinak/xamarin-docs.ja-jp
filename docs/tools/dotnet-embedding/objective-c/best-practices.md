---
title: ObjC の Embeddinator 4000 のベスト プラクティス
ms.prod: xamarin
ms.assetid: 63C7F5D2-8933-4D4A-8348-E9CBDA45C472
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 93dd98dcff772adceb3650ec327cc1a14e4e056b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="embeddinator-4000-best-practices-for-objc"></a>ObjC の Embeddinator 4000 のベスト プラクティス

これは、下書きと可能性がある機能との同期で現在サポートされていません、ツールによってです。 願っているこのドキュメントを個別に発展最終的に、最終的なツールと一致、つまり長期的な最適な方法でないイミディ エイトの回避策をお勧めします。

このドキュメントの大部分は、サポートされている他の言語にも適用されます。 ただしすべて提供例としては、c# と目標 C には


# <a name="exposing-a-subset-of-the-managed-code"></a>マネージ コードのサブセットを公開します。

生成されたネイティブ ライブラリ/フレームワークには、各公開されているマネージ Api を呼び出す Objective C コードが含まれています。 複数の API サーフェスを (を公開する) より大きなネイティブ_グルー_ライブラリになります。

ネイティブの開発者に必要な Api のみを公開する、異なる、小規模なアセンブリを作成することをお勧めがあります。 そのファサードはまた、可視性、名前を付けるのエラー チェックしています...、生成されたコードより詳細に制御できます。


# <a name="exposing-a-chunkier-api"></a>Chunkier API を公開します。

ネイティブ コードから移行する費用価格があるにマネージ (その逆)。 公開することをお勧め、 _chatty ではなく chunky な_ネイティブの開発者に Api 例。

**Chatty です**
```
public class Person {
    public string FirstName { get; set; }
    public string LastName { get; set; }
}
```

```csharp
// this requires 3 calls / transitions to initialize the instance
Person *p = [[Person alloc] init];
p.firstName = @"Sebastien";
p.lastName = @"Pouliot";
```

**Chunky**
```
public class Person {
    public Person (string firstName, string lastName) {}
}
```

```csharp
// a single call / transition will perform better
Person *p = [[Person alloc] initWithFirstName:@"Sebastien" lastName:@"Pouliot"];
```

遷移の数は小さいため、パフォーマンスが向上されます。 これより小さなネイティブ ライブラリにも、生成されるコードの少ないも必要です。


# <a name="naming"></a>名前付け

名前付けは、他のユーザーの無効化および 1 ではオフのエラーをキャッシュにされているコンピューター サイエンスの世界での 2 つの最も困難な問題のいずれかです。 願っています .NET を埋め込むことができます、シールドする以外のすべての名前付けから。

## <a name="types"></a>種類

Objective C は、名前空間をサポートしていません。 一般に、その型が付いた 2 (Apple) 用 (サード パーティ) の 3 文字のようなプレフィックス、または`UIView`UIKit のビューのことを示しているフレームワークです。

.NET 型の名前空間をスキップしていますはできませんと重複している、または複雑になる場合、名前を導入できます。 これは、ため、既存の .NET 型が非常に長い例。

```csharp
namespace Xamarin.Xml.Configuration {
    public class Reader {}
}
```

同様に使用されます。

```csharp
id reader = [[Xamarin_Xml_Configuration_Reader alloc] init];
```

ただし、型と型を再公開できます。

```csharp
public class XAMXmlConfigReader : Xamarin.Xml.Configuration.Reader {}
```

Objective C の複数の表示を行うなど、使用します。

```csharp
id reader = [[XAMXmlConfigReader alloc] init];
```

## <a name="methods"></a>メソッド

でもの .NET 名は、OBJECTIVE-C API に最適ですではない可能性があります。

Objective c の名前付け規則は、.NET (pascal 形式の場合より詳細ではなくキャメル ケース) と異なります。
お読みください、 [Cocoa のコーディング ガイドライン](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingMethods.html#//apple_ref/doc/uid/20001282-BCIGIJJF)です。

Objective C 開発者の観点から見る、メソッドを`Get`プレフィックスは、つまり、インスタンスを所有していないことを意味、[ルールの取得](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-SW1)です。

この名前付け規則は、一致する .NET GC 世界で.NET メソッドを`Create`プレフィックスは、.NET の動作は同じです。 ただし、Objective C 開発者は、通常意味自分が所有する、返されるインスタンス、つまり、[ルールを作成](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-103029)です。

# <a name="exceptions"></a>例外

広範なエラーを報告する例外を使用する .NET できわめて commont することをお勧めします。 ただし、これらは低速で ObjC でかなりと同じです。 可能な限り Objective C の開発者から非表示にする必要があります。

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

## <a name="exceptions-inside-init"></a>内部での例外 `init*`

.NET でコンス トラクターは、成功し、返す、、(_できれば_) 有効なインスタンス、または例外をスローします。

Objective C は、これに対し、`init*`を返す`nil`インスタンスを作成できません。 これは、Apple のフレームワークの多くで使用される一般的でもない一般的なパターンです。 その他のいくつかの場合、`assert`発生することができます (および現在のプロセスを強制終了)。

ジェネレーターに従って同じ`return nil`生成されたパターン`init*`メソッドです。 マネージ例外がスローされたかどうかは、印刷されます (を使用して`NSLog`) および`nil`が呼び出し元に返されます。

## <a name="operators"></a>演算子

ObjC では、オーバー ロードされるように C# の場合は、ため、クラス セレクターに変換は、これらの演算子は許可されません。

[「わかりやすい」](https://msdn.microsoft.com/en-us/library/ms229032(v=vs.110).aspx)演算子のオーバー ロード方が優先的に名前付きメソッドが生成されるときに検出されより簡単に API を利用を生成できます。

演算子をオーバーライドするクラス = = または! = も標準 Equals (Object) メソッドをオーバーライドする必要があります。
