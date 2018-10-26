---
title: Objective C のサポート
description: このドキュメントでは、.NET の埋め込みでは、OBJECTIVE-C のサポートの説明を提供します。 これは、自動参照カウント、NSString、プロトコル、NSObject プロトコル、例外、および詳細について説明します。
ms.prod: xamarin
ms.assetid: 3367A4A4-EC88-4B75-96D0-51B1FCBCE614
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: 48caa70cf2bd408f8afc673b400f7d5a4369e108
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110727"
---
# <a name="objective-c-support"></a>Objective C のサポート

## <a name="specific-features"></a>特定の機能

Objective C の世代では、注目すべきいくつかの特別な機能をいます。

### <a name="automatic-reference-counting"></a>自動参照カウント

自動参照カウント (弧) を使用して、**必要**を生成されたバインドを呼び出します。 .NET の埋め込みベースのライブラリを使用してプロジェクトをコンパイルする必要があります`-fobjc-arc`します。

### <a name="nsstring-support"></a>NSString サポート

Api を公開する`System.String`型に変換されます`NSString`します。 これにより、メモリ管理を処理する場合よりも簡単に`char*`します。

### <a name="protocols-support"></a>プロトコルのサポート

マネージ インターフェイスは、すべてのメンバーは OBJECTIVE-C プロトコルに変換されます`@required`します。

### <a name="nsobject-protocol-support"></a>NSObject プロトコルのサポート

既定では、既定のハッシュおよび .NET と OBJECTIVE-C ランタイムの両方の等値と見なされますのようなセマンティクスを共有に入れ替えることはできません。

マネージ型のよりも優先されます`Equals(Object)`または`GetHashCode`、(.NET) の既定の動作が十分なことを通常意味します。 既定 Objective C の動作は可能性があることを意味十分ではありませんか。

このような場合は、コード ジェネレーターの上書き、 [ `isEqual:` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418795-isequal?language=objc)メソッドと[ `hash` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418859-hash?language=objc)プロパティで定義されている、 [ `NSObject`プロトコル](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc)します。 これにより、OBJECTIVE-C コードから透過的に使用するカスタムのマネージ実装できます。

### <a name="exceptions-support"></a>例外サポート

渡す`--nativeexception`への引数として`objcgen`Objective C の例外をキャッチおよび処理できるマネージ例外に変換されます。 

### <a name="comparison"></a>条件式

マネージ型を実装する`IComparable`(またはそのジェネリック バージョン`IComparable<T>`) を返す、OBJECTIVE-C で使いやすいメソッドが生成されます、`NSComparisonResult`を受け入れると、`nil`引数。 これにより、生成済み API の OBJECTIVE-C 開発者向けにわかりやすい。 例えば:

```objc
- (NSComparisonResult)compare:(XAMComparableType * _Nullable)other;
```

### <a name="categories"></a>カテゴリ

マネージ拡張のメソッドは、カテゴリに変換されます。 次の拡張メソッドなど、 `Collection`:

```csharp
public static class SomeExtensions {
    public static int CountNonNull (this Collection collection) { ... }
    public static int CountNull (this Collection collection) { ... }
}
```

このような Objective C カテゴリを作成します。

```objc
@interface Collection (SomeExtensions)

- (int)countNonNull;
- (int)countNull;

@end
```

1 つのマネージ型は、いくつかの種類を拡張し、複数の Objective C カテゴリが生成されます。

### <a name="subscripting"></a>添字演算子 

管理対象のインデックス付きプロパティは、オブジェクトの添字演算子に変換されます。 例えば:

```csharp
public bool this[int index] {
    get { return c[index]; }
    set { c[index] = value; }
}
```

ような Objective C を作成します。

```objc
- (id)objectAtIndexedSubscript:(int)idx;
- (void)setObject:(id)obj atIndexedSubscript:(int)idx;
```

Objective C の添字の構文を使用してこれを使用できます。

```objc
if ([intCollection [0] isEqual:@42])
    intCollection[0] = @13;
```

で、インデクサーの種類に応じて適切な場所にインデックス付きまたはキー付きの添字演算子が生成されます。

