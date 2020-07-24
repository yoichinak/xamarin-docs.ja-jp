---
title: Android でのページライフサイクルイベント
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、アプリケーションの一時停止と再開におけるページイベントの消失と表示を無効にする、Android プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: F6E3759C-D347-407A-91A2-CF9B3B7D4CBD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: fb4d1e28fded70005ef23eb4f7540eccd2fba372
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939309"
---
# <a name="page-lifecycle-events-on-android"></a>Android でのページライフサイクルイベント

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この Android プラットフォーム固有のは、 [`Disappearing`](xref:Xamarin.Forms.Page.Appearing) [`Appearing`](xref:Xamarin.Forms.Page.Appearing) AppCompat を使用するアプリケーションについて、アプリケーションの一時停止と再開のイベントをそれぞれ無効にするために使用されます。 さらに、ソフトキーボードの動作モードがに設定されている場合、一時停止時にソフトキーボードを表示するかどうかを制御する機能も含まれてい [`WindowSoftInputModeAdjust.Resize`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize) ます。

> [!NOTE]
> イベントに依存するアプリケーションの既存の動作を維持するために、これらのイベントは既定で有効になっています。 これらのイベントを無効にすると、AppCompat イベントサイクルが、AppCompat イベントサイクルに一致します。

このプラットフォーム固有のは [`Application.SendDisappearingEventOnPause`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPauseProperty) 、、 [`Application.SendAppearingEventOnResume`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResumeProperty) 、およびの各 [`Application.ShouldPreserveKeyboardOnResume`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResumeProperty) 添付プロパティを値に設定することにより、XAML で使用でき `boolean` ます。

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"             xmlns:androidAppCompat="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize"
             androidAppCompat:Application.SendDisappearingEventOnPause="false"
             androidAppCompat:Application.SendAppearingEventOnResume="false"
             androidAppCompat:Application.ShouldPreserveKeyboardOnResume="true">
  ...
</Application>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;
...

Xamarin.Forms.Application.Current.On<Android>()
     .UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize)
     .SendDisappearingEventOnPause(false)
     .SendAppearingEventOnResume(false)
     .ShouldPreserveKeyboardOnResume(true);
```

メソッドは、 `Application.Current.On<Android>` このプラットフォーム固有のが Android でのみ実行されることを指定します。 [ `Application.SendDisappearingEventOnPause` ] (Xref: Xamarin.FormsPlatformConfiguration. Androidingeventonpause Xamarin.Forms () を指定します。IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Android、 Xamarin.Forms 。Application}, system.string) メソッドを名前空間で使用して、 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) [`Disappearing`](xref:Xamarin.Forms.Page.Appearing) アプリケーションがバックグラウンドに入ったときのページイベントの発生を有効または無効にします。 [ `Application.SendAppearingEventOnResume` ] (Xref: Xamarin.FormsPlatformConfiguration. AndroidSpecific のアプリケーションの Xamarin.Forms 構成 () です。IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Android、 Xamarin.Forms 。Application}, system.string) メソッドを使用して、 [`Appearing`](xref:Xamarin.Forms.Page.Appearing) バックグラウンドからアプリケーションが再開されたときのページイベントの発生を有効または無効にします。 [ `Application.ShouldPreserveKeyboardOnResume` ] (Xref: Xamarin.FormsPlatformConfiguration. ShouldPreserveKeyboardOnResume (を指定します。 Xamarin.FormsIPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Android、 Xamarin.Forms 。Application}, system.string) メソッドを使用して、ソフトキーボードの動作モードがに設定されている場合に、休止時にソフトキーボードを表示するかどうかを制御し [`WindowSoftInputModeAdjust.Resize`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize) ます。

結果として、 [`Disappearing`](xref:Xamarin.Forms.Page.Appearing) との [`Appearing`](xref:Xamarin.Forms.Page.Appearing) ページイベントは、それぞれアプリケーションの一時停止と再開時には起動されず、アプリケーションが一時停止したときにソフトキーボードが表示された場合は、アプリケーションの再開時にも表示されます。

[![ライフサイクルイベントプラットフォーム固有](page-lifecycle-events-images/keyboard-on-resume.png)](page-lifecycle-events-images/keyboard-on-resume-large.png#lightbox "ライフサイクルイベントプラットフォーム固有")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific の AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
