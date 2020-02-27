---
title: Windows 上の既定のイメージディレクトリ
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、イメージアセットの読み込み元となるプロジェクト内のディレクトリを定義する Windows プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 537A032B-74DD-4D43-864E-7D7113286D0D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/16/2020
ms.openlocfilehash: 52197b980726936f4368ef1e4507ea671c9e70b1
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77646660"
---
# <a name="default-image-directory-on-windows"></a>Windows 上の既定のイメージディレクトリ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

プラットフォーム固有のユニバーサル Windows プラットフォームは、イメージアセットの読み込み元となるプロジェクト内のディレクトリを定義します。 XAML で使用されるのは、イメージアセットを含むプロジェクトディレクトリを表す `string` に `Application.ImageDirectory` を設定することです。

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
             ...
             windows:Application.ImageDirectory="Assets">
    ...
</Application>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...
Application.Current.On<Windows>().SetImageDirectory("Assets");
```

`Application.On<Windows>` メソッドは、このプラットフォーム固有のがユニバーサル Windows プラットフォーム上でのみ実行されることを指定します。 [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)名前空間の `Application.SetImageDirectory` メソッドは、イメージの読み込み元のプロジェクトディレクトリを指定するために使用されます。 さらに、`GetImageDirectory` メソッドを使用して、アプリケーションイメージ資産を含むプロジェクトディレクトリを表す `string` を返すことができます。

その結果、アプリケーションで使用されるすべてのイメージが、指定されたプロジェクトディレクトリから読み込まれます。

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
