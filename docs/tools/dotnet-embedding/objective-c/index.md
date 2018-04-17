---
title: Objective C のサポート
ms.prod: xamarin
ms.assetid: 3367A4A4-EC88-4B75-96D0-51B1FCBCE614
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 66ba31b1054559516fdbfbeb0421a82f4a0e9fa5
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2018
---
# <a name="objective-c-support"></a>Objective C のサポート

## <a name="specific-features"></a>特定の機能

Objective C の生成には、注目すべきである、いくつかの特別な機能があります。

### <a name="automatic-reference-counting"></a>自動参照カウント

自動参照カウント (円弧) を使用するは**必要**を生成されたバインディングを呼び出します。 Embeddinator ベースのライブラリを使用してプロジェクトをコンパイルする必要があります`-fobjc-arc`です。

### <a name="nsstring-support"></a>NSString サポート

Api を公開する`System.String`型に変換されます`NSString`です。 これにより、メモリ管理は、処理するときにより簡単に`char*`です。

### <a name="protocols-support"></a>プロトコルのサポート

マネージ インターフェイスはすべてのメンバーが、OBJECTIVE-C プロトコルに変換されます`@required`です。

### <a name="nsobject-protocol-support"></a>NSObject プロトコルのサポート

既定では、既定のハッシュと等値 .NET と Objective C ランタイムの両方の想定する、交換することとよく似たセマンティクスを共有しています。

マネージ型がよりも優先`Equals(Object)`または`GetHashCode`、(.NET) の既定の動作が不十分だったことを通常意味です。 これは既定 Objective C の動作が可能性があることを意味十分ではありませんか。

このような場合で、ジェネレーターよりも優先、 [ `isEqual:` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418795-isequal?language=objc)メソッドおよび[ `hash` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418859-hash?language=objc)で定義されたプロパティ、 [ `NSObject`プロトコル](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc)です。 これにより、透過的に Objective C コードから使用するカスタムのマネージ実装できます。

### <a name="exceptions-support"></a>例外のサポート

渡す`--nativeexception`への引数として`objcgen`マネージ例外をキャッチおよび処理できる Objective C の例外に変換します。 

### <a name="comparison"></a>条件式

実装する型をマネージ`IComparable`(またはそのジェネリック バージョン`IComparable<T>`) を返す OBJECTIVE-C フレンドリ メソッドが生成されます、`NSComparisonResult`を受け入れると、`nil`引数。 これにより、生成済み API の Objective C の開発者にわかりやすい。 例えば:

```objc
- (NSComparisonResult)compare:(XAMComparableType * _Nullable)other;
```

### <a name="categories"></a>カテゴリ

マネージ拡張機能のカテゴリにメソッドが変換されます。 次の拡張メソッドなど、 `Collection`:

```csharp
public static class SomeExtensions {
    public static int CountNonNull (this Collection collection) { ... }
    public static int CountNull (this Collection collection) { ... }
}
```

このような OBJECTIVE-C カテゴリーを作成します。

```objc
@interface Collection (SomeExtensions)

- (int)countNonNull;
- (int)countNull;

@end
```

単一のマネージ型では、いくつかの型を拡張、Objective C の複数のカテゴリが生成されます。

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

Objective C の添字構文を使用してこれを使用できます。

```objc
if ([intCollection [0] isEqual:@42])
    intCollection[0] = @13;
```

インデクサーの種類に応じて適切な場所にインデックスまたはキー付きの添字演算子が生成されます。

