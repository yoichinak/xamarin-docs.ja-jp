---
title: タブレットアプリとデスクトップアプリのレイアウト
description: この記事で Xamarin.Forms は、携帯電話ではなくタブレットのアプリケーションレイアウトを最適化する方法について説明します。
ms.prod: xamarin
ms.assetid: D62F472B-4345-4983-8403-659A538B591F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/01/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8c53b1e58ad97f7d0e17972a2b232c16e05ecc1a
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86934889"
---
# <a name="layout-for-tablet-and-desktop-apps"></a>タブレットアプリとデスクトップアプリのレイアウト

Xamarin.Formsサポートされているプラットフォームで使用可能なすべてのデバイスの種類をサポートします。そのため、スマートフォンだけでなく、以下でもアプリを実行できます。

- Ipad
- Android タブレット、
- Windows タブレットおよびデスクトップコンピューター (Windows 10 を実行)。

このページでは、以下について簡単に説明します。

- サポートされている[デバイスの種類](#device-types)
- タブレットと携帯電話のレイアウトを[最適化](#optimize-for-tablet-and-desktop)する方法。

## <a name="device-types"></a>デバイスの種類

でサポートされているすべてのプラットフォームで、より大きな画面デバイスを使用でき Xamarin.Forms ます。

### <a name="ipads-ios"></a>Ipad (iOS)

この Xamarin.Forms テンプレートには、[**デバイスの >** ] 設定を [ **Universal** ] (iPhone と iPad の両方がサポートされていることを意味します) に設定することによって、iPad のサポートが自動的に含まれます。

快適な起動エクスペリエンスを提供し、すべてのデバイスで全画面解像度が使用されるようにするには、(ストーリーボードを使用して) [iPad 固有の起動画面](~/ios/app-fundamentals/images-icons/launch-screens.md)が用意されていることを確認してください。 これにより、iPad ミニ、iPad、iPad Pro デバイスでアプリが正しく表示されるようになります。

IOS 9 より前では、すべてのアプリがデバイスで全画面表示を使用していましたが、一部の Ipad では、[分割画面のマルチタスキング](~/ios/platform/multitasking.md)を実行できます。
つまり、アプリは画面の横、画面の幅の50%、または画面全体で、スリムな列のみを使用できます。

[![iPad の分割画面の例](tablet-images/ipad-sml.png)](tablet-images/ipad.png#lightbox "iPad の分割画面の例")

画面の分割機能では、320ピクセル程度、または1366ピクセル幅の幅で動作するようにアプリを設計する必要があります。

### <a name="android-tablets"></a>Android タブレット

Android エコシステムでは、小型の携帯電話から大型のタブレットまで、さまざまな画面サイズがサポートされています。 Xamarin.Formsではすべての画面サイズをサポートできますが、他のプラットフォームと同様に、大規模なデバイス向けにユーザーインターフェイスを調整することもできます。

さまざまな画面解像度をサポートする場合は、さまざまなサイズのネイティブイメージリソースを提供して、ユーザーエクスペリエンスを最適化することができます。
Android アプリプロジェクトのフォルダーとファイル名を構成してアプリに最適化されたイメージリソースを含める方法の詳細については、 [android のリソース](~/android/app-fundamentals/resources-in-android/index.md)に関するドキュメント (特に、[さまざまな画面サイズのリソースの作成](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)) を参照してください。

### <a name="windows-tablets-and-desktops"></a>Windows タブレットおよびデスクトップ

Windows を実行しているタブレットおよびデスクトップコンピューターをサポートするには、windows の[UWP サポート](~/xamarin-forms/platform/windows/installation/index.md)を使用する必要があります。これは、windows 10 で実行されるユニバーサルアプリを構築します。

Windows タブレットおよびデスクトップで実行されているアプリは、全画面を実行するだけでなく、任意の大きさに変更できます。

[![Windows 分割画面の例](tablet-images/splitscreen-sml.png)](tablet-images/splitscreen.png#lightbox "Windows 分割画面の例")

## <a name="optimize-for-tablet-and-desktop"></a>タブレットおよびデスクトップ用に最適化する

Xamarin.Formsスマートフォンまたはタブレット/デスクトップデバイスが使用されているかどうかに応じて、ユーザーインターフェイスを調整できます。 これは、タブレットやデスクトップコンピューターなどの大画面デバイスのユーザーエクスペリエンスを最適化できることを意味します。

### <a name="deviceidiom"></a>デバイス. 表現形式

クラスを使用して、 [`Device`](~/xamarin-forms/platform/device.md) アプリまたはユーザーインターフェイスの動作を変更できます。 列挙体を使用して、 `Device.Idiom`

```csharp
if (Device.Idiom == TargetIdiom.Phone)
{
    HeroImage.Source = ImageSource.FromFile("hero.jpg");
} else {
    HeroImage.Source = ImageSource.FromFile("herotablet.jpg");
}
```

この方法は、個々のページのレイアウトに大幅な変更を加える場合や、大きな画面でまったく異なるページを表示する場合にも拡張できます。

### <a name="leverage-masterdetailpage"></a>Masterのページを活用する

は、 [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) 大規模な画面に最適です。特に、を使用して [`UISplitViewController`](xref:UIKit.UISplitViewController) ネイティブの iOS エクスペリエンスを提供する iPad では最適です。

[この Xamarin のブログ記事](https://devblogs.microsoft.com/xamarin/bringing-xamarin-forms-apps-to-tablets/)をご覧になり、1つのレイアウトを使用するスマートフォンで別のレイアウトを使用できるようにユーザーインターフェイスを調整する方法をご確認ください `MasterDetailPage` 。

## <a name="related-links"></a>関連リンク

- [Xamarin のブログ](https://devblogs.microsoft.com/xamarin/bringing-xamarin-forms-apps-to-tablets/)
- [MyShoppe サンプル](https://github.com/jamesmontemagno/myshoppe)
