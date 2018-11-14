---
title: 右から左のローカライズ
description: 右から左のローカリゼーションでは、Xamarin.Forms アプリケーションを右から左方向のサポートを追加します。
ms.prod: xamarin
ms.assetid: 90E0CB16-C42A-4CC8-A70E-0C2CFB64A429
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 67b0d90290b18c7a5b55c5e3496b54970a8cfc38
ms.sourcegitcommit: 6be6374664cd96a7d924c2e0c37aeec4adf8be13
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/13/2018
ms.locfileid: "51617606"
---
# <a name="right-to-left-localization"></a>右から左のローカライズ

_右から左のローカリゼーションでは、Xamarin.Forms アプリケーションを右から左方向のサポートを追加します。_

> [!NOTE]
> 右から左のローカライズには、iOS 9 以降および Android で 17 以上の API の使用が必要です。

フローの方向とは、目で、ページの UI 要素をスキャンする方向です。 アラビア語やヘブライ語など、一部の言語では、UI 要素が右から左にフローの方向にレイアウトされることが必要です。 これは、設定で実現できる、 [ `VisualElement.FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティ。 このプロパティを取得または、レイアウトを制御しのいずれかに設定する必要がある親要素内でどの UI 要素のフローの方向に設定、 [ `FlowDirection` ](xref:Xamarin.Forms.FlowDirection)列挙値。

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

設定、 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティを[ `RightToLeft` ](xref:Xamarin.Forms.FlowDirection.RightToLeft)要素からフローを右、右から左への読み取り順序、およびコントロールのレイアウトを通常の配置を設定右から左。

[![右から左方向にアラビア語 TodoItemPage](rtl-images/TodoItemPage-Arabic.png "右から左方向にアラビア語 TodoItemPage")](rtl-images/TodoItemPage-Arabic-Large.png#lightbox "TodoItemPage 右から左方向にアラビア語")

> [!TIP]
> のみを設定する必要があります、 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)初期レイアウトのプロパティ。 実行時にこの値を変更すると、パフォーマンスに影響を与える負荷の高いレイアウト プロセス。

既定の[ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)親のない要素のプロパティの値が[ `LeftToRight` ](xref:Xamarin.Forms.FlowDirection.LeftToRight)、既定値の中に`FlowDirection`親を持つ要素が[ `MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent). したがって、要素の継承、`FlowDirection`ビジュアル ツリーと任意の要素内の親からプロパティ値がその親から取得した値を上書きできます。

> [!TIP]
> 右から左方向言語用のアプリをローカライズする場合は、設定、 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)ページまたはルート レイアウトのプロパティ。 これにより、すべてのページ、またはフローの方向に適切に応答のルート レイアウト内に含まれる要素。

## <a name="respecting-device-flow-direction"></a>デバイスのフロー方向を優先

選択した言語に基づくデバイスのフロー方向を考慮し、領域が、明示的な開発者の選択肢と、自動的に行われません。 設定で行うことができます、 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) ページまたはルート レイアウト プロパティを`static` [ `Device.FlowDirection` ](xref:Xamarin.Forms.Device.FlowDirection)値。

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

```csharp
this.FlowDirection = Device.FlowDirection;
```

ページ、またはルート レイアウトでは、すべての子要素は既定では、継承、 [ `Device.FlowDirection` ](xref:Xamarin.Forms.Device.FlowDirection)値。

## <a name="platform-setup"></a>プラットフォームのセットアップ

右から左のロケールを有効にするには、特定のプラットフォームの設定が必要です。

### <a name="ios"></a>iOS