これは、[記事](http://nshipster.com/object-subscripting/)は導入添字演算子にも最適です。

## <a name="main-differences-with-net"></a>.NET での主な相違点

### <a name="constructors-vs-initializers"></a>Vs 初期化子のコンス トラクター

OBJECTIVE-C でを呼び出す初期化子の継承チェーン内の親クラスのいずれかのプロトタイプが使用不可とマークされていない限り (`NS_UNAVAILABLE`)。

C# の場合、コンス トラクターは継承されないため、クラス内のコンス トラクター メンバーを明示的に宣言する必要があります。

OBJECTIVE-C、する c# API の右側の表現を公開する`NS_UNAVAILABLE`が親クラスから、子クラスに存在しない任意の初期化子を追加します。

C# API:

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

Objective C では、API を表示します。

```objc
@interface SuperUnique : Unique

- (instancetype)initWithId:(int)id NS_UNAVAILABLE;
- (instancetype)init;

@end
```

ここでは、`initWithId:`利用不可としてマークされています。

### <a name="operator"></a>演算子

Objective C は演算子をサポートしていないため、演算子をクラス セレクターに変換が C# の場合は、オーバー ロードします。

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

ただし、一部の .NET 言語はサポートしない演算子のオーバー ロードをためは含めることも一般的にな[「わかりやすい」](https://docs.microsoft.com/dotnet/standard/design-guidelines/operator-overloads)メソッドに加えて、演算子オーバー ロードをという名前です。

演算子のバージョンと「わかりやすい」のバージョンの両方も見つからないかどうか、親しみやすいバージョンのみを生成するように同じ OBJECTIVE-C 名が生成されます。

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

一般的な演算子で`==`(C#) として処理される、一般的な演算子の上記のとおりです。

ただし、「わかりやすい」の等号演算子が見つからないかどうかは、両方の演算子`==`と演算子`!=`生成ではスキップされます。

### <a name="datetime-vs-nsdate"></a>DateTime vs NSDate

[ `NSDate` ](https://developer.apple.com/reference/foundation/nsdate?language=objc)ドキュメント。

> `NSDate` オブジェクトは、任意の特定の calendrical システムまたはタイム ゾーンに関係なく、でもで単一のポイントをカプセル化します。 Date オブジェクトは不変である絶対参照日を基準としたロケールに依存しない間隔を表す (00:1 月 1 日の 2001 年に 00:00 UTC)。

理由のため`NSDate`日の間のすべての変換を参照し、 `DateTime` UTC で行う必要があります。

#### <a name="datetime-to-nsdate"></a>NSDate に DateTime

変換するときに`DateTime`に`NSDate`、`Kind`プロパティ`DateTime`に考慮されます。

|種類|結果|
|---|---|
|`Utc`|指定されたを使用して変換を実行`DateTime`が格納されるオブジェクトします。|
|`Local`|呼び出しの結果`ToUniversalTime()`を指定した`DateTime`オブジェクトが変換に使用します。|
|`Unspecified`|指定された`DateTime`オブジェクトは UTC で同一の動作と見なされますと`Kind`は`Utc`します。|

変換では、次の数式を使用します。

```
TimeInterval = DateTimeObjectTicks - NSDateReferenceDateTicks / TicksPerSecond
```

この数式では。 

- `NSDateReferenceDateTicks` 基に計算されて、 `NSDate` 2001 年 1 月 1 日の 00時 00分: 00 (utc) を参照します。 
    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```
- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond) 定義されました。 [`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)

作成する、`NSDate`オブジェクト、`TimeInterval`と共に使用される、 `NSDate` [dateWithTimeIntervalSinceReferenceDate:](https://developer.apple.com/reference/foundation/nsdate/1591577-datewithtimeintervalsincereferen?language=objc)セレクター。

#### <a name="nsdate-to-datetime"></a>Datetime NSDate

変換`NSDate`に`DateTime`は次の公式を使用します。

```
DateTimeTicks = NSDateTimeIntervalSinceReferenceDate * TicksPerSecond + NSDateReferenceDateTicks
```

この数式では。 

- `NSDateReferenceDateTicks` 基に計算されて、 `NSDate` 2001 年 1 月 1 日の 00時 00分: 00 (utc) を参照します。 
    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```
- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond) 定義されました。 [`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)

計算の後に`DateTimeTicks`、 `DateTime` [コンス トラクター](https://docs.microsoft.com/dotnet/api/system.datetime.-ctor?#System_DateTime__ctor_System_Int64_System_DateTimeKind_)呼び出されると、設定、`kind`に`DateTimeKind.Utc`です。

> [!NOTE]
> `NSDate` 指定できます`nil`が、`DateTime`構造体を定義することはできませんが、.NET では、`null`です。 提供する場合、 `nil` `NSDate`、既定値に変換されます`DateTime`にマップする値`DateTime.MinValue`です。

`NSDate` 高い最大と最小値より小さくをサポートしている`DateTime`です。 変換するときに`NSDate`に`DateTime`、これらの高い値と低い値に変更されます、 `DateTime` [MaxValue](https://docs.microsoft.com/dotnet/api/system.datetime.maxvalue)または[MinValue](https://docs.microsoft.com/dotnet/api/system.datetime.minvalue)、それぞれします。
