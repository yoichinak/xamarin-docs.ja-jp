---
title: 右から左へのローカライズ
description: 右から左へのローカライズでは、Xamarin.Forms アプリケーションに、右から左へのフロー方向のサポートが追加されます。
ms.prod: xamarin
ms.assetid: 90E0CB16-C42A-4CC8-A70E-0C2CFB64A429
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 11/05/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5d06b0467a5029fec6d0c92a683114b9e0d04685
ms.sourcegitcommit: 044e8d7e2e53f366942afe5084316198925f4b03
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/06/2021
ms.locfileid: "97940188"
---
# <a name="right-to-left-localization"></a>右から左へのローカライズ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/todolocalizedrtl)

_右から左へのローカライズでは、Xamarin.Forms アプリケーションに、右から左へのフロー方向のサポートが追加されます。_

> [!NOTE]
> 右から左へのローカライズには、iOS 9 以上、および Android の API 17 以上を使用する必要があります。

フロー方向とは、ページ上の UI 要素を視覚でスキャンしていく方向のことです。 アラビア語やヘブライ語などの一部の言語では、UI 要素が右から左へのフロー方向で配置される必要があります。 これは、[`VisualElement.FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) プロパティを設定することで実現できます。 このプロパティでは、レイアウトを制御する任意の親要素内での UI 要素のフロー方向が取得または設定され、次のいずれかの [`FlowDirection`](xref:Xamarin.Forms.FlowDirection) 列挙値に設定される必要があります。

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

要素の [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) プロパティを [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft) に設定すると、一般的に配置が右に、読む順が右から左に、コントロールのレイアウトが右から左に設定されます。

[![右から左へのフロー方向を持つアラビア語の TodoItemPage](rtl-images/TodoItemPage-Arabic.png "右から左へのフロー方向を持つアラビア語の TodoItemPage")](rtl-images/TodoItemPage-Arabic-Large.png#lightbox "右から左へのフロー方向を持つアラビア語の TodoItemPage")

> [!TIP]
> 初期レイアウトでは [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) プロパティのみを設定する必要があります。 実行時にこの値を変更すると、パフォーマンスに影響を与える負荷の高いレイアウト プロセスが発生します。

親のない要素の [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) プロパティの既定値は [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight) ですが、親のある要素の `FlowDirection` の既定値は [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent) になります。 そのため、要素ではビジュアル ツリー内の親から `FlowDirection` プロパティ値を継承し、任意の要素でその親から取得した値をオーバーライドできます。

> [!TIP]
> 右から左方向の言語用にアプリをローカライズする場合、ページ上またはルート レイアウトで [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) プロパティを設定します。 これにより、そのフロー方向に適切に対応して、ページ (ルート レイアウト) 内に含まれるすべての要素が設定されます。

## <a name="respecting-device-flow-direction"></a>デバイスのフロー方向を優先する

選択した言語とリージョンに基づいてデバイスのフロー方向を優先するには、開発者が明示的に選択する必要があり、自動的に行われることはありません。 これは、ページ (ルート レイアウト) の [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) プロパティを `static` [`Device.FlowDirection`](xref:Xamarin.Forms.Device.FlowDirection) 値に設定することで実現できます。

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

```csharp
this.FlowDirection = Device.FlowDirection;
```

その後、既定では、ページ (ルート レイアウト) の子要素はすべて、[`Device.FlowDirection`](xref:Xamarin.Forms.Device.FlowDirection) 値を継承します。

## <a name="platform-setup"></a>プラットフォームの設定

特定のプラットフォームの設定では、右から左のロケールを有効にする必要があります。

### <a name="ios"></a>iOS

**Info.plist** の `CFBundleLocalizations` キー用の配列項目に対してサポートされる言語として、必要な右から左のロケールを追加する必要があります。 次の例では、`CFBundleLocalizations` キーの配列に追加されているアラビア語を示しています。

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>ar</string>
</array>
```

![Info.plist のサポートされる言語](rtl-images/ios-locales.png "Info.plist のサポートされる言語")

詳細については、「[iOS でのローカライズの基本事項](../../../ios/app-fundamentals/localization/index.md#localization-basics-in-ios)」を参照してください。

右から左へのローカライズは、デバイスまたはシミュレーター上の言語とリージョンを **Info.plist** で指定された右から左のロケールに変更することによって、テストできます。

> [!WARNING]
> iOS で言語とリージョンを右から左のロケールに変更するときに、そのロケールに必要なリソースを含めていない場合、任意の [`DatePicker`](xref:Xamarin.Forms.DatePicker) ビューで例外がスローされることに注意してください。 たとえば、`DatePicker` を含むアラビア語のアプリをテストする場合、 **[iOS ビルド]** ウィンドウの **[国際化]** セクションに **[mideast]** が選択されていることを確認します。

### <a name="android"></a>Android

アプリの **AndroidManifest.xml** ファイルは更新されているため、`<uses-sdk>` ノードで `android:minSdkVersion` 属性は 17 に設定され、`<application>` ノードで `android:supportsRtl` 属性は `true` に設定されます。

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest ... >
    <uses-sdk android:minSdkVersion="17" ... />
    <application ... android:supportsRtl="true">
    </application>
</manifest>
```

その後、デバイスまたはエミュレーターで右から左方向の言語を使用するように変更するか、または **[設定] > [開発者向けオプション]** で **[Force RTL layout direction]\(RTL レイアウト方向を使用\)** をオンにすることで、右から左へのローカライズをテストできます。

### <a name="universal-windows-platform-uwp"></a>ユニバーサル Windows プラットフォーム (UWP)

必要な言語リソースは、**Package.appxmanifest** ファイルの `<Resources>` ノードで指定する必要があります。 次の例では、`<Resources>` ノードに追加されているアラビア語を示します。

```xml
<Resources>
    <Resource Language="x-generate"/>
    <Resource Language="en" />
    <Resource Language="ar" />
</Resources>
```

さらに、UWP では、アプリの既定のカルチャが明示的に .NET Standard ライブラリで定義されている必要があります。 これは、`AssemblyInfo.cs` (別のクラス) の `NeutralResourcesLanguage` 属性を既定のカルチャに設定することで実現できます。

```csharp
using System.Resources;

[assembly: NeutralResourcesLanguage("en")]
```

右から左へのローカライズは、デバイス上の言語とリージョンを適切な右から左のロケールに変更するこでテストできます。

## <a name="limitations"></a>制限事項

Xamarin.Forms での右から左へのローカライズには、現在いくつかの制限があります。

- [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) のボタンの場所、ツール バー項目の場所、および切り替えアニメーションは、[`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) プロパティではなく、デバイスのロケールによって制御されます。
- [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) のスワイプ方向は反転されません。
- [`Image`](xref:Xamarin.Forms.Image) のビジュアル コンテンツは反転されません。
- [`WebView`](xref:Xamarin.Forms.WebView) のコンテンツでは [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) プロパティを優先しません。
- テキストの配置を制御するには、`TextDirection` プロパティを追加する必要があります。

### <a name="ios"></a>iOS

- [`Stepper`](xref:Xamarin.Forms.Stepper) の向きは、[`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) プロパティではなく、デバイスのロケールによって制御されます。
- [`EntryCell`](xref:Xamarin.Forms.EntryCell) のテキストの配置は、[`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) プロパティではなく、デバイスのロケールによって制御されます。
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) のジェスチャと配置は反転されません。

