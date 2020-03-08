---
title: Unified API の概要
description: Xamarin の Unified API を使用すると、Mac と iOS の間でコードを共有し、同じバイナリで32と64ビットのアプリケーションをサポートすることができます。
ms.prod: xamarin
ms.assetid: 5F0CEC18-5EF6-4A99-9DCF-1A3B57EA157C
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: 8402a48602dd94578e688faeb038aec69684e7d4
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/07/2020
ms.locfileid: "78917557"
---
# <a name="unified-api-overview"></a>Unified API の概要

Xamarin の Unified API を使用すると、Mac と iOS の間でコードを共有し、同じバイナリで32と64ビットのアプリケーションをサポートすることができます。 Unified API は、新しい Xamarin および Xamarin プロジェクトで既定で使用されます。

> [!IMPORTANT]
> Unified API の前にある Xamarin Classic API は非推奨とされました。
>
> - Classic API (monotouch.dialog) をサポートする最新バージョンの Xamarin.iOS 9.10 がありました。
> - Classic API は引き続き Xamarin.Mac でサポートされますが、更新されなくなりました。 非推奨とされているため、開発者はアプリケーションを Unified API に移行する必要があります。

## <a name="updating-classic-api-based-apps"></a>Classic API ベースのアプリの更新

プラットフォームに関連する指示に従います。

- [既存のアプリの更新](updating-apps.md)
- [既存の iOS アプリの更新](updating-ios-apps.md)
- [既存の Mac アプリの更新](updating-mac-apps.md)
- [既存の Xamarin.Forms アプリの更新](updating-xamarin-forms-apps.md)
- [バインドの Unified API への移行](update-binding.md)

## <a name="tips-for-updating-code-to-the-unified-api"></a>[コードを Unified API に更新する場合のヒント](updating-tips.md)

移行するアプリケーションに関係なく、Unified API に正常に更新できるように、[これらのヒント](updating-tips.md)を確認してください。

## <a name="library-split"></a>ライブラリの分割

この時点から、Api は2つの方法で表示されます。

- **Classic API:** 32ビット (のみ) に制限され、`monotouch.dll` アセンブリおよび `XamMac.dll` アセンブリで公開されます。
- **Unified API:** `Xamarin.iOS.dll` と `Xamarin.Mac.dll` アセンブリで使用可能な1つの API を使用して、32と64ビットの両方の開発をサポートします。

つまり、(App Store を対象としない) 企業の開発者にとっては、引き続き既存のクラシック Api を使用し続けることができます。これは、永続的に維持されます。また、新しい Api にアップグレードすることもできます。

<a name="namespace-changes" />

## <a name="namespace-changes"></a>名前空間の変更

Microsoft では、Mac と iOS の製品間でコードを共有するための摩擦を減らすために、製品の Api の名前空間を変更しています。

ここでは、iOS 製品からプレフィックス "Monotouch.dialog" を削除し、Mac 製品の "モノ Mac" をデータ型にドロップします。

これにより、条件付きコンパイルを使用せずに、Mac と iOS のプラットフォーム間でコードを簡単に共有できるようになり、ソースコードファイルの先頭でのノイズが軽減されます。

- **Classic API:** 名前空間は `MonoTouch.` または `MonoMac.` プレフィックスを使用します。
- **Unified API:** 名前空間プレフィックスがありません

## <a name="runtime-defaults"></a>ランタイムの既定値

既定では、Unified API は、オブジェクトの所有権を追跡するために、 **SGen**ガベージコレクターと[新しい参照カウント](~/ios/internals/newrefcount.md)システムを使用します。 これと同じ機能が、Xamarin. Mac に移植されました。

これにより、開発者が以前のシステムに直面したさまざまな問題が解決され、[メモリ管理](~/cross-platform/deploy-test/memory-perf-best-practices.md)も容易になります。

Classic API に対しても新しい Refcount を有効にできることに注意してくださいが、既定値は控えめであり、ユーザーが変更を行う必要はありません。 Unified API では、既定値を変更し、開発者がコードのリファクタリングと再テストを同時に行うことができるようになりました。

## <a name="api-changes"></a>API の変更

Unified API は、非推奨のメソッドを削除します。また、クラシック Api の元の Monotouch.dialog およびモノの Mac 名前空間にバインドされたときに、API 名に入力ミスがあったインスタンスがいくつかあります。 これらのインスタンスは、新しい統合 Api で修正されており、コンポーネント、iOS、および Mac アプリケーションで更新する必要があります。 ここでは、最も一般的なものの一覧を示します。

