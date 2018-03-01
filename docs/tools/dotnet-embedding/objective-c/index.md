---
title: "Objective C のサポート"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3367A4A4-EC88-4B75-96D0-51B1FCBCE614
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: f6d19f0f6573b17dfb3feb6bf131686413d4e68f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="objective-c-support"></a>Objective C のサポート

## <a name="specific-features"></a>特定の機能

ObjC の生成には、注目すべきであるほとんどの特別な機能がありません。

### <a name="automatic-reference-counting"></a>自動参照カウント

自動参照カウント (円弧) を使用するは**必要**を生成されたバインディングを呼び出します。 Embeddinator ベースのライブラリを使用してプロジェクトをコンパイルする必要があります`-fobjc-arc`です。

### <a name="nsstring-support"></a>NSString サポート

Api を公開する`System.String`型に変換されます`NSString`です。 これにより、メモリ管理を処理するより簡単に`char*`です。

### <a name="protocols-support"></a>プロトコルのサポート

マネージ インターフェイスはすべてのメンバーは ObjC プロトコルに変換されます`@required`です。

### <a name="nsobject-protocol-support"></a>NSObject プロトコルのサポート

既定であると既定のハッシュ、.net と ObjC ランタイムの両方の等値正常であり、交換可能非常によく似たセマンティクスを共有するいるとします。

マネージ型がよりも優先`Equals(Object)`または`GetHashCode`(.NET) の既定の動作が最適なものを通常意味します。 デフォルト Objective C ではありませんいずれかのことが想定されます。

このようなケースで、ジェネレーターよりも優先、 [ `isEqual:` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418795-isequal?language=objc)メソッドおよび[ `hash` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418859-hash?language=objc)で定義されたプロパティ、 [ `NSObject`プロトコル](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc)です。 これにより、透過的に ObjC コードから使用するカスタムのマネージ実装できます。

### <a name="comparison"></a>条件式

実装する型をマネージ`IComparable`ジェネリック バージョンでは`IComparable<T>`ObjC フレンドリ メソッドを表すオブジェクトを生成、`NSComparisonResult`を受け入れると、`nil`引数。 これにより、生成済み API ObjC の開発者にとってのわかりやすい例。

```csharp
- (NSComparisonResult)compare:(XAMComparableType * _Nullable)other;
```

### <a name="categories"></a>カテゴリ

マネージ拡張機能のカテゴリにメソッドが変換されます。 次の拡張メソッドで例`Collection`:

```csharp
    public static class SomeExtensions {

        public static int CountNonNull (this Collection collection) { ... }

        public static int CountNull (this Collection collection) { ... }
    }
```

このような OBJECTIVE-C カテゴリーを作成します。

```csharp
@interface Collection (SomeExtensions)

- (int)countNonNull;
- (int)countNull;
@end
```

単一の管理対象 Objective C の複数のカテゴリを生成し、型はいくつかの種類を拡張します。

### <a name="subscripting"></a>添字演算子 

管理対象のインデックス付きプロパティは、オブジェクトの添字演算子に変換されます。 例:

```csharp
    public bool this[int index] {
        get { return c[index]; }
        set { c[index] = value; }
    }
```

ような Objective C を作成します。

```csharp
- (id)objectAtIndexedSubscript:(int)idx;
- (void)setObject:(id)obj atIndexedSubscript:(int)idx;
```

Objective C の添字構文を使用してこれを使用できます。

```csharp
    if ([intCollection [0] isEqual:@42])
        intCollection[0] = @13;
```

インデクサーの種類に応じて適切な場所にインデックスまたはキー付きの添字演算子が生成されます。

