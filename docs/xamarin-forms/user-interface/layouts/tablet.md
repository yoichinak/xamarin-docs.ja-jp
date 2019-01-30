---
title: タブレットとデスクトップ アプリのレイアウト
description: この記事では、タブレット、スマート フォンではなく用の Xamarin.Forms アプリケーションのレイアウトを最適化する方法について説明します。
ms.prod: xamarin
ms.assetid: D62F472B-4345-4983-8403-659A538B591F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/01/2016
ms.openlocfilehash: 9d1f54fa4753ba2ef44ba9b8b48a84a3ca932c4b
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233849"
---
# <a name="layout-for-tablet-and-desktop-apps"></a>タブレットとデスクトップ アプリのレイアウト

Xamarin.Forms は、電話だけでなく、アプリはで実行もできるため、サポート対象のプラットフォームで利用可能なすべてのデバイスの種類をサポートしています。

* Ipad、
* Android タブレットでは、
* Windows タブレットとデスクトップ コンピューターが (Windows 10 を実行している)。

このページについて説明します。

* サポートされている[デバイスの種類](#Device_Types)と
* 方法[最適化](#optimize)スマート フォンとタブレットのレイアウト。

<a name="Device_Types" />

## <a name="device-types"></a>デバイスの種類

大きな画面デバイスすべての Xamarin.Forms でサポートされるプラットフォームを利用できます。

### <a name="ipads-ios"></a>Ipad (iOS)

Xamarin.Forms テンプレート iPad のサポートを構成することによって自動的に含まれます、 **Info.plist > デバイス**設定**ユニバーサル**(つまり、iPhone と iPad の両方がサポートされています)。

スタートアップ快適なエクスペリエンスを提供し、全画面表示の解像度がすべてのデバイスで使用されることを確認することを確認してください、 [iPad 固有の起動画面](~/ios/app-fundamentals/images-icons/launch-screens.md)(ストーリー ボードを使用して) 提供されます。 これにより、iPad mini、iPad、および iPad Pro のデバイスで、アプリが正しくレンダリングされます。

IOS 9 の前に、すべてのアプリがデバイスで、全画面表示をかかりましたが、いくつかの Ipad が行えるようになりました[マルチタス キングに画面を分割](~/ios/platform/multitasking.md)します。
つまり、アプリが画面では、画面、または画面全体の幅の 50% の横にあるスリムな列だけをかかる場合があります。

[![](tablet-images/ipad-sml.png "分割画面の例の iPad")](tablet-images/ipad.png#lightbox "iPad 分割画面の例")

分割画面機能では、幅、または 1366 としてピクセルをわずか 320 ピクセルでうまく機能するアプリを設計する必要がありますを意味します。

### <a name="android-tablets"></a>Android タブレット

Android のエコシステムには、多種多様な大規模なタブレットまでの小規模な携帯電話からのサポートされている画面サイズ、します。 Xamarin.Forms は、すべての画面サイズをサポートできるとして他のプラットフォームで可能性がありますしたい大きなデバイスのユーザー インターフェイスを調整します。

多くのさまざまな画面解像度をサポートする場合は、ユーザー エクスペリエンスを最適化するためにさまざまなサイズで、ネイティブ イメージ リソースを行うことができます。
レビュー、 [Android リソース](~/android/app-fundamentals/resources-in-android/index.md)ドキュメント (特に[画面サイズを変更するためのリソースを作成する](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)) フォルダーとファイル名に Android アプリを構成する方法の詳細についてアプリで最適化されたイメージ リソースをインクルードするプロジェクトです。

### <a name="windows-tablets-and-desktops"></a>Windows タブレットとデスクトップ

タブレットと Windows を実行しているデスクトップ コンピューターをサポートするは、使用する必要があります[Windows UWP サポート](~/xamarin-forms/platform/windows/installation/index.md)、Windows 10 で実行されるユニバーサル アプリをビルドします。

Windows タブレットとデスクトップで実行されているアプリは実行されている全画面表示のさらに任意次元に変更できます。

[![](tablet-images/splitscreen-sml.png "Windows は、画面の例を分割")](tablet-images/splitscreen.png#lightbox "Windows 分割画面の例")


<a name="optimize" />

## <a name="optimizing-for-tablet-and-desktop"></a>タブレットとデスクトップの最適化

に応じて、Xamarin.Forms のユーザー インターフェイスを調整するには、スマート フォンかどうか、またはタブレット/デスクトップ デバイスが使用されています。 これは、タブレットやデスクトップ コンピューターなどの大画面のデバイスのユーザー エクスペリエンスを最適化できることを意味します。


### <a name="deviceidiom"></a>Device.Idiom

使用することができます、 [ `Device` ](~/xamarin-forms/platform/device.md)アプリやユーザー インターフェイスの動作を変更するクラス。 使用して、`Device.Idiom`列挙することができます

```csharp
if (Device.Idiom == TargetIdiom.Phone)
{
    HeroImage.Source = ImageSource.FromFile("hero.jpg");
} else {
    HeroImage.Source = ImageSource.FromFile("herotablet.jpg");
}
```

このアプローチは、個々 のページのレイアウトに大幅な変更を加えるか、大きい画面上のまったく別のページを表示するためにも拡張できます。

### <a name="leveraging-masterdetailpage"></a>MasterDetailPage を利用します。

[ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage)使用 iPad では特に、大きな画面に最適ですが、 [ `UISplitViewController` ](xref:UIKit.UISplitViewController)ネイティブ iOS エクスペリエンスを提供します。

レビュー[この Xamarin ブログの投稿](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/)に電話を使用して、1 つのレイアウトと大きい画面が別に使用できるように、ユーザー インターフェイスの適応方法を参照してください (で、 `MasterDetailPage`)。



## <a name="related-links"></a>関連リンク

- [Xamarin ブログ](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/)
- [MyShoppe サンプル](https://github.com/jamesmontemagno/myshoppe)