これは、[記事](http://nshipster.com/object-subscripting/)は添字演算子導入として優れています。

## <a name="main-differences-with-net"></a>.NET での主な相違点

### <a name="constructors-vs-initializers"></a>Vs 初期化子のコンス トラクター

Objective-c で呼び出せる初期化子のいずれか、継承チェーンの親クラスのいずれかのプロトタイプは利用不可としてマークされていない場合 (`NS_UNAVAILABLE`)。

C#コンス トラクターは継承されないため、クラス内のコンス トラクター メンバーを明示的に宣言する必要があります。

適切な表現を公開する、 C# API、OBJECTIVE-C を`NS_UNAVAILABLE`が親クラスから、子クラスに存在しないすべての初期化子に追加されます。

C#API:

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

Objective C では、API が浮かび上がってきます。

```objc
@interface SuperUnique : Unique

- (instancetype)initWithId:(int)id NS_UNAVAILABLE;
- (instancetype)init;

@end
```

ここでは、`initWithId:`利用不可としてマークされています。

### <a name="operator"></a>演算子

OBJECTIVE-C では演算子としてのオーバー ロードをサポートしていませんC#ため、演算子はクラス セレクターに変換されます。

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

ただし、一部の .NET 言語はサポートしない演算子のオーバー ロード、ためを含めることも一般的にはに、 [「わかりやすい」](https://docs.microsoft.com/dotnet/standard/design-guidelines/operator-overloads)という名前のメソッドに加えて、演算子のオーバー ロードします。

かどうか、演算子のバージョンと「わかりやすい」のバージョンの両方が見つかると、親しみやすいバージョンのみが生成されます同じ Objective C の名前を生成するようにします。

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

ようになります。

```objc
+ (instancetype)add:(Overloads_AllOperatorsWithFriendly *)anObjectC1 c2:(Overloads_AllOperatorsWithFriendly *)anObjectC2;
```

### <a name="equality-operator"></a>等値演算子

一般的な演算子で`==`でC#、一般的な操作の説明するように上位として処理されます。

ただし、「わかりやすい」の等号演算子が見つからないかどうかは、両方の演算子`==`と演算子`!=`生成はスキップされます。

### <a name="datetime-vs-nsdate"></a>DateTime vs NSDate

[ `NSDate` ](https://developer.apple.com/reference/foundation/nsdate?language=objc)ドキュメント。

> `NSDate` オブジェクトは、任意の特定の calendrical システムまたはタイム ゾーンの独立した時間で 1 つのポイントをカプセル化します。 Date オブジェクトは不変では、絶対参照の日付を基準とした、インバリアントの時間間隔を表す (00:1 月 1 日の 2001 年に 00:00 UTC)。

により`NSDate`日付、すべての変換に間を参照し、 `DateTime` UTC で行う必要があります。

#### <a name="datetime-to-nsdate"></a>NSDate する DateTime

変換するときに`DateTime`に`NSDate`、`Kind`プロパティ`DateTime`に考慮されます。

|種類|結果|
|---|---|
|`Utc`|指定されたを使用して変換を実行`DateTime`オブジェクトです。|
|`Local`|呼び出しの結果`ToUniversalTime()`提供`DateTime`変換オブジェクトに使用します。|
|`Unspecified`|指定された`DateTime`オブジェクトは UTC で同一の動作と見なされますと`Kind`は`Utc`します。|

変換では、次の数式を使用します。

```
TimeInterval = DateTimeObjectTicks - NSDateReferenceDateTicks / TicksPerSecond
```

この数式では。 

- `NSDateReferenceDateTicks` に基づいて計算されますが、 `NSDate` 1 月 1 日の 2001 年に 00時 00分: 00 UTC の日付を参照します。 
    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```
- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond) 定義されます。 [`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)

作成する、`NSDate`オブジェクト、`TimeInterval`を併用、 `NSDate` [dateWithTimeIntervalSinceReferenceDate:](https://developer.apple.com/reference/foundation/nsdate/1591577-datewithtimeintervalsincereferen?language=objc)セレクター。

#### <a name="nsdate-to-datetime"></a>Datetime NSDate

変換`NSDate`に`DateTime`は次の式を使用します。

```
DateTimeTicks = NSDateTimeIntervalSinceReferenceDate * TicksPerSecond + NSDateReferenceDateTicks
```

この数式では。 

- `NSDateReferenceDateTicks` に基づいて計算されますが、 `NSDate` 1 月 1 日の 2001 年に 00時 00分: 00 UTC の日付を参照します。 
    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```
- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond) 定義されます。 [`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)

計算後`DateTimeTicks`、 `DateTime` [コンス トラクター](https://docs.microsoft.com/dotnet/api/system.datetime.-ctor?#System_DateTime__ctor_System_Int64_System_DateTimeKind_)呼び出されると、設定、`kind`に`DateTimeKind.Utc`します。

> [!NOTE]
> `NSDate` `nil`が、`DateTime`構造体で定義することはできませんが、.NET では、`null`します。 提供する場合、 `nil` `NSDate`は既定値に変換されます`DateTime`値にマップする`DateTime.MinValue`します。

`NSDate` 高い最大と最小値より小さくサポート`DateTime`します。 変換するときに`NSDate`に`DateTime`、これらの高い値と低い値に変更は、 `DateTime` [MaxValue](https://docs.microsoft.com/dotnet/api/system.datetime.maxvalue)または[MinValue](https://docs.microsoft.com/dotnet/api/system.datetime.minvalue)、それぞれします。
