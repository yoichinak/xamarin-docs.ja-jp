---
title: 統一された API の概要
description: Xamarin の Unified API では、Mac、iOS、およびサポート 32 ビットおよび 64 ビット アプリケーションを同じバイナリ間でコードを共有できます。
ms.prod: xamarin
ms.assetid: 5F0CEC18-5EF6-4A99-9DCF-1A3B57EA157C
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 1d159d280bd3b8855c32e3e437dfdefcbe0463cb
ms.sourcegitcommit: 4859da8772dbe920fdd653180450e5ddfb436718
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2018
ms.locfileid: "50235026"
---
# <a name="unified-api-overview"></a>統一された API の概要

Xamarin の Unified API では、Mac、iOS、およびサポート 32 ビットおよび 64 ビット アプリケーションを同じバイナリ間でコードを共有できます。 Unified API は、新しい Xamarin.iOS および Xamarin.Mac プロジェクトに既定で使用されます。

> [!IMPORTANT]
> Unified API の前に、Xamarin のクラシック API は非推奨とされました。 
> - 最後のバージョンのクラシック API (monotouch.dll) をサポートするために Xamarin.iOS では、Xamarin.iOS 9.10 をしました。
> - Xamarin.Mac には、従来の API では、引き続きサポートされていますは更新されなく。 非推奨であるために、開発者は、Unified API アプリケーションを移動します。

## <a name="updating-classic-api-based-apps"></a>クラシックによる API ベースのアプリを更新します。

お使いのプラットフォームに関連する手順に従います。

- [既存のアプリの更新](updating-apps.md)
- [既存の iOS アプリの更新](updating-ios-apps.md)
- [既存の Mac アプリの更新](updating-mac-apps.md)
- [既存の Xamarin.Forms アプリの更新](updating-xamarin-forms-apps.md)
- [バインドの Unified API への移行](update-binding.md)

## <a name="tips-for-updating-code-to-the-unified-apiupdating-tipsmd"></a>[コードを Unified API に更新する場合のヒント](updating-tips.md)

移行するアプリケーションに関係なく、チェック アウト[これらのヒント](updating-tips.md)Unified API を正常に更新できます。

## <a name="library-split"></a>ライブラリの分割

この時点以降は、当社の Api は、2 つの方法に表示されます。

-  **クラシック API:** 32 ビット (のみ) に制限付きで公開されていると、`monotouch.dll`と`XamMac.dll`アセンブリ。
-  **Unified API:** で使用できる 1 つの API で 32 と 64 ビットの開発をサポート、`Xamarin.iOS.dll`と`Xamarin.Mac.dll`アセンブリ。

つまり、企業の開発者 (しない対象アプリ ストア) を引き続き使用できます、既存のクラシック Api 私たちは保持を維持することや、永久にそれらは、新しい Api にアップグレードできます。

<a name="namespace-changes" />

## <a name="namespace-changes"></a>Namespace の変更

Mac および iOS 製品間でコードを共有する手間を減らすためには、製品の Api の名前空間を変更しています。

削除されるプレフィックス"MonoTouch"iOS 製品および"MonoMac"からのデータ型に、Mac の製品から。

これは条件付きコンパイルを使用しなくても iOS と Mac のプラットフォーム間でコードを共有するが簡単であり、ソース コード ファイルの上部にあるノイズの軽減されます。

-  **クラシック API:** 名前空間を使用して、`MonoTouch.`または`MonoMac.`プレフィックス。
-  **Unified API:** 名前空間プレフィックスなし

## <a name="runtime-defaults"></a>ランタイムの既定値

既定では、Unified API、 **SGen**ガベージ コレクターと[新しい参照カウント](~/ios/internals/newrefcount.md)オブジェクトの所有権を追跡するためのシステムです。 この同じ機能は Xamarin.Mac に移植されています。

これでさまざまな開発者を選択し、古いシステムに直面しても簡単に問題が解決[メモリ管理](~/cross-platform/deploy-test/memory-perf-best-practices.md)します。

従来の API の場合でも、新しい参照カウントを有効にすることですが、既定値は保守的とユーザーに変更を加える必要はありません。 Unified API を使用して、既定の変更の営業案件を同時にすべての機能強化を開発者に提供をリファクタリングして、コードを再テストすることです。

## <a name="api-changes"></a>API の変更点

Unified API は非推奨のメソッドを削除し、いくつかのインスタンスがある入力ミスがない従来の Api での元 MonoTouch と MonoMac 名前空間にバインドされているときに、API 名があります。 これらのインスタンスが新しい Unified Api で修正され、コンポーネント、iOS および Mac アプリケーションを更新する必要があります。 次に遭遇する可能性の最も一般的なものの一覧を示します。

