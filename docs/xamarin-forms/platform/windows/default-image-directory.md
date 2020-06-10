---
title: "Windows 上の既定のイメージディレクトリ" 説明: "プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装しなくても、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、イメージアセットの読み込み元となるプロジェクトのディレクトリを定義する Windows プラットフォーム固有のを使用する方法について説明します。
ms. 製品: xamarin ms. assetid: 537A032B-74DD-4D43-864E-7D7113286D0D: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 01/16/2020 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="default-image-directory-on-windows"></a>Windows 上の既定のイメージディレクトリ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

プラットフォーム固有のユニバーサル Windows プラットフォームは、イメージアセットの読み込み元となるプロジェクト内のディレクトリを定義します。 を XAML で使用するには、 `Application.ImageDirectory` `string` イメージアセットを含むプロジェクトディレクトリを表すをに設定します。

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
             ...
             windows:Application.ImageDirectory="Assets">
    ...
</Application>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...
Application.Current.On<Windows>().SetImageDirectory("Assets");
```

`Application.On<Windows>`メソッドは、このプラットフォーム固有のがユニバーサル Windows プラットフォームでのみ実行されることを指定します。 `Application.SetImageDirectory`名前空間のメソッドは、 [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) イメージの読み込み元のプロジェクトディレクトリを指定するために使用されます。 また、メソッドを使用して、 `GetImageDirectory` `string` アプリケーションイメージアセットを含むプロジェクトディレクトリを表すを返すこともできます。

その結果、アプリケーションで使用されるすべてのイメージが、指定されたプロジェクトディレクトリから読み込まれます。

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