配列の項目を必要な右から左のロケールをサポートされている言語として追加する、`CFBundleLocalizations`キー **Info.plist**します。 次の例では、アラビア語の配列に追加されて、`CFBundleLocalizations`キー。

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>ar</string>
</array>
```

![Info.plist がサポートされている言語](rtl-images/ios-locales.png "Info.plist でサポートされる言語")

詳細については、次を参照してください。 [iOS のローカリゼーション基本](https://docs.microsoft.com/xamarin/ios/app-fundamentals/localization/#localization-basics-in-ios)します。

右から左のローカリゼーションで指定されたロケールが右から左に、言語と、デバイスまたはシミュレーター上の領域を変更することでテストできます**Info.plist**します。

> [!WARNING]
> Ios では、ロケールが右から左に任意の言語と地域を変更する場合に注意してください[ `DatePicker` ](xref:Xamarin.Forms.DatePicker)ビュー、ロケールに必要なリソースが含まれていない場合、例外がスローされます。 たとえば、アラビア語を持つアプリをテストするときに、 `DatePicker`、いることを確認**mideast**でが選択されている、**国際化**のセクション、 **iOS ビルド**ウィンドウ。

### <a name="android"></a>Android

アプリの**AndroidManifest.xml**ファイルを更新する必要があるように、`<uses-sdk>`ノード セット、 `android:minSdkVersion` 17、属性、`<application>`ノード セット、`android:supportsRtl`属性を`true`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest ... >
    <uses-sdk android:minSdkVersion="17" ... />
    <application ... android:supportsRtl="true">
    </application>
</manifest>
```

デバイス/エミュレーターを使用して、右から左の言語を変更するか、有効にすると、右から左のローカライズをテストすることができますし、 **Force RTL レイアウトの方向**で**設定 > 開発者向けオプション**します。

### <a name="universal-windows-platform-uwp"></a>ユニバーサル Windows プラットフォーム (UWP)

必要な言語リソースを指定する必要があります、`<Resources>`のノード、 **Package.appxmanifest**ファイル。 次の例では、アラビア語に追加されたことを示しています、`<Resources>`ノード。

```xml
<Resources>
    <Resource Language="x-generate"/>
    <Resource Language="en" />
    <Resource Language="ar" />
</Resources>
```

さらに、UWP では、アプリの既定のカルチャが .NET Standard ライブラリに明示的に定義されている必要があります。 これを設定して実行できます、`NeutralResourcesLanguage`属性`AssemblyInfo.cs`、または既定のカルチャに別のクラス。

```csharp
using System.Resources;

[assembly: NeutralResourcesLanguage("en")]
```

右から左のローカライズは、言語およびデバイス上の領域を右から左の適切なロケールに変更することでテストできます。

## <a name="limitations"></a>制限事項

現在、Xamarin.Forms 右から左のローカライズには、いくつかの制限があります。

- [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) ボタンの場所 ツールバーの項目の場所と遷移のアニメーションがデバイスのロケールによって制御されるのではなく[ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティ。
- [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) スワイプ方向が反転しません。
- [`Image`](xref:Xamarin.Forms.Image) ビジュアル コンテンツが反転しません。
- [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[]))向きがデバイスのロケールによって制御されるのではなく[ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティ。
- [`WebView`](xref:Xamarin.Forms.WebView) コンテンツを優先しません、 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティ。
- A`TextDirection`プロパティは、追加する、テキストの配置を制御する必要があります。

### <a name="ios"></a>iOS

- [`Stepper`](xref:Xamarin.Forms.Stepper) 印刷の向きがデバイスのロケールによって制御されるのではなく[ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティ。
- [`EntryCell`](xref:Xamarin.Forms.EntryCell) テキストの配置は、デバイスのロケールによって制御ではなく、 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティ。
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) ジェスチャと配置は反転されません。

### <a name="android"></a>Android

- [`SearchBar`](xref:Xamarin.Forms.SearchBar) 印刷の向きがデバイスのロケールによって制御されるのではなく[ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティ。
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) デバイスのロケールで位置を制御ではなく、 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティ。

### <a name="uwp"></a>UWP

- [`Editor`](xref:Xamarin.Forms.Editor) テキストの配置は、デバイスのロケールによって制御ではなく、 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティ。
- [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) によってプロパティが継承されない[ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage)子。
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) テキストの配置は、デバイスのロケールによって制御ではなく、 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティ。

## <a name="right-to-left-language-support-with-xamarinuniversity"></a>Xamarin.University 右左の言語からをサポート

> [!VIDEO https://youtube.com/embed/f2lQ5yw3iiU]

**Xamarin.Forms 3.0 右から左へサポートによって[Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>関連リンク

- [TodoLocalizedRTL サンプル アプリ](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalizedRTL/)
