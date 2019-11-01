---
title: 目的-C のサポート
description: このドキュメントでは、.NET 埋め込みでの目的 C のサポートについて説明します。 自動参照カウント、NSString、プロトコル、NSObject プロトコル、例外などについて説明します。
ms.prod: xamarin
ms.assetid: 3367A4A4-EC88-4B75-96D0-51B1FCBCE614
author: davidortinau
ms.author: daortin
ms.date: 11/14/2017
ms.openlocfilehash: e72f950dc6fcf12e70714e0fbb996ad5ea432548
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029706"
---
# <a name="objective-c-support"></a>目的-C のサポート

## <a name="specific-features"></a>特定の機能

目標 C の世代には、注意すべきいくつかの特殊な機能があります。

### <a name="automatic-reference-counting"></a>自動参照カウント

生成されたバインディングを呼び出すには、自動参照カウント (弧) を使用する**必要**があります。 .NET 埋め込みベースのライブラリを使用するプロジェクトは、`-fobjc-arc`を使用してコンパイルする必要があります。

### <a name="nsstring-support"></a>NSString のサポート

`System.String` 型を公開する Api は、`NSString`に変換されます。 これにより、`char*`を処理する場合よりもメモリ管理が簡単になります。

### <a name="protocols-support"></a>プロトコルのサポート

マネージインターフェイスは、すべてのメンバーが `@required`する目的の C プロトコルに変換されます。

### <a name="nsobject-protocol-support"></a>NSObject プロトコルのサポート

既定では、.NET と目的 C ランタイムの両方の既定のハッシュおよび等価性は、類似したセマンティクスを共有しているため、交換可能であると見なされます。

マネージ型が `Equals(Object)` または `GetHashCode`をオーバーライドする場合は、通常、既定の (.NET) 動作が不十分であったことを意味します。これは、既定の目的 C の動作では不十分である可能性があることを意味します。

このような場合、ジェネレーターは[`NSObject` プロトコル](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc)で定義されている[`isEqual:`](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418795-isequal?language=objc)メソッドと[`hash`](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418859-hash?language=objc)プロパティをオーバーライドします。 これにより、カスタムマネージ実装を目的の C コードから透過的に使用できるようになります。

### <a name="exceptions-support"></a>例外のサポート

`--nativeexception` を引数として `objcgen` に渡すと、マネージ例外が、キャッチして処理できるように、目的の C 例外に変換されます。 

### <a name="comparison"></a>条件式

