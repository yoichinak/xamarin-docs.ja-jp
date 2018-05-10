---
title: 統一された API の概要
description: 新しいスタイルの API よりも簡単にこれまで Mac と iOS だけでなくできると、同じ 32 と 64 ビットのアプリケーションをサポートするバイナリ間でコードを共有します。
ms.prod: xamarin
ms.assetid: 5F0CEC18-5EF6-4A99-9DCF-1A3B57EA157C
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 0bdbf4a41ad5737603fccc7e78bc588a2f3acee3
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
---
# <a name="unified-api-overview"></a>統一された API の概要

_新しいスタイルの API よりも簡単にこれまで Mac と iOS だけでなくできると、同じ 32 と 64 ビットのアプリケーションをサポートするバイナリ間でコードを共有します。_

Mac と iOS の間で共有コードを改善し、32 と 64 ビット、上で動作する基本の 1 つのコードがある開発者を有効にする早期 2015 紹介 Unified API を呼び出す Xamarin.Mac と Xamarin.iOS の両方の製品の新しい API です。

> [!IMPORTANT]
> **プロファイル非推奨のクラシック:** クラシック プロファイル (monotouch.dll) から機能を徐々 に廃止する開始 Xamarin.iOS で新しいプラットフォームが追加されるとします。 たとえば、非 NRC (ref カウントには新しい) オプションが削除されました。 NRC が常に有効になってすべて統合されたアプリケーション (つまり NRC 以外がオプションではなかったことはありません) の既知の問題はありません。 今後のリリースでは、ガベージ コレクターとして Boehm を使用するオプションを削除します。 サポートしても、オプションの統合されたアプリケーションで使用できることはありませんでした。 従来のサポートを完全に削除は、Xamarin.iOS 10.0 のリリースで [次へ] の秋にスケジュールされます。

## <a name="ios"></a>iOS

`Xamarin.iOS.dll` Xamarin.iOS 8.6 と共に出荷されたアセンブリは、**最初の安定性とサポートされているバージョン**iOS 用、統合 API のです。
統合 API の以前のプレビュー バージョンは、閉じるが完全に互換性がありません。

## <a name="mac"></a>Mac

`Xamarin.Mac.dll` Xamarin.Mac の安定したチャネル内のアセンブリは、**最初の安定性とサポートされているバージョン**for mac 統合 API の
統合 API の以前のプレビュー バージョンは、閉じるが完全に互換性がありません。

## <a name="runtime-defaults"></a>ランタイムの既定値

既定値は、Unified API、 **SGen**ガベージ コレクターおよび[新しい参照カウント](~/ios/internals/newrefcount.md)オブジェクトの所有権を追跡するためのシステムです。 この同じ機能が Xamarin.Mac に移植されています。

これで、複数の開発者が直面しています。 以前のシステムでし、も簡単に問題が解決[メモリ管理](~/cross-platform/deploy-test/memory-perf-best-practices.md)です。

従来の API に対しても新しい参照カウントを有効にすることに注意してください。 が、既定値は保守的とユーザーに変更を加える必要はありません。 Unified API を使用して、既定の変更の営業案件を要したし、同時にすべての機能強化を開発者に提供、リファクタリングされ、そのコードの再テストします。

<a name="namespace-changes" />

## <a name="library-split"></a>ライブラリの分割

この時点から、2 つの方法で、Api が提示されます。

-  **クラシック API:** 32 ビット (のみ) に限定で公開されていると、`monotouch.dll`と`XamMac.dll`アセンブリ。
-  **統一された API:** で利用可能な単一の API の 32 と 64 ビットの開発をサポート、`Xamarin.iOS.dll`と`Xamarin.Mac.dll`アセンブリ。

つまり、企業の開発者 (いない対象とするアプリ ストア) を引き続き使用できる既存の従来の Api おは保持を保持する永久にしたりするは、新しい Api にアップグレードできます。

### <a name="namespace-changes"></a>Namespace 変更

Mac および iOS 製品間でコードを共有する摩擦を減らすためには、製品の Api の名前空間を変更しています。

削除されるプレフィックス"MonoTouch"iOS 製品や"MonoMac"からのデータ型に Mac 製品からです。

これは条件付きコンパイルを行わなくても、Mac、iOS プラットフォーム間でコードを共有するが簡単であり、ソース コード ファイルの上部にあるノイズの軽減されます。

-  **クラシック API:** 名前空間を使用して`MonoTouch.`または`MonoMac.`プレフィックス。
-  **統一された API:** 名前空間プレフィックスなし

