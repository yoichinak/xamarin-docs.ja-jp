---
title: Windows 上の既定のイメージディレクトリ
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、イメージアセットの読み込み元となるプロジェクト内のディレクトリを定義する Windows プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 537A032B-74DD-4D43-864E-7D7113286D0D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4dd8655d4cd84f7d4214801e55677706eb67efa7
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93374681"
---
# <a name="default-image-directory-on-windows"></a>Windows 上の既定のイメージディレクトリ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

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

- [PlatformSpecifics (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)