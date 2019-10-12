---
title: タブレットアプリとデスクトップアプリのレイアウト
description: この記事では、スマートフォンではなく、タブレット用に Xamarin のフォームアプリケーションレイアウトを最適化する方法について説明します。
ms.prod: xamarin
ms.assetid: D62F472B-4345-4983-8403-659A538B591F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/01/2016
ms.openlocfilehash: f91d0127d0f2ffe37e3e0ff016dee551a679ad84
ms.sourcegitcommit: e354aabfb39598e0ce11115db3e6bcebb9f68338
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/11/2019
ms.locfileid: "72273115"
---
# <a name="layout-for-tablet-and-desktop-apps"></a>タブレットアプリとデスクトップアプリのレイアウト

Xamarin. Forms はサポートされているプラットフォームで使用できるすべてのデバイスの種類をサポートしています。そのため、スマートフォンだけでなく、以下でもアプリを実行できます。

- Ipad
- Android タブレット、
- Windows タブレットおよびデスクトップコンピューター (Windows 10 を実行)。

このページでは、以下について簡単に説明します。

- サポートされている[デバイスの種類](#Device_Types)
- タブレットと携帯電話のレイアウトを[最適化](#optimize)する方法。

<a name="Device_Types" />

## <a name="device-types"></a>デバイスの種類

Xamarin. Forms でサポートされているすべてのプラットフォームで、より大きな画面デバイスを使用できます。

### <a name="ipads-ios"></a>Ipad (iOS)

Xamarin テンプレートには、 **[デバイスの >]** 設定を **[Universal]** (iPhone と iPad の両方がサポートされている) に設定することによって自動的に iPad サポートが含まれます。

快適な起動エクスペリエンスを提供し、すべてのデバイスで全画面解像度が使用されるようにするには、(ストーリーボードを使用して) [iPad 固有の起動画面](~/ios/app-fundamentals/images-icons/launch-screens.md)が用意されていることを確認してください。 これにより、iPad ミニ、iPad、iPad Pro デバイスでアプリが正しく表示されるようになります。

IOS 9 より前では、すべてのアプリがデバイスで全画面表示を使用していましたが、一部の Ipad では、[分割画面のマルチタスキング](~/ios/platform/multitasking.md)を実行できます。
つまり、アプリは画面の横、画面の幅の50%、または画面全体で、スリムな列のみを使用できます。

[![](tablet-images/ipad-sml.png "iPad の分割画面の例")](tablet-images/ipad.png#lightbox "iPad の分割画面の例")

画面の分割機能では、320ピクセル程度、または1366ピクセル幅の幅で動作するようにアプリを設計する必要があります。

### <a name="android-tablets"></a>Android タブレット

Android エコシステムでは、小型の携帯電話から大型のタブレットまで、さまざまな画面サイズがサポートされています。 Xamarin はすべての画面サイズをサポートできますが、他のプラットフォームと同様に、大規模なデバイス向けにユーザーインターフェイスを調整することもできます。

さまざまな画面解像度をサポートする場合は、さまざまなサイズのネイティブイメージリソースを提供して、ユーザーエクスペリエンスを最適化することができます。
Android[リソース](~/android/app-fundamentals/resources-in-android/index.md)のドキュメント (特に、[さまざまな画面サイズのリソースを作成](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)する) を確認し、最適化されたイメージリソースを含めるように android アプリプロジェクトのフォルダーとファイル名を構成する方法の詳細については、「」を参照してください。アプリ。

### <a name="windows-tablets-and-desktops"></a>Windows タブレットおよびデスクトップ

Windows を実行しているタブレットおよびデスクトップコンピューターをサポートするには、windows の[UWP サポート](~/xamarin-forms/platform/windows/installation/index.md)を使用する必要があります。これは、windows 10 で実行されるユニバーサルアプリを構築します。

Windows タブレットおよびデスクトップで実行されているアプリは、全画面を実行するだけでなく、任意の大きさに変更できます。

[![](tablet-images/splitscreen-sml.png "Windows 分割画面の例")](tablet-images/splitscreen.png#lightbox "Windows 分割画面の例")

<a name="optimize" />

## <a name="optimizing-for-tablet-and-desktop"></a>タブレットおよびデスクトップ向けの最適化

スマートフォンまたはタブレット/デスクトップデバイスが使用されているかどうかに応じて、Xamarin. フォームユーザーインターフェイスを調整できます。 これは、タブレットやデスクトップコンピューターなどの大画面デバイスのユーザーエクスペリエンスを最適化できることを意味します。

### <a name="deviceidiom"></a>Device.Idiom

[@No__t-1](~/xamarin-forms/platform/device.md)クラスを使用して、アプリまたはユーザーインターフェイスの動作を変更できます。 @No__t 0 列挙体を使用して、

```csharp
if (Device.Idiom == TargetIdiom.Phone)
{
    HeroImage.Source = ImageSource.FromFile("hero.jpg");
} else {
    HeroImage.Source = ImageSource.FromFile("herotablet.jpg");
}
```

この方法は、個々のページのレイアウトに大幅な変更を加える場合や、大きな画面でまったく異なるページを表示する場合にも拡張できます。

### <a name="leveraging-masterdetailpage"></a>Masterのページを活用する

[@No__t-1](xref:Xamarin.Forms.MasterDetailPage)は、特に、 [`UISplitViewController`](xref:UIKit.UISplitViewController)を使用してネイティブの iOS エクスペリエンスを提供する iPad で、大きな画面に最適です。

[この Xamarin のブログ記事](https://devblogs.microsoft.com/xamarin/bringing-xamarin-forms-apps-to-tablets/)をご覧になり、1つのレイアウトを使用する電話と、より大きい画面で別のレイアウト (`MasterDetailPage`) を使用できるようにユーザーインターフェイスを調整する方法を確認してください。

## <a name="related-links"></a>関連リンク

- [Xamarin のブログ](https://devblogs.microsoft.com/xamarin/bringing-xamarin-forms-apps-to-tablets/)
- [MyShoppe サンプル](https://github.com/jamesmontemagno/myshoppe)
