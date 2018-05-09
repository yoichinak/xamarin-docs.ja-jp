---
title: 右から左へのローカライズ
description: 右から左へのローカライズでは、Xamarin.Forms アプリケーションを右から左へのフローの方向のサポートを追加します。
ms.prod: xamarin
ms.assetid: 90E0CB16-C42A-4CC8-A70E-0C2CFB64A429
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 3201c3161d66163cabffdb36465356192bdd3843
ms.sourcegitcommit: daa089d41cfe1ed0456d6de2f8134cf96ae072b1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
---
# <a name="right-to-left-localization"></a>右から左へのローカライズ

_右から左へのローカライズでは、Xamarin.Forms アプリケーションを右から左へのフローの方向のサポートを追加します。_

> [!NOTE]
> 右から左へのローカライズには、iOS 9 以上と API 17 以上 Android での使用が必要です。

フロー方向とは、ページ上の UI 要素が目によってスキャンされる方向です。 アラビア語やヘブライ語など、一部の言語では、UI 要素が右から左へのフローの方向にレイアウトされることが必要です。 設定してこれを行う、 [ `VisualElement.FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティです。 このプロパティを取得またはそれらのレイアウトを制御しのいずれかに設定する必要がある親要素内でどの UI 要素のフローの方向に設定、 [ `FlowDirection` ](xref:Xamarin.Forms.FlowDirection)列挙値。

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToRight`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

設定、 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティを[ `RightToLeft` ](xref:Xamarin.Forms.FlowDirection.RightToLeft)要素で、右、右から左への読み取り順序とコントロールのレイアウトを受け渡しする通常の配置を設定右から左へ。

[![右から左方向にアラビア語の TodoItemPage](rtl-images/TodoItemPage-Arabic.png "、右から左へのフローの方向とアラビア語の TodoItemPage")](rtl-images/TodoItemPage-Arabic-Large.png#lightbox "右から左へのフローの方向とアラビア語の TodoItemPage")

> [!TIP]
> のみを設定する必要があります、 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)初期レイアウトのプロパティです。 実行時にこの値を変更すると、パフォーマンスに影響する高価なレイアウト処理が行われます。

既定値[ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 、親のない要素のプロパティの値が[ `LeftToRight` ](xref:Xamarin.Forms.FlowDirection.LeftToRight)、既定値を while`FlowDirection`の親を持つ要素が[ `MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent). そのため、要素が継承、`FlowDirection`をビジュアル ツリー、および任意の要素の親からプロパティ値がその親から取得された値を上書きできます。

> [!TIP]
> 右から左へ記述する言語用のアプリをローカライズする場合は、設定、 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)ページまたはルートのレイアウトのプロパティです。 これにより、すべてのページ、または、フローの方向に適切に応答のルート レイアウト内に含まれる要素です。

## <a name="respecting-device-flow-direction"></a>デバイスのフローの方向を考慮し

デバイスのフローの方向を考慮し、選択した言語に基づいてし、地域が、明示的な開発者に選択肢は自動的に行われません。 設定で行うことができます、 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) ページまたはルート レイアウト プロパティを`static` [ `Device.FlowDirection` ](xref:Xamarin.Forms.Device.FlowDirection)値。

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

```csharp
this.FlowDirection = Device.FlowDirection;
```

ページ、またはルート レイアウトのすべての子要素は既定では、継承、 [ `Device.FlowDirection` ](xref:Xamarin.Forms.Device.FlowDirection)値。

## <a name="platform-setup"></a>プラットフォームのセットアップ

特定のプラットフォームの設定は、右から左へのロケールを有効にする必要があります。

### <a name="ios"></a>iOS

必須の右から左のロケールがサポートされている言語としての配列項目に追加する必要があります、`CFBundleLocalizations`キー **Info.plist**です。 次の例では、アラビア語の配列に追加されて、`CFBundleLocalizations`キー。

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>ar</string>
</array>
```

![サポートされている Info.plist 言語](rtl-images/ios-locales.png "Info.plist でサポートされる言語")

詳細については、次を参照してください。 [iOS のローカリゼーション基本](https://docs.microsoft.com/en-gb/xamarin/ios/app-fundamentals/localization/#localization-basics-in-ios)です。

右から左にローカライズするには、右から左のロケールで指定されているを言語と、デバイスまたはシミュレーター上の領域を変更してテストできます**Info.plist**です。

> [!WARNING]
> なお、変更するときに、言語と地域、iOS で右から左のロケールに、 [ `DatePicker` ](xref:Xamarin.Forms.DatePicker)ビュー、ロケールに必要なリソースが含まれていない場合、例外がスローされます。 たとえば、アラビア語を持つアプリをテストするときに、 `DatePicker`、いることを確認**mideast**でが選択されている、**国際化**のセクションで、 **iOS ビルド**ウィンドウです。

### <a name="android"></a>Android

アプリの**AndroidManifest.xml**ファイルを更新する必要があるように、`<uses-sdk>`ノード セット、 `android:minSdkVersion` 17、属性と`<application>`ノード セット、`android:supportsRtl`属性を`true`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest ... >
    <uses-sdk android:minSdkVersion="17" ... />
    <application ... android:supportsRtl="true">
    </application>
</manifest>
```

デバイス/エミュレーターを使用して、右から左へ記述する言語を変更するか、有効にすると、右から左へのローカライズをテストすることができますし、 **Force RTL レイアウトの方向**で**設定 > 開発者オプション**です。

### <a name="universal-windows-platform-uwp"></a>ユニバーサル Windows プラットフォーム (UWP)

必要な言語リソースを指定する必要があります、`<Resources>`のノード、 **Package.appxmanifest**ファイル。 次の例は、アラビア語に追加されたことを示しています、`<Resources>`ノード。

```xml
<Resources>
    <Resource Language="x-generate"/>
    <Resource Language="en" />
    <Resource Language="ar" />
</Resources>
```

さらに、UWP では、アプリの既定のカルチャが .NET 標準ライブラリで明示的に定義されている必要があります。 これには、設定して、`NeutralResourcesLanguage`属性`AssemblyInfo.cs`、または既定のカルチャを別のクラス。

```csharp
using System.Resources;

[assembly: NeutralResourcesLanguage("en")]
```

右から左へのローカライズ言語と、デバイス上の領域を右から左への適切なロケールに変更することでテストできます。

## <a name="limitations"></a>制限事項

現在、Xamarin.Forms 右から左へのローカライズには、いくつかの制限があります。

- [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) ボタンの場所 ツールバー項目の場所、および遷移のアニメーションは、デバイスのロケールによって制御ではなく、 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティです。
- [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) スワイプ方向が反転しません。
- [`Image`](xref:Xamarin.Forms.Image) ビジュアル コンテンツが反転しません。
- [`DisplayAlert`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/) および[ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet/p/System.String/System.String/System.String/System.String[]/)方向は、デバイスのロケールによって制御ではなく、 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティです。
- [`WebView`](xref:Xamarin.Forms.WebView) コンテンツを考慮せず、 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティです。
- A`TextDirection`プロパティは、追加する、テキストの配置を制御する必要があります。

### <a name="ios"></a>iOS

- [`Stepper`](xref:Xamarin.Forms.Stepper) 印刷の向きは、デバイスのロケールによって制御ではなく、 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティです。
- [`EntryCell`](xref:Xamarin.Forms.EntryCell) テキストの配置は、デバイスのロケールによって制御ではなく、 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティです。
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) ジェスチャと位置合わせは逆になっていません。

### <a name="android"></a>Android

- [`SearchBar`](xref:Xamarin.Forms.SearchBar) 印刷の向きは、デバイスのロケールによって制御ではなく、 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティです。
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) 配置は、デバイスのロケールによって制御ではなく、 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティです。

### <a name="uwp"></a>UWP

- [`Editor`](xref:Xamarin.Forms.Editor) テキストの配置は、デバイスのロケールによって制御ではなく、 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティです。
- [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) プロパティがによって継承されない[ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage)子。
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) テキストの配置は、デバイスのロケールによって制御ではなく、 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティです。

## <a name="right-to-left-language-support-with-xamarinuniversity"></a>Xamarin.University 右左の言語からをサポート

> [!VIDEO https://youtube.com/embed/f2lQ5yw3iiU]

**右から左への Xamarin.Forms 3.0 サポートによって[Xamarin 大学](https://university.xamarin.com/)**

## <a name="related-links"></a>関連リンク

- [TodoLocalizedRTL サンプル アプリ](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalizedRTL/)