### <a name="android"></a>Android

- [`SearchBar`](xref:Xamarin.Forms.SearchBar) の向きは、[`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) プロパティではなく、デバイスのロケールによって制御されます。
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) の配置は、[`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) プロパティではなく、デバイスのロケールによって制御されます。

### <a name="uwp"></a>UWP

- [`Editor`](xref:Xamarin.Forms.Editor) のテキストの配置は、[`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) プロパティではなく、デバイスのロケールによって制御されます。
- [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) プロパティは子の [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) に継承されません。
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) のテキストの配置は、[`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) プロパティではなく、デバイスのロケールによって制御されます。

## <a name="force-right-to-left-layout"></a>右から左へのレイアウトを強制する

Xamarin.iOS および Xamarin.Android アプリケーションには、それぞれのプラットフォーム プロジェクトを変更することにより、デバイスの設定に関係なく、常に右から左へのレイアウトを使用するように強制することができます。

### <a name="ios"></a>iOS

Xamarin.iOS アプリケーションには、次のように **AppDelegate** クラスを変更することにより、常に右から左へのレイアウトを使用するように強制することができます。

1. `IntPtr_objc_msgSend` 関数を `AppDelegate` クラスの最初の行として宣言します。

   ```csharp
   [System.Runtime.InteropServices.DllImport(ObjCRuntime.Constants.ObjectiveCLibrary, EntryPoint = "objc_msgSend")]
   internal extern static IntPtr IntPtr_objc_msgSend(IntPtr receiver, IntPtr selector, UISemanticContentAttribute arg1);
   ```

1. `FinshedLaunching` メソッドから戻る前に、`FinishedLaunching` メソッドから `IntPtr_objc_msgSend` 関数を呼び出します。

   ```csharp
   bool result = base.FinishedLaunching(app, options);

   ObjCRuntime.Selector selector = new ObjCRuntime.Selector("setSemanticContentAttribute:");
   IntPtr_objc_msgSend(UIView.Appearance.Handle, selector.Handle, UISemanticContentAttribute.ForceRightToLeft);

   return result;
   ```

この方法は、常に右から左へのレイアウトを必要とするアプリケーションの場合に役立ち、[`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) プロパティを設定する必要がなくなります。

`IntrPtr_objc_msgSend` メソッドの詳細については、「[Xamarin. iOS の Objective-C セレクター](~/ios/internals/objective-c-selectors.md)」を参照してください。

### <a name="android"></a>Android

Xamarin.Android アプリケーションには、次の行を含むように **MainActivity** クラスを変更することで、常に右から左へのレイアウトを使用するように強制することができます。

```csharp
Window.DecorView.LayoutDirection = LayoutDirection.Rtl;
```

> [!NOTE]
> この方法を使用するには、右から左へのレイアウトをサポートするようにアプリケーションをセットアップする必要があります。 詳細については、[Android プラットフォームのセットアップ](#android)に関するページを参照してください。

この方法は、常に右から左へのレイアウトを必要とするアプリケーションの場合に役立ち、大部分のコントロールに対して [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) プロパティを設定する必要がなくなります。 ただし、[`CollectionView`](xref:Xamarin.Forms.CollectionView) などの一部のコントロールには、`LayoutDirection` プロパティを考慮するのでなく、引き続き `FlowDirection` プロパティを設定する必要があります。

## <a name="right-to-left-language-support-with-xamarinuniversity"></a>Xamarin.University での右から左方向の言語のサポート

> [!VIDEO https://youtube.com/embed/f2lQ5yw3iiU]

**Xamarin.Forms 3.0 での右から左方向へのサポートに関するビデオ**

## <a name="related-links"></a>関連リンク

- [TodoLocalizedRTL サンプル アプリ](/samples/xamarin/xamarin-forms-samples/todolocalizedrtl)
