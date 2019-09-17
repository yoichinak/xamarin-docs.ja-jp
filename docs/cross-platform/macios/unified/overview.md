---
title: Unified API の概要
description: Xamarin の Unified API を使用すると、Mac と iOS の間でコードを共有し、同じバイナリで32と64ビットのアプリケーションをサポートすることができます。
ms.prod: xamarin
ms.assetid: 5F0CEC18-5EF6-4A99-9DCF-1A3B57EA157C
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: cc27b3162c03f0292d3910f86fac9ed89b117f6f
ms.sourcegitcommit: 61a35d0643eb3bf5adb8f8831da54771d8dde626
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/17/2019
ms.locfileid: "71033323"
---
# <a name="unified-api-overview"></a>Unified API の概要

Xamarin の Unified API を使用すると、Mac と iOS の間でコードを共有し、同じバイナリで32と64ビットのアプリケーションをサポートすることができます。 Unified API は、新しい Xamarin および Xamarin プロジェクトで既定で使用されます。

> [!IMPORTANT]
> Unified API の前にある Xamarin Classic API は非推奨とされました。 
>
> - Classic API (monotouch.dialog) をサポートする最新バージョンの Xamarin. iOS 9.10 がありました。
> - Classic API は引き続き Xamarin. Mac でサポートされますが、更新されなくなりました。 非推奨とされているため、開発者はアプリケーションを Unified API に移行する必要があります。

## <a name="updating-classic-api-based-apps"></a>Classic API ベースのアプリの更新

プラットフォームに関連する指示に従います。

- [既存のアプリの更新](updating-apps.md)
- [既存の iOS アプリの更新](updating-ios-apps.md)
- [既存の Mac アプリの更新](updating-mac-apps.md)
- [既存の Xamarin.Forms アプリの更新](updating-xamarin-forms-apps.md)
- [バインドの Unified API への移行](update-binding.md)

## <a name="tips-for-updating-code-to-the-unified-apiupdating-tipsmd"></a>[コードを Unified API に更新する場合のヒント](updating-tips.md)

移行するアプリケーションに関係なく、Unified API に正常に更新できるように、[これらのヒント](updating-tips.md)を確認してください。

## <a name="library-split"></a>ライブラリの分割

この時点から、Api は2つの方法で表示されます。

- **Classic API:** 32ビット (のみ) に制限され、 `monotouch.dll` `XamMac.dll`アセンブリとアセンブリで公開されます。
- **Unified API:** `Xamarin.iOS.dll` と`Xamarin.Mac.dll`アセンブリで利用できる1つの API を使用して、32と64ビットの両方の開発をサポートします。

つまり、企業の開発者 (アプリストアをターゲットにしない) の場合は、引き続き既存のクラシック Api を使用し続けることができます。これは、永続的に維持されます。また、新しい Api にアップグレードすることもできます。

<a name="namespace-changes" />

## <a name="namespace-changes"></a>名前空間の変更

Microsoft では、Mac と iOS の製品間でコードを共有するための摩擦を減らすために、製品の Api の名前空間を変更しています。

ここでは、iOS 製品からプレフィックス "Monotouch.dialog" を削除し、Mac 製品の "モノ Mac" をデータ型にドロップします。

これにより、条件付きコンパイルを使用せずに、Mac と iOS のプラットフォーム間でコードを簡単に共有できるようになり、ソースコードファイルの先頭でのノイズが軽減されます。

- **Classic API:** 名前空間`MonoTouch.`は`MonoMac.`またはプレフィックスを使用します。
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

**クラシック**api では、従来の api や非推奨とされ**ている**api を使用できません。 `CS0616` 適切`[Obsolete]`な API を作成するための属性メッセージ (警告の一部) があるため、(手動または自動で) アップグレードを開始する前に、警告を修正する方が簡単です。

ここでは、プロジェクトの更新の前または後に使用できる、クラシック API と統合 API の変更の[*相違*](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)点を公開しています。 クラシックで obsoletes 呼び出しを修正することは、多くの場合、時間の短縮 (ドキュメント参照の削減) です。

[既存の iOS アプリ](~/cross-platform/macios/unified/updating-ios-apps.md)または[Mac アプリ](~/cross-platform/macios/unified/updating-mac-apps.md)を Unified API に更新するには、次の手順に従います。
このページの残りの部分を確認し、コードの移行に関する追加情報については、[次のヒント](~/cross-platform/macios/unified/updating-tips.md)を参照してください。

### <a name="nuget"></a>NuGet

以前に Monotouch10 Classic API でサポートされていた NuGet パッケージは、 platform モニカーを使用してアセンブリを公開しました。

Unified API には、互換性のあるパッケージの新しいプラットフォーム識別子である**iOS10**が導入されています。 このプラットフォームのサポートを追加するには、Unified API に対してビルドすることによって、既存の NuGet パッケージを更新する必要があります。