これは、[記事](http://nshipster.com/object-subscripting/)は導入添字演算子にも最適です。

## <a name="main-differences-with-net"></a>.NET での主な相違点

### <a name="constructors-vs-initializers"></a>コンス トラクターと初期化子

OBJECTIVE-C でを呼び出す初期化子のプロトタイプの継承チェーン内の親クラスのいずれかの利用不可 (NS_UNAVAILABLE) としてマークされている場合を除き、します。

C# の場合、クラス内のコンス トラクター メンバーを宣言する必要があります明示的には、コンス トラクターは継承されません。

Objective C に c# API の右側の表現を公開する追加`NS_UNAVAILABLE`が親クラスから、子クラスに存在しない任意の初期化子にします。

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

```objectivec
@interface SuperUnique : Unique

- (instancetype)initWithId:(int)id NS_UNAVAILABLE;
- (instancetype)init;

@end
```

ここでことが分かります`initWithId:`利用不可としてマークされています。

### <a name="operator"></a>演算子

ObjC は演算子をサポートしていないため、演算子をクラス セレクターに変換が C# の場合は、オーバー ロードします。

```csharp
    public static AllOperators operator + (AllOperators c1, AllOperators c2)
    {
        return new AllOperators (c1.Value + c2.Value);
    }
```

から

```csharp
+ (instancetype)add:(Overloads_AllOperators *)anObjectC1 c2:(Overloads_AllOperators *)anObjectC2;
```

ただし、一部の .NET 言語はサポートしない演算子のオーバー ロードをためは含めることも一般的にな[「わかりやすい」](https://msdn.microsoft.com/en-us/library/ms229032(v=vs.110).aspx)メソッドに加えて、演算子オーバー ロードをという名前です。

演算子のバージョンと「わかりやすい」のバージョンの両方も見つからないかどうか、親しみやすいバージョンのみを生成するように、同じ目標 c 名が生成されます。

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

```csharp
+ (instancetype)add:(Overloads_AllOperatorsWithFriendly *)anObjectC1 c2:(Overloads_AllOperatorsWithFriendly *)anObjectC2;
```

### <a name="equality-operator"></a>等値演算子

一般的な演算子での = = C# の場合は、一般的な操作の説明したように上位として処理されます。

ただし、「わかりやすい」の等号演算子が見つからないかどうかは、両方の演算子 = = 演算子と! = はジェネレーションでスキップされます。

### <a name="datetime-vs-nsdate"></a>DateTime vs NSDate

[NSDate の](https://developer.apple.com/reference/foundation/nsdate?language=objc)ドキュメント。

> NSDate オブジェクトは、任意の特定の calendrical システムまたはタイム ゾーンに関係なく、でもで単一のポイントをカプセル化します。 Date オブジェクトは不変である絶対参照日を基準としたロケールに依存しない間隔を表す (00:1 月 1 日の 2001 年に 00:00 UTC)。

理由のため`NSDate`日の間のすべての変換を参照し、 `DateTime` UTC で行う必要があります。

#### <a name="datetime-to-nsdate"></a>NSDate に DateTime

変換するときに`DateTime`に`NSDate`DateTime の`Kind`プロパティを考慮します。

<table>
<tr><th> 種類         </th><th> 結果                                                                                            </th></tr>
<!--tr><td> ------------ </td><td> -------------------------------------------------------------------------------------------------- </td></tr-->
<tr><td> (utc)          </td><td> 変換は、指定された DateTime オブジェクトをそのまま使用して実行されます。                                  </td></tr>
<tr><td> ローカル        </td><td> 呼び出しの結果`ToUniversalTime ()`変換に指定された datetime オブジェクトを使用します。 </td></tr>
<tr><td> 指定されていません。  </td><td> 指定された DateTime オブジェクトは UTC で同一の動作の種類としてと見なされます (utc) = = です。                </td></tr>
</table>

変換は、次の数式を使用して行われます。

> [!NOTE]
> **TimeInterval** NSDateReferenceDateTicks [dt] DateTimeObjectTicks - を =/ [TicksPerSecond](https://msdn.microsoft.com/en-us/library/system.timespan.tickspersecond(v=vs.110).aspx)

NSDate の使用、TimeInterval したら[dateWithTimeIntervalSinceReferenceDate:](https://developer.apple.com/reference/foundation/nsdate/1591577-datewithtimeintervalsincereferen?language=objc)セレクターを作成します。

#### <a name="nsdate-to-datetime"></a>Datetime NSDate

NSDate から DateTime、NSDate を取得するいると仮定する日付を参照するインスタンスは、 **1 月 1 日の 2001 年に 00時 00分: 00 (utc)**し、次の式を使用します。

> [!NOTE]
> **DateTimeTicks** NSDateTimeIntervalSinceReferenceDate を = * [TicksPerSecond](https://msdn.microsoft.com/en-us/library/system.timespan.tickspersecond(v=vs.110).aspx) + NSDateReferenceDateTicks [dt]

計算した後、 **DateTimeTicks**次 DateTime を使用してお[コンス トラクター](https://msdn.microsoft.com/en-us/library/w0d47c9c(v=vs.110).aspx)設定、`kind`に`DateTimeKind.Utc`です。

認識する必要があるいくつかの考慮事項がある、NSDate `nil` DateTime 構造体を .NET では、および定義することはできませんが、`null`です。 指定した場合、 `nil` NSDate お翻訳されますにマップされる既定の DateTime 値に`DateTime.MinValue`です。

MinValue および MaxValue は異なるも NSDate は DateTime のより大きい値と下限の境界をサポートできるため、その設定を datetime のする高いまたは低い値に与えるたびに[MaxValue](https://msdn.microsoft.com/en-us/library/system.datetime.maxvalue(v=vs.110).aspx)または[MinValue](https://msdn.microsoft.com/en-us/library/system.datetime.minvalue(v=vs.110).aspx)それぞれします。

**dt**: NSDate 参照日**1 月 1 日の 2001 年に 00時 00分: 00 (utc)** => `new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;`
