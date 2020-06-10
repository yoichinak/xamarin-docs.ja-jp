---
title: "Windows 上の SearchBar Spell Check" 説明: "プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、SearchBar がスペルチェックエンジンと対話できるようにするための Windows プラットフォーム固有のを使用する方法について説明します。
ms: xamarin ms. assetid:79 74C91F-75 0247 DB3-b0edavidbritch c45e25 ミリ秒。テクノロジ: xamarin-forms author: ms. author: dabritch ms. date: 10/24/2018 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="searchbar-spell-check-on-windows"></a>Windows 上の SearchBar Spell Check

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

これユニバーサル Windows プラットフォームプラットフォーム固有であるため、は [`SearchBar`](xref:Xamarin.Forms.SearchBar) スペルチェックエンジンと対話できます。 添付プロパティを値に設定することにより、XAML で使用 [`SearchBar.IsSpellCheckEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.IsSpellCheckEnabledProperty) され `boolean` ます。

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SearchBar ... windows:SearchBar.IsSpellCheckEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

searchBar.On<Windows>().SetIsSpellCheckEnabled(true);
```

`SearchBar.On<Windows>`メソッドは、このプラットフォーム固有のがユニバーサル Windows プラットフォームでのみ実行されることを指定します。 [ `SearchBar.SetIsSpellCheckEnabled` ] (Xref: Xamarin.FormsPlatformConfiguration. SetIsSpellCheckEnabled (を指定します。 Xamarin.FormsIPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Windows、 Xamarin.Forms 。SearchBar}, Boolean)) メソッドを名前空間で、 [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) スペルチェックをオンまたはオフにします。 また、メソッドを `SearchBar.SetIsSpellCheckEnabled` 使用して、[ `SearchBar.GetIsSpellCheckEnabled` ] (xref: を呼び出してスペルチェックを切り替えることもできます Xamarin.Forms 。PlatformConfiguration. GetIsSpellCheckEnabled (を指定します。 Xamarin.FormsIPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Windows、 Xamarin.Forms 。SearchBar})): スペルチェックが有効かどうかを返すメソッド。

```csharp
searchBar.On<Windows>().SetIsSpellCheckEnabled(!searchBar.On<Windows>().GetIsSpellCheckEnabled());
```

結果として、に入力されたテキストは [`SearchBar`](xref:Xamarin.Forms.SearchBar) スペルチェックが可能で、ユーザーに正しくないスペルが示されます。

![SearchBar スペルチェックプラットフォーム固有](searchbar-spell-check-images/searchbar-spellcheck.png "SearchBar スペルチェックプラットフォーム固有")

> [!NOTE]
> 名前空間のクラスには、 `SearchBar` [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) [`EnableSpellCheck`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.EnableSpellCheck*) [`DisableSpellCheck`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.DisableSpellCheck*) それぞれのスペルチェックを有効または無効にするために使用できるメソッドとメソッドもあり `SearchBar` ます。

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