> [!IMPORTANT]
> _"エラー3に ' monotouch.dialog ' と ' 0.0.000 ' の両方を同じ Xamarin に含めることはできません" という形式のエラーが発生した場合は、' monotouch.dialog ' が明示的に参照されていますが、' ' は ' xxx, Version =, Culture = によって参照されています。ニュートラル, PublicKeyToken = null ' "_ アプリケーションを統合 api に変換した後、通常は、Unified API に更新されていないコンポーネントまたは NuGet パッケージがプロジェクトにあることが原因です。 既存のコンポーネントまたは NuGet を削除し、統合された Api をサポートし、クリーンビルドを実行するバージョンに更新する必要があります。

### <a name="the-road-to-64-bits"></a>64ビットへの道路

32および64ビットアプリケーションのサポートと、フレームワークに関する情報の背景については、 [32 および64ビットプラットフォームに関する考慮事項](~/cross-platform/macios/32-and-64/index.md)を参照してください。

 <a name="new-data-types" />

#### <a name="new-data-types"></a>新しいデータ型

違いの中核となるのは、Mac と iOS の両方の Api で、アーキテクチャ固有のデータ型を使用します。このデータ型は、32ビットプラットフォームでは常に32ビット、64ビットプラットフォームでは64ビットです。

たとえば、目標 C は`NSInteger` 、データ`int32_t`型を`int64_t` 32 ビットシステム上のと64ビットシステムにマップします。

この動作に対応するために、この Unified API では、の以前の`int`使用 (.net では、 `System.Int32`常にが定義されている) を`System.nint`新しいデータ型に置き換えます。  "N" は "ネイティブ" の意味であると考えることができます。したがって、プラットフォームのネイティブ整数型です。

さらに`nint` 、必要`nuint`に応じて、その上に構築されたデータ型を提供しています。`nfloat`

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

インデクサー C#では型`int`が必要であるため、コレクションまたは配列内`int`の要素にアクセスするには、値をに明示的にキャスト`nint`する必要があります。 例えば:

```csharp
public List<string> Names = new List<string>();
...

public string GetName(nint index) {
    return Names[(int)index];
}

```

からへの`int`キャストは64ビットでは損失`nint`が発生するため、これは予期される動作です。暗黙的な変換は行われません。

### <a name="converting-datetime-to-nsdate"></a>DateTime から NSDate への変換

統合 api を使用する場合、から値へ`DateTime` `NSDate`の暗黙的な変換は実行されなくなりました。 これらの値は、ある型から別の型に明示的に変換する必要があります。 このプロセスを自動化するには、次の拡張メソッドを使用できます。

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

Xamarin. iOS classic API (monotouch.dialog) 内では、 `[Obsolete]`次の2つの方法で属性が使用されていました。

- **非推奨の iOS API:** これは、新しい API によって置き換えられているために、API の使用を停止することを Apple にヒントがある場合です。 Classic API は依然として適切であり、(古いバージョンの iOS をサポートしている場合は) 必要になることがあります。
 このような API ( `[Obsolete]`および属性) は、新しい Xamarin. iOS アセンブリに含まれています。
- **API が正しくありません**API によっては、名前に入力ミスがありました。

元のアセンブリ (monotouch.dialog および XamMac) では、古いコードは互換性のために残されていましたが、Unified API アセンブリからは削除されています (Xamarin. iOS .dll と Xamarin. Mac)。

<a name="NSObject_ctor" />

### <a name="nsobject-subclasses-ctorintptr"></a>NSObject サブクラス (IntPtr)

すべて`NSObject`のサブクラスには、 `IntPtr`を受け入れるコンストラクターがあります。 これは、ネイティブの ObjC ハンドルから新しいマネージインスタンスをインスタンス化する方法です。

クラシックでは、 `public`これはコンストラクターでした。 ただし、この機能をユーザーコードで誤用するのは簡単でした。たとえば、1つの ObjC インスタンスに対して複数のマネージインスタンスを作成したり、必要なマネージ状態 (サブクラスの場合) を持たないマネージインスタンスを作成し*たり*することができます。

これらの問題を回避するため`IntPtr`に、コンストラクター `protected`は、サブクラス化のためだけに使用される**統合**API になっています。 これにより、適切な/安全な API を使用して、ハンドルからマネージインスタンスを作成することができます。つまり、

```csharp
var label = Runtime.GetNSObject<UILabel> (handle);
```

この API は、(既に存在する場合は) 既存のマネージインスタンスを返すか、または新しいマネージインスタンスを作成します (必要な場合)。 これは、従来の API と統合 API の両方で既に使用できます。

が現在も`.ctor(NSObjectFlag)` `protected`になっていますが、これはサブクラスの外部ではほとんど使用されませんでした。

<a name="NSAction" />

### <a name="nsaction-replaced-with-action"></a>NSAction がアクションに置き換えられました

統合 api を使用する`NSAction`と、は標準の .net `Action`を優先して削除されました。 は一般的な .net 型で`Action`あるのに対し`NSAction`て、これは大幅な改善ですが、は Xamarin. iOS に固有のものです。 これらはどちらもまったく同じことを行いますが、互いに互換性のない型であるため、同じ結果を得るためにより多くのコードを記述する必要があります。

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

