---
title: コードを Unified API に更新する場合のヒント
description: このドキュメントでは、Xamarin の Unified API を使用するようにアプリケーションを更新する際に役立つ一般的なエラーとさまざまなヒントについて説明します。
ms.prod: xamarin
ms.assetid: 8DD34D21-342C-48E9-97AA-1B649DD8B61F
ms.date: 03/29/2017
author: davidortinau
ms.author: daortin
ms.openlocfilehash: ee207ffc83f887e9c86c650b6f93fc2ff9f5b69c
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73014990"
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

`DependsOnTargets="_CollectBundleResources"` 属性を追加します。 以下に例を示します。

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

- `enum`s を比較するには、`(int)` キャストが必要になる場合があります。

- `NSDictionary.IntValue` が `nint`を返すようになったので、代わりに使用できる `Int32Value` があります。

- `nfloat` および `nint` 型を `const`; に設定することはできません。`static readonly nint` は妥当な代替手段です。

- `MonoTouch.` 名前空間に直接含まれていたものは、一般に `ObjCRuntime.` 名前空間にあります (例: `MonoTouch.Constants.Version` が `ObjCRuntime.Constants.Version`)。

- `nint` と `nfloat` 型をシリアル化しようとすると、オブジェクトをシリアル化するコードが破損する可能性があります。 移行後に、シリアル化コードが期待どおりに動作することを必ず確認してください。

- 場合によっては、自動ツールによって `#if #else` 条件付きコンパイラディレクティブ内のコードミスが生じることがあります。 この場合は、手動で修正を行う必要があります (以下の一般的なエラーを参照してください)。

- `[Export]` を使用して手動でエクスポートしたメソッドは、移行ツールによって自動的に修正されない場合があります。たとえば、このコード snippert では、戻り値の型を `nfloat`に手動で更新する必要があります。

  ```csharp
  [Export("tableView:heightForRowAtIndexPath:")]
  public nfloat HeightForRow(UITableView tableView, NSIndexPath indexPath)
  ```

- Unified API は、NSDate と .NET DateTime の間の暗黙の変換を提供しません。これは、可逆変換ではないためです。 に関連するエラーが発生しないようにするには `DateTimeKind.Unspecified` `NSDate`にキャストする前に .NET `DateTime` をローカルまたは UTC に変換します。

- 目標-C カテゴリのメソッドが Unified API の拡張メソッドとして生成されるようになりました。 たとえば、`UIView.DrawString` 以前に使用していたコードは、Unified API 内の `NSString.DrawString` を参照するようになりました。

- `VideoSettings` で AVFoundation クラスを使用するコードは、`WeakVideoSettings` プロパティを使用するように変更する必要があります。 これには、設定クラスのプロパティとして使用できる `Dictionary`が必要です。次に例を示します。

  ```csharp
  vidrec.WeakVideoSettings = new AVVideoSettings() { ... }.Dictionary;
  ```