### <a name="api-changes"></a>API の変更

Unified API が非推奨のメソッドを削除し、いくつかのインスタンスがある場所が入力ミス API 名で、従来の Api で元 MonoTouch と MonoMac 名前空間にバインドされているときにします。 これらのインスタンスが、新しい Unified Api で修正され、コンポーネント、iOS および Mac のアプリケーションで更新する必要があります。 次に遭遇する最も一般的なものの一覧を示します。

|従来の API メソッドの名前|統一された API のメソッド名|
|--- |--- |
|`UINavigationController.PushViewControllerAnimated()`|`UINavigationController.PushViewController()`|
|`UINavigationController.PopViewControllerAnimated()`|`UINavigationController.PopViewController()`|
|`CGContext.SetRGBFillColor()`|`CGContext.SetFillColor()`|
|`NetworkReachability.SetCallback()`|`NetworkReachability.SetNotification()`|
|`CGContext.SetShadowWithColor`|`CGContext.SetShadow`|
|`UIView.StringSize`|`UIKit.UIStringDrawing.StringSize`|

クラシックから Unified API に切り替えると、変更の一覧についてを参照してください、[クラシック (monotouch.dll) と統合 (Xamarin.iOS.dll) API の相違点](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)ドキュメント。

### <a name="updating-to-unified"></a>統合された更新

内のいくつかの古い/切断/非推奨 API**クラシック**では使用できない、**統合**API です。 修正が簡単になります、 `CS0616` 、(手動または自動) を開始する前に警告をアップグレードする必要があるありますので、`[Obsolete]`右 API に説明するメッセージ (警告の一部) の属性です。

パブリッシュすることに注意してください、 [ *diff* ](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)クラシック vs の前に、または後、プロジェクトの更新に使用できる API の変更を統合します。 まだ修正、クラシックは呼び出しを多くの場合、参照する時間の節約 (以下のドキュメント参照)。

次の手順に従って[既存の iOS アプリを更新する](~/cross-platform/macios/unified/updating-ios-apps.md)、または[Mac アプリ](~/cross-platform/macios/unified/updating-mac-apps.md)Unified API にします。
このページの残りの部分を確認し、[これらのヒント](~/cross-platform/macios/unified/updating-tips.md)コードの移行の詳細についてはします。

## <a name="components-and-nuget"></a>コンポーネントと NuGet

ほとんどの既存のコンポーネントと NuGet パッケージは、Unified API をサポートするために更新する必要があります。
従来の API に対して作成されたコンポーネントは、Unified API プロジェクトから参照できません。

既存のコンポーネントへの参照がありませんでしたを`monotouch.dll`(または`XamMac.dll`) 更新プログラムは必要ありません。

### <a name="components"></a>コンポーネント

