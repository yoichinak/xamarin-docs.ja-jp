---
title: Windows 上の VisualElement アクセスキー
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、VisualElement のアクセスキーを指定する Windows プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 771AF785-76B8-4372-89F5-E4D521D21E0C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3cd4a9078a22c1f002cbc414490455c716ba884a
ms.sourcegitcommit: 342cfbd2502ad92cadada4fa9aec669b99d7830a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2020
ms.locfileid: "96604587"
---
# <a name="visualelement-access-keys-on-windows"></a>Windows 上の VisualElement アクセスキー

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

アクセスキーは、ユニバーサル Windows プラットフォーム (UWP) 上のアプリの使いやすさとアクセシビリティを向上させるキーボードショートカットです。タッチやマウスを使用する代わりに、ユーザーがキーボードを使用してアプリの表示可能な UI をすばやく移動したり操作したりできるようにします。 これらは、Alt キーと1つ以上の英数字キーの組み合わせであり、通常は順番に押されています。 キーボードショートカットは、1つの英数字を使用するアクセスキーに対して自動的にサポートされます。

アクセスキーのヒントは、アクセスキーを含むコントロールの横に表示されるフローティングバッジです。 各アクセスキーのヒントには、関連付けられたコントロールをアクティブにするための英数字のキーが含まれています。 ユーザーが Alt キーを押すと、アクセスキーのヒントが表示されます。

この UWP プラットフォーム固有のは、のアクセスキーを指定するために使用され [`VisualElement`](xref:Xamarin.Forms.VisualElement) ます。 XAML で使用されるのは、 [`VisualElement.AccessKey`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyProperty) 添付プロパティを英数字の値に設定し、必要に応じて添付プロパティを列挙値に設定し、添付プロパティをに、添付プロパティをに設定することによって [`VisualElement.AccessKeyPlacement`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyPlacementProperty) [`AccessKeyPlacement`](xref:Xamarin.Forms.AccessKeyPlacement) [`VisualElement.AccessKeyHorizontalOffset`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyHorizontalOffsetProperty) `double` [`VisualElement.AccessKeyVerticalOffset`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyVerticalOffsetProperty) `double` 行います。

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <ContentPage Title="Page 1"
                 windows:VisualElement.AccessKey="1">
        <StackLayout Margin="20">
            ...
            <Switch windows:VisualElement.AccessKey="A" />
            <Entry Placeholder="Enter text here"
                   windows:VisualElement.AccessKey="B" />
            ...
            <Button Text="Access key F, placement top with offsets"
                    Margin="20"
                    Clicked="OnButtonClicked"
                    windows:VisualElement.AccessKey="F"
                    windows:VisualElement.AccessKeyPlacement="Top"
                    windows:VisualElement.AccessKeyHorizontalOffset="20"
                    windows:VisualElement.AccessKeyVerticalOffset="20" />
            ...
        </StackLayout>
    </ContentPage>
    ...
</TabbedPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

var page = new ContentPage { Title = "Page 1" };
page.On<Windows>().SetAccessKey("1");

var switchView = new Switch();
switchView.On<Windows>().SetAccessKey("A");
var entry = new Entry { Placeholder = "Enter text here" };
entry.On<Windows>().SetAccessKey("B");
...

var button4 = new Button { Text = "Access key F, placement top with offsets", Margin = new Thickness(20) };
button4.Clicked += OnButtonClicked;
button4.On<Windows>()
    .SetAccessKey("F")
    .SetAccessKeyPlacement(AccessKeyPlacement.Top)
    .SetAccessKeyHorizontalOffset(20)
    .SetAccessKeyVerticalOffset(20);