|従来の API メソッドの名前|統一された API のメソッド名|
|--- |--- |
|`UINavigationController.PushViewControllerAnimated()`|`UINavigationController.PushViewController()`|
|`UINavigationController.PopViewControllerAnimated()`|`UINavigationController.PopViewController()`|
|`CGContext.SetRGBFillColor()`|`CGContext.SetFillColor()`|
|`NetworkReachability.SetCallback()`|`NetworkReachability.SetNotification()`|
|`CGContext.SetShadowWithColor`|`CGContext.SetShadow`|
|`UIView.StringSize`|`UIKit.UIStringDrawing.StringSize`|

クラシックから Unified API に切り替える場合、変更の完全な一覧を参照してください、[クラシック (monotouch.dll) vs 統合 (Xamarin.iOS.dll) API の相違点](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)ドキュメント。

## <a name="updating-to-unified"></a>統合を更新しています

古い/分割/非推奨の API がいくつか**クラシック**では使用できない、**統合**API。 解決しやすくなりますが、 `CS0616` 、(手動または自動) を開始する前に警告をアップグレードする必要があるありますので、`[Obsolete]`メッセージ (警告の一部) の属性を適切な API を参照してください。

公開されますが、 [ *diff* ](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)クラシックとの統合の前に、または後、プロジェクトの更新に使用できる API の変更点。 まだ修正、クラシックは呼び出しを多くの場合、参照する時間を節約 (以下のドキュメント参照)。

次の手順に従います[既存の iOS アプリを更新する](~/cross-platform/macios/unified/updating-ios-apps.md)、または[Mac アプリ](~/cross-platform/macios/unified/updating-mac-apps.md)Unified API にします。
このページの残りの部分を確認し、[これらのヒント](~/cross-platform/macios/unified/updating-tips.md)コードの移行の詳細については。

### <a name="nuget"></a>NuGet

クラシック API を使用して Xamarin.iOS 以前はサポートされている NuGet パッケージを使用して、アセンブリの公開、 **Monotouch10**プラットフォームのモニカー。

Unified API には互換性のあるパッケージの新しいプラットフォーム識別子が導入されています**Xamarin.iOS10**します。 既存の NuGet パッケージは、Unified API に対してビルドすることで、このプラットフォームのサポートを追加する更新する必要があります。

> [!IMPORTANT]
> フォームでエラーがあれば _"エラー 3 は、同じ Xamarin.iOS プロジェクトで 'monotouch.dll' と 'Xamarin.iOS.dll' の両方を含めることはできません - 'Xamarin.iOS.dll' は 'monotouch.dll' によって参照されますが、明示的に参照される ' xxx、バージョン = 0.0.000、Culture = neutral, PublicKeyToken = null'"_ Unified Api にアプリケーションを変換した後は通常、Unified API に更新されていないプロジェクトで、コンポーネントまたは NuGet パッケージすることが原因です。 既存のコンポーネント/NuGet の削除を許可し、Unified Api をサポートするバージョンに更新し、クリーン ビルドを実行する必要があります。

### <a name="the-road-to-64-bits"></a>64 ビットへの道

32 ビットおよび 64 ビット アプリケーションとフレームワークに関する情報をサポートしている背景情報についてを参照してください。、 [32 ビットおよび 64 ビット プラットフォームに関する考慮事項](~/cross-platform/macios/32-and-64/index.md)します。

 <a name="new-data-types" />

#### <a name="new-data-types"></a>新しいデータ型

相違点の中核には、Mac、iOS Api の両方を常に 32 ビットに 32 ビットのプラットフォームと 64 ビット プラットフォーム上の 64 ビット アーキテクチャに固有のデータ型を使用します。

たとえば、OBJECTIVE-C でマップ、`NSInteger`のデータ型`int32_t`32 ビット システムでと`int64_t`64 ビット システムで。

Unified API では、この動作を一致するように、前に使用を交換する`int`(常として定義されている .NET の`System.Int32`) を新しいデータ型:`System.nint`します。  考えることができます"n"の意味として「ネイティブ」ので、プラットフォームのネイティブの整数を入力します。

紹介`nint`、`nuint`と`nfloat`必要に応じて、それらの上に構築もデータ型を提供します。

これらのデータ型の変更の詳細については、次を参照してください。、[ネイティブ型](~/cross-platform/macios/nativetypes.md)ドキュメント。

### <a name="how-to-detect-the-architecture-of-ios-apps"></a>IOS アプリのアーキテクチャを検出する方法

アプリケーションが 32 ビットまたは 64 ビット iOS システムで実行されているかどうかを把握する必要がある状況である可能性があります。 アーキテクチャを確認する次のコードを使用できます。

