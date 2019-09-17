---
title: 目的-C のサポート
description: このドキュメントでは、.NET 埋め込みでの目的 C のサポートについて説明します。 自動参照カウント、NSString、プロトコル、NSObject プロトコル、例外などについて説明します。
ms.prod: xamarin
ms.assetid: 3367A4A4-EC88-4B75-96D0-51B1FCBCE614
author: conceptdev
ms.author: crdun
ms.date: 11/14/2017
ms.openlocfilehash: 8798e0acf7b1184c64c7012b2f724e2fa7d2c816
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292766"
---
# <a name="objective-c-support"></a>目的-C のサポート

## <a name="specific-features"></a>特定の機能

目標 C の世代には、注意すべきいくつかの特殊な機能があります。

### <a name="automatic-reference-counting"></a>自動参照カウント

生成されたバインディングを呼び出すには、自動参照カウント (弧) を使用する**必要**があります。 .NET 埋め込みベースのライブラリを使用するプロジェクトは、 `-fobjc-arc`を使用してコンパイルする必要があります。

### <a name="nsstring-support"></a>NSString のサポート

型を公開`System.String`する api はに`NSString`変換されます。 これにより、を扱う場合よりも`char*`メモリ管理が簡単になります。

### <a name="protocols-support"></a>プロトコルのサポート

マネージインターフェイスは、すべてのメンバーが`@required`である目的 C プロトコルに変換されます。

### <a name="nsobject-protocol-support"></a>NSObject プロトコルのサポート

既定では、.NET と目的 C ランタイムの両方の既定のハッシュおよび等価性は、類似したセマンティクスを共有しているため、交換可能であると見なされます。

マネージ型がまたは`Equals(Object)`を`GetHashCode`オーバーライドする場合は、通常、既定の (.net) 動作では不十分であったことを意味します。これは、既定の目的 C の動作では不十分である可能性があることを意味します。

このような場合、ジェネレーターは、 [`isEqual:`](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418795-isequal?language=objc) [ `NSObject`プロトコル](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc)で[`hash`](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418859-hash?language=objc)定義されているメソッドとプロパティをオーバーライドします。 これにより、カスタムマネージ実装を目的の C コードから透過的に使用できるようになります。

### <a name="exceptions-support"></a>例外のサポート

`--nativeexception` を`objcgen`引数として渡すと、マネージ例外は、キャッチして処理できるように、目標 C の例外に変換されます。 

### <a name="comparison"></a>条件式

(またはその`IComparable`ジェネリックバージョン`IComparable<T>`) を実装するマネージ型では、を`NSComparisonResult`返し、引数を`nil`受け入れる、目的の C のわかりやすいメソッドが生成されます。 これにより、生成された API が目的の C 開発者にとってよりわかりやすくなります。 例えば:

```objc
- (NSComparisonResult)compare:(XAMComparableType * _Nullable)other;
```

### <a name="categories"></a>カテゴリ

マネージ拡張メソッドは、カテゴリに変換されます。 たとえば、に対して`Collection`次の拡張メソッドを使用します。

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

マネージインデックス付きプロパティは、オブジェクト時に変換されます。 例えば:

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

この[記事](http://nshipster.com/object-subscripting/)では、時の概要について説明します。

## <a name="main-differences-with-net"></a>.NET との主な違い

### <a name="constructors-vs-initializers"></a>コンストラクターと初期化子

目的 C では、継承チェーン内の任意の親クラスの任意の初期化子プロトタイプを呼び出すことができます (使用不可 (`NS_UNAVAILABLE`) としてマークされている場合を除く)。

でC#は、クラス内のコンストラクターメンバーを明示的に宣言する必要があります。これは、コンストラクターが継承されないことを意味します。

C# API の正しい表現を目的の C に公開するために`NS_UNAVAILABLE` 、は親クラスの子クラスに存在しない任意の初期化子に追加されます。

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

ここで`initWithId:`は、は使用不可としてマークされています。

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

では、 `==`のC#一般的な演算子は、前述のように一般的な演算子として処理されます。

ただし、"表示" 等号演算子が見つかった場合は、演算子`==`と演算子`!=`の両方が生成時にスキップされます。

### <a name="datetime-vs-nsdate"></a>DateTime vs NSDate

次の[`NSDate`](https://developer.apple.com/reference/foundation/nsdate?language=objc)ドキュメントを参照してください。

> `NSDate`オブジェクトは、特定の calendrical システムまたはタイムゾーンに依存しない、1つの時点をカプセル化します。 日付オブジェクトは不変であり、絶対参照日付を基準としたインバリアントな時間間隔を表します (00:00:00 UTC の1月 2001)。

参照日付のため、と`DateTime`の間のすべての変換は UTC で実行する必要があります。 `NSDate`

#### <a name="datetime-to-nsdate"></a>DateTime から NSDate

から`DateTime`に`NSDate`変換する場合、 `Kind`の`DateTime`プロパティは次のように考慮されます。

|種類|結果|
|---|---|
|`Utc`|変換は、指定さ`DateTime`れたオブジェクトをそのように使用して実行されます。|
|`Local`|指定さ`ToUniversalTime()` `DateTime`れたオブジェクトでを呼び出した結果が変換に使用されます。|
|`Unspecified`|指定さ`DateTime`れたオブジェクトは UTC であると見なされるの`Kind`で`Utc`、がの場合も同じ動作になります。|

変換では、次の式を使用します。

```
TimeInterval = DateTimeObjectTicks - NSDateReferenceDateTicks / TicksPerSecond
```

この数式の場合: 

- `NSDateReferenceDateTicks`は、 `NSDate` 2001 年1月1日の 00:00:00 UTC の参照日付に基づいて計算されます。 

    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```

- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond)定義されている[`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)

`NSDate` `NSDate`オブジェクトを`TimeInterval`作成するには、を[dateWithTimeIntervalSinceReferenceDate:](https://developer.apple.com/reference/foundation/nsdate/1591577-datewithtimeintervalsincereferen?language=objc) selector と共に使用します。

#### <a name="nsdate-to-datetime"></a>NSDate to DateTime

から`NSDate` へ`DateTime`の変換では、次の式を使用します。

```
DateTimeTicks = NSDateTimeIntervalSinceReferenceDate * TicksPerSecond + NSDateReferenceDateTicks
```

この数式の場合: 

- `NSDateReferenceDateTicks`は、 `NSDate` 2001 年1月1日の 00:00:00 UTC の参照日付に基づいて計算されます。 

    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```

- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond)定義されている[`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)

計算`DateTimeTicks` `DateTimeKind.Utc` `kind`が完了したら、[コンストラクター](https://docs.microsoft.com/dotnet/api/system.datetime.-ctor?#System_DateTime__ctor_System_Int64_System_DateTimeKind_)が呼び出され、をに設定します。`DateTime`

> [!NOTE]
> `NSDate``nil`はに`DateTime`することができますが、は .net の構造体であり`null`、定義上、をにすることはできません。 `nil`を指定`NSDate`した場合は、に`DateTime.MinValue`マップされる既定`DateTime`値に変換されます。

`NSDate`は、より`DateTime`大きい最大値と下限値をサポートします。 から`NSDate`に`DateTime` 変換する場合、これらの値が大きい値と下限値はそれぞれ [MaxValue](https://docs.microsoft.com/dotnet/api/system.datetime.maxvalue) または [MinValue](https://docs.microsoft.com/dotnet/api/system.datetime.minvalue) に変更されます。`DateTime`