iOS のコンポーネントで、 [Xamarin コンポーネント ストア](https://components.xamarin.com/)Unified API プロジェクトの作業を更新する必要があります。 Xamarin では、発行およびその他の作成者が同じ操作を実行するように促すおのコンポーネントを更新中です。

### <a name="nuget"></a>NuGet

従来の API を使用して Xamarin.iOS 以前がサポートされている NuGet パッケージを使用して、アセンブリの公開、 **Monotouch10**プラットフォーム モニカーです。

Unified API の互換性のあるパッケージは、新しいプラットフォーム id が導入されています**Xamarin.iOS10**です。 既存の NuGet パッケージは、このプラットフォームのサポートを追加する、統合 API に対するビルドで更新する必要があります。

> [!IMPORTANT]
> フォームでエラーがあれば _"エラー 3 は、同じ Xamarin.iOS プロジェクトに 'monotouch.dll' と 'Xamarin.iOS.dll' の両方を含めることはできません - 'Xamarin.iOS.dll' は 'monotouch.dll' によって参照されますが、明示的に参照される ' xxx、バージョン = 0.0.000、Culture = neutral, PublicKeyToken = null'"_ Unified Api にアプリを変換した後は、通常、統合 API が更新されていないプロジェクトで、コンポーネントまたは NuGet パッケージを持つためです。 削除する既存のコンポーネント/NuGet Unified Api をサポートするバージョンに更新して、クリーン ビルドを行う必要があります。

<a name="deprecated-apis" />

## <a name="arrays-and-systemcollectionsgeneric"></a>配列と System.Collections.Generic

C# のインデクサーには、型が想定しているため`int`、明示的にキャストする必要があります`nint`値`int`コレクションまたは配列要素にアクセスします。 例えば:

```csharp
public List<string> Names = new List<string>();
...

public string GetName(nint index) {
    return Names[(int)index];
}

```

これは想定される動作は、ためからキャスト`int`に`nint`は、64 ビットのデータの損失、暗黙的な変換が完了していません。

## <a name="converting-datetime-to-nsdate"></a>NSDate DateTime に変換します。

統合 Api の暗黙的な変換を使用するときに`DateTime`に`NSDate`値は実行できなくします。 これらの値は、別に 1 つの型から明示的に変換する必要があります。 次の拡張メソッドは、このプロセスの自動化に使用できます。

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

## <a name="deprecated-apis-and-typos"></a>非推奨 Api や入力ミス

内部 Xamarin.iOS classic API (monotouch.dll)、`[Obsolete]`属性は、2 つの方法で使用されました。

-  **IOS API を非推奨:** これは、Apple のヒントを新しいバージョンが置き換えされているため、API の使用を停止する場合。 従来の API がまだ正常で多くの場合、必要な (iOS の古いバージョンをサポートする) 場合。
 このような API (および`[Obsolete]`属性) 新しい Xamarin.iOS アセンブリに組み込まれます。
-  **無効な API**一部 API では、入力ミスが、名前にします。

古いコードの互換性のために使用できるが保たれてお (monotouch.dll および XamMac.dll) は、元のアセンブリが、Unified API アセンブリ (Xamarin.iOS.dll および Xamarin.Mac) から削除されています

<a name="NSObject_ctor" />

## <a name="nsobject-subclasses-ctorintptr"></a>NSObject サブクラス .ctor(IntPtr)

各`NSObject`サブクラスを受け付けるコンス トラクターがある、`IntPtr`です。 これは、ネイティブ ObjC ハンドルから新しいマネージ インスタンスをインスタンス化お方法です。

これはクラシックの`public`コンス トラクターです。 いくつかの作成などのマネージ インスタンス 1 つの ObjC インスタンスのことがユーザー コードでこの機能を悪用する簡単で、ただし*または*予想される管理対象の状態 (サブクラス) も遅れマネージ インスタンスを作成します。

これらの種類の問題を回避するのに、`IntPtr`コンス トラクターは今すぐ`protected`で**統一された**サブクラス化のみをするための API です。 これにより修正/安全な API を使用して、つまり、ハンドルからマネージ インスタンスを作成

    var label = Runtime.GetNSObject<UILabel> (handle);

(既に存在する場合)、この API は、既存のマネージ インスタンスに戻りますか (必須) 場合、新しいものを作成します。 クラシックと統合の両方の API に表示されています。

なお、`.ctor(NSObjectFlag)`もここでは、`protected`この 1 つがサブクラス化の外部で使用されたことはほとんどありませんが、します。

<a name="NSAction" />

## <a name="nsaction-replaced-with-action"></a>アクションに置き換えられ NSAction

Api を使用した、統合、`NSAction`が標準の .NET 優先して削除された`Action`です。 これは、ためにの大きな改善`Action`共通の .NET 型がありますが`NSAction`to Xamarin.iOS 特定されました。 どちらもまったく同じことが異なると互換性のない型およびその他のコードが同じ結果を得るために書き込まれることが発生しました。

たとえば、既存の Xamarin アプリには、次のコードが含まれているとします。

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (new NSAction (delegate() {
    ShowDropDownAnimated (tblDataView);
}));
```
単純なラムダを今すぐ置換できます。

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (() => ShowDropDownAnimated(tblDataView));
```

コンパイラ エラーになるためを以前、`Action`に割り当てることができません`NSAction`が`UITapGestureRecognizer`与えるようになりました、`Action`の代わりに、 `NSAction` Unified Api で有効です。

## <a name="custom-delegates-replaced-with-actiont"></a>アクションに置き換えられ、カスタム デリゲート<T>

**統一された**(1 つのパラメーターなど) 簡単なものに置き換えられました .net デリゲート`Action<T>`です。 たとえば、

    public delegate void NSNotificationHandler (NSNotification notification);

として使用するようになりました、`Action<NSNotification>`です。 このプロモート コードでは、再利用し、Xamarin.iOS と独自のアプリケーションの両方の内部コードの重複を低減できます。

## <a name="taskbool-replaced-with-taskbooleannserror"></a>タスク<bool>< ブール値、NSError >> タスクに置き換え

**クラシック**いくつかの非同期 Api が返す`Task<bool>`です。 ただし一部それらの場所ときに使用するには、`NSError`つまり、シグネチャの一部であった、`bool`が既に`true`を取得する例外をキャッチしなければ、`NSError`です。

このパターンが変更された一部のエラーは非常に一般的であり、戻り値に役立たないため**統一された**を返す、`Task<Tuple<Boolean,NSError>>`です。 これにより、成功とエラーの非同期呼び出し中に発生した可能性がありますの両方を確認できます。

## <a name="nsstring-vs-string"></a>NSString vs 文字列

一部の定数をいくつかのケースから変更する必要がある`string`に`NSString`、例。 `UITableViewCell`

**クラシック**

    public virtual string ReuseIdentifier { get; }

**Unified**

    public virtual NSString ReuseIdentifier { get; }

一般に、.NET を優先して`System.String`型です。 ただし、一部のネイティブ API、Apple ガイドラインに関係なく定数ポインター (文字列自体ではありません) の比較は、これが機能する場合のみとして定数を公開して`NSString`です。

 <a name="protocols" />

## <a name="objective-c-protocols"></a>Objective C のプロトコル

元の MonoTouch 最適ではない完全なサポート ObjC プロトコルと、一部がありませんでした、API は、最も一般的なシナリオをサポートするために追加されました。 この制限はもう存在しませんが、旧バージョンと互換性のため、いくつかの Api の場所に内部`monotouch.dll`と`XamMac.dll`です。

これらの制限を削除および Unified Api 上でクリーンアップします。 ほとんどの変更は、次のようになります。

**クラシック**

    public virtual AVAssetResourceLoaderDelegate Delegate { get; }

**Unified**

    public virtual IAVAssetResourceLoaderDelegate Delegate { get; }

`I`手段をプレフィックス**統一された**ObjC プロトコル用の特定の型ではなく、インターフェイスを公開します。 ない必要がある場合をサブクラス化 Xamarin.iOS が提供される特定の種類が容易できるようになります。

より正確かつ使いやすいなどいくつかの API も使用できます。

**クラシック**

    public virtual void SelectionDidChange (NSObject uiTextInput);

**Unified**

    public virtual void SelectionDidChange (IUITextInput uiTextInput);

このような API のドキュメントを参照することがなく、しやすいようになりましたし、プロトコル/インターフェイスに基づく役に立つの提案と、IDE のコード補完機能が提供されます。

### <a name="nscoding-protocol"></a>NSCoding プロトコル

当社の元のバインドをサポートしなかった場合でも、.ctor(NSCoder) - すべての型が含まれる、`NSCoding`プロトコルです。  1 つ`Encode(NSCoder)`メソッドが、`NSObject`オブジェクトをエンコードします。
このメソッドはインスタンスが NSCoding プロトコルに準拠している場合にのみ機能します。

Unified API でこれが解決されました。  のみ、新しいアセンブリを持つ、`.ctor(NSCoder)`を型に準拠している場合`NSCoding`です。 このような型が今すぐ必要も、`Encode(NSCoder)`に準拠したメソッド、`INSCoding`インターフェイスです。

影響が少ない: ほとんどの場合にこの変更は影響しませんアプリケーション古い、削除、コンス トラクターを使用することもできません。

## <a name="further-tips"></a>他のヒント

注意すべき追加の変更が記載されて、 [Unified API にアプリを更新するためのヒント](~/cross-platform/macios/unified/updating-tips.md)です。

## <a name="sample-code"></a>サンプル コード

年 7 月 31日の時点でのこの新しい API に iOS サンプルのポートを公開しましたが、`magic-types`で分岐[monotouch サンプル](https://github.com/xamarin/monotouch-samples/commits/magic-types)です。

Mac でのデータベースの両方のサンプルをチェック、 [mac サンプル](https://github.com/xamarin/mac-samples)リポジトリ (Mavericks または Yosemite での新しい Api を表示する) だけでなくマジック型分岐での 32/64 ビット サンプル[mac サンプル](https://github.com/xamarin/monotouch-samples/commits/magic-types)です。

## <a name="related-links"></a>関連リンク

- [IOS アプリの更新](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Mac アプリケーションの更新](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Xamarin.Forms アプリの更新](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [バインドの更新](~/cross-platform/macios/unified/update-binding.md)
- [クロスプラットフォーム アプリでのネイティブ型の使用](~/cross-platform/macios/native-types-cross-platform.md)
- [ヒントの更新](~/cross-platform/macios/unified/updating-tips.md)
- [従来の vs Unified API の相違点](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
