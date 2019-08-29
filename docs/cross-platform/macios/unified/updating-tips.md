---
title: コードを Unified API に更新する場合のヒント
description: このドキュメントでは、Xamarin の Unified API を使用するようにアプリケーションを更新する際に役立つ一般的なエラーとさまざまなヒントについて説明します。
ms.prod: xamarin
ms.assetid: 8DD34D21-342C-48E9-97AA-1B649DD8B61F
ms.date: 03/29/2017
author: asb3993
ms.author: amburns
ms.openlocfilehash: 2b82de58b9d2f9e8acb8996f484845f9a71b6e80
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2019
ms.locfileid: "70120312"
---
# <a name="tips-for-updating-code-to-the-unified-api"></a>コードを Unified API に更新する場合のヒント

以前の Xamarin ソリューションを Unified API に更新すると、次のエラーが発生する可能性があります。

## <a name="nsinvalidargumentexception-could-not-find-storyboard-error"></a>NSInvalidArgumentException はストーリーボードエラーを見つけることができませんでした

現在のバージョンの Visual Studio for Mac には、自動化された移行ツールを使用してプロジェクトを統合 Api に変換した後に発生する可能性がある[バグ](https://bugzilla.xamarin.com/show_bug.cgi?id=25569)があります。 更新後、次の形式でエラーメッセージが表示されます。

```console
Objective-C exception thrown. Name: NSInvalidArgumentException Reason: Could not find a storyboard named 'xxx' in bundle NSBundle...
```

この問題を解決するには、次の手順を実行します。次のビルドターゲットファイルを検索します。

```console
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/lib/mono/2.1/Xamarin.iOS.Common.targets
```

このファイルでは、次のターゲット宣言を検索する必要があります。

```xml
<Target Name="_CopyContentToBundle"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

`DependsOnTargets="_CollectBundleResources"`属性を追加します。 以下に例を示します。

```xml
<Target Name="_CopyContentToBundle"
        DependsOnTargets="_CollectBundleResources"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

ファイルを保存し、Visual Studio for Mac を再起動して、プロジェクトのクリーン & リビルドを実行します。 この問題の修正プログラムは、Xamarin によってまもなくリリースされます。

## <a name="useful-tips"></a>役に立つヒント

移行ツールを使用すると、手動操作を必要とするコンパイラエラーが発生する可能性があります。
手動で修正する必要があるものには、次のようなものがあります。

- を`enum`比較すると、 `(int)`キャストが必要になる場合があります。

- `NSDictionary.IntValue`がを返す`nint` `Int32Value`ようになりました。代わりにを使用できます。

- `nfloat`および`nint`型をに設定`const`することはできません。  `static readonly nint`は、適切な代替手段です。

- `MonoTouch.`名前空間に直接含まれていたものは、一般に`ObjCRuntime.` `MonoTouch.Constants.Version`名前空間にあります (たとえば`ObjCRuntime.Constants.Version`、はになります)。

- および型をシリアル化しようとすると、 `nint` `nfloat`オブジェクトをシリアル化するコードが壊れている可能性があります。 移行後に、シリアル化コードが期待どおりに動作することを必ず確認してください。

- 場合によっては、自動`#if #else`ツールが条件付きコンパイラディレクティブ内のコードをミスすることがあります。 この場合は、手動で修正を行う必要があります (以下の一般的なエラーを参照してください)。

- を使用して`[Export]`手動でエクスポートしたメソッドは、移行ツールによって自動的に修正されない場合があります。 `nfloat`たとえば、このコード snippert では、戻り値の型を手動でに更新する必要があります。

    ```csharp
    [Export("tableView:heightForRowAtIndexPath:")]
    public nfloat HeightForRow(UITableView tableView, NSIndexPath indexPath)
    ```

- Unified API は、NSDate と .NET DateTime の間の暗黙の変換を提供しません。これは、可逆変換ではないためです。 に`DateTime` `DateTimeKind.Unspecified` キャストする`NSDate`前に、.net をローカルまたは UTC に変換することに関連するエラーを回避する。

- 目標-C カテゴリのメソッドが Unified API の拡張メソッドとして生成されるようになりました。 たとえば、以前に使用して`UIView.DrawString`いたコード`NSString.DrawString`は、Unified API で参照されるようになりました。

- で`VideoSettings` avfoundation クラスを使用するコードは、 `WeakVideoSettings`プロパティを使用するように変更する必要があります。 これには`Dictionary`、設定クラスのプロパティとして使用できるが必要です。次に例を示します。

    ```csharp
    vidrec.WeakVideoSettings = new AVVideoSettings() { ... }.Dictionary;
    ```

- NSObject `.ctor(IntPtr)`コンストラクターが public から protected に変更されました ([不適切な使用を防ぐため](~/cross-platform/macios/unified/overview.md#NSObject_ctor))。

- `NSAction`は、標準の .NET `Action`に[置き換えら](~/cross-platform/macios/unified/overview.md#NSAction)れました。 一部の単純な (単一パラメーター) デリゲートも、に`Action<T>`置き換えられました。

最後に、[クラシック v Unified API の違い](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)を参照して、コード内の api の変更を検索します。 [このページ](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)を検索すると、クラシック api と、更新されたものを見つけることができます。

> [!NOTE]
> 移行`MonoTouch.Dialog`後も名前空間は変わりません。 コードで monotouch.dialog を使用している場合は、その名前空間を使用し続ける`MonoTouch.Dialog`必要`Dialog`があり**ます。** に変更しないでください。

## <a name="common-compiler-errors"></a>一般的なコンパイラエラー

その他の一般的なエラーの例を次に示します。

**エラー CS0012:型 ' Monotouch.dialog ' は、参照されていないアセンブリで定義されています。**

ファイルこれは通常、プロジェクトが Unified API でビルドされていないコンポーネントまたは NuGet パッケージを参照していることを意味します。 すべてのコンポーネントと NuGet パッケージを削除してから、追加し直す必要があります。 それでもエラーが解決しない場合は、外部ライブラリが Unified API をまだサポートしていない可能性があります。

**エラー MT0034:同じ Xamarin に ' monotouch.dialog ' と ' monotouch.dialog ' の両方を含めることはできません。 iOS プロジェクト ' 0.6.3.0 ' は明示的に参照されていますが、' ' は ' Xamarin. Mobile, Version =, Culture = ニュートラル, PublicKeyToken = null ' によって参照されています。**

ファイルこのエラーの原因となっているコンポーネントを削除してから、プロジェクトにもう一度追加してください。

**エラー CS0234:型または名前空間の名前 ' Foundation ' が名前空間 ' Monotouch.dialog ' に存在しません。アセンブリ参照が不足していないかどうかを確認してください。**

ファイルVisual Studio for Mac の自動化された移行ツールで`MonoTouch.Foundation`は、 `Foundation`へのすべての参照を更新する*必要がありますが*、一部のインスタンスでは手動で更新する必要があります。 など、以前にに`MonoTouch`含まれて`UIKit`いた他の名前空間でも、同様のエラーが発生することがあります。

**エラー CS0266:型 ' double ' を ' system.string ' に暗黙的に変換することはできません。**

修正: 型を変更し、 `nfloat`をにキャストします。 このエラーは、他の型に対して、64ビット相当のもの`nint`(、など) でも発生する可能性があります。

```csharp
nfloat scale = (nfloat)Math.Min(rect.Width, rect.Height);
```

**エラー CS0266:型 ' CoreGraphics. CGRect ' を ' RectangleF ' に暗黙的に変換することはできません。明示的な変換が存在します (キャストがないことを確認してください)。**

ファイル`RectangleF`インスタンスをに`CGPoint`、を、`PointF`からに変更します。 `SizeF` `CGRect` `CGSize` 名前空間`using System.Drawing;`は、(まだ`using CoreGraphics;`存在しない場合) に置き換える必要があります。

**エラー CS1502:' CoreGraphics. CGContext. SetLineDash (system.string, system.string) ' には、無効な引数がいくつか含まれています。**

ファイル配列の型を`nfloat[]`に変更し`Math.PI`、明示的にキャストします。

```csharp
grphc.SetLineDash (0, new nfloat[] { 0, 3 * (nfloat)Math.PI });
```

**エラー CS0115: ' WordsTableSource. RowsInSection (UIKit. UITableView, int) ' はオーバーライドとしてマークされていますが、オーバーライドする適切なメソッドが見つかりませんでした**

ファイル戻り値とパラメーターの型をに`nint`変更します。 これは、通常、、、、、など`UITableViewSource`を`NumberOfSections`含む`RowsInSection` `GetHeightForRow` `TitleForHeader` `GetViewForHeader`メソッドのオーバーライドで発生します。

```csharp
public override nint RowsInSection (UITableView tableview, nint section) {
```

**エラー CS0508: `WordsTableSource.NumberOfSections(UIKit.UITableView)`: 戻り値の型は、オーバーライドされたメンバーに一致するために ' system.string ' でなければなりません`UIKit.UITableViewSource.NumberOfSections(UIKit.UITableView)`**

ファイル戻り値の型がに`nint`変更された場合は、戻り値をに`nint`キャストします。

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    return (nint)navItems.Count;
}
```

**エラー CS1061:型 ' CoreGraphics. CGPath ' に ' AddElipseInRect ' の定義が含まれていません**

ファイルスペルを`AddEllipseInRect`修正します。 その他の名前変更は次のとおりです。

- "Color. Black" をに`NSColor.Black`変更します。
- MapKit ' AddAnnotation ' をに`AddAnnotations`変更します。
- AVFoundation ' Dataencoding ' をに`Encode`変更します。
- AVFoundation ' Avfoundation. TypeQRCode ' をに`AVMetadataObjectType.QRCode`変更します。
- AVFoundation ' VideoSettings ' をに`WeakVideoSettings`変更します。
- Popviewコントローラーのアニメーションを`PopViewController`に変更します。
- CoreGraphics ' CGBitmapContext ' をに`SetFillColor`変更します。

**エラー CS0546: ' MapKit... 座標 ' にオーバーライド可能な set アクセサー (CS0546) が含まれていないため、オーバーライドできません**

MKAnnotation をサブクラス化してカスタム注釈を作成する場合、座標フィールドに setter はなく、getter のみです。

[修正](https://forums.xamarin.com/discussion/comment/109505/#Comment_109505):

- 座標を追跡するためのフィールドを追加する
- 座標プロパティの getter にこのフィールドを返します。
- SetCoordinate メソッドをオーバーライドし、フィールドを設定します。
- 渡された座標パラメーターを使用して、.ctor で SetCoordinate を呼び出します

次のようになっているはずです。

```csharp
class BasicPinAnnotation : MKAnnotation
{
    private CLLocationCoordinate2D _coordinate;

    public override CLLocationCoordinate2D Coordinate
    {
        get
        {
            return _coordinate;
        }
    }

    public override void SetCoordinate(CLLocationCoordinate2D value)
    {
        _coordinate = value;
    }

    public BasicPinAnnotation (CLLocationCoordinate2D coordinate)
    {
        SetCoordinate(coordinate);
    }
}
```

## <a name="related-links"></a>関連リンク

- [アプリの更新](~/cross-platform/macios/unified/updating-apps.md)
- [IOS アプリの更新](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Mac アプリを更新しています](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Xamarin.Forms アプリの更新](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [バインドの更新](~/cross-platform/macios/unified/update-binding.md)
- [クロスプラットフォーム アプリでのネイティブ型の使用](~/cross-platform/macios/native-types-cross-platform.md)
- [クラシックと Unified API の違い](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
