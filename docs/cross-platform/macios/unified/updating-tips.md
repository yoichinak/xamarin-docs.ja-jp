---
title: "コードの統合の API を更新するためのヒント"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8DD34D21-342C-48E9-97AA-1B649DD8B61F
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 027b5869dbf52adf70c57d192ed638b10a6c8cdc
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="tips-for-updating-code-to-the-unified-api"></a>コードの統合の API を更新するためのヒント

IOS および Mac プロジェクト Unified API (Xamarin.iOS.dll または Xamarin.Mac.dll) を参照する、必要なコード変更の多くを作成して、Mac は変換用の Visual Studio で自動的に移行ツールを使用して (を参照してください、 [Xamarin Studio のリリースノートのセクション 'Unified API コード移行ツールをクラシック'](http://developer.xamarin.com/releases/studio/xamarin.studio_5.7/xamarin.studio_5.7/)詳細については特定) します。

## <a name="nsinvalidargumentexception-could-not-find-storyboard-error"></a>ストーリー ボード エラー NSInvalidArgumentException が見つかりませんでした。

[バグ](https://bugzilla.xamarin.com/show_bug.cgi?id=25569)現在のバージョンの Visual Studio for Mac を自動的に移行ツールを使用して、Unified Api に、プロジェクトを変換した後に発生することができます。 更新後、形式で、エラー メッセージが発生した場合。

```console
Objective-C exception thrown. Name: NSInvalidArgumentException Reason: Could not find a storyboard named 'xxx' in bundle NSBundle...

```

この問題を解決し、次のビルド ターゲット ファイルを探しには、次を行うことができます。

```console
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/lib/mono/2.1/Xamarin.iOS.Common.targets
```

このファイルには、ターゲットの次の宣言を検索する必要があります。

```xml
<Target Name="_CopyContentToBundle"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

追加し、`DependsOnTargets="_CollectBundleResources"`属性をします。 以下に例を示します。

```xml
<Target Name="_CopyContentToBundle"
        DependsOnTargets="_CollectBundleResources"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

ファイルを保存、for Mac を Visual Studio を再起動しクリーンアップ操作を行います (&)、プロジェクトの再構築します。 この問題の修正プログラムは、Xamarin で間もなくリリースされる必要があります。

## <a name="useful-tips"></a>便利なヒント

移行ツールを使用すると、引き続き手動による介入を必要とするいくつかコンパイラ エラーが発生する可能性があります。
手動で修正する必要があるものは次のとおりです。

* 比較する`enum`秒かかることが、`(int)`キャストします。

* `NSDictionary.IntValue` 返します、`nint`は、`Int32Value`代わりに使用できます。

* `nfloat` および`nint`型をマークすることはできません`const`です。  `static readonly nint`妥当な代替です。

* 直接にするために使用すること、`MonoTouch.`名前空間は、通常のようになりました、`ObjCRuntime.`名前空間 (例::`MonoTouch.Constants.Version`現在`ObjCRuntime.Constants.Version`)。

* シリアル化しようとするときにオブジェクトをシリアル化するコードが分割`nint`と`nfloat`型です。 シリアル化コードが移行後に期待どおりに動作を確認してください。

* 自動化されたツールが内のコードをミスする場合があります`#if #else`条件付きコンパイル ディレクティブです。 ここでは、修正プログラムを手動で行う必要があります (以下の一般的なエラーを参照してください)。

* 使用して手動でエクスポートされたメソッド`[Export]`自動的に解決される戻り値の型を手動で更新する必要がありますこのコード snippert の例では、移行ツールによって`nfloat`:

    ```csharp
    [Export("tableView:heightForRowAtIndexPath:")]
    public nfloat HeightForRow(UITableView tableView, NSIndexPath indexPath)
    ```

 * Unified API では、無損失変換ではないために、NSDate および .NET の datetime 型の間の暗黙的な変換は提供されません。 関連するエラーを防ぐために`DateTimeKind.Unspecified`.NET に変換`DateTime`をローカルまたは utc のどちらにキャストする前に`NSDate`です。

 * Objective C のカテゴリのメソッドは、Unified API での拡張メソッドとして生成されますようになりました。 たとえば、コードを使用していた`UIView.DrawString`参照は`NSString.DrawString`Unified API でします。

 * 持つ AVFoundation クラスを使用したコード`VideoSettings`使用に変更する必要があります、`WeakVideoSettings`プロパティです。 これは、必要があります、`Dictionary`は設定クラスのプロパティとして使用できる例。

    ```csharp
    vidrec.WeakVideoSettings = new AVVideoSettings() { ... }.Dictionary;
    ```

 * NSObject`.ctor(IntPtr)`コンス トラクターから変更されているためにパブリックに保護された ([不適切な使用を防ぐため](~/cross-platform/macios/unified/index.md#NSObject_ctor))。

 * `NSAction` されました[交換](~/cross-platform/macios/unified/index.md#NSAction)starndard .NET と`Action`です。 いくつかの単純な (1 つのパラメーター) のデリゲートが置き換えられても`Action<T>`します。

最後を参照してください、[クラシック v Unified API の相違点](http://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)コードの Api への変更を検索します。 検索[このページ](http://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)クラシック Api とどのようなしたに更新されましたを検出するのに役立ちます。

**注:** 、`MonoTouch.Dialog`移行後に同じ名前空間のままです。 コードで使用する場合**MonoTouch.Dialog**その名前空間を使用して操作を続行する必要があります*いない*変更`MonoTouch.Dialog`に`Dialog`!

## <a name="common-compiler-errors"></a>一般的なコンパイラ エラー

一般的なエラーの他の例は、ソリューションと共に、次に示します。

**エラー CS0012: 型 'MonoTouch.UIKit.UIView' は、参照されていないアセンブリで定義されます。**

修正: つまり、通常、プロジェクトは、コンポーネントまたは Unified API でビルドされていない NuGet パッケージを参照します。 削除して、すべてのコンポーネントと NuGet の再追加する必要がありますパッケージです。 これで、エラーが解決しない場合、外部ライブラリまだサポートしていません Unified API。

**エラー MT0034: 同じ Xamarin.iOS プロジェクトでの 'monotouch.dll' と 'Xamarin.iOS.dll' の両方を含めることはできません 'monotouch.dll' によって参照されますが、明示的に参照される 'Xamarin.iOS.dll' ' Xamarin.Mobile, Version = 0.6.3.0、Culture = neutral,PublicKeyToken = null' です。**

修正: は、このエラーの原因となったコンポーネントを削除して、再度プロジェクトに追加します。

**エラー CS0234: 型または名前空間の名前 'Foundation' が 'MonoTouch' 名前空間に存在しません。アセンブリ参照が見当たりませんか。**

修正: Visual Studio for Mac では、自動的に移行ツール*必要があります*すべて更新`MonoTouch.Foundation`への参照`Foundation`ただし一部のインスタンスでこれらが必要があります手動で更新する、します。 他の名前空間に含まれていたのと同様のエラーが表示される`MonoTouch`など`UIKit`です。

**エラー CS0266: が 'System.float' に ' double' 型を暗黙に変換できません。**

: 型を変更して、修正にキャスト`nfloat`です。 64 ビット対応のプロパティを持つ他の型のこのエラーがあります (など`nint`、)

```csharp
nfloat scale = (nfloat)Math.Min(rect.Width, rect.Height);
```

**エラー CS0266: は、型 'System.Drawing.RectangleF' を ' CoreGraphics.CGRect' を暗黙的に変換できません。明示的な変換が存在するが不足しています、キャストしますか?)**

修正: インスタンスの変更`RectangleF`に`CGRect`、`SizeF`に`CGSize`、および`PointF`に`CGPoint`です。 名前空間`using System.Drawing;`に置き換える必要がある`using CoreGraphics;`(でない場合、存在)。

**エラー CS1502: 最適な適してメソッドをオーバー ロード ' CoreGraphics.CGContext.SetLineDash (System.nfloat、System.nfloat[])' が無効な引数**

修正: 変更配列型を`nfloat[]`と明示的にキャスト`Math.PI`です。

```csharp
grphc.SetLineDash (0, new nfloat[] { 0, 3 * (nfloat)Math.PI });
```

**エラー CS0115: 'WordsTableSource.RowsInSection (UIKit.UITableView, int)' マークが付いているオーバーライドがないに適切なメソッドをオーバーライドするが見つかりませんでした。**

修正: に戻り値の値とパラメーターの型を変更する`nint`です。 これは、問題は発生などのメソッドのオーバーライド内で`UITableViewSource`など、 `RowsInSection`、 `NumberOfSections`、 `GetHeightForRow`、 `TitleForHeader`、 `GetViewForHeader`, などです。

```csharp
public override nint RowsInSection (UITableView tableview, nint section) {
```

**エラー CS0508: `WordsTableSource.NumberOfSections(UIKit.UITableView)': return type must be 'System.nint' to match overridden member `UIKit.UITableViewSource.NumberOfSections(UIKit.UITableView)'**

修正: 戻り値の型が変更されたときに`nint`、戻り値をキャスト`nint`です。

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    return (nint)navItems.Count;
}
```

**エラー CS1061: 型 'CoreGraphics.CGPath' は定義 'AddElipseInRect' の**

修正: にスペルを訂正する`AddEllipseInRect`です。 その他の名前変更は次のとおりです。

* 'Color.Black' に変更`NSColor.Black`です。
* MapKit 'AddAnnotation' に変更`AddAnnotations`です。
* AVFoundation 'DataUsingEncoding' に変更`Encode`です。
* AVFoundation 'AVMetadataObject.TypeQRCode' に変更`AVMetadataObjectType.QRCode`です。
* AVFoundation 'VideoSettings' に変更`WeakVideoSettings`です。
* 変更を PopViewControllerAnimated`PopViewController`です。
* CoreGraphics 'CGBitmapContext.SetRGBFillColor' に変更`SetFillColor`です。

**エラー CS0546: が 'MapKit.MKAnnotation.Coordinate' にオーバーライド可能な set アクセサー (cs0546 エラー) があるないためにオーバーライドできません。**

MKAnnotation をサブクラス化して、カスタムの注釈を作成するときに、座標フィールドには setter、getter のみがありません。

[修正](https://forums.xamarin.com/discussion/comment/109505/#Comment_109505):

* 座標を追跡するフィールドを追加します。
* 座標のプロパティの get アクセス操作子にこのフィールドを返す
* SetCoordinate メソッドをオーバーライドし、フィールドを設定
* 渡された座標パラメーターを使用して、ctor で SetCoordinate を呼び出す

次のようになります。

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
- [Mac アプリケーションの更新](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Xamarin.Forms アプリの更新](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [バインドの更新](~/cross-platform/macios/unified/update-binding.md)
- [クロスプラットフォーム アプリでのネイティブ型の使用](~/cross-platform/macios/native-types-cross-platform.md)
- [従来の vs Unified API の相違点](http://developer.xamarin.comhttps://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