|Classic API メソッド名|Unified API メソッド名|
|--- |--- |
|`UINavigationController.PushViewControllerAnimated()`|`UINavigationController.PushViewController()`|
|`UINavigationController.PopViewControllerAnimated()`|`UINavigationController.PopViewController()`|
|`CGContext.SetRGBFillColor()`|`CGContext.SetFillColor()`|
|`NetworkReachability.SetCallback()`|`NetworkReachability.SetNotification()`|
|`CGContext.SetShadowWithColor`|`CGContext.SetShadow`|
|`UIView.StringSize`|`UIKit.UIStringDrawing.StringSize`|

クラシックから Unified API に切り替える場合の変更の完全な一覧については、「 [classic (monotouch.dialog)」 API と「API の相違点](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)」ドキュメントを参照してください。

## <a name="updating-to-unified"></a>更新 (統合に)

**クラシック**api では、従来の api や非推奨とされ**ている**api を使用できません。 `[Obsolete]` 属性メッセージ (警告の一部) を使用して適切な API を作成できるため、(手動または自動) アップグレードを開始する前に、`CS0616` の警告を修正する方が簡単です。

ここでは、プロジェクトの更新の前または後に使用できる、クラシック API と統合 API の変更の[*相違*](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)点を公開しています。 クラシックで obsoletes 呼び出しを修正することは、多くの場合、時間の短縮 (ドキュメント参照の削減) です。

[既存の iOS アプリ](~/cross-platform/macios/unified/updating-ios-apps.md)または[Mac アプリ](~/cross-platform/macios/unified/updating-mac-apps.md)を Unified API に更新するには、次の手順に従います。
このページの残りの部分を確認し、コードの移行に関する追加情報については、[次のヒント](~/cross-platform/macios/unified/updating-tips.md)を参照してください。

### <a name="nuget"></a>NuGet

以前に Monotouch10 Classic API でサポートされていた NuGet パッケージは、 platform モニカーを使用してアセンブリを公開しました。

Unified API には、互換性のあるパッケージの新しいプラットフォーム識別子である**iOS10**が導入されています。 このプラットフォームのサポートを追加するには、Unified API に対してビルドすることによって、既存の NuGet パッケージを更新する必要があります。

> [!IMPORTANT]
> "エラー 3" という形式のエラーが発生した場合は _、同じ Xamarin に "monotouch.dialog" と "xamarin. iOS .dll" の両方を含めることはできません。 ios プロジェクト ' monotouch.dialog ' は明示的に参照されていますが、' 0.0.000 ' は、アプリケーションを統合 api に変換した後、' xxx, Version =, Culture = 中立的, PublicKeyToken = null ' によって参照_されています。そのため、通常は、Unified API に更新されていないプロジェクト内のコンポーネントまたは NuGet パッケージが 既存のコンポーネントまたは NuGet を削除し、Unified API をサポートするバージョンに更新し、クリーン ビルドを実行する必要があります。

### <a name="the-road-to-64-bits"></a>64 ビットへの道のり

32および64ビットアプリケーションのサポートと、フレームワークに関する情報の背景については、 [32 および64ビットプラットフォームに関する考慮事項](~/cross-platform/macios/32-and-64/index.md)を参照してください。

 <a name="new-data-types" />

#### <a name="new-data-types"></a>新しいデータ型

違いの中核となるのは、Mac と iOS の両方の Api で、アーキテクチャ固有のデータ型を使用します。このデータ型は、32ビットプラットフォームでは常に32ビット、64ビットプラットフォームでは64ビットです。

たとえば、32ビットシステムでは `NSInteger` データ型を `int32_t` にマップし、64ビットシステムでは `int64_t` にマップします。

この動作に対応するために、Unified API では、以前に使用した `int` (.NET では、常に `System.Int32`として定義されている) を新しいデータ型 (`System.nint`) に置き換えます。  "N" は "ネイティブ" の意味であると考えることができます。したがって、プラットフォームのネイティブ整数型です。

`nint`、`nuint` と `nfloat` について説明し、必要に応じて、その上に構築されたデータ型を提供します。

これらのデータ型の変更の詳細については、[ネイティブ型](~/cross-platform/macios/nativetypes.md)のドキュメントを参照してください。

### <a name="how-to-detect-the-architecture-of-ios-apps"></a>IOS アプリのアーキテクチャを検出する方法

アプリケーションが32ビットまたは64ビットの iOS システムで実行されているかどうかを確認する必要がある場合があります。 アーキテクチャを確認するには、次のコードを使用します。

```csharp
if (IntPtr.Size == 4) {
    Console.WriteLine ("32-bit App");
} else if (IntPtr.Size == 8) {
    Console.WriteLine ("64-bit App");
}
```

<a name="deprecated-apis" />

### <a name="arrays-and-systemcollectionsgeneric"></a>配列と system.string

