---
title: "iOS での ScrollView コンテンツのタッチ" の説明: "プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、ScrollView がタッチジェスチャを処理するか、そのコンテンツに渡すかを制御する iOS プラットフォーム固有のを使用する方法について説明します。 "
ms. 製品: xamarin ms. assetid: 99F823DB379-40F0-A343-a9783c341120 ms. テクノロジ: xamarin-forms author: davidbritch ms. author: dabritch:: 10/24/2018 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="scrollview-content-touches-on-ios"></a>IOS での ScrollView コンテンツへの触れる

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

IOS のでタッチジェスチャが開始されると、暗黙的なタイマーがトリガーされ [`ScrollView`](xref:Xamarin.Forms.ScrollView) `ScrollView` ます。これは、タイマースパン内のユーザー操作に基づいて、ジェスチャを処理するか、そのコンテンツに渡すかどうかを決定します。 既定では、iOS はコンテンツにタッチしますが、これにより、 `ScrollView` コンテンツが `ScrollView` 必要になったときにジェスチャに優先されない状況が発生する可能性があります。 したがって、このプラットフォーム固有のは、がタッチジェスチャを処理するか、そのコンテンツに渡すかを制御し `ScrollView` ます。 添付プロパティを値に設定することにより、XAML で使用 `ScrollView.ShouldDelayContentTouches` され `boolean` ます。

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

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

scrollView.On<iOS>().SetShouldDelayContentTouches(false);
```

メソッドは、 `ScrollView.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 `ScrollView.SetShouldDelayContentTouches`名前空間のメソッドは、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) が [`ScrollView`](xref:Xamarin.Forms.ScrollView) タッチジェスチャを処理するか、そのコンテンツに渡すかを制御するために使用されます。 また、メソッドを `SetShouldDelayContentTouches` 使用して、コンテンツ `ShouldDelayContentTouches` のタッチが遅れているかどうかを返すメソッドを呼び出すことによって、コンテンツの遅延を切り替えることができます。

```csharp
scrollView.On<iOS>().SetShouldDelayContentTouches(!scrollView.On<iOS>().ShouldDelayContentTouches());
```

結果として、は、 [`ScrollView`](xref:Xamarin.Forms.ScrollView) コンテンツの受信遅延を無効にすることができます。このシナリオでは、はの [`Slider`](xref:Xamarin.Forms.Slider) ページではなくジェスチャを受け取り [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) ます。

[![](scrollview-content-touches-images/scrollview-delay-content-touches.png "ScrollView Delay Content Touches Platform-Specific")](scrollview-content-touches-images/scrollview-delay-content-touches-large.png#lightbox "ScrollView Delay Content Touches Platform-Specific")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
