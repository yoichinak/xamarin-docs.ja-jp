---
title: コードを Unified API に更新する場合のヒント
description: このドキュメントでは、Xamarin の Unified API を使用するアプリケーションを更新するときに一般的なエラーと便利なさまざまなヒントを説明します。
ms.prod: xamarin
ms.assetid: 8DD34D21-342C-48E9-97AA-1B649DD8B61F
ms.date: 03/29/2017
author: asb3993
ms.author: amburns
ms.openlocfilehash: a5083e1d31377caece1b8fb4faf33b6e3ff88202
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61211821"
---
# <a name="tips-for-updating-code-to-the-unified-api"></a>コードを Unified API に更新する場合のヒント

古い Xamarin ソリューションを Unified API を更新する場合、次のエラーが発生する可能性があります。

## <a name="nsinvalidargumentexception-could-not-find-storyboard-error"></a>ストーリー ボード エラー NSInvalidArgumentException が見つかりませんでした。

[バグ](https://bugzilla.xamarin.com/show_bug.cgi?id=25569)現在のバージョンの Visual Studio for Mac の自動移行ツールを使用して、Unified Api に、プロジェクトを変換した後に発生することができます。 フォームでエラー メッセージが発生した場合の更新。

```console
Objective-C exception thrown. Name: NSInvalidArgumentException Reason: Could not find a storyboard named 'xxx' in bundle NSBundle...
```

この問題を解決し、次のビルド ターゲット ファイルを検索するには、次を行うことができます。

```console
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/lib/mono/2.1/Xamarin.iOS.Common.targets
```

このファイルに次のターゲットの宣言を検索する必要があります。

```xml
<Target Name="_CopyContentToBundle"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

追加して、`DependsOnTargets="_CollectBundleResources"`属性をします。 以下に例を示します。

```xml
<Target Name="_CopyContentToBundle"
        DependsOnTargets="_CollectBundleResources"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

ファイルを保存 for Mac、Visual Studio を再起動すると、クリーンの操作を行います、プロジェクトの再構築します。 この問題の修正は、Xamarin で間もなくリリースされる必要があります。

## <a name="useful-tips"></a>有用なヒント

移行ツールを使用すると、まだ手動介入を必要とするいくつかのコンパイラ エラーが発生する可能性があります。
手動で修正する必要がある事項は次のとおりです。

* 比較する`enum`s が必要があります、`(int)`キャストします。

* `NSDictionary.IntValue` 返すようになりました、`nint`は、`Int32Value`代わりに使用できます。

* `nfloat` `nint`型をマークすることはできません`const`;  `static readonly nint`妥当な選択肢としては、します。

* 直接使用されていたもの、`MonoTouch.`名前空間は通常のようになりました、`ObjCRuntime.`名前空間 (例::`MonoTouch.Constants.Version`が`ObjCRuntime.Constants.Version`)。

* シリアル化する際、オブジェクトをシリアル化するコードが動作しなく`nint`と`nfloat`型。 シリアル化コードが移行後に期待どおりに動作を確認してください。

* 自動化されたツール内のコードに失敗した場合もあります`#if #else`条件付きコンパイル ディレクティブです。 この場合、修正プログラムを手動で行う必要があります (以下の一般的なエラーを参照してください)。

* 使用して手動でエクスポートされたメソッド`[Export]`戻り値の型を手動で更新する必要がありますこのコード snippert などで、移行ツールによってが自動的に修正されない`nfloat`:

    ```csharp
    [Export("tableView:heightForRowAtIndexPath:")]
    public nfloat HeightForRow(UITableView tableView, NSIndexPath indexPath)
    ```

 * ロスレス変換ではないために、Unified API は NSDate と .NET の DateTime の間の暗黙的な変換を提供しません。 関連するエラーを防ぐために`DateTimeKind.Unspecified`.NET に変換`DateTime`をローカルまたは UTC をキャストする前に`NSDate`します。

 * Objective C カテゴリ メソッドは、Unified API での拡張メソッドとして生成されますようになりました。 たとえば、使用していたコード`UIView.DrawString`参照は`NSString.DrawString`Unified API にします。

 * コードで AVFoundation クラスを使用して`VideoSettings`を使用して変更する必要があります、`WeakVideoSettings`プロパティ。 これが必要です、`Dictionary`は、設定クラスのプロパティとして使用できる例。

    ```csharp
    vidrec.WeakVideoSettings = new AVVideoSettings() { ... }.Dictionary;
    ```

 * NSObject`.ctor(IntPtr)`コンス トラクターがから変更されたためにパブリックに保護された ([不適切な使用を防ぐため](~/cross-platform/macios/unified/overview.md#NSObject_ctor))。

 * `NSAction` されました[交換](~/cross-platform/macios/unified/overview.md#NSAction)starndard .NET で`Action`します。 いくつかの単純な (1 つのパラメーター) のデリゲートが置き換えられましたも`Action<T>`します。

最後を参照してください、[クラシック v Unified API の相違点](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)コード Api への変更を検索します。 検索[このページ](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)クラシック Api とどのようなしたに更新されてを検出するのに役立ちます。

**注:** 、`MonoTouch.Dialog`移行後に同じ名前空間を変更します。 コードで使用する場合**MonoTouch.Dialog** - その名前空間を使用することは引き続き*いない*変更`MonoTouch.Dialog`に`Dialog`!

## <a name="common-compiler-errors"></a>一般的なコンパイラ エラー

他の一般的なエラーの例は、ソリューションと共に、以下に示します。

**エラー CS0012:型 'MonoTouch.UIKit.UIView' は、参照されていないアセンブリで定義されます。**

修正:通常、つまり、プロジェクトは、コンポーネントまたは Unified API を使用したビルドされていない NuGet パッケージを参照します。 削除して、すべてのコンポーネントと NuGet を再度追加する必要がありますパッケージ。 これで、エラーが解決しない場合、外部ライブラリがまだサポートされていません Unified API。

**エラー MT0034:-同じ Xamarin.iOS プロジェクトで 'monotouch.dll' と 'Xamarin.iOS.dll' の両方を含めることはできません 'Xamarin.iOS.dll' は 'monotouch.dll' によって参照されますが、明示的に参照される ' Xamarin.Mobile、バージョン 0.6.3.0、カルチャを = = neutral, PublicKeyToken = null'。**

修正:このエラーの原因となっているコンポーネントを削除し、再度プロジェクトに追加します。

**エラー CS0234:型または名前空間の名前を 'Foundation' が 'MonoTouch' 名前空間に存在しません。アセンブリ参照が見当たりませんか。**

修正:Visual Studio for Mac の自動移行ツール*する必要があります*すべて更新`MonoTouch.Foundation`への参照`Foundation`が手動で更新する必要がいくつかのインスタンスで、します。 他の名前空間に含まれていたのと同様のエラーが表示される`MonoTouch`など`UIKit`します。

**エラー CS0266:'System.float' に ' double' 型に暗黙的に変換することはできません。**

修正: 型を変更しにキャスト`nfloat`します。 このエラーは、64 ビット対応のプロパティは、他の種類にも発生可能性があります (など`nint`、)

```csharp
nfloat scale = (nfloat)Math.Min(rect.Width, rect.Height);
```

**エラー CS0266:型 'System.Drawing.RectangleF' を ' CoreGraphics.CGRect' を暗黙的に変換できません。明示的な変換が存在します (を忘れていませんか?)**

修正:インスタンスに変更`RectangleF`に`CGRect`、`SizeF`に`CGSize`と`PointF`に`CGPoint`します。 名前空間`using System.Drawing;`置き換える必要があります`using CoreGraphics;`(存在していない) 場合。

**エラー CS1502:最適な適してメソッドをオーバー ロードされた ' CoreGraphics.CGContext.SetLineDash (System.nfloat、System.nfloat[])' が無効な引数**

修正:配列型を変更する`nfloat[]`と明示的にキャスト`Math.PI`します。

```csharp
grphc.SetLineDash (0, new nfloat[] { 0, 3 * (nfloat)Math.PI });
```

**エラー CS0115: 'WordsTableSource.RowsInSection (UIKit.UITableView, int)' がオーバーライドがオーバーライドに適したメソッドとしてマークされています。**

修正:戻り値の値とパラメーターの型を変更する`nint`します。 あるようなメソッドのオーバーライドでよく実行`UITableViewSource`など、 `RowsInSection`、 `NumberOfSections`、 `GetHeightForRow`、 `TitleForHeader`、`GetViewForHeader`など。

```csharp
public override nint RowsInSection (UITableView tableview, nint section) {
```

**エラー CS0508:`WordsTableSource.NumberOfSections(UIKit.UITableView)': return type must be 'System.nint' to match overridden member `UIKit.UITableViewSource.NumberOfSections(UIKit.UITableView)'**

修正:戻り値の型が変更された場合`nint`、戻り値をキャスト`nint`します。

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    return (nint)navItems.Count;
}
```

**エラー CS1061:型 'CoreGraphics.CGPath' に 'AddElipseInRect' の定義が含まれていません**

修正:スペルの修正`AddEllipseInRect`します。 その他の名前変更は次のとおりです。

* 'Color.Black' に変更`NSColor.Black`します。
* MapKit 'AddAnnotation' に変更`AddAnnotations`します。
* AVFoundation 'DataUsingEncoding' に変更`Encode`します。
* AVFoundation 'AVMetadataObject.TypeQRCode' に変更`AVMetadataObjectType.QRCode`します。
* AVFoundation 'VideoSettings' に変更`WeakVideoSettings`します。
* 変更を PopViewControllerAnimated`PopViewController`します。
* CoreGraphics 'CGBitmapContext.SetRGBFillColor' に変更`SetFillColor`します。

**エラー CS0546: が 'MapKit.MKAnnotation.Coordinate' にオーバーライド可能な set アクセサー (CS0546) があるないためにオーバーライドできません。**

MKAnnotation をサブクラス化してカスタム注釈を作成するときに、座標フィールドには setter、getter のみがありません。

[修正](https://forums.xamarin.com/discussion/comment/109505/#Comment_109505):

* 座標を追跡するフィールドを追加します。
* 座標のプロパティの get アクセス操作子でこのフィールドを返す
* SetCoordinate メソッドをオーバーライドし、フィールドの設定
* 渡された座標パラメーターを使用して、ctor に SetCoordinate を呼び出す

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

- [アプリを更新します。](~/cross-platform/macios/unified/updating-apps.md)
- [IOS アプリの更新](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Mac アプリを更新します。](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Xamarin.Forms アプリを更新します。](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [バインドを更新しています](~/cross-platform/macios/unified/update-binding.md)
- [クロスプラットフォーム アプリでのネイティブ型の使用](~/cross-platform/macios/native-types-cross-platform.md)
- [クラシックと Unified API の相違点](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