インデクサー C#では `int`の型が想定されるため、コレクションまたは配列内の要素にアクセスするには、`nint` 値を `int` に明示的にキャストする必要があります。 例 :

```csharp
public List<string> Names = new List<string>();
...

public string GetName(nint index) {
    return Names[(int)index];
}

```

これは予期される動作です。 `int` から `nint` へのキャストは64ビットでは損失されるため、暗黙的な変換は行われません。

### <a name="converting-datetime-to-nsdate"></a>DateTime から NSDate への変換

統合 Api を使用する場合、`DateTime` から `NSDate` 値への暗黙的な変換は実行されなくなりました。 これらの値は、ある型から別の型に明示的に変換する必要があります。 このプロセスを自動化するには、次の拡張メソッドを使用できます。

```csharp
public static DateTime NSDateToDateTime(this NSDate date)
{
    // NSDate has a wider range than DateTime, so clip
    // the converted date to DateTime.Min|MaxValue.
    double secs = date.SecondsSinceReferenceDate;
    if (secs < -63113904000)
        return DateTime.MinValue;
    if (secs > 252423993599)
        return DateTime.MaxValue;
    return (DateTime) date;
}

public static NSDate DateTimeToNSDate(this DateTime date)
{
    if (date.Kind == DateTimeKind.Unspecified)
        date = DateTime.SpecifyKind (date, /* DateTimeKind.Local or DateTimeKind.Utc, this depends on each app */)
    return (NSDate) date;
}

```

<a name="deprecated-typos" />

### <a name="deprecated-apis-and-typos"></a>非推奨の Api と入力ミス

Xamarin. iOS classic API (monotouch.dialog) 内では、`[Obsolete]` 属性が2つの異なる方法で使用されました。

- **非推奨の IOS API:** これは、新しい API によって置き換えられているために、API の使用を停止することを Apple にヒントがある場合です。 Classic API は依然として適切であり、(古いバージョンの iOS をサポートしている場合は) 必要になることがあります。
 このような API (および `[Obsolete]` 属性) は、新しい Xamarin. iOS アセンブリに含まれています。
- **API が正しくありません**API によっては、名前に入力ミスがありました。

元のアセンブリ (monotouch.dialog および XamMac) では、古いコードは互換性のために残されていましたが、Unified API アセンブリからは削除されています (Xamarin. iOS .dll と Xamarin. Mac)。

<a name="NSObject_ctor" />

### <a name="nsobject-subclasses-ctorintptr"></a>NSObject サブクラス (IntPtr)

すべての `NSObject` サブクラスには、`IntPtr`を受け入れるコンストラクターがあります。 これは、ネイティブの ObjC ハンドルから新しいマネージインスタンスをインスタンス化する方法です。

クラシックでは、これは `public` コンストラクターでした。 ただし、この機能をユーザーコードで誤用するのは簡単でした。たとえば、1つの ObjC インスタンスに対して複数のマネージインスタンスを作成したり、必要なマネージ状態 (サブクラスの場合) を持たないマネージインスタンスを作成し*たり*することができます。

このような問題を回避するために、`IntPtr` コンストラクターは、サブクラス化にのみ使用される**統合**API で `protected` されるようになりました。 これにより、適切な/安全な API を使用して、ハンドルからマネージインスタンスを作成することができます。つまり、

```csharp
var label = Runtime.GetNSObject<UILabel> (handle);
```

この API は、(既に存在する場合は) 既存のマネージインスタンスを返すか、または新しいマネージインスタンスを作成します (必要な場合)。 これは、従来の API と統合 API の両方で既に使用できます。

`.ctor(NSObjectFlag)` も `protected` になっていますが、これはサブクラスの外部ではほとんど使用されませんでした。

<a name="NSAction" />

### <a name="nsaction-replaced-with-action"></a>NSAction がアクションに置き換えられました

統合 Api を使用すると、標準の .NET `Action`を優先して `NSAction` が削除されました。 `Action` は一般的な .NET 型であるのに対し、`NSAction` は Xamarin. iOS に固有であるため、これは大幅な改善です。 これらはどちらもまったく同じことを行いますが、互いに互換性のない型であるため、同じ結果を得るためにより多くのコードを記述する必要があります。

たとえば、既存の Xamarin アプリケーションに次のコードが含まれているとします。

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (new NSAction (delegate() {
    ShowDropDownAnimated (tblDataView);
}));
```

これは、単純なラムダに置き換えることができるようになりました。

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (() => ShowDropDownAnimated(tblDataView));
```

以前は、`Action` を `NSAction`に割り当てることはできないため、コンパイラエラーになりますが、`UITapGestureRecognizer` は、統合 Api で有効な `NSAction` ではなく `Action` を取得するようになりました。

### <a name="custom-delegates-replaced-with-actiont"></a>アクション\<T > に置き換えられたカスタムデリゲート

