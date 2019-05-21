---
title: Windows の VisualElement アクセス キー
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事を VisualElement をアクセス キーを指定します。 Windows プラットフォームに固有の使用方法について説明します。
ms.prod: xamarin
ms.assetid: 771AF785-76B8-4372-89F5-E4D521D21E0C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: c480f398c37ce43b634e0ec1c955b965466757f1
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/20/2019
ms.locfileid: "65926842"
---
# <a name="visualelement-access-keys-on-windows"></a>Windows の VisualElement アクセス キー

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

アクセス キーは、キーボードの代わりにタッチを使用して、アプリの表示の UI を使用したにすばやく移動し、対話ユーザー向けの直感的な方法を提供することで、使いやすさとアプリ、ユニバーサル Windows プラットフォーム (UWP) でのアクセシビリティを向上させるためのキーボード ショートカットまたは、マウスを選択します。 これらは、Alt キーと 1 つまたは複数英数字キー、順番に押された通常の組み合わせです。 キーボード ショートカットは自動的に 1 つの英数字を使用して、アクセス キーのサポートします。

アクセス キーのヒントの縦棒は浮動バッジのアクセス キーを含むコントロールの横に表示されます。 各アクセス キーのヒントには、関連付けられているコントロールをアクティブ化する英数字キーが含まれています。 ユーザーは、Alt キーを押すと、アクセス キーのヒントが表示されます。

この UWP プラットフォームに固有の使用のアクセス キーを指定する、 [ `VisualElement`](xref:Xamarin.Forms.VisualElement)します。 XAML で設定して使用される、 [ `VisualElement.AccessKey` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyProperty)添付プロパティの英数字の値に設定し、必要に応じて、 [ `VisualElement.AccessKeyPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyPlacementProperty)添付プロパティを@property[ `AccessKeyPlacement` ](xref:Xamarin.Forms.AccessKeyPlacement)列挙型で、 [ `VisualElement.AccessKeyHorizontalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyHorizontalOffsetProperty)添付プロパティを`double`、および[ `VisualElement.AccessKeyVerticalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyVerticalOffsetProperty)添付プロパティを`double`:

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

代わりに、fluent API を使用して C# から使用できます。

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

`VisualElement.On<Windows>`メソッドは、このプラットフォームに固有はユニバーサル Windows プラットフォームでのみ実行されるを指定します。 [ `VisualElement.SetAccessKey` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKey(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.String))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)名前空間のアクセス キーの値を設定するため、`VisualElement`します。 [ `VisualElement.SetAccessKeyPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKeyPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},Xamarin.Forms.AccessKeyPlacement))メソッドでアクセス キーのヒントを表示するために使用する位置を指定、 [ `AccessKeyPlacement` ](xref:Xamarin.Forms.AccessKeyPlacement)次の値を提供する列挙体。

- [`Auto`](xref:Xamarin.Forms.AccessKeyPlacement.Auto) – アクセス キーのヒントの配置は、オペレーティング システムによって決定されることを示します。
- [`Top`](xref:Xamarin.Forms.AccessKeyPlacement.Top) – より上のアクセス キーのヒントが表示されることを示します、`VisualElement`します。
- [`Bottom`](xref:Xamarin.Forms.AccessKeyPlacement.Bottom) – アクセス キーのヒントが下のエッジの下に表示されることを示します、`VisualElement`します。
- [`Right`](xref:Xamarin.Forms.AccessKeyPlacement.Right) – アクセス キーのヒントの右端の右側に表示されることを示します、`VisualElement`します。
- [`Left`](xref:Xamarin.Forms.AccessKeyPlacement.Left) – の左エッジから左へのアクセス キーのヒントが表示されることを示します、`VisualElement`します。
- [`Center`](xref:Xamarin.Forms.AccessKeyPlacement.Center) – の中心をアクセス キーのヒントを重ねて表示はことを示します、`VisualElement`します。

> [!NOTE]
> 通常、 [ `Auto` ](xref:Xamarin.Forms.AccessKeyPlacement.Auto)キー ヒントの配置が不十分で、適応ユーザー インターフェイスのサポートが含まれています。

[ `VisualElement.SetAccessKeyHorizontalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKeyHorizontalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Double))と[ `VisualElement.SetAccessKeyVerticalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKeyVerticalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Double))アクセス キーのヒントの場所のより詳細なコントロールには、メソッドを使用することができます。 引数、`SetAccessKeyHorizontalOffset`メソッドは、アクセス キー ヒント左に移動する距離または右側に、方法と引数を示します、`SetAccessKeyVerticalOffset`メソッドは、アクセス キーのヒントを上下に移動するまでの方法を示します。

>[!NOTE]
> アクセス キーの配置が設定されている場合は、アクセス キーのヒントのオフセットを設定することはできません`Auto`します。

さらに、 [ `GetAccessKey` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKey(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement}))、 [ `GetAccessKeyPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKeyPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement}))、 [ `GetAccessKeyHorizontalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKeyHorizontalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement}))、および[ `GetAccessKeyVerticalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKeyVerticalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement}))メソッドを使用することができますアクセスを取得するキーの値とその場所。

結果は、アクセス キーのヒントは、いずれかの横に表示できる[ `VisualElement` ](xref:Xamarin.Forms.VisualElement) Alt キーを押してアクセス キーを定義するインスタンス。

![VisualElement アクセス プラットフォームに固有のキー](visualelement-access-keys-images/visualelement-accesskeys.png "VisualElement アクセス プラットフォームに固有のキー")

ユーザー アクセス後に Alt キーを押してアクセス キーをアクティブにしたときのキーの既定のアクション、`VisualElement`が実行されます。 などの場合、ユーザーがアクティブにアクセス キーで、 [ `Switch` ](xref:Xamarin.Forms.Switch)、`Switch`が切り替えられました。 ユーザーがアクセス キーをアクティブ化のときに、 [ `Entry` ](xref:Xamarin.Forms.Entry)、`Entry`がフォーカスを取得します。 ユーザーがアクセス キーをアクティブ化のときに、 [ `Button`](xref:Xamarin.Forms.Button)のイベント ハンドラー、 [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked)イベントを実行します。

アクセス キーの詳細については、[アクセス キー](/windows/uwp/design/input/access-keys#key-tip-positioning)を参照してください。

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