```csharp
if (IntPtr.Size == 4) {
    Console.WriteLine ("32-bit App");
} else if (IntPtr.Size == 8) {
    Console.WriteLine ("64-bit App");
}
```

<a name="deprecated-apis" />

### <a name="arrays-and-systemcollectionsgeneric"></a>配列と System.Collections.Generic

C#インデクサーの型を期待する`int`、明示的にキャストする必要があります`nint`値`int`コレクションまたは配列要素にアクセスします。 例えば:

```csharp
public List<string> Names = new List<string>();
...

public string GetName(nint index) {
    return Names[(int)index];
}

```

これは想定される動作であるためですからキャスト`int`に`nint`は 64 ビットの損失を伴う、暗黙的な変換は行われません。

### <a name="converting-datetime-to-nsdate"></a>NSDate DateTime に変換します。

Unified Api の暗黙的な変換を使用する場合`DateTime`に`NSDate`値は実行できなくします。 これらの値は、1 つの型から別に明示的に変換する必要があります。 このプロセスを自動化する、次の拡張メソッドを使用できます。

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

### <a name="deprecated-apis-and-typos"></a>非推奨の Api や入力ミス

内部 Xamarin.iOS クラシック API (monotouch.dll)、`[Obsolete]`属性は、2 つの方法で使用されていました。

-  **IOS API を非推奨とされます:** これは、Apple のヒントを新しいによって置き換えされているため、API の使用を停止する場合。 クラシック API も問題と多くの場合、必要なは (iOS の古いバージョンをサポートする) 場合。
 このような API (および`[Obsolete]`属性)、新しい Xamarin.iOS アセンブリに含まれます。
-  **無効な API**一部の API が、名前にタイプミスを必要があります。

古いコードの互換性のために使用できるを保持する (monotouch.dll と XamMac.dll) は、元のアセンブリのいますが、Unified API アセンブリ (Xamarin.iOS.dll と Xamarin.Mac) から削除されました

<a name="NSObject_ctor" />

### <a name="nsobject-subclasses-ctorintptr"></a>NSObject サブクラス .ctor(IntPtr)

すべて`NSObject`サブクラスを受け入れるコンス トラクターには、`IntPtr`します。 これは、ネイティブ ObjC ハンドルから新しいマネージ インスタンスのインスタンスを作成しますか。

これは従来で、`public`コンス トラクター。 いくつかの作成などのマネージ ObjC インスタンスは 1 つのインスタンスをユーザー コードでは、この機能を誤って使用する簡単だ、ただし*または*(サブクラス) の予期される状態の管理がないマネージ インスタンスを作成します。

この種の問題を回避するために、`IntPtr`コンス トラクターは、今すぐ`protected`で**統合**API、サブクラス化にのみ使用します。 これにより、修正/安全な API を使用して、つまり、ハンドルからマネージ インスタンスを作成

    var label = Runtime.GetNSObject<UILabel> (handle);

(存在する場合)、この API は、既存のマネージ インスタンスに戻りますか (必須) と、新しいものを作成します。 既にクラシックと統合の両方の API で使用できるようになります。

なお、`.ctor(NSObjectFlag)`も`protected`この 1 つをサブクラス化の外部で使用されたことはほとんどありませんが。

<a name="NSAction" />

### <a name="nsaction-replaced-with-action"></a>NSAction アクションに置き換えられます

Unified api で、`NSAction`標準の .NET を優先してが取り外されて`Action`します。 これは、ためにの大きな進歩`Action`は共通の .NET 型に対し`NSAction`Xamarin.iOS に固有のものでした。 まったく同じ処理を行う、両方がなく distinct と互換性のない型コードを同じ結果を実現するために書き込まれることが発生しました。

たとえば、次のように、既存の Xamarin アプリケーションには、次のコードが含まれている場合。

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (new NSAction (delegate() {
    ShowDropDownAnimated (tblDataView);
}));
```
単純なラムダを今すぐ置換できます。

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (() => ShowDropDownAnimated(tblDataView));
```

ため、コンパイラ エラーになりますが、以前、`Action`に割り当てることができません`NSAction`が`UITapGestureRecognizer`受け取るようになりました、`Action`の代わりに、 `NSAction` Unified Api で有効です。

### <a name="custom-delegates-replaced-with-actiont"></a>アクションに置き換えられ、カスタム デリゲート<T>

**統合**(例: 1 つのパラメーター) を簡単な .net のデリゲートに置き換えられました`Action<T>`します。 たとえば、

    public delegate void NSNotificationHandler (NSNotification notification);

として使用できるようになりました、`Action<NSNotification>`します。 この昇格コードでは、再利用し、Xamarin.iOS と独自のアプリケーションの両方の内部コードの重複を削減します。