以前`Action`は、 `NSAction`に`NSAction`割り当てることはできないため、コンパイラエラーになり`UITapGestureRecognizer`ました`Action`が、ではなくが使用されるようになったため、統合 api で有効になっています。

### <a name="custom-delegates-replaced-with-actiont"></a>アクション\<T > に置き換えられたカスタムデリゲート

**統合**では、単純な (1 つのパラメーターなど) .net デリゲート`Action<T>`がに置き換えられました。 たとえば、

```csharp
public delegate void NSNotificationHandler (NSNotification notification);
```

をとして使用できる`Action<NSNotification>`ようになりました。 これにより、コードの再利用が促進され、Xamarin と独自のアプリケーションの両方でコードの重複が減少します。

### <a name="taskbool-replaced-with-taskbooleannserror"></a>タスク\<bool > タスク < ブール値、:^(nserror error > に置き換えられました >

**クラシック**では、を返す`Task<bool>`非同期 api がいくつかありました。 ただし、が`NSError`シグネチャに含まれていた場合 (つまり、 `bool`が既に存在`true`していて、 `NSError`を取得するために例外をキャッチする必要がある場合) には、が使用されます。

一部のエラーは非常に一般的で、戻り値は役に立たなかったため 、このパターンは`Task<Tuple<Boolean,NSError>>`統合されてを返すように変更されました。 これにより、成功と、非同期呼び出し中に発生した可能性のあるエラーの両方を確認できます。

### <a name="nsstring-vs-string"></a>NSString と文字列

場合によっては、いくつかの定数を`string`から`NSString`に変更する必要がありました。たとえば、`UITableViewCell`

**クラシック**

```csharp
public virtual string ReuseIdentifier { get; }
```

**ネットワーク**

```csharp
public virtual NSString ReuseIdentifier { get; }
```

一般に、.net `System.String`型を優先します。 ただし、Apple のガイドラインにかかわらず、一部のネイティブ API は定数ポインターを比較しています (文字列自体ではありません`NSString`)。これは、定数をとして公開した場合にのみ機能します。

 <a name="protocols" />

### <a name="objective-c-protocols"></a>目的-C プロトコル

元の Monotouch.dialog は ObjC プロトコルを完全にサポートしていませんでした。また、最も一般的なシナリオをサポートするために、API が最適化されていません。 この制限はなくなりましたが、旧バージョンとの互換性のために、 `monotouch.dll`と`XamMac.dll`の内部でいくつかの api が保持されています。

これらの制限は、統合 Api で削除され、クリーンアップされています。 ほとんどの変更は次のようになります。

**クラシック**

```csharp
public virtual AVAssetResourceLoaderDelegate Delegate { get; }
```

**ネットワーク**

```csharp
public virtual IAVAssetResourceLoaderDelegate Delegate { get; }
```

プレフィックス`I`は、特定の型ではなく、objc プロトコルに対して、**統合**されたインターフェイスを公開することを意味します。 これにより、提供されている特定の型をサブクラス化する必要がなくなります。

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

元のバインディングでは、プロトコルを`NSCoding`サポートしていない場合でも、すべての型に対して .ctor (nscoder) が含まれていました。  オブジェクトを`Encode(NSCoder)`エンコードするために、 `NSObject`内に1つのメソッドが存在しました。
ただし、このメソッドは、インスタンスが NSCoding プロトコルに準拠している場合にのみ機能します。

Unified API、このことを修正しました。  新しいアセンブリには、 `.ctor(NSCoder)`型がに`NSCoding`準拠している場合にのみが含まれます。 また、このような型`Encode(NSCoder)`には、 `INSCoding`インターフェイスに準拠するメソッドが含まれるようになりました。

影響: 低ほとんどの場合、この変更はアプリケーションに影響を与えず、古いコンストラクターを削除することができませんでした。

## <a name="further-tips"></a>その他のヒント

[アプリを Unified API に更新するためのヒント](~/cross-platform/macios/unified/updating-tips.md)には、注意が必要な追加の変更が記載されています。

## <a name="sample-code"></a>サンプル コード

7月31日の時点で、 `magic-types` [monotouch.dialog](https://github.com/xamarin/monotouch-samples/commits/magic-types)でブランチのこの新しい API に iOS サンプルのポートを公開しました。

Mac の場合、 [mac サンプル](https://github.com/xamarin/mac-samples)リポジトリ (Mavericks/ヨークの新しい api を表示) と、マジック types branch [Mac サンプル](https://github.com/xamarin/monotouch-samples/commits/magic-types)の32/64 ビットサンプルの両方でサンプルを確認しています。

## <a name="related-links"></a>関連リンク

- [IOS アプリの更新](updating-ios-apps.md)
- [Mac アプリを更新しています](updating-mac-apps.md)
- [Xamarin.Forms アプリの更新](updating-xamarin-forms-apps.md)
- [バインドの更新](update-binding.md)
- [ヒントの更新](updating-tips.md)
- [クラシックと Unified API の違い](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
- [クロスプラットフォーム アプリでのネイティブ型の使用](~/cross-platform/macios/native-types-cross-platform.md)