**統合**では、単純な (1 つのパラメーターなど) .net デリゲートが `Action<T>`に置き換えられました。 例:

```csharp
public delegate void NSNotificationHandler (NSNotification notification);
```

を `Action<NSNotification>`として使用できるようになりました。 これにより、コードの再利用が促進され、Xamarin と独自のアプリケーションの両方でコードの重複が減少します。

### <a name="taskbool-replaced-with-taskbooleannserror"></a>タスク\<ブール > タスク < Boolean、:^(Nserror error > に置き換えられました >

**クラシック**では、`Task<bool>`を返す非同期 api がいくつかありました。 ただし、`NSError` が署名に含まれていた場合 (つまり、`bool` が既に `true` ていて、`NSError`を取得するために例外をキャッチする必要がある場合など) は、を使用します。

一部のエラーは非常に一般的で、戻り値は役に立たなかったため、このパターンは**統合**され、`Task<Tuple<Boolean,NSError>>`を返すように変更されました。 これにより、成功と、非同期呼び出し中に発生した可能性のあるエラーの両方を確認できます。

### <a name="nsstring-vs-string"></a>NSString と文字列

場合によっては、いくつかの定数を `string` から `NSString`に変更する必要がありました。たとえば、`UITableViewCell`

**クラシック**

```csharp
public virtual string ReuseIdentifier { get; }
```

**ネットワーク**

```csharp
public virtual NSString ReuseIdentifier { get; }
```

一般に、.NET `System.String` の種類を優先します。 ただし、Apple のガイドラインにかかわらず、一部のネイティブ API は定数ポインターを比較しています (文字列自体ではありません)。これは、定数を `NSString`として公開する場合にのみ機能します。

 <a name="protocols" />

### <a name="objective-c-protocols"></a>目的-C プロトコル

元の Monotouch.dialog は ObjC プロトコルを完全にサポートしていませんでした。また、最も一般的なシナリオをサポートするために、API が最適化されていません。 この制限はもう存在しませんが、旧バージョンとの互換性のために、`monotouch.dll` と `XamMac.dll`の内部でいくつかの Api が保持されます。

これらの制限は、統合 Api で削除され、クリーンアップされています。 ほとんどの変更は次のようになります。

**クラシック**

```csharp
public virtual AVAssetResourceLoaderDelegate Delegate { get; }
```

**ネットワーク**

```csharp
public virtual IAVAssetResourceLoaderDelegate Delegate { get; }
```

`I` プレフィックスは、ObjC プロトコルに対して、特定の型ではなく、インターフェイス**を公開する**ことを意味します。 これにより、提供されている特定の型をサブクラス化する必要がなくなります。

また、一部の API は、より正確で使いやすいものにすることもできました。次に例を示します。

**クラシック**

```csharp
public virtual void SelectionDidChange (NSObject uiTextInput);
```

**ネットワーク**

```csharp
public virtual void SelectionDidChange (IUITextInput uiTextInput);
```

このような API は、ドキュメントを参照しなくても簡単に使用できるようになりました。また、IDE コードを完了すると、プロトコル/インターフェイスに基づいてより有用な提案を得ることができます。

#### <a name="nscoding-protocol"></a>NSCoding プロトコル

元のバインディングでは、`NSCoding` プロトコルがサポートされていない場合でも、すべての型に対して .ctor (NSCoder) が含まれていました。  オブジェクトをエンコードするための `NSObject` に1つの `Encode(NSCoder)` メソッドがありました。
ただし、このメソッドは、インスタンスが NSCoding プロトコルに準拠している場合にのみ機能します。

Unified API、このことを修正しました。  型が `NSCoding`に準拠している場合、新しいアセンブリには `.ctor(NSCoder)` のみが含まれます。 また、このような型には、`INSCoding` インターフェイスに準拠した `Encode(NSCoder)` メソッドが用意されています。

影響が少ない: ほとんどの場合、この変更はアプリケーションに影響を与えず、古いコンストラクターを削除できませんでした。

## <a name="further-tips"></a>その他のヒント

[アプリを Unified API に更新するためのヒント](~/cross-platform/macios/unified/updating-tips.md)には、注意が必要な追加の変更が記載されています。


## <a name="related-links"></a>関連リンク

- [IOS アプリの更新](updating-ios-apps.md)
- [Mac アプリを更新しています](updating-mac-apps.md)
- [Xamarin. Forms アプリの更新](updating-xamarin-forms-apps.md)
- [バインドの更新](update-binding.md)
- [ヒントの更新](updating-tips.md)
- [クラシックと Unified API の違い](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
- [クロスプラットフォーム アプリでのネイティブ型の使用](~/cross-platform/macios/native-types-cross-platform.md)