### <a name="taskbool-replaced-with-taskbooleannserror"></a>タスク<bool>< ブール値、NSError >> タスクに置き換えられます

**クラシック**いくつかの非同期 Api が返す`Task<bool>`します。 ものもありますそれらの場所を使用する場合は、`NSError`つまり、シグネチャの一部であった、`bool`が既に`true`を取得する例外をキャッチする必要がありました、 `NSError`。

このパターンが変更されたいくつかのエラーは非常に一般的であり、戻り値に役立たないため**統合**を返す、`Task<Tuple<Boolean,NSError>>`します。 これにより、成功と非同期呼び出し中に発生した可能性がありますエラーの両方を確認できます。

### <a name="nsstring-vs-string"></a>NSString vs 文字列

いくつかの定数をいくつかのケースから変更する必要がある`string`に`NSString`、例。 `UITableViewCell`

**クラシック**

    public virtual string ReuseIdentifier { get; }

**統合**

    public virtual NSString ReuseIdentifier { get; }

.NET を優先する一般`System.String`型。 ただし、いくつかのネイティブ API、Apple のガイドラインに関係なく定数ポインター (文字列自体ではない) の比較は、ときとして定数を公開していますこれのみ使用できます`NSString`します。

 <a name="protocols" />

### <a name="objective-c-protocols"></a>OBJECTIVE-C プロトコル

API は、最適ではない完全にサポート ObjC プロトコルと、一部のない元 MonoTouch 最も一般的なシナリオをサポートするために追加されました。 この制限はもう存在しませんが、旧バージョンと互換性のため、いくつかの Api は保持内`monotouch.dll`と`XamMac.dll`します。

これらの制限を削除および Unified Api でクリーンアップします。 ほとんどの変更は、次のようになります。

**クラシック**

    public virtual AVAssetResourceLoaderDelegate Delegate { get; }

**統合**

    public virtual IAVAssetResourceLoaderDelegate Delegate { get; }

`I`手段をプレフィックス**統合**ObjC プロトコルの特定の型ではなく、インターフェイスを公開します。 これはいない必要がある場合をサブクラス化 Xamarin.iOS が提供される特定の種類簡単になります。

またより正確かつ簡単に使用できるをなどあるいくつかの API も使用できます。

**クラシック**

    public virtual void SelectionDidChange (NSObject uiTextInput);

**統合**

    public virtual void SelectionDidChange (IUITextInput uiTextInput);

このような API が、ドキュメントを参照せず、しやすいようになりましたし、IDE のコード補完機能の提供、プロトコル/インターフェイスに基づくさらに便利な候補とします。

#### <a name="nscoding-protocol"></a>NSCoding プロトコル

サポートされない場合でも、元のバインドに - すべての型に対して、.ctor(NSCoder) に含まれる、`NSCoding`プロトコル。  1 つ`Encode(NSCoder)`メソッドに存在していた、`NSObject`オブジェクトをエンコードします。
このメソッドは、インスタンスが NSCoding プロトコルに準拠している場合のみ機能します。

Unified API でこれを修正しました。  新しいアセンブリしか、`.ctor(NSCoder)`を型に準拠している場合`NSCoding`します。 このような型が今すぐ必要も、`Encode(NSCoder)`メソッドに準拠した、`INSCoding`インターフェイス。

影響が少ない: ほとんどの場合にこの変更は影響しませんアプリケーション古い、削除、コンス トラクターを使用することもありません。

## <a name="further-tips"></a>他のヒント

注意すべき追加の変更が記載されて、 [Unified API にアプリを更新するためのヒント](~/cross-platform/macios/unified/updating-tips.md)します。

## <a name="sample-code"></a>サンプル コード

年 7 月 31 日の時点での iOS サンプルは、この新しい API へのポートを公開しましたが、`magic-types`で分岐[monotouch サンプル](https://github.com/xamarin/monotouch-samples/commits/magic-types)します。

Mac の両方でサンプルを確認しています、 [mac サンプル](https://github.com/xamarin/mac-samples)リポジトリ (Mavericks または Yosemite で新しい Api を表示) と 32/64 ビットのサンプルでは、マジック型ブランチ[mac サンプル](https://github.com/xamarin/monotouch-samples/commits/magic-types)します。

## <a name="related-links"></a>関連リンク

- [IOS アプリの更新](updating-ios-apps.md)
- [Mac アプリを更新します。](updating-mac-apps.md)
- [Xamarin.Forms アプリを更新します。](updating-xamarin-forms-apps.md)
- [バインドを更新しています](update-binding.md)
- [ヒントを更新しています](updating-tips.md)
- [クラシックと Unified API の相違点](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
- [クロスプラットフォーム アプリでのネイティブ型の使用](~/cross-platform/macios/native-types-cross-platform.md)