...
```

`VisualElement.On<Windows>`メソッドは、このプラットフォーム固有のがユニバーサル Windows プラットフォームでのみ実行されることを指定します。 [ `VisualElement.SetAccessKey` ] (Xref: Xamarin.FormsPlatformConfiguration. WindowsSpecific VisualElement. SetAccessKey ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Windows、 Xamarin.Forms 。VisualElement}, System.string) メソッドを名前空間で使用して、の [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) アクセスキーの値を設定し `VisualElement` ます。 [ `VisualElement.SetAccessKeyPlacement` ] (Xref: Xamarin.FormsPlatformConfiguration. WindowsSpecific. SetAccessKeyPlacement ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Windows、 Xamarin.Forms 。VisualElement}、 Xamarin.Forms 。AccessKeyPlacement) メソッド。必要に応じて、 [`AccessKeyPlacement`](xref:Xamarin.Forms.AccessKeyPlacement) 次の有効な値を提供する列挙体を使用して、アクセスキーのヒントを表示する位置を指定します。

- [`Auto`](xref:Xamarin.Forms.AccessKeyPlacement.Auto) –アクセスキーのヒントの配置がオペレーティングシステムによって決定されることを示します。
- [`Top`](xref:Xamarin.Forms.AccessKeyPlacement.Top) –アクセスキーのヒントがの上端の上に表示されることを示し `VisualElement` ます。
- [`Bottom`](xref:Xamarin.Forms.AccessKeyPlacement.Bottom) –アクセスキーのヒントがの下端の下に表示されることを示し `VisualElement` ます。
- [`Right`](xref:Xamarin.Forms.AccessKeyPlacement.Right) –アクセスキーのヒントがの右端の右側に表示されることを示し `VisualElement` ます。
- [`Left`](xref:Xamarin.Forms.AccessKeyPlacement.Left) –アクセスキーのヒントがの左端の左側に表示されることを示し `VisualElement` ます。
- [`Center`](xref:Xamarin.Forms.AccessKeyPlacement.Center) –アクセスキーのヒントがの中央に表示されることを示し `VisualElement` ます。

> [!NOTE]
> 通常、 [`Auto`](xref:Xamarin.Forms.AccessKeyPlacement.Auto) キーヒントの配置は十分であり、アダプティブユーザーインターフェイスがサポートされています。

[ `VisualElement.SetAccessKeyHorizontalOffset` ] (Xref: Xamarin.FormsPlatformConfiguration. WindowsSpecific Xamarin.Forms ()。IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Windows、 Xamarin.Forms 。VisualElement}、system.string)、および [ `VisualElement.SetAccessKeyVerticalOffset` ] (xref: Xamarin.Forms 。PlatformConfiguration. WindowsSpecific Xamarin.Forms ()。IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Windows、 Xamarin.Forms 。VisualElement}, system.string) メソッドを使用すると、アクセスキーヒントの場所をより細かく制御できます。 メソッドの引数は、 `SetAccessKeyHorizontalOffset` アクセスキーヒントを左または右に移動する距離を示します。メソッドの引数は、 `SetAccessKeyVerticalOffset` アクセスキーのヒントを上下に移動する距離を示します。

>[!NOTE]
> アクセスキーの位置が設定されている場合、アクセスキーのオフセットを設定することはできません `Auto` 。

また、[ `GetAccessKey` ] (xref: Xamarin.FormsPlatformConfiguration. WindowsSpecific. VisualElement ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Windows、 Xamarin.Forms 。VisualElement})、[ `GetAccessKeyPlacement` ] (xref: Xamarin.Forms 。PlatformConfiguration. WindowsSpecific. GetAccessKeyPlacement ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Windows、 Xamarin.Forms 。VisualElement})、[ `GetAccessKeyHorizontalOffset` ] (xref: Xamarin.Forms 。PlatformConfiguration. WindowsSpecific Xamarin.Forms ()。IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Windows、 Xamarin.Forms 。VisualElement})、および [ `GetAccessKeyVerticalOffset` ] (xref: Xamarin.Forms 。PlatformConfiguration. WindowsSpecific Xamarin.Forms ()。IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Windows、 Xamarin.Forms 。VisualElement})) メソッドを使用して、アクセスキーの値とその場所を取得できます。

結果として、Alt キーを押すと、アクセスキーを定義するインスタンスの横にアクセスキーのヒントが表示され [`VisualElement`](xref:Xamarin.Forms.VisualElement) ます。

![VisualElement アクセスキープラットフォーム固有](visualelement-access-keys-images/visualelement-accesskeys.png "VisualElement アクセスキープラットフォーム固有")

ユーザーがアクセスキーをアクティブにすると、Alt キーを押した後にアクセスキーを押すと、の既定のアクションが `VisualElement` 実行されます。 たとえば、ユーザーがでアクセスキーをアクティブ化すると、 [`Switch`](xref:Xamarin.Forms.Switch) `Switch` が切り替わります。 ユーザーがでアクセスキーをアクティブ化すると、に [`Entry`](xref:Xamarin.Forms.Entry) よって `Entry` フォーカスが得られます。 ユーザーがでアクセスキーをアクティブ化すると [`Button`](xref:Xamarin.Forms.Button) 、イベントのイベントハンドラー [`Clicked`](xref:Xamarin.Forms.Button.Clicked) が実行されます。

> [!WARNING]
> 既定では、アクセスキーはモーダルダイアログが表示されたときにアクティブにすることができ `DisplayAlert` ます。たとえば、メソッドとメソッドを使用 `DisplayPromptAsync` します。 ただし、このシナリオでは、アクセスキーを無効にするためにカスタムロジックを記述することができます。 これは、 `Dispatcher.AcceleratorKeyActivated` `MainPage` UWP プロジェクトのクラスでイベントを処理し、 `Handled` `true` モーダルダイアログが表示されたときにイベント引数のプロパティをに設定することによって実現できます。

アクセスキーの詳細については、「 [アクセスキー](/windows/uwp/design/input/access-keys)」を参照してください。

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