`IComparable` (またはそのジェネリックバージョン `IComparable<T>`) を実装するマネージ型では、`NSComparisonResult` を返し、`nil` 引数を受け取る、目的の C のわかりやすいメソッドが生成されます。 これにより、生成された API が目的の C 開発者にとってよりわかりやすくなります。 (例:

```objc
- (NSComparisonResult)compare:(XAMComparableType * _Nullable)other;
```

### <a name="categories"></a>カテゴリ

マネージ拡張メソッドは、カテゴリに変換されます。 たとえば、`Collection`の次の拡張メソッドがあります。

```csharp
public static class SomeExtensions {
    public static int CountNonNull (this Collection collection) { ... }
    public static int CountNull (this Collection collection) { ... }
}
```

は、次のような目的の C カテゴリを作成します。

```objc
@interface Collection (SomeExtensions)

- (int)countNonNull;
- (int)countNull;

@end
```

1つのマネージ型が複数の型を拡張する場合、複数の目的 C のカテゴリが生成されます。

### <a name="subscripting"></a>添字演算子

マネージインデックス付きプロパティは、オブジェクト時に変換されます。 (例:

```csharp
public bool this[int index] {
    get { return c[index]; }
    set { c[index] = value; }
}
```

次のような目的で C を作成します。

```objc
- (id)objectAtIndexedSubscript:(int)idx;
- (void)setObject:(id)obj atIndexedSubscript:(int)idx;
```

これは、時構文で使用できます。

```objc
if ([intCollection [0] isEqual:@42])
    intCollection[0] = @13;
```

インデクサーの種類に応じて、インデックス付きまたはキー付きの時が適切な場所に生成されます。

この[記事](https://nshipster.com/object-subscripting/)では、時の概要について説明します。

## <a name="main-differences-with-net"></a>.NET との主な違い

### <a name="constructors-vs-initializers"></a>コンストラクターと初期化子

目的 C では、継承チェーン内の任意の親クラスの任意の初期化子プロトタイプを呼び出すことができます。ただし、使用不可 (`NS_UNAVAILABLE`) としてマークされている場合は除きます。

でC#は、クラス内のコンストラクターメンバーを明示的に宣言する必要があります。これは、コンストラクターが継承されないことを意味します。

C# API の正しい表現を目的の C に公開するために、親クラスの子クラスに存在しない初期化子には`NS_UNAVAILABLE`が追加されます。

C#API

```csharp
public class Unique {
    public Unique () : this (1)
    {
    }

    public Unique (int id)
    {
    }
}

public class SuperUnique : Unique {
    public SuperUnique () : base (911)
    {
    }
}
```

目的-C が提示した API:

```objc
@interface SuperUnique : Unique

- (instancetype)initWithId:(int)id NS_UNAVAILABLE;
- (instancetype)init;

@end
```

ここでは、`initWithId:` が使用不可としてマークされています。

### <a name="operator"></a>演算子

目標 C は演算子のオーバーロードをそのC#ようにサポートしていないため、演算子はクラスセレクターに変換されます。

```csharp
public static AllOperators operator + (AllOperators c1, AllOperators c2)
{
    return new AllOperators (c1.Value + c2.Value);
}
```

から

```objc
+ (instancetype)add:(Overloads_AllOperators *)anObjectC1 c2:(Overloads_AllOperators *)anObjectC2;
```

ただし、一部の .NET 言語では演算子のオーバーロードがサポートされていないため、演算子のオーバーロードに加えて["わかりやすい"](https://docs.microsoft.com/dotnet/standard/design-guidelines/operator-overloads)名前付きメソッドを含めることもよくあります。

演算子のバージョンと "フレンドリ" の両方のバージョンが見つかった場合は、同じ目的の C 名に生成されるため、フレンドリバージョンのみが生成されます。

```csharp
public static AllOperatorsWithFriendly operator + (AllOperatorsWithFriendly c1, AllOperatorsWithFriendly c2)
{
    return new AllOperatorsWithFriendly (c1.Value + c2.Value);
}

public static AllOperatorsWithFriendly Add (AllOperatorsWithFriendly c1, AllOperatorsWithFriendly c2)
{
    return new AllOperatorsWithFriendly (c1.Value + c2.Value);
}
```

となり

```objc
+ (instancetype)add:(Overloads_AllOperatorsWithFriendly *)anObjectC1 c2:(Overloads_AllOperatorsWithFriendly *)anObjectC2;
```

### <a name="equality-operator"></a>等値演算子

一般に、の `==` C#は、前述のように一般的な演算子として処理されます。

ただし、"表示" 等号演算子が見つかった場合は、演算子 `==` と演算子 `!=` の両方が生成時にスキップされます。

### <a name="datetime-vs-nsdate"></a>DateTime vs NSDate

[`NSDate`](https://developer.apple.com/reference/foundation/nsdate?language=objc)のドキュメントを参照してください。

> `NSDate` オブジェクトは、特定の calendrical システムまたはタイムゾーンに依存せず、1つの時点をカプセル化します。 日付オブジェクトは不変であり、絶対参照日付を基準としたインバリアントな時間間隔を表します (00:00:00 UTC の1月 2001)。

`NSDate` 参照日付のため、`DateTime` 間の変換はすべて UTC で行う必要があります。

#### <a name="datetime-to-nsdate"></a>DateTime から NSDate

`DateTime` から `NSDate`に変換する場合、`DateTime` の `Kind` プロパティが次のように考慮されます。

|種類|結果|
|---|---|
|`Utc`|変換は、指定された `DateTime` オブジェクトをそのように使用して実行されます。|
|`Local`|指定された `DateTime` オブジェクトで `ToUniversalTime()` を呼び出した結果が変換に使用されます。|
|`Unspecified`|指定された `DateTime` オブジェクトは UTC であると見なされるため、`Kind` が `Utc`場合にも同じ動作になります。|

変換では、次の式を使用します。

```
TimeInterval = DateTimeObjectTicks - NSDateReferenceDateTicks / TicksPerSecond
```

この数式の場合: 

- `NSDateReferenceDateTicks` は、2001年1月1日の 00:00:00 UTC の `NSDate` 参照日付に基づいて計算されます。 

    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```

- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond)は[`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)で定義されています

`NSDate` オブジェクトを作成するには、`TimeInterval` を `NSDate` [dateWithTimeIntervalSinceReferenceDate:](https://developer.apple.com/reference/foundation/nsdate/1591577-datewithtimeintervalsincereferen?language=objc)セレクターと共に使用します。

#### <a name="nsdate-to-datetime"></a>NSDate to DateTime

`NSDate` から `DateTime` への変換では、次の式を使用します。

```
DateTimeTicks = NSDateTimeIntervalSinceReferenceDate * TicksPerSecond + NSDateReferenceDateTicks
```

この数式の場合: 

- `NSDateReferenceDateTicks` は、2001年1月1日の 00:00:00 UTC の `NSDate` 参照日付に基づいて計算されます。 

    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```

- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond)は[`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)で定義されています

`DateTimeTicks`を計算した後、`DateTime`[コンストラクター](https://docs.microsoft.com/dotnet/api/system.datetime.-ctor?#System_DateTime__ctor_System_Int64_System_DateTimeKind_)が呼び出され、その `kind` が `DateTimeKind.Utc`に設定されます。

> [!NOTE]
> `NSDate` は `nil`できますが、`DateTime` は .NET の構造体であり、定義によって `null`することはできません。 `nil` `NSDate`を指定すると、`DateTime.MinValue`にマップされる既定の `DateTime` 値に変換されます。

`NSDate` は、`DateTime`よりも高い最大値と下限値をサポートします。 `NSDate` から `DateTime`に変換するときに、これらの値の上限と下限をそれぞれ `DateTime` [MaxValue](https://docs.microsoft.com/dotnet/api/system.datetime.maxvalue)または[MinValue](https://docs.microsoft.com/dotnet/api/system.datetime.minvalue)に変更します。
