---
title: IOS で ScrollView コンテンツの仕上げ
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、ScrollView がタッチ ジェスチャを処理するか、そのコンテンツに渡すかどうかを制御、iOS プラットフォームに固有の使用方法について説明します。
ms.prod: xamarin
ms.assetid: 99F823DB-B379-40F0-A343-A9783C341120
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 7f6ec13b1b21a1526bb53f260c4d80e881e7feba
ms.sourcegitcommit: 395774577f7524b57035c5cca3c9034a4b636489
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/10/2019
ms.locfileid: "54209777"
---
# <a name="scrollview-content-touches-on-ios"></a>IOS で ScrollView コンテンツの仕上げ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

iOSでは、[`ScrollView`](xref:Xamarin.Forms.ScrollView) 内でタッチジェスチャーが開始される時、暗黙のタイマーが呼ばれます。そして `ScrollView` は、そのタイマーの間のユーザーのアクションに基づいて、そのジェスチャーを処理するか、その中のコンテンツに渡すかを決めます。 既定では iOS の `ScrollView` はコンテンツのタッチを遅らせますが、これは `ScrollView` のコンテンツでジェスチャーが発生すべきときに発生しない、というようないくつかの状況での問題の原因となりえます。 したがって、このプラットフォーム仕様は `ScrollView` がタッチジェスチャーを処理するか、その中のコンテンツに渡すかどうかを制御します。 これは、 XAML で `ScrollView.ShouldDelayContentTouches` 添付プロパティを `boolean` 値に設定して使用します。

```xaml
<MasterDetailPage ...
                  xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <MasterDetailPage.Master>
        <ContentPage Title="Menu" BackgroundColor="Blue" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <ContentPage>
            <ScrollView x:Name="scrollView" ios:ScrollView.ShouldDelayContentTouches="false">
                <StackLayout Margin="0,20">
                    <Slider />
                    <Button Text="Toggle ScrollView DelayContentTouches" Clicked="OnButtonClicked" />
                </StackLayout>
            </ScrollView>
        </ContentPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>
```

代わりに、fluent API を使用して C# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

scrollView.On<iOS>().SetShouldDelayContentTouches(false);
```

`ScrollView.On<iOS>`メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。 `ScrollView.SetShouldDelayContentTouches` メソッドは、[`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 名前空間に存在し、 [`ScrollView`](xref:Xamarin.Forms.ScrollView)がタッチジェスチャーを処理するか、その中のコンテンツに渡すかどうかを制御するために使用されます。 さらに、 `SetShouldDelayContentTouches` メソッドは、コンテンツのタッチを遅らせるかどうかを返す `ShouldDelayContentTouches` メソッドを呼び出すことで、コンテンツのタッチの遅延を切り替えることにも使用できます。

```csharp
scrollView.On<iOS>().SetShouldDelayContentTouches(!scrollView.On<iOS>().ShouldDelayContentTouches());
```

その結果は、 [ `ScrollView` ](xref:Xamarin.Forms.ScrollView)はコンテンツのタッチを受けるときの遅延は無効にできます。そのためこのシナリオでは、 [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) の [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) ではなく[`Slider`](xref:Xamarin.Forms.Slider) がジェスチャーを受け取ります。

[![](scrollview-content-touches-images/scrollview-delay-content-touches.png "ScrollView 遅延コンテンツのプラットフォームに固有の接触")](scrollview-content-touches-images/scrollview-delay-content-touches-large.png#lightbox "ScrollView 遅延コンテンツのプラットフォームに固有の接触")

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