- NSObject `.ctor(IntPtr)` コンストラクターが public から protected に変更されました ([不適切な使用を防ぐため](~/cross-platform/macios/unified/overview.md#NSObject_ctor))。

- `NSAction` は、標準の .NET `Action`に[置き換えら](~/cross-platform/macios/unified/overview.md#NSAction)れました。 一部の単純な (単一パラメーター) デリゲートも `Action<T>`に置き換えられました。

最後に、[クラシック v Unified API の違い](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)を参照して、コード内の api の変更を検索します。 [このページ](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)を検索すると、クラシック api と、更新されたものを見つけることができます。

> [!NOTE]
> `MonoTouch.Dialog` 名前空間は、移行後も同じままです。 コードで Monotouch.dialog を使用している場合は、その名前空間を使用し続ける必要があり**ます。** `MonoTouch.Dialog` を `Dialog`*に変更しないでください*。

## <a name="common-compiler-errors"></a>一般的なコンパイラエラー

その他の一般的なエラーの例を次に示します。

**エラー CS0012: 型 ' Monotouch.dialog ' は、参照されていないアセンブリで定義されています。**

修正: 通常は、プロジェクトが、Unified API でビルドされていないコンポーネントまたは NuGet パッケージを参照していることを意味します。 すべてのコンポーネントと NuGet パッケージを削除してから、追加し直す必要があります。 それでもエラーが解決しない場合は、外部ライブラリが Unified API をまだサポートしていない可能性があります。

**エラー MT0034: 同じ Xamarin に ' monotouch.dialog ' と ' ' の両方を含めることはできません。 iOS プロジェクト ' monotouch.dialog ' は明示的に参照されていますが、' ' は ' Xamarin. Mobile, Version = 0.6.3.0, Culture = ニュートラル ' によって参照されています。PublicKeyToken = null '。**

修正: このエラーの原因となっているコンポーネントを削除し、プロジェクトに再度追加します。

**エラー CS0234: 型または名前空間の名前 ' Foundation ' が名前空間 ' Monotouch.dialog ' に存在しません。アセンブリ参照が不足していないかどうかを確認してください。**

修正: Visual Studio for Mac の自動化された移行ツールは `Foundation`へのすべての `MonoTouch.Foundation` 参照を更新する*必要があり*ますが、一部のインスタンスでは手動で更新する必要があります。 以前に `MonoTouch`に含まれていた他の名前空間 (`UIKit`など) についても、同様のエラーが発生する可能性があります。

**エラー CS0266: 型 ' double ' を ' system.string ' に暗黙的に変換することはできません**

修正: 型を変更し、`nfloat`にキャストします。 このエラーは、その他の型 (`nint`、など) が64ビットの場合にも発生する可能性があります。

```csharp
nfloat scale = (nfloat)Math.Min(rect.Width, rect.Height);
```

**エラー CS0266: 型 ' CoreGraphics. CGRect ' を ' RectangleF ' に暗黙的に変換することはできません。明示的な変換が存在します (キャストがないことを確認してください)。**

修正: インスタンスを `RectangleF` に変更して `CGRect`、`SizeF` を `CGSize`、`PointF` に `CGPoint`します。 名前空間 `using System.Drawing;` は、`using CoreGraphics;` に置き換える必要があります (まだ存在しない場合)。

**エラー CS1502: ' CoreGraphics. CGContext. SetLineDash (system.string, system.string []) ' には、無効な引数がいくつか含まれています。**

修正: 配列の種類を `nfloat[]` に変更し、`Math.PI`を明示的にキャストします。

```csharp
grphc.SetLineDash (0, new nfloat[] { 0, 3 * (nfloat)Math.PI });
```

**エラー CS0115: ' WordsTableSource. RowsInSection (UIKit. UITableView, int) ' はオーバーライドとしてマークされていますが、オーバーライドする適切なメソッドが見つかりませんでした**

修正: 戻り値とパラメーターの型を `nint`に変更します。 これは、`RowsInSection`、`NumberOfSections`、`GetHeightForRow`、`TitleForHeader`、`GetViewForHeader`など、`UITableViewSource`のメソッドのオーバーライドでよく発生します。

```csharp
public override nint RowsInSection (UITableView tableview, nint section) {
```

**エラー CS0508: `WordsTableSource.NumberOfSections(UIKit.UITableView)`: オーバーライドされたメンバーに対応するには、戻り値の型を ' system.string ' にする必要があり `UIKit.UITableViewSource.NumberOfSections(UIKit.UITableView)`**

修正: 戻り値の型が `nint`に変更された場合は、戻り値を `nint`にキャストします。

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    return (nint)navItems.Count;
}
```

**エラー CS1061: 型 ' CoreGraphics. CGPath ' に ' AddElipseInRect ' の定義が含まれていません**

修正: `AddEllipseInRect`するスペルを修正します。 その他の名前変更は次のとおりです。

- "Color. Black" を `NSColor.Black`に変更します。
- MapKit ' AddAnnotation ' を `AddAnnotations`に変更します。
- AVFoundation ' Dataencoding ' を `Encode`に変更します。
- AVFoundation ' Avfoundation. TypeQRCode ' を `AVMetadataObjectType.QRCode`に変更します。
- AVFoundation ' VideoSettings ' を `WeakVideoSettings`に変更します。
- Popviewコントローラーを `PopViewController`に変更します。
- CoreGraphics ' CGBitmapContext ' を `SetFillColor`に変更します。

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
- [Xamarin. Forms アプリの更新](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [バインドの更新](~/cross-platform/macios/unified/update-binding.md)
- [クロスプラットフォーム アプリでのネイティブ型の使用](~/cross-platform/macios/native-types-cross-platform.md)
- [クラシックと Unified API の違い](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
