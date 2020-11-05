---
title: Android でのソフトキーボード入力モード
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、ソフトキーボード入力領域の動作モードを設定する Android プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: AFCDC92F-F61E-42F6-904B-50B5C4949970
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e041c766d0a4beefb8b4c74b9024c7554c818511
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93368636"
---
# <a name="soft-keyboard-input-mode-on-android"></a>Android でのソフトキーボード入力モード

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この Android プラットフォーム固有のは、ソフトキーボード入力領域の動作モードを設定するために使用され、XAML では、 [`Application.WindowSoftInputModeAdjust`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.WindowSoftInputModeAdjustProperty) 添付プロパティを列挙体の値に設定することによって使用され [`WindowSoftInputModeAdjust`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust) ます。

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize">
  ...
</Application>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

App.Current.On<Android>().UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize);
```

メソッドは、 `Application.On<Android>` このプラットフォーム固有のが Android でのみ実行されることを指定します。 [ `Application.UseWindowSoftInputModeAdjust` ] (Xref: Xamarin.FormsPlatformConfiguration. AndroidSpecific Xamarin.Forms ()IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Android、 Xamarin.Forms 。アプリケーション}、 Xamarin.Forms 。PlatformConfiguration. AndroidSpecific) メソッドを名前空間で使用して、 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) ソフトキーボード入力領域の動作モードを設定します。 [`WindowSoftInputModeAdjust`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust) 列挙体の値は [`Pan`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Pan) と [`Resize`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize) です。 この `Pan` 値は、 [`AdjustPan`](xref:Android.Views.SoftInput.AdjustPan) 入力コントロールにフォーカスがあるときにウィンドウのサイズを変更しない調整オプションを使用します。 代わりに、ウィンドウの内容はパンされるので、現在のフォーカスはソフトキーボードによって隠されません。 この `Resize` 値は、 [`AdjustResize`](xref:Android.Views.SoftInput.AdjustResize) ソフトキーボード用の領域を確保するために、入力コントロールにフォーカスがあるときにウィンドウのサイズを変更する調整オプションを使用します。

結果として、入力コントロールにフォーカスがあるときに、ソフトキーボード入力領域の動作モードを設定できます。

[![ソフトキーボード動作モードプラットフォーム固有](soft-keyboard-input-mode-images/pan-resize.png)](soft-keyboard-input-mode-images/pan-resize-large.png#lightbox "ソフトキーボード動作モード Platform-Specific")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific の AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)